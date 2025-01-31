---
title: Bästa praxis för driftsättning
description: Upptäck de bästa sätten att driftsätta Adobe Commerce i molninfrastruktur.
feature: Cloud, Deploy, Best Practices
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# Bästa praxis för driftsättning

Bygg och distribuera skript som aktiveras när du sammanfogar kod till en fjärrmiljö. Dessa skript använder [konfigurationsfiler](../environment/overview.md) för miljön och programkod för att tillhandahålla molninfrastruktur med lämpliga data och tjänster. Dessa skript används också för att installera eller uppdatera Adobe Commerce-programmet, tredjepartstjänster och anpassade tillägg i molnmiljön.

Processen för att skapa och distribuera skiljer sig något åt för varje plan:

- **Starter-planer** - För integreringsmiljön byggs och distribueras alla aktiva grenar till en fullständig miljö för åtkomst och testning. Testa koden fullständigt efter sammanslagning med grenen `staging`. Om du vill starta webbplatsen trycker du på `staging` till `master` för att distribuera till produktionsmiljön. Du har fullständig åtkomst till alla grenar via kommandona [!DNL Cloud Console] och CLI.

- **Pro-planer** - För integreringsmiljön byggs och distribueras alla aktiva grenar till en fullständig miljö för åtkomst och testning. Koppla koden till grenen `integration` innan du sammanfogar den med miljöerna Förproduktion och Förproduktion. Du kan sammanfoga till miljö för förproduktion och produktion med hjälp av kommandona [!DNL Cloud Console] eller SSH och `magento-cloud` CLI.

## Spåra processen

Du kan spåra bygg- och distributionsåtgärder i realtid med terminalen eller statusmeddelandena [!DNL Cloud Console] - `in-progress`, `pending`, `success` eller `failed` - som visas under distributionsprocessen. Du kan visa information i loggfilerna. Se [Visa loggar](../test/log-locations.md).

Om du använder externa GitHub-databaser visas inte åtgärdsloggen i GitHub-sessionen. Du kan dock fortfarande följa aktiviteten i gränssnittet för den externa databasen och [!DNL Cloud Console]. Se [Integrationer](../integrations/overview.md).

