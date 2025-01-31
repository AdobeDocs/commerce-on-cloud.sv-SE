---
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---
# Molnfragment

## Elasticsearch-varning {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch 7.11 och senare stöds inte för Adobe Commerce i molninfrastruktur. Adobe Commerce version 2.3.7-p3, 2.4.3-p2 och 2.4.4 och senare stöder OpenSearch-tjänsten. De lokala anläggningarna fortsätter att stödja Elasticsearch.

## Förbättrad integrering {#enhanced-integration-envs}

>[!NOTE]
>
>Projekt som etablerades före den 5 juni 2020 hade flera mindre integreringsmiljöer. Om du behöver en större integreringsmiljö för testning och utveckling kan du begära en uppgradering till förbättrade integreringsmiljöer. Mer information finns i artikeln [Integreringsmiljöbegäran](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html) i _Adobe Commerce Help Center_.

## Sammanfogningsalternativ {#merge-options}

Som standard skriver distributionsprocessen över alla inställningar i filen `env.php`, men du kan välja att sammanfoga ett eller flera värden för en tjänstkonfiguration utan att skriva över alla värden.

Ställ in alternativet `_merge` på något av följande:

- `true`—**Sammanfoga** de konfigurerade tjänstvärdena med miljövariabelvärdena.
- `false`—**Skriv över** de konfigurerade tjänstvärdena med miljövariabelvärdena.

## Privat databas {#private-repository}

>[!NOTE]
>
>Adobe rekommenderar starkt att du använder ett privat arkiv för Adobe Commerce i molninfrastrukturprojekt för att skydda all tillverkarspecifik information eller allt utvecklingsarbete, som tillägg och känsliga konfigurationer.

## Självbetjäningsvarning för proffs {#pro-self-service-warning}

>[!WARNING]
>
>Vissa **Pro-projekt** kräver en supportbiljett för att uppdatera vägkonfigurationen i filen `routes.yaml` och i cron-konfigurationen i filen `.magento.app.yaml`. Adobe rekommenderar att du uppdaterar och testar YAML-konfigurationsfiler i en integreringsmiljö och sedan distribuerar ändringar i mellanlagringsmiljön. Om dina ändringar inte tillämpas på mellanlagringswebbplatser efter att du har omdistribuerat och det inte finns några relaterade felmeddelanden i loggen, **MÅSTE** [Skicka en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) som beskriver de konfigurationsändringar du har gjort. Inkludera eventuella uppdaterade YAML-konfigurationsfiler i biljetten.

## Support för Pro services {#pro-update-service}

>[!TIP]
>
>För Pro-projekt måste du [skicka en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) om du bara vill installera eller uppdatera [tjänster](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/service/services-yaml.html) i `Staging`- och `Production`-miljöer.
>
>Ange vilka tjänständringar som krävs, inkludera dina uppdaterade `.magento.app.yaml`- och `services.yaml`-filer och ange PHP-versionen i biljetten. Om du vill göra ändringar i PHP-version, tillägg eller miljöinställningar för självbetjäning läser du [PHP-inställningar](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/php-settings.html) i _Programkonfiguration_.
>
>För ändringar i en aktiv produktionsmiljö (**endast Pro**) krävs minst 48 timmars varsel. Detta ger molninfrastruktursteamet tillräckligt med tid för att samla in resurser och genomföra en säker uppgradering. Anmälningsperioden börjar när infrastrukturteamet godkänner begäran och schemalägger uppgraderingen, exklusive helger. Om du t.ex. vill att uppgraderingarna ska vara klara på måndag måste du få en bekräftelse på den schemalagda uppgraderingen senast på onsdagen. Under perioder med hög efterfrågan kan det ta längre tid att behandla din begäran.

## Pro-säkerhetskopiering {#pro-backups}

>[!TIP]
>
>I Pro Staging- och Production-miljöer måste du [skicka en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) för att hämta en specifik säkerhetskopia med information om datum, tid och tidszon i biljetten.
>
>Adobe återställer **inte** miljöer från en automatisk säkerhetskopiering. Se [Återställ en DB-ögonblicksbild från mellanlagring eller produktion](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html) om du behöver hjälp med att välja en metod för att återställa en ögonblicksbild av mellanlagring eller produktion.

## Återdistribuera varning {#redeploy-warning}

