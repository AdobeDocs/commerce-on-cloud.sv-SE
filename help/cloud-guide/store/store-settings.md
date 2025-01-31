---
title: Hantera butikskonfiguration
description: Lär dig hur du hanterar och synkroniserar lagringskonfigurationsinställningar för alla Adobe Commerce i molninfrastrukturmiljöer.
feature: Cloud, Configuration, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# Hantera butikskonfiguration

Standardkonfigurationerna för din butik lagras i en `config.xml` för rätt modul. När du ändrar inställningarna i Commerce Admin eller CLI `bin/magento config:set` återspeglas ändringarna i kärndatabasen, särskilt i tabellen `core_config_data`. De här inställningarna skriver över standardkonfigurationerna som lagras i filen `config.xml`.

Lagringsinställningarna, som refererar till konfigurationerna i avsnittet Admin **Stores** > **Settings** > **Configuration** , lagras i distributionskonfigurationsfilerna baserat på typen av konfiguration:

- `app/etc/config.php` - konfigurationsinställningar för butiker, webbplatser, moduler eller tillägg, statisk filoptimering och systemvärden för statisk innehållsdistribution. Se referensen [config.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html) i _konfigurationshandboken_.
- `app/etc/env.php` - värden för systemspecifika åsidosättningar och känsliga inställningar som _INTE_ ska lagras i källkontrollen. Se referensen [env.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html) i _konfigurationshandboken_.

>[!NOTE]
>
>Eftersom Adobe Commerce i molninfrastrukturen endast stöder produktions- och underhållslägen är **Avancerat** > **Utvecklare** inte tillgängligt i Admin. Du måste ha [systemadministratörsbehörighet](../project/user-access.md) för att kunna slutföra konfigurationshanteringsuppgifter. Du kan konfigurera ytterligare inställningar med [miljövariabler](../environment/configure-env-yaml.md).

Konfigurationshantering är ett sätt att driftsätta enhetliga lagringsinställningar i alla miljöer med minimalt antal driftavbrott med hjälp av driftsättning i pipeline. Adobe Commerce i molninfrastrukturprojektet innehåller byggservern, bygg och distribuera skript och distributionsmiljöer som utformats med [pipeline-distributionsstrategin](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) i åtanke.

## Schema för åsidosättning av konfiguration

Alla systemkonfigurationer ställs in under bygg- och distributionsfaserna enligt följande åsidosättningsschema:

1. Om det finns en miljövariabel använder du den anpassade konfigurationen och ignorerar standardkonfigurationen.
1. Om det inte finns någon miljövariabel använder du konfigurationen från ett `MAGENTO_CLOUD_RELATIONSHIPS` namn/värde-par i [`.magento.app.yaml` file](../application/configure-app-yaml.md) . Ignorera standardkonfigurationen.
1. Om det inte finns någon miljövariabel och `MAGENTO_CLOUD_RELATIONSHIPS` inte innehåller något namn/värde-par tar du bort all anpassad konfiguration och använder värdena från standardkonfigurationen.

Sammanfattningsvis åsidosätter miljövariabler alla andra värden.

>[!TIP]
>
>Se [Konfigurationshantering](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) i _Konfigurationsguiden_ för mer information om åsidosättningsschemat för pipeline-distribution.

Om samma inställning är konfigurerad på flera platser använder programmet följande konfigurationshierarki för att avgöra vilket värde som ska användas i miljön:

