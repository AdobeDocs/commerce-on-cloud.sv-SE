---
title: Felmeddelanden för ECE-verktygspaket
description: Se en lista med felkoder och meddelanden som kan visas under Adobe Commerce när det gäller processer för att bygga, distribuera och efterdistribuera molninfrastrukturer.
recommendations: noDisplay
role: Developer
exl-id: e240c268-b171-44e9-9fa4-f0e91b0d9899
source-git-commit: 350cfea06f036f0787b330e6e40c5af46a30f5ad
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Felmeddelanden för ECE-verktyg

Referensen till det här felmeddelandet innehåller information om felsökning som kan inträffa under byggandet, distributionen och efterdistributionen av Adobe Commerce-infrastrukturen.

Alla viktiga felmeddelanden och varningsmeddelanden som inträffar under distributionen skrivs till både `var/log/cloud.log`- och `/var/log/cloud.error.log`-filerna. Loggfilen för molnfel innehåller endast fel från den senaste distributionen. En tom fil visar att distributionen lyckades utan fel.

I filen `cloud.error.log` formateras varje post som en JSON-sträng för enklare tolkning:

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

Felmeddelanden kategoriseras efter en av distributionsfaserna: skapa, distribuera och efterdistribuera. Varje avsnitt innehåller en lista med associerade fel med följande information för varje fel:

- **Felkod**: Den Adobe Commerce-tilldelade identifieraren för felmeddelandet
- **Scen**: Anger om felet uppstod under stadiet för att skapa, distribuera eller efterdistribuera
- **Steg**: Anger det steg i distributionsscenariot som kan returnera felet. Om kolumnen _Steg_ är tom är felet ett allmänt fel som kan returneras av flera steg, eller under förbearbetningsåtgärder. Mer information om stegen för att skapa, distribuera och efterdistribuera finns i [Scenariobaserad distribution](../deploy/scenario-based.md).
- **Förslag**: Tillhandahåller vägledning för felsökning och lösning
- **Titel (felbeskrivning)**: En beskrivning som sammanfattar orsaken till felet
- **Typ**: Anger om felet är ett kritiskt fel eller en varning

{{$include /help/_includes/automated/ece-tools-error-codes.md}}
