---
title: Testvägledning
description: Läs om testningstyper och de bästa sätten att starta Adobe Commerce på molninfrastrukturen.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# Testvägledning

När du har konfigurerat och anpassat ditt Adobe Commerce i ett molninfrastrukturprojekt är det bäst att testa programmet noggrant innan du startar butikens webbplats. Testning hjälper till att hantera förväntningarna på klusterstorleken på rätt sätt och skalas för framtida affärsbehov.

## Funktionstestning

Under utvecklingen är det viktigt att du utför funktionstestning från början till slut på ditt Adobe Commerce i molninfrastrukturprojekt. Se följande riktlinjer för funktionstestning i Docker-miljön:

- **Programtestning** - Använd [Magento Functional Testing Framework (MFTF)](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/) för programtestning i Cloud Docker-miljö.

- **Kodtestning** - Använd [ramverket för kodekationstestning för PHP](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/) för att validera kod som är avsedd att bidra till molnpaketets databaser.

## Bästa tillvägagångssätt före lansering

Tänk på följande testtyper som en bra metod att utföra innan webbplatsen startas:

- **Läs in test** - Utför ett inläsningstest för att förstå systemets beteende under en förväntad inläsning. Testa till exempel ett antal aktiva användare samtidigt i programmet, där varje användare utför ett visst antal transaktioner inom den angivna varaktigheten. Det här testet visar svarstiden för viktiga affärskritiska transaktioner, som databasen eller programserverfunktionen. Ett belastningstest kan hjälpa till att identifiera flaskhalsar.

- **Stresstest** - Utmana de övre gränserna för kapaciteten i systemet för att avgöra om systemet fungerar tillräckligt när den aktuella belastningen ligger långt över det förväntade maxvärdet.

- **Säkerhetsgenomsökning** - Adobe tillhandahåller ett kostnadsfritt [säkerhetsgenomsökningsverktyg](../launch/overview.md#set-up-the-security-scan-tool) för dina webbplatser.

- **Penetrationstest** - Är en auktoriserad simulerad cyberattack på ett datorsystem som utformats för att utvärdera systemets säkerhet. Inpenetrationstestet hjälper till att identifiera svagheter eller sårbarheter, inklusive potentialen för obehöriga parter att få tillgång till systemfunktioner och data.

>[!WARNING]
>
>Kunderna får inte själva utföra säkerhetsbedömningar av AWS infrastruktur eller AWS tjänster. Om du upptäcker ett säkerhetsproblem i någon av de AWS-tjänster som har observerats i din säkerhetsbedömning [kontaktar du AWS Security](mailto:aws-security@amazon.com) omedelbart. Se [AWS kundprofiler för penetrationstestning](https://aws.amazon.com/security/penetration-testing/).
