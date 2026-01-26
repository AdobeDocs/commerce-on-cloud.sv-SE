---
title: '[!DNL ECE-Tools]-paket'
description: Lär dig mer om paketet  [!DNL ECE-Tools] och hur det hjälper dig att hantera och distribuera Adobe Commerce.
exl-id: 15d762ef-bca7-480b-b719-caf131dc9180
source-git-commit: db34528be490f92cc61c609ca143c01ef3284157
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 0%

---

# ECE-verktygspaket

Paketet [!DNL ECE-Tools] är en uppsättning skript och verktyg som är utformade för att hantera och distribuera programmet [!DNL Commerce]. Paketet `ece-tools` förenklar många processer, t.ex. hantering av cron-jobb, verifiering av projektkonfiguration och användning av Adobe-korrigeringar och snabbkorrigeringar. Du kan visa och bidra till [koden med öppen källkod [!DNL ECE-Tools] på GitHub](https://github.com/magento/ece-tools).

{{ece-tools-package}}

Paketet `ece-tools` är kompatibelt med Adobe Commerce, från och med version 2.1.4, och innehåller skript och Adobe Commerce på molninfrastrukturskommandon som är utformade för att hantera koden och automatiskt skapa och distribuera dina projekt.

Följande visar tillgängliga `ece-tools`-kommandon:

```bash
php ./vendor/bin/ece-tools list
```

## Bygg och driftsätt

Paketet `ece-tools` innehåller kommandon för att utföra åtgärder för att skapa, distribuera och efterdistribuera stadier när du startar Adobe Commerce på molninfrastruktursprogrammet. Kommandot `php ./vendor/bin/ece-tools build` startar till exempel programbyggprocessen.

Som standard finns dessa `ece-tools`-kommandon i [hooks-egenskapen &#x200B;](../application/hooks-property.md) i konfigurationsfilen `.magento.app.yaml` .

## Dockningskonfigurationsgenerator

Paketet `ece-tools` innehåller ett beroende för paketet [magento/magento-cloud-docker](https://github.com/magento/magento-cloud-docker) som innehåller funktioner och konfigurationsfiler för Docker-bilder för att starta en Docker-utvecklingsmiljö för Adobe Commerce i molninfrastrukturen. Du kan också köra Cloud Docker för Commerce som ett fristående paket. Se [Dockerutveckling](../dev-tools/cloud-docker.md).

## Tjänster, flöden och variabler

Du kan använda paketet `ece-tools` om du vill visa detaljerad information om de Base64-kodade [Cloud-variabler](../environment/variables-cloud.md) som används i valfri molnmiljö. Följande kommando visar alla tjänster, flöden och variabler.

```bash
php ./vendor/bin/ece-tools env:config:show
```

Om du vill visa en viss uppsättning information använder du följande format:

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` - Visar relationsdata från miljövariabeln `MAGENTO_CLOUD_RELATIONSHIPS` som definierats i filen `services.yaml`.
- `routes` - Visar de konfigurerade vägarna för projektet med miljövariabeln `MAGENTO_CLOUD_ROUTES`.
- `variables` - Visar de konfigurerade variablerna för projektet med miljövariabeln `MAGENTO_CLOUD_VARIABLES`.

Exempelutdata för alternativet `services`:

```
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## Verifiera miljökonfiguration

Det finns en uppsättning verifieringskommandon som hjälper dig att utvärdera projektets konfiguration. I avsnittet [Smarta guider](../deploy/smart-wizards.md) i avsnittet _Optimera distribution_ finns en detaljerad beskrivning av varje guidekommando. Kommandot `wizard:ideal-state` körs automatiskt under byggfasen. Så här verifierar du det idealiska läget för ditt projekt:

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>Du måste köra kommandot `wizard:ideal-state` i fjärrmolnmiljön. Kommandot returnerar alltid felet `The configured state is not ideal` när det körs i den lokala utvecklingsmiljön.

Exempel:

```
Ideal state is configured
```

Se [Versionsinformation för verktygen](../release-notes/cloud-tools-suite.md).

## Adobe patchar och anpassade patchar

Paketet `ece-tools` innehåller ett beroende för paketet [&#x200B; magento/magento-cloud-patches](https://github.com/magento/magento-cloud-patches) som innehåller Adobe-korrigeringar och snabbkorrigeringar som förbättrar integreringen av alla Adobe Commerce-versioner med molnmiljöer och stöder snabb leverans av viktiga korrigeringar. &quot;Levererar även anpassade korrigeringsfiler som du lägger till i ditt Adobe Commerce i molninfrastrukturprojekt. Se [Tillämpa korrigeringar](../development/apply-patches.md).

