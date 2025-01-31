---
title: Översikt över konfigurationsfiler
description: Läs om hur du konfigurerar molninfrastrukturmiljön för att stödja driftsättning och hantering av din anpassade Adobe Commerce-butik.
feature: Cloud, Configuration, Services, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Översikt över konfigurationsfiler

I Adobe Commerce molninfrastruktur ingår behållare med program, tjänster och en databas som ger ett komplett system för Adobe Commerce programkodbas och filer.

Du kan konfigurera programinställningar, anslutningsvägar, bygg- och distributionsåtgärder samt meddelanden för att stödja dina projektmiljöer med följande konfigurationsfiler:

| Konfiguration | Filnamn | Beskrivning |
| ------------- | -------- | ----------- |
| [Program](../application/configure-app-yaml.md) | `.magento.app.yaml` | Definierar hur Adobe Commerce ska byggas och driftsättas, inklusive tjänster, krokar och kundjobb. |
| [Miljö](configure-env-yaml.md) | `.magento.env.yaml` | Centraliserar hanteringen av bygg- och distributionsåtgärder i alla miljöer, inklusive Pro Staging och Production, med hjälp av miljövariabler. |
| [Routes](../routes/routes-yaml.md) | `.magento/routes.yaml` | Konfigurera cachelagring, omdirigering och SSI (server-side includes). |
| [Tjänst](../services/services-yaml.md) | `.magento/services.yaml` | Definierar de tjänster som Adobe Commerce använder efter namn och version. Den här filen kan till exempel innehålla versioner av MariaDB, PHP-tillägg, Redis, RabbitMQ och Elasticsearch eller OpenSearch. Du måste öppna en supportanmälan för att överföra dessa ändringar till Pro Plan Staging and Production-miljöer. |
| [PHP-inställningar](../application/php-settings.md#configure-php) | `php.ini` | En valfri fil som kan läggas till i projektet. Inställningarna i den här filen läggs till de som hanteras av molninfrastrukturen. |

{style="table-layout:auto"}

## Konfigurationsuppdateringar för Pro-miljöer

För Adobe Commerce i molninfrastrukturen Pro Staging and Production-miljöer kan du uppdatera många konfigurationsalternativ i din lokala utvecklingsmiljö och implementera ändringarna för dessa miljöer. Du måste dock [skicka in en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) för att kunna uppdatera följande konfigurationsalternativ:

- Installera eller uppdatera tjänster i filen `.magento/services.yaml`.
- Ändra konfigurationen för egenskaperna `mounts` och `disk` i filen `.magento.app.yaml`.

{{pro-self-service-warning}}
