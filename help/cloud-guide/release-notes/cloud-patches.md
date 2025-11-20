---
title: Molnkorrigeringar för Commerce
description: Se en lista över de senaste förbättringarna av Cloud Patches-paketet.
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: a4454ebc-72a4-42c1-b591-6237c97fe913
source-git-commit: a4933ed231f22becd9b0188c6c51f1e5ae5384eb
workflow-type: tm+mt
source-wordcount: '2543'
ht-degree: 0%

---

# Molnkorrigeringar för Commerce

Paketet [Cloud Patches](https://github.com/magento/magento-cloud-patches) innehåller en uppsättning nödvändiga korrigeringar som förbättrar integreringen av alla Adobe Commerce-versioner med molnmiljöer och stöder snabb leverans av viktiga korrigeringar.

Cloud Patches for Commerce-paketet är beroende av ECE-verktygspaketet och installeras och uppdateras när du installerar eller uppdaterar ECE-verktygspaketet. Du kan också använda och hantera molnkorrigeringar för Commerce som ett fristående paket för att tillämpa korrigeringar på ett Adobe Commerce-projekt som inte finns på molnplattformen. I versionsinformationen beskrivs de senaste förbättringarna av det här paketet.

>[!TIP]
>
>Om du vill vara säker på att projektet innehåller alla nödvändiga korrigeringar uppdaterar du till den [senaste versionen av ece-tools](../dev-tools/update-package.md).

>[!NOTE]
>
>Se [Tillämpa korrigeringsfiler](../development/apply-patches.md) för instruktioner om hur du använder korrigeringsfiler i dina projekt.

Paketet `magento/magento-cloud-patches` använder följande versionssekvens: `<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.1.12 {#latest}

Releasedatum: 13 november 2025

- ![korrigeringsikon](../../assets/fix.svg) **Symfoni-paket** - Stöd för de senaste YAML-symbolpaketen har lagts till.<!-- MCLOUD-14020 -->
- ![korrigeringsikon](../../assets/fix.svg) **Laga** - Korrigera [utcheckningen misslyckas när JS-minification och paketering är aktiverade](https://experienceleague.adobe.com/sv/docs/experience-cloud-kcs/kbarticles/ka-27997), vilket beskrivs i *Commerce Knowledgebase* .
- ![korrigeringsikon](../../assets/fix.svg) **Förbättrad kategorivy** - MCLOUD-13752: Förbättra kategorivy.<!-- MCLOUD-13752 | MCLOUD-14139  -->

## v1.1.11

Releasedatum: 9 september 2025

- ![korrigeringsikon](../../assets/fix.svg) **WebAPI**-Fix för CVE-2025-54236.<!-- MCLOUD-14016 -->

## v1.1.10

Releasedatum: 7 augusti 2025

- ![ny ikon](../../assets/new.svg) **PHP 8.4** - Lagt till funktionstester.<!-- MCLOUD-13312 -->

## v1.1.9

Releasedatum: 9 juni 2025

- ![korrigeringsikon](../../assets/fix.svg) **Förbättrad kategorivy** - Förbättra kategorivy.<!-- MCLOUD-13752	 - -->
- ![korrigeringsikon](../../assets/fix.svg) **Förbättrad administratörscache**-Improve-admin-cache-efficient CVE-2025-47110.<!-- MCLOUD-13753	 - -->

## v1.1.8

Releasedatum: 3 juni 2025

- ![fix-ikon](../../assets/fix.svg) **Förbättrad kompatibilitet med 2.4.8**-uppdaterade tredjepartsbibliotek för bättre kompatibilitet med 2.4.8<!-- MCLOUD-13707	 - -->

## v1.1.7

Releasedatum: 5 maj 2025

- ![ny ikon](../../assets/new.svg) **Uppdaterad patch för Commerce 2.4.4 till 2.4.8** - Det här är en uppdaterad patch för [CVE-2025-24434](https://experienceleague.adobe.com/sv/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/increased-execution-time-for-bulk-asynchronous-web-endpoints-post-apsb25-08-security-patch) som släpptes i 1.1.7<!-- MCLOUD-13619 -->

## v1.1.6

Releasedatum: 24 april 2025

- ![ny ikon](../../assets/new.svg) **Uppdaterad patch för Commerce 2.4.4 till 2.4.7** - Det här är en uppdaterad patch för [CVE-2025-24434](https://experienceleague.adobe.com/sv/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08) som släpptes i 1.1.4<!-- MCLOUD-13240 -->

## v1.1.5

Releasedatum: 15 april 2025

- ![ny ikon](../../assets/new.svg) **Lagt till patch för B2B 1.5.2** - Korrigera problemet AVS2E-3833 med B2B-modulen 1.5.2 och MariaDB 10.6<!-- MCLOUD-13605	-->

## v1.1.4

Releasedatum: 13 februari 2025

- ![ny ikon](../../assets/new.svg) **Lagt till patch för Commerce 2.4.4 till 2.4.7** - Uppdateringspatchar [CVE-2025-24434](https://experienceleague.adobe.com/sv/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08).<!-- MCLOUD-13240	 - -->

## v1.1.3

Releasedatum: 6 februari 2025

- ![ny ikon](../../assets/new.svg) **PHP 8.4** - Stöd för PHP 8.4 har lagts till.<!-- MCLOUD-13149	 - -->

## v1.1.2

Releasedatum: 5 november 2024

- ![korrigeringsikon](../../assets/fix.svg) **Lagt till korrigering för Commerce 2.4.4 till 2.4.7** - Den här uppdateringen åtgärdar en allvarlig [CVE-2024-45115](https://experienceleague.adobe.com/sv/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-73) -säkerhetslucka för Adobe Commerce när B2B-modulen används.<!-- MCLOUD-12980 - -->

## v1.1.1

Releasedatum: 5 november 2024

- ![korrigeringsikon](../../assets/fix.svg) **Lagt till korrigering för Commerce 2.4.4 till 2.4.7** - Den här uppdateringen åtgärdar ett kritiskt [CVE-2024-34102](https://experienceleague.adobe.com/sv/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-40-revised-to-include-isolated-patch-for-cve-2024-34102?lang=en) CosmicSting-problem.<!-- MCLOUD-12980 - -->

## v1.1.0

Releasedatum: 7 oktober 2024

- ![korrigeringsikon](../../assets/fix.svg) **Refererad kod** - Stödet för tidigare PHP-versioner (7.4, 7.3, 7.2) och relaterade bibliotek har tagits bort.<!-- MCLOUD-9278 - -->
- ![korrigeringsikon](../../assets/fix.svg) **Uppgraderad Monolog-version** - Stöd för monolog 3.6 har lagts till.<!-- MCLOUD-12855 - -->
- ![korrigeringsikon](../../assets/fix.svg) **Programkorrigering för programserver** - Åtgärdar ett känt fel i GraphQL Application Server. `CatalogGraphQl\\Model\\Config\\AttributeReader` i version 2.4.7 innehöll ett fel som kan leda till att GraphQL begär svar baserat på den gamla attributkonfigurationen.<!-- ACPT-1876 -->

## v1.0.27

Releasedatum: 21 maj 2024

- **Stöd för PHP 8.3** - Den här korrigeringen åtgärdar kompatibilitetsfel mellan PHP 8.3 och Composer-paketets version.

## v1.0.26

Releasedatum: 8 april 2024

- ![ny ikon](../../assets/new.svg) **PHP** - Stöd för PHP 8.3 har lagts till.

## v1.0.25

Releasedatum: 16 januari 2024

- **Cacheförbättringar**-Den här korrigeringen förbättrar effektiviteten i layoutcachen och minskar minnesanvändningen avsevärt för Adobe Commerce version 2.4.4 och senare.<!-- MCLOUD-11514 -->
- **Förbättringar av CRON-jobb**-Den här korrigeringen åtgärdar ett problem där missade jobb i onödan väntar på cron-jobblocks för Adobe Commerce version 2.4.4 och senare.<!-- MCLOUD-11329 -->

## v1.0.24

Releasedatum: 15 september 2023

- **Prestandaförbättring**- Den här korrigeringen åtgärdar ett problem som påverkar prestandan genom att minska antalet gånger som samma distributionskonfigurationer läses in för Adobe Commerce 2.4.6 till 2.4.6-p1<!-- MCLOUD-10604 -->

## v1.0.23

Releasedatum: 31 juli 2023

- **Korrigeringen MCLOUD-10604 har tagits bort**-Den här korrigeringen har flyttats till QPT.<!-- MCLOUD-10736 -->

## v1.0.22

Releasedatum: 19 juni 2023

- **Förbättrad QPT CLI-guide/utdata** - En varning har lagts till i QPT CLI-guiden/utdata som påminner dig om att verifiera korrigeringsinformation och -krav om det finns beroenden.<!-- ACP2E-1963 -->
- **Lagt till patchar för Commerce 2.4.6:**
   - Verifieringen av `regexp cache tag` har korrigerats.<!-- MCLOUD-10226 -->
   - Förbättrade prestanda genom att minska antalet gånger som samma distributionskonfigurationer läses in.<!-- MCLOUD-10604 -->
- **Lagt till korrigeringar för Commerce 2.3.7 till 2.4.6** - Korrigerade ett problem som orsakade en ökning med ett slumpmässigt värde i stället för en ökning med 1 för `catalog_product_entity_*`-tabellerna.<!-- MCLOUD-10032 -->
- **Lagt till korrigeringar för Commerce 2.4.0 till 2.4.6** - Korrigerade ett fel som anger att `The file can't be deleted. Warning!unlink: No such file or directory`, som inträffade när JS/CSS-cache tömdes från administratören.<!-- MCLOUD-10279 -->

## v1.0.21

Releasedatum: 10 mars 2023

- **Utökat stöd för PHP 8.2** - Korrigerade kompatibilitetsproblem med vissa PHP 8.2.x-versioner som stöder Commerce 2.4.6.

## v1.0.20

Releasedatum: 27 oktober 2022

- **Lagt till förbättringar av L2-cache** - Den här korrigeringen åtgärdar ett problem med tömning av den lokala L2-cachen för Commerce version 2.4.0 och 2.4.1.<!-- MCLOUD-7845 -->

## v1.0.19

Releasedatum: 13 september 2022

- **Utökat stöd för PHP 8.1** - Korrigerade kompatibilitetsproblem med vissa PHP 8.1.x-versioner.

## v1.0.18

Releasedatum: 11 augusti 2022

Viktig patch för Adobe Commerce 2.4.5:

- **Problem med beställningar med Braintree-betalningar** - Korrigeringen åtgärdar ett kritiskt problem som förhindrar administratörer från att göra nya beställningar eller beställningar.<!-- MCLOUD-9137 -->

Se [Administratören kan inte skapa en order/ändra ordning när Braintree-betalning är aktiverad](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html?lang=sv-SE).

## v1.0.17

Releasedatum: 24 maj 2022

Åtgärdade begränsningar för säkerhetsuppdateringar i filen `patches.json`.

## v1.0.16

Releasedatum: 31 mars 2022

Viktig patch för Adobe Commerce 2.3.3-p1 och senare:

Uppdaterade patchar för att lösa en **kritisk** -sårbarhet som resulterar i oautentiserad fjärrexekvering av kod.<!-- MCLOUD-8479 -->

Se [Adobe säkerhetsbulletin APSB22-12](https://helpx.adobe.com/se/security/products/magento/apsb22-12.html).

## v1.0.15

Releasedatum: 10 mars 2022

- **Stöd för PHP 8.1** - Stöd för PHP 8.1 har lagts till och stödet för PHP 7.0 och 7.1 har tagits bort.
- **Lagt till korrigering för Adobe Commerce 2.3.3** - Fast valuta som visas på produktsidan.

## v1.0.14

Releasedatum: 13 februari 2022

Viktig patch för Adobe Commerce 2.3.3-p1 och senare:

En korrigering har lagts till för att åtgärda en **kritisk** -sårbarhet som resulterar i oautentiserad fjärrexekvering av kod.<!-- MCLOUD-8461 -->

Se [Adobe säkerhetsbulletin APSB22-12](https://helpx.adobe.com/se/security/products/magento/apsb22-12.html).

## v1.0.13

Releasedatum: 25 oktober 2021

- **Uppdatera Monolog** - Uppdaterade minimiversionen som krävs för paketet `monolog` till `^2.3`.<!-- ACMP-1263 -->
- **Inkompatibel PHP-metod** - Korrigerad inkompatibel PHP-metod för Adobe Commerce version 2.4.3 och 2.3.7-p1.<!-- AC-384 -->
- **PHP-fel** - Korrigerade ett `PHP error 'Undefined variable: errorMessage' ...`-fel som inträffade när en korrigering skulle tillämpas.<!-- ACP2E-138 -->

## v1.0.12

Releasedatum: 12 augusti 2021

Viktig patch för Adobe Commerce 2.4.3 och 2.3.7-p1:

- **Problem med API-hastighetsbegränsning** - Den här korrigeringen åtgärdar en standardhastighetsgräns som förhindrar att webb-API:er bearbetar begäranden med mer än 20 objekt i en array. Den här korrigeringen höjer standardvärdet för hastighetsgränsen. Se versionsinformationen för Adobe Commerce [&#x200B; 2.4.3](https://experienceleague.adobe.com/sv/docs/commerce-operations/release/notes/adobe-commerce/2-4-3#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## v1.0.11

Releasedatum: 29 juli 2021

- **Korrigerat ett problem som orsakats av användning av navigeringsrutan i B2B-lager** - För kunder som har använt navigeringsrutan i B2B-lager åtgärdar den här korrigeringen ett `Undefined offset` -fel som visas på söksidan efter att butiksvyn har växlats.<!--MCLOUD-5287-->

- **PayPal Checkout patch** - Korrigerar ett Adobe Commerce 2.3.7-problem med PayPal Express där det tidigare inköpsorderpriset visas.<!--MC-42674-->

- **Stöd för korrigeringskategori** - Stöd har lagts till för bearbetning av korrigeringskategorier och ursprungskällor som tilldelats kvalitetspatchar. Med kategorierna kan kunderna använda filter och sortering för att hitta korrigeringar snabbare när de använder [kvalitetskorrigeringsverktyget](https://github.com/magento/quality-patches) och det platsomfattande analysverktyget (SWAT). <!--MC-38577-->

## v1.0.10

Releasedatum: 10 maj 2021

- **Kompatibilitet med Adobe Commerce 2.3.7** - Konflikt för matchade dispositionsberoenden vid installation i Adobe Commerce 2.3.7.<!--MC-42131-->
- **Korrigerade ett problem som orsakats av att en paketerad korrigering har använts flera gånger**. Om en paketerad korrigering (en som innehåller andra borttagna korrigeringsfiler) används flera gånger kan de inkluderade borttagna paketen återställas. Alla patchar används nu bara en gång. Om du försöker tillämpa samma paket igen visas ett meddelande om att korrigeringen redan har tillämpats.<!--MC-41912-->
- **Navigeringskorrigering i B2B-lager** - Korrigerade ett annat problem som hindrade navigering i lager från att visa alla produktalternativ när användaren aktiverar den delade B2B-katalogen.<!--MCLOUD-7742-->

## v1.0.9

Releasedatum: 1 februari 2021

- **Navigeringskorrigering i B2B-lager** - Korrigerade problemet som hindrade navigering i lager från att visa alla produktalternativ när den delade B2B-katalogen var aktiverad.<!--MCLOUD-6923-->
- **Kompatibilitet med PHP 7.4** - Korrigerade ett kompatibilitetsproblem med molnkorrigeringar med PHP 7.4.<!--MCLOUD-7367-->
- **Föråldrade korrigeringar blir synliga** - Korrigerade ett molnkorrigeringsproblem där borttagna korrigeringar visas i korrigeringstabellen efter att en ersättningskorrigering som innehåller hela innehållet i den borttagna korrigeringen har tillämpats. Det här kan hända om du har använt en korrigering som kombinerar flera andra korrigeringar.<!--MC-40626-->
- **Tysta fel vid tillämpning av korrigeringar** - Korrigerade ett molnkorrigeringsproblem där `git apply`-kommandot tyst misslyckades med att tillämpa korrigeringar i vissa miljöer.<!--MC-40529-->

## v1.0.8

Releasedatum: 14 oktober 2020

- **Kompatibilitetsuppdateringar för magento/magento-cloud-patches** - Uppdaterade `symfony` och `semver` versionsbegränsningarna i `composer.json`-filen för kompatibilitet med Adobe Commerce 2.4.1 och senare versioner.<!--MCLOUD-7111-->

## v1.0.7

Releasedatum: 14 oktober 2020

- **Redis-korrigeringar för Adobe Commerce 2.3.0 till 2.3.5, 2.4.0** - Redis-korrigeringarna har uppdaterats för att ge stöd för att lägga till produkter i en kategori när en nivå 2-cache implementerades. <!--MCLOUD-6659-->

- **Braintree VBE-korrigering** - Korrigerar ett fel som uppstod när en administratör försökte visa en Braintree-kvittningsrapport. <!--MCLOUD-6684-->

- Nu använder kommandot `ece-patches apply` Unix `patch`-kommandot för att tillämpa korrigeringar om Git inte är tillgängligt på värdsystemet. <!--MCLOUD-7069-->

## v1.0.6

Utgivningsdatum:

- **Redis Patches for Adobe Commerce 2.3.0 - 2.3.4** - Optimize communication and improve performance
   - Minska storleken på nätverksöverföringar mellan Redis och Adobe Commerce
   - Åtgärda konkurrensförhållanden vid inläsning och skrivning av Redis
   - Skriv om bascachekortet för att hantera fel när du sparar
   - Minska Redis CPU-förbrukning<!--MCLOUD-6139-->

- **Redis-korrigeringar för Adobe Commerce 2.3.0 - 2.3.5** - Förbättra prestanda och åtgärda fel
   - Åtgärda implementeringen av cache-låset för att förhindra oändliga lås
   - Förbättra den nuvarande låsmekanismen
   - Implementera signerade lås för att förhindra upplåsning från parallella förfrågningar
   - Åtgärda följande fel som inträffar vid Redis-skrivåtgärden: `OOM command not allowed when used memory > maxmemory`
   - Korrigera bearbetning av rensad cache med taggen `cat_p` som körs under produktuppdateringar <!--MCLOUD-6110-->

- Korrigerade ett problem som orsakade ett fel när den nödvändiga `amzn/amazon-pay-module`-korrigeringen tillämpades på Adobe Commerce i molninfrastrukturprojekt med Adobe Commerce v2.2.6 eller 2.3.5, som inte innehåller den här modulen. Korrigeringsprocessen hoppar över korrigeringen `amzn/amazon-pay-module` om modulen inte är installerad.<!--MCLOUD-6588-->

## v1.0.5

Releasedatum: 26 juni 2020

- **Prestandaförbättringar för Redis** - Lägger till optimeringsfunktioner för Redis i Adobe Commerce version 2.3.3 och 2.3.4. Dessa korrigeringar ingick i Adobe Commerce version 2.3.5.<!--MCLOUD-5771-->

- **New Relic-loggregistrering** - Lägger till det Monolog ProcessorInterface som krävs för att stödja förbättringar av New Relic loggningsfunktioner som introducerats i molnkomponenter i Commerce version 1.0.4. Den här korrigeringen krävs för att installera Adobe Commerce 2.1.x. Om korrigeringen inte tillämpas misslyckas bygget under `di:compile` -processen.<!--MCLOUD-6029-->

## v1.0.4

Releasedatum: 12 maj 2020

- **Amazon Pay-utcheckning** - Åtgärdar ett problem med Amazon Pay-betalningswidgeten som förhindrade kunder från att ändra betalningsmetod i steget _Granska och betalningar_ under utcheckningsprocessen.<!--MCLOUD-5930-->

- **Produktvisning på kategorisida** - Korrigerar ett fel som förhindrade produkter från att visas på kategorisidan i vyn _Visa alla sidor_.<!--MCLOUD-5684-->

- **Page Builder-bildöverföring** - Korrigerar ett Page Builder-gränssnittsproblem som ibland orsakade följande fel när bilder överfördes till bildgalleriet: `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **Undvik onödiga varningar för webbplatskartor** - Lägger till ett nytt försök när fel uppstår under generering av webbplatskartor och hoppar över e-postmeddelanden från kunder i fall där fel kan återställas automatiskt.<!--MCLOUD-3025-->

- **Platsens prestandaförbättring** - Korrigerar ett prestandaproblem med funktionen `Magento\Framework\App\DeploymentConfig\Reader::load` som periodvis har haft långa inläsningstider som påverkar platsens prestanda. <!--MCLOUD-5650-->

- Korrigeringstilldelningen för betalningsmetodkorrigeringar har uppdaterats för betalningsmodulerna i stället för Magento baspaket (magento/magento2-base) så att betalningskorrigeringarna bara tillämpas om betalningsmodulerna finns.<!--MCLOUD-5666-->

- Uppdaterade korrigeringar för kompatibilitet med Magento Open Source.<!--MCLOUD-5701-->

## v1.0.3

Releasedatum: 28 april 2020

- Korrigering för korrigeringen &quot;FPC håller på att inaktiveras under distributioner&quot; som stöd för Adobe Commerce 2.3.5 har lagts till.

## v1.0.2

Releasedatum: 27 februari 2020

Den här versionen innehåller följande korrigeringar och viktiga korrigeringar:

- **Kompatibilitetsuppdateringar för magento/magento-cloud-patches**

   - Versionsbegränsningarna `symfony` och `semver` i filen `composer.json` har uppdaterats för kompatibilitet med Adobe Commerce 2.4 och senare versioner.<!--MAGECLOUD-5127-->

   - Begränsningarna i `composer.json` har uppdaterats för kompatibilitet med `ece-tools` 2002.0.22 och senare 2002.0.x-versioner.

- **PayPal Express Checkout** - Publicerad den 12 februari 2020. Den här korrigeringen åtgärdar ett problem som påverkar beställningar som gjorts med PayPal Express Checkout där leveransadressen för ordern anger ett land som har angetts manuellt i textfältet i stället för valts i listrutan på leveranssidan. Se hela patchbeskrivningen på sidan för hämtning av patchar.

- **Programdistributionskorrigering** - En korrigering har lagts till för att åtgärda ett problem som inaktiverade helsidescachen under distributionsprocessen. Den här programfixen gäller för Adobe Commerce 2.3.2 och senare versioner.

- **Scope-parameter för Async/Bulk API** - Uppdaterade den här korrigeringen för att åtgärda ett syntaxfel i filen `composer.json`. Den här programfixen gäller för Magento Open Source 2.3.1 och 2.3.2. Se hela patchbeskrivningen på sidan för hämtning av patchar.

## v1.0.1

Releasedatum: 6 februari 2020

Vi har tagit med alla Magento Open Source 2.x-patchar från programvarunedladdningssidan i magento/magento-cloud-patches v1.0.1. Om du har kopierat korrigeringar till ditt projekt tidigare bör du ta bort dem för att undvika konflikter.

Den här versionen innehåller följande korrigeringar och viktiga korrigeringar:

- **Åtgärda krondödlägen och förbättra kronlåsning**—

   - Korrigerar ett problem med att vissa kroniska jobb inte körs på grund av ett felaktigt statusvärde i tabellen `cron_schedule`. Nu använder vi Adobe Commerce låsramverk för att kontrollera och uppdatera kundjobbsstatus i stället för att använda tabellen `cron_schedule`. Kronjobb som har avslutats med en felstatus provas igen under nästa kronkörning i stället för att vänta i 24 timmar.

   - Lägger till en _återförsöksåtgärd_ för att undvika dödläge under uppdateringar av data i tabellen `cron_schedule`.

- **Uppdaterat `magento/magento-cloud-patches` för att inkludera alla tillgängliga patchar för Magento Open Source 2.x** - Uppdaterat paketet magento/magento-cloud-patches så att det innehåller alla Magento Open Source 2.x-patchar som finns på sidan för programhämtningar. Om du har kopierat några Magento Open Source-korrigeringar till ditt Adobe Commerce i ett molninfrastrukturprojekt tidigare bör du ta bort dem för att undvika konflikter.<!--MAGECLOUD-4606-->

- **Elasticsearch katalogsidnumreringskorrigering** - Ersatte Elasticsearch katalogsidbrytningskorrigering som levererades i magento/magento-cloud-patches v1.0 med en mer effektiv korrigering.<!--MAGECLOUD-4847-->

- **Page Builder-korrigeringar** - I Cloud-korrigeringar för Commerce 1.0.0 paketerade vi Page Builder-korrigeringar för en känd Page Builder-sårbarhet (RCE) för fjärrexekvering med den initiala korrigeringen som bygger på Adobe Commerce 2.3.3. Vi har uppdaterat dessa korrigeringsfiler med en stabilare implementering som baseras på Adobe Commerce 2.3.4, som innehåller flera optimeringar för att åtgärda problemet.<!--MAGECLOUD-4884-->

  Om du har paketet magento/magento-cloud-patches 1.0.0 är du fortfarande skyddad från Page Builder RCE-säkerhetsluckor. Om du uppdaterar till 1.0.1 eller senare har du en bättre implementering av samma korrigering.

## v1.0.0

Releasedatum: 14 november 2019

Detta är den första versionen av paketet [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches), som är ett nytt beroende för den version av paketet `ece-tools` som är version 2002.0.22 eller senare.

Den här versionen innehåller följande korrigeringar och viktiga korrigeringar:

- **Page Builder-säkerhetspatchar för 2.3.1.x- och 2.3.2.x-versioner** - Åtgärdar ett fel i Page Builder-förhandsvisningen som gör att oautentiserade användare kan komma åt vissa mallmetoder som kan användas för att aktivera godtycklig kodkörning över nätverket (RCE), vilket resulterar i globala informationsläckor. Problemet kan uppstå om du använder versioner av Page Builder som inte stöds i Adobe Commerce version 2.3.1 och 2.3.2.<!--MAGECLOUD-4649-->

- **MSI-korrigeringar** - Åtgärdar problem som orsakade indexeringsfel och prestandaproblem när standardlagerinställningar användes för att hantera lager.<!--MAGECLOUD-4428-->

- **Bakåtkompatibilitet för nya e-postgränssnitt**-Åtgärdar ett bakåtkompatibilitetsproblem som orsakas av PHP-gränssnittet i `Magento\Framework\Mail\EmailMessageInterface` som introducerades i Adobe Commerce v2.3.3. I den här korrigeringens omfång ärvs de nya `EmailMessageInterface` från den gamla `MessageInterface` och Adobe Commerce kärnmoduler återställs till att vara beroende av `MessageInterface`.<!--MAGECLOUD-4422-->

- **Katalognumrering fungerar inte i Elasticsearch 6.x**. Åtgärdar ett kritiskt problem med sökresultatsidnumrering som påverkar kunder som använder Elasticsearch 6.x som katalogsökmotor.<!--MAGECLOUD-4448-->
