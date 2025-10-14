---
title: Uppdatera ECE-verktygspaketet
description: Lär dig hur du uppgraderar ECE-verktygspaketet för att dra nytta av de senaste korrigeringarna och funktionerna som används i Adobe Commerce för molninfrastruktur.
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Uppdatera ECE-verktygspaketet

En uppdatering av `ece-tools`-paketet uppdaterar även de andra [&#x200B; Cloud Tools Suite för Commerce-paketen](../release-notes/cloud-tools-suite.md), som är beroenden för `ece-tools`. Därför måste du använda en version av Adobe Commerce i molninfrastrukturen som stöder paketet `ece-tools`.

{{ece-tools-package}}

**Förutsättningar**:

- Innan du uppdaterar `ece-tools` bör du läsa versionsinformationen för [Cloud Tools Suite for Commerce](../release-notes/cloud-tools-suite.md).
- Om du uppdaterar från `ece-tools` 2002.0.22 eller tidigare till 2002.1.0 kan du granska [Inkompatibla ändringar bakåt](../release-notes/backward-incompatible-changes.md) och göra nödvändiga ändringar i ditt Adobe Commerce-infrastrukturprojekt i molnet.
- Granska [Uppgraderingar och korrigeringar](../development/commerce-version.md#upgrade-from-older-versions) för att avgöra vilka ECE-verktyg-versioner som är kompatibla med ditt Adobe Commerce i molninfrastruktursprojekt.

{{upgrade-tip}}

**Så här uppdaterar du `ece-tools` package**:

1. Utför en uppdatering med Composer på din lokala arbetsstation.

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >Om du inte kan uppdatera efter `ece-tools` version 2002.0.8, se [Uppgradera projekt för att använda ECE-verktygspaket](install-package.md).

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Efter testvalideringen sammanfogar du den här grenen med integreringsgrenen.
