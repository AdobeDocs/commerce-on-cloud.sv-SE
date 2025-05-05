---
title: Egenskaper
description: Använd egenskapslistan som referens när du konfigurerar programmet  [!DNL Commerce]  för att skapa och distribuera till molninfrastrukturen.
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
exl-id: 32bd1f64-43d6-48a3-84b7-bea22f125bb0
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Egenskaper för programkonfiguration

Filen `.magento.app.yaml` använder egenskaper för att hantera miljöstöd för programmet [!DNL Commerce].

| Namn | Beskrivning | Standard | Obligatoriskt |
| ------ | --------------------------------- | ------- | -------- |
| [`access`](#access) | Anpassa användarroller | — | Nej |
| [`crons`](crons-property.md) | Uppdatera specifikationer och schemalägg cron-jobb | — | Nej |
| [`dependencies`](#dependencies) | Aktivera ytterligare beroenden | `php:composer/composer: '2.2.4'` | Nej |
| [`disk`](#disk) | Definiera den beständiga diskstorleken | `5120` | Ja |
| [`firewall`](firewall-property.md) | (Endast Starter) Kontrollera utgående trafik | — | Nej |
| [`hooks`](hooks-property.md) | Skräddarsy kommandon för faserna för bygge, driftsättning och efterdriftsättning | — | Nej |
| [`mounts`](#mounts) | Ange banor | Banor:<ul><li>`"var": "shared:files/var"`</li><li>`"app/etc": "shared:files/etc"`</li><li>`"pub/media": "shared:files/media"`</li><li>`"pub/static": "shared:files/static"`</li></ul> | Nej |
| [`name`](#name) | Definiera programnamnet | `mymagento` | Ja |
| [`relationships`](#relationships) | Karttjänster | Tjänster:<ul><li>`database: "mysql:mysql"`</li><li>`redis: "redis:redis"`</li><li>`opensearch: "opensearch:opensearch"`</li></ul> | Nej |
| [`runtime`](#runtime) | Körningsegenskapen innehåller tillägg som krävs av programmet [!DNL Commerce]. | Tillägg:<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | Ja |
| [`type`](#type-and-build) | Ange basbehållarbilden | `php:8.3` | Ja |
| [`variables`](variables-property.md) | Använda en miljövariabel för en viss Commerce-version | — | Nej |
| [`web`](web-property.md) | Hantera externa begäranden | — | Ja |
| [`workers`](workers-property.md) | Hantera externa begäranden | — | Ja, om inte egenskapen web används |

{style="table-layout:auto"}

## `name`

Egenskapen `name` innehåller det programnamn som används i filen [`routes.yaml`](../routes/routes-yaml.md) för att definiera HTTP-uppströmmen (som standard `mymagento:http`). Om värdet för `name` till exempel är `app` måste du använda `app:http` i det överordnade fältet.

>[!WARNING]
>
>Ändra inte namnet på programmet efter att det har distribuerats. Detta leder till dataförlust.

## `type` och `build`

Egenskaperna `type` och `build` innehåller information om basbehållaravbildningen för att skapa och köra projektet.

Det `type`-språk som stöds är PHP. Ange PHP-versionen enligt följande:

```yaml
type: php:<version>
```

Egenskapen `build` avgör vad som händer som standard när projektet skapas. `flavor` anger en standarduppsättning med byggåtgärder som ska köras. I följande exempel visas standardkonfigurationen för `type` och `build` från `magento-cloud/.magento.app.yaml`:

```yaml
# The toolstack used to build the application.
type: php:8.4
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.7.2'
```

### Installera och använda Composer 2

Egenskapen `build: flavor:` används inte för Composer 2.x och du måste därför installera Composer manuellt under byggfasen. Om du vill installera och använda Composer 2.x i dina Starter- och Pro-projekt måste du göra tre ändringar i din `.magento.app.yaml`-konfiguration:

1. Ta bort `composer` som `build: flavor:` och lägg till `none`. Den här ändringen förhindrar att Creative Cloud använder standardversionen av Composer 1.x för att köra byggåtgärder.
1. Lägg till `composer/composer: '^2.0'` som ett `php`-beroende för installation av Composer 2.x.
1. Lägg till byggaktiviteterna `composer` i en `build`-krok för att köra byggaktiviteterna med Composer 2.x.

Använd följande konfigurationsfragment i din egen `.magento.app.yaml`-konfiguration:

```yaml
# 1. Change flavor to none.
build:
    flavor: none

# 2. Add Composer ^2.0 as a php dependency.
dependencies:
    php:
        composer/composer: '^2.0'

# 3. Add a build hook to run the build tasks using Composer 2.x.
hooks:
    build: |
        set -e
        composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
```

Mer information om Composer finns i [Obligatoriska paket](../development/overview.md#required-packages).

## `dependencies`

Ange beroenden som programmet kan behöva under byggprocessen.

Adobe Commerce stöder beroenden av följande språk:

- PHP
- Ruby
- Node.js

Dessa beroenden är oberoende av de eventuella beroendena för ditt program och är tillgängliga i `PATH`, under byggprocessen och i körningsmiljön för ditt program.

Du kan ange dessa beroenden enligt följande:

```yaml
ruby:
   sass: "~3.4"
nodejs:
   grunt-cli: "~0.3"
```

## `runtime`

Används för att ändra PHP-konfigurationen vid körning, till exempel för att aktivera tillägg. Följande tillägg krävs:

```yaml
runtime:
    extensions:
        - xsl
        - newrelic
        - sodium
```

Mer information om hur du aktiverar tillägg finns i [PHP-inställningar](php-settings.md).

## `disk`

Definierar programmets beständiga diskstorlek i MB.

```yaml
disk: 5120
```

Den minsta rekommenderade diskstorleken är 256 MB. Om felet `UserError: Error building the project: Disk size may not be smaller than 128MB` visas ökar du storleken till 256 MB.

>[!NOTE]
>
>För Pro Staging- och Production-miljöer måste du [skicka in en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) för att uppdatera konfigurationen `mounts` och `disk` för ditt program. När du skickar biljetten anger du de konfigurationsändringar som krävs och inkluderar en uppdaterad version av din `.magento.app.yaml`-fil.
>
>Det går inte att tillfälligt öka diskutrymmet i Förproduktion eller Produktion. Den här processen går inte att ångra.

## `relationships`

Definierar tjänstmappningen i programmet.

Relationen `name` är tillgänglig för programmet i miljövariabeln `MAGENTO_CLOUD_RELATIONSHIPS`. Relationen `<service-name>:<endpoint-name>` mappas till de namn- och typvärden som definierats i filen `.magento/services.yaml`.

```yaml
relationships:
    <name>: "<service-name>:<endpoint-name>"
```

Följande är ett exempel på standardrelationerna:

```yaml
relationships:
    database: "mysql:mysql"
    redis: "redis:redis"
    opensearch: "opensearch:opensearch"
    rabbitmq: "rabbitmq:rabbitmq"
```

En fullständig lista över de tjänstetyper och slutpunkter som stöds finns i [Tjänster](../services/services-yaml.md).

## `mounts`

Ett objekt vars nycklar är sökvägar i förhållande till programmets rot. Monteringen är ett skrivbart område på disken för filer. Följande är en standardlista över mängder som har konfigurerats i filen `magento.app.yaml` med syntaxen `volume_id[/subpath]`:

```yaml
 # The mounts that will be performed when the package is deployed.
mounts:
    "var": "shared:files/var"
    "app/etc": "shared:files/etc"
    "pub/media": "shared:files/media"
    "pub/static": "shared:files/static"
```

Formatet för att lägga till din montering i den här listan är följande:

```bash
"/public/sites/default/files": "shared:files/files"
```

- `shared` - Delar en volym mellan program i en miljö.
- `disk` - Definierar storleken som är tillgänglig för den delade volymen.

>[!NOTE]
>
>För Pro Staging- och Production-miljöer måste du [skicka in en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) för att uppdatera konfigurationen `mounts` och `disk` för ditt program. När du skickar biljetten anger du de konfigurationsändringar som krävs och inkluderar en uppdaterad version av din `.magento.app.yaml`-fil.

Du kan göra monteringen webbtillgänglig genom att lägga till den i [`web`](web-property.md)-blocket med platser.

>[!WARNING]
>
>När platsen har data ska du inte ändra `subpath`-delen av monteringsnamnet. Det här värdet är den unika identifieraren för området `files`. Om du ändrar det här namnet förlorar du alla webbplatsdata som lagras på den gamla platsen.

## `access`

Egenskapen `access` anger en lägsta användarrollnivå som tillåter SSH-åtkomst till miljöerna. De tillgängliga användarrollerna är:

- `admin` - Kan ändra inställningar och köra åtgärder i miljön. Har behörighet för _medverkande_ och _visningsprogram_.
- `contributor` - Kan överföra kod till den här miljön och grenar från miljön. Har _visningsprogrambehörighet_.
- `viewer` - Kan endast visa miljön.

Standardanvändarrollen är `contributor`, vilket begränsar SSH-åtkomsten från användare med endast _visningsprogrambehörighet_. Du kan ändra användarrollen till `viewer` för att tillåta SSH-åtkomst för användare med endast _visningsprogrambehörighet_:

```yaml
access:
    ssh: viewer
```
