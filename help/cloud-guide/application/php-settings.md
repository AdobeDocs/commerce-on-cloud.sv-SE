---
title: PHP-inställningar
description: Läs om de optimala PHP-inställningarna för Commerce-programkonfiguration i molninfrastrukturen.
feature: Cloud, Configuration, Extensions
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---

# PHP-inställningar

Du kan välja vilken [version av PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) som ska köras i `.magento.app.yaml`-filen:

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>Om du uppgraderar till PHP 8.1 och senare tar du bort JSON från egenskapen [`runtime: extensions:` ](properties.md#runtime) i filen `.magento.app.yaml` och distribuerar om. JSON-tillägget installeras i molnmiljön sedan PHP 8.0.

## Konfigurera PHP

Du kan anpassa PHP-inställningarna för din miljö med hjälp av en `php.ini`-fil som läggs till i konfigurationen som underhålls av Adobe Commerce.

Lägg till filen `php.ini` i programmets rot (databasroten) i databasen.

>[!TIP]
>
>Om du konfigurerar PHP-inställningar på ett felaktigt sätt kan det uppstå problem, så endast avancerade administratörer bör ange dessa alternativ.

### Öka PHP-minnesgränsen

Om du vill öka PHP-minnesgränsen lägger du till följande inställning i filen `php.ini`:

```ini
memory_limit = 1G
```

För felsökning ökar du värdet till 2G.

### Optimera konfigurationen för realpath_cache

Ange följande `realpath_cache`-inställningar för att förbättra programmets prestanda.

```conf
;
; Increase realpath cache size
;
realpath_cache_size = 10M

;
; Increase realpath cache ttl
;
realpath_cache_ttl = 7200
```

Med de här inställningarna kan PHP-processer cachelagra sökvägar till filer i stället för att leta upp dem för varje sidinläsning. Se [Prestandajustering](https://www.php.net/manual/en/ini.core.php) i PHP-dokumentationen.

>[!NOTE]
>
>En lista med rekommenderade PHP-konfigurationsinställningar finns i [Nödvändiga PHP-inställningar](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html) i _installationshandboken_.

### Kontrollera anpassade PHP-inställningar

När du har överfört `php.ini`-ändringarna till molnmiljön kan du kontrollera att den anpassade PHP-konfigurationen har lagts till i din miljö. Använd till exempel SSH för att logga in i fjärrmiljön och visa filen med något som liknar följande:

```bash
cat /etc/php/<php-version>/fpm/php.ini
```

>[!WARNING]
>
>Om du använder Cloud Docker för Commerce för lokal utveckling läser du [Docker-tjänstbehållare](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container) för mer information om hur du använder en anpassad `php.ini` fil i en Docker-miljö.

## Aktivera tillägg

Du kan aktivera eller inaktivera PHP-tillägg i avsnittet `runtime:extension`. Dessutom blir de angivna tilläggen tillgängliga i Docker PHP-behållarna.

>[!IMPORTANT]
>
>Innan du aktiverar tillägg är det viktigt att du förstår att PHP-versionen måste vara kompatibel med det operativsystem som är värd för projektet. Din projektmiljö kan kräva att infrastrukturteamet uppgraderar operativsystemet innan du kan fortsätta.

Exempel i filen `.magento.app.yaml`:

```yaml
runtime:
    extensions:
        - sockets
        - sodium
        - ssh2
    disabled_extensions:
        - bcmath
        - bz2
        - calendar
        - exif
```

Använd SSH för att logga in i en miljö och lista PHP-tilläggen.

```bash
php -m
```

Mer information om ett specifikt PHP-tillägg finns i [PHP-tilläggslistan](https://www.php.net/manual/en/extensions.alphabetical.php).

Följande tabell visar vilka PHP-tillägg som stöds när du distribuerar Adobe Commerce på molnplattformen.

{{$include /help/_includes/templated/php-extensions-cloud.md}}

PHP-modulkraven är knutna till Adobe Commerce-versionen. Se [PHP-krav](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html).

### Stöd för tillägg

För Pro-projekt krävs ytterligare stöd för följande tillägg:

- `ioncube`
- `sourceguardian`

Om du till exempel vill konfigurera PHP så att bara SourceGuardian-skyddade skript körs i alla miljöer, måste följande alternativ anges i filen `php.ini`:

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

Se [avsnitt 3.5 i SourceGuardian-dokumentationen](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf). _Det här är en länk till PDF_.

[Skicka in en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) om du vill ha hjälp med att installera dessa PHP-tillägg i alla produktionsmiljöer och Pro Staging-miljöer. Inkludera din uppdaterade `.magento/services.yaml`-fil, `.magento.app.yaml`-fil med den uppdaterade PHP-versionen och eventuella ytterligare PHP-tillägg. Om du vill ändra en produktionsmiljö måste du ange minst 48 timmars varsel. Det kan ta upp till 48 timmar för molninfrastrukturteamet att uppdatera ditt projekt.

>[!WARNING]
>
>PHP som kompilerats med felsökning stöds inte och avsökningen kan hamna i konflikt med [!DNL XDebug] eller [!DNL XHProf]. Inaktivera dessa tillägg när du aktiverar avsökningen. Avsökningen står i konflikt med vissa PHP-tillägg som [!DNL Pinba] eller IonCube.
