---
title: Skyddsblock
description: Läs mer om Adobe Commerce skyddsblockfunktion för molninfrastruktur och hur den skyddar din webbplats mot kända säkerhetsproblem.
feature: Cloud, Configuration, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---

# Skyddsblock

Adobe Commerce i molninfrastruktur har en skyddsblockerande funktion som under vissa omständigheter begränsar åtkomsten till webbplatser med säkerhetsbrister. Denna partiella blockeringsmetod förhindrar utnyttjande av kända säkerhetsbrister. Inaktuell programvara innehåller ofta explosioner, så det är viktigt att skydda sig mot dessa explosioner genom att delvis blockera åtkomsten till dessa webbplatser.

## Hur skyddsblocket fungerar

Adobe Commerce har en databas med signaturer av kända säkerhetsbrister i program med öppen källkod som ofta används i molninfrastruktur. Säkerhetskontrollen analyserar endast kända säkerhetsluckor i öppen källkodsprojekt. Den kan inte undersöka anpassningar som du skriver.

Säkerhetsgenomsökning körs:

- När du skickar ny kod till Git
- När nya säkerhetsluckor läggs till i databasen

Om en allvarlig säkerhetslucka upptäcks i programmet avvisas Git-push.

Det finns två typer av block som körs:

1. **Hela blocket** - för utvecklingswebbplatser. Felmeddelandet som medföljer `git push` innehåller detaljerad information om sårbarheten.

1. **Delblock** - för produktionswebbplatser, vilket gör att webbplatsen i huvudsak kan vara online. Beroende på säkerhetsluckans natur kan delar av en begäran, t.ex. en frågesträng, cookies eller andra rubriker, tas bort från GET-begäranden. Alla andra förfrågningar kan blockeras helt, t.ex. inloggning, formuläröverföring eller produktutcheckning.

Avblockering sker automatiskt när säkerhetsrisken har åtgärdats. Blocket tas bort kort efter att du har tillämpat en säkerhetsuppgradering som tar bort säkerhetsluckan.

## Avanmäl dig från skyddsblocket

Skyddsdelen finns till för att skydda dig mot kända säkerhetsluckor i den programvara du distribuerar Adobe Commerce i molninfrastruktur.

Du kan dock avanmäla dig från skyddsblocket genom att lägga till följande i [`.magento.app.yaml`](../application/configure-app-yaml.md):

```yaml
   preflight:
      enabled: false
```

Du kan avanmäla dig från en viss kontroll, till exempel:

```yaml
   preflight:
      enabled: true
      ignore_rules: [ "drupal:SA-CORE-2014-005" ]
```
