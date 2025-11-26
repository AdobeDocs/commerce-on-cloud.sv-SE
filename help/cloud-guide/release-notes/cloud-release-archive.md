---
title: Arkiv med veringsanteckningar för e-postverktyg
description: Lär dig mer om arkiverade förbättringar för verktygen.
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
exl-id: 3ba39fa6-88e9-4177-956d-f3e382bf59e3
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '7145'
ht-degree: 0%

---

# Arkiv med veringsanteckningar för e-postverktyg

>[!NOTE]
>
>Versionsinformationen innehåller information och uppdateringar för `ece-tools` v2002.0.22 och senare. Se [Versionsinformation för Cloud Tools Suite](cloud-tools-suite.md) för att få de senaste uppdateringarna för `ece-tools` och andra Cloud-paket.

## v2002.0.22

`ece-tools` 2002.0.22-versionen ändrar strukturen för `ece-tools`-paketet så att `Adobe Commerce on cloud infrastructure`-patcharna inte längre kan frigöras från ECE-verktygsversionen. Från och med den här versionen levereras korrigeringar och viktiga korrigeringar med paketet [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches), som är ett nytt beroende för paketet `ece-tools`. Vi har gjort dessa ändringar för att minska komplexiteten när det gäller att schemalägga uppdateringar och arbeta med bidrag från communities.

- ![ny ikon](../../assets/new.svg) **Ändringar i ECE-verktygspaketet**

   - ![ny ikon](../../assets/new.svg) Flyttade Adobe Commerce-korrigeringarna från `ece-tools`-paketet till ett nytt [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches)-dispositionspaket.

   - ![ny ikon](../../assets/new.svg) Uppdaterade filen `composer.json` för paketet `ece-tools` för att lägga till ett beroende för paketet `magento/magento-cloud-patches` v1.0.0.

   - ![korrigeringsikonen](../../assets/fix.svg) Ett problem som orsakade att `ece-tools`-korrigeringsprocessen bröts när korrigeringsuppsättningar tillämpades ovanpå säkerhetsuppdateringar, med början i version 2.3.2-p2 och senare. Problemet uppstod med det nya versionshanteringsschemat som används för [patchar med endast säkerhet](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/security-patches/overview).<!--MAGECLOUD-4661-->

- ![korrigeringsikon](../../assets/fix.svg) **Korrigerar och viktiga korrigeringar**-Uppdatera dina molnmiljöer med `ece-tools` version 2002.0.22 för att använda följande korrigeringar och viktiga korrigeringar. Dessa korrigeringar ingår i paketet `magento/magento-cloud-patches` v1.0.0.

   - ![korrigeringsikon](../../assets/fix.svg) **Page Builder-säkerhetspatchar för version 2.3.1.x och 2.3.2.x**-Åtgärdar ett fel i Page Builder-förhandsvisningen som gör att oautentiserade användare kan komma åt vissa mallmetoder som kan användas för att aktivera godtycklig kodkörning över nätverket (RCE), vilket resulterar i globala informationsläckor. Problemet kan uppstå om du använder versioner av Page Builder som inte stöds i Adobe Commerce version 2.3.1 och 2.3.2.<!--MAGECLOUD-4649-->

   - ![korrigeringsikon](../../assets/fix.svg) **MSI-korrigeringar**-Åtgärdar problem som orsakade indexeringsfel och prestandaproblem när standardlagerinställningarna för lagerhantering användes.<!--MAGECLOUD-4428-->

   - ![korrigeringsikon](../../assets/fix.svg) **Bakåtkompatibilitet för nya e-postgränssnitt**-Åtgärdar ett bakåtkompatibilitetsproblem som orsakas av `Magento\Framework\Mail\EmailMessageInterface` PHP-gränssnittet som introducerades i Adobe Commerce v2.3.3. I den här korrigeringens omfång ärvs de nya `EmailMessageInterface` från den gamla `MessageInterface` och Adobe Commerce kärnmoduler återställs till att vara beroende av `MessageInterface`.<!--MAGECLOUD-4422-->

   - ![korrigeringsikon](../../assets/fix.svg) **Katalognumrering fungerar inte i Elasticsearch 6.x**-Korrigerar ett kritiskt problem med sökresultatnumrering som påverkar kunder som använder Elasticsearch 6.x som katalogsökmotor.<!--MAGECLOUD-4448-->

## v2002.0.21

- ![ny ikon](../../assets/new.svg) **Docker-uppdateringar**—

   - ![ny ikon](../../assets/new.svg) **Nya dockningsbilder** - Stöds av version 2.3.3 och senare<!-- MAGECLOUD-3345 -->

      - PHP version 7.3.<!-- MAGECLOUD-4017 -->

      - Finska cacheminnet 6.2.0<!-- MAGECLOUD-4017 -->

   - ![ny ikon](../../assets/new.svg) Stöd har lagts till för att tillämpa anpassad krokkonfiguration som anges i `.magento.app.yaml` i Docker-miljön. Tidigare hade Docker-miljön bara stöd för standardkonfigurationen för krok.<!-- MAGECLOUD-3505-->

   - ![ny ikon](../../assets/new.svg) Docker ENV-filer genereras inte längre under Docker-bygget och kommandot `docker:config:convert` är föråldrat. Motsvarande data lagras nu i filen `docker-compose.yml`.<!-- MAGECLOUD-3816-->

   - ![ny ikon](../../assets/new.svg) **PHP-bilden**-Added Node.js to the PHP Docker image to support node, npm, and grunt-cli capabilities.<!-- MAGECLOUD-3953 -->

