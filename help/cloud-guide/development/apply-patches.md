---
title: Tillämpa patchar
description: Lär dig hur du använder korrigeringsfiler i Adobe Commerce i molninfrastrukturprojekt.
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# Tillämpa patchar

[Molnkorrigeringar för Commerce](https://github.com/magento/magento-cloud-patches) och [kvalitetskorrigeringsverktyget](https://github.com/magento/quality-patches) levererar korrigeringsfiler till det installerade Adobe Commerce-programmet.

- Cloud Patches for Commerce-paketet innehåller nödvändiga korrigeringar med viktiga korrigeringar
- Kvalitetskorrigeringar ger valfria kvalitetskorrigeringar med låg påverkan som [enskilda korrigeringar](https://experienceleague.adobe.com/docs/commerce-operations/release/planning/versioning-policy.html?lang=sv-SE#individual-patch) som inte innehåller inkompatibla ändringar bakåt

Se [Tillgängliga korrigeringar](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=sv-SE) i _Commerce Operations Tools Guide_ om du vill visa en fullständig lista över släppta korrigeringsfiler.

Båda paketen förbättrar integreringen av alla Adobe Commerce-versioner med molnmiljöer och stöder snabb leverans av viktiga, valfria och anpassade korrigeringar. Du kan använda dessa paket för att tillämpa, återställa och visa allmän information om alla enskilda korrigeringsfiler som är tillgängliga för Commerce.

>[!TIP]
>
>Du kan använda [kvalitetskorrigeringsverktyget](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=sv-SE) och molnkorrigeringar för Commerce som fristående paket för Magento Open Source- och Adobe Commerce-projekt. Vi rekommenderar att du använder verktyget Kvalitetskorrigeringar för icke-molnprojekt.

När du distribuerar ändringar i fjärrmiljön använder `ece-tools`-paketet `magento/magento-cloud-patches` och `magento/quality-patches` för att söka efter väntande korrigeringar och tillämpar dem automatiskt i följande ordning:

1. Tillämpa alla nödvändiga Commerce-korrigeringsfiler som ingår i Creative Cloud Patches for Commerce-paketet.
1. Använd de valfria Commerce-korrigeringsfilerna som finns i kvalitetskorrigeringsverktyget.
1. Använd anpassade korrigeringsfiler i katalogen `/m2-hotfixes` i alfabetisk ordning efter korrigeringsnamn.

>[!NOTE]
>
>När du uppdaterar `ece-tools`-paketet eller Cloud Patches for Commerce-paketet tillämpas de senaste nödvändiga korrigeringsfilerna nästa gång du distribuerar ditt projekt, eller så kan du distribuera dem direkt med `ece-patches apply` CLI-kommandot och omdistribuera din molnmiljö. Du kan inte hoppa över [nödvändiga korrigeringar](https://github.com/magento/magento-cloud-patches/tree/develop/patches) under distributionsprocessen.

## Förutsättningar

{{upgrade-tip}}

Verktyget för kvalitetsuppdateringar är beroende av molnkorrigeringar för Commerce och paketet `ece-tools`. Om du vill använda de senaste korrigeringarna måste du ha [den senaste versionen av ECE-Tools](../dev-tools/update-package.md) installerad. Minimiversionen av ECE-Tools är 2002.1.2.

## Visa tillgängliga korrigeringar och status

Så här visar du en lista över tillgängliga enskilda korrigeringar:

```bash
php ./vendor/bin/ece-patches status
```

Exempelsvar:

```
More detailed information about patches you can find on https://support.magento.com/
╔════════════════╤═════════════════════════════════════════════════╤══════════╤═════════════╤═════════════════════════════════╗
║ Id             │ Title                                           │ Type     │ Status      │ Details                         ║
╠════════════════╪═════════════════════════════════════════════════╪══════════╪═════════════╪═════════════════════════════════╣
║ MAGECLOUD-5069 │ FPC is getting disabled during deployments      │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-page-cache    ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5650    │ Hold deployment config after reading from file  │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5684    │ Pagination Not working - product_list_limit=all │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-elasticsearch ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-65837       │ Fix load balancer issue                         │Deprecated│ Applied     │ Recommended replacement: MC-1   ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ BUNDLE-2554    │ Set Payment info bug                            │ Required │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - amzn/amazon-pay-module       ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-1           │ Fixes issue 1                                   │ Optional │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-2           │ Fixes issue 2                                   │ Optional │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-3           │ Fixes issue 3                                   │ Optional │ Not applied │ Required patches:               ║
║                │                                                 │          │             │  - MC-2                         ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ N/A            │ ../m2-hotfixes/MDVA_custom__2.3.5_ce.patch      │ Custom   │ N/A         │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-framework     ║
╚════════════════╧═════════════════════════════════════════════════╧══════════╧═════════════╧═════════════════════════════════╝
Magento 2 Enterprise Edition, version 2.3.5.0
```

Statustabellen innehåller följande typer av information:

- **Typ**:
   - `Optional` - Alla korrigeringsfiler från kvalitetsverktyget och Cloud Patches-paketet är valfria för Adobe Commerce- och Magento Open Source-installationer. För Adobe Commerce i molninfrastruktur är alla korrigeringsfiler valfria.
   - `Required` - Alla korrigeringsfiler från Cloud Patches for Commerce-paketet krävs för molnkunder.
   - `Deprecated` - Den enskilda korrigeringen är markerad som inaktuell och vi rekommenderar att du återställer den om du har använt den. När du har återställt en borttagen korrigering visas den inte längre i statustabellen.
   - `Custom` - Alla korrigeringar från katalogen m2-hotfixes.

- **Status**:
   - `Applied` - Korrigeringen har tillämpats.
   - `Not applied` - Korrigeringen har inte tillämpats.
   - `N/A` - Det går inte att definiera status för korrigeringen på grund av konflikter.

- **Information**:
   - `Affected components` - Listan med berörda moduler.
   - `Required patches` - Listan över nödvändiga korrigeringar (beroenden).
   - `Recommended replacement` - Den korrigering som rekommenderas som ersättning för en borttagen korrigering.

## Tillämpa en korrigering i en lokal miljö

Du kan tillämpa korrigeringsfiler manuellt i en lokal miljö och testa dem innan du distribuerar dem.

**Så här använder du enskilda korrigeringsfiler i en lokal utvecklingsmiljö**:

1. Lägg till variabeln QUALITY_PATCH i filen `.magento.env.yaml` och visa de nödvändiga korrigeringarna under.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

1. Använd patcharna från projektets rot.

   ```bash
   php ./vendor/bin/ece-patches apply
   ```

   Kommandot `ece-patches apply` använder korrigeringar i följande ordning:
   - Nödvändiga patchar
   - Valfria enskilda korrigeringsfiler
   - Anpassade korrigeringar från katalogen `/m2-hotfixes`

1. Rensa cachen.

   ```bash
   php ./bin/magento cache:clean
   ```

1. Testa patcharna och gör nödvändiga ändringar i anpassade patchar.

## Tillämpa en korrigering i en fjärrmiljö

>[!WARNING]
>
>Vi rekommenderar att du testar alla korrigeringsfiler i en integrations- eller mellanlagringsmiljö innan du distribuerar dem till produktionsmiljön.

**Så här använder du korrigeringsfiler i en fjärrmiljö**:

1. Lägg till variabeln `QUALITY_PATCHES` i filen `.magento.env.yaml` och visa de nödvändiga korrigeringarna under.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

   >[!NOTE]
   >
   >När du har uppgraderat till en ny version av Adobe Commerce måste du tillämpa korrigeringarna igen om de inte ingår i den nya versionen.

1. Lägg till, bekräfta och skicka den uppdaterade `.magento.env.yaml`-filen.

   ```bash
   git add .magento.env.yaml
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

## Tillämpa en anpassad korrigering

När du distribuerar använder ECE-Tools alla Adobe-korrigeringar och eventuella anpassade korrigeringar som du lägger till i katalogen `/m2-hotfixes` i projektroten.

>[!NOTE]
>
>Alla namn på korrigeringsfiler måste avslutas med tillägget `.patch`.

**Så här använder och testar du en anpassad korrigering i en molnmiljö**:

1. Skapa en katalog med namnet `m2-hotfixes` om den inte finns i projektroten

   ```bash
   mkdir m2-hotfixes
   ```

1. Kopiera korrigeringsfilen till katalogen `/m2-hotfixes`.

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Testa alla korrigeringsfiler i en förproduktionsmiljö. För Adobe Commerce i molninfrastruktur kan du skapa grenar med CLI-kommandot `magento-cloud environment:branch <branch-name>`.

## Återställa en anpassad korrigering

Så här återställer eller avinstallerar du en tidigare tillämpad anpassad korrigering:

1. Ta bort korrigeringsfilen från katalogen `/m2-hotfixes`.

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Revert patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Testa i förproduktionsmiljö. För Adobe Commerce i molninfrastruktur kan du skapa grenar med CLI-kommandot `magento-cloud environment:branch <branch-name>`.

## Tillämpa korrigeringar på ett icke-molnprojekt

Använd [kvalitetsverktyget ](https://github.com/magento/quality-patches) för projekt i Magento Open Source och Adobe Commerce.

## Återställa en korrigering i en lokal miljö

Du kan återställa alla tidigare tillämpade korrigeringsfiler i en lokal utvecklingsmiljö med hjälp av CLI:n för `ece-patches`.

Så här återställer du alla tillämpade patchar:

```bash
php ./vendor/bin/ece-patches revert
```

Med det här kommandot återställs alla patchar i följande ordning:

- Återställer alla tillämpade anpassade korrigeringar från katalogen /m2-hotfixes.
- Återställer alla tillämpade valfria enskilda korrigeringsfiler.
- Återställer alla nödvändiga korrigeringar som används.

## Loggning

Verktyget Kvalitetsuppdateringar loggar alla åtgärder till filen `<Project_root>/var/log/patch.log`.
