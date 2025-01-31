---
title: Distribuera till mellanlagring och produktion
description: Lär dig hur du distribuerar din Adobe Commerce på molninfrastrukturkod till miljöer för stapling och produktion för ytterligare testning.
feature: Cloud, Console, Deploy, SCD, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1310'
ht-degree: 0%

---

# Distribuera till mellanlagring och produktion

Processen för att distribuera och publicera börjar med utveckling, fortsätter till Förproduktion och slutar med att publicera i Produktion. Adobe är en totallösning för hela miljön som ger enhetliga konfigurationer. Alla miljöer har stöd för direkt URL-åtkomst till butiken och administratörs- och SSH-åtkomst för CLI-kommandon.

När du är redo att distribuera din butik måste du slutföra distributionen och testningen i mellanlagringsmiljön innan du distribuerar till Production. I det här avsnittet finns detaljerade anvisningar och information om bygg- och distributionsprocessen, migrering av data och innehåll samt testning.

>[!TIP]
>
>Adobe rekommenderar att du skapar en [säkerhetskopia](../storage/snapshots.md) av miljön före distributioner.

Du kan även aktivera [Spåra distributioner med New Relic](../monitor/track-deployments.md) för att övervaka distributionshändelser och hjälpa dig att analysera prestanda mellan distributioner.

## Startdistributionsflöde

Adobe rekommenderar att du skapar en `staging`-gren från `master`-grenen för att få bästa möjliga stöd för utveckling och distribution av Starter-planen. Sedan har du två av dina fyra aktiva miljöer klara: `master` för produktion och `staging` för mellanlagring.

Mer information om processen finns i [Arbetsflöde för att utveckla och distribuera start](../architecture/starter-develop-deploy-workflow.md).

## Driftsättningsflöde

Pro levereras med en stor integreringsmiljö med två aktiva grenar, en global `master`-gren, mellanlagring och produktionsgrenar. När du skapar ditt projekt är koden redo att förgrena, utveckla och pusha för att bygga och driftsätta din webbplats. Även om integreringsmiljön kan ha många grenar har Förproduktion och Produktion bara en gren för varje miljö.

Mer information om processen finns i [Arbetsflödet Framkalla och distribuera i Pro](../architecture/pro-develop-deploy-workflow.md).

## Distribuera kod till mellanlagring

I mellanlagringsmiljön finns en nästan produktionsmiljö som innehåller en databas, webbserver och alla tjänster, inklusive Fastly och New Relic. Du kan överföra, sammanfoga och distribuera fullständigt via [[!DNL Cloud Console]](../project/overview.md)- eller [ Cloud CLI-kommandona](../dev-tools/cloud-cli-overview.md) via ett terminalprogram.

### Distribuera kod med [!DNL Cloud Console]

[!DNL Cloud Console] innehåller funktioner för att skapa, hantera och distribuera kod i integrerings-, mellanlagrings- och produktionsmiljöer för Starter- och Pro-planer.

**För Pro-projekt distribuerar du integrationsgrenen till mellanlagring**:

