---
title: Hooks, egenskap
description: Se exempel på hur du konfigurerar hooks-egenskapen i  [!DNL Commerce] programmets konfigurationsfil.
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Hooks, egenskap

Använd avsnittet `hooks` om du vill köra kommandon för användargränssnitt under faserna för att skapa, distribuera och efterdistribuera:

- **`build`** - Kör kommandon _innan_ paketerar programmet. Tjänster, som databasen eller Redis, är inte tillgängliga eftersom programmet ännu inte har distribuerats. Lägg till anpassade kommandon _före_ som standard `php ./vendor/bin/ece-tools` så att anpassat innehåll fortsätter till distributionsfasen.

- **`deploy`** - Kör kommandon _efter_ att du paketerat och distribuerat programmet. Du kan nu komma åt andra tjänster. Eftersom standardkommandot `php ./vendor/bin/ece-tools` kopierar katalogen `app/etc` till rätt plats, måste du lägga till anpassade kommandon _efter_ kommandot för distribution för att förhindra att anpassade kommandon misslyckas.

- **`post_deploy`** - Kör kommandon _efter_-distributionen av programmet och _efter_ börjar behållaren acceptera anslutningar. Koppeln `post_deploy` rensar cachen och läser in cachen i förväg (varmar). Du kan anpassa sidlistan med hjälp av variabeln `WARM_UP_PAGES` i [scenen efter distributionen](../environment/variables-post-deploy.md). Även om det inte krävs fungerar detta tillsammans med miljövariabeln `SCD_ON_DEMAND`.

I följande exempel visas standardkonfigurationen i filen `.magento.app.yaml`. Lägg till CLI-kommandon under `build`, `deploy` eller `post_deploy` avsnitten _före_ kommandot `ece-tools`:

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

Du kan också anpassa byggfasen ytterligare genom att använda kommandona `generate` och `transfer` för att utföra ytterligare åtgärder när du skapar kod eller flyttar filer.

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` - gör att krokar misslyckas vid det första misslyckade kommandot i stället för det sista misslyckade kommandot.
- `build:generate` - tillämpar korrigeringar, validerar konfiguration, genererar ID och genererar statiskt innehåll om SCD är aktiverat för byggfasen.
- `build:transfer` - överför genererad kod och statiskt innehåll till det slutliga målet.

Kommandona körs från programkatalogen (`/app`). Du kan använda kommandot `cd` för att ändra katalogen. Hakarna misslyckas om det sista kommandot i dem misslyckas. Lägg till `set -e` i början av kroken för att få dem att misslyckas med det första misslyckade kommandot.

**Så här kompilerar du Sass-filer med Grund**:

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

Kompilera Sass-filer med `grunt` före distributionen av statiskt innehåll, vilket sker under bygget. Placera kommandot `grunt` före kommandot `build`.

{{scenarios}}
