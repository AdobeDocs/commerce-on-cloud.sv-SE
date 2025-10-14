---
title: Globala variabler
description: Se en lista med miljövariabler som styr åtgärder i Adobe Commerce för driftsättning av molninfrastruktur.
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# Globala variabler

Globala variabelkontrollåtgärder för varje fas i distributionsprocessen [!DNL Commerce]: skapa, distribuera och efterdistribuera. Eftersom globala variabler påverkar varje fas måste du ange dem i `global`-scenen för filen `.magento.env.yaml`:

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

Mer information om hur du anpassar bygg- och distributionsprocessen:

- [Distributionskonfiguration](configure-env-yaml.md)
- [Distributionsprocess](../deploy/process.md)

## `ENABLE_EVENTING`

- **Standard**-_Inte angivet_
- **Version** - Adobe Commerce 2.4.5 och senare

När värdet är `true` aktiveras cron att köra meddelandekökonsumenter. Adobe I/O Events för Adobe Commerce använder meddelandeköer för att snabba upp leveransen av viktiga händelser.

Adobe rekommenderar att du också lägger till variabeln [`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner) i `deploy` -scenen i filen `.magento.env.yaml` med `cron_run` inställd på `true`.

I följande exempel visas en fullt konfigurerad `ENABLE_EVENTING`-variabel.

```yaml
stage:
  global:
    ENABLE_EVENTING: true
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 0
      consumers: []
```

## ENABLE_WEBHOOKS

- **Standard**-_Inte angivet_
- **Version** - Adobe Commerce 2.4.4 och senare

När inställningen är `true` aktiveras Commerce webbhooks. Webbhoten körs på en extern slutpunkt, till exempel en App Builder-körningsåtgärd eller ett lagerhanteringssystem från en annan leverantör. [_Webhooks-guiden_](https://developer.adobe.com/commerce/extensibility/webhooks) beskriver den här funktionen i detalj.

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **Standard**—_Inte angivet_
- **Version** - Adobe Commerce 2.1.4 och senare

Åsidosätter den lägsta loggningsnivån för alla utdataströmmar utan att ändra koden, vilket hjälper vid felsökning av problem med distributionen. Om distributionen misslyckas kan du använda den här variabeln för att öka loggningsgranulariteten globalt. Se [Loggnivåer](log-handlers.md#log-levels). Värdet `min_level` i loggningshanteraren skriver över den här inställningen.

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>Inställningen för variabeln `MIN_LOGGING_LEVEL` ändrar inte loggnivåkonfigurationen för filhanteraren, som är inställd på `debug` som standard.

## `SCD_ON_DEMAND`

- **Standard**—_Inte angivet_
- **Version** - Adobe Commerce 2.1.4 och senare

Aktivera generering av statiskt innehåll när en användare begär det (SCD). Statiskt innehåll on demand är idealiskt för utvecklings- och testarbetsflöden eftersom det minskar driftsättningstiden.

Om du läser in cachen i förväg med [`post_deploy`-kroken &#x200B;](../application/hooks-property.md) minskas platsens drifttid. Cachevärmaren är bara tillgänglig för Pro-projekt som innehåller miljöer för mellanlagring och produktion i [!DNL Cloud Console] och för Starter-projekt. Lägg till miljövariabeln `SCD_ON_DEMAND` på scenen `global` i filen `.magento.env.yaml`:

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

Variabeln `SCD_ON_DEMAND` hoppar över SCD i båda faserna (skapa och distribuera), rensar mapparna `pub/static` och `var/view_preprocessed` och skriver följande till filen `app/etc/env.php`:

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **Standard**—_Inte angivet_
- **Version** - Adobe Commerce 2.2.0 och senare

Gör att du kan öka den maximala förväntade körningstiden för statisk innehållsdistribution.

Som standard sätter Adobe Commerce den maximala förväntade körningen till 900 sekunder, men i vissa fall behöver du mer tid för att slutföra distributionen av statiskt innehåll för ett Cloud-projekt.

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Standard**—_Inte angivet_
- **Version** - Adobe Commerce 2.4.2 och senare

Ange `true` för att förhindra att statiskt innehåll genereras för överordnade teman under bygg- och distributionsfaserna. När det här alternativet är inställt på `true` genereras mindre statiskt innehåll, vilket förbättrar genererings- och distributionstiderna.

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **Standard**—_Inte angivet_
- **Version** - Adobe Commerce 2.3.0 och senare

[Baler](https://github.com/magento/baler) är en modul som skannar din genererade JavaScript-kod och skapar ett optimerat JavaScript-paket. Genom att distribuera det optimerade paketet till din webbplats kan du minska antalet nätverksförfrågningar när du läser in webbplatsen och förbättra sidinläsningstiden.

Ange `true` om du vill köra Baler efter att ha utfört distributionen av statiskt innehåll.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Installera och konfigurera Baler-modulen innan du använder den här funktionen. Eftersom Balans är i alfaversion kan du bara aktivera det här alternativet i mellanlagringsmiljöer.

## `SKIP_HTML_MINIFICATION`

- **Standard**:
   - `true` - för `ece-tools` 2002.0.13 och senare
   - `false` - för tidigare versioner av `ece-tools`
- **Version** - Adobe Commerce 2.1.4 och senare

Aktiverar eller inaktiverar kopiering av statiska vyfiler till katalogen `<magento_root>/init/` i slutet av byggfasen. Om värdet är `true` kopieras inga filer och HTML-miniatyrbilder är tillgängliga på begäran. Ange det här värdet till `true` om du vill minska driftstoppen vid distribution till miljö för förproduktion och produktion.

- **`false`** - Kopierar katalogen `view_preprocessed` till katalogen `<magento_root>/init/` i slutet av byggfasen och återställer katalogen i katalogen `<magento_root>/var` i början av distributionsfasen.
- **`true`** - Aktiverar miniatyr på begäran av HTML; kopierar _inte_ `<magento_root>var/view_preprocessed` till katalogen `<magento_root>/init/` i slutet av byggfasen.

Lägg till miljövariabeln `SKIP_HTML_MINIFICATION` på scenen `global` i filen `.magento.env.yaml`:

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **Standard**—_Inte angivet_
- **Version** - Adobe Commerce 2.1.4 och senare

Använd variabeln `X_FRAME_CONFIGURATION` för att ändra rubrikkonfigurationen för [`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html?lang=sv-SE) för din Adobe Commerce-webbplats. Den här konfigurationen styr hur webbläsaren återger en sida i en `<frame>`, `<iframe>` eller `<object>`. Använd något av följande alternativ:

- `DENY` - Det går inte att visa sidan i en ram.
- `SAMEORIGIN`—(Standardinställningen för Adobe Commerce.) Sidan kan bara visas i en ram med samma ursprung som sidan.

>[!WARNING]
>
>Alternativet `ALLOW-FROM <uri>` har tagits bort eftersom webbläsare som stöds av Adobe Commerce inte längre stöder det. Se [Webbläsarkompatibilitet](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility).

Lägg till miljövariabeln `X_FRAME_CONFIGURATION` på scenen `global` i filen `.magento.env.yaml`:

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