>[!NOTE]
>
>I integreringsmiljöer kan du inte visa distributionsloggarna från [!DNL Cloud Console]. Den här funktionen är endast tillgänglig för produktions- och mellanlagringsmiljöer. Du kan dock visa loggar för varje distributionsfas i alla miljöer med hjälp av loggarna [build och deploy](../test/log-locations.md#build-and-deploy-logs) . Felsökningsinformation finns i [Distributionsfelsreferens](../dev-tools/error-reference.md).

Du kan aktivera [Spåra distributioner med New Relic](../monitor/track-deployments.md) för att övervaka distributionshändelser och analysera prestanda mellan distributioner.

## Bästa tillvägagångssätt för byggen och driftsättningen

Granska de bästa metoderna och övervägandena för din distributionsprocess:

- **Kontrollera att du kör den senaste versionen av `ece-tools` package**

  Se [Versionsinformation för ECE-verktyg](../release-notes/ece-tools-package.md).

- **Följ processen för att skapa och distribuera**

  Se till att du har rätt kod i varje miljö för att undvika att skriva över konfigurationer när du sammanfogar kod mellan miljöer. Om du till exempel vill använda konfigurationsändringar i alla miljöer ändrar och testar du ändringarna i den lokala miljön innan du distribuerar till fjärrintegreringsmiljön. Distribuera och testa sedan ändringarna i mellanlagringsmiljön innan du distribuerar till Production. När du sammanfogar från en miljö till en annan skrivs all kod i miljön över, förutom miljöspecifik konfiguration och inställningar.

- **Använd samma variabler i olika miljöer**

  Värdena för dessa variabler kan variera mellan olika miljöer, men du behöver vanligtvis samma variabler i varje miljö. Se [Konfigurationshantering för lagringsinställningar](../store/store-settings.md).

- **Behåll känsliga konfigurationsvärden och data i miljöspecifika variabler**

  Dessa värden innehåller variabler som anges med Cloud CLI, [!DNL Cloud Console] eller som läggs till i filen `env.php`. Se [Variabelnivåer](../environment/variable-levels.md).

- **Kontrollera att all kod är tillgänglig i miljögrenen**

  Att referera till kod från andra grenar, till exempel en privat gren, kan orsaka problem under bygg- och distributionsprocessen. Om du till exempel refererar till ett tema från en privat gren är temat inte tillgängligt och kan inte byggas med programkoden.

- **Lägg till tillägg, integreringar och kod i itererade grenar**

  Gör och testa ändringar lokalt, skicka till `integration`, sedan till `staging` och `production`. Testa och åtgärda problem i varje miljö innan du sammanfogar uppdateringarna med nästa miljö. Vissa tillägg och integreringar måste aktiveras och konfigureras i en viss ordning på grund av beroenden. Om du lägger till och testar i grupper kan det göra bygg- och driftsättningsprocessen mycket enklare och hjälpa till att avgöra var problemen uppstår.

- **Verifiera tjänstversioner och relationer och möjligheten att ansluta**

  Kontrollera vilka tjänster som är tillgängliga för ditt program och se till att du använder den senaste kompatibla versionen. Se [Servicerelationer](../services/services-yaml.md#service-relationships) och [Systemkrav](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) i _installationshandboken_ för rekommenderade versioner.

- **Testa lokalt och i integreringsmiljön innan du distribuerar till Förproduktion och Produktion**

  Identifiera och åtgärda problem i den lokala miljön och integreringsmiljön för att förhindra längre driftstopp när du distribuerar till förproduktionsmiljöer.

  >[!TIP]
  >
  >Det finns [smarta guidekommandon](../deploy/smart-wizards.md) som du kan använda för att verifiera att din molnprojektskonfiguration följer bästa praxis för bygg- och distributionskonfiguration, inklusive strategi för statisk innehållsdistribution (SCD).

- **När du har slutfört testningen i lokala miljöer och integreringsmiljöer distribuerar och testar du i mellanlagringsmiljön**

  Se [Testning av mellanlagring och produktion](../test/staging-and-production.md).

- **Kontrollera konfigurationen för produktionsmiljön**

  Utför följande uppgifter innan du distribuerar till Production:

   - Kontrollera att du kan ansluta till alla tre noderna i produktionsmiljön med [SSH](../development/secure-connections.md).

   - Kontrollera att indexerare är inställda på _Uppdatera i schema_. Se [Indexeringslägen](https://developer.adobe.com/commerce/php/development/components/indexing/) i _Utvecklarhandbok för tillägg_.

   - Förbered miljön genom att uppdatera alla miljöspecifika variabler i produktionskoden, verifiera tjänstens tillgänglighet och kompatibilitet samt göra andra nödvändiga konfigurationsändringar.

- **Övervaka distributionsprocessen**

  Granska distributionsstatusmeddelandena och åtgärda problemen efter behov. Granska [loggarna](../test/log-locations.md#) i molnet för detaljerade loggmeddelanden.

## Fem faser av integration, skapande och driftsättning

Följande faser utförs i den lokala utvecklingsmiljön och i integreringsmiljön. För Pro-planer distribueras koden inte till förproduktionsmiljöer eller produktionsmiljöer i dessa initiala faser.

### Fas 1: Validering av kod och konfiguration

När du först konfigurerade ett projekt utgör [molninfrastrukturmallen](https://github.com/magento/magento-cloud) en grund för kodfilerna. Den här kodrapporten klonas till ditt projekt som grenen `master`.

- **För Starter**—`master`-grenen är din produktionsmiljö.
- **För Pro**—`master` börjar som ursprunglig gren för integreringsmiljön.

Skapa en gren från `master` för din anpassade kod, tillägg och moduler samt tredjepartsintegreringar. Det finns en fjärrintegrerad miljö för att testa koden i molnet.

När du skickar koden från den lokala arbetsytan till fjärrdatabasen slutförs en serie kontroller och kodvalideringar innan skripten byggs och distribueras. Den inbyggda Git-servern validerar det du gör och gör ändringar. Om du till exempel lägger till en OpenSearch-tjänst verifierar den inbyggda Git-servern att topologin för ditt kluster ändras i enlighet med detta.

Om du har ett syntaxfel i en konfigurationsfil avvisar Git-servern push-åtgärden. Se [Skyddsblock](../development/protective-block.md).

Den här fasen kör även `composer install` för att hämta beroenden.

### Fas 2: Bygge

>[!NOTE]
>
>Under byggfasen är platsen inte i underhållsläge och om fel eller problem uppstår inte tas bort. Den skapar bara den kod som har ändrats sedan föregående version.

Den här fasen skapar kodbasen och kör kopplingar i avsnittet `build` i `.magento.app.yaml`. Standardbyggkroken är kommandot `php ./vendor/bin/ece-tools` och utför följande:

- Tillämpar korrigeringar i `vendor/magento/ece-patches` och valfria, projektspecifika korrigeringar i `m2-hotfixes`
- Återskapar kod och konfigurationen för [beroendeinjektionen](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary) (d.v.s. katalogen `generated/` som innehåller `generated/code` och `generated/metapackage`) med `bin/magento setup:di:compile`.
- Kontrollerar om filen [`app/etc/config.php`](../store/store-settings.md) finns i kodbasen. Adobe Commerce genererar automatiskt den här filen om den inte upptäcks under byggfasen och innehåller en lista med moduler och tillägg. Om den finns fortsätter byggfasen som vanligt, statiska filer komprimeras med GZIP och distribueras, vilket minskar driftstoppen i distributionsfasen. Mer information om hur du anpassar eller inaktiverar filkomprimering finns i [byggalternativ](../environment/variables-build.md).

>[!WARNING]
>
>För närvarande har klustret inte skapats, så försök inte ansluta till en databas eller anta att det finns en aktiv daemon-process.

När programmet har byggts monteras det i ett **skrivskyddat filsystem**. Du kan konfigurera specifika monteringspunkter som ska läsas/skrivas. Du kan inte använda FTP på servern och lägga till moduler. I stället måste du lägga till kod i din lokala databas och köra `git push`, som skapar och distribuerar miljön. Information om projektstrukturen finns i [Lokal projektkatalogstruktur](../project/file-structure.md).

### Fas 3: Förbered instruktionsmarginalen

Resultatet av byggfasen är ett skrivskyddat filsystem som kallas _instruktionsmarginal_. Den här fasen skapar ett arkiv och placerar instruktionsmarginalen i permanent förvaring. Nästa gång du trycker på kod används instruktionsmarginalen från arkivet om tjänsten inte ändrades.

- Gör att integrationen går fortare genom att återanvända oförändrad kod
- Om koden ändras uppdateras instruktionsmarginalen för nästa version så att den återanvänds
- Möjliggör omedelbar återställning av en distribution vid behov
- Inkluderar statiska filer om filen `app/etc/config.php` finns i kodbasen

Instruktionsmarginalen innehåller alla filer och mappar **förutom följande** monteringar som konfigurerats i `magento.app.yaml`:

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### Fas 4: Distribuera instruktionsmarginaler och kluster

Dina program och alla [backend](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary)-tjänster tillhandahålls enligt följande:

- Monterar varje tjänst i en behållare, till exempel webbserver, OpenSearch, [!DNL RabbitMQ]
- Monterar filsystemet för läsbehörighet (monterat på ett distribuerat lagringsrutnät med hög tillgänglighet)
- Konfigurerar nätverket så att tjänsterna kan &quot;se&quot; varandra (och bara varandra)

>[!NOTE]
>
>Gör ändringarna i en Git-gren när alla bygg- och driftsättningar är klara och kör igen. Alla miljöfilsystem är _skrivskyddade_. Ett skrivskyddat system garanterar deterministiska driftsättningar och förbättrar säkerheten på platsen dramatiskt eftersom ingen process kan skriva till filsystemet. Det fungerar också för att säkerställa att koden är identisk i miljö som omfattar integrering, mellanlagring och produktion.

### Fas 5: Distributionsnätverk

>[!NOTE]
>
>I den här fasen försätts programmet [!DNL Commerce] i underhållsläge tills distributionen är slutförd.

I det sista steget körs ett distributionsskript som du kan använda för att anonymisera data i utvecklingsmiljöer, rensa cacheminnen och fråga efter externa verktyg för kontinuerlig integrering. När det här skriptet körs har du tillgång till alla tjänster i din miljö, till exempel Redis.

Om filen `app/etc/config.php` inte finns i kodbasen komprimeras statiska filer med `gzip` och distribueras under den här fasen, vilket ökar längden på distributionsfasen och platsunderhållet.

>[!NOTE]
>
>Mer information om hur du anpassar eller inaktiverar filkomprimering finns i [distribuera variabler](../environment/variables-deploy.md).

Det finns två driftsättningskrokar. `pre-deploy.php`-kroken slutför nödvändig rensning och hämtning av resurser och kod som genererats i byggkroken. Haken `php ./vendor/bin/ece-tools deploy` kör en serie kommandon och skript:

- Om Adobe Commerce **inte är installerat** installeras det med `bin/magento setup:install`, distributionskonfigurationen `app/etc/env.php` och databasen för den angivna miljön, till exempel Redis och URL-adresser för webbplatser, uppdateras. **Viktigt!** När du slutförde [Första distributionen](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/launch/overview.html) under installationen installerades Adobe Commerce och distribuerades i alla miljöer.

- Om Adobe Commerce **är installerat** utför du alla nödvändiga uppgraderingar. Distributionsskriptet kör `bin/magento setup:upgrade` för att uppdatera databasschemat och data (vilket är nödvändigt efter tilläggs- eller kärnkodsuppdateringar) och även uppdaterar distributionskonfigurationen, `app/etc/env.php`, och databasen för din miljö. Distributionsskriptet rensar slutligen cacheminnet för Adobe Commerce.

- Skriptet genererar eventuellt statiskt webbinnehåll med kommandot `magento setup:static-content:deploy`.

- Använder omfattningar (`-s`-flagga i byggskript) med standardinställningen `quick` för statisk innehållsdistributionsstrategi. Du kan anpassa strategin med miljövariabeln [`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy). Mer information om dessa alternativ och funktioner finns i [Distributionsstrategier för statiska filer](../deploy/static-content.md) och flaggan `-s` för [Distribuera statiska visningsfiler](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

>[!NOTE]
>
>Distributionsskriptet använder de värden som definieras av konfigurationsfiler i katalogen `.magento`, och skriptet tar sedan bort katalogen och dess innehåll. Din lokala utvecklingsmiljö påverkas inte.

### Efterdistribution: konfigurera routning

När distributionen körs stoppas inkommande trafik vid startpunkten i 60 sekunder och routningen konfigureras om så att webbtrafiken kommer till det nyligen skapade klustret.

Distributionen har slutförts. Underhållsläget tas bort så att normal åtkomst tillåts och säkerhetskopierade (BAK) filer skapas för `app/etc/env.php` och `app/etc/config.php`-konfigurationsfilerna.

Aktivera generering av statiskt innehåll med variabeln `SCD_ON_DEMAND` och konfigurera [`post_deploy` kroken](../application/hooks-property.md) så att den rensar cachen och läser in cacheminnet _efter_ att behållaren börjar acceptera anslutningar och _under_ normal, inkommande trafik.

Mer information om hur du granskar bygg- och distributionsloggar finns i [Visa loggar](../test/log-locations.md#view-and-manage-logs).
