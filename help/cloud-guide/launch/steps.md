---
title: Starta steg
description: Lär dig hur du slutför starten av webbplatsen.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---

# Starta steg

När du har testat och slutfört din startchecklista kan du starta de sista stegen. De här stegen omfattar att gå in i biljetter, överföra åtkomst till webbplatsen och slutligen testa din butik när den är live.

Adobe supporttekniker arbetar med dig genom processen, kontrollerar status och hjälper dig om det uppstår några frågor eller problem. Alla problem bör spåras med biljetter för att på bästa sätt fånga upp vad som hände och hur det löstes. När du påbörjar kontinuerliga iterationer som distribuerar uppdateringar till din startade butik kan liknande problem uppstå igen. Dessa biljetter kan hjälpa till att identifiera problemen och hjälpa till att justera era driftsättningsuppgifter.

## Kontakta Adobe för att starta webbplatsen

Kontakta Adobe Commerce support och uppdatera alla biljetter för att starta en webbplats (live) med det datum och den tidpunkt då de var avsedda att byta och starta affären.

## Växla DNS till den nya platsen

Värdet för TTL (Time-to-Live) är viktigt för att kontrollera din ändrade domän. När du ändrar A- och CNAME-posterna tar uppdateringen den TTL-konfigurerade tiden att uppdatera korrekt. Mer information om DNS-inställningar finns i [DNS-konfigurationer](checklist.md#update-dns-configuration-with-production-settings).

### Så här byter du till den nya platsen:

1. Åtkomst till din DNS-tjänst.

1. Uppdatera dina A- och CNAME-poster för dina domäner och värdnamn.

1. Vänta tills TTL-tiden har gått och starta om webbläsaren.

1. Få åtkomst till din butik via domänen storefront.

## Testa livebutiken

Slutför några UAT-tester i din livebutik för att bekräfta att allt läses in och att åtgärderna slutförs korrekt. En lista med tester finns i [Testa distributionen](../test/staging-and-production.md#complete-uat-testing).

## Efter start

Adobe kontrollerar och övervakar webbplatsen för att säkerställa att alla tjänster och all åtkomst är grön. Vi finns till hands för att kunna gå igenom och kontrollera att alla systemloggar, tjänster, cachning och funktioner fungerar som du och dina kunder behöver.

Om det uppstår några problem kan du skapa och spåra problem med supporten. Inkludera så mycket information som möjligt, inklusive datum/tid, specifik funktion med ett problem, symtom och udda beteenden, tillägg med mera. Vi undersöker loggarna, problemet och samarbetar med dig för att lösa problemet så snabbt som möjligt.
