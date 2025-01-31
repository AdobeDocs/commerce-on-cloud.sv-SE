---
title: Intag av data
description: Lär dig hur du visar och hanterar datainhämtning från Commerce i New Relic.
feature: Cloud, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# Intag av data

New Relic är beroende av omfattande data för att kunna tillhandahålla effektiv övervakning och analys, men stora datauppsättningar kan påverka resultaten, prestandan och efterlevnaden i rätt tid. I det här avsnittet finns vägledning om hur du hanterar datainmatning och strategier för att förfina data så att de blir så effektiva som möjligt.

New Relic tillhandahåller en _datahanteringsvy_ som sammanfattar din plananvändning per datakälla.

**Så här visar du dina importdata och -källor**:

1. Klicka på **[!UICONTROL Manage your data]** på New Relic-användarmenyn.
1. Klicka på **[!UICONTROL Data management]** i listan _Administration_.

   ![Datahantering](../../assets/new-relic/data-ingestion.png)

   Fliken **[!UICONTROL Data ingestion]** visar data som har importerats för dagen och datakällan för data.
Fliken för datalagring visar och styr hur länge data lagras.

1. Välj fliken **[!UICONTROL Limits]** och se begränsningarna för ditt konto.

Datakällor för Adobe Commerce:

- **APM-händelser** - händelsedata används i diagram och instrumentpaneler
- **Infrastruktur** - bearbeta och lagra mätvärden, som CPU, lagring, nätverk
- **Loggning** - loggar för CDN, APM och programserver

Loggdata bidrar till en stor del av intaget. Se hur du [visar och analyserar loggdata](log-management.md#view-and-analyze-log-data) och samarbetar med din Adobe-representant för att utforma en strategi för datainhämtning och kvarhållningsbehov. Läs mer om [att hantera datainhämtning](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/) i _New Relic-dokumentationen_.
