---
title: Hantera tillägg
description: Lär dig hur du installerar och hanterar tillägg i Adobe Commerce i molninfrastruktur.
feature: Cloud, Extensions, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Hantera tillägg

Du kan utöka dina Adobe Commerce-programfunktioner genom att lägga till ett tillägg från [Commerce Marketplace](https://marketplace.magento.com). Du kan till exempel lägga till ett tema för att ändra utseendet på din butik eller lägga till ett språkpaket för att lokalisera din butik och administratör.

>[!NOTE]
>
>För att undvika installationsproblem måste alla Marketplace-inköp slutföras med samma konto (MAGEID) som äger molnprojektet.

## Kompositörens namn för ett tillägg

I det här avsnittet beskrivs hur du hämtar Composer-namnet och versionen av ett tillägg från Commerce Marketplace, men du hittar namnet och versionen på _any_-modulen i modulens Composer-fil. Öppna filen `composer.json` i en textredigerare och notera värdena `"name"` och `"version"`.

**Så här hämtar du dispositionsnamnet för en modul från Commerce Marketplace**:

1. Logga in på [Commerce Marketplace](https://marketplace.magento.com) med det användarnamn och lösenord som du använde för att köpa komponenten.

1. Klicka på ditt användarnamn i det övre högra hörnet och välj **Min profil**.

   ![Gå till ditt Marketplace-konto](../../assets/marketplace/my-profile.png)

1. Klicka på **Mina köp** på sidan _Mitt konto_.

   ![Marketplace, inköpshistorik](../../assets/marketplace/my-purchases.png)

1. På sidan _Mina inköp_ väljer du en modul som du har köpt och klickar på **Teknisk information**.

1. Klicka på **Kopiera** om du vill kopiera [!UICONTROL Component name] till Urklipp.

1. Öppna en textredigerare och klistra in komponentnamnet och lägg till ett kolontecken (`:`).

1. I **teknisk information** klickar du på **Kopiera** för att kopiera [!UICONTROL Component version] till Urklipp.

1. Lägg till versionsnumret i komponentnamnet efter kolonet i textredigeraren. Exempel:

   ```text
   extension-name/magento2:1.0.1
   ```

## Installera ett tillägg

Adobe rekommenderar att du arbetar i en utvecklingsgren när du lägger till ett tillägg till implementeringen. När du installerar ett tillägg infogas tilläggets namn (`<VendorName>_<ComponentName>`) automatiskt i filen [`app/etc/config.php`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html?lang=sv-SE). Du behöver inte redigera filen direkt.

**Så här installerar du ett tillägg**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Skapa eller checka ut en utvecklingsgren. Se [förgreningar](../development/cli-branches.md).

1. Använd Composer-namnet och -versionen för att lägga till tillägget i `require`-delen av filen `composer.json`.

   ```bash
   composer require <extension-name>:<version> --no-update
   ```

1. Uppdatera projektberoenden.

   ```bash
   composer update
   ```

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install <extension-name>"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!WARNING]
   >
   >När du installerar ett tillägg måste du inkludera filen `composer.lock` när du skickar kodändringar till fjärrmiljön. Kommandot `composer install` läser filen `composer.lock` för att aktivera definierade beroenden i fjärrmiljön.

1. När bygget och distributionen är klar loggar du in på fjärrmiljön med en SSH och kontrollerar att tillägget är installerat.

   ```bash
   bin/magento module:status <extension-name>
   ```

   Ett tilläggsnamn har formatet `<VendorName>_<ComponentName>`.

   Exempelsvar:

   ```
   Module is enabled
   ```

   Om du råkar ut för distributionsfel läser du [distributionsfel för tillägg](../deploy/recover-failed-deployment.md).

## Hantera tillägg

När du lägger till ett tillägg med Composer aktiveras tillägget automatiskt av distributionsprocessen. Om du redan har installerat tillägget kan du aktivera eller inaktivera det med CLI. Använd formatet `<VendorName>_<ComponentName>` när du hanterar tillägg.

Aktivera eller inaktivera aldrig ett tillägg när du är inloggad i fjärrmiljöer.

**Så här aktiverar eller inaktiverar du ett tillägg**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Aktivera eller inaktivera en modul. Kommandot `module` uppdaterar filen `config.php` med den begärda statusen för modulen.

   >Aktivera en modul.

   ```bash
   bin/magento module:enable <module-name>
   ```

   >Inaktivera en modul.

   ```bash
   bin/magento module:disable <module-name>
   ```

1. Om du har aktiverat en modul använder du `ece-tools` för att uppdatera konfigurationen.

   ```bash
   ./vendor/bin/ece-tools module:refresh
   ```

1. Verifiera statusen för en modul.

   ```bash
   bin/magento module:status <module-name>
   ```

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Disable <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

## Uppgradera ett tillägg

Innan du fortsätter behöver du namnet och versionen för dispositionen. Bekräfta också att tillägget är kompatibelt med ditt projekt och Adobe Commerce-versionen. [Kontrollera den PHP-version](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=sv-SE) som krävs innan du börjar.

**Så här uppdaterar du ett tillägg**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Skapa eller checka ut en utvecklingsgren. Se [förgreningar](../development/cli-branches.md).

1. Öppna filen `composer.json` i en textredigerare.

1. Leta reda på tillägget och uppdatera versionen.

1. Spara ändringarna och avsluta textredigeraren.

1. Uppdatera projektberoenden.

   ```bash
   composer update
   ```

1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

Om du råkar ut för fel kan du läsa [Återställa från komponentfel](../deploy/recover-failed-deployment.md). Mer information om hur du använder tillägg med Adobe Commerce finns i [Tillägg](https://experienceleague.adobe.com/docs/commerce-admin/start/resources/extensions.html?lang=sv-SE) i _Admin Guide_.
