---
title: Konfigurera miljö
description: Lär dig hur du konfigurerar bygg- och distributionsåtgärder i alla Commerce-miljöer på molninfrastrukturer, inklusive Pro Staging och Production, med hjälp av miljövariabler.
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# Konfigurera miljövariabler för distribution

Filen `.magento.env.yaml` använder miljövariabler för att centralisera hanteringen av bygg- och distributionsåtgärder i alla dina miljöer, inklusive Pro Staging och Production. Om du vill konfigurera unika åtgärder i varje miljö måste du ändra den här filen i varje miljö.

>[!TIP]
>
>YAML-filer är skiftlägeskänsliga och tillåter inte tabbar. Var noga med att använda konsekvent indrag i hela `.magento.env.yaml`-filen, annars kanske inte konfigurationen fungerar som förväntat. Exemplen i dokumentationen och i exempelfilen använder _indrag med två blanksteg_. Använd kommandot [ece-tools validate](#validate-configuration-file) för att kontrollera konfigurationen.

## Filstruktur

Filen `.magento.env.yaml` innehåller två avsnitt: `stage` och `log`. Avsnittet `stage` kontrollerar åtgärder som utförs under faserna i [Cloud-distributionsprocessen](../deploy/process.md).

- `stage` - Använd scenavsnittet för att definiera vissa åtgärder för följande distributionssteg:
   - `global` - Styr åtgärder i både bygg-, distributions- och postdistributionsfaserna. Du kan åsidosätta dessa inställningar i avsnitten för att skapa, distribuera och efterdistribuera.
   - `build` - Kontrollerar endast åtgärder i byggfasen. Om du inte anger några inställningar i det här avsnittet, kommer byggfasen att använda inställningar från det globala avsnittet.
   - `deploy` - Kontrollerar endast åtgärder i distributionsfasen. Om du inte anger inställningar i det här avsnittet används inställningarna från det globala avsnittet i distributionsfasen.
   - `post-deploy` - Kontrollerar åtgärder _efter_-distributionen av programmet och _efter_ kan behållaren börja acceptera anslutningar.
- `log` - Använd loggavsnittet för att konfigurera [meddelanden](set-up-notifications.md), inklusive meddelandetyper och detaljnivå.
   - `slack` - Konfigurera ett meddelande som ska skickas till en robot från Slack.
   - `email` - Konfigurera ett e-postmeddelande som ska skickas till en eller flera e-postmottagare.
   - [logghanterare](log-handlers.md) - Konfigurera maskinvaru- och programprogrammeddelanden som skickas till en fjärrloggningsserver.

### Miljövariabler

Paketet `ece-tools` ställer in värden i filen `env.php` baserat på värden från [Cloud-variabler](variables-cloud.md), variabler som angetts i [!DNL Cloud Console] och konfigurationsfilen `.magento.env.yaml`. Miljövariablerna i filen `.magento.env.yaml` anpassar molnmiljön genom att åsidosätta din befintliga Commerce-konfiguration. Om standardvärdet är `Not Set` utför `ece-tools`-paketet **NO**-åtgärden och använder standardvärdet [!DNL Commerce] eller värdet från MAGENTO_CLOUD_RELATIONSHIPS-konfigurationen. Om standardvärdet anges används `ece-tools`-paketet för att ange standardvärdet.

Följande avsnitt innehåller detaljerade definitioner, till exempel om ett standardvärde har angetts eller inte, av alla variabler som du kan använda i filen `.magento.env.yaml`:

- [Global](variables-global.md) - variabelkontrollåtgärder i varje fas: skapa, distribuera och efterdistribuera
- [Build](variables-build.md) - variabelkontrollbyggåtgärder
- [Distribuera](variables-deploy.md) - variabelstyrningsdistributionsåtgärder
- [Efter distribution](variables-post-deploy.md) - variabelkontrollåtgärder efter distribution

### Skapa konfigurationsfil från CLI

Du kan generera en `.magento.env.yaml`-konfigurationsfil för en molnmiljö med följande `ece-tools`-kommandon.

>Skapar en konfigurationsfil

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>Uppdatera värden i konfigurationsfilen

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

Båda kommandona kräver ett enda argument: en JSON-formaterad array som anger ett värde för minst en variabel för build, deploy eller post-deploy. Följande kommando anger till exempel värden för variablerna `SCD_THREADS` och `CLEAN_STATIC_FILES`:

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

Och skapar en `.magento.env.yaml`-fil med följande inställningar:

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

Du kan använda kommandot `cloud:config:update` för att uppdatera den nya filen. Följande kommando ändrar till exempel värdet `SCD_THREADS` och lägger till konfigurationen `SCD_COMPRESSION_TIMEOUT`:

```bash
php vendor/bin/ece-tools cloud:config:update '{"stage":{"build":{"SCD_THREADS":3, "SCD_COMPRESSION_TIMEOUT":1000}}}'
```

Den uppdaterade filen innehåller följande konfiguration:

```yaml
stage:
  build:
    SCD_THREADS: 3
    SCD_COMPRESSION_TIMEOUT: 1000
  deploy:
    CLEAN_STATIC_FILES: false
```

### Verifiera konfigurationsfil

Använd följande `ece-tools`-kommando för att validera konfigurationsfilen `.magento.env.yaml` innan du skickar ändringar till fjärrmolnmiljön.

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

Följande exempelsvar innehåller en lista med objekt som ska korrigeras:

```
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## PHP-konstanter

Du kan använda PHP-konstanter i `.magento.env.yaml`-fildefinitioner i stället för hårdkodade värden. I följande exempel definieras `driver_options` med en PHP-konstant:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
        indexer:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
      _merge: true
```

>[!WARNING]
>
>Konstantparsning fungerar inte när en tidigare version av ett `symfony/yaml`-paket än 3.2 används.

## Felhantering

När ett fel inträffar på grund av ett oväntat värde i konfigurationsfilen `.magento.env.yaml` får du ett felmeddelande. I följande felmeddelande visas en lista med föreslagna ändringar av varje objekt med ett oväntat värde, som ibland innehåller giltiga alternativ:

```
- Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:
  Item CRON_CONSUMERS_RUNNER is not supposed to be in stage build. Please move it to one of possible stages: global, deploy
  Item SKIP_SCD has unexpected type string. Please use one of next types: boolean
  Item VERBOSE_COMMANDS has unexpected type boolean. Please use one of next types: string
  Item SKIP_HTML_MINIFICATION has unexpected type string. Please use one of next types: boolean
  Item CRON_CONSUMERS_RUNNER has unexpected type boolean. Please use one of next types: array
  Item VAR_WARM_UP_PAGES is not allowed in configuration.
  Item WARM_UP_PAGES has unexpected type string. Please use one of next types: array
```

Gör eventuella korrigeringar, implementera och skicka ändringarna vidare. Om du inte får något felmeddelande godkänns valideringen av ändringarna i konfigurationsfilen.

## Optimering av konfigurationshantering

Om du har aktiverat Configuration Management efter att ha dumpat konfigurationerna, bör du flytta SCD_*-variablerna från distributionen till byggfasen. Se [Statiska strategier för innehållsdistribution](../deploy/static-content.md).

>Före konfigurationshantering:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

>När du har aktiverat Configuration Management flyttar du SCD_*-variablerna till byggfasen:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```
