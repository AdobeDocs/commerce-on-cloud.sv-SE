---
title: Bästa tillvägagångssätt för att uppgradera ditt projekt
description: Se en lista över de bästa sätten att uppgradera dina projektfiler.
feature: Cloud, Best Practices, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Bästa tillvägagångssätt för att uppgradera ditt projekt

Följ vedertagna standarder för byggen och distribution och använd arbetsflödet [Uppgraderingar och korrigeringar](../development/commerce-version.md) för att uppgradera programmet. Använd följande riktlinjer för att planera uppgraderings- och efteruppgraderingsarbetet:

- **Säkerhetskopiera ditt projekt** - Säkerhetskopiera databasen i integrerings-, mellanlagrings- och produktionsmiljöer innan du uppgraderar Adobe Commerce och eventuella tillägg från tredje part eller anpassade. Se [Säkerhetskopiera databasen](../development/commerce-version.md#project-backup).

- **Kontrollera kompatibilitetsproblem**-

   - Kontrollera att alla anpassade teman är kompatibla med den nya Adobe Commerce-versionen

   - När du har uppgraderat tillägg från tredje part och anpassade tillägg använder du kommandot `magento-cloud local:build` för att validera Composer-beroenden innan du distribuerar.

   - Granska versionsinformationen och tilläggsdokumentationen för Adobe Commerce för att kontrollera att du har implementerat de tillfälliga lösningar eller konfigurationsändringar som krävs för att åtgärda kända funktionsproblem och fel som gäller den uppgraderade Adobe Commerce-versionen och tillägg.

   - Kontrollera att de installerade tjänstversionerna är kompatibla med den nya Adobe Commerce-versionen och uppgradera tjänsterna efter behov. Se [Tjänster](../services/services-yaml.md).

   - Testa databasen för att åtgärda eventuella problem som uppstår vid uppdateringar av Adobe Commerce-versionen och tillägg.

   - Gör nödvändiga uppdateringar av miljöspecifika inställningar innan du distribuerar till fjärrmiljön.

   - Kontrollera att söktjänstversionen är kompatibel med PHP-klientversionen. Se [Konfigurera Elasticsearch](../services/elasticsearch.md) eller [Konfigurera OpenSearch](../services/opensearch.md).

- **Kontrollera databasanslutningen och tillgängligt lagringsutrymme i fjärrmiljöer**-

   - Använd SSH för att logga in på fjärrservern och verifiera anslutningen till MySQL-databasen. Se [Ansluta till databasen](../services/mysql.md#connect-to-the-database).

   - Verifiera tillgängligt lagringsutrymme i fjärrmiljön-Använd kommandot `disk free` om du vill visa och hantera tillgängligt diskutrymme i dina molnmiljöer. Se [Hantera diskutrymme](../storage/manage-disk-space.md).

      - Kontrollera storleken på den uppgraderade databasen och kontrollera att filen `services.yaml` har tillräckligt med ledigt diskutrymme.

      - Frigör diskutrymme-Rensa cacheminnet och rensa katalogerna `/log` och `/tmp` innan du distribuerar.

- **Planera och utför en uppgradering på lokala miljöer och integreringsmiljöer innan du distribuerar till mellanlagring** - Efter uppgraderingen testar du distributionen och löser eventuella problem.

- **Koppla kod till mellanlagring och sedan till produktion**-Testa och lös eventuella problem i mellanlagringsmiljön innan du flyttar ändringarna till produktionsmiljön.

- **Slutför efteruppgraderingsuppgifter**-

   - Använd SSH för att logga in på fjärrservern och verifiera följande:

      - Kontrollera indexerarens status och indexera om det behövs. Se [Hantera indexerare](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html?lang=sv-SE) i _Konfigurationsguiden_.

      - Kontrollera `cron`-loggarna och `cron_schedule`-tabellen i Adobe Commerce-databasen för att verifiera kron-status och kör kron-jobb igen efter behov.
Se [Loggning](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=sv-SE#logging) i _Konfigurationshandboken_.

   - Slutför UAT-testet (User Acceptance Testing UAT) efter uppgraderingen och åtgärda eventuella problem som rör uppgraderingar från tredje part och anpassade tillägg.
