---
title: Uppgradera Commerce
description: Lär dig hur du uppgraderar Adobe Commerce-versionen i molninfrastrukturprojektet.
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: bcb5b00f7f203b53eae5c1bc1037cdb1837ad473
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 0%

---

# Uppgradera Commerce

Du kan uppgradera Adobe Commerce kodbas till en nyare version. Innan du uppgraderar ditt projekt bör du läsa [Systemkraven](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=sv-SE) i _installationshandboken_ för att få information om de senaste programvaruversionskraven.

Beroende på din projektkonfiguration kan din uppgradering innehålla följande:

- Uppdatera filen `.magento/services.yaml` med nya versioner för MariaDB (MySQL), OpenSearch, RabbitMQ och Redis för kompatibilitet med nya Adobe Commerce-versioner.
- Uppdatera filen `.magento.app.yaml` med nya inställningar för kopplingar och miljövariabler.
- Uppgradera tillägg från tredje part till den senaste version som stöds.

{{upgrade-tip}}

{{pro-update-service}}

## Konfigurationsfiler

Innan du uppgraderar programmet måste du uppdatera dina projektkonfigurationsfiler så att de tar hänsyn till ändringar av standardkonfigurationsinställningarna för Adobe Commerce i molninfrastrukturen eller för programmet. De senaste standardvärdena finns i GitHub-databasen [magento-cloud](https://github.com/magento/magento-cloud).

### composer.json

Kontrollera alltid att beroendena i filen `composer.json` är kompatibla med Adobe Commerce-versionen innan du uppgraderar.

Så här uppdaterar du filen `composer.json` för Adobe Commerce version 2.4.4 och senare**:

1. Lägg till följande `allow-plugins` i avsnittet `config`:

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. Lägg till följande plugin-program i avsnittet `require`:

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. Lägg till följande komponent i avsnittet `extra:component_paths`:

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. Spara filen. Verkställ eller skicka inte ändringar till din gren än.

1. Fortsätt med uppgraderingsprocessen.

## Säkerhetskopiering av projekt

Vi rekommenderar att du skapar en säkerhetskopia av ditt projekt före en uppgradering. Gör så här för att säkerhetskopiera integrerings-, mellanlagrings- och produktionsmiljöer.

**Så här säkerhetskopierar du databasen och koden för integreringsmiljön**:

1. Skapa en lokal säkerhetskopia av fjärrdatabasen.

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >Kommandot `magento-cloud db:dump` kör kommandot [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) med flaggan `--single-transaction` som gör att du kan säkerhetskopiera databasen utan att låsa tabellerna.

1. Säkerhetskopiera kod och media.

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   Du kan också utelämna `[--media]` om du har ett stort antal statiska filer som redan finns i källkontrollen.

**Så här säkerhetskopierar du databasen för förproduktionsmiljö eller produktionsmiljö innan du distribuerar**:

1. Använd SSH för att logga in i fjärrmiljön.

1. Skapa en [databasdump](../storage/database-dump.md). Om du vill välja en målkatalog för DB-dumpen använder du alternativet `--dump-directory`.

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   Dumpningsåtgärden skapar en `dump-<timestamp>.sql.gz`-arkivfil i fjärrprojektkatalogen. Se [Säkerhetskopiera databas](../storage/database-dump.md).

## Programuppgradering

Granska informationen om [tjänstversionerna](../services/services-yaml.md#service-versions) för de senaste programversionskraven innan du uppgraderar ditt program.

**Så här uppgraderar du programversionen**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Ange [version-begränsningen](overview.md#cloud-metapackage) för måluppgraderingsversionen. Det här steget är bara nödvändigt om målversionen ligger utanför den befintliga begränsningen.

   ```bash
   composer require-commerce "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >Du måste använda syntaxen för versionsbegränsning för att kunna uppdatera paketet `ece-tools`. Du hittar versionsbegränsningen i filen `composer.json` för den version av [programmallen](https://github.com/magento/magento-cloud/blob/master/composer.json) som du använder för uppgraderingen.

1. Uppdatera din `composer.json`-fil med Commerce viktigaste uppgraderingsversion.

   ```bash
   composer require-commerce magento/product-enterprise-edition 2.4.8 --no-update
   ```

1. Om du använder B2B ska du uppdatera din `composer.json`-fil med den [version](https://experienceleague.adobe.com/sv/docs/commerce-operations/release/product-availability#adobe-authored-extensions) som stöds för Commerce.

   ```bash
   composer require-commerce magento/extension-b2b 1.5.2 --no-update
   ```

1. Uppdatera projektberoenden.

   ```bash
   composer update
   ```

1. Granska de korrigeringar som används för närvarande:

   - Om det finns korrigeringsfiler installerade i katalogen `m2-hotfixes` [skickar du en Adobe Commerce Support-anmälan](https://experienceleague.adobe.com/sv/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) och arbetar med Adobe Commerce Support för att kontrollera vilka korrigeringsfiler som fortfarande kan användas i den nya versionen. Ta bort den eller de icke tillämpliga korrigeringarna från katalogen `m2-hotfixes`.

   - Om [kvalitetsuppdateringar] används i filen `.magento.env.yaml` kontrollerar du om de fortfarande kan användas i den nya versionen. Ta bort den eller de korrigeringar som inte är tillämpliga från `QUALITY_PATCHES`-avsnittet i `.magento.env.yaml`-filen.

   **Metod 1**: [Verifiera tillämpliga versioner i versionsinformationen för kvalitetspatchar](https://experienceleague.adobe.com/sv/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **Metod 2**: [Visa tillgängliga korrigeringar och status](https://experienceleague.adobe.com/sv/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **Metod 3**: [Sök efter korrigeringsfiler](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=sv-SE)


1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   `git add -A` krävs för att lägga till alla ändrade filer i källkontrollen på grund av hur Composer konverterar baspaket. Både `composer install` och `composer update` konverterar filer från baspaketet (`magento/magento2-base` och `magento/magento2-ee-base`) till paketroten.

   De filer som Composer konverterar tillhör den nya versionen av Adobe Commerce, som skriver över den gamla versionen av samma filer. För närvarande är konvertering inaktiverat i Adobe Commerce, så du måste lägga till de konverterade filerna i källkontrollen.

1. Vänta tills distributionen är klar.

1. Verifiera uppgraderingen i integrerings-, mellanlagrings- eller produktionsmiljön genom att använda SSH för att logga in och kontrollera versionen.

   ```bash
   php bin/magento --version
   ```

### Uppgradera tillägg

Granska tillägg- och modulsidor från tredje part på Marketplace eller andra företagswebbplatser och verifiera stödet för Adobe Commerce och Adobe Commerce i molninfrastrukturen. Om du måste uppgradera tillägg och moduler från tredje part rekommenderar Adobe att du arbetar i en ny integreringsgren med dina tillägg inaktiverade.

**Så här verifierar och uppgraderar du dina tillägg**:

1. Skapa en gren på din lokala arbetsstation.

1. Inaktivera tilläggen efter behov.

1. Ladda ned tilläggsuppgraderingar när de blir tillgängliga.

1. Installera uppgraderingen enligt dokumentationen från tredje part.

1. Aktivera och testa tillägget.

1. Lägg till, implementera och skicka kodändringarna till fjärrkontrollen.

1. Kör till och testa i er integreringsmiljö.

1. Kör till mellanlagringsmiljön för att testa i en förproduktionsmiljö.

Adobe rekommenderar starkt att du uppgraderar din produktionsmiljö _före_, inklusive de uppgraderade tilläggen när du startar webbplatsen.

>[!NOTE]
>
>När du uppgraderar din programversion uppdateras uppgraderingsprocessen automatiskt till den senaste versionen av modulen [Snabbt CDN](../cdn/fastly.md#fastly-cdn-module-for-magento-2) .

## Felsök uppgraderingen

Om uppgraderingen misslyckades får du ett felmeddelande i webbläsaren om att du inte kan komma åt butiken eller administrationspanelen:

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**Så här löser du felet**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Använd SSH för att logga in i fjärrmiljön.

   ```bash
   magento-cloud ssh
   ```

1. Öppna filen `./app/var/report/<error number>`.

1. [Granska loggarna](../test/log-locations.md) och ta reda på orsaken till problemet.

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
