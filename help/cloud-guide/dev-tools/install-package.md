---
title: Uppgradera projekt till ECE-Tools
description: Lär dig hur du uppgraderar din Adobe Commerce i molninfrastrukturprojekt så att du kan använda ECE-verktygspaketet och dra nytta av de senaste fixarna och funktionerna.
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Uppgradera projekt till ECE-verktygspaket

Adobe har ersatt paketen `magento/magento-cloud-configuration` och `magento/ece-patches` till förmån för paketet `ece-tools`, vilket förenklar många molnprocesser. Om du använder ett äldre Adobe Commerce i ett molninfrastrukturprojekt som _inte_ innehåller paketet `ece-tools` måste du utföra en manuell _uppgradering_-process en gång i projektet.

>[!WARNING]
>
>Om ditt projekt innehåller paketet `ece-tools` kan du hoppa över följande uppgradering. Verifiera genom att hämta versionen [!DNL Commerce] med kommandot `php vendor/bin/ece-tools -V` i den lokala projektrotkatalogen.

Den här projektuppgraderingsprocessen kräver att du uppdaterar versionsbegränsningen `magento/magento-cloud-metapackage` i filen `composer.json` i rotkatalogen. Begränsningen gör att Adobe Commerce kan uppdateras i molninfrastruktursmetapaket, inklusive borttagning av borttagna paket, utan att du behöver uppgradera din nuvarande Adobe Commerce-version.

{{upgrade-tip}}

## Ta bort inaktuella paket

Innan du utför en uppgradering för att använda paketet `ece-tools` bör du kontrollera filen `composer.lock` för följande borttagna paket:

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## Uppdatera metapaketet

För varje Adobe Commerce-version krävs en annan begränsning som bygger på följande:

```
>=current_version <next_version
```

- För `current_version` anger du den Adobe Commerce-version som ska installeras.
- För `next_version` anger du nästa korrigeringsversion efter det värde som anges i `current_version`.

Om du vill installera Adobe Commerce `2.3.5-p2` anger du `current_version` till `2.3.5` och `next_version` till `2.3.6`. Begränsningen `">=2.3.5 <2.3.6"` installerar det senaste tillgängliga paketet för 2.3.5.

Du kan alltid hitta den senaste metapaketbegränsningen i mallen [`magento-cloud`](https://github.com/magento/magento-cloud/blob/master/composer.json).

I följande exempel begränsas Adobe Commerce i molninfrastrukturmetapaketet till alla versioner som är större än eller lika med den aktuella versionen, 2.4.7, och lägre än nästa version, 2.4.8:

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
},
```

## Uppgradera projektet

Om du vill uppgradera ditt projekt till att använda paketet `ece-tools` måste du uppdatera metapaketet och egenskaperna för `.magento.app.yaml`-krokarna och utföra en Composer-uppdatering.

**Så här uppgraderar du projekt till att använda extraverktyg**:

1. Uppdatera versionsbegränsningen `magento/magento-cloud-metapackage` i filen `composer.json`.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.7 <2.4.8" --no-update
   ```

1. Uppdatera metapaketet.

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. Ändra krokkommandona i filen `magento.app.yaml`.

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

1. Lägg till och verkställ kodändringarna. I det här exemplet uppdaterades följande filer:

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. Skicka dina kodändringar till fjärrservern och sammanfoga den här grenen med grenen `integration`.

   ```bash
   git push origin <branch-name>
   ```
