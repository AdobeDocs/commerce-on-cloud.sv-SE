---
title: Automatisk skalning
description: Läs om hur Adobe Commerce i molninfrastruktur kan skalas upp för att tillgodose resurskrav.
feature: Cloud, Auto Scaling
topic: Architecture
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---

# Automatisk skalning

Automatisk skalning lägger automatiskt till eller tar bort resurser i molninfrastrukturen för att upprätthålla optimala prestanda och rimliga kostnader. Den här funktionen är för närvarande bara tillgänglig för projekt som konfigurerats med en [skalad arkitektur](scaled-architecture.md).

## Webbservernoder

[Webbnivån](scaled-architecture.md#web-tier) skalas för att passa en ökning av antalet processförfrågningar och högre trafikkrav. För närvarande skalas funktionen för automatisk skalförändring bara vågrätt genom att webbservernoder läggs till eller tas bort.

En automatisk skalningshändelse inträffar när CPU användning och trafik når ett fördefinierat tröskelvärde:

- **Noder tillagda** - CPU:er/kärnor för alla aktiva webbnoder har en kapacitet på 75 % i en minut och trafiken ökar med 20 % under 5 minuter i följd.
- **Borttagna noder** - CPU:er/kärnor för alla aktiva webbnoder läses in till 60 % under 20 minuter. Noderna tas bort i den ordning som de lades till.

Minimi- och maximitröskelvärdena fastställs och fastställs utifrån de avtalade resursgränserna för varje handlare. Detta minskar risken för oändlig skalning.

## Övervaka tröskelvärden med New Relic

Du kan använda [New Relic-tjänsten](../monitor/new-relic-service.md) för att övervaka vissa tröskelvärden, som värdantal och CPU-användning. I följande New Relic-frågor används en variabelnotation för `cluster-id` endast som exempel.

>[!TIP]
>
>En referens om hur du skapar frågor finns i [NRQL-syntax, satser och funktioner](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) i _New Relic_ -dokumentationen.
>Använd dina frågor för att skapa en [New Relic-kontrollpanel](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

### Värdantal

Följande exempel på New Relic-fråga visar värdantalet i miljön:

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

I följande skärmbild avser **APM-värdar som visas** antalet värdar med transaktioner som loggats under den valda perioden.

![New Relic-värdantal](../../assets/new-relic/host-count.png)

### Användning i CPU

Följande exempel på New Relic-fråga visar CPU användning för webbnoder:

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![New Relic webbnoder - CPU-användning](../../assets/new-relic/web-node-cpu-usage.png)

## Aktivera automatisk skalförändring

[Skicka en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) om du vill aktivera eller inaktivera automatisk skalning för ditt Adobe Commerce i molninfrastrukturprojekt. Välj följande orsaker i biljetten:

- **Kontaktorsak**: Begäran om infrastrukturändring
- **Kontaktorsak till Adobe Commerce-infrastruktur**: Annan begäran om infrastrukturändring

>[!IMPORTANT]
>
>Funktionen för autoskalning fångar upp oväntade händelser. Även om automatisk skalförändring är aktiverat rekommenderar Adobe att du fortsätter att [skicka en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) om du förväntar dig en kommande händelse.

### Belastningstestning

Adobe aktiverar automatisk skalning i molnprojektet _staging_ kluster först. När du har utfört och slutfört inläsningstestning i din miljö aktiverar Adobe sedan automatisk skalning i ditt produktionskluster. Mer information om inläsningstestning finns i [Prestandatestning](../launch/checklist.md#performance-testing).

### IP TILLÅTELSELISTA

När automatisk skalning har aktiverats kommer den utgående webbnodstrafiken från tjänstnodernas IP-adresser. Om du använder ett tillåtelselista med en tredjepartstjänst som inte medföljer ditt Adobe Commerce i ett molninfrastrukturprojekt bör du kontrollera IP-adresserna i tillåtelselista för tredjepartstjänsten.

Exempel:

- Om tillåtelselista innehåller IP-adresserna för dina tjänstnoder (1, 2 och 3) behövs ingen åtgärd.
- Om tillåtelselista innehåller IP-adresserna för dina tjänstnoder (1, 2 och 3) och webbnoder (4, 5 och 6) - i det här fallet alla sex noder - krävs ingen åtgärd.
- Om tillåtelselista innehåller IP-adresserna _only_ för dina webbnoder (4, 5 och 6) måste du uppdatera tillåtelselista så att den innehåller IP-adresserna för tjänstnoderna.
