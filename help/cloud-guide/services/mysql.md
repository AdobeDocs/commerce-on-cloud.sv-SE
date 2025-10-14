---
title: Konfigurera tjänsten MySQL
description: Lär dig hur du hanterar MySQL-tjänsten för beständig datalagring med Adobe Commerce i molninfrastrukturen.
feature: Cloud, Services, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 0%

---

# Konfigurera tjänsten MySQL

Tjänsten `mysql` ger beständig datalagring baserat på [MariaDB](https://mariadb.com/), version 10.2 till 10.4, som stöder [&#x200B; XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) -lagringsmotorn och återimplementerade funktioner från MySQL 5.6 och 5.7.

Omindexering av MariaDB 10.4 tar längre tid jämfört med andra versioner av MariaDB eller MySQL. Se [Indexerare](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html?lang=sv-SE#indexers) i guiden _Bästa metoder för prestanda_.

>[!WARNING]
>
>Var försiktig när du uppgraderar MariaDB från version 10.1 till 10.2. MariaDB 10.1 är den sista versionen som har stöd för _XtraDB_ som lagringsmotor. MariaDB 10.2 använder _InnoDB_ för lagringsmotorn. När du har uppgraderat från 10.1 till 10.2 kan du inte återställa ändringen. Adobe Commerce har stöd för båda lagringsmotorerna, men du måste kontrollera att tillägg och andra system som används i ditt projekt är kompatibla med MariaDB 10.2. Se [Inkompatibla ändringar mellan 10.1 och 10.2](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102).

{{service-instruction}}

**Så här aktiverar du MySQL**:

1. Lägg till önskat namn, typ och diskvärde (i MB) i filen `.magento/services.yaml`.

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >MySQL-fel, till exempel `PDO Exception: MySQL server has gone away`, kan inträffa på grund av otillräckligt diskutrymme. Kontrollera att du har allokerat tillräckligt med diskutrymme till tjänsten i filen [`.magento/services.yaml`](services-yaml.md#disk).

1. Konfigurera relationerna i filen `.magento.app.yaml`.

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [Verifiera tjänstrelationerna](services-yaml.md#service-relationships).

{{service-change-tip}}

## Konfigurera MySQL-databas

Du har följande alternativ när du konfigurerar MySQL-databasen:

- **`schemas`** - Ett schema definierar en databas. Standardschemat är `main`-databasen.
- **`endpoints`** - Varje slutpunkt representerar en autentiseringsuppgift med specifik behörighet. Standardslutpunkten är `mysql`, som har `admin` åtkomst till databasen `main`.
- **`properties`** - Egenskaper används för att definiera ytterligare databaskonfigurationer.

Följande är en grundläggande exempelkonfiguration i filen `.magento/services.yaml`:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

`properties` i exemplet ovan ändrar standardinställningarna för `optimizer` enligt [rekommendationerna i guiden &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html?lang=sv-SE#indexers) för bästa prestanda.

**MariaDB-konfigurationsalternativ**:

| Alternativ | Beskrivning | Standardvärde |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | Standardteckenuppsättningen. | utf8mb4 |
| `default_collation` | Standardkollationen. | utf8mb4_unicode_ci |
| `max_allowed_packet` | Maximal storlek för paket, i MB. Intervall `1` till `100`. | 16 |
| `optimizer_switch` | Ange värden för frågeoptimeraren. Se [MariaDB-dokumentation](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch). | |
| `optimizer_use_condition_selectivity` | Välj den statistik som används av optimeraren. Intervall `1` till `5`. Se [MariaDB-dokumentation](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity). | 4 för 10.4 och senare |

### Konfigurera flera databasanvändare

Du kan också konfigurera flera användare med olika behörigheter för åtkomst till databasen `main`.

Som standard finns det en slutpunkt med namnet `mysql` som har administratörsåtkomst till databasen. Om du vill konfigurera flera databasanvändare måste du definiera flera slutpunkter i filen `services.yaml` och deklarera relationerna i filen `.magento.app.yaml`. För Pro Staging- och Production-miljöer [skickar du en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) för att begära ytterligare användare.

Använd en kapslad array för att definiera slutpunkterna för specifik användaråtkomst. Varje slutpunkt kan ange åtkomst till ett eller flera scheman (databaser) och olika behörighetsnivåer för varje.

Giltiga behörighetsnivåer är:

- `ro`: Endast SELECT-frågor tillåts.
- `rw`: SELECT-frågor och INSERT-, UPDATE- och DELETE-frågor tillåts.
- `admin`: Alla frågor tillåts, inklusive DDL-frågor (CREATE TABLE, DROP TABLE med flera).

Exempel:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

I föregående exempel ger slutpunkten `admin` administratörsnivååtkomst till databasen `main`, slutpunkten `reporter` skrivskyddad åtkomst och slutpunkten `importer` ger skrivskyddad åtkomst, vilket innebär:

- Användaren `admin` har fullständig kontroll över databasen.
- Användaren `reporter` har endast SELECT-behörighet.
- Användaren `importer` har behörigheterna SELECT, INSERT, UPDATE och DELETE.

Lägg till de slutpunkter som definieras i exemplet ovan till egenskapen `relationships` för filen `.magento.app.yaml`. Exempel:

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>Om du konfigurerar en MySQL-användare kan du inte använda åtkomstkontrollsfunktionen [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) för lagrade procedurer och vyer.

## Anslut till databasen

Om du vill komma åt MariaDB-databasen direkt måste du använda en SSH för att logga in i fjärrmolnmiljön och ansluta till databasen.

1. Använd SSH för att logga in i fjärrmiljön.

   ```bash
   magento-cloud ssh
   ```

1. Hämta inloggningsuppgifterna för MySQL från egenskaperna `database` och `type` i variabeln [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships).

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   eller

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Leta reda på MySQL-informationen i svaret. Exempel:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. Anslut till databasen.

   - Använd följande kommando för Starter:

     ```bash
     mysql -h database.internal -u <username>
     ```

   - För Pro använder du följande kommando med värdnamn, portnummer, användarnamn och lösenord som hämtats från variabeln `$MAGENTO_CLOUD_RELATIONSHIPS`.

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>Du kan använda kommandot `magento-cloud db:sql` för att ansluta till fjärrdatabasen och köra SQL-kommandon.

## Anslut till sekundär databas

>[!IMPORTANT]
>
>Den här funktionen är endast tillgänglig i Pro Production- och Staging-kluster.

Ibland måste du ansluta till den sekundära databasen för att förbättra databasprestanda eller lösa problem med låsning av databaser. Om den här konfigurationen krävs använder du `"port" : 3304` för att upprätta anslutningen. Se [Bästa praxis för att konfigurera MySQL-slavanslutningen](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html?lang=sv-SE) i guiden _Bästa praxis för implementering_.

## Felsökning

Se följande Adobe Commerce supportartiklar för hjälp med felsökning av MySQL-problem:

- [Kontrollerar långsamma frågor och processer MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html?lang=sv-SE)
- [Skapa databasdump i molnet](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html?lang=sv-SE)
- [Felsökning för datamigreringsverktyget](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html?lang=sv-SE)
- [Uppgradering av Adobe Commerce: kompakt till dynamiska tabeller 2.2.x, 2.3.x till 2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html?lang=sv-SE)
