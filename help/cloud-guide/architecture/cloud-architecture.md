---
title: Cloud-arkitektur för Commerce
description: Se hur Starter och Pro Project Architectures kontrast för Commerce i molninfrastrukturen.
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
exl-id: 7c1e895d-0f88-4f11-919a-b3b5748ca5f0
source-git-commit: 5fc2082ca2aae8a1466821075c01ce756ba382cc
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---

# Cloud-arkitektur för Commerce

Adobe Commerce i molninfrastruktur har en Starter- och en Pro-plan. Varje plan har en unik arkitektur som driver utvecklingen och driftsättningen av Adobe Commerce. Både Starter-planen och Pro-planens arkitektur driftsätter databaser, webbservrar och cachningsservrar i flera miljöer för komplett testning med stöd för kontinuerlig integrering.

Som jämförelse innehåller varje plan följande infrastrukturfunktioner och produkter som stöds.

|          | Starter | Pro |
| -------- | --------------------| ------------------ |
| Kärnfunktioner | <ul><li>[Alla Adobe Commerce-funktioner](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html?lang=sv-SE)</li><li>PayPal Onboarding Tool</li><li>[Commerce Reporting](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[Alla Adobe Commerce-funktioner](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html?lang=sv-SE)</li><li>PayPal Onboarding Tool</li><li>[Commerce Reporting](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[B2B-modul](https://business.adobe.com/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| Infrastruktur och driftsättning | <ul><li>Kontinuerliga verktyg för molnintegrering med obegränsat antal användare</li><li>CDN (Fast Content Delivery Network), Image Optimization (IO) och extra säkerhet med generösa bandbreddstillägg. Tjänsten Web Application Firewall (WAF) är endast tillgänglig i produktionsmiljöer.</li><li>[New Relic](../monitor/new-relic-service.md) APM (Performance Monitoring) på tre grenar: `master` och två av dina val<br>Plattformsproduktion, staging och utvecklingsmiljöer (totalt fyra aktiva miljöer) optimerade för Adobe Commerce</li><li>Tryckfiltrering (utgående brandvägg)</li></ul> | <ul><li>Kontinuerliga verktyg för molnintegrering med obegränsat antal användare</li><li>CDN (Fast Content Delivery Network), Image Optimization (IO) och extra säkerhet med generösa bandbreddstillägg. Tjänsten Web Application Firewall (WAF) är endast tillgänglig i produktionsmiljöer.</li><li>[New Relic](../monitor/new-relic-service.md) Infrastructure on Production + APM (Performance Monitoring) on staging and production. [Princip för hanterade aviseringar](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) för Adobe Commerce-principen implementerar övervakning av bästa praxis för att proaktivt informera dig om program- och infrastrukturproblem som påverkar webbplatsens prestanda.</li><li>Plattform som en tjänst (PaaS)-baserad [Integrationsutveckling](pro-architecture.md#integration-environment) -miljö (totalt 2 aktiva miljöer) optimerad för Adobe Commerce</li><li>Infrastruktur som en tjänst (IaaS) - dedikerad virtuell infrastruktur för miljöer med staging och produktion</li></ul> |
| Infrastruktur med hög tillgänglighet | | [Arkitektur med hög tillgänglighet](pro-architecture.md#redundant-hardware) med en konfiguration med tre servrar i den underliggande infrastrukturen som en tjänst (IaaS) för att tillhandahålla tillförlitlighet och tillgänglighet i företagsklass |
| Dedikerad maskinvara | | Isolerad och dedikerad maskinvara i den underliggande infrastrukturen som en tjänst (IaaS) som ger ännu högre tillförlitlighet och tillgänglighet |
| E-postsupport dygnet runt | Övervakning och e-postsupport dygnet runt för kärnapplikationen och molninfrastrukturen | Övervakning och e-postsupport dygnet runt för kärnapplikationen och molninfrastrukturen |
| En dedikerad kundteknisk rådgivare (CTA) | | Dedikerad teknisk kontohantering för den inledande startperioden, med början från prenumerationen tills den första webbplatsstarten |
| Tillägg\* | <ul><li>[B2B-modul](https://business.adobe.com/products/magento/b2b-ecommerce.html)</li></ul> |

\* _Tillgängligt för en extra avgift_

## Startprojekt

Arkitekturen för [Starter-planen](starter-architecture.md) har fyra miljöer:

- **Integration** - Integreringsmiljön innehåller två testbara miljöer. Varje miljö innehåller en aktiv Git-gren, databas, webbserver, cachning, vissa tjänster, miljövariabler och konfigurationer.

- **Förproduktion** - När kod och tillägg godkänns i testerna kan du sammanfoga din `integration` -gren med mellanlagringsmiljön, som blir din testmiljö för förproduktion. Den innehåller den `staging` aktiva grenen, databasen, webbservern, cachningen, tredjepartstjänster, miljövariabler, konfigurationer och tjänster, som Fastly och New Relic.

- **Produktion** - När koden är klar och testad sammanfogas all kod till `master` för distribution till produktionsplatsen. Den här miljön innehåller din aktiva `master`-gren, databas, webbserver, cachelagring, tredjepartstjänster, miljövariabler och konfigurationer.

- **Inaktiv** - Du har ett obegränsat antal inaktiva grenar.

## Pro-projekt

Planarkitekturen [Pro](pro-architecture.md) har en global `master` med tre miljöer:

- **Integration** - Integreringsmiljön innehåller en testbar miljö som innehåller en databas, webbserver, cachelagring, vissa tjänster, miljövariabler och konfigurationer. Du kan utveckla, distribuera och testa koden innan du sammanfogar den till mellanlagringsmiljön.

   - _Inaktiv_ - Du kan ha ett obegränsat antal inaktiva grenar baserat på miljön `integration`, men bara en aktiv gren (inte inklusive `integration` ).

- **Mellanlagring** - Mellanlagringsmiljön är till för förproduktionstestning och innehåller en databas, webbserver, cachelagring, tredjepartstjänster, miljövariabler, konfigurationer och tjänster, t.ex. Snabbt.

- **Produktion** - Produktionsmiljön innehåller en arkitektur med tre noder och hög tillgänglighet för data, tjänster, cachelagring och lagring. Produktionen är en aktiv, offentlig butiksmiljö med miljövariabler, konfigurationer och tredjepartstjänster.

## Programvara och tjänster som stöds

Adobe Commerce i molninfrastrukturen använder:

- Operativsystem: Debian GNU/Linux
- Webbserver: Nginx
- Database: MySQL (MariaDB)
- CDN (Content Delivery Network): Snabbt CDN

Du kan konfigurera följande tjänster:

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [Redis](../services/redis.md)
- [KaninMQ](../services/rabbitmq.md)
- [ActiveMQ](../services/activemq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>Se [Systemkrav](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=sv-SE) i _installationshandboken_ för rekommenderade versioner.

Modulen Snabbt CDN används för CDN och cachelagring i miljöer med staging och produktion. Se [Konfigurera snabbtjänster](../cdn/fastly.md).

Mer information om hur du konfigurerar de programversioner som ska användas i implementeringen finns i följande Adobe Commerce om konfigurationsfiler för molninfrastruktur:

- [Programkonfiguration (.magento.app.yaml)](../application/configure-app-yaml.md)
- [Miljökonfiguration (.magento.env.yaml)](../environment/configure-env-yaml.md)
- [Routningskonfiguration (route.yaml)](../routes/routes-yaml.md)
- [Tjänstkonfiguration (services.yaml)](../services/services-yaml.md)