- ![ny ikon](../../assets/new.svg) **Uppdateringar för miljövariabeln**-

   - ![ny ikon](../../assets/new.svg) LOCK_PROVIDER **distribueringsvariabeln** LOCK_PROVIDER har lagts till för att konfigurera låsprovidern som förhindrar att dubblettcron-jobb och cron-grupper startas. Se variabelbeskrivningen i avsnittet [distribuera variabler](../environment/variables-deploy.md#lock_provider).<!-- MAGECLOUD-4052 -->

   - ![ny ikon](../../assets/new.svg) Lagt till miljövariabeln **CONSUMERS_WAIT_FOR_MAX_MESSAGES** för att konfigurera hur konsumenterna ska bearbeta meddelanden från meddelandekön när miljövariabeln `CRON_CONSUMERS_RUNNER` används för att hantera kronijobb. Se variabelbeskrivningen i avsnittet [distribuera variabler](../environment/variables-deploy.md#consumers_wait_for_max_messages).<!-- MAGECLOUD-4071 -->

   - ![korrigeringsikonen](../../assets/fix.svg) Ett problem som kan orsaka databasdeadlock-fel när `consumers_runner` cron-jobbet startar flera instanser av samma konsument på olika noder har åtgärdats. Om du har aktiverat variabeln [**CRON_CONSUMERS_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) för distribution i din miljö använder jobbet `consumers_runner` alternativet `single-thread` för att starta en instans av varje konsument på endast en nod.<!-- MAGECLOUD-3913 -->

   - ![korrigeringsikon](../../assets/fix.svg) Ett problem som påverkar funktionen [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) som använder en standardlagrings-URL har korrigerats. Om kommandot `config:show:default-url` inte kan hämta en bas-URL används nu URL:en från variabeln MAGENTO_CLOUD_ROUTES.<!-- MAGECLOUD-3866 -->

- ![ny ikon](../../assets/new.svg) Loggningsinformationen som returnerades av kommandot `module:refresh` har uppdaterats. Nu kan du se en detaljerad lista över aktiverade moduler i filen `cloud.log`.<!-- MAGECLOUD-2514 -->

- ![ny ikon](../../assets/new.svg) Förbättrad versionskompatibilitetsvalidering och varningsmeddelanden om kompatibilitetsproblem mellan Adobe Commerce-versionen och installerade tjänster som Elasticsearch, [!DNL RabbitMQ], Redis och DB.<!-- MAGECLOUD-3535 -->

- ![ny ikon](../../assets/new.svg) Stöd för RabitMQ version 3.8 har lagts till.<!-- MAGECLOUD-4674-->

- ![ny ikon](../../assets/new.svg) Uppdaterade interaktiva valideringar för tjänstkompatibilitet för att återspegla vilka versioner som stöds i de nya versionerna av Adobe Commerce 2.3.3 och 2.2.10. Se [Systemkrav](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements) i _Installationsguiden_ för rekommenderade versioner.<!-- MAGECLOUD-4018 -->

- ![korrigeringsikon](../../assets/fix.svg) Förbättrade loggmeddelandet som returnerades när processen för hantering av cron-jobb i distributionsfasen försöker stoppa ett cron-jobb som redan har slutförts för att klargöra att problemet inte är ett fel. Loggnivån har ändrats från `INFO` till `DEBUG`.<!-- MAGECLOUD-3653-->

- ![korrigeringsikonen](../../assets/fix.svg) Ett fel som inte stör distributionsprocessen när ett fel uppstod under `setup:upgrade`-aktiviteten har åtgärdats när kommandot `app:config:import` kördes.<!-- MAGECLOUD-3806 -->

- ![ny ikon](../../assets/new.svg) Ändrade standardloggnivån för filhanteraren till `debug` för att minska detaljmängden i loggen som visas i [!DNL Cloud Console] samtidigt som detaljerad information för felsökning fanns.<!-- MAGECLOUD-3871 -->

- ![korrigeringsikonen](../../assets/fix.svg) Ett fel som orsakade att statiskt innehåll distribuerades under bygget korrigerades. Efter en installation och `ece-tools` config dump uppstod ett fel om ingen språkinställning har angetts för administratörsanvändaren i filen `config.php`. Det finns nu ett standardspråk för administratörsanvändaren i filen `config.php`.<!-- MAGECLOUD-3957 -->

- ![korrigeringsikonen](../../assets/fix.svg) Korrigerade ett `Undefined index error` som inträffar när ett `magento-cloud` CLI-kommando misslyckas i en miljö som inte har konfigurerats med en säker URL (https). Unece-verktygspaketet använder bas-URL:en (http) om den säkra URL:en inte är tillgänglig.<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![ny ikon](../../assets/new.svg) **Docker Updates**—

   - ![ny ikon](../../assets/new.svg) Du kan nu utföra funktionstestning med paketet `ece-tools` i Docker-miljön. Se [programtestning](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing).<!-- MAGECLOUD-3129/3684 -->

   - ![ny ikon](../../assets/new.svg) Stöd för konfiguration av PHP-moduler har lagts till med filen `.magento.app.yaml`. Alla [PHP-tillägg som anges i `.magento.app.yaml` file](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/php-settings#enable-extensions) blir tillgängliga i Docker PHP-behållare.<!-- MAGECLOUD-3357 -->

   - ![ny ikon](../../assets/new.svg) Det finns nya kommandon som förbättrar Docker-kommandoradsupplevelsen. Se avsnittet [`bin/magento-docker` i Docker-referensen ](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference#cloud-docker-cli).<!-- MAGECLOUD-3569 -->

   - ![ny ikon](../../assets/new.svg) Lagt till möjligheten att använda Mutagen.io för att synkronisera filer under utvecklingen mellan den lokala värden och Docker.<!-- MAGECLOUD-3559 -->

   - ![korrigeringsikonen](../../assets/fix.svg) Korrigerade standardsökvägen när Docker-miljön användes. När du nu använder SSH för att logga in i Docker-behållaren finns du som förväntat i projektroten i katalogen `/app`.<!-- MAGECLOUD-3582 -->

   - ![korrigeringsikon](../../assets/fix.svg) Natriumbiblioteket uppdaterades från version 1.0.11 till version 1.0.18 och tillägget Natrium-PHP uppdaterades.<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >Adobe Commerce-kunder med molninfrastruktur måste [skicka in en Adobe Commerce-supportanmälan](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket) för att uppgradera libnatriumpaketet i Pro Production- och Stage-miljöer innan de uppgraderar till Adobe Commerce 2.3.2. För närvarande kan du inte uppgradera Starter-miljöer till Adobe Commerce 2.3.2.

   - ![korrigeringsikon](../../assets/fix.svg) `analysis-icu` och `analysis-phonetic` Elasticsearch-plugin-program har lagts till i alla Docker-bilder.<!-- MAGECLOUD-3446 -->

   - ![korrigeringsikon](../../assets/fix.svg) Förbättrade valideringar: När du använder alternativ för kommandot `docker:build` måste du ange ett värde när du använder ett alternativ. Dessutom lades valideringen för nodversionen till när kommandot `docker:build run` användes.<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![ny ikon](../../assets/new.svg) **Uppdateringar för miljövariabeln**—

   - ![ny ikon](../../assets/new.svg) Stöd för databastabellprefix har lagts till med miljövariabeln [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!-- MAGECLOUD-2901 -->

   - ![ny ikon](../../assets/new.svg) Lagt till variabeln **FORCE_UPDATE_URLS** för distribution av bas-URL:er för uppdatering av Pro- och Starter-produktions- och mellanlagringsmiljöer. Se definitionen i [distribuera variabler](../environment/variables-deploy.md#force_update_urls) -innehållet.<!-- MAGECLOUD-3602 -->

   - ![ny ikon](../../assets/new.svg) Lagt till variabeln **TTFB_TESTED_PAGES** efter distributionen för att konfigurera sidtesterna _Tid till första byte_ för att kontrollera programprestanda på webbplatser som distribueras till molninfrastrukturen. Se variabelbeskrivningen i [variabeln post-deploy](../environment/variables-post-deploy.md).<!-- MAGECLOUD-3643 -->

   - ![korrigeringsikonen](../../assets/fix.svg) Åtgärdade ett problem med flertrådig SCD, som orsakade slumpmässiga fel vid distribution av statiskt innehåll. Den tillfälliga lösningen innebar att variabeln **SCD_THREADS** ställdes in på `1`. Nu kan du öka antalet efter behov. Se definitionerna i [distribuera variabler](../environment/variables-deploy.md#scd_threads) och [build-variablerna](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3611 -->

   - ![korrigeringsikon](../../assets/fix.svg) Du kan konfigurera miljövariabeln **WARM_UP_PAGES** så att den cachelagrar enstaka sidor, flera domäner och flera sidor. Se den utökade definitionen i [variabeln ](../environment/variables-post-deploy.md#warm_up_pages) efter distribution.<!-- MAGECLOUD-3258 -->

- ![korrigeringsikonen](../../assets/fix.svg) har lagt till filen `pub/static/.htaccess` i exkluderingslistan. [Korrigering skickad av Björn Kraus på PHOENIX MEDIA GmbH](https://github.com/magento/ece-tools/pull/455).<!-- MAGECLOUD-3545/Github#455 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett fel korrigerades när alla valideringsmeddelanden visades som `Critical` om minst en validerare på kritisk nivå returnerade ett fel.<!-- MAGECLOUD-3178 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som orsakade ett distributionsfel om bas-URL:en inte fanns i databasen har åtgärdats.<!-- MAGECLOUD-3075 -->

- ![ny ikon](../../assets/new.svg) lade till ett nytt **`env:config:show`-kommando** i paketet `ece-tools` som visar miljötjänster, vägar eller variabler. Se [Tjänster, vägar och variabler](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/ece-tools/package-overview#services-routes-and-variables). [Funktionen har skickats in av Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![korrigeringsikonen](../../assets/fix.svg) Ett fel som orsakade ett kritiskt fel vid försök att installera Adobe Commerce 2.2.6 eller tidigare med `ece-tools` develop efter skalomfaktorisering har åtgärdats.<!-- MAGECLOUD-3665 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som orsakade att installationer av Adobe Commerce 2.1.x och 2.2.x misslyckades med en varning om att använda en föråldrad version av Carbon har åtgärdats.<!-- MAGECLOUD-3704 -->

- ![korrigeringsikon](../../assets/fix.svg) Minskar loggnivån `cloud.log` för gränssnittsutdata från `info` till `debug`.<!-- MAGECLOUD-3277 -->

- ![korrigeringsikonen](../../assets/fix.svg) lade till alternativet `--remove-definers (-d)` i kommandot `ece-tools db-dump` för att ta bort definierare från dumpfilen.<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![korrigeringsikonen](../../assets/fix.svg) Korrigerade ett fel som skrev över `env.php`-filen under en distribution, vilket ledde till att anpassade konfigurationer gick förlorade. Den här uppdateringen ser till att Adobe Commerce i molninfrastrukturen uppdaterar filen `env.php` vid varje distribution, samtidigt som anpassade konfigurationer bevaras.<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![ny ikon](../../assets/new.svg) **Docker Updates**—

   - ![ny ikon](../../assets/new.svg) Nu har Docker-miljön stöd för den cron-konfiguration som definieras i egenskapen [crons i filen .magento.app.yaml](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/crons-property).<!-- MAGECLOUD-3150 -->

   - ![ny ikon](../../assets/new.svg) **Ny dockningsbehållare** - En [TLS-termineringsproxybehållare](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#varnish-container) har lagts till för att underlätta varnish SSL-avslutningen via HTTPS.<!-- MAGECLOUD-2890 -->

   - ![ny ikon](../../assets/new.svg) **Ny dockningsbild** - En Node.js-bild har lagts till som stöd för Gulp och andra funktioner, som JS Unit Testing för Jasmine.<!-- MAGECLOUD-3345 -->

   - ![ny ikon](../../assets/new.svg) **Docker-bygglägen** - Nu kan du välja att starta Docker-miljön i [produktions- eller utvecklarläge](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/#launch-mode). Utvecklarläget stöder aktiv utveckling med fullständig, skrivbar behörighet för filsystemet.<!-- MAGECLOUD-3152/3511 -->

   - ![korrigeringsikon](../../assets/fix.svg) Ett problem som orsakade att Docker-distributionen misslyckades med ett `Name or service not known`-fel om cachen är konfigurerad för en tjänst som inte är tillgänglig har åtgärdats. Nu kan du ta bort en tjänst från [`.magento/services.yaml`-filen ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml). Docker-konfigurationsgeneratorn uppdaterar tjänsten i filen `docker/config.php.dist` automatiskt.<!-- MAGECLOUD-3369 -->

   - ![ny ikon](../../assets/new.svg) har lagt till interaktiva valideringar för tjänstkompatibilitet. Om en begärd tjänst är inkompatibel med Adobe Commerce-versionen eller andra tjänster visar det _interaktiva läget_ ett meddelande och ett alternativ för att fortsätta. Se [Tjänstversionerna](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers) för Docker. Använd alternativet `-n` om du vill hoppa över interaktiviteten för CICD-syften.<!-- MAGECLOUD-3251 -->

   - ![korrigeringsikon](../../assets/fix.svg) Ett fel med Docker compose `db-dump` -kommandot som raderade befintliga dumpar har åtgärdats.<!-- MAGECLOUD-3366 -->

   - ![korrigeringsikon](../../assets/fix.svg) Ett problem som tilldelade Redis `session`, `default` och `page_cache` cachelagring till samma databas-ID har korrigerats.<!-- MAGECLOUD-3172 -->

- ![ny ikon](../../assets/new.svg) **Uppdateringar för miljövariabeln**—

   - ![ny ikon](../../assets/new.svg) Den nya miljövariabeln **ELASTICSUITE\_CONFIGURATION** behåller dina anpassade tjänstinställningar mellan distributioner. Se definitionen i [distribuera variabler](../environment/variables-deploy.md#elasticsuite_configuration) -innehållet.<!-- MAGECLOUD-3205 -->

   - ![ny ikon](../../assets/new.svg) lade till miljövariabeln **SCD_MAX_EXECUTION_TIMEOUT** så att du kan öka tiden för att slutföra distributionen av statiskt innehåll från filen `.magento.env.yaml`. Se definitionen i [distribuera variabler](../environment/variables-deploy.md#scd_max_execution_time), [build-variabler](../environment/variables-build.md#scd_max_execution_time) och [globala variabler](../environment/variables-global.md#scd_max_execution_time).<!-- MAGECLOUD-2822 -->

      - ![ny ikon](../../assets/new.svg) Lagt till miljövariabeln **MAGENTO_CLOUD_LOCKS_DIR** för att konfigurera sökvägen till monteringspunkten för låsprovidern i molninfrastrukturen. Lås-providern förhindrar att duplicerade cron-jobb och cron-grupper startas. Den här variabeln stöds i Adobe Commerce version 2.2.5 och senare och konfigureras automatiskt. Se definitionen i [Cloud-variabler](../environment/variables-cloud.md).<!-- MAGECLOUD-3135 -->

      - ![korrigeringsikon](../../assets/fix.svg) Ändrade standardvärden för miljövariabeln **SCD_THREADS** så att det optimala värdet automatiskt fastställdes baserat på det upptäckta antalet CPU-trådar. Se de uppdaterade definitionerna i [distribuera variabler](../environment/variables-deploy.md#scd_threads) och [build-variablerna](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3382 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem med en korrigering för DB Isolation Mechanism som orsakade ett fel vid uppgradering till Adobe Commerce i molninfrastrukturversionen 2002.0.16 har åtgärdats.<!-- MAGECLOUD-3383 -->

- ![korrigeringsikon](../../assets/fix.svg) En korrigering som ersätter _Google Image Charts_ med _Image-Charts_ har lagts till. Läs DevBlog-artikeln [Borttagning och uppdatering av Google Image Charts för M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006).<!-- MAGECLOUD-3456 -->

- ![korrigeringsikon](../../assets/fix.svg) Valideringen för variabeln [SEARCH_CONFIGURATION](../environment/variables-deploy.md#search_configuration) har lagts till. Distributionen misslyckas när alternativet &quot;engine&quot; inte har angetts och `_merge` inte krävs.<!-- MAGECLOUD-3470 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som exponerade känsliga data efter ett undantag har korrigerats. Nu maskeras den känsliga informationen korrekt.<!-- MAGECLOUD-3525 -->

- ![korrigeringsikon](../../assets/fix.svg) Förbättrade feltoleranta inställningar för Magento Open Source-paketet. Om Adobe Commerce inte kan läsa data från instansen Redis `slave` görs en läsning från instansen Redis `master`. Se [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>`ece-tools` version 2002.0.17 innehåller en viktig säkerhetsuppdatering. Se [Tekniska resurser: Magento Open Source Patches](https://magento.com/tech-resources/download#download2288).

- ![ny ikon](../../assets/new.svg) **Tjänstuppdateringar** - Stöds av följande Adobe Commerce-versioner: 2.2.8 och senare 2.2.x, 2.3.1 och senare 2.3.x

   - Stöd för Elasticsearch version 6.x.<!-- MAGECLOUD-3196 --> har lagts till

   - Stöd för Redis version 5.0 har lagts till.

- ![ny ikon](../../assets/new.svg) **Nya dockningsbilder** - Följande tjänster har lagts till i Docker-bygget:

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![ny ikon](../../assets/new.svg) **Ny miljövariabel** - Tidigare fanns en hårdkodad timeout för SCD-komprimering. Nu kan du konfigurera tidsgränsen för SCD-komprimering med miljövariabeln **SCD_COMPRESSION_TIMEOUT** . Se definitionerna i [build-variablerna](../environment/variables-build.md#scd_compression_timeout) och [distribuera variabler](../environment/variables-deploy.md#scd_compression_timeout) -innehållet.<!-- MAGECLOUD-2870 -->

- ![korrigeringsikonen](../../assets/fix.svg) lade till alternativet `--use-rewrites` i installationskommandot så att det använder omskrivningar från webbservern för genererade länkar i butiken och administratörsåtkomst för att förbättra säkerheten och kundupplevelsen.<!-- MAGECLOUD-3246 -->

- ![korrigeringsikon](../../assets/fix.svg) Tidsstämplar har lagts till i filen `var/log/install_upgrade.log` så att datum för installations- och uppgraderingshändelser visas.<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![ny ikon](../../assets/new.svg) **Docker-uppdateringar**—

   - Standardtjänstkonfigurationen som genereras i Docker-miljön är nu densamma som standardkonfigurationen i molnmallen.<!-- MAGECLOUD-3025 -->

   - Du kan skicka e-post från din Docker-miljö med tjänsten `sendmail`.<!-- MAGECLOUD-2907 -->

   - Lagt till möjligheten att [konfigurera Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug) att felsöka i Cloud Docker-miljön.<!-- MAGECLOUD-2891 -->

   - Ett problem med webbtjänstbehörighet när filen `docker-compose.yml` genererades har korrigerats.<!-- MAGECLOUD-2883 -->

- ![ny ikon](../../assets/new.svg) **Uppgraderingsförbättring** - Verifieringen har lagts till för att bekräfta att egenskapen `autoload` i filen `composer.json` innehåller nödvändiga konfigurationsändringar innan den uppgraderas till Adobe Commerce v2.3. Se [Uppgraderingsversion](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/commerce-version).<!-- MAGECLOUD-2392 -->

- ![ny ikon](../../assets/new.svg) Komprimeringsprocessen vid distribuering av statiskt innehåll omfattar nu alla resurser - internt genererade eller anpassade - och inträffar under byggfasen i början av [`build:transfer` -avsnittet](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property). Tidigare inträffade komprimeringsprocessen innan anpassade miniatyrbilder och paketering av statiska resurser tillämpades. [Korrigering skickad av Rafael Garcia Lepper från Tryzens Limited](https://github.com/magento/ece-tools/pull/413).<!-- MAGECLOUD-3104 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett databasanslutningsfel som inträffade under distributionen omedelbart efter konfigurering av en ytterligare databas- och tjänstrelation har åtgärdats. Den här korrigeringen åtgärdar också ett fel som uppstod under konfigurationsprocessen för Commerce Reporting för Starter. För Starter är den här uppgraderingen ett måste för att kunna använda Commerce Reporting.<!-- MAGECLOUD-3035 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett verifieringsfel med databaskonfigurationen som gjorde att distributionsprocessen misslyckades har åtgärdats.<!-- MAGECLOUD-3003 -->

- ![korrigeringsikon](../../assets/fix.svg) Begränsningen uppdaterades med rätt version av `symfony/yaml`-paketet som ska användas med [PHP-konstanter](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants). Konstantparsning fungerar inte när en `symfony/yaml`-paketversion används tidigare än 3.2. [Korrigering från Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![ny ikon](../../assets/new.svg) **Miljökonfigurationskontroll** - Verifieringen har lagts till för att kontrollera PHP-versionen och varna användarna om de inte använder den senaste rekommenderade versionen.<!--MAGECLOUD-2903-->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem med bearbetning av felformaterade JSON-variabler har korrigerats. Om en JSON-variabel nu orsakar ett syntaxfel visas en varning i filen `cloud.log` och distributionen fortsätter med standardvariabeln.<!-- MAGECLOUD-2851 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett anslutningsfel som inträffade under distributionen omedelbart efter inaktivering av Redis-tjänsten har åtgärdats.<!-- MAGECLOUD-2747 -->

- ![ny ikon](../../assets/new.svg) **Loggningsändringar** - [loggnivån](../environment/log-handlers.md#log-levels) från `Info` till `Notice` har uppdaterats för följande händelser för bygg- och distributionsprocesser:<!--MAGECLOUD-2925-->

   - Starta och avsluta processen för att stämma av installerade moduler i `composer.json` med delade konfigurationsinställningar i filen `app/etc/config.php`

   - Starta och avsluta konfigurationsvalideringsprocessen

   - Början och slutet av processen `setup:di:compile` för att generera klasser

- ![ny ikon](../../assets/new.svg) **Nya miljövariabler**—

   - **[RESOURCE_CONFIGURATION distribuerar variabel](../environment/variables-deploy.md#resource_configuration)** - Använd den här variabeln för att mappa ett resursnamn till en databasanslutning.<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[X_FRAME_CONFIGURATION global variabel](../environment/variables-global.md#x_frame_configuration)** - Använd den här variabeln om du vill ändra rubrikkonfigurationen för `X-Frame-Options` för återgivning av en Adobe Commerce-sida i en `<frame>`, `<iframe>` eller `<object>`.<!-- MAGECLOUD-3048 -->

- ![korrigeringsikon](../../assets/fix.svg) **Uppdateringar för miljövariabeln** - Följande miljövariabler har ändrats:

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)** - Lagt till möjligheten att förhandsladda cachen för angivna sidor på alla domäner som definierats för en Adobe Commerce-butik. Tidigare, om din plats konfigurerades med flera domäner, misslyckades processen efter distributionen med att läsa in cachen för de angivna sidorna i icke-standarddomäner och returnerade följande fel i loggen efter distributionen: `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL** - Dokumentationen och exempelfilen `.magento.env.yaml` har uppdaterats med rätt standardvärden för SCD-komprimeringsnivå. Se definitionerna i [build-variablerna](../environment/variables-build.md#scd_compression_level) och [distribuera variabler](../environment/variables-deploy.md#scd_compression_level) -innehållet.<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES** - Den här miljövariabeln är inaktuell. Använd [SCD_MATRIX](../environment/variables-build.md#scd_matrix) för att styra temakonfigurationen.<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX** - Verifieringsprocessen har korrigerats för att förhindra ett problem som uppstod när SCD_MATRIX ignorerade ett temavärde som innehöll olika teckenfall. Se definitionerna i [build-variablerna](../environment/variables-build.md#scd_matrix) och [distribuera variabler](../environment/variables-deploy.md#scd_matrix) -innehållet.<!-- MAGECLOUD-2904 -->

   - **ADMIN-variabler**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - Förbättrad säkerhet vid hantering av autentiseringsuppgifter för Admin-användaren med hjälp av systemvariabler. Du kan inte längre använda miljövariablerna ADMIN_EMAIL, ADMIN_USERNAME och ADMIN_PASSWORD för att åsidosätta administratörsautentiseringsuppgifter under uppgraderingar. Om du inte kan komma åt administratörspanelen använder du funktionen _Glömt lösenord_ eller kommandot `admin:user:create` CLI för att skapa en ny administratörsanvändare. Se [Öppna din administratörspanel](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/start/onboarding#admin).

      - ADMIN_EMAIL behövs inte längre när du uppgraderar eller använder korrigeringsfiler.

## v2002.0.15

- ![ny ikon](../../assets/new.svg) **Docker-uppdateringar**—

   - Nu använder Docker-generatorn de tjänster som anges i konfigurationsfilerna `.magento.app.yaml` och `.magento/services.yaml` när [du skapar Docker-miljön](https://developer.adobe.com/commerce/cloud-tools/docker/configure/). Du kan välja en annan tjänstversion med build-parametrar.<!-- MAGECLOUD-2888 -->

   - PHP 7.2-bild har lagts till - Stöd för PHP 7.2 har lagts till i Cloud Docker. [Starta Docker-konfigurationen](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) har uppdaterats med alternativet `docker:build --php` för att ange vilken version av PHP som är kompatibel med din version av Adobe Commerce.<!-- MAGECLOUD-2799 -->

   - En [Cron-behållare](https://developer.adobe.com/commerce/cloud-tools/docker/containers/cli#cron-container) har lagts till baserat på PHP-CLI-bilden.<!-- MAGECLOUD-2565 -->

   - Följande tjänster har lagts till i Docker-bygget:

      - [!DNL RabbitMQ] 3.5 och 3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7, 2.4 och 5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2 och 4.0<!-- MAGECLOUD-2886 -->

- ![ny ikon](../../assets/new.svg) **Konfigurera med PHP-konstanter** - Stöd för [PHP-konstanter](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants) har lagts till i konfigurationsfilen `.magento.env.yaml`.<!-- MAGECLOUD- 2575 -->

- ![ny ikon](../../assets/new.svg) **Ny miljövariabel** - Som standard är Google Analytics aktiverat endast i produktionsmiljön. Du kan aktivera Google Analytics i miljö för förproduktion och integrering med miljövariabeln [ENABLE_GOOGLE_ANALYTICS](../environment/variables-deploy.md#enable_google_analytics).<!--MAGECLOUD-2879-->

- ![korrigeringsikonen](../../assets/fix.svg) Ett fel som tog bort anpassade seriekonfigurationer från filen `env.php` efter en omdistribution har åtgärdats. Nu finns anpassade cron-konfigurationer kvar i filen `env.php`.<!-- MAGECLOUD-2923 -->

- ![korrigeringsikon](../../assets/fix.svg) Inkonsekvenser i meddelanden och [loggnivåer](../environment/log-handlers.md#log-levels) har korrigerats för faser för skapande, distribution och efterdistribution. De inledande och avslutande loggmeddelandenivåerna har ökat från **info** till **notice** för alla faser och delfaser. Inledande och avslutande loggmeddelanden har lagts till, där så är lämpligt.<!-- MAGECLOUD-2919 -->

- ![korrigeringsikonen](../../assets/fix.svg) Åtgärdade ett problem med kronprocesser som förhindrade att fasen efter distributionen startades, när den konfigurerades. Om du har aktiverat funktionen för postdistribution aktiveras nu kroniprocesserna igen i början av fasen efter distributionen.<!-- MAGECLOUD-2862 -->

- ![korrigeringsikonen](../../assets/fix.svg) Ett problem som gjorde att Adobe Commerce inte kunde installeras när en anpassad databaskonfiguration angavs har åtgärdats. Tidigare användes databaskonfigurationen från variabeln [MAGENTO_CLOUD_RELATIONSHIP](../environment/variables-cloud.md) av installationsprocessen, även om du har angett anpassad anslutningsinformation i miljövariabeln [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!--MAGECLOUD-2736-->

- ![korrigeringsikonen](../../assets/fix.svg) Kommandot `config:dump` har korrigerats så att det omfattar alla webbplatsspråk i avsnittet `system` i filen `config.php`.<!--MAGECLOUD-2740-->

- ![korrigeringsikon](../../assets/fix.svg) Ett fel som resulterade i _uppvärmningsfel_ under fasen efter distributionen har korrigerats av källans bas-URL-referens.<!--MAGECLOUD-2797-->

- ![korrigeringsikonen](../../assets/fix.svg) Ett fel som genererade filer felaktigt under `setup:di:compile`-processen som påverkade Amazon-betalningsmodulen har korrigerats.<!--MAGECLOUD-2850-->

## v2002.0.14

- ![ny ikon](../../assets/new.svg) **Verifiera idealiskt läge** - Guiden `ideal-state` verifierar nu den aktuella konfigurationen under varje distribution och ger tydliga instruktioner om hur konfigurationen uppdateras för att uppnå en snabbare driftsättning utan driftavbrott.<!--MAGECLOUD-2372-->

- ![korrigeringsikon](../../assets/fix.svg) **PCI-kompatibilitet** - Meddelandeprotokollen för Adobe Commerce i molninfrastruktur har uppdaterats så att TLS (Transport Layer Security) version 1.2 krävs vid anslutning till meddelandetjänster från tredje part. Om du använder en meddelandetjänst som inte stöder TLS version 1.2 måste du uppgradera tjänsten. Annars visas följande felmeddelande när ditt Adobe Commerce-program försöker ansluta till meddelandeservern för att skicka ett e-postmeddelande: `Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![ny ikon](../../assets/new.svg) **Distributionsförbättring** - Verifieringen har lagts till för att varna kunder om alternativen för Förproduktion eller Produktion har aktiverats i `dev`, `debug` eller `debug_logging` för att förhindra prestandaproblem som orsakas av för stor loggningsaktivitet.<!--MAGECLOUD-2517-->

- ![korrigeringsikon](../../assets/fix.svg) **Distributionskorrigeringar**—

   - Nu aktiveras underhållsläget i början av distributionsfasen och inaktiveras i slutet. Om distributionen misslyckas förblir platsen i underhållsläge tills distributionsproblemen har lösts. Tidigare återgick webbplatsen till produktionsläget även om distributionen misslyckades.<!--MAGECLOUD-2603-->

      - Distributionsfasvalideringskontrollerna omarbetades för att nedgradera felnivån för följande distributionsproblem från `CRITICAL` till `WARNING` så att distributionen slutförs. Tidigare orsakade dessa problem att distributionen misslyckades.

      - Miljökonfigurationen innehåller felaktiga värden för distribuerings- eller molnvariabler.

   - Elasticsearch-versionen i molninfrastrukturen är inte kompatibel med den version av modulen elasticsearch/elasticsearch som stöds av Adobe Commerce i molninfrastrukturen. Läs [Elasticsearch felsökningsartikel](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento) i Adobe Commerce Support Knowledgebase.<!--MAGECLOUD-2600-->

   - Ett problem med de delade konfigurationsinställningarna i filen `app/etc/config.php` som orsakade `recursion detected` fel under distributionen har korrigerats.<!--MAGECLOUD-2173-->

- ![korrigeringsikon](../../assets/fix.svg) **Kronrelaterade korrigeringar**—

   - Korrigerat ett problem med cron-schemaläggning som hindrade jobb från att köras om du angav en annan kronifrekvens än standardfrekvensen (1 minut).<!--MAGECLOUD-2602-->

   - Korrigerade ett fel i distributionsfasen som gjorde att kronijobb kunde fortsätta att köras under distributionen, vilket kan orsaka databaslås och andra kritiska problem. Nu stoppas alla cron-jobb innan distributionsfasen börjar och startas om när distributionen är klar.&lt;!—MAGECLOUD—2537—>

   - Korrigerade cron job-arbetsflödet i version 2.2.x för att låsa upp frysta cron-jobb så att de kan stoppas innan distributionen påbörjas. Tidigare avbröts distributionen av ett låst kron.<!--MAGECLOUD-2501-->

- ![korrigeringsikon](../../assets/fix.svg) Ändrade formatet för filen `config.php` som skapades av kommandot `vendor/bin/ece-tools config:dump` till att använda kort matrissyntax och indrag med 4 blanksteg så att den överensstämmer med Adobe Commerce kodningsstandarder.<!--MAGECLOUD-2527-->

- ![korrigeringsikon](../../assets/fix.svg) Ett distributionsfel som inträffar när `.magento.env.yaml` innehåller `{{ base_url }}` och `{{ unsecure_base_url }}` platshållare för webbkonfigurationer i stället för standardkonfigurationen för URL för ett Adobe Commerce-projekt i molninfrastrukturprojekt har åtgärdats./<!--MAGECLOUD-2607-->

## v2002.0.13

- ![ny ikon](../../assets/new.svg) **Aktivera driftsättning utan driftavbrott** - Nu köar Adobe Commerce på molninfrastrukturköer begäranden med nödvändiga databasändringar under distributionen och ändringarna tillämpas så fort distributionen är klar. Förfrågningar kan hållas i upp till 5 minuter för att säkerställa att inga sessioner går förlorade. Se [Statiska alternativ för innehållsdistribution för att minska driftsättningstiderna i molnet](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud).<!--MAGECLOUD-2169-->

- ![ny ikon](../../assets/new.svg) **Docker Compose for Cloud** - Följande förbättringar har gjorts i processen [Docker setup and configuration](https://developer.adobe.com/commerce/cloud-tools/docker/configure/):

   - Ett kommando har lagts till -`docker:config:convert` för att konvertera PHP-konfigurationsfiler till Docker ENV-format för att förenkla miljökonfigurationen. Nu kopierar du PHP-konfigurationsfilerna till Docker-katalogen och konverterar dem till Docker ENV-filer. Se [Starta Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).<!--MAGECLOUD-2359-->

   - Installationsprocessen för Adobe Commerce i molninfrastruktur stöder nu driftsättning i både skrivskyddade och skrivskyddade filsystem för att mer noggrant emulera molnfilsystemet. Se [Konfigurera Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).&lt;!—MAGECLOUD—2357—>

   - **Stöd för Redis-tjänster** - En Redis-bild har lagts till som distribueras till en Docker-behållare och konfigureras automatiskt för att fungera med Docker-installationen.&lt;!—MAGECLOUD—2442—>

   - Nu har du DB-dumpfunktionen när du använder Cloud Docker [databasbehållare](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service#database-container). Du kan också [dela filer](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#sharing-data-between-host-machine-and-container) mellan en värddator och en behållare med hjälp av katalogen `docker/mnt`.<!-- MAGECLOUD-2577 -->

   - **Stöd för lack-tjänster** - En lack-bild som automatiskt distribueras till en Docker-behållare har lagts till. Efter distributionen kan du konfigurera lack manuellt enligt Adobe Commerce bästa praxis. Se [Konfigurera och använda engelska](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/varnish/config-varnish).&lt;!—MAGECLOUD—2358—>

   - Säker åtkomst till webbplatser - SSL-stöd har lagts till för att du ska få åtkomst till Adobe Commerce Store och administratörspanelen.&lt;!—MAGECLOUD—2360—>

- ![korrigeringsikon](../../assets/fix.svg) **Förbättrat stöd för Adobe Commerce i molninfrastrukturtillägg** - Minimiversionskravet för guzzlehttp/guzzle-paketet i Adobe Commerce i molninfrastrukturen [Composer.json-filen](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/overview) har uppgraderats till version 6.2 så att `ece-tools`-paketet är kompatibelt med fler tillägg.<!--MAGECLOUD-2205-->

- ![ny ikon](../../assets/new.svg) **Använd anpassade ändringar i ditt Adobe Commerce-program under byggfasen** - Vi delar upp byggfasen i två separata processer så att du kan använda krokar för att tillämpa anpassade ändringar i det genererade statiska innehållet innan du paketerar programmet för distribution. Processen _build :generate_genererar kod, tillämpar korrigeringar och genererar statiskt innehåll. Processen _build:transfer_ överför den genererade koden och det statiska innehållet till det slutliga målet. Se [Programkopplingar](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property).<!--MAGECLOUD-2363-->

- ![korrigeringsikon](../../assets/fix.svg) **Miljökonfigurationskontroller** - Förbättrad validering av miljökonfigurationen för att varna kunderna om versionsinkompatibilitet och konfigurationsfel innan Adobe Commerce byggs och distribueras i molninfrastrukturen.

   - Versionsspecifik validering har lagts till för att identifiera miljövariabler och -värden som inte stöds eller som är inaktuella.<!--MAGECLOUD-2183-->

   - En kompatibilitetskontroll för Elasticsearch har lagts till för att varna användare om Elasticsearch konfigurationsproblem. Distributionen misslyckas nu om Elasticsearch-tjänstversionen på servern inte är kompatibel med Adobe Commerce. Tidigare slutfördes distributionen även om Elasticsearch-versionen var inkompatibel, vilket orsakade problem med produktkatalogen efter webbplatsdistributionen.<!--MAGECLOUD-2389-->

     Du kan lösa inkompatibiliteten genom att [skicka en supportanmälan](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices) för att uppgradera Elasticsearch till en kompatibel version, eller ändra Adobe Commerce-konfigurationen för att ange en kompatibel version av Elasticsearch PHP-klienten.

      - För Adobe Commerce version 2.1.x till 2.2.2 uppgraderar du Elasticsearch till version 2.4.

      - För Adobe Commerce version 2.2.3 och senare, uppgradera Elasticsearch till version 5.2.

      - Om du har Elasticsearch 1.x eller 2.x och inte vill uppgradera uppdaterar du Adobe Commerce Elasticsearch PHP-klientversionskravet i Composer.json till `"elasticsearch/elasticsearch": "~2.0"`.

   - Förbättrad validering av miljövariabler för att identifiera konfigurationsinställningar som kan orsaka konflikter under bygg-, distributions- och efterdriftfaserna. Ett varningsmeddelande visas till exempel under installations- och uppgraderingsprocessen om den globala inställningen för distribution av statiskt innehåll står i konflikt med inställningarna i bygg- eller distributionsfasen.<!--MAGECLOUD-2156-->

- ![korrigeringsikon](../../assets/fix.svg) **Uppdateringar för miljövariabeln** - Följande miljövariabler har ändrats:

   - **[SKIP_HTML_MINIFICATION global variable](../environment/variables-global.md#skip_html_minification)** - Ändrade standardvärdet till `true` för att aktivera HTML-innehållsminifiering vid behov, vilket minimerar driftstoppen vid distribution till miljö för mellanlagring och produktion. Den här konfigurationen krävs för distributioner utan driftstopp.<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES distribuerar variabeln](../environment/variables-deploy.md#clean_static_files)** - lade till möjligheten att hantera bearbetning av rena statiska filer för statiskt innehåll som genereras under byggfasen baserat på miljövariabelinställningen CLEAN_STATIC_FILES. Tidigare rensades alltid statiska innehållsfiler som genererades under byggfasen.<!--MAGECLOUD-1506-->

- ![korrigeringsikon](../../assets/fix.svg) **Loggning** - Följande ändringar har gjorts för att förbättra loggmeddelanden och minska loggstorleken:

   - Loggposter för distributionsfel inkluderar nu kommandoutdata från de åtgärder som orsakar felen, även om miljökonfigurationen inte anger loggning på felsökningsnivå. Se [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489-->

   - Loggning har lagts till för distributionsfel som inträffar när genererade fabriker som krävs av vissa tillägg inte kan genereras korrekt eftersom filsystemet är i skrivskyddat läge.<!--MAGECLOUD-2209-->

   - Minskad loggstorlek för distributionen och problem med korrigerad formatering orsakade av konfigurationskommandon som använder den interaktiva förloppsindikatorn.<!--MAGECLOUD-2402-->

   - Eliminerade onödig ordalydelse och uppdaterade prioritetsnivåerna för vissa loggsatser.<!--MAGECLOUD-2227-->

- ![korrigeringsikon](../../assets/fix.svg) **Korrigeringsspecifika korrigeringar**—

   - Ändrade standardinställningarna för konfiguration av cron-jobb för historikens livslängd från 3d (4320 min) till 1h (60 min) för att förhindra prestandaproblem och distributionsfel som kan uppstå när cron-kön fylls för snabbt.<!--MAGECLOUD-2427-->

   - Förbättrad process för hantering av cron-jobb under driftsättningsfasen för att förhindra databaslås och andra kritiska problem. Nu stoppas alla cron-jobb under distributionsfasen och de startas om när distributionen har slutförts.<!--MAGECLOUD-2445-->

   - Ett problem med låsmekanismen för schemaläggning av konsumenter som startades av cron-jobb i Adobe Commerce version 2.2.0 och senare har korrigerats för att förhindra att cron-jobb startar dubblettkonsumenter.<!--MAGECLOUD-2464-->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem med den [statiska innehållskomprimeringsprocessen](../environment/variables-intro.md) (`gzip`) som orsakade `not overwritten`- och `no such file or directory`-fel när den komprimerade filen refererades under distributionsprocessen har åtgärdats.<!-- MAGECLOUD-2182-->

- ![korrigeringsikonen](../../assets/fix.svg) Ett problem som hindrade kommandot `php ./vendor/bin/ece-tools config:dump` från att ta bort överflödiga avsnitt från filen `config.php` under dumpprocessen om lagringsspråket inte har angetts har åtgärdats. Nu kan du enkelt flytta konfigurationsfiler mellan miljöer. När du har uppdaterat till `ece-tools` v2002.0.13 kan du återskapa äldre `config.php`-filer med det förbättrade `config:dump`-kommandot. Se [Konfigurationshantering för butiksinställningar](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure-store/store-settings).<!--MAGECLOUD-2444-->

- ![korrigeringsikon](../../assets/fix.svg) Ett fel som orsakade ett fel under distributionsfasen har korrigerats om flödeskonfigurationen i filen `.magento/routes.yaml` dirigerar om från en [apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/)-domän till en `www`-domän.<!--MAGECLOUD-2556-->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem med alternativet `_merge` för variabeln [`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration) som orsakade felaktiga sammanslagningsresultat om du inte inkluderade parametern `engine` i den uppdaterade `.magento.env.yaml`-konfigurationsfilen har åtgärdats. Sammanfogningsåtgärden skriver nu bara över de värden som du anger i den uppdaterade `.magento.env.yaml` utan att du behöver ange parametern `engine`.<!--MAGECLOUD-2520-->

- ![korrigeringsikon](../../assets/fix.svg) Ett Redis-konfigurationsproblem som felaktigt aktiverade sessionslås för Adobe Commerce i molninfrastrukturversion 2.2.1 och senare, vilket kan orsaka långsamma prestanda och timeout har åtgärdats. Sessionslås är nu inaktiverat som standard. Problemet orsakades av en ändring av standardbeteendet för parametern `disable_locking` som introducerades i v1.3.4 i Redis-sessionshanterarpaketet. Se [colinkvart/hour/php-redis-session-abstract package](https://github.com/colinmollenhour/php-redis-session-abstract).<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![ny ikon](../../assets/new.svg) **Docker Compose för Cloud** - lade till ett kommando -`docker:build` - för att generera en [Docker Compose](https://developer.adobe.com/commerce/cloud-tools/docker/configure/) -konfiguration från molndatabasen `ece-tools`.<!-- MAGECLOUD-2250 -->

- ![ny ikon](../../assets/new.svg) **Ändra nationella inställningar** - Nu kan du ändra nationella inställningar för butiker utan att behöva exportera och importera konfigurationsprocessen. Medan programmet är i produktion och SCD_ON_DEMAND är aktiverat är språkinställningarna för lagring och administration tillgängliga.<!-- MAGECLOUD-2019 -->

- ![ny ikon](../../assets/new.svg) <!-- MAGECLOU-1998 -->**Webbplatskarta och robotar** - Skapade ett [arbetsflöde](../store/robots-sitemap.md) för att lägga till en `robots.txt`-fil och generera en `sitemap.xml`-fil för en enskild domänkonfiguration utan att infrastrukturen behöver ändras.

- ![ny ikon](../../assets/new.svg) **Guider** - Två [guider ](../deploy/smart-wizards.md) har lagts till som hjälp för molnkonfigurationen:<!-- MAGECLOUD-1910 -->

   - `ideal-state` - konfigurera det idealiska läget för minimal driftsättning under driftstopp

   - `master-slave` - konfigurera belastningsutjämning för databas och Redis

- ![ny ikon](../../assets/new.svg) **Moduluppdatering** - Lagt till ett molnkommando -`module:refresh` - för att aktivera moduler som inaktiverats eller inte uttryckligen aktiverats, på samma sätt som det görs automatiskt under ett bygge.<!-- MAGECLOUD-1521 -->

- ![ny ikon](../../assets/new.svg) Lagt till möjligheten att välja att sammanfoga eller skriva över konfiguration för tjänster med alternativet `_merge` i konfigurationerna [CACHE](../environment/variables-deploy.md#cache_configuration), [SESSION](../environment/variables-deploy.md#session_configuration), [QUEUE](../environment/variables-deploy.md#queue_configuration) och [SEARCH](../environment/variables-deploy.md#search_configuration).<!-- MAGECLOUD-2105 -->

- ![ny ikon](../../assets/new.svg) **Exempelfil för miljökonfiguration** - Vi lade till en `.magento.env.yaml` exempelfil i ECE-Tools-paketet som innehåller en detaljerad beskrivning och möjliga värden för varje miljövariabel.<!-- MAGECLOUD-1908 -->

   - Vi har även lagt till en djupgående validering för konfigurationen `.magento.env.yaml` som förhindrar fel i distributionsprocessen som orsakas av oväntade värden. När ett fel inträffar får du nu ett detaljerat felmeddelande som börjar med: `Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![ny ikon](../../assets/new.svg) lade till följande [**miljövariabler**](../environment/variables-intro.md):

   - Nu kan du definiera flera språkområden för varje tema med den nya miljövariabeln [SCD_MATRIX](../environment/variables-deploy.md#scd_matrix) , vilket minskar antalet temafiler som ska distribueras.<!-- MAGECLOUD-1501 -->

   - Miljövariabeln [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) har lagts till för att anpassa databasanslutningarna för distribution.<!-- MAGECLOUD-2047 -->

   - Den nya variabeln [MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level) åsidosätter den lägsta loggningsnivån för alla utdataströmmar utan att göra ändringar i koden.<!-- MAGECLOUD-2129 -->

- ![korrigeringsikonen](../../assets/fix.svg) Åtgärdade ett fel som orsakade driftavbrott mellan distributionen och efterdistributionen. Nu börjar fasen efter distributionen _omedelbart_ efter att distributionsfasen har avslutats.

- ![korrigeringsikonen](../../assets/fix.svg) Ett fel som inte rensade de lyckade cron-jobben, de med `status = success`, från schemat har åtgärdats.<!-- MAGECLOUD-2268 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem med kroken `post_deploy` som rensade cachen i distributionsfasen i stället för i projektfasen efter distributionen har korrigerats.<!-- MAGECLOUD-2113 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem har korrigerats vid användning av SCD med flera språkområden, vilket genererade samma `js-translation.json` fil för varje språkområde.<!-- MAGECLOUD-2034 -->

- ![korrigeringsikon](../../assets/fix.svg) Optimerade kommandot `db:dump` i paketet `ece-tools` för att undvika att tabeller låses och öka hastigheten.<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>ECE-verktygen version 2002.0.11 krävs för kompatibilitet med 2.2.4.

- ![ny ikon](../../assets/new.svg) **Konfigurerar skrivskyddade anslutningar till icke-huvudnoder** - I den här versionen finns möjligheten att konfigurera en skrivskyddad anslutning till en icke-huvudnod så att den tar emot skrivskyddad trafik (för [MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)).<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection) och for <!--MAGECLOUD-143 -->

- ![ny ikon](../../assets/new.svg) **Konfigurationsguiden** - En guide har lagts till för att verifiera konfigurationen för statisk innehållsdistribution. Se [Smarta guider](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 -->

- ![ny ikon](../../assets/new.svg) **Stöd för Symfony Console** - Stöd för Symfony Console 4 har lagts till med Adobe Commerce 2.3.<!-- MAGECLOUD-1966 -->

- ![korrigeringsikon](../../assets/fix.svg) **Optimeringar för cron-schemaläggning** - Förbättrad köhantering och förbättrad loggning för felsökning av kron-relaterade problem.<!-- MAGECLOUD-1607 -->

- ![korrigeringsikon](../../assets/fix.svg) Distributionsvalideringen misslyckas om ett `ADMIN_EMAIL`- eller `ADMIN_USERNAME`-värde är detsamma som ett befintligt administratörskonto.<!-- MAGECLOUD-1221 -->

- ![korrigeringsikon](../../assets/fix.svg) SOLR-stöd för 2.2.x-versioner har tagits bort. 2.1.x-versioner behåller möjligheten att aktivera SOLR.<!-- MAGECLOUD-1282 -->

- ![korrigeringsikon](../../assets/fix.svg) Den första installationen av miljöerna Förproduktion och Förproduktion i ett PRO-projekt innehåller nu olika indexprefix för Elasticsearch för att förhindra eventuella konflikter och samtidigt identifiera poster som tillhör respektive miljö.<!-- MAGECLOUD-1489 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett fel som avbröt byggfasen för en äldre arkitektur under distributionen av statiskt innehåll har åtgärdats.<!-- MAGECLOUD-2021 -->

- ![korrigeringsikon](../../assets/fix.svg) **Kronspecifika förbättringar** - Kronimplementeringen har omarbetats:<!-- MAGECLOUD-1607 -->

   - Korrigerade ett problem som gjorde att cron-kön snabbt fylldes. Nu kan man ta bort de gamla cron-jobben på ett mer tillförlitligt sätt.

   - Ordnade om sekvensen av kroniska jobb så att alla jobb i separata trådar startar före den allmänna gruppen.

   - Förbättrad loggning för att underlätta felsökning av kron.

   - **OBS!** - Den här versionen åtgärdar många kronrelaterade problem. Om du för närvarande använder vissa kronrelaterade korrigeringar i _m2-snabbkorrigeringar_ tar du bort dem.

- ![korrigeringsikon](../../assets/fix.svg) **SCD-specifika förbättringar**—

   - Du kan använda miljövariablerna `VERBOSE_COMMANDS` och `SCD_COMPRESSION_LEVEL` under både faserna _build_ och de_ploy.<!-- MAGECLOUD-1819 -->

   - Korrigerade ett problem som gjorde att distributionen misslyckades med ett slumpmässigt fel när ett oväntat värde för miljövariabeln `SCD_COMPRESSION_LEVEL` påträffades. Förbättrad konfigurationsvalidering för att ge meningsfulla meddelanden. Se [`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level) för godtagbara värden.<!-- MAGECLOUD-2043 -->

   - Åtgärdade beteendet för konfigurationsflödet för miljövariabeln `SCD_COMPRESSION_LEVEL` så att åsidosättningarna fungerar som förväntat.<!-- MAGECLOUD-2044 -->

   - Ett problem som förhindrade konfigurationen av miljövariabeln `SCD_THREADS` i `.magento.env.yaml` file _deploy_ stage har korrigerats.<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![ny ikon](../../assets/new.svg) **Statisk innehållsdistribution (SCD)** - Det finns en ny, alternativ distributionsprocess för att generera statiskt innehåll när det efterfrågas (on-demand). Detta minskar driftstoppen och förbättrar cachehanteringen genom att generera de mest kritiska resurserna.<!-- MAGECLOUD-1285 -->

   - **Ny miljövariabel** - Den globala miljövariabeln `SCD_ON_DEMAND` har lagts till för att generera statiskt innehåll vid begäran.<!-- MAGECLOUD-1738 -->

   - **Koppling efter distribution** - En `post_deploy`-krok för filen `.magento.app.yaml` har lagts till som rensar cachen och läser in cachen _i förväg_ när behållaren har accepterat anslutningar. Det är bara tillgängligt för Pro-projekt som innehåller miljöer för mellanlagring och produktion i [!DNL Cloud Console] och för Starter-projekt. Detta fungerar tillsammans med miljövariabeln `SCD_ON_DEMAND`, även om det inte krävs.<!-- MAGECLOUD-1788 -->

- ![ny ikon](../../assets/new.svg) **Optimering** - Optimerad flyttning eller kopiering av filer under distributionen för att förbättra distributionshastigheten och minska inläsningen i filsystemet.<!-- MAGECLOUD-1842 -->

- ![ny ikon](../../assets/new.svg) **Distributionsloggning** - Lagt till möjlighet att aktivera hanterare för Syslog och GELF (Graylog Extended Log Format) för utdataloggar under distributionsprocessen. Se [Loggningshanterare](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->

- ![ny ikon](../../assets/new.svg) lade till följande [**miljövariabler**](../environment/variables-intro.md):

   - `CRYPT_KEY` - Ange en kryptografisk nyckel till en annan miljö när du flyttar en databas.<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION`—_Global_ miljövariabel som hoppar över kopiering av statiska vyfiler i katalogen `var/view_preprocessed` och genererar minifierad HTML när den efterfrågas.<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND`—_Global_-miljövariabel som genererar statiskt innehåll vid begäran.<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES` - Du kan lista de sidor som ska användas för att läsa in cachen i förväg. Tillgängligt i de nya [variablerna efter distribution](../environment/variables-post-deploy.md).

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som innebar att en lokal korrigering som bröt distributionen för en instans korrigerades. ECE-Tools kan nu identifiera att en korrigering har tillämpats.<!-- MAGECLOUD-982 -->

- ![korrigeringsikon](../../assets/fix.svg) Korrigerade en konflikt mellan JavaScript-paketering och GZIP-funktioner. Nu fungerar de här funktionerna tillsammans korrekt.<!-- MAGECLOUD-1735 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som orsakade att CLI-kommandon för ECE-verktyg misslyckades i tidigare PHP 7.0.x-versioner har åtgärdats.<!-- MAGECLOUD-1744 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som förhindrade statisk innehållsdistribution med den kompakta strategin i flera trådar har åtgärdats.<!-- MAGECLOUD-1822 -->

- ![korrigeringsikonen](../../assets/fix.svg) Ett problem med Redis-sessionslås som orsakade en fördröjd administratörsinloggning har åtgärdats. Dessutom är korrigeringen tillgänglig för 2.1.x.<!-- MAGECLOUD-1853 -->

## v2002.0.9

>[!NOTE]
>
>Du måste [uppgradera Adobe Commerce på molninfrastruktursmetapaketet](../dev-tools/install-package.md#update-the-metapackage) för att få tillgång till detta och alla framtida uppdateringar.

- ![new icon](../../assets/new.svg) **ece-tools** - Paketet `ece-tools` har nu stöd för Adobe Commerce 2.1.x.<!-- MAGECLOUD-1086 -->

- ![ny ikon](../../assets/new.svg) **Redis-konfiguration** - Nu kan du [konfigurera Redis](../environment/variables-deploy.md#cache_configuration)-sidan och standardcachen samt Redis-sessionslagring med en miljövariabel.<!-- MAGECLOUD-1552 -->

- ![ny ikon](../../assets/new.svg) **Förbättringar av tjänsterna Search, AMQP och Redis** - Tjänstkonfigurationsflödet har förenats så att det nu fungerar på samma sätt för alla tjänster. Det går inte längre att redigera filen `env.php` manuellt för att konfigurera tjänster. Du måste använda miljövariabler eller filen `.magento.env.yaml` i stället.<!-- MAGECLOUD-1437 -->

- ![korrigeringsikon](../../assets/fix.svg) **Miljövariabler**—

   - Användningen av `env:STATIC_CONTENT_THREADS` har tagits bort och kommer att tas bort i en framtida version. Använd [SCD_THREADS](../environment/variables-deploy.md#scd_threads) i stället.<!-- MAGECLOUD-1507 -->

   - Miljövariabeln `STATIC_CONTENT_EXCLUDE_THEMES` har tagits bort. Du måste använda miljövariabeln `SCD_EXCLUDE_THEMES` i stället.<!-- MAGECLOUD-1640 -->

- ![korrigeringsikon](../../assets/fix.svg) **Loggning** - Vi förenklade loggningen runt inbyggda korrigeringsåtgärder.<!-- MAGECLOUD-1674 -->

- ![korrigeringsikon](../../assets/fix.svg) Vi har tagit bort `developer`-lägesstöd och miljövariabeln `APPLICATION_MODE` eftersom de orsakade oväntat beteende.<!-- MAGECLOUD-1615 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som orsakade statiska innehållsdistributionsfel relaterade till Redis har åtgärdats. Distributionen av statiskt innehåll med flera trådar körs nu som avsett.<!-- MAGECLOUD-1630 -->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som hindrade användare från att spara ändringar i konfigurationsfält i Admin, som markerats som känsliga efter att kommandot `app:config:dump` körts, har åtgärdats.<!-- MAGECLOUD-1175 -->

- ![korrigeringsikon](../../assets/fix.svg) Vi har lagt till stöd för en tidigare version av `symfony/yaml` för att åtgärda konflikter med vissa paket som ännu inte är kompatibla med den senaste versionen.<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>Vi sammanfogade `vendor/magento/ece-patches` med `vendor/magento/ece-tools` i den här versionen. Du behöver inte längre uppdatera paketet `vendor/magento/ece-patches` separat.

**Nya funktioner:**

- **Förbättrad loggning**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - Vi förbättrade loggmeddelanden för att ge bättre förklaringar när bygg- eller distributionsprocessen åsidosätter en miljövariabel.
   - Du kan nu se installations- och uppgraderingsförloppet i realtid. Anpassa filen `install_update.log` för att visa förloppet. Exempel:

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **Nytt cron-kommando** - Nu kan du låsa upp specifika fastnade cron-jobb i stället för att stoppa och starta om alla med kommandot [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451). Inte tillgängligt i 2.1.<!-- MAGECLOUD-1367 -->

- **Enhetlig konfigurationsfil** - Nu kan du konfigurera bygg- och distributionsfaser med hjälp av en [`.magento.env.yaml`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml)-fil.<!-- MAGECLOUD-1369 -->

- **Säkerhetskopiera konfigurationsfiler** - Distributionsprocessen skapar nu automatiskt en säkerhetskopia av konfigurationsfilerna `app/etc/env.php` och `app/etc/config.php` efter distributionen. Vi lade också till ett [nytt CLI-kommando](https://support.magento.com/hc/en-us/articles/360033182871) för att återställa dessa konfigurationsfiler från en säkerhetskopia.<!-- MAGECLOUD-1372 -->

- **Felsökning av valideringsfel** - Vi ändrade det kommando som du måste använda för att lösa valideringsfel när `config.php` inte innehåller tillräckligt med data för statisk innehållsdistribution. Tidigare instruerades du av felmeddelandet att köra `bin/magento app:config:dump`. Nu måste du köra `php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 -->

- **Nya miljövariabler** - Nu kan du använda miljövariabler för att ansluta anpassade [sök](../environment/variables-deploy.md#search_configuration)- och [AMQP-baserade](../environment/variables-deploy.md#queue_configuration)-tjänster till din webbplats.<!-- MAGECLOUD-1410 -->

- Vi implementerade smarta patchar. Paketet tillämpar nu korrigeringar som inte baseras på Adobe Commerce i molninfrastrukturversionen, utan på den patchade paketversionen.<!--MAGECLOUD-1090-->

**Lösta problem:**

- Ett loggningsproblem som orsakade byggfel har åtgärdats.<!-- MAGECLOUD-1162 -->

- Ett problem som orsakade timeout-undantag vid körning av distributioner i interaktivt läge har åtgärdats.<!-- MAGECLOUD-1389 -->

- Vi har åtgärdat ett problem som orsakade fel när vi använde den kompakta strategin för att generera statiskt innehåll. Inte tillgängligt i 2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485-->

- Ett problem som hindrade distributionsskriptet från att identifiera miljöer för staging och produktion har korrigerats.<!-- MAGECLOUD-1493 -->

- Ett problem som orsakade att nätverksproblem orsakade avbrott i databasanslutningar och orsakade fel under installationen och uppgraderingen har åtgärdats.<!-- MAGECLOUD-1520 -->

- Ett problem har korrigerats som förhindrar dig från att exportera konfigurationsfilerna med `app:config:dump` mer än en gång. Inte tillgängligt i 2.1.<!--  MAGECLOUD-1567  -->

- Ett problem som orsakade en fördröjning av inloggningen för _Admin_ har åtgärdats i Redis-sessionen _locking_. Inte tillgängligt i 2.1.<!--  MAGECLOUD-1582  -->

- Vi har åtgärdat ett implementeringsproblem relaterat till versionshantering som orsakade en konflikt med andra Composer-baserade korrigeringsmoduler.<!-- MAGECLOUD-1450 -->

- Ett problem som orsakade PHP-minnesproblem under importen har korrigerats.<!-- MAGECLOUD-1310 -->

- Korrigering har tagits bort. Korrigeringsfel i `colinmollenhour/credis` v1.6 har åtgärdats för att aktivera stöd för Adobe Commerce i molninfrastruktur 2.2.1. Inte tillgängligt i 2.1.<!-- MAGECLOUD-1033 -->

## v2002.0.7

**Lösta problem:**

- Vi tog bort `var/view_preprocessed`-symbolen för att åtgärda ett problem som orsakade JavaScript-miniatyrbildskonflikter.<!-- MAGECLOUD-1454 -->

## v2002.0.6

**Lösta problem:**

- Ett problem som orsakade `gzip` fel när ett fil- eller katalognamn innehåller blanksteg har åtgärdats.<!-- MAGECLOUD-1413 -->

- Ett problem som hindrade distributionsskript från att identifiera och aktivera modulberoenden har åtgärdats.<!-- MAGECLOUD-1424 -->

## v2002.0.5

**Nya funktioner:**

- **Konfigurera en cron-konsument med en miljövariabel** - Nu kan du konfigurera cron-konsumenter med den nya miljövariabeln `CRON_CONSUMERS_RUNNER`.

- **Konfigurationssökning** - Vi söker nu efter viktiga komponenter under bygg-/distributionsprocessen och stoppar processen om genomsökningen misslyckas, vilket förhindrar onödiga driftavbrott på grund av att platsen är i underhållsläge.

- **Bygg/distribuera meddelanden** - Vi har lagt till en konfigurationsfil som du kan använda för att [konfigurera Slack- och/eller e-postmeddelanden](../environment/set-up-notifications.md) för att skapa/distribuera åtgärder i alla dina miljöer.

- **Statisk innehållskomprimering** - Nu komprimeras statiskt innehåll med [gzip](https://www.gnu.org/software/gzip/) under bygg- och distributionsfaserna. Den här komprimeringen, i kombination med snabb komprimering, minskar storleken på din butik och ökar distributionshastigheten. Om det behövs kan du inaktivera komprimering med ett [build-alternativ](../environment/variables-build.md) eller en [distribueringsvariabel](../environment/variables-deploy.md). Mer information finns i följande avsnitt:

   - [Programmiljövariabler](../application/variables-property.md)

   - [Prestanda för statisk innehållsdistribution](../deploy/static-content.md)

   - [Distributionsprocess](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)

- **Konfigurationshantering** - Nu genereras en `app/etc/config.php`-fil automatiskt i Git-databasen under byggfasen om den inte redan finns. Den automatiskt genererade filen innehåller bara en lista med moduler och tillägg. Om filen redan finns fortsätter byggfasen som vanligt. Om du följer [Configuration Management](../store/store-settings.md) vid ett senare tillfälle uppdaterar kommandona filen utan att ytterligare steg krävs. Mer information finns i [Distributionsprocessen](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices).

- **Databasdumpar** - Vi har lagt till ett `magento/ece-tools` CLI-kommando för att skapa databasdumpar i alla miljöer. I Pro-planproduktionsmiljöer dumpas det här kommandot endast från en av tre noder med hög tillgänglighet, vilket innebär att produktionsdata som skrivs till en annan nod under dumpningen inte kan kopieras. Vi rekommenderar att du sätter programmet i underhållsläge innan du gör en databasdump i produktionsmiljöer. Mer information finns i [Hantering av säkerhetskopiering](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/storage/snapshots).

- **Begränsningar för kroniskt intervall har hävts** - Standardvärdet för kroniskt intervall för alla miljöer som etablerats i områdena us-3, eu-3 och ap-3 är 1 minut. Standardkronidintervallet i alla andra regioner är 5 minuter för Pro Integration-miljöer och 1 minut för Pro Staging- och Production-miljöer. Om du vill ändra dina befintliga cron-jobb redigerar du inställningarna i `.magento.app.yaml` eller skapar en supportbiljett för produktions-/mellanlagringsmiljöer. Mer information finns i [Konfigurera cron-jobb](../application/crons-property.md#set-up-cron-jobs).

**Lösta problem:**

- Ett problem som orsakade långa distributionstider på grund av distributionsprocessen som anropar åtgärden `cache-clean` före statisk innehållsdistribution har åtgärdats.<!-- MAGECLOUD-1327 -->

- Ett problem som orsakade fel under det statiska innehållsgenereringssteget i distributionen i produktionsmiljöer har korrigerats.<!-- MAGECLOUD-1322 -->

- Ett problem har korrigerats som förhindrar att vissa `magento/ece-tools`-kommandon loggar utdata till `stderr`.<!-- MAGECLOUD-1264 -->

- Ett problem har korrigerats som förhindrar att bas-URL-värden i `env.php` uppdateras i förgreningar.<!-- MAGECLOUD-1242 -->

- Ett problem som orsakade att kommandot `magento setup:install` lade till ett osäkert prefix (`http://`) för att skydda bas-URL:er har åtgärdats.<!-- MAGECLOUD-1171 -->

- Ett problem har korrigerats som förhindrar att korrigeringsfel orsakar distributionsfel.<!-- MAGECLOUD-1170 -->

- Ett problem har korrigerats som förhindrar `ece-tools` från att stoppa körningen och utlösa ett undantag om inga korrigeringsfiler kan användas.<!-- MAGECLOUD-1152 -->

- Ett problem som orsakade fel när butiken lästes in efter att HTML-miniatyren aktiverats i Admin har åtgärdats.<!-- MAGECLOUD-1138 -->

## v2002.0.4

**Lösta problem:**

- Du kan nu [manuellt återställa fastnade cron-jobb](https://support.magento.com/hc/en-us/articles/360033099451) med ett CLI-kommando i alla miljöer via SSH-åtkomst. Distributionsprocessen återställer automatiskt cron-jobb.<!-- MAGECLOUD-1355 -->

## v2002.0.3

**Lösta problem:**

- Vi har åtgärdat ett problem som gjorde att sidor gick ut eftersom Redis tog för lång tid att läsa/skriva. Du kan nu använda parametern `disable_locking` i Redis-konfigurationer för att förhindra problemet.<!-- MAGECLOUD-1311 -->

## v2002.0.2

**Lösta problem:**

- Konfigurationsprocessen [!DNL RabbitMQ] hämtar nu alla nödvändiga parametrar automatiskt.<!-- MAGECLOUD-1246 -->

## v2002.0.1

**Nya funktioner:**

- Adobe Commerce i molninfrastrukturen har nu stöd för omfattningar och [statiska strategier för innehållsdistribution](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy). Vi har lagt till parametern `–s` med standardinställningen `quick` för den statiska innehållsdistributionsstrategin. Du kan använda miljövariabeln [SCD_STRATEGY](../environment/variables-deploy.md) för att anpassa och använda dessa strategier med dina bygg- och distributionsåtgärder. Variabeln stöder alternativen `standard`, `quick` eller `compact`. Om du väljer `compact` åsidosätter vi värdet `STATIC_CONTENT_THREADS` med `1`, vilket kan göra distributionen långsammare, särskilt i produktionsmiljöer. Inte tillgängligt i 2.1.<!--- MAGECLOUD-1057 -->

- Vi har skapat en loggfil över miljöer för att hämta in och kompilera bygg- och distributionsåtgärder. Filen `var/log/cloud.log` finns i programmets rotkatalog.<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**Lösta problem:**

- Paketet `ece-tools` har uppdaterats så att det blir kompatibelt med Adobe Commerce i molninfrastruktur 2.2.0 och senare.<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- Ett problem som hindrade `ece-tools` från att stoppa körningen och utlösa ett undantag om inga korrigeringsfiler kan användas har åtgärdats.<!-- MAGECLOUD-1186 -->

- Vi har åtgärdat ett problem som medförde att undantag utlöstes när kompilering av beroendeinjektion (ID) hoppas över under byggen.<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- Ett problem som gjorde att distributionsprocessen skrev över anpassade Redis-konfigurationer i filen `env.php` har åtgärdats.<!-- MAGECLOUD-1019 -->

- Ett problem som orsakade omdirigeringsslingor på grund av inaktivering som standard av säker administratör har åtgärdats.<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>Det här paketet är inte längre kompatibelt med andra versioner av Adobe Commerce i molninfrastrukturen och **bör inte** användas.

### Inledande version

Inledande version av `ece-tools` för Adobe Commerce i molninfrastruktur 2.2.0.
