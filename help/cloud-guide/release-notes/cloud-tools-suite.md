---
title: Versionsinformation om Cloud Tools Suite
description: Läs om de senaste förbättringarna av Cloud Tools Suite för Adobe Commerce.
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
source-git-commit: b90959335c91dd0631d270ebb522524cf1db6ff0
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Versionsinformation om Commerce Cloud Tools Suite

Den här versionsinformationen innehåller information om de senaste förbättringarna av Creative Suite for Commerce-paketen som är utformade för att distribuera och hantera Adobe Commerce-installationer och uppgraderingar på molnplattformen.

| Versionsinformation | Version | Beskrivning | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [`ece-tools`-paket](ece-tools-package.md) | 2002.2.7 | En uppsättning skript och verktyg som utformats för att hantera och driftsätta Cloud-projekt | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.7) |
| [Molnkorrigeringar för Commerce](cloud-patches.md) | 1.1.10 | En uppsättning patchar som förbättrar integreringen av alla Adobe Commerce-versioner med molnmiljöer. Det här paketet innehåller Adobe Commerce-korrigeringar och tillgängliga snabbkorrigeringar som tillämpas när du använder `ece-tools` för att distribuera | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.10) |
| [Cloud Docker för Commerce](cloud-docker.md) | 1.4.4 | Funktions- och konfigurationsfiler för Docker-bilder som ska distribuera Adobe Commerce till en lokal molnmiljö | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.4) |
| [Cloud-komponenter för Commerce](cloud-components.md) | 1.1.3 | Utökad Adobe Commerce-funktionalitet för webbplatser i molninfrastrukturen | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.3) |

När du uppdaterar till ECE-Tools 2002.1.0 eller senare uppdateras du automatiskt till de senaste versionerna av de andra paketen, som är beroenden för paketet `ece-tools`. En lista över beroenden finns i [Cloud-metapaketet](../development/overview.md#cloud-metapackage).

Den nya versionen av `ece-tools` (2002.2.0) är endast tillgänglig med PHP-version 8.1 och senare (8.2, 8.3). Äldre PHP-versioner har tagits bort (7.4, 7.3, 7.2). Du kan använda tidigare versioner av `ece-tools` med tidigare PHP-versioner.
