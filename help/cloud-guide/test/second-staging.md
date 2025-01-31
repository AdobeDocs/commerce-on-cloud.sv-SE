---
title: Andra mellanlagringsmiljön
description: Lär dig fördelarna och användningsområdena med en andra testmiljö för parallell testning, problemisolering, versionskontroll med mera.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# Andra mellanlagringsmiljön

I Adobe Commerce Cloud infrastruktur säkerställer staging-miljön omfattande testning och validering i en miljö som kan spegla produktionsmiljön. I den här miljön kan du identifiera och åtgärda fel på ett säkert sätt, samtidigt som du ser till att alla nya funktioner och ändringar är sömlöst integrerade innan de påverkar din aktiva webbplats.

Vissa projekt kräver ett mer avancerat utvecklingsarbetsflöde. För att tillgodose detta behov erbjuder Adobe en extra staging-miljö som ett tilläggsalternativ till din molninfrastruktur. Det är fördelaktigt att ha två testmiljöer för komplexa projekt och att ha ett parallellt samarbete mellan team.

En sekundär staging-miljö ger följande fördelar:

- **Parallell testning**: Med två mellanlagringsmiljöer kan team testa nya funktioner och viktiga uppdateringar samtidigt utan att påverka varandras utveckling. Du kan t.ex. dedikera en miljö för funktionstestning medan du reserverar den andra miljön för prestanda- och stresstestning.

- **Isolering av problem**: När fel eller problem uppstår i den primära mellanlagringsmiljön kan du replikera och diagnostisera dem i den sekundära miljön utan att stoppa testförloppet. Detta säkerställer att felsökning inte stör pågående tester.

- **Versionskontroll**: Hantera olika versioner av programmet effektivare. Den primära testmiljön kan köra den senaste stabila versionen, medan de sekundära testfunktionerna experimenterar. Detta hjälper er att hantera releasecykler bättre och säkerställer att experimentella förändringar inte stör stabila byggen.

- **Förbättrad utrullningsplanering**: Du kan simulera olika distributionsscenarier. Den primära mellanlagringsmiljön kan härma den aktuella produktionsinställningen, medan den sekundära miljön kan konfigureras för att testa kommande versioner.

- **Testning av användargodkännande (UAT)**: När du använder den primära miljön för pågående utveckling kan du dedikera den sekundära miljön till UAT. Detta gör att användare och kunder kan testa och ge feedback om nya funktioner i en kontrollerad inställning, utan att det påverkar testningen.

- **Regressionstestning**: Du kan använda en miljö för framåtriktade tester och en annan för regressionstestning, så att nya ändringar inte bryter befintliga funktioner.

- **Utbildning och demonstration**: Använd en miljö för att utbilda nya teammedlemmar eller för att visa nya funktioner för intressenter. Detta säkerställer att utbildningsaktiviteterna inte avbryter testningen.

- **Inställningar för privat länk**: Mallar- och integreringsmiljöer har inte stöd för konfiguration av privat länk. Om ditt utvecklingsarbetsflöde kräver ytterligare miljöer som är anslutna via Private Link för utveckling, testning eller något annat ändamål bör du överväga att lägga till en extra mellanlagringsmiljö.

Två mellanlagringsmiljöer kan avsevärt förbättra arbetsflödets effektivitet, riskhantering och den övergripande kvaliteten i programmet. Dessa miljöer ger säkerhet och flexibilitet som en staging-miljö inte kan erbjuda.

>[!NOTE]
>
>Den här installationen är ett tillägg. Kunder som vill ha en sekundär miljö måste kontakta sin säljare för att begära en. Försäljaren kan ge information om priser och provisioneringsprocessen.

Om ditt projekt redan har en extra staging-miljö, eller om du funderar på att uppgradera ditt aktuella projekt, bör du tänka på följande tidslinjer och ansvarsområden för etableringen:

- Etableringsprocessen kan ta upp till två veckor efter att du har bekräftat din förfrågan hos din Adobe-säljare.

- Nya staging-miljöer är bara tillgängliga i samma region som dina nuvarande staging- och Production-miljöer.

- Du måste uppdatera antingen integrerings- eller huvudmiljön och ange vilka miljöer din sekundära staging-miljö ska baseras på. Den sekundära mellanlagringsmiljön kan inte kopieras från förproduktionsmiljön eller produktionsmiljön.

- När du har fått den nya testmiljön kan du inkludera den i utvecklingsflödet. Precis som med andra miljöer ansvarar du för koduppdateringar och databasuppdateringar.

## Begär miljön

Gör så här för att underlätta din begäran:

1. Uppgradera din gren.

   Uppgradera din Integration- eller Master-gren med koden och databasen som du vill använda för den nya miljön.

1. Kontakta din säljare.

   Kontakta din Adobe-säljare och förse dem med den specifika gren som du vill använda som bas för din nya staging-miljö (integrering eller master).

1. Bekräfta information.

   När du har bekräftat informationen med din säljare väntar du tills de har bekräftat att begäran om etablering har tagits emot och bearbetas. När provisioneringsprocessen är klar skickar Adobe-teamet en bekräftelse till dig.

1. Driftsätt i din nya miljö.

   Följ [standarddistributionsstegen](../deploy/staging-production.md) för att distribuera koden och databasen till den nya mellanlagringsmiljön.
