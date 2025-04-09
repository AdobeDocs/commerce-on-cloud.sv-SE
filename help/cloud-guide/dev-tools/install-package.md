---
title: Uppgradera projekt för att använda ECE-Tools
description: Lär dig hur du uppgraderar ditt Adobe Commerce i molninfrastrukturprojekt för att använda ECE-Tools-paketet och dra nytta av de senaste korrigeringarna och funktionerna.
feature: Cloud, Install
exl-id: 164c47e4-c871-41a3-b268-581d426e7a7f
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Uppgradera projektet för att använda ECE-Tools-paketet

Adobe har ersatt paketen `magento/magento-cloud-configuration` och `magento/ece-patches` till förmån för paketet `ece-tools`, vilket förenklar många molnprocesser. Om du använder ett äldre Adobe Commerce i molninfrastrukturprojekt som _inte_ innehåller `ece-tools` paketet måste du utföra en manuell _uppgradering_ av projektet en gång.

>[!WARNING]
>
>Om projektet innehåller `ece-tools` paketet kan du hoppa över följande uppgradering. Kontrollera genom att hämta versionen med hjälp av kommandot i `php vendor/bin/ece-tools -V` den lokala projektrotkatalogen[!DNL Commerce].

Den här projektuppgraderingsprocessen kräver att du uppdaterar versionsbegränsningen `magento/magento-cloud-metapackage` `composer.json` i filen i rotkatalogen. Den här begränsningen gör det möjligt att uppdatera Adobe Commerce på metapaket för molninfrastruktur, inklusive att ta bort inaktuella paket, utan att uppgradera din nuvarande Adobe Commerce-version.

{{upgrade-tip}}

## Ta bort föråldrade paket

Innan du utför en uppgradering för att använda `ece-tools` paketet bör du kontrollera `composer.lock` filen för följande föråldrade paket:

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## Uppdatera metapaketet

Varje Adobe Commerce-version kräver olika begränsningar baserat på följande:

```
>=current_version <next_version
```

- För `current_version`anger du den Adobe Commerce-version som ska installeras.
- För `next_version`anger du nästa korrigeringsversion efter det värde som anges i `current_version`.

Om du vill installera Adobe Commerce `2.3.5-p2` anger du `current_version` till `2.3.5` och `next_version` till `2.3.6`. Begränsningen `">=2.3.5 <2.3.6"` installerar det senaste tillgängliga paketet för 2.3.5.

Du kan alltid hitta den senaste metapaketbegränsningen i mallen[`magento-cloud`](https://github.com/magento/magento-cloud/blob/master/composer.json).

I följande exempel placeras en begränsning för metapaketet Adobe Commerce om molninfrastruktur för alla versioner som är större än eller lika med den aktuella versionen 2.4.8 och lägre än nästa version 2.4.9:

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
},
```

## Uppgradera projektet

För att uppgradera ditt projekt till att använda `ece-tools` paketet måste du uppdatera metapaketet och hooks-egenskaperna `.magento.app.yaml` , och utföra en Composer-uppdatering.

**Så här uppgraderar du projektet till att använda ece-tools**:

1. Uppdatera versionsbegränsningen `magento/magento-cloud-metapackage` i `composer.json` filen.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.8 <2.4.9" --no-update
   ```

1. Uppdatera metapaketet.

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. Ändra hook-kommandona i `magento.app.yaml` filen.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Sök efter och ta bort de [borttagna paketen](#remove-deprecated-packages). De borttagna paketen kan förhindra en lyckad uppgradering.

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. Du kan behöva uppdatera paketet `ece-tools`.

   ```bash
   composer update magento/ece-tools
   ```

1. Lägg till och checka in kodändringarna. I det här exemplet har följande filer uppdaterats:

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. Skicka kodändringarna till fjärrservern och sammanfoga den här grenen med grenen `integration` .

   ```bash
   git push origin <branch-name>
   ```
