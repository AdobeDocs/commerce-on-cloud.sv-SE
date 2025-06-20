---
title: Starta webbplatsen
description: Lär dig hur du påbörjar förberedelser för att starta webbplatsen.
exl-id: 95abc7aa-ed4d-44f7-96aa-517c646bc00d
source-git-commit: 38ac38d4edd0f317155d0d4537021a29a21d5761
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 0%

---

# Starta webbplatsen

När du har slutfört distributionen och testningen i integrerings- och mellanlagringsmiljöer kan du börja förbereda för att starta webbplatsen. För det första bör du slutföra all utveckling och testning innan du arbetar i produktionsmiljön. Är du redo att starta? Granska checklistor, metodtips och slutsteg för att starta webbplatsen.

Om du har markerat den här informationen innan du distribuerar och testar i Förproduktion bör du först granska fördelarna med testning i Förproduktion i nästa avsnitt. Staging är en nära produktionsmiljö som körs på liknande maskinvara, konfigurationer, arkitektur och tjänster. Det kan minska driftstoppen och göra tillägg, service, anpassade konfigurationer och tester av användargodkännande för handlare till viktiga komponenter för att lansera webbplatser och butiker.

## Varför testa fullt ut i integrering, staging och produktion?

Vi rekommenderar starkt att du testar i integrerings-, mellanlagrings- och produktionsmiljöer eftersom det är så komplext att säkerställa att din egen kod, dina egna teman, tillägg och tredjepartsintegreringar fungerar tillsammans i butikerna. Följande är vanliga problem som du kan identifiera och lösa när du har slutfört testningen i integrerings- och mellanlagringsmiljöerna innan du uppdaterar produktionsmiljön:

- Mellanlagring stöder alla produktionstjänster, funktioner, databasdata, teknikstackar, arkitektur med mera. Det speglar produktion, vilket innebär att om fel uppstår i Förproduktion visas en varning innan de inträffar i Produktionen.

- Kod som fungerar i den lokala integreringsmiljön kanske inte fungerar i miljöer med staging och produktion.

- Integrationsmiljöer stöder inte vissa tjänster som är tillgängliga i Förproduktion, som Fastly och New Relic.

- [Testa](../test/guidance.md) din webbplats fullständigt med olika verktyg i Mellanlagring för inläsning, stress, prestanda och webbplatsresurser.

- Eftersom integreringsmiljöer bara har databaser ifyllda med testdata, och inte matchar en produktionsliknande miljö, kan du hitta ytterligare fel eller oväntade beteenden när du testar i staging- eller produktionsmiljöer.

## Krav för att starta webbplatsen

Du behöver följande information och resurser för att förbereda dig för att starta webbplatsen:

- CNAME-postinformation för konfiguration av DNS

- Lista över alla domäner i StoreFront som ska läggas till i certifikatet

- SSL-/TLS-certifikat