| Prioritet | Konfiguration<br>Metod | Beskrivning |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br> miljövariabler | Värden som lagts till från fliken _Variabler_ i miljökonfigurationen i [!DNL Cloud Console]. Ange värden här för känsliga eller miljöspecifika konfigurationer. Inställningarna som anges här kan inte redigeras från administratören. Se [Miljökonfigurationsvariabler](../project/overview.md#configure-environment). |
| 2 | `.magento.app.yaml` | Värden som lagts till i `variables`-avsnittet i filen `.magento.app.yaml`. Ange värden här för att säkerställa en konsekvent konfiguration i alla miljöer. **Ange inte känsliga värden i filen `.magento.app.yaml`.** Se [Programinställningar](../application/configure-app-yaml.md). |
| 3 | `app/etc/env.php` | Miljöspecifika konfigurationsvärden som lagras här läggs till med kommandot `app:config:dump`. Ange systemspecifika och känsliga värden med hjälp av systemvariabler eller CLI. Se [Känsliga data](#sensitive-data). Filen `env.php` ingår **inte** i källkontrollen. |
| 4 | `app/etc/config.php` | Värden som lagras här läggs till med kommandot `app:config:dump`. Delade konfigurationsvärden läggs till i `config.php`. Ange delad konfiguration från administratören eller med CLI. Filen `config.php` ingår i källkontrollen. |
| 5 | Databas | Värden som lagras här läggs till genom att konfigurationer ställs in i administratören. Konfigurationer som angetts med någon av ovanstående metoder är låsta (nedtonade) och kan inte redigeras från administratören. |
| 6 | `config.xml` | Många konfigurationer har standardvärden angivna i filen `config.xml` för en modul. Om Adobe Commerce inte hittar något värde som angetts av någon av de föregående metoderna, återgår det till standardvärdet, om det angetts. |

{style="table-layout:auto"}

## Konfigurationsdump

Du kan använda följande `ece-tools`-kommando för att generera en `config.php`-fil som innehåller alla aktuella lagringskonfigurationer:

```bash
./vendor/bin/ece-tools config:dump
```

Data&quot;dumpade&quot; till filen `app/etc/config.php` blir _låsta_, vilket innebär att motsvarande fält i Commerce Admin blir **skrivskyddade**. Filen `config.php` innehåller bara de inställningar som du konfigurerar. Standardvärdena låses inte. Genom att bara låsa de värden som du uppdaterar säkerställs också att alla tillägg som används i mellanlagrings- och produktionsmiljöer inte bryts på grund av skrivskyddade konfigurationer, speciellt Snabbt.

>[!WARNING]
>
>Kommandot `ece-tools config:dump` hämtar inte detaljerade konfigurationer för moduler, t.ex. B2B. Om du behöver en omfattande konfigurationsdump använder du kommandot `app:config:dump`, men det här kommandot låser konfigurationsvärden i ett skrivskyddat läge.

### Känsliga data

Alla känsliga konfigurationer exporteras till filen `app/etc/env.php` när du använder kommandot `bin/magento app:config:dump`. Du kan ange känsliga värden med CLI-kommandot: `bin/magento config:sensitive:set`. Mer information om hur du anger konfigurationsinställningar som känsliga eller systemspecifika finns i [Känsliga och miljöspecifika inställningar](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) i guiden _Commerce PHP-tillägg_ .

Se en lista med [känsliga eller systemspecifika inställningar](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html) i _konfigurationshandboken_.

### SCD-prestanda

Beroende på storleken på din butik kan du ha ett stort antal statiska innehållsfiler att distribuera. Statiskt innehåll distribueras vanligtvis under distributionsfasen när programmet är i underhållsläge. Den optimala konfigurationen är att generera statiskt innehåll under byggfasen. Se [Välja en distributionsstrategi](../deploy/static-content.md).

Om du har aktiverat Configuration Management efter att ha dumpat konfigurationerna, bör du flytta SCD_*-variablerna från distributionsfasen till byggfasen för att korrekt aktivera statisk innehållsgenerering under byggfasen. Se [Miljövariabler](../environment/configure-env-yaml.md#environment-variables).

**Före konfigurationshantering**:

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

**När konfigurationshantering har aktiverats**:

Flytta SCD_*-variablerna till byggscenen:

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

>[!NOTE]
>
>Innan statiska filer distribueras komprimeras statiskt innehåll med GZIP i bygg- och distributionsfaserna. Komprimering av statiska filer minskar belastningen på servern och ökar platsens prestanda. Mer information om hur du anpassar eller inaktiverar filkomprimering finns i [byggalternativ](../environment/variables-build.md).

## Procedur för att hantera dina inställningar

Följande illustrerar en översikt över processen på hög nivå:

![Översikt över hanteringen av startkonfigurationen](../../assets/starter/configuration-management-flow.png)

**Så här konfigurerar du arkivet och skapar en konfigurationsfil**:

1. Slutför alla konfigurationer för butikerna i Admin för en av miljöerna:

   - Startsida: En aktiv utvecklingsgren
   - Pro: En aktiv gren i integreringsmiljön

   Dessa konfigurationer inkluderar inte de faktiska produkterna såvida du inte planerar att dumpa databasen från den här miljön till mellanlagrings- och produktionsmiljöer. Utvecklingsdatabaser innehåller vanligtvis inte alla data i din butik.

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Skapa en lokal dump av fjärrdatabasen.

   ```bash
   magento-cloud db:dump
   ```

1. Lägg till, implementera och push-ändra kod för att uppdatera en fjärrmiljö.

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

När distributionen är klar loggar du in på Admin för den uppdaterade miljön för att verifiera inställningarna. Fortsätt att sammanfoga eventuella ytterligare konfigurationer till miljö för förproduktion och produktion efter behov.

### Uppdatera konfigurationer

När du ändrar miljön via Admin och kör kommandot igen läggs nya konfigurationer till i koden i filen `config.php`.

>[!WARNING]
>
>Du kan redigera filen `config.php` manuellt i miljö för förproduktion och produktion, men du bör **inte** göra det. Filen hjälper till att hålla alla konfigurationer konsekventa i alla miljöer. Ta aldrig bort filen `config.php` för att återskapa den. Om du tar bort filen kan specifika konfigurationer och inställningar som krävs för bygg- och distributionsprocesser tas bort.

### Återställ konfigurationsfiler

Kopior av de ursprungliga `app/etc/env.php`- och `app/etc/config.php`-filerna skapades under distributionsprocessen och lagras i samma mapp. Följande visar BAK (säkerhetskopieringsfiler) och PHP (originalfiler) i samma `app/etc`-mapp:

```
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

Äldre konfigurationer använde filen `app/etc/config.local.php`. Se [Migrera äldre konfigurationer](#migrate-older-configurations).

**Så här återställer du konfigurationsfiler**:

1. Använd SSH på din lokala arbetsstation för att logga in på fjärrprojektet och -miljön.

   ```bash
   magento-cloud ssh
   ```

1. Kontrollera platsen och tillgängligheten för säkerhetskopieringsfilerna.

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   Exempelsvar:

   ```
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. Återställ säkerhetskopierade filer.

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### Migrera äldre konfigurationer

Om du uppgraderar till Adobe Commerce i molninfrastruktur 2.2 eller senare kanske du vill migrera inställningar från filen `config.local.php` till den nya `config.php`-filen. Om konfigurationsinställningarna i din administratör matchar innehållet i filen följer du instruktionerna för att generera och lägga till filen `config.php`.

Om de skiljer sig åt kan du lägga till innehåll från filen `config.local.php` till den nya `config.php`-filen:

1. Följ instruktionerna för att generera filen `config.php`.

1. Öppna filen `config.php` och ta bort den sista raden.

1. Öppna filen `config.local.php` och kopiera innehållet.

1. Klistra in innehållet i filen `config.php`, spara och lägg sedan till det i Git.

1. Driftsätt i olika miljöer.

Du slutför bara den här migreringen en gång. Efter migreringen använder du filen `config.php`.

### Ändra nationella inställningar

Du kan ändra språkinställningarna för din butik utan att följa en komplex import- och exportprocess för konfiguration, _om_ du har [SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand) aktiverat. Du kan uppdatera språkinställningarna med Admin.

Du kan lägga till ytterligare en språkinställning i förings- eller produktionsmiljön genom att aktivera `SCD_ON_DEMAND` i en integrationsgren, generera en uppdaterad `config.php`-fil med den nya språkinställningsinformationen och kopiera konfigurationsfilen till målmiljön.

>[!WARNING]
>
>Den här processen **skriver över** lagringskonfigurationen. Gör bara följande om miljöerna innehåller samma arkiv.

1. Aktivera variabeln `SCD_ON_DEMAND` med [`.magento.env.yaml` file](../environment/configure-env-yaml.md) i integreringsmiljön.

1. Lägg till de nödvändiga språkinställningarna med din administratör.

1. Använd SSH för att logga in i fjärrmiljön och generera filen `/app/etc/config.php` som innehåller alla språkområden.

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. Kopiera den nya konfigurationsfilen från fjärrintegreringsmiljön till den lokala systemkatalogen.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Lägg till, implementera och push-ändra kod för att uppdatera en fjärrmiljö.
