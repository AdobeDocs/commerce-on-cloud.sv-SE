---
title: Konfigurera tjänster
description: Lär dig hur du konfigurerar tjänster som används av Adobe Commerce i molninfrastruktur.
feature: Cloud, Configuration, Services
exl-id: ddf44b7c-e4ae-48f0-97a9-a219e6012492
source-git-commit: 5fc2082ca2aae8a1466821075c01ce756ba382cc
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 0%

---

# Konfigurera tjänster

Filen `services.yaml` definierar de tjänster som stöds och används av Adobe Commerce i molninfrastrukturen, till exempel MySQL, Redis och Elasticsearch eller OpenSearch. Du behöver inte prenumerera på externa tjänsteleverantörer.

>[!NOTE]
>
>Filen `.magento/services.yaml` hanteras lokalt i katalogen `.magento` i ditt projekt. Konfigurationen nås under byggprocessen för att endast definiera de tjänstversioner som krävs i integreringsmiljön och tas bort när distributionen har slutförts, så du kommer inte att hitta dem på servern.


Distributionsskriptet använder konfigurationsfilerna i katalogen `.magento` för att etablera miljön med de konfigurerade tjänsterna. En tjänst blir tillgänglig för ditt program om den ingår i egenskapen [`relationships`](../application/properties.md#relationships) för filen `.magento.app.yaml`. Filen `services.yaml` innehåller värdena _type_ och _disk_. Tjänsttypen definierar tjänsten _name_ och _version_.

Om du ändrar en tjänstkonfiguration tillhandahålls miljön med de uppdaterade tjänsterna, vilket påverkar följande miljöer:

- Alla startmiljöer inklusive produktion `master`
- Integreringsmiljöer för Pro

{{pro-update-service}}

## Standardtjänster och tjänster som stöds

Molninfrastrukturen stöder och distribuerar följande tjänster:

- [ActiveMQ](activemq.md)
- [MySQL](mysql.md)
- [Redis](redis.md)
- [KaninMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

Du kan visa standardversioner och diskvärden i den aktuella [standardfilen `services.yaml`](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml). I följande exempel visas tjänsterna `mysql`, `redis`, `opensearch` eller `elasticsearch`, `rabbitmq` och `activemq-artemis` som definieras i konfigurationsfilen `services.yaml`:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024

activemq-artemis:
    type: activemq-artemis:2.42
    disk: 1024
```

## Tjänstvärden

Du måste ange tjänst-ID och tjänsttypskonfigurationen `type: <name>:<version>`. Om tjänsten använder beständig lagring måste du ange ett diskvärde.

Använd följande format:

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

Värdet `service-id` identifierar tjänsten i projektet. Du kan bara använda alfanumeriska gemener: `a` till `z` och `0` till `9`, till exempel `redis`.

Det här _service-id_-värdet används i [`relationships`](../application/properties.md#relationships)-egenskapen i konfigurationsfilen `.magento.app.yaml`:

```yaml
relationships:
    redis: "<name>:redis"
```

Du kan namnge flera instanser av varje tjänsttyp. Du kan till exempel använda flera Redis-instanser - en för en session och en för cache.

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

Om du byter namn på en tjänst i `services.yaml`-filen **tas följande bort permanent**:

- Den befintliga tjänsten innan du skapar en tjänst med det nya namn du anger.
- Alla befintliga data för tjänsten tas bort. Adobe rekommenderar att du [säkerhetskopierar startmiljön](../storage/snapshots.md) innan du ändrar namnet på en befintlig tjänst.

### `type`

Värdet `type` anger tjänstens namn och version. Exempel:

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

Värdet `disk` anger storleken på den beständiga disklagringen (i MB) som ska allokeras till tjänsten. Tjänster som använder beständig lagring, till exempel MySQL, måste ange ett diskvärde. Tjänster som använder minne i stället för beständig lagring, som Redis, kräver inget diskvärde.

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

Det aktuella standardlagringsutrymmet per projekt är 5 GB eller 512 MB. Du kan distribuera det här beloppet mellan programmet och var och en av dess tjänster.

## Servicerelationer

I Adobe Commerce för molninfrastrukturprojekt avgör tjänsten [relationer](../application/properties.md#relationships) som konfigurerats i filen `.magento.app.yaml` vilka tjänster som är tillgängliga för ditt program.

Du kan hämta konfigurationsdata för alla tjänstrelationer från miljövariabeln [`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md). Konfigurationsdata innehåller tjänstnamn, typ och version tillsammans med all nödvändig anslutningsinformation, t.ex. portnummer och inloggningsuppgifter.

**Så här verifierar du relationer i den lokala miljön**:

1. Visa relationerna för den aktiva miljön i din lokala miljö.

   ```bash
   magento-cloud relationships
   ```

1. Bekräfta `service` och `type` från svaret. Svaret ger anslutningsinformation, t.ex. IP-adress och portnummer.

   >Förkortat provsvar

   ```yaml
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**Så här verifierar du relationer i fjärrmiljöer**:

1. Använd SSH för att logga in i fjärrmiljön.

1. Visa konfigurationsdata för relationer för alla tjänster som konfigurerats i miljön.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   eller använd följande `ece-tools`-kommando för att visa relationer:

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. Bekräfta `service` och `type` från svaret. Svaret innehåller anslutningsinformation, t.ex. IP-adress och portnummer samt eventuella inloggningsuppgifter för användarnamn och lösenord.

## Tjänstversioner

Tjänstversion och kompatibilitetsstöd för Adobe Commerce i molninfrastruktur avgörs av vilka versioner som distribueras och testas i molninfrastrukturen, och skiljer sig ibland från de versioner som stöds av Adobe Commerce lokala distributioner. Se [Systemkrav](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) i _Installationshandboken_ för en lista över programberoenden från tredje part som Adobe har testat med specifika versioner av Adobe Commerce och Magento Open Source.

### EOL-kontroller för programvara

Under distributionsprocessen kontrollerar paketet `ece-tools` installerade tjänstversioner mot slutdatum (EOL) för varje tjänst.

- Om en tjänstversion är inom tre månader från EOL-datumet visas ett meddelande i distributionsloggen.
- Om EOL-datumet redan har infallit visas ett varningsmeddelande.

För att upprätthålla säkerheten måste du uppdatera de installerade programversionerna innan de når EOL. Du kan granska EOL-datum i [ece-tools&#39; `eol.yaml` file](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml).

### Migrera till OpenSearch

{{elasticsearch-support}}

Information om Adobe Commerce version 2.4.4 och senare finns i [Konfigurera OpenSearch-tjänsten](opensearch.md).

## Ändra tjänstversion

Du kan uppgradera den installerade tjänstversionen för kompatibilitet med den Adobe Commerce-version som är distribuerad i din molnmiljö.

Du kan inte nedgradera tjänstversionen för en installerad tjänst direkt. Du kan dock skapa en tjänst med den version som krävs. Se [Nedgradera tjänstversion](#downgrade-version).

### Uppgradera installerad tjänstversion

Du kan uppgradera den installerade tjänstversionen genom att uppdatera tjänstkonfigurationen i filen `services.yaml`.

1. Ändra värdet [`type`](#type) för tjänsten i filen `.magento/services.yaml`:

   > Ursprunglig tjänstdefinition

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > Uppdaterad tjänstdefinition

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### Nedgraderingsversion

Du kan inte nedgradera en installerad tjänst direkt. Du har två alternativ:

1. Byt namn på en befintlig tjänst till den nya versionen, som tar bort den befintliga tjänsten och data, och lägger till en ny.

1. Skapa en tjänst och spara data från den befintliga tjänsten.

När du ändrar tjänstversionen måste du uppdatera tjänstkonfigurationen i filen `services.yaml` och uppdatera relationerna i filen `.magento.app.yaml`.

**Så här nedgraderar du en tjänstversion genom att byta namn på en befintlig tjänst**:

1. Byt namn på den befintliga tjänsten i filen `.magento/services.yaml` och ändra versionen.

   >[!WARNING]
   >
   >Om du byter namn på en befintlig tjänst ersätts den och alla data tas bort. Om du behöver behålla data skapar du en tjänst i stället för att byta namn på den befintliga.

   Om du till exempel vill nedgradera MariaDB-versionen för tjänsten _mysql_ från version 10.4 till 10.3 ändrar du den befintliga konfigurationen för _service-id_ och _typ_ .

   > Ursprunglig `services.yaml`-definition

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > Ny `services.yaml`-definition

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. Uppdatera relationerna i filen `.magento.app.yaml`.

   > Ursprunglig `.magento.app.yaml`-konfiguration

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Uppdaterad `.magento.app.yaml`-konfiguration

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Lägg till, implementera och push-överföra kodändringar.

**Så här nedgraderar du en tjänst genom att skapa en tjänst**:

1. Lägg till en tjänstdefinition i filen `services.yaml` för ditt projekt med den nedgraderade versionsspecifikationen. Se _mysql2_ i följande exempel:

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. Ändra relationskonfigurationen i filen `.magento.app.yaml` om du vill använda den nya tjänsten.

   > Ursprunglig `.magento.app.yaml`-konfiguration

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Ny `.magento.app.yaml`-konfiguration

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Lägg till, implementera och push-överföra kodändringar.
