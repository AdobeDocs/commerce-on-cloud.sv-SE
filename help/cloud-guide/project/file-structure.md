---
title: Projektstruktur
description: Läs om filstrukturen och projektmallarna för Adobe Commerce om molninfrastrukturen.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Projektstruktur

Ett Adobe Commerce-projekt för molninfrastruktur innehåller viktiga filer för autentiseringsuppgifter och programkonfiguration. Dessa filer är tillgängliga i som mallar enligt Adobe Commerce-versionen. Se molnmallarna som baseras på Adobe Commerce-versionen i [`magento/magento-cloud` GitHub-databasen ](https://github.com/magento/magento-cloud).

I följande tabell beskrivs filerna som ingår i ett molnprojekt:

| Fil | Beskrivning |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | Konfigurationsfil som omdirigerar `www` till den överordnade domänen och `php`-programmet för HTTP. Se [Konfigurera vägar](../routes/routes-yaml.md). |
| `/.magento/services.yaml` | En konfigurationsfil som definierar en MySQL-instans (MariaDB), Redis, OpenSearch eller Elasticsearch. Se [Konfigurera tjänster](../services/services-yaml.md). |
| `/app` | Mappen `code` används för anpassade moduler. Mappen `design` används för [anpassade teman](../store/custom-theme.md). Mappen `etc` innehåller konfigurationsfiler för programmet. |
| `/m2-hotfixes` | Används för anpassade patchar. |
| `/update` | En tjänstmapp som används av supportmodulen. |
| `.gitignore` | Ange vilka filer och kataloger som ska ignoreras. Se [`.gitignore` referens ](#ignoring-files). |
| `.magento.app.yaml` | En konfigurationsfil som definierar egenskaperna för att skapa programmet. Se [Konfigurera program](../application/configure-app-yaml.md). |
| `.magento.env.yaml` | Konfigurationsfil för byggnings-, distributions- och postdistributionsfaserna. Paketet `ece-tools` innehåller ett exempel på filen. Se [Konfigurera miljöer](../environment/configure-env-yaml.md). |
| `composer.json` | Hämtar Adobe Commerce och konfigurationsskripten för att förbereda programmet. Se [Obligatoriska paket](../development/overview.md#required-packages). |
| `composer.lock` | Lagrar versionsberoenden för varje paket. Se [Obligatoriska paket](../development/overview.md#required-packages). |
| `magento-vars.php` | Används för att definiera [flera arkiv](../store/multiple-sites.md) och webbplatser med variabler. |

{style="table-layout:auto"}

>[!NOTE]
>
>När du skickar dina lokala ändringar till fjärrservern använder distributionsskriptet de värden som definieras av konfigurationsfilerna i katalogen `.magento`, och skriptet tar sedan bort katalogen och dess innehåll. Din lokala utvecklingsmiljö påverkas inte.

## Programmets rotkatalog

Platsen för programmets rotkatalog beror på miljön.

- **Start- och Pro-integrering**: `/app`
- **Startproduktion**: `/<project-ID>`
- **Pro Staging**: `/<project-ID>_stg`
- **Pro Production**: `/<project-ID>`

### Skrivbara kataloger

Miljöerna för fjärrintegrering, mellanlagring och produktion är skrivskyddade. Följande kataloger är de *enda* skrivbara katalogerna av säkerhetsskäl:

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>I produktions- och mellanlagringsmiljöer har varje nod i trenodsklustret en `/tmp`-katalog som inte delas med de andra noderna.

## Ignorera filer

Det finns en `.gitignore`-basfil hos Adobe Commerce i molninfrastrukturens projektdatabas. Se den senaste [.gitignore-filen i magento-cloud-databasen ](https://github.com/magento/magento-cloud/blob/master/.gitignore). Om du vill lägga till en fil som finns i listan `.gitignore` kan du använda alternativet `-f` (force) när du mellanlagrar en implementering:

```bash
git add <path/filename> -f
```

## Ändra basmall

Du kan använda följande steg för att ändra strukturen i ett befintligt projekt så att den återspeglar den senaste basmallen för Adobe Commerce i molninfrastrukturen.

1. Klona projektet till en lokal arbetsstation.

1. Uppdatera filen `composer.json` med följande värden för avsnittet `extra`.

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. Lägg till filen `.gitignore` som är utformad för basmallen. Om du till exempel behöver filen `.gitignore` för mallen version 2.2.6 använder du filen [.gitignore för 2.2.6](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore) som referens.

1. Rensa Git-cachen.

   ```bash
   git rm -r --cached .
   ```

1. Lägg till och verkställ ändringar.

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
