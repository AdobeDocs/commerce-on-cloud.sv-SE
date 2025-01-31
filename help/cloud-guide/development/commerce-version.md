---
title: Uppgradera Commerce
description: Lär dig hur du uppgraderar Adobe Commerce-versionen i molninfrastrukturprojektet.
feature: Cloud, Upgrade
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1547'
ht-degree: 0%

---

# Uppgradera Commerce

Du kan uppgradera Adobe Commerce kodbas till en nyare version. Innan du uppgraderar ditt projekt bör du läsa [Systemkraven](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) i _installationshandboken_ för att få information om de senaste programvaruversionskraven.

Beroende på din projektkonfiguration kan din uppgradering innehålla följande:

- Uppdateringstjänster - som MariaDB (MySQL), OpenSearch, RabbitMQ och Redis - för kompatibilitet med nya Adobe Commerce-versioner.
- Konvertera en äldre konfigurationshanteringsfil.
- Uppdatera filen `.magento.app.yaml` med nya inställningar för kopplingar och miljövariabler.
- Uppgradera tillägg från tredje part till den senaste version som stöds.
- Uppdatera filen `.gitignore`.

{{upgrade-tip}}

{{pro-update-service}}

## Uppgradera från äldre versioner

Om du påbörjar en uppgradering från en version av Commerce som är äldre än 2.1 kan vissa begränsningar i Adobe Commerce kodbas påverka din möjlighet att _uppdatera_ till en specifik ECE-verktygsversion eller att _uppgradera_ till nästa version av Commerce som stöds. Använd följande tabell för att fastställa den bästa banan:

