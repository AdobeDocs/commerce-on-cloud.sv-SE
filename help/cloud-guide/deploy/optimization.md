---
title: Optimera molndistributionen
description: Lär dig mer om hur du kan optimera driftsättningsprocessen för Adobe Commerce i molninfrastrukturprojekt, inklusive att minska driftstopp, driftsättning av statiskt innehåll, scenariobaserad driftsättning och smarta guider.
feature: Cloud, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# Optimera driftsättningen

Webbplatsprestanda kan försämras under driftsättningsprocessen. Hur lång tid en plats är i underhållsläge när den distribueras till en produktionsplats beror på många faktorer, till exempel miljökonfigurationen och mängden innehåll som en plats innehåller. Det första bästa sättet att optimera din molndistribution är att [uppgradera för att använda `ece-tools`](../dev-tools/install-package.md) för att utnyttja paketfunktionerna, till exempel kommandon för att skapa en säkerhetskopia av databasen och verifiera miljökonfigurationen.

Följande ämnen kan hjälpa dig att bättre förstå hur du optimerar distributionsprocessen:

- [Molndistributionsprocess](process.md)
Det finns tre faser i driftsättningsprocessen i molnet, och du kan utnyttja styrkorna och svagheterna i varje fas till din fördel.

- [Ingen driftsättning vid driftstopp](reduce-downtime.md)
Förstå vad som händer under driftsättningen och hur ni minskar antalet driftavbrott i era butiksupplevelser under en uppdatering av produktionsmiljön.

- [Statisk innehållsdistribution](static-content.md)
Det bästa sättet att optimera er molndistribution är att styra hur och när statiskt innehåll ska genereras.

- [Smarta guider](smart-wizards.md)
Paketet `ece-tools` innehåller smarta guidekommandon som snabbt utvärderar projektkonfigurationen.

- [Spåra distributioner med New Relic](../monitor/track-deployments.md)
Använd New Relic-tjänsten för att övervaka driftsättningshändelser och analysera driftsättningseffekten till övergripande prestanda.
