---
title: Cloud-komponenter för Commerce
description: Se en lista med de senaste förbättringarna av Cloud Components-paketet.
recommendations: noDisplay, catalog
exl-id: 34aec593-e2ea-4060-a6b9-6f4cb95a11c0
source-git-commit: dcf71ffbdafae46e6a02735c090c33a8fe248bc6
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# Cloud-komponenter för Commerce

Paketet [Cloud Components](https://github.com/magento/magento-cloud-components) innehåller utökade Adobe Commerce Core-funktioner för webbplatser som distribueras i molninfrastrukturen. Detta paket är beroende av ECE-verktygspaketet. Versionsinformationen beskriver de senaste förbättringarna av det här paketet, som är en komponent i [Cloud Tools Suite för Commerce](cloud-tools-suite.md).

Paketet `magento/magento-cloud-components` använder följande versionssekvens: `<major>.<minor>.<patch>`

Versionsinformationen innehåller:

- ![ny ikon](../../assets/new.svg) Nya funktioner
- ![korrigeringsikon](../../assets/fix.svg) Korrigeringar och förbättringar

<!--Add release notes below-->

## v1.1.2 {#latest}

Releasedatum: 3 juni 2025

- ![fix-ikon](../../assets/fix.svg) **Förbättrad kompatibilitet med 2.4.8**-uppdaterade tredjepartsbibliotek för bättre kompatibilitet med 2.4.8<!-- MCLOUD-13707	 - -->

## v1.1.1

Releasedatum: 6 februari 2025

- ![ny ikon](../../assets/new.svg) **PHP 8.4** - Stöd för PHP 8.4 har lagts till.<!-- MCLOUD-13148	 - -->
- ![korrigeringsikon](../../assets/fix.svg) **Åtgärda problem med cacheuppvärm** - Korrigerade ett problem med kategori-URL:er under cacheuppvarningen.<!-- MCLOUD-12454 - -->


## v1.1.0

Releasedatum: 7 oktober 2024

- ![korrigeringsikon](../../assets/fix.svg) **Refererad kod** - Stödet för äldre PHP-versioner 7.4, 7.3, 7.2 och relaterade bibliotek har tagits bort.<!-- MCLOUD-9278 - -->
- ![korrigeringsikon](../../assets/fix.svg) **Uppgraderad Monolog-version** - Stöd för monolog 3.6 har lagts till.<!-- MCLOUD-12855 - -->

## v1.0.14

Releasedatum: 8 april 2024

- ![ny ikon](../../assets/new.svg) **PHP** - Stöd för PHP 8.3 har lagts till.

## v1.0.13

Releasedatum: 10 mars 2023

- ![ny ikon](../../assets/new.svg) **Förbättrat stöd för PHP 8.2** - Korrigerade kompatibilitetsproblem med vissa PHP 8.2.x-versioner som stöder Commerce 2.4.6.

## v1.0.12

Releasedatum: 13 september 2022

- ![korrigeringsikon](../../assets/fix.svg) **Fel vid varning** - Korrigerade ett fel som försökte [varma](../environment/variables-post-deploy.md#warm_up_pages) när sidsynligheten är inställd på [**Inte synlig enskilt**](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-attributes-product#simple-product-csv-file-structure) i administratören, vilket resulterade i `ERROR: Warming up failed: <link to page>` fel i distributionsloggen.<!-- MCLOUD-9134 -->

## v1.0.11

Releasedatum: 4 augusti 2022

- ![korrigeringsikon](../../assets/fix.svg) **Stöd för kompatibilitet med Symfony 5.4 har lagts till** - Korrigeringar för kompatibilitet med Symfony 5.4.<!-- AC-3550 -->

## v1.0.10

Releasedatum: 10 mars 2022

- ![ny ikon](../../assets/new.svg) **Stöd för PHP 8.1** - Stöd för PHP 8.1 har lagts till och stöd för PHP 7.1 har tagits bort.

## v1.0.9

Releasedatum: 25 oktober 2021

- ![korrigeringsikon](../../assets/fix.svg) **Uppdatera Monolog** - Uppdaterade minimiversionen som krävs för paketet `monolog` till `^2.3`.<!-- ACMP-1263 -->

## v1.0.8

Releasedatum: 29 juli 2021

- ![korrigeringsikon](../../assets/fix.svg) **Slutande snedstreck har tagits bort från automatiskt genererade URL:er** - De avslutande snedstrecken från URL:er för kategorisidor som genererats under cachevärmen har tagits bort.<!--MCLOUD-7192-->

## v1.0.7

Releasedatum: 9 september 2020

- ![ny ikon](../../assets/new.svg) **Loggningsförbättringar** - Minska storleken på filen `cache.log` för att förbättra prestanda.<!--MCLOUD-6859-->

- ![korrigeringsikon](../../assets/fix.svg) Ett typfel i cachekonfigurationsvärdena som orsakade att CLI-kommandot `php bin/magento cache:evict` misslyckades har åtgärdats.

## v1.0.6

Releasedatum: 5 augusti 2020

- ![ny ikon](../../assets/new.svg) **Förbättra Redis-prestanda** - Kommandot `./bin/magento cache:evict` har lagts till för att ta bort utgångna Redis-nycklar, vilket minskar Redis minnesanvändning för att förbättra prestandan.<!--MCLOUD-6023-->

- ![korrigeringsikon](../../assets/fix.svg) Stöd för *New Relic-loggar i kontext* har tagits bort för att åtgärda ett prestandaproblem.<!--MCLOUD-6422-->

## v1.0.5

Releasedatum: 25 juni 2020

- ![korrigeringsikon](../../assets/fix.svg) Ett fel som introducerades i magento/magento-cloud-components version 1.0.4 som gjorde att rensningscacheåtgärden misslyckades under distributionsfasen, vilket avbröt distributionsprocessen, har åtgärdats.

## v1.0.4

Releasedatum: 25 juni 2020

- ![ny ikon](../../assets/new.svg) **Implementerade New Relic-loggar i kontext** - Programloggar som genererats av Adobe Commerce visas nu i spår i New Relic för att förbättra felsökningsfunktionerna.<!--MCLOUD-6029-->

- ![ny ikon](../../assets/new.svg) **Förbättrad loggning** - Loggning har lagts till för att spåra cacheogiltigförklaring och fullständiga indexeringshändelser.<!--MCLOUD-6157-->

## v1.0.3

Releasedatum: 27 februari 2020

- ![korrigeringsikon](../../assets/fix.svg) Ett kompatibilitetsproblem som stöder `ece-tools` 2002.0.x-versioner som använder äldre PHP-versioner har åtgärdats.

## v1.0.2

Releasedatum: 6 februari 2020

- ![ny ikon](../../assets/new.svg) Utökade funktionerna för miljövariabeln `WARM_UP_PAGES` så att den stöder cacheförinläsning för specifika produktsidor. En detaljerad funktionsbeskrivning finns i avsnittet [variabler](../environment/variables-post-deploy.md#warm_up_pages) för <!--MAGECLOUD-4444-->.

- ![korrigeringsikon](../../assets/fix.svg) Ett problem har korrigerats där en ogiltig arkiv-URL gör att kroken efter distributionen misslyckas när funktionen `WARM_UP_PAGES` används för att fylla i cachen. Problemet uppstod bara när URL-omskrivning inaktiverades.<!-- MAGECLOUD-4094 -->

## v1.0.1

Releasedatum: 23 juli 2019

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som påverkar funktionen [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) som använder en standardlagrings-URL har korrigerats. Om kommandot `config:show:default-url` inte kan hämta en bas-URL används nu URL:en från variabeln MAGENTO_CLOUD_ROUTES.<!-- MAGECLOUD-3866 -->

## v1.0.0

Releasedatum: 12 juni 2019

Detta är den första versionen av paketet [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components), som är ett nytt beroende för paketversionen `ece-tools` 2002.0.20 och senare.

- ![ny ikon](../../assets/new.svg) lade till möjligheten att använda regex-mönster för att konfigurera miljövariabeln **WARM_UP_PAGES** så att den cachelagrar enstaka sidor, flera domäner och flera sidor. Se [Variabler efter distribution](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