| Aktuell version | Uppgraderingssökväg |
| ----------------- | ------------ |
| 2.1.3 och tidigare versioner | Uppgradera Adobe Commerce till version 2.1.4 eller senare innan du fortsätter. Utför sedan en [engångsuppgradering för att installera ECE-Tools](../dev-tools/install-package.md). |
| 2.1.4 - 2.1.14 | [Uppdatera ECE-verktygspaket](../dev-tools/update-package.md).<br>Se versionsinformation för [2002.0.9](../release-notes/cloud-release-archive.md#v200209) och senare versioner av 2002.0.x. |
| 2.1.15 - 2.1.16 | [Uppdatera ECE-verktygspaket](../dev-tools/update-package.md).<br>Se versionsinformation för [2002.0.9](../release-notes/cloud-release-archive.md#v200209) och senare. |
| 2.2.x och senare | [Uppdatera ECE-verktygspaket](../dev-tools/update-package.md).<br>Se versionsinformation för [2002.0.8](../release-notes/cloud-release-archive.md#v200208) och senare. |

{style="table-layout:auto"}

{{ece-tools-package}}

## Konfigurationshantering

Äldre versioner av Adobe Commerce, som 2.1.4 eller senare till 2.2.x eller senare, använde en `config.local.php`-fil för Configuration Management. I Adobe Commerce version 2.2.0 och senare används filen `config.php`, som fungerar exakt som filen `config.local.php` men som har olika konfigurationsinställningar som innehåller en lista över aktiverade moduler och ytterligare konfigurationsalternativ.

När du uppgraderar från en äldre version måste du migrera filen `config.local.php` för att kunna använda den nyare filen `config.php`. Gör så här för att säkerhetskopiera konfigurationsfilen och skapa en.

**Så här skapar du en tillfällig `config.php` fil**:

1. Skapa en kopia av filen `config.local.php` och ge den namnet `config.php`.

1. Lägg till den här filen i mappen `app/etc` i ditt projekt.

1. Lägg till och implementera filen på din gren.

1. Skicka filen till din integrationsgren.

1. Fortsätt med uppgraderingsprocessen.

>[!WARNING]
>
>När du har uppgraderat kan du ta bort filen `config.php` och generera en ny, fullständig fil. Du kan bara ta bort den här filen om du vill ersätta den här gången. När du har genererat en ny, fullständig `config.php`-fil kan du inte ta bort filen för att skapa en ny. Se [Konfigurationshantering och distribution av pipeline](../store/store-settings.md).

### Verifiera Zend Framework Composer-beroenden

När du uppgraderar till **2.3.x eller senare från 2.2.x** kontrollerar du att Zend Framework-beroenden har lagts till i `autoload` -egenskapen för `composer.json`-filen som stöder Laminas. Denna plugin stöder nya krav för Zend Framework, som har migrerat till Laminas-projektet. Se [Migrering av Zend Framework till Laminas Project](https://community.magento.com/t5/Magento-DevBlog/Migration-of-Zend-Framework-to-the-Laminas-Project/ba-p/443251) på _Magento DevBlog_.

**Så här kontrollerar du `auto-load:psr-4`-konfigurationen**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Ta en titt på er integreringsgren.

1. Öppna filen `composer.json` i en textredigerare.

1. Kontrollera avsnittet `autoload:psr-4` för Zend plugin-hanterarimplementering för styrenheternas beroende.

   ```json
    "autoload": {
       "psr-4": {
          "Magento\\Framework\\": "lib/internal/Magento/Framework/",
          "Magento\\Setup\\": "setup/src/Magento/Setup/",
          "Magento\\": "app/code/Magento/",
          "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
       },
   }
   ```

1. Om Zend-beroendet saknas uppdaterar du filen `composer.json`:

   - Lägg till följande rad i avsnittet `autoload:psr-4`.

     ```json
     "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
     ```

   - Uppdatera projektberoenden.

     ```bash
     composer update
     ```

   - Lägg till, implementera och push-ändra kod.

     ```bash
     git add -A
     ```

     ```bash
     git commit -m "Add Zend plugin manager implementation for controllers dependency for Laminas support"
     ```

     ```bash
     git push origin <branch-name>
     ```

   - Sammanfoga ändringarna i mellanlagringsmiljön och sedan till Produktion.

## Konfigurationsfiler

Innan du uppgraderar programmet måste du uppdatera dina projektkonfigurationsfiler så att de tar hänsyn till ändringar av standardkonfigurationsinställningarna för Adobe Commerce i molninfrastrukturen eller för programmet. De senaste standardvärdena finns i GitHub-databasen [magento-cloud](https://github.com/magento/magento-cloud).

### .magento.app.yaml

Granska alltid värdena i filen [.magento.app.yaml](../application/configure-app-yaml.md) för den installerade versionen, eftersom den styr hur programmet bygger och distribuerar till molninfrastrukturen. Följande exempel är för version 2.4.7 och använder Composer 2.7.2. Egenskapen `build: flavor:` används inte för Composer 2.x; se [Installera och använda Composer 2](../application/properties.md#installing-and-using-composer-2).

**Så här uppdaterar du `.magento.app.yaml`-filen**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Öppna och redigera filen `magento.app.yaml`.

1. Uppdatera PHP-alternativen.

   ```yaml
   type: php:8.3
   
   build:
       flavor: none
   dependencies:
       php:
           composer/composer: '2.7.2'
   ```

1. Ändra kommandona för `hooks`-egenskapen `build` och `deploy`.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           composer install
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Lägg till följande miljövariabler i slutet av filen.

   För Adobe Commerce 2.2.x till 2.3.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

   För Adobe Commerce 2.4.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

1. Spara filen. Verkställ eller skicka inga ändringar till fjärrmiljön än.

1. Fortsätt med uppgraderingsprocessen.

### composer.json

Kontrollera alltid att beroendena i filen `composer.json` är kompatibla med Adobe Commerce-versionen innan du uppgraderar.

**Så här uppdaterar du `composer.json`-filen för Adobe Commerce version 2.4.4 och senare**:

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

1. Ange uppgraderingsversionen med syntaxen [för versionsbegränsning](overview.md#cloud-metapackage).

   ```bash
   composer require "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >Du måste använda syntaxen för versionsbegränsning för att kunna uppdatera paketet `ece-tools`. Du hittar versionsbegränsningen i filen `composer.json` för den version av [programmallen](https://github.com/magento/magento-cloud/blob/master/composer.json) som du använder för uppgraderingen.

1. Uppdatera projektet.

   ```bash
   composer update
   ```

1. Granska de korrigeringar som används för närvarande:

   - Om det finns korrigeringsfiler installerade i katalogen `m2-hotfixes` [skickar du en Adobe Commerce Support-anmälan](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case) och arbetar med Adobe Commerce Support för att kontrollera vilka korrigeringsfiler som fortfarande kan användas i den nya versionen. Ta bort den eller de icke tillämpliga korrigeringarna från katalogen `m2-hotfixes`.

   - Om [kvalitetsuppdateringar] används i filen `.magento.env.yaml` kontrollerar du om de fortfarande kan användas i den nya versionen. Ta bort den eller de korrigeringar som inte är tillämpliga från `QUALITY_PATCHES`-avsnittet i `.magento.env.yaml`-filen.

   **Metod 1**: [Verifiera tillämpliga versioner i versionsinformationen för kvalitetspatchar](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **Metod 2**: [Visa tillgängliga korrigeringar och status](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **Metod 3**: [Sök efter korrigeringsfiler](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=en)


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

### Skapa filen config.php

Som nämndes i [Konfigurationshantering](#configuration-management) måste du skapa en uppdaterad `config.php`-fil efter uppgraderingen. Slutför eventuella ytterligare konfigurationsändringar via administratören i din integreringsmiljö.

**Så här skapar du en systemspecifik konfigurationsfil**:

1. Använd ett SSH-kommando från terminalen för att generera filen `/app/etc/config.php` för miljön.

   ```bash
   ssh <SSH-URL> "<Command>"
   ```

   Om du till exempel vill köra `scd-dump` på grenen `integration` i Pro:

   ```bash
   ssh <project-id-integration>@ssh.us.magentosite.cloud "php vendor/bin/ece-tools config:dump"
   ```

1. Överför filen `config.php` till dina lokala arbetsstationer med `rsync` eller `scp`. Du kan bara lägga till den här filen lokalt i grenen.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
   ```

   Detta genererar en uppdaterad `/app/etc/config.php`-fil med en modullista och konfigurationsinställningar.

>[!WARNING]
>
>Om du vill uppgradera tar du bort filen `config.php`. När den här filen har lagts till i koden ska du **inte** ta bort den. Om du måste ta bort eller redigera inställningar redigerar du filen manuellt.

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

Adobe rekommenderar starkt att du uppgraderar din produktionsmiljö _före_, inklusive de uppgraderade tilläggen i din webbplatsstartprocess.

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
