---
title: Miljövariabler
description: Se en lista över miljövariabler som är specifika för Adobe Commerce om molninfrastruktur.
feature: Cloud, Build, Configuration, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Miljövariabler

Med Adobe Commerce i molninfrastruktur kan du tilldela miljövariabler för att åsidosätta konfigurationsalternativen. Paketet `ece-tools` ställer in värden i filen `env.php` baserat på värden från [Cloud-variabler](variables-cloud.md), variabler som angetts i [!DNL Cloud Console] och konfigurationsfilen `.magento.env.yaml`.

Miljövariablerna i filen `.magento.env.yaml` anpassar molnmiljön genom att åsidosätta din befintliga Commerce-konfiguration. Om standardvärdet är `Not Set` utför `ece-tools`-paketet **NO**-åtgärden och använder standardvärdet [!DNL Commerce] eller värdet från `MAGENTO_CLOUD_RELATIONSHIPS`-konfigurationen. Om standardvärdet anges används `ece-tools`-paketet för att ange standardvärdet.

Typerna av miljövariabler är bland annat:

- [ADMIN](variables-admin.md) - variabler åsidosätter projekt-ADMIN-variabler
- [MAGENTO_CLOUD](variables-cloud.md) - variabler som är specifika för molninfrastrukturen
- Variabler som används i filen `.magento.env.yaml`:
   - [Global](variables-global.md) - variabler påverkar stadier för skapande, distribution och efterdistribution
   - [Build](variables-build.md) - variabelkontrollbyggåtgärder
   - [Distribuera](variables-deploy.md) - variabelstyrningsdistributionsåtgärder
   - [Efter distribution](variables-post-deploy.md) - variabelkontrollåtgärder efter distribution

Variabler är _hierarkiska_, vilket innebär att om en variabel inte åsidosätts ärvs den från den överordnade miljön.

Du kan ange [ADMIN-variabler](variables-admin.md) från [!DNL Cloud Console] eller med Adobe Commerce CLI. Du kan hantera andra miljövariabler från filen [`.magento.env.yaml`](configure-env-yaml.md) för att hantera bygg- och distributionsåtgärder i alla dina miljöer, inklusive Pro Staging och Production, utan att behöva använda en supportanmälan.

>[!TIP]
>
>YAML-filer är skiftlägeskänsliga och tillåter inte tabbar. Var noga med att använda konsekvent indrag i hela `.magento.env.yaml`-filen, annars kanske inte konfigurationen fungerar som förväntat. Exemplen i den här dokumentationen och i exempelfilen använder _indrag med två blanksteg_. Använd kommandot [ece-tools validate](configure-env-yaml.md#validate-configuration-file) för att kontrollera konfigurationen.
