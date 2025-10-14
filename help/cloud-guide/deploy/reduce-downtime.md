---
title: Driftsättning utan driftstopp
description: Lär dig hur du minskar den totala driftstoppen när du distribuerar Adobe Commerce i molninfrastrukturprojekt.
feature: Cloud, Deploy, SCD, Themes
exl-id: c216c5e9-d787-4428-b67a-b6aee814ded5
source-git-commit: b831bc5bce0f76ec8972b3578c500508dd4d7d41
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---

# Driftsättning utan driftstopp

Adobe Commerce i molninfrastrukturen kör programmet i [_underhållsläge_](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html?lang=sv-SE#production-mode) under distributionsfasen, vilket gör att webbplatsen kopplas från tills distributionen är klar. Hur lång tid produktionsplatsen är i underhållsläge beror på platsens storlek, antalet ändringar som gjorts under distributionen och konfigurationen för statisk innehållsdistribution. Det går att konfigurera ditt projekt så att det distribueras med en **noll**-nedtidseffekt.

Under distributionsprocessen bevarar alla anslutningsköer i upp till 5 minuter alla aktiva sessioner och väntande åtgärder, som att lägga till i kundvagnen eller i kassan. Efter distributionen släpps kön och anslutningarna fortsätter utan avbrott. Om du vill använda det här _anslutningsundantaget_ till din fördel och minska distributionen till _noll_ driftavbrott, måste du konfigurera projektet så att det använder den mest effektiva distributionsstrategin.

>[!NOTE]
>
>Använd [Smart Wizard](smart-wizards.md) för att kontrollera om ditt molnprojekt är optimalt konfigurerat för att minimera driftsättningsdriftavbrott. Smart Wizard kontrollerar din aktuella konfiguration och vägleder dig genom rekommenderade konfigurationsjusteringar för att aktivera bästa praxis för driftsättningar utan driftavbrott.

Följ de här stegen för att minska den tid det tar för butiken att distribuera en uppdatering till produktionen:

1. [Uppgradera till `ece-tools` package](../dev-tools/install-package.md) eller [uppdatera `ece-tools` version](../dev-tools/update-package.md)
Adobe Commerce i molninfrastrukturprojektet måste ha det senaste `ece-tools` -paketet så att du har verktygen som behövs för att konfigurera en optimal distribution. Om du har de senaste `ece-tools` fortsätter du till nästa steg.

   >[!NOTE]
   >
   >Även om det är en god vana att använda det senaste `ece-tools`-paketet fungerar distributionsmetoden utan driftstopp med `ece-tools` [&#x200B; version 2002.0.13](../release-notes/cloud-release-archive.md#v2002013) och senare.

1. [Konfigurera distribution av statiskt innehåll](static-content.md)
Om statisk innehållsdistribution misslyckas i distributionsfasen fastnar platsen i underhållsläge. När ett fel inträffar under byggfasen undviker processen driftavbrott eftersom den aldrig påbörjar distributionsfasen. [Att generera statiskt innehåll under byggfasen med minifierad HTML](static-content.md#setting-the-scd-on-build), även kallat det idealiska läget, är den optimala konfigurationen för driftsättningar utan driftavbrott och _förhindrar_ driftavbrott om ett fel inträffar.

1. [Konfigurera kroken efter distribution](../application/hooks-property.md)
Du måste konfigurera kroken efter distributionen för att rensa och värma cachen. Som standard rensas cacheminnet under distributionsfasen när platsen är nere. Att flytta cacheminnet till fasen efter distributionen innebär att cacheminnet förblir levande tills distributionsfasen är klar och sedan kan du rensa cacheminnet på ett säkert sätt.

   Anpassa listan med sidor som används för att förhandsladda cachen med miljövariabeln [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages).

1. [Minska temafiler](../environment/variables-deploy.md#scdmatrix)
Du kan minska antalet onödiga temafiler genom att konfigurera miljövariabeln SCD\_MATRIX.

1. [Snabba upp distributionen av statiskt innehåll](../environment/variables-deploy.md#scdthreads)
Du kan snabba upp distributionsprocessen genom att uppdatera miljövariabeln SCD\_THREADS för att öka antalet trådar för distribution av statiskt innehåll.

>[!NOTE]
>
>Du kan validera projektkonfigurationen för optimal distribution genom att [köra guiden för idealiskt läge](smart-wizards.md#verifying-an-ideal-configuration).