1. [Logga in](https://accounts.magento.cloud) i ditt projekt.
1. Välj grenen `integration`.
1. Välj alternativet **Koppla** som ska distribueras till Förproduktion.

   ![Sammanfoga](../../assets/button-merge.png){width="150"}

1. Slutför alla [tester](../test/staging-and-production.md) i mellanlagringsmiljön.
1. När mellanlagring är klar väljer du alternativet **Sammanfoga** för att distribuera till Produktion.

**Distribuera utvecklingsgrenen till mellanlagring** för Starter:

1. [Logga in](https://accounts.magento.cloud) i ditt projekt.
1. Välj den förberedda kodgrenen.
1. Välj alternativet **Koppla** som ska distribueras till Förproduktion.

   ![Sammanfoga](../../assets/button-merge.png){width="150"}

1. Slutför alla [tester](../test/staging-and-production.md) i mellanlagringsmiljön.
1. När mellanlagring är klar väljer du alternativet **Sammanfoga** som ska distribueras till produktion (`master`).

### Distribuera kod med kommandoraden

Cloud CLI innehåller kommandon för att distribuera kod. Du behöver SSH- och Git-åtkomst till ditt projekt.

#### Steg 1: Distribuera och testa integreringsmiljön

1. När du har loggat in i projektet ska du ta en titt på integreringsmiljön:

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Synkronisera den lokala integreringsmiljön med fjärrmiljön:

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Skapa en ögonblicksbild av miljön som en säkerhetskopia:

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Uppdatera kod i din lokala gren efter behov.

1. Lägg till, implementera och skicka ändringar till miljön.

   ```bash
   git add -A && git commit -m "Commit message" && git push origin <environment-ID>
   ```

1. Komplett testning.

#### Steg 2: Sammanfoga ändringar i Förproduktion och distribuera

1. Ta en titt på Mellanlagringsmiljön:

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Synkronisera din lokala mellanlagringsmiljö med fjärrmiljön:

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Skapa en ögonblicksbild av miljön som en säkerhetskopia:

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Koppla integreringsmiljön till mellanlagring för att distribuera:

   ```bash
   magento-cloud environment:merge <integration-ID>
   ```

1. Komplett testning.

#### Steg 3: Distribuera till produktion

1. Kolla in, synkronisera och skapa en ögonblicksbild av din lokala produktionsmiljö.

1. Koppla mellanlagringsmiljön till produktionen för att distribuera:

   ```bash
   magento-cloud environment:merge <staging-ID>
   ```

1. Komplett testning.

## Migrera statiska filer

[Statiska filer](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary) lagras i `mounts`. Det finns två metoder för att migrera filer från en källmonteringsplats, till exempel din lokala miljö, till en målmonteringsplats. Båda metoderna använder verktyget `rsync`, men Adobe rekommenderar att du använder CLI `magento-cloud` för att flytta filer mellan den lokala miljön och fjärrmiljön. Och Adobe rekommenderar att du använder metoden `rsync` när du flyttar filer från en fjärrkälla till en annan fjärrplats.

### Migrera filer med CLI

Du kan använda kommandona `mount:upload` och `mount:download` CLI för att migrera filer mellan den lokala miljön och fjärrmiljön. Båda kommandona använder verktyget `rsync`, men CLI-kommandona innehåller alternativ och uppmaningar som är anpassade till Adobe Commerce i molninfrastrukturmiljön. Om du till exempel använder det enkla kommandot utan alternativ uppmanas du att välja vilka monteringar som ska laddas upp eller laddas ned.

```bash
magento-cloud mount:download
```

Exempelsvar:

```
Enter a number to choose a mount to download from:
  [0] app/etc
  [1] pub/static
  [2] var
  [3] pub/media
  [4] All mounts
 > 3

Target directory: ~/pub/media/

Downloading files from the remote mount pub/media to pub/media

Are you sure you want to continue? [Y/n] Y
```

**Så här överför du filer från en lokal `pub/media/`-mapp till fjärrmappen `pub/media/` för den aktuella miljön**:

```bash
magento-cloud mount:upload --source /path/to/project/pub/media/ --mount pub/media/
```

Exempelsvar:

```
Uploading files from pub/media to the remote mount pub/media

Are you sure you want to continue? [Y/n] Y

  building file list ...   done
  ./
  sample-file.jpeg

  sent 8.43K bytes  received 48 bytes  3.39K bytes/sec
  total size is 154.57K  speedup is 18.23
```

Använd alternativet `--help` för kommandona `mount:upload` och `mount:download` om du vill se fler alternativ. Det finns till exempel ett `--delete`-alternativ för att ta bort överflödiga filer under migreringen.

### Migrera filer med hjälp av synkronisering

Du kan också använda verktyget `rsync` för att migrera filer.

```bash
rsync -azvP <source> <destination>
```

Det här kommandot använder följande alternativ:

- `a`-arkiv
- `z`-komprimera filer under migreringen
- `v` - utförlig
- `P`-partiella förlopp

Se hjälpen för [rsync](https://linux.die.net/man/1/rsync).

>[!NOTE]
>
>Om du vill överföra media direkt från fjärr-till-fjärr-miljöer måste du aktivera vidarebefordran av SSH-agenten, se [GitHub-vägledning](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

**Så här migrerar du statiska filer från fjärr-till-fjärr-miljöer direkt (snabb hantering)**:

1. Använd SSH för att logga in i källmiljön. Använd inte CLI:n för `magento-cloud`. Det är viktigt att använda alternativet `-A` eftersom det möjliggör vidarebefordran av autentiseringsagentanslutningen.

   >[!TIP]
   >
   >Om du vill hitta länken **SSH-åtkomst** i [!DNL Cloud Console] markerar du miljön och klickar på **Åtkomstplats**.

   ```bash
   ssh -A <environment_ssh_link@ssh.region.magento.cloud>
   ```

1. Använd kommandot `rsync` för att kopiera katalogen `pub/media` från källmiljön till en annan fjärrmiljö.

   ```bash
   rsync -azvP pub/media/ <destination_environment_ssh_link@ssh.region.magento.cloud>:pub/media/
   ```

1. Logga in på den andra fjärrmiljön för att verifiera att filerna har migrerats.

## Migrera databasen

>[!WARNING]
>
>Det kan ta lång tid att importera och exportera databaser, vilket kan påverka webbplatsens prestanda och tillgänglighet. Schemalägg import- och exportåtgärder under tider med låg belastning för att förhindra långsamma prestanda eller avbrott på produktionsplatser.

>[!BEGINSHADEBOX]

**Förutsättning:** En databasdump (se steg 3) ska innehålla databasutlösare. Bekräfta att du har privilegiet [TRIGGER](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger) om du vill dumpa dem.

>[!IMPORTANT]
>
>Integreringsmiljödatabasen är endast till för utvecklingstestning och kan innehålla data som du inte vill migrera till Förproduktion och Produktion.

Adobe **rekommenderar inte** att migrera data från integrering till mellanlagring och produktion för kontinuerlig integrering. Du kan skicka testdata eller skriva över viktiga data. Viktiga konfigurationer skickas med kommandot [konfigurationsfilen](../store/store-settings.md) och `setup:upgrade` under bygget och distributionen.

>[!ENDSHADEBOX]

Adobe **rekommenderar** att du migrerar data från Production till Staging för att fullständigt testa din webbplats och lagra den i en närproduktionsmiljö med alla tjänster och inställningar.

>[!NOTE]
>
>Om du vill överföra media från fjärr-till-fjärr-miljöer direkt måste du aktivera Ssh Agent-vidarebefordran, se [GitHub-vägledning](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

### Säkerhetskopiera databasen

Det är en god vana att skapa en säkerhetskopia av databasen. Följande procedur använder vägledningen från [Säkerhetskopiera databasen](../storage/database-dump.md).

**Så här dumpar du databasen**:

1. [Använd SSH för att logga in i fjärrmiljön](../development/secure-connections.md#use-an-ssh-command) som innehåller den databas som ska kopieras.

1. Visa en lista över miljörelationerna och notera databasinloggningsinformationen.

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

   För Pro Staging and Production finns databasens namn i variabeln `MAGENTO_CLOUD_RELATIONSHIPS` (vanligtvis samma som programnamnet och användarnamnet).

1. Skapa en säkerhetskopia av databasen. Om du vill välja en målkatalog för DB-dumpen använder du alternativet `--dump-directory`.

   För Starter-miljöer och Pro-integreringsmiljöer använder du `main` som namn på databasen:

   ```bash
   php vendor/bin/ece-tools db-dump main
   ```

   Dumpa alternativ:
   - `--dump-directory=<dir>` - Välj en målkatalog för databasdumpen
   - `--remove-definers` - Ta bort DEFINER-satser från databasdumpen

1. Även om metoden ECE-Tools är att föredra är en annan metod att skapa en databasdumpfil med hjälp av MySQL i GZIP-format.

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers <database-name> | gzip - > /tmp/database.sql.gz
   ```

   Om du har konfigurerat tvåfaktorsautentisering i målmiljön är det bättre att exkludera relaterade 2FA-tabeller för att undvika att konfigurera om den efter databasmigrering:

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers --ignore-table=<database-name>.tfa_user_config --ignore-table=<database-name>.tfa_country_codes <database-name> | gzip - > /tmp/database.sql.gz
   ```

1. Skriv `logout` för att avsluta SSH-anslutningen.

### Släpp och återskapa databasen

När du importerar data måste du släppa och skapa en databas.

**Så här släpper och återskapar du databasen**:

1. Upprätta en [SSH-tunnel](../development/secure-connections.md#ssh-tunneling) till fjärrmiljön.

1. Anslut till databastjänsten.

   ```bash
   mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
   ```

1. Släpp databasen vid `MariaDB [main]>`-prompten.

   För integrering med Starter och Pro:

   ```shell
   drop database main;
   ```

   För produktion:

   ```shell
   drop database <cluster-id>;
   ```

   För mellanlagring:

   ```shell
   drop database <cluster-ID_stg>;
   ```

1. Återskapa databasen.

   För integrering med Starter och Pro:

   ```shell
   create database main;
   ```

1. Importera databasen.

   Import för produktion:

   ```shell
   zcat <cluster-ID>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   Import för mellanlagring:

   ```shell
   zcat <cluster-ID_stg>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   Dessa kommandon expanderar databasdumpfilen, tar bort `DEFINER`-programsatserna och importerar databasen med de angivna autentiseringsuppgifterna.
