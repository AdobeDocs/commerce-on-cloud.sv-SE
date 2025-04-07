---
title: Versionsinformation för ECE-verktyg
description: Se en lista över de senaste förbättringarna av ECE-verktygspaketet.
recommendations: noDisplay, catalog
last-substantial-update: 2024-04-03T00:00:00Z
exl-id: 3cbfe698-d75d-4a16-877a-52c214595344
source-git-commit: 3d5c84890f48a26938b42783b591b876fd2a2fd1
workflow-type: tm+mt
source-wordcount: '3059'
ht-degree: 0%

---

# Versionsinformation för ECE-verktyg

Paketet [ece-tools](https://github.com/magento/ece-tools) är en uppsättning skript och verktyg som är utformade för att hantera och distribuera Cloud-projekt. Versionsinformationen beskriver de senaste förbättringarna av det här paketet, som ingår i [Creative Cloud-verktygen för Commerce](cloud-tools-suite.md).

>[!NOTE]
>
>Mer information om hur du uppdaterar till den senaste versionen av `ece-tools`-paketet finns i [Uppgradera ECE-verktyg](../dev-tools/update-package.md) .

Paketet `ece-tools` använder följande versionssekvens: `200<major>.<minor>.<patch>`

Versionsinformationen innehåller:

- ![ny ikon](../../assets/new.svg) Nya funktioner
- ![korrigeringsikon](../../assets/fix.svg) Korrigeringar och förbättringar

<!--Add release notes below-->

## v2002.2.2 {#latest}

Releasedatum: 3 april 2025

- ![ny ikon](../../assets/new.svg) **Valkey** - Stöd har lagts till för en ny tjänst (Valkey), som ersätter Redis.<!-- MCLOUD-13455	 - -->
- ![korrigeringsikon](../../assets/fix.svg) **OpenSearch2 for 2.4.4/2.4.5**—Added support for `opensearch2` in Adobe Commerce versions 2.4.4/2.4.5. <!-- MCLOUD-13493	 - -->

## v2002.2.1

Releasedatum: 6 februari 2024

- ![ny ikon](../../assets/new.svg) **PHP 8.4** - Stöd för PHP 8.4 har lagts till.<!-- MCLOUD-13145	 - -->
- ![korrigeringsikon](../../assets/fix.svg) **Valideraren för OpenSearch**-Korrigerade valideraren som gav upphov till ett missvisande meddelande om fel version av tjänsten.<!-- MCLOUD-13184	 - -->


## v2002.2.0

Releasedatum: 7 oktober 2024

- ![ny ikon](../../assets/new.svg) **MariaDB 11.4** - Stöd för MariaDB 11.4 har lagts till.
- ![korrigeringsikon](../../assets/fix.svg) **Refererad kod**-Borttaget stöd för äldre PHP-versioner 7.4, 7.3, 7.2 och relaterade bibliotek.<!-- MCLOUD-9278 -->
- ![korrigeringsikon](../../assets/fix.svg) **Uppgraderad Monolog-version** - Tillagt stöd för monolog 3.6.<!-- MCLOUD-12855 -->
- ![korrigeringsikon](../../assets/fix.svg) **Validerare för RabbitMQ, MariaDB och PHP**-Korrigerade valideraren som gav upphov till ett missvisande meddelande om fel tjänstversion.

## v2002.1.19

Releasedatum: 21 maj 2024

- ![ny ikon](../../assets/new.svg) **Lua** - Alternativet useLua för CACHE_CONFIGURATION har lagts till.
- ![korrigeringsikon](../../assets/fix.svg) **Validator** - Uppdaterade validerare för nya versioner av Redis och RabbitMQ.

## v2002.1.18

Releasedatum: 8 april 2024

- ![ny ikon](../../assets/new.svg) **PHP** - Stöd för PHP 8.3 har lagts till.
- ![korrigeringsikon](../../assets/fix.svg) **Validator** - Uppdaterad EOL-validerare.

## v2002.1.17

Releasedatum: 16 januari 2024

- ![korrigeringsikon](../../assets/fix.svg) **Validerare för Elasticsearch &amp; OpenSearch** - Korrigerade valideraren som gav upphov till ett felande meddelande om att installera en söktjänst när LiveSearch är aktiverat.<!-- MCLOUD-10167 -->
- ![korrigeringsikon](../../assets/fix.svg) **Distributionsvarning** - Ett problem som resulterade i distributionsvarningar för mappar som inte är tomma har korrigerats.<!-- MCLOUD-8958 -->

## v2002.1.16

Releasedatum: 16 oktober 2023

- ![ny ikon](../../assets/new.svg) **ENABLE_WEBHOOKS global miljövariabel** - har lagt till den globala variabeln [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) som kan användas med Commerce-webbhooks för att ansluta till en extern slutpunkt, till exempel App Builder runtime-åtgärd eller ett lagerhanteringssystem från tredje part.

## v2002.1.15

Releasedatum: 31 juli 2023

- ![korrigeringsikon](../../assets/fix.svg) **Felkoder** - Uppdaterat felkodsschema och dokumentgenerator för felkod.
- ![korrigeringsikon](../../assets/fix.svg) **Validerare för anpassad Redis-modell**-Uppdaterade valideraren för anpassade Redis-backend-modeller. [Se exemplet för cachekonfiguration](../environment/variables-deploy.md#cache_configuration).
- ![korrigeringsikon](../../assets/fix.svg) **Validator för RabbitMQ**-Added support for RabbitMQ 3.11
- ![korrigeringsikon](../../assets/fix.svg) **Korrigerade fel länk**-Korrigerade fel länk till startdokumentationen i välkomstmallen.

## v2002.1.14

Releasedatum: 10 mars 2023

- ![ny ikon](../../assets/new.svg) **PHP** - Stöd för PHP 8.2 har lagts till.
- ![ny ikon](../../assets/new.svg) **Validerare for Services** - Uppdaterade validerare för Commerce 2.4.6 som krävs: MariaDB 10.6, Redis 7.0, PHP 8.2, OpenSearch 2.x och RabbitMQ 3.9.
- ![korrigeringsikon](../../assets/fix.svg) **ece-tools db-dump** - Korrigerade ett fel som gjorde att `db-dump`-åtgärden stoppades för tidigt.

## v2002.1.13

Releasedatum: 27 oktober 2022

- ![ny ikon](../../assets/new.svg) **Stöd för Adobe I/O Events för Adobe Commerce har lagts till**. Tilläggsutvecklare kan nu använda [Adobe I/O Events](https://developer.adobe.com/events/docs/)-ramverket för att skicka Commerce-händelseinformation från molninstanser till sina program skrivna för [Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/). Adobe I/O Events för Adobe Commerce finns i Partner Preview.<!-- CEXT-932 -->
- ![ny ikon](../../assets/new.svg) **Validator för OPCache-konfiguration** - En validerare har lagts till för att kontrollera om konfigurationen för OPCache innehåller uteslutna sökvägar.<!-- MCLOUD-9485 -->
- ![korrigeringsikon](../../assets/fix.svg) **Ett problem med cachekonfigurationen för GraphQL** har korrigerats. Nu behåller ECE-Tools värdet för GraphQL `id_salt` i konfigurationen för `cache` i filen `app/etc/env.php`.<!-- MCLOUD-9486 -->

## v2002.1.12

Releasedatum: 13 september 2022

- ![ny ikon](../../assets/new.svg) **Aktivera`synchronous_replication`**—ECE-Tools anger `synchronous_replication=>true` i filen `app/etc/env.php` när `MYSQL_USE_SLAVE_CONNECTION` är aktiverat. Den här konfigurationen påverkar bara Commerce 2.4.6+. Se variabelbeskrivningen `MYSQL_USE_SLAVE_CONNECTION` i [Distribuera variabler](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![ny ikon](../../assets/new.svg) **OpenSearch** - Funktioner för att konfigurera och ställa in `opensearch`-motorn för nästa version av Adobe Commerce 2.4.6 har lagts till. Se [Konfigurera OpenSearch-tjänsten](../services/opensearch.md).<!-- MCLOUD-9236 -->

## v2002.1.11

Releasedatum: 4 augusti 2022

- ![korrigeringsikon](../../assets/fix.svg) **ElasticSuite-validerare och OpenSearch** - Ett problem med integritetskontrollen för ElasticSuite när OpenSearch är installerat har åtgärdats.<!-- MCLOUD-8767 -->
- ![korrigeringsikon](../../assets/fix.svg) **Returtyper för distributionskommandon** - Korrigerade returtyper för distributionskommandon.<!-- AC-3208 -->
- ![korrigeringsikon](../../assets/fix.svg) **[!DNL RabbitMQ]problem med installation av nya Commerce 2.4.5** - Korrigerat [!DNL RabbitMQ] kraschproblem vid installation av nya Commerce 2.4.5.<!-- MCLOUD-9059 -->

## v2002.1.10

Releasedatum: 31 mars 2022

- ![korrigeringsikon](../../assets/fix.svg) **Elasticsearch 7.10** - Uppdaterade validerare med stöd för version 7.10 av Elasticsearch.<!-- MCLOUD-8548 -->

## v2002.1.9

Releasedatum: 10 mars 2022

- ![ny ikon](../../assets/new.svg) **OpenSearch** - Stöd för OpenSearch i Adobe Commerce version 2.4.4, 2.4.3-p2 och 2.3.7-p3 har lagts till.<!-- MCLOUD-8296 -->
- ![ny ikon](../../assets/new.svg) **PHP** - Stöd för PHP 8.1 har lagts till.
- ![korrigeringsikon](../../assets/fix.svg) **symfony/process** - Kompatibiliteten med symbolen/processen ^5.3 har lagts till.<!-- MCLOUD-8283 -->

- ![ny ikon](../../assets/new.svg) **Användar flera processer** - Ett `multiple_processes`-alternativ har lagts till så att du kan ange antalet processer som ska anges för varje konsument. Se variabelbeskrivningen `CRON_CONSUMERS_RUNNER` i [Distribuera variabler](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->
- ![ny ikon](../../assets/new.svg) **OpenSearch-schema och fullständig värdsökväg** - har lagts till för att konfigurera ett Elasticsearch-schema och fullständig värdsökväg.
- ![korrigeringsikon](../../assets/fix.svg) **AWS S3** - Metoden för aktivering av AWS S3 har ändrats.
- ![korrigeringsikon](../../assets/fix.svg) **Korrigera drivrutinen_options-läsare** - Läste in driver_options-konfiguration för databasanslutning från `env.php`-filen av `ece-tools` för validerare.<!-- MCLOUD-8420 -->

## v2002.1.8

Releasedatum: 25 oktober 2021

- ![ny ikon](../../assets/new.svg) **Alternativ dumpplats** - alternativet `--dump-directory` har lagts till så att du kan välja en målkatalog för en DB-dump. `/app/var/dump-main` är nu standardmålkatalog för en DB-dump. Se [Hantering av säkerhetskopiering: Dumpa databasen](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![korrigeringsikon](../../assets/fix.svg) **Uppdatera Monolog** - Uppdaterade minimiversionen som krävs för paketet `monolog` till `^2.3`.<!-- ACMP-1263 -->
- ![korrigeringsikon](../../assets/fix.svg) **Uppdatera Symfony** - Symfony-beroendena har uppdaterats så att de är kompatibla med Adobe Commerce 2.4.4.<!-- ACMP-1533 -->
- ![korrigeringsikon](../../assets/fix.svg) **Funktion/lös automatisk inläsning** - Korrigerade ett fel vid distribution till en integreringsmiljö och `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.`-felet visades.<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

Releasedatum: 29 juli 2021

**Konfigurationsuppdateringar**—

- ![ny ikon](../../assets/new.svg) Stöd för Composer 2.0 har lagts till.<!--MCLOUD-8003-->

- ![korrigeringsikon](../../assets/fix.svg) **Uppdaterade dispositionskrav för`symphony/console`** - Versionskraven för ECE-verktygen `composer.json` för paketet `symphony/console` uppdaterades för att åtgärda ett problem som gjorde att `di:compile`-kommandona misslyckades med följande fel: `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![korrigeringsikon](../../assets/fix.svg) Uppdaterade programvarukontrollerna vid slutet av livscykeln (`eol.yaml`) så att Elasticsearch 7.9.x <!--MCLOUD-7938--> inkluderades

## v2002.1.6

Releasedatum: 20 april 2021

- ![ny ikon](../../assets/new.svg) **Redis-autentiseringsuppgifter** - Lagt till möjlighet att läsa Redis-autentiseringsuppgifter från egenskapen `relationships` under distributionsfasen.<!--MCLOUD-7694-->

- ![ny ikon](../../assets/new.svg) **Elasticsearch autentiseringsuppgifter** - Lagt till möjligheten att läsa Elasticsearch autentiseringsuppgifter från egenskapen `relationships` under distributionsfasen.<!--MCLOUD-7695-->

- ![ny ikon](../../assets/new.svg) **Dedikerad sessionslagringstjänst** - Lagt till `redis-session` som ett andra alternativ för sessionslagring. Du kan använda tjänsten `redis-session` för att lagra sessionsinformation och använda tjänsten `redis` för att få bättre prestanda.<!--MCLOUD-7698-->

- ![ny ikon](../../assets/new.svg) **Borttagna SPLIT_DB-meddelanden** - Valideringsvarning och viktiga meddelanden för det borttagna `SPLIT_DB`-alternativet för Adobe Commerce 2.4.2 och dess borttagning i Adobe Commerce 2.5.0 har lagts till.<!--MCLOUD-7806-->

- ![korrigeringsikon](../../assets/fix.svg) **Elasticsearch-version från relationer** - Korrigerad tjänstverifierare för att hämta rätt version av Elasticsearch från `relationships` -egenskaperna i Cloud Docker- och integreringsmiljöer.<!--MCLOUD-7572-->

- ![korrigeringsikon](../../assets/fix.svg) **Flexibel Redis-portvalidering** - Redis kan nu validera porten i en anpassad cacheanslutning från URL:en `server`. Du kan t.ex. lägga till ditt portnummer till serverns URL enligt följande: `server: 'tcp://rfs-store-simple-page-cache:26379'`. Detta förhindrar valideringsfel där alternativet `port` antingen saknas eller är felaktigt.<!--MCLOUD-7722-->

- ![korrigeringsikon](../../assets/fix.svg) **Uppgraderar till Adobe Commerce 2.4.2** - Korrigerade ett problem som innebar att användare måste köra `bin/magento setup:upgrade` manuellt för att kunna använda sina webbplatser efter uppgradering till Adobe Commerce 2.4.2.<!--MCLOUD-7776-->

## v2002.1.5

Releasedatum: 1 februari 2021

- ![ny ikon](../../assets/new.svg) **Fjärrlagring** - Miljövariabeln `REMOTE_STORAGE` har lagts till för att aktivera molnprojekt för fjärrlagring av mediefiler med hjälp av en lagringstjänst, till exempel AWS S3. Det här konfigurationsalternativet är en del av ECE-verktygspaketet, men stöds inte på Adobe Commerce i molninfrastrukturen.<!--MCLOUD-7153-->

- ![ny ikon](../../assets/new.svg) **Nytt `cloud:config:validate` kommando** - Kommandot `php vendor/bin/ece-tools cloud:config:validate` har lagts till för att validera `.magento.env.yaml`-konfigurationen innan ändringar skickas till fjärrmolnmiljön.<!--MCLOUD-7120-->

- ![ny ikon](../../assets/new.svg) **Tömmer opcache** - Stöd för PHP-alternativet `opcache.enable_cli` har lagts till för att tömma OPCache innan distributionskroken körs. Den här konfigurationen återställer cachekonfigurationen för att säkerställa att de aktuella konfigurationsinställningarna tillämpas på varje distribution.<!--MCLOUD-7015-->

- ![ny ikon](../../assets/new.svg) **Validering av Aurora DB** - Databastjänstens validering har uppdaterats så att den är kompatibel med Aurora-databasen.<!--MCLOUD-7269-->

- ![ny ikon](../../assets/new.svg) **Ny miljövariabel för SCD_NO_PARENT** - lade till miljövariabeln `SCD_NO_PARENT` (för Adobe Commerce >=2.4.2) för att hantera genereringen av statiskt innehåll för överordnade teman.<!--MCLOUD-7284-->

- ![korrigeringsikon](../../assets/fix.svg) **Minnesbegränsningar och kommandon** - Korrigerade ett fel där `php vendor/bin/ece-tools`-kommandon inte fungerade om storleken på `cloud.log`-filen överskred PHP-minnesgränsen. I stället för att läsa in hela `cloud.log`-filen i minnet läser vi nu bara en mindre delmängd av data från loggfilen.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![korrigeringsikon](../../assets/fix.svg) **Anpassade databasanslutningar** - Korrigerade ett `.magento.env.yaml`-konfigurationsproblem där anpassade databasanslutningar som definierats för `DATABASE_CONFIGURATION` inte användes. Anslutningsinställningarna lades inte till i `app/etc/env.php`.<!--MCLOUD-7426-->

- ![korrigeringsikon](../../assets/fix.svg) **Tomma felloggar** - Korrigerade ett problem som gjorde att distributioner misslyckades om `cloud.error.log` var tom.<!--MCLOUD-7296-->

- ![korrigeringsikon](../../assets/fix.svg) **MariaDB 10.3-validering** - Verifieringen av MariaDB 10.3 för Adobe Commerce 2.3.6-p1 har korrigerats.<!--MCLOUD-7416-->

- ![korrigeringsikon](../../assets/fix.svg) **Cache:flush-loggning** - Förbättrade loggposter som anger början och slut på `cache:flush`-steget.<!--MCLOUD-7503-->

## v2002.1.4

Releasedatum: 19 november 2020

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som orsakade ett distributionsfel när sökmotorn som anges i miljövariabeln `SEARCH_CONFIGURATION` har ett annat värde än `elasticsearch` har åtgärdats.<!--MCLOUD-7283-->

## v2002.1.3

Releasedatum: 9 november 2020

**Infrastrukturuppdateringar**—

- ![ny ikon](../../assets/new.svg) Tillagt ECE-verktyg-stöd för den skrivskyddade `pub/static`-katalogen när statiskt innehåll är inställt på att distribueras i byggfasen.<!--MC-37699-->

- ![ny ikon](../../assets/new.svg) Stöd för Elasticsearch 7.9 och Redis 6 har lagts till för kompatibilitet med kommande Adobe Commerce-versioner.<!--MCLOUD-7191-->

- ![korrigeringsikonen](../../assets/fix.svg) Uppdaterade ECE-verktygen `composer.json` för att lägga till ett nödvändigt beroende för verktyget för kvalitetspatchar. Detta korrigerar ett cirkelberoende som fanns mellan ECE-Tools- och magento-cloud-patches-paketen.<!--MCLOUD-6910-->

**Förbättringar av validering och logg**—

- ![ny ikon](../../assets/new.svg) Lagt till sökmotorvalidering för att se till att `elasticsearch` är inställt för Adobe Commerce i molninfrastruktur 2.4 och senare. Om valideringen misslyckas stoppas distributionen med ett kritiskt felmeddelande som föreslår korrigeringar för problemet. Se [Allvarliga fel, Distribuera stadium](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![ny ikon](../../assets/new.svg) Lagt till Elasticsearch-validering för att kontrollera kompatibiliteten mellan Elasticsearch och Adobe Commerce-versionen.<!--MCLOUD-7193-->

- ![ny ikon](../../assets/new.svg) Uppdaterade felmeddelandet om Elasticsearch-kompatibilitet för att visa de versioner av Elasticsearch som är kompatibla med Adobe Commerce Elasticsearch-modulen. Felmeddelandet innehåller nu de specifika Elasticsearch-versioner som ska installeras i din molninfrastruktur, så att den är kompatibel med Elasticsearch-modulen som används i din version av Adobe Commerce. Se [Varningsfel, Distribuera stadium](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![ny ikon](../../assets/new.svg) Lagt till varningsfel `2026` och `2027` för ogiltig inställning för miljövariabel i `MAGE_MODE`. Det enda giltiga värdet är `production`. Före den här korrigeringen kunde `MAGE_MODE` anges till `developer` utan distributionsfel, så att fel bara uppstår senare när du försöker skriva till skrivskyddade filer. Se [Varningsfel](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![korrigeringsikon](../../assets/fix.svg) Verifieringen för Redis-, RabbitMQ- och MySQL-tjänster har åtgärdats för att säkerställa att dessa versioner är kompatibla med Adobe Commerce-versionen. Giltiga versioner av dessa tjänster har nu skrivits till `cloud.log`.<!--MCLOUD-7098-->

- ![korrigeringsikonen](../../assets/fix.svg) `cloud.log` har uppdaterats för att inkludera gränsen för antal samtidiga begäranden för att skicka begäranden under cachevärmen. Det här värdet är konfigurerat i variabeln [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency) efter distribution.<!--MCLOUD-5563-->

**CLI-kommandouppdateringar**—

- ![ny ikon](../../assets/new.svg) Tillagda CLI-kommandon (`cloud:config:create` och `cloud:config:update`) för att skapa och uppdatera `.magento.env.yaml`-filen med en konfiguration som kan innehålla en eller flera variabler för att skapa, distribuera och efterdistribuera. Se [Skapa konfigurationsfil från CLI](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->

**Uppdateringar av miljövariabeln**—

- ![ny ikon](../../assets/new.svg) lade till byggvariabeln [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload). Om variabeln anges till `true` stoppas programmet från att köra kommandot `composer dump-autoload` under en installation av en Cloud Docker för Commerce. Variabeln är endast relevant för Cloud Docker för Commerce-behållare med skrivbara filsystem (skapade för testning och utveckling med `./vendor/bin/ece-docker build:compose --with-test`). Om du hoppar över kommandot `composer dump-autoload` vid sådana installationer förhindras fel när andra kommandon som försöker få åtkomst till filer från en borttagen `generated`-katalog körs.<!--MCLOUD-6939-->

## v2002.1.2

Releasedatum: 5 augusti 2020

**Förbättringar av validering och logg**—

- ![ny ikon](../../assets/new.svg) har lagt till filen `schema.error.yaml` som innehåller alla fel- och varningsmeddelanden som kan inträffa under processen för att skapa, distribuera och distribuera samt förslag på hur du löser felen. Informationen i den här filen finns också i _molnguiden för Commerce_. Se [Referens för felmeddelande för extraverktyg](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![ny ikon](../../assets/new.svg) Ändrade poster i Cloud-felloggen (`/var/log/cloud.error.log`) till JSON-format för att göra loggen enklare att tolka programmatiskt.<!--MCLOUD-5879-->

- ![ny ikon](../../assets/new.svg) Lagt till ytterligare felkontroller för att skapa, distribuera och efterdistribuera bearbetning samt förbättrade befintliga kontroller:

   - Felkod 2026 - Det gick inte att återställa vissa data som genererats under byggfasen till de monterade katalogerna

   - Felkod 3004 - Det går inte att skapa säkerhetskopior

   - Felkod 102 - Ytterligare kontroller har lagts till för problem som inträffar när filen `env.php` inte är skrivbar <!--MCLOUD-6221-->

- ![ny ikon](../../assets/new.svg) Lagt till miljövariabeln **QUALITY_PATCHES** för att ange en eller flera kvalitetsuppdateringar som ska användas under distributionen. Se [Skapa variabler](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

Releasedatum: 25 juni 2020

- ![ny ikon](../../assets/new.svg) **Infrastrukturuppdateringar**—

   - ![ny ikon](../../assets/new.svg) **Loggningsförbättringar** - Förbättrad loggspårning genom att tilldela avslutskoder till kritiska distributionsfel och visa avslutskoderna i felmeddelanden och logghändelser. Se [Referens för felmeddelande för extraverktyg](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![ny ikon](../../assets/new.svg) Förbättrade processen för databasdumpar (`vendor/bin/ece-tools db-dump`) och uppdaterade loggmeddelanden för att förtydliga att databasdumpåtgärden växlar programmet till underhållsläge, stoppar konsumentköprocesser och inaktiverar kronijobb innan dumpningen börjar.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![korrigeringsikonen](../../assets/fix.svg) Ett problem har korrigerats för att se till att projektets URL uppdateras korrekt vid distributionen till mellanlagrings- och produktionsmiljöer. Nu använder `ece-tools` URL:en för vägen med attributet `primary:true` angivet i projektvägskonfigurationen. Se [Distribuera variabler](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![korrigeringsikonen](../../assets/fix.svg) Uppdaterade arbetsflödet för `generate.xml` build scenario för att tillämpa korrigeringar. Patchar måste användas tidigare för att uppdatera Adobe Commerce för att åtgärda problem som kan få `di:compile` och `module:refresh`-stegen att misslyckas.<!--MCLOUD-5941-->

   - ![korrigeringsikonen](../../assets/fix.svg) Åtgärdade ett fel i installationsprocessen som felaktigt returnerade felet `Crypt key missing`. Värdet `crypt/key` genereras automatiskt under installationen.<!--MCLOUD-6120-->

- ![ny ikon](../../assets/new.svg) **Tjänstuppdateringar**—

   - ![ny ikon](../../assets/new.svg) Stöd för PHP 7.4 och MariaDB 10.4 har lagts till.<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![ny ikon](../../assets/new.svg) **Uppdateringar för miljövariabeln**—

   - ![ny ikon](../../assets/new.svg) Lade till variabeln **SCD_USE_BALER** för att aktivera Baler-modulen för JavaScript-paketering under byggprocessen för Adobe Commerce i molninfrastruktur. Se variabelbeskrivningen i [build-variablerna](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![ny ikon](../../assets/new.svg) Lagt till miljövariabeln **REDIS_BACKEND** för att konfigurera Redis-serverdelsmodellen för Redis-cache för Adobe Commerce 2.3.5 eller senare. Se variabelbeskrivningen i [distribueringsvariablerna](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->

- ![ny ikon](../../assets/new.svg) **CLI-kommandouppdateringar**—

   - ![ny ikon](../../assets/new.svg) Uppdaterade följande CLI-kommandon med ett alternativ för mer detaljerad loggning:

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     Loggningsnivån för varje anrop bestäms av konfigurationen för variabeln [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) i filen `.magento.env.yaml`.<!--MCLOUD-3503-->

- ![ny ikon](../../assets/new.svg) **Valideringsförbättringar**—

   - ![ny ikon](../../assets/new.svg) **Kompatibilitetskontroller för Elasticsearch 7.x** - Uppdaterad Elasticsearch-validering för kompatibilitetskontroller för Elasticsearch 7.x.<!--MCLOUD-5542-->

   - ![ny ikon](../../assets/new.svg) **Uppdaterad tjänstversion och EOL-valideringskontroller** - Uppdaterad validering för att kontrollera installerade tjänstversioner mot Adobe Commerce 2.4.0-kraven.<!--MCLOUD-6144-->

   - ![korrigeringsikon](../../assets/fix.svg) Ett valideringsfel har korrigerats så att följande varningsmeddelande efter distributionen bara visas om den `post-deploy`-krokkonfigurationen saknas i filen `.magento.app.yaml`:

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![ny ikon](../../assets/new.svg) **Lagt till validering för Zend Framework-beroenden** - Kompositionsberoendevalidering har lagts till för Zend Framework som har migrerats till Laminas-projektet. Om de nödvändiga beroendena saknas visas följande felmeddelande under byggprocessen.

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     Se [Verifiera Zend Framework-beroenden](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![ny ikon](../../assets/new.svg) **Lagt till validering för `env.php` fil och data** - Kontroller för filen `env.php` och data har lagts till under installations- och uppgraderingsprocessen.<!--MCLOUD-5991-->

      - Om filen `env.php` saknas i installationen och värdet `crypt/key` inte anges i filen `.magento.app.yaml` misslyckas distributionen med följande meddelande:

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - Om installationen inte innehåller filen `env.php`, eller om konfigurationen bara innehåller en cachetyp, körs kommandot `cron:enable` under uppgraderingsprocessen för att återställa filen med alla `cache_types`. Följande meddelande läggs till i loggen:

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

Releasedatum: 6 februari 2020

- ![ny ikon](../../assets/new.svg) **Infrastrukturuppdateringar**—

   - ![ny ikon](../../assets/new.svg) **Lagt till separat paket för Cloud Docker för Commerce** - Docker-paketet kopplades från `ece-tools`-paketet för att upprätthålla kodkvaliteten och tillhandahålla oberoende releaser. Uppdateringar och korrigeringar relaterade till `ece-tools` hanteras från GitHub-databasen [ magento-cloud-docker](https://github.com/magento/magento-cloud-docker).<!--MAGECLOUD-2927-->

   - ![ny ikon](../../assets/new.svg) **Uppdaterade patchfunktioner** - har flyttat patchfunktionen från ECE-verktygspaketet till ett separat [magento-cloud-patches](https://github.com/magento/magento-cloud-patches) -paket. Under distributionen använder `ece-tools` det nya paketet för att tillämpa korrigeringar. Se [Versionsinformation om molnpatchar](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![ny ikon](../../assets/new.svg) **Uppdaterade Composer-beroenden** - `composer.json`-filen för Adobe Commerce i molninfrastrukturen har uppdaterats med ett beroende för `magento/magento-cloud-docker`-paketet. `ece-tools` innehåller nu beroenden för alla paket i [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). Dessa paket installeras och uppdateras automatiskt när du installerar eller uppdaterar `ece-tools`.

- ![ny ikon](../../assets/new.svg) **Stöd för scenariobaserade distributioner**—<!--MAGECLOUD-4101-->

   - ![ny ikon](../../assets/new.svg) Nu kan du anpassa processerna för att skapa, distribuera och efterdistribuera med hjälp av XML-konfigurationsfiler för att åsidosätta eller anpassa standardkonfigurationen.

   - ![ny ikon](../../assets/new.svg) **Konfigurationen `hooks` har ändrats i`.magento.app.yaml`** - Konfigurationsformatet `hooks` har uppdaterats för att stödja scenariobaserade distributioner. Det äldre formatet från den tidigare versionen av ECE-Tools 2002.0.x stöds fortfarande. Du måste dock uppdatera till det nya formatet för att kunna använda den scenariobaserade distributionsfunktionen. Se [Scenariobaserade distributioner](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>Granska [bakåt innan du uppdaterar till ECE-Tools version 2002.1.0   inkompatibla ändringar ](backward-incompatible-changes.md) om du vill veta mer om ändringar som kan kräva att du   uppdatera Adobe Commerce om projektkonfiguration eller processer för molninfrastruktur.

- ![ny ikon](../../assets/new.svg) **Tjänstuppdateringar**—

   - ![ny ikon](../../assets/new.svg) Stöd för PHP 7.3 har lagts till.<!--MAGECLOUD-4022-->

   - ![ny ikon](../../assets/new.svg) Stöd för RabbitMQ 3.8 har lagts till.<!--MAGECLOUD-4674-->

   - ![ny ikon](../../assets/new.svg) Valideringen har lagts till för att kontrollera installerade tjänstversioner mot EOL-datumet för varje tjänst. Nu får kunderna ett meddelande om en tjänstversion är inom tre månader efter EOL-datumet och en varning om EOL-datumet redan har infallit.<!--MAGECLOUD-4076-->

   - ![korrigeringsikon](../../assets/fix.svg) Ett konfigurationsfel för Elasticsearch har korrigerats för att kontrollera att rätt Elasticsearch-inställningar har konfigurerats i alla miljöer.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>I [tjänstversionerna](../services/services-yaml.md#service-versions) finns en lista över tjänster som används i Adobe Commerce i molninfrastruktur och versionskompatibiliteten med molnmallen.

- ![ny ikon](../../assets/new.svg) **Uppdateringar för miljövariabeln**—

   - ![ny ikon](../../assets/new.svg) Utökade funktionerna för miljövariabeln `WARM_UP_PAGES` så att den stöder cacheförinläsning för specifika produktsidor. Se den utökade definitionen i avsnittet [variabler](../environment/variables-post-deploy.md#warm_up_pages) efter distribution.<!--MAGECLOUD-4444-->

   - ![ny ikon](../../assets/new.svg) Lagt till miljövariabeln `ERROR_REPORT_DIR_NESTING_LEVEL` för att förenkla datahanteringen för felrapporter i katalogen `<magento_root>/var/report/`. Se variabelbeskrivningen i avsnittet [build variables](../environment/variables-build.md#error_report_dir_nesting_level).

   - ![korrigeringsikonen](../../assets/fix.svg) `SCD_EXCLUDE_THEMES`, `STATIC_CONTENT_THREADS`,`DO_DEPLOY_STATIC_CONTENT` och `STATIC_CONTENT_SYMLINK` miljövariablerna har tagits bort. Se [Inkompatibla ändringar bakåt](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![korrigeringsikon](../../assets/fix.svg) Ett fel i konfigurationsprocessen för Elastic Suite har korrigerats så att standardkonfigurationen skrivs över som förväntat när du konfigurerar variabeln `ELASTICSUITE_CONFIGURATION` deploy utan alternativet `_merge` .<!--MAGECLOUD-4388-->

- ![ny ikon](../../assets/new.svg) **CLI-kommandouppdateringar**—

   - ![ny ikon](../../assets/new.svg) **Nytt cron-kommando** - Nu kan du hantera kron-bearbetning manuellt i din Adobe Commerce i molninfrastrukturmiljö med kommandona `cron:disable` och `cron:enable` . Använd kommandot disable (inaktivera) för att stoppa alla aktiva cron-processer och inaktivera alla cron-jobb. Använd kommandot enable för att återaktivera cron-jobb när du är klar. Se [Inaktivera cron-jobb](../application/crons-property.md#disable-cron-jobs).

   - ![ny ikon](../../assets/new.svg) **Förbättrad felrapportering** - Förbättrad loggning har lagts till för CLI-kommandofel som inträffar under ECE-verktygsbearbetning.<!--MAGECLOUD-4849-->

   - ![ny ikon](../../assets/new.svg) **Ta bort inaktuella byggkommandon**- Följande byggkommandon har tagits bort: `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump` och `ece-tools docker`-kommandon har bytt namn till `ece-docker`. Se [Inkompatibla ändringar bakåt](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![ny ikon](../../assets/new.svg) Borttagen den borttagna `build_options.ini`-filen och tillagda valideringen för att misslyckas med bygget om filen finns. Använd filen [.magento.env.yaml](../environment/configure-env-yaml.md) för att konfigurera byggalternativ.

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som gjorde att byggprocessen misslyckades när filen `config.php` var tom har åtgärdats.<!--MAGECLOUD-4127-->

## 2002.0.23

Releasedatum: 27 februari 2020

- ![korrigeringsikon](../../assets/fix.svg) Ett kompatibilitetsproblem med `ece-tools` 2002.0.x-versioner som förhindrar att statiskt innehåll genereras korrekt vid behov i produktionsläge har åtgärdats.

## Äldre versioner

Se arkivet med [versionsinformation](cloud-release-archive.md) för version 2002.0.22 och tidigare.
