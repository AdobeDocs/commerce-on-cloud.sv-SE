---
title: Utvecklingsöversikt
description: Förbered dig för lokal utveckling med ett Adobe Commerce-projekt för molninfrastruktur.
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 14fb0b41-1c3a-4abc-8726-cea16ab00ba8
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Utvecklingsöversikt

Adobe Commerce i molninfrastrukturens fjärrmiljöer är **skrivskyddade**, inklusive alla Starter-miljöer och alla Pro-integrerings-, mellanlagrings- och produktionsmiljöer. I en lokal utvecklingsmiljö kan du skriva och testa kod innan du skickar den till en integreringsmiljö för ytterligare testning och driftsättning till Förproduktion och Produktion.

Innan du förbereder din lokala arbetsyta bör du kontrollera att du har dina [inloggningsuppgifter](../../get-started/prepare-workspace.md). Lokal utveckling kräver installation av PHP och Composer om du inte väljer att använda [Cloud Docker för Commerce](#docker-environment).

## Obligatoriska paket

Adobe Commerce i molninfrastruktur använder Composer för att hantera beroenden och uppgraderingar för projekt. För lokal utveckling måste du installera de PHP- och Composer-versioner som är kompatibla med ditt Cloud-projekt. Om du till exempel använder molnmallen [!DNL Commerce] 2.4.8 ser du att [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.8/.magento.app.yaml)-konfigurationsfilen använder **PHP 8.4** och **Composer 2.8.4**.

Composer installerar nödvändiga bibliotek och beroenden för ditt projekt i katalogen `vendor`. Följande Composer-filer som krävs finns i projektets rotkatalog:

- `composer.json` - Använd filen `composer.json` för att hantera produktinstallationer och uppgraderingar.
- `composer.lock` - Filen `composer.lock` lagrar en uppsättning exakta versionsberoenden som uppfyller versionsbegränsningarna för varje paket i projektets beroendeträd.

**Vanliga kommandon:**

| Kommando | Beskrivning |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | Uppdateringar av de senaste versionerna av beroenden som återspeglas i filen `composer.json`. Filen `composer.lock` uppdateras. |
| `composer install` | Läser filen `composer.lock` för att hämta beroenden. Det är en god vana att behålla en uppdaterad kopia av `composer.lock` i projektdatabasen. |

{style="table-layout:auto"}

När du har lagt till, implementerat och skickat den uppdaterade koden körs kommandot `composer install` automatiskt under [byggfasen](../deploy/process.md#build-phase-build-phase) i distributionsprocessen.

### Cloud-metapaket

Adobe Commerce i molninfrastrukturen använder ett metapaket som kräver `magento/product-enterprise-edition`. Använd följande begränsningssyntax för att hämta de senaste uppdateringarna för den senaste versionen av Commerce:

```text
>=current_version <next_version
```

Om du till exempel vill använda den senaste versionen av Adobe Commerce 2.4.9 anger du `2.4.8` som den&quot;aktuella&quot; versionen och `2.4.9` som&quot;nästa&quot; version i filen `composer.json`:

```text
"magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
```

Huvudpaketen i det här metapaketet är följande:

- **vendor/magento/ece-tools** - Paketet `ece-tools` är kompatibelt med Adobe Commerce version 2.1.4 och senare för att ge dig en mängd funktioner som du kan använda för att hantera ditt Adobe Commerce i molninfrastrukturprojekt. Det innehåller skript och Adobe Commerce på molninfrastrukturskommandon som är utformade för att hantera koden och automatiskt bygga och driftsätta dina projekt. Se översikten över paketet [`ece-tools`](../dev-tools/package-overview.md).
- **vendor/magento/product-enterprise-edition** - Det här metapaketet kräver programkomponenter, bland annat moduler, ramverk, teman.
- **vendor/fastly2/magento2** - Den här modulen hanterar snabbkorrigeringsnumret och tjänster för Pro Staging- och Production- och Starter Production-miljöerna. Se [Snabba tjänster](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2).
- **vendor/magento/module-paypal-on-boarding** - Den här modulen erbjuder PayPal-betalningsgateway-utcheckning genom att ansluta till ditt PayPal-handelskonto. Se [PayPal On-Boarding-verktyget](../store/paypal.md).
- **vendor/aem/rum** - Den här modulen hanterar datainsamlingsverktyget [Operational Telemetry](../monitor/operational-telemetry.md).

>[!TIP]
>
>En lista över beroenden och tredjepartslicenser finns i [Cloud-paket för Adobe Commerce](/help/cloud-guide/release-notes/cloud-packages.md) i _Versionsinformation för Commerce_.

## Dockningsmiljö

Du kan använda verktyget Cloud Docker för Commerce för att emulera Adobe Commerce i molninfrastruktursproduktions- och utvecklingsmiljöer för lokal utveckling. För Cloud Docker för Commerce krävs inte att PHP och Composer installeras lokalt.

- [Lokal utveckling med Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/) på Adobe Developer webbplats
- [Dockningsarkitektur och vanliga kommandon](../dev-tools/cloud-docker.md)
- [Versionsinformation om Cloud Docker](../release-notes/cloud-docker.md)

>[!TIP]
>
>Mer information om hur du använder Git-baserade värdtjänster med Adobe Commerce för molninfrastruktur finns i [Integrationer](../integrations/overview.md).
