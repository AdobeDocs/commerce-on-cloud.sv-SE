---
title: Versionsinformation om Cloud Tools Suite
description: Läs om de senaste förbättringarna av Cloud Tools Suite för Adobe Commerce.
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
source-git-commit: 2375b3a2a368f4db5157d3995cc48fd5f4418972
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 3%

---

# Versionsinformation om Commerce Cloud Tools Suite

Den här versionsinformationen innehåller information om de senaste förbättringarna av Creative Suite for Commerce-paketen som är utformade för att distribuera och hantera Adobe Commerce-installationer och uppgraderingar på molnplattformen.

| Versionsinformation | Version | Beskrivning | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [ece-tools-paket](ece-tools-package.md) | 2002.2.10 | En uppsättning skript och verktyg som utformats för att hantera och driftsätta Cloud-projekt | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.10) |
| [Molnkorrigeringar för Commerce](cloud-patches.md) | 1.1.13 | En uppsättning patchar som förbättrar integreringen av alla Adobe Commerce-versioner med molnmiljöer. Det här paketet innehåller Adobe Commerce-korrigeringar och tillgängliga snabbkorrigeringar som tillämpas när du använder `ece-tools` för att distribuera | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.13) |
| [Cloud Docker för Commerce](cloud-docker.md) | 1.4.7 | Funktions- och konfigurationsfiler för Docker-bilder som ska distribuera Adobe Commerce till en lokal molnmiljö | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.7) |
| [Cloud-komponenter för Commerce](cloud-components.md) | 1.1.4 | Utökad Adobe Commerce-funktionalitet för webbplatser i molninfrastrukturen | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.4) |

När du uppdaterar till ECE-Tools 2002.1.0 eller senare uppdateras du automatiskt till de senaste versionerna av de andra paketen, som är beroenden för paketet `ece-tools`. En lista över beroenden finns i [Cloud-metapaketet](../development/overview.md#cloud-metapackage).