>[!WARNING]
>
>Distributionsprocessen börjar när du utför en sammanfogning, push eller synkronisering av miljön, eller när du utlöser en manuell omdistribution, under vilken [!DNL Commerce]-programmet är i underhållsläge. För en produktionsmiljö rekommenderar Adobe att man slutför detta under tider med låg belastning för att undvika avbrott i tjänsten.

## Flödesplatshållare {#route-placeholder}

>[!NOTE]
>
>I följande exempel på flödeskonfiguration används flödesmallar med platshållare. Platshållaren `{default}` representerar den standarddomän som konfigurerats för platsen. Om projektet har flera domäner använder du platshållaren `{all}` för att konfigurera routning för standarddomänen och alla alias. Se [Konfigurera vägar](/help/cloud-guide/routes/routes-yaml.md).

## SCD-timing {#scd-timing-warning}

>[!WARNING]
>
>Om du har problem med statiska innehållsfiler i programmet efter distributionen, till exempel om det saknas anpassade temafiler, ökar du den förväntade körningstiden till 900 sekunder eller mer.

## Scenariobaserad driftsättning {#scenarios}

>[!NOTE]
>
>Med [!DNL ECE-Tools] 2002.1.0 och senare kan du använda den scenariobaserade distributionsfunktionen för att anpassa processerna för att skapa, distribuera och efterdistribuera din Adobe Commerce i molninfrastrukturprojekt. Se [Scenariobaserad distribution](/help/cloud-guide/deploy/scenario-based.md).

## Andra mellanlagring {#second-staging}

>[!NOTE]
>
>Vissa projekt kräver ett mer avancerat utvecklingsarbetsflöde. För att tillgodose detta behov erbjuder Adobe en [extra mellanlagringsmiljö](/help/cloud-guide/test/second-staging.md) som ett tilläggsalternativ till din molninfrastruktur.

## Tjänstinstruktion {#service-instruction}

Använd följande instruktioner för tjänstkonfiguration i Pro Integration-miljöer och Starter-miljöer, inklusive grenen `master`.

>[!NOTE]
>
>[Skicka en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) om du vill ändra tjänstkonfigurationen i Pro Production- och mellanlagringsmiljöer.

## Tjänständring {#service-change-tip}

>[!TIP]
>
>Efter den första konfigurationen av tjänsten kan du ändra programversionen för en installerad tjänst genom att uppdatera konfigurationsfilerna för `services.yaml` och `.magento.app.yaml`. Mer information om hur du uppgraderar eller nedgraderar en tjänst finns i [Ändra tjänstversion](/help/cloud-guide/services/services-yaml.md#change-service-version).

## Tips för distribution av Studio {#stuck-deployment-tip}

>[!TIP]
>
>Använd [Adobe Commerce felsökare för distribution](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html) i _Commerce Help Center_ om du behöver hjälp med fastlagda distributioner.

## Uppdatera till ECE-verktyg {#ece-tools-package}

>[!NOTE]
>
>Om du använder en version av Adobe Commerce i molninfrastrukturen som inte innehåller paketet `ece-tools` måste du utföra en [engångsuppgradering](/help/cloud-guide/dev-tools/install-package.md) till ditt molnprojekt för att ta bort föråldrade paket. Om du för närvarande använder paketet `ece-tools` och behöver uppdatera det, se [Uppdatera paketet ECE-Tools](/help/cloud-guide/dev-tools/update-package.md).

## Uppgradering {#upgrade-tip}

>[!TIP]
>
>Innan du påbörjar en uppgradering eller en korrigeringsprocess skapar du en aktiv gren i integreringsmiljön och checkar ut den nya grenen till din lokala arbetsstation. Genom att dedikera en gren till uppgraderingen eller korrigeringsprocessen undviker du störningar i pågående arbete.

<!-- Fastly-related snippets begin -->

## Administratörsinloggning {#admin-login-step}

1. [Logga in](/help/get-started/onboarding.md#access-your-admin-panel) i administratören.

## Automatisera anpassad VCL-fragmentdistribution {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>I stället för att överföra anpassade VCL-fragment manuellt kan du lägga till fragment i katalogen `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` i din miljö. Utdrag i den här katalogen överförs automatiskt när du klickar på _överför VCL till Fastly_ i Commerce Admin. Mer information om Magento 2 finns i [Automatiserad distribution av anpassade VCL-fragment](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) i modulen Fast CDN.

<!-- Fastly-related snippets end -->
