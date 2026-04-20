---
title: Bästa tillvägagångssätt för att uppgradera ditt projekt
description: Se en lista över de bästa sätten att uppgradera dina projektfiler.
feature: Cloud, Best Practices, Upgrade
exl-id: 64f92739-9170-4cbf-90ef-aab6593a37ca
source-git-commit: 31494a956babaf15320d0ffa86fcba9e845d53a1
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# Bästa tillvägagångssätt för att uppgradera ditt projekt

Följ vedertagna standarder för byggen och distribution och använd arbetsflödet [Uppgraderingar och korrigeringar](../development/commerce-version.md) för att uppgradera programmet. Använd följande riktlinjer för att planera uppgraderings- och efteruppgraderingsarbetet:

- **Säkerhetskopiera ditt projekt** - Säkerhetskopiera databasen i integrerings-, mellanlagrings- och produktionsmiljöer innan du uppgraderar Adobe Commerce och eventuella tillägg från tredje part eller anpassade. Se [Säkerhetskopiera databasen](../development/commerce-version.md#project-backup).

- **Kontrollera kompatibilitetsproblem**-

   - Kontrollera att alla anpassade teman är kompatibla med den nya Adobe Commerce-versionen

   - När du har uppgraderat tillägg från tredje part och anpassade tillägg kan du använda kommandot `magento-cloud local:build` för att validera Composer-beroenden innan du distribuerar och köra [uppgraderingskompatibilitetsverktyget](#use-the-upgrade-compatibility-tool) för att identifiera inkompatibiliteter på kodnivå mellan den aktuella versionen och målversionen. Använd sedan verktyget [Kompatibilitet för uppgradering](https://fluffyjaws.adobe.com/#use-the-upgrade-compatibility-tool) för att identifiera och prioritera inkompatibiliteter på kodnivå innan du distribuerar till integrering, mellanlagring eller produktion.

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

      - Kontrollera indexerarens status och indexera om det behövs. Se [Hantera indexerare](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html) i _Konfigurationsguiden_.

      - Kontrollera `cron`-loggarna och `cron_schedule`-tabellen i Adobe Commerce-databasen för att verifiera kron-status och kör kron-jobb igen efter behov.
Se [Loggning](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html#logging) i _Konfigurationshandboken_.

   - Slutför UAT-testet (User Acceptance Testing UAT) efter uppgraderingen och åtgärda eventuella problem som rör uppgraderingar från tredje part och anpassade tillägg.

## Använda verktyget Kompatibilitet för uppgradering

Kör uppgraderingskompatibilitetsverktyget (UCT) som en del av föruppgraderingsanalysen för att förstå omfattningen och effekten av en uppgradering.

- UCT jämför den aktuella instansen med en målversion av Adobe Commerce och returnerar en lista med allvarliga problem, fel och varningar som måste åtgärdas innan du uppgraderar.
- Använd `--coming-version (-c)` för att jämföra med den planerade målversionen och `--ignore-current-version-compatibility-issues` för att bara fokusera på nya problem som introducerats i uppgraderingen.
- Behandla UCT HTML-rapporten som indata i uppgraderingskontrolllistan tillsammans med tilläggskompatibilitet, tjänstversioner och databaskontroller.

Information om inställningar och användning finns i:

- [Översikt över verktyget Kompatibilitet för uppgradering](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/overview)
- [Kör verktyget Kompatibilitet för uppgradering](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/run)

För molnhandlare som använder Site-Wide Analysis Tool kan du även utlösa UCT från kontrollpanelen och hämta HTML-rapporten direkt från widgeten. Se Integrera [webbplatsövergripande analysverktyg](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/integrate-analysis-tool).
