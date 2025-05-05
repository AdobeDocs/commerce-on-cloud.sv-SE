---
title: Variables, egenskap
description: Använd variabelegenskapen för att anpassa butikskonfigurationsalternativen för programmet  [!DNL Commerce] .
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Variables, egenskap

Du kan använda programbaserade miljövariabler för att anpassa butikskonfigurationer. Dessa variabler använder en specifik syntax. Se [Åsidosätt konfigurationsinställningar](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html?lang=sv-SE) i _Konfigurationshandboken_.

Följande miljövariabler som ingår i filen `.magento.app.yaml` krävs för specifika versioner av programmet [!DNL Commerce].

Krävs för Adobe Commerce 2.2.x till 2.3.x:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

För Adobe Commerce 2.4.x anger du följande variabler:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```