Som en del av Adobe Commerce prenumeration på molninfrastruktur tillhandahåller Adobe ett domänvaliderat SSL/TLS-certifikat som utfärdas av Låt oss kryptera. Varje Pro Production-, Staging- och Starter Production-miljö (`master`) har ett unikt certifikat som omfattar alla domäner och underdomäner i den miljön. Dessa certifikat etableras och överförs automatiskt till din plats när du har uppdaterat din DNS-konfiguration för utveckling och produktion. Se [Etablera SSL-/TLS-certifikat](../cdn/fastly-configuration.md#provision-ssltls-certificates).

>[!NOTE]
>
>Om du vill distribuera ditt eget SSL-certifikat för utökad validering för ditt företag i stället för att använda krypteringscertifikatet för Låt oss kontakta din CTA eller [Skicka en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

## Konfigurera verktyget för säkerhetsgenomsökning

>[!NOTE]
>
>Verktyget för säkerhetsgenomsökning använder följande offentliga IP-adresser:
>
>```text
>52.87.98.44
>34.196.167.176
>3.218.25.102
>```
>
>Lägg till de här IP-adresserna till en tillåtelselista i brandväggsreglerna för nätverket så att verktyget kan skanna din webbplats. Verktyget skickar endast begäranden till portarna 80 och 443.

Med verktyget för säkerhetsgenomsökning kan du regelbundet övervaka dina butikers webbplatser och få uppdateringar för kända säkerhetsrisker, skadlig programvara och inaktuell programvara. Det här verktyget är en kostnadsfri tjänst för alla implementeringar och versioner av Adobe Commerce i molninfrastrukturen. Du kommer åt verktyget via ditt [Commerce Marketplace-konto](https://account.magento.com/customer/account/login).

- Övervaka webbplatsens säkerhetsstatus och använda säkerhetsuppdateringar

- Ta emot säkerhetsuppdateringar och platsspecifika meddelanden

>[!NOTE]
>
>Adobe rekommenderar att du använder verktyget för säkerhetsgenomsökning framför andra verktyg från tredje part för att säkerställa bästa möjliga servicekvalitet under undersökningen av resultaten.

Mer information om hur du konfigurerar och använder verktyget för säkerhetssökning finns i [användarhandboken](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-scan). Vanligtvis börjar du använda det här verktyget när du börjar testa användargodkännande (UAT).

Varje plats som du skannar måste registreras via fliken Security Scan. Under registreringsprocessen måste du godkänna ansvarsfriskrivningen innan du kan börja skanna. Du styr både schemat och auktoriserar användaren att ta emot meddelanden när varje skanning är klar. Du kan schemalägga genomsökningar efter ett visst, återkommande datum och tid, eller köra en genomsökning på begäran efter behov.

I verktyget för säkerhetsgenomsökning används flera användaragentsträngar för att simulera aktivitet av skadlig kod i verkligheten. Du kan se följande användaragenter i dina analyser eller åtkomstloggar:

```text
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0
GuzzleHttp/6.3.3 curl/7.29.0 PHP/7.1.18
Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36
Visbot/2.0 (+http://www.visvo.com/en/webmasters.jsp;bot@visvo.com)
```

## Skanna din webbplats

1. Gå till ditt [Commerce Marketplace-konto](https://account.magento.com/customer/account/login).

1. Klicka på fliken Säkerhetsgenomsökning och välj **Gå till Säkerhetsgenomsökning**.

1. Välj **Kör skanning** i kolumnen _Åtgärder_ för platsen. En meddelandestatus visar den schemalagda sökningen.

### Så här granskar du rapporten:

1. När rapporten är klar visas ett meddelande.

1. Markera den rapport som du vill visa i kolumnen **Rapporter** på webbplatsraden. Ordern är den senaste till den äldsta.

I rapporten listas problem, inklusive misslyckade sökningar, oidentifierade resultat och lyckade sökningar. Varje tävlingsbidrag innehåller detaljerad information om sökningen, en lista över problem som ska undersökas och vilka åtgärder som ska vidtas. Vissa av dessa åtgärder kan kräva att du hämtar och installerar säkerhetsuppdateringar. Lägg till nödvändiga korrigeringsfiler i en utvecklingsgren på den lokala arbetsstationen innan du lägger till dem i produktionsgrenen.

Skanningsresultaten innehåller en etikett som beskriver status för genomsökning eller misslyckande med detaljerad information om utförda kontroller:

- &quot;Misslyckades&quot; innebär att webbplatsen innehåller en allvarlig säkerhetslucka.

- &quot;Oidentifierad&quot; tyder på att ditt team eller din värdleverantör behöver göra en djupare granskning för att avgöra om ytterligare åtgärder krävs.

Sökresultaten innehåller även förslag på åtgärder för varje misslyckat säkerhetstest. Resultatet av säkerhetsgenomsökningen skyddas och kan bara visas av den registrerade användaren. Endast användare som är utsedda i platsregistreringsprocessen får meddelanden om slutförd skanning.

## Klart att starta webbplatsen

Se följande när du är redo att starta processen:

- [Öppna checklista](checklist.md)

- [Starta steg](steps.md)
