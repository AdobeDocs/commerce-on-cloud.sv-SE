---
source-git-commit: 7f2934af84c947046fed3a32c3b6e2937aed418a
workflow-type: tm+mt
source-wordcount: '2554'
ht-degree: 4%

---
# Felkoder för ECE-verktyg

<!--Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository.-->

## Allvarliga fel

Kritiska fel indikerar ett problem med Commerce i molninfrastrukturens projektkonfiguration som orsakar ett distributionsfel, till exempel felaktig, stöds inte eller så saknas en konfiguration för nödvändiga inställningar. Innan du kan distribuera måste du uppdatera konfigurationen för att åtgärda felen.

### Byggfas

| Felkod | Bygg steg | Felbeskrivning (rubrik) | Föreslagen åtgärd |
| - | - | - | - |
| 2 |  | Kan inte skriva till filen `./app/etc/env.php` | Distributionsskriptet kan inte göra nödvändiga ändringar i filen `/app/etc/env.php`. Kontrollera din behörighet i filsystemet. |
| 3 |  | Konfigurationen har inte definierats i filen `schema.yaml` | Konfigurationen har inte definierats i filen `./vendor/magento/ece-tools/config/schema.yaml`. Kontrollera att namnet på config-variabeln är korrekt och definierat. |
| 4 |  | Det gick inte att parsa filen `.magento.env.yaml` | Filformatet `./.magento.env.yaml` är ogiltigt. Använd en YAML-tolk för att kontrollera syntaxen och åtgärda eventuella fel. |
| 5 |  | Det går inte att läsa filen `.magento.env.yaml` | Det går inte att läsa filen `./.magento.env.yaml`. Kontrollera filbehörigheter. |
| 6 |  | Det går inte att läsa filen `.schema.yaml` | Det går inte att läsa filen `./vendor/magento/ece-tools/config/magento.env.yaml`. Kontrollera filbehörigheter och återdistribuera (`magento-cloud environment:redeploy`). |
| 7 | refresh-modules | Kan inte skriva till filen `./app/etc/config.php` | Distributionsskriptet kan inte göra nödvändiga ändringar i filen `/app/etc/config.php`. Kontrollera din behörighet i filsystemet. |
| 8 | validate-config | Det går inte att läsa filen `composer.json` | Det går inte att läsa filen `./composer.json`. Kontrollera filbehörigheter. |
| 9 | validate-config | Filen `composer.json` saknar det obligatoriska avsnittet för automatisk inläsning | Det obligatoriska `autoload`-avsnittet saknas i filen `composer.json`. Jämför avsnittet för automatisk inläsning med filen `composer.json` i molnmallen och lägg till den konfiguration som saknas. |
| 10 | validate-config | Filen `.magento.env.yaml` innehåller ett alternativ som inte har deklarerats i schemat, eller ett alternativ som har konfigurerats med ett ogiltigt värde eller en ogiltig scen | Filen `./.magento.env.yaml` innehåller en ogiltig konfiguration. Mer information finns i felloggen. |
| 11 | refresh-modules | Kommandot misslyckades: `/bin/magento module:enable --all` | Försök köra `composer update` lokalt. Bekräfta sedan och skicka den uppdaterade `composer.lock`-filen. Mer information finns i `cloud.log`. Lägg till alternativet `VERBOSE_COMMANDS: '-vvv'` i filen `.magento.env.yaml` om du vill ha mer detaljerad information om kommandoutdata. |
| 12 | apply-patches | Det gick inte att tillämpa korrigeringen |  |
| 13 | set-report-dir-nesting-level | Det går inte att skriva till filen `/pub/errors/local.xml` |  |
| 14 | copy-sample-data | Det gick inte att kopiera exempeldatafiler |  |
| 15 | compile-di | Kommandot misslyckades: `/bin/magento setup:di:compile` | Mer information finns i `cloud.log`. Lägg till `VERBOSE_COMMANDS: '-vvv'` i `.magento.env.yaml` om du vill ha mer detaljerade kommandoutdata. |
| 16 | dump-autoload | Kommandot misslyckades: `composer dump-autoload` | Kommandot `composer dump-autoload` misslyckades. Mer information finns i `cloud.log`. |
| 17 | run-baler | Kommandot för att köra `Baler` för JavaScript-paketering misslyckades | Kontrollera miljövariabeln `SCD_USE_BALER` för att verifiera att Baler-modulen är konfigurerad och aktiverad för JS-paketering. Om du inte behöver Baler-modulen anger du `SCD_USE_BALER: false`. |
| 18 | compress-static-content | Nödvändigt verktyg hittades inte (timeout, bash) |  |
| 19 | deploy-static-content | Kommandot `/bin/magento setup:static-content:deploy` misslyckades | Mer information finns i `cloud.log`. Lägg till alternativet `VERBOSE_COMMANDS: '-vvv'` i filen `.magento.env.yaml` om du vill ha mer detaljerad information om kommandoutdata. |
| 20 | compress-static-content | Komprimeringen av statiskt innehåll misslyckades | Mer information finns i `cloud.log`. |
| 21 | säkerhetskopieringsdata: statiskt innehåll | Det gick inte att kopiera statiskt innehåll till katalogen `init` | Mer information finns i `cloud.log`. |
| 22 | säkerhetskopieringsdata: skrivbara kataloger | Det gick inte att kopiera vissa skrivbara kataloger till katalogen `init` | Det gick inte att kopiera skrivbara kataloger till mappen `./init`. Kontrollera din behörighet i filsystemet. |
| 23 |  | Det går inte att skapa ett logger-objekt |  |
| 24 | säkerhetskopieringsdata: statiskt innehåll | Det gick inte att rensa katalogen `./init/pub/static/` | Det gick inte att rensa mappen `./init/pub/static`. Kontrollera din behörighet i filsystemet. |
| 25 |  | Det går inte att hitta Composer-paketet | Om du har installerat Adobe Commerce-programversionen direkt från GitHub-databasen kontrollerar du att miljövariabeln `DEPLOYED_MAGENTO_VERSION_FROM_GIT` är konfigurerad. |
| 26 | validate-config | Ta bort modulkonfigurationen Magento Braintree som inte längre stöds i Adobe Commerce och Magento Open Source 2.4 och senare. | Stöd för Braintree-modulen ingår inte längre i Magento 2.4.0 och senare. Ta bort variabeln CONFIG_STORES_DEFAULT_PAYMENT_BRAINTREE_CHANNEL från variabelavsnittet för variabler i filen `.magento.app.yaml`. Använd ett officiellt tillägg från Commerce Marketplace i stället för betalningssupport från Braintree. |

