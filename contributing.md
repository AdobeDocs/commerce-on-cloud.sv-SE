---
source-git-commit: 305df8acb8c41369b34e2c20d9c7df6824222931
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 1%

---
# Bidrar

Tack för att du väljer att bidra!

Nedan följer ett antal riktlinjer som du kan följa när du bidrar till projektet.

## Uppförandekod

Detta projekt följer Adobes [uppförandekod](code-of-conduct.md). Genom att delta
du förväntas behålla den här koden. Rapportera oacceptabla beteenden till
[Grp-opensourceoffice@adobe.com](mailto:Grp-opensourceoffice@adobe.com).

## Handbok för Contributor

Se [Contributor-handboken](https://experienceleague.adobe.com/docs/contributor/contributor-guide/introduction.html?lang=sv-SE).

## Har du en fråga?

Börja med att lämna in ett ärende. Befintliga committers i det här projektet arbetar för att nå
konsensus om projektriktning och skicka ut lösningar inom problemtrådar
(i tillämpliga fall).

## Licensavtal för deltagare

Alla bidrag från tredje part till detta projekt måste åtföljas av en undertecknad bidragsgivare
licensavtal. Detta ger Adobe tillstånd att återdistribuera dina bidrag
som en del av projektet. [Underteckna vårt CLA](https://opensource.adobe.com/cla.html). Du
behöver bara skicka in ett Adobe CLA en gång, så om du har skickat in ett tidigare,
Du är redo att gå!

## Kodgranskningar

Alla inlagor ska lämnas in i form av en begäran om utlysning och behöver granskas
efter projektcommitters. Läs dokumentationen för [GitHub-begäran](https://docs.github.com/articles/about-pull-requests/)
om du vill ha mer information om hur du skickar pull-begäranden.

Till sist följer du mallen för [pull-begäran](PULL_REQUEST_TEMPLATE.md) när
skicka en pull-förfrågan!

## Från medverkande till committer

Vi älskar bidrag från vår community! Om du vill gå ett steg längre än medverkande
och bli en committer med fullständig skrivåtkomst och en medarbetare i projektet, måste du
bli inbjuden till projektet. Befintliga committers använder en intern nominering
processer som måste uppnå lat samförstånd (tystnad innebär godkännande) före inbjudningar
utfärdas. Om du känner att du är kvalificerad och vill bli mer involverad,
Jag kan kontakta befintliga committers för att diskutera det.

## Säkerhetsproblem

Säkerhetsproblem ska inte rapporteras i den här felspåraren. [Rapportera i stället ett problem till våra säkerhetsexperter](https://helpx.adobe.com/se/security/alertus.html)

## Nyheter

Om dina ändringar innehåller nya ämnen, viktiga uppdateringar eller korrigeringar som behöver markeras, kan du lägga till en kort beskrivning i avsnittet [Nyheter](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/overview#whats-new) direkt från din pull-begäran.

Så här lägger du till en markering med nyheter:

1. Inkludera taggen `whatsnew` med lämplig beskrivning i texten för pull-begäran i slutet. Beskrivningen ska innehålla kontext om ändringen och en länk till målämnet eller -avsnitten. Använd följande format (kodblockcitattecken är endast för representation, ta inte med dem i texten för pull-begäran):

   ```text
   whatsnew
   Short description of the change in the [target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/target-topic.html).
   ```

   eller, om det finns flera ämnen:

   ```text
   whatsnew
   Short description of the changes in the [first target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/target-topic.html), [second target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-target-topic.html), and [third target topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/third-target-topic.html).
   ```

   Du kan också använda listor för flera markeringar:

   ```text
   whatsnew
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-topic.html).
   ```

   ```text
   whatsnew
   The following changes were made to the documentation:
   - Short description of the first change in the [first topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/first-topic.html).
   - Short description of the second change in the [second topic](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/second-topic.html).
   ```

1. Lägg till etiketter som anger ändringstypen. Etiketter som stöds är etiketter för varje typ av ändring, som:

   - `new-topic` - för nya ämnen
   - `major-update` - för större uppdateringar som kan innehålla betydande ändringar av innehåll, struktur eller funktionalitet
   - `technical` - för tekniska ändringar som inte betraktas som större uppdateringar men som fortfarande kräver uppmärksamhet

**Viktigt:**

1. `whatsnew`-delen måste börja från `whatsnew`-taggen och vara i slutet av pull-begärandetexten.
1. Beskrivningarna av ändringarna måste innehålla fungerande länkar. Kontrollera att länkarna är korrekta och leder till rätt avsnitt. Om ämnet är nytt kontrollerar du att länkarna fungerar efter att du har sammanfogat pull-begäran och publicerat det nya ämnet. Det går bra att åtgärda länkarna när pull-begäran har sammanfogats.

Du kan till exempel söka i stängda pull-begäranden i databasen för att se hur befintliga markeringar formateras och jämföra dem med avsnittet [Nyheter](https://experienceleague.adobe.com/sv/docs/commerce-cloud-service/user-guide/overview#whats-new) för att se hur de visas i dokumentationen.
