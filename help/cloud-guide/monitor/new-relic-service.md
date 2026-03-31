---
title: New Relic
description: Läs mer om New Relic-tjänsten som finns i ditt Adobe Commerce i molninfrastrukturprojekt.
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
exl-id: 10966241-311d-4b68-804d-4c9569bf933d
source-git-commit: 3784e7b2ddc8f6ae20fd2c6fd557f2408d870cf2
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# Översikt över tjänsten New Relic

Alla Adobe Commerce-projekt i molninfrastrukturen omfattar tillgång till New Relic-tjänsten för att hjälpa till att övervaka prestanda och undersöka händelser i [!DNL Commerce]-programmet och molninfrastrukturen.

Följande New Relic-funktioner kan användas i produktions- och mellanlagringsmiljöer:

- [New Relic APM](#new-relic-apm) (Pro och Starter)
- [New Relic Infrastructure](#new-relic-infrastructure) (endast Pro)
- [New Relic Log Management](#new-relic-log-management) (endast Pro)

>[!INFO]
>
>Andra New Relic-funktioner är inte tillgängliga i Adobe Commerce-projekt.
>
>Adobe Commerce på Cloud-kunder kan inte integrera eller skicka data från externa servrar direkt till sina New Relic-konton. New Relic-tjänsten är begränsad till att övervaka Commerce Cloud programmiljö. Endast tredjepartstjänster som anropas av själva programmet (till exempel externa API:er eller tjänster som anropas under körning) kan spåras och övervakas i New Relic.

## NEW RELIC APM

[New Relic för programprestandahantering (APM)](https://docs.newrelic.com/introduction-apm/) är en programanalysprodukt som hjälper dig att analysera och förbättra programinteraktioner. New Relic APM är tillgängligt för alla Adobe Commerce i molninfrastrukturprojekt och har följande funktioner:

- **Fokusera på specifika transaktioner** - Markera och övervaka aktivt viktiga kundåtgärder på webbplatsen, till exempel tillägg i kundvagnen, utcheckning eller bearbetning av en betalning.
- **Övervakning av databasfråga** - Leta upp och övervaka databasfrågor som påverkar prestandan.
- **Appkarta** - Visa alla programberoenden på din webbplats, tillägg och externa tjänster.
- **[!DNL Apdex]poäng** - Utvärdera prestanda och skapa varningar som identifierar problem och meddelar dig när de inträffar, t.ex. om webbplatsen påverkas av en Flash-försäljning eller en webbhändelse. Se [Indexpoäng](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/).
- **Hanterade aviseringar för Adobe Commerce**-Använd den här New Relic-aviseringsprincipen för att övervaka program- och infrastrukturprestanda baserat på bästa praxis i branschen. Se [Övervaka prestanda med Hanterade aviseringar för Adobe Commerce-aviseringsprincip](investigate-performance.md/#monitor-performance-with-managed-alerts).
- **Spåra distributioner** - Övervaka distributionshändelser och analysera distributionens påverkan på övergripande prestanda. Se [Spåra distributioner](track-deployments.md).

I ditt infrastrukturprojekt för Adobe Commerce i molnet ingår programvaran för New Relic APM-tjänsten tillsammans med en licensnyckel. Du behöver inte köpa eller installera någon ytterligare programvara.

## New Relic Infrastructure

Pro-projekt innehåller [New Relic Infrastructure (NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/)-tjänsten, som automatiskt ansluter till programdata och prestandaanalyser för att tillhandahålla dynamisk serverövervakning. Den här tjänsten är tillgänglig i Pro Production- och Staging-miljöer.

## New Relic Log Management

Alla molninfrastrukturprojekt innehåller [New Relic logghantering](log-management.md). Tjänsten är förkonfigurerad för att samla alla loggdata från din miljö för förproduktion och produktion och visa dem i en centraliserad kontrollpanel för logghantering.
