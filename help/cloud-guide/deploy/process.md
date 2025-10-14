---
title: Distributionsprocess
description: Lär dig hur driftsättning fungerar för Adobe Commerce i molninfrastrukturprojekt.
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Distributionsprocess

Distributionsprocessen börjar när du sammanfogar, kör eller synkroniserar din miljö, eller när du utlöser en [manuell omdistribution](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). Distributionsprocessen tar tid, men det finns sätt att optimera distributionen som beror på om du utvecklar och testar eller arbetar med en aktiv webbplats. Det viktigaste är att du kan styra den [statiska innehållsdistributionen](static-content.md).

Distributionsprocessen består av tre olika faser: bygg, driftsätt och postdriftsättning. Varje fas utför specifika åtgärder med begränsade resurser:

## ![Byggfas](../../assets/status-build.png) - byggfas

Fasen _build_ sätter ihop behållare för de tjänster som definierats i konfigurationsfilerna, installerar beroenden baserat på filen `composer.lock` och kör de build-kopplingar som definierats i filen `.magento.app.yaml`. Utan möjlighet att ansluta till tjänster eller komma åt databasen beror byggfasen på de resurser som är begränsade till miljön.

## ![Distributionsfas](../../assets/status-deploy.png) Distributionsfas

Fasen _deploy_ placerar en temporär spärr på inkommande begäranden och överför platsen till [underhållsläge](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html?lang=sv-SE). Distributionsfasen använder de nya behållarna och när filsystemet har monterats öppnar nätverksanslutningar, aktiverar de tjänster som definierats i `relationships`-avsnittet i `.magento.app.yaml`-filen och kör de distributionskopplingar som definierats i `.magento.app.yaml`-filen. Allt är _skrivskyddat_, utom för kataloger som definieras i filen `.magento.app.yaml`. Som standard innehåller egenskapen [`mounts` &#x200B;](../application/properties.md#mounts) följande kataloger:

- `app/etc` - innehåller konfigurationsfilerna `env.php` och `config.php`
- `pub/media` - innehåller alla mediedata, till exempel produkter eller kategorier
- `pub/static` - innehåller genererade statiska filer
- `var` - innehåller temporära filer som skapats under körning

Alla andra kataloger har skrivskyddad behörighet. Den nya platsen blir aktiv i slutet av distributionsfasen när den övergår från underhållsläge och frigör det tillfälliga undantaget för inkommande begäranden.

Under distributionsfasen sparas kopior av `app/etc/config.php`- och `app/etc/env.php`-distributionskonfigurationsfilerna med BAK-tillägget. Mer information om hur du återställer de här filerna finns i [Lagringsinställningar](../store/store-settings.md#restore-configuration-files).

## ![Fas efter distribution](../../assets/status-post-deploy.png) Fas efter distribution

Fasen _post-deploy_ kör de efterdistribuerade kopplingar som definierats i filen `.magento.app.yaml`. Om du utför en åtgärd i den här fasen kan det påverka webbplatsens prestanda, men du kan använda miljövariabeln [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) för att fylla i cachen.

## ![Verifiera läge](../../assets/status-verify.png) Verifiera konfigurationer

Du kan testa den optimala konfigurationen för projektets status genom att köra [Smarta guider](smart-wizards.md).

>[!NOTE]
>
>Med `ece-tools` 2002.1.0 och senare kan du använda den scenariobaserade distributionsfunktionen för att anpassa processerna för att skapa, distribuera och efterdistribuera din Adobe Commerce i molninfrastrukturprojekt. Se [Scenariobaserad distribution](scenario-based.md).