### Distribuera fas

| Felkod | Distribuera steg | Felbeskrivning (rubrik) | Föreslagen åtgärd |
| - | - | - | - |
| 101 | fördistribution: cache | Felaktig cachekonfiguration (port eller värd saknas) | Cachekonfigurationen saknar nödvändiga parametrar `server` eller `port`. Mer information finns i `cloud.log`. |
| 102 |  | Kan inte skriva till filen `./app/etc/env.php` | Distributionsskriptet kan inte göra nödvändiga ändringar i filen `/app/etc/env.php`. Kontrollera din behörighet i filsystemet. |
| 103 |  | Konfigurationen har inte definierats i filen `schema.yaml` | Konfigurationen har inte definierats i filen `./vendor/magento/ece-tools/config/schema.yaml`. Kontrollera att namnet på config-variabeln är korrekt och att det är definierat. |
| 104 |  | Det gick inte att parsa filen `.magento.env.yaml` | Konfigurationen har inte definierats i filen `./vendor/magento/ece-tools/config/schema.yaml`. Kontrollera att namnet på config-variabeln är korrekt och att det är definierat. |
| 105 |  | Det går inte att läsa filen `.magento.env.yaml` | Det går inte att läsa filen `./.magento.env.yaml`. Kontrollera filbehörigheter. |
| 106 |  | Det går inte att läsa filen `.schema.yaml` |  |
| 107 | fördistribution: rensning-redis-cache | Det gick inte att rensa Redis-cachen | Det gick inte att rensa Redis-cachen. Kontrollera att Redis cachekonfigurationen är korrekt och att Redis-tjänsten är tillgänglig. Se [tjänsten Setup Redis](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/redis.html). |
| 140 | fördistribution: rensningsnyckelcache | Det gick inte att rensa Valkey-cachen | Det gick inte att rensa Valkey-cachen. Kontrollera att konfigurationen för Valkey-cache är korrekt och att Valkey-tjänsten är tillgänglig. Se [Konfigurera Valkey-tjänsten](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/valkey.html). |
| 108 | fördriftsätta: set-production-mode | Kommandot `/bin/magento maintenance:enable` misslyckades | Mer information finns i `cloud.log`. Lägg till alternativet `VERBOSE_COMMANDS: '-vvv'` i filen `.magento.env.yaml` om du vill ha mer detaljerad information om kommandoutdata. |
| 109 | validate-config | Felaktig databaskonfiguration | Kontrollera att miljövariabeln `DATABASE_CONFIGURATION` är korrekt konfigurerad. |
| 110 | validate-config | Felaktig sessionskonfiguration | Kontrollera att miljövariabeln `SESSION_CONFIGURATION` är korrekt konfigurerad. Konfigurationen måste innehålla minst parametern `save`. |
| 111 | validate-config | Felaktig sökkonfiguration | Kontrollera att miljövariabeln `SEARCH_CONFIGURATION` är korrekt konfigurerad. Konfigurationen måste innehålla minst parametern `engine`. |
| 112 | validate-config | Felaktig resurskonfiguration | Kontrollera att miljövariabeln `RESOURCE_CONFIGURATION` är korrekt konfigurerad. Konfigurationen måste innehålla minst parametern `connection`. |
| 113 | validate-config:elasticsuite-integrity | ElasticSuite är installerat, men Elasticsearch-tjänsten är inte tillgänglig | Kontrollera att miljövariabeln `SEARCH_CONFIGURATION` är korrekt konfigurerad och verifiera att Elasticsearch-tjänsten är tillgänglig. |
| 114 | validate-config:elasticsuite-integrity | ElasticSuite är installerat, men en annan sökmotor används | ElasticSuite har installerats, men en annan sökmotor har konfigurerats. Uppdatera miljövariabeln `SEARCH_CONFIGURATION` för att aktivera Elasticsearch och verifiera tjänstkonfigurationen i Elasticsearch-filen `services.yaml`. |
| 115 |  | Databasfrågekörningen misslyckades |  |
| 116 | install-update: installation | Kommandot `/bin/magento setup:install` misslyckades | Mer information finns i `cloud.log` och `install_upgrade.log`. Lägg till alternativet `VERBOSE_COMMANDS: '-vvv'` i filen `.magento.env.yaml` om du vill ha mer detaljerad information om kommandoutdata. |
| 117 | install-update: config-import | Kommandot `app:config:import` misslyckades | Mer information finns i `cloud.log`. Lägg till alternativet `VERBOSE_COMMANDS: '-vvv'` i filen `.magento.env.yaml` om du vill ha mer detaljerad information om kommandoutdata. |
| 118 |  | Nödvändigt verktyg hittades inte (timeout, bash) |  |
| 119 | install-update: deploy-static-content | Kommandot `/bin/magento setup:static-content:deploy` misslyckades | Mer information finns i `cloud.log`. Lägg till alternativet `VERBOSE_COMMANDS: '-vvv'` i filen `.magento.env.yaml` om du vill ha mer detaljerad information om kommandoutdata. |
| 120 | compress-static-content | Komprimeringen av statiskt innehåll misslyckades | Mer information finns i `cloud.log`. |
| 121 | deploy-static-content:generate | Det går inte att uppdatera den distribuerade versionen | Det går inte att uppdatera filen `./pub/static/deployed_version.txt`. Kontrollera din behörighet i filsystemet. |
| 122 | rent statiskt innehåll | Det gick inte att rensa statiska innehållsfiler |  |
| 123 | install-update: split-db | Kommandot `/bin/magento setup:db-schema:split` misslyckades | Mer information finns i `cloud.log`. Lägg till alternativet `VERBOSE_COMMANDS: '-vvv'` i filen `.magento.env.yaml` om du vill ha mer detaljerad information om kommandoutdata. |
| 124 | rensa-vy-förbearbetad | Det gick inte att rensa mappen `var/view_preprocessed` | Det går inte att rensa mappen `./var/view_preprocessed`. Kontrollera din behörighet i filsystemet. |
| 125 | install-update: reset-password | Det gick inte att uppdatera filen `/var/credentials_email.txt` | Det gick inte att uppdatera filen `/var/credentials_email.txt`. Kontrollera din behörighet i filsystemet. |
| 126 | install-update: update | Kommandot `/bin/magento setup:upgrade` misslyckades | Mer information finns i `cloud.log` och `install_upgrade.log`. Lägg till alternativet `VERBOSE_COMMANDS: '-vvv'` i filen `.magento.env.yaml` om du vill ha mer detaljerad information om kommandoutdata. |
| 127 | rensningscache | Kommandot `/bin/magento cache:flush` misslyckades | Mer information finns i `cloud.log`. Lägg till alternativet `VERBOSE_COMMANDS: '-vvv'` i filen `.magento.env.yaml` om du vill ha mer detaljerad information om kommandoutdata. |
| 128 | disable-maintain-mode | Kommandot `/bin/magento maintenance:disable` misslyckades | Mer information finns i `cloud.log`. Lägg till `VERBOSE_COMMANDS: '-vvv'` i `.magento.env.yaml` om du vill ha mer detaljerade kommandoutdata. |
| 129 | install-update: reset-password | Det går inte att läsa mallen för lösenordsåterställning |  |
| 130 | install-update: cache_type | Kommandot misslyckades: `php ./bin/magento cache:enable` | Kommandot `php ./bin/magento cache:enable` körs bara när Adobe Commerce installerades, men filen `./app/etc/env.php` saknas eller är tom i början av distributionen. Mer information finns i `cloud.log`. Lägg till `VERBOSE_COMMANDS: '-vvv'` i `.magento.env.yaml` om du vill ha mer detaljerade kommandoutdata. |
| 131 | install-update | Nyckelvärdet `crypt/key` finns inte i filen `./app/etc/env.php` eller i molnmiljövariabeln `CRYPT_KEY` | Det här felet inträffar om filen `./app/etc/env.php` inte finns när Adobe Commerce-distributionen startar, eller om värdet `crypt/key` är odefinierat. Om du migrerade databasen från en annan miljö hämtar du krypteringsnyckelvärdet från den miljön. Lägg sedan till värdet i molnmiljövariabeln [CRYPT_KEY](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy.html#crypt_key) i den aktuella miljön. Se [Adobe Commerce-krypteringsnyckel](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/develop/overview.html#gather-credentials). Om du av misstag tog bort filen `./app/etc/env.php` använder du följande kommando för att återställa den från de säkerhetskopiefiler som skapades från en tidigare distribution: `./vendor/bin/ece-tools backup:restore` CLI-kommando.&quot; |
| 132 |  | Kan inte ansluta till Elasticsearch-tjänsten | Kontrollera om Elasticsearch har giltiga inloggningsuppgifter och verifiera att tjänsten körs |
| 137 |  | Det går inte att ansluta till OpenSearch-tjänsten | Kontrollera om det finns giltiga OpenSearch-autentiseringsuppgifter och verifiera att tjänsten körs |
| 133 | validate-config | Ta bort modulkonfigurationen Magento Braintree som inte längre stöds i Adobe Commerce eller Magento Open Source 2.4 och senare versioner. | Stöd för Braintree-modulen ingår inte längre i Adobe Commerce eller Magento Open Source 2.4.0 och senare. Ta bort variabeln CONFIG_STORES_DEFAULT_PAYMENT_BRAINTREE_CHANNEL från variabelavsnittet för variabler i filen `.magento.app.yaml`. Använd i stället ett officiellt Braintree Payments-tillägg från Commerce Marketplace för support från Braintree. |
| 134 | validate-config | Adobe Commerce och Magento Open Source 2.4.0 kräver att Elasticsearch-tjänsten är installerad | Installera Elasticsearch-tjänsten |
| 138 | validate-config | Adobe Commerce och Magento Open Source 2.4.4 kräver att OpenSearch- eller Elasticsearch-tjänsten installeras | Installera OpenSearch-tjänsten |
| 135 | validate-config | Sökmotorn måste anges till Elasticsearch för Adobe Commerce och Magento Open Source >= 2.4.0 | Kontrollera variabeln SEARCH_CONFIGURATION för alternativet `engine`. Om den är konfigurerad tar du bort alternativet eller anger värdet som elasticsearch. |
| 136 | validate-config | Delad databas har tagits bort från Adobe Commerce och Magento Open Source 2.5.0. | Om du använder en delad databas måste du återgå till eller migrera till en enda databas eller använda ett annat tillvägagångssätt. |
| 139 | validate-config | Felaktig sökmotor | Denna version av Adobe Commerce eller Magento Open Source stöder inte OpenSearch. Använd version 2.3.7-p3, 2.4.3-p2 eller senare |

### Steg efter driftsättning

| Felkod | Steg efter distribution | Felbeskrivning (rubrik) | Föreslagen åtgärd |
| - | - | - | - |
| 201 | is-deploy-failed | Distributionsfasen misslyckades |  |
| 202 |  | Filen `./app/etc/env.php` är inte skrivbar | Distributionsskriptet kan inte göra nödvändiga ändringar i filen `/app/etc/env.php`. Kontrollera din behörighet i filsystemet. |
| 203 |  | Konfigurationen har inte definierats i filen `schema.yaml` | Konfigurationen har inte definierats i filen `./vendor/magento/ece-tools/config/schema.yaml`. Kontrollera att namnet på config-variabeln är korrekt och att det är definierat. |
| 204 |  | Det gick inte att parsa filen `.magento.env.yaml` | Filformatet `./.magento.env.yaml` är ogiltigt. Använd en YAML-tolk för att kontrollera syntaxen och åtgärda eventuella fel. |
| 205 |  | Det går inte att läsa filen `.magento.env.yaml` | Kontrollera filbehörigheter. |
| 206 |  | Det går inte att läsa filen `.schema.yaml` |  |
| 207 | uppvärmning | Det gick inte att förhandsladda vissa uppvärmningssidor |  |
| 208 | time-to-first-byte | Det gick inte att testa tiden till den första byten (TTFB) |  |
| 227 | rensningscache | Kommandot `/bin/magento cache:flush` misslyckades | Mer information finns i `cloud.log`. Lägg till `VERBOSE_COMMANDS: '-vvv'` i `.magento.env.yaml` om du vill ha mer detaljerade kommandoutdata. |

### Allmänt

| Felkod | Allmänt steg | Felbeskrivning (rubrik) | Föreslagen åtgärd |
| - | - | - | - |
| 243 |  | Konfigurationen har inte definierats i filen `schema.yaml` | Kontrollera att namnet på config-variabeln är korrekt och att det är definierat. |
| 244 |  | Det gick inte att parsa filen `.magento.env.yaml` | Filformatet `./.magento.env.yaml` är ogiltigt. Använd en YAML-tolk för att kontrollera syntaxen och åtgärda eventuella fel. |
| 245 |  | Det går inte att läsa filen `.magento.env.yaml` | Det går inte att läsa filen `./.magento.env.yaml`. Kontrollera filbehörigheter. |
| 246 |  | Det går inte att läsa filen `.schema.yaml` |  |
| 247 |  | Det går inte att generera en modul för inaktivering | Mer information finns i `cloud.log`. |
| 248 |  | Det går inte att aktivera en modul för inaktivering | Mer information finns i `cloud.log`. |
| 249 |  | Det gick inte att generera modulen AdobeCommerceWebhookPlugins | Mer information finns i `cloud.log`. |
| 250 |  | Det gick inte att aktivera modulen AdobeCommerceWebhookPlugins | Mer information finns i `cloud.log`. |

## Varningsfel

Varningsfel indikerar ett problem med Commerce i molninfrastrukturens projektkonfiguration, till exempel felaktig, borttagen, stöds inte eller saknade konfigurationsinställningar för valfria funktioner som kan påverka webbplatsens funktion. Även om en varning inte orsakar ett distributionsfel bör du granska varningsmeddelanden och uppdatera konfigurationen för att åtgärda dem.

### Byggfas

| Felkod | Bygg steg | Felbeskrivning (rubrik) | Föreslagen åtgärd |
| - | - | - | - |
| 1001 | validate-config | Filen app/etc/config.php finns inte |  |
| 1002 | validate-config | The ./build_options.ini stöds inte längre |  |
| 1003 | validate-config | Modulavsnittet saknas i den delade konfigurationsfilen |  |
| 1004 | validate-config | Konfigurationen är inte kompatibel med den här versionen av Magento |  |
| 1005 | validate-config | SCD-alternativ ignorerade |  |
| 1006 | validate-config | Det konfigurerade tillståndet är inte idealiskt |  |
| 1007 | run-baler | JS-paketering för baler kan inte användas |  |

### Distribuera fas

| Felkod | Distribuera steg | Felbeskrivning (rubrik) | Föreslagen åtgärd |
| - | - | - | - |
| 2001 | fördistribution:cache | Cachen är konfigurerad för en Redis-tjänst som inte är tillgänglig. Konfigurationen ignoreras. |  |
| 2032 | fördistribution:cache | Cachen har konfigurerats för en Valkey-tjänst som inte är tillgänglig. Konfigurationen ignoreras. |  |
| 2002 | validate-config | Det konfigurerade tillståndet är inte idealiskt |  |
| 2003 | validate-config | Värdet för katalogkapslingsnivån för felrapportering har inte konfigurerats |  |
| 2004 | validate-config | Ogiltig konfiguration i ./pub/errors/local.xml fil. |  |
| 2005 | validate-config | Administratörsdata används endast för att skapa en administratörsanvändare under den första installationen. Alla ändringar av administratörsdata ignoreras under uppgraderingsprocessen. | Efter den första installationen kan du ta bort admindata från konfigurationen. |
| 2006 | validate-config | Administratörsanvändaren skapades inte eftersom administratörens e-postadress inte har angetts | Efter installationen kan du skapa en administratörsanvändare manuellt: Använd ssh för att ansluta till din miljö. Kör sedan kommandot `bin/magento admin:user:create`. |
| 2007 | validate-config | Uppdatera PHP-version till rekommenderad version |  |
| 2008 | validate-config | Solr-stödet har ersatts i Adobe Commerce och Magento Open Source 2.1. |  |
| 2009 | validate-config | Solr stöds inte längre av Adobe Commerce och Magento Open Source 2.2 eller senare. |  |
| 2010 | validate-config | Elasticsearch-tjänsten installeras på infrastrukturskiktet, men används inte som sökmotor. | Överväg att ta bort Elasticsearch-tjänsten från infrastrukturskiktet för att optimera resursanvändningen. |
| 2011 | validate-config | Elasticsearch tjänstversion på infrastrukturskiktet är inte kompatibel med den aktuella versionen av modulen elasticsearch/elasticsearch som används av ditt Adobe Commerce-program. |  |
| 2012 | validate-config | Den aktuella konfigurationen är inte kompatibel med den här versionen av Adobe Commerce |  |
| 2013 | validate-config | SCD-alternativ ignorerades eftersom distributionsprocessen inte kördes i byggfasen |  |
| 2014 | validate-config | Konfigurationen innehåller inaktuella variabler eller värden |  |
| 2015 | validate-config | Miljökonfigurationen är inte giltig |  |
| 2016 | validate-config | Det går inte att avkoda JSON-typkonfigurationen |  |
| 2017 | validate-config | Den aktuella konfigurationen är inte kompatibel med den här versionen av Adobe Commerce |  |
| 2018 | validate-config | Vissa tjänster har genomgått EOL |  |
| 2019 | validate-config | Konfigurationsalternativet för MySQL-sökning är föråldrat | Använd Elasticsearch istället. |
| 2029 | validate-config | Delad databas har tagits bort i Adobe Commerce och Magento Open Source 2.4.2 och tas bort i 2.5. | Om du använder en delad databas bör du börja planera att återgå till eller migrera till en enskild databas eller använda ett annat tillvägagångssätt. |
| 2020 | install-update | Adobe Commerce-installationen slutfördes, men konfigurationsfilen `app/etc/env.php` saknas eller är tom. | Nödvändiga data återställs från miljökonfigurationer och från filen .magento.env.yaml. |
| 2021 | install-update:db-connection | För delade databaser används anpassade anslutningar |  |
| 2022 | install-update:db-connection | Du har ändrat till en databaskonfiguration som inte är kompatibel med slavanslutningen. |  |
| 2023 | install-update:split-db | Aktivering av en delad databas hoppas över. |  |
| 2024 | install-update:split-db | Variabeln SPLIT_DB saknar konfigurationen för delade anslutningstyper. |  |
| 2025 | install-update:split-db | Slavanslutningen har inte angetts. |  |
| 2026 | pre-deploy:restore-writable-dirs | Det gick inte att återställa vissa data som genererats under byggfasen till de monterade katalogerna | Mer information finns i `cloud.log`. |
| 2027 | validate-config:image-mode-variable | Lägesvärdet för miljövariabeln MAGE_MODE stöds inte | Ta bort miljövariabeln MAGE_MODE eller ändra dess värde till &quot;production&quot;. Adobe Commerce i molninfrastrukturen har endast stöd för produktionsläget. |
| 2028 | fjärrlagring | Det gick inte att aktivera fjärrlagring. | Verifiera autentiseringsuppgifter för fjärrlagring. |
| 2030 | validate-config | Elasticsearch och OpenSearch-tjänsterna installeras båda på infrastrukturnivå. Adobe Commerce och Magento Open Source 2.4.4 och senare använder OpenSearch som standard | Ta bort Elasticsearch- eller OpenSearch-tjänsten från infrastrukturlagret för att optimera resursanvändningen. |

### Steg efter driftsättning

| Felkod | Steg efter distribution | Felbeskrivning (rubrik) | Föreslagen åtgärd |
| - | - | - | - |
| 3001 | validate-config | Felsökningsloggning är aktiverat i Adobe Commerce | Aktivera inte felsökningsloggning för produktionsmiljöerna om du vill spara diskutrymme. |
| 3002 | uppvärmning | Det går inte att hämta arkiv-URL:er |  |
| 3003 | uppvärmning | Det går inte att hämta arkiv-URL |  |
| 3004 | säkerhetskopia | Kan inte skapa säkerhetskopior |  |

### Allmänt

| Felkod | Allmänt steg | Felbeskrivning (rubrik) | Föreslagen åtgärd |
| - | - | - | - |
| 4001 |  | Det går inte att hämta antalet systemprocessorer: |  |
