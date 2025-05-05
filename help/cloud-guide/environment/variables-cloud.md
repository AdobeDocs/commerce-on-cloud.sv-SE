---
title: Molnspecifika variabler
description: Se en lista över miljövariabler som är specifika för Adobe Commerce om molninfrastruktur.
feature: Cloud, Configuration
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Molnspecifika variabler

Miljövariabler som är specifika för Adobe Commerce i molninfrastrukturen använder prefixet `MAGENTO_CLOUD_*`:

| Variabel | Beskrivning |
| -------- | --------------- |
| `MAGENTO_CLOUD_APP_DIR` | Den absoluta sökvägen till programkatalogen. |
| `MAGENTO_CLOUD_APPLICATION` | Ett base64-kodat JSON-objekt som beskriver programmet. Den mappar till filinnehållet `.magento.app.yaml` och har undernycklar. |
| `MAGENTO_CLOUD_APPLICATION_NAME` | Namnet på programmet som konfigurerats i filen `.magento.app.yaml`. |
| `MAGENTO_CLOUD_DOCUMENT_ROOT` | Den absoluta sökvägen till webbdokumentets rot, om tillämpligt. |
| `MAGENTO_CLOUD_ENVIRONMENT` | Namnet på miljögrenen. |
| `MAGENTO_CLOUD_PROJECT` | Projekt-ID. |
| `MAGENTO_CLOUD_RELATIONSHIPS` | Ett base64-kodat JSON-objekt som representerar slutpunktsdefinition för nyckel (relationsnamn) och värde (arrayer för relationspar). Varje relationsslutpunktsdefinition är en uppdelad form av en URL. Den har en `scheme`, en `host`, en `port` och _valfritt_ a `username`, `password`, `path` och ytterligare information i `query`. |
| `MAGENTO_CLOUD_ROUTES` | Beskriv de vägar som definierats i miljöfilen `.magento/routes.yaml`. |
| `MAGENTO_CLOUD_TREE_ID` | Programmets träd-ID, som motsvarar SHA för trädet i Git. |
| `MAGENTO_CLOUD_VARIABLES` | Ett base64-kodat JSON-objekt med nyckelvärdepar, till exempel `"key":"value"`. |
| `MAGENTO_CLOUD_LOCKS_DIR` | Anger sökvägen till monteringspunkten för låsleverantören i molninfrastrukturen. Lås-providern förhindrar att duplicerade cron-jobb och cron-grupper startas. |

>[!WARNING]
>
>Om du vill lägga till miljövariabler i [åsidosätta konfigurationsinställningar](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html?lang=sv-SE) med [[!DNL Cloud Console]](../project/overview.md) måste du lägga till `env:` som i följande exempel som prefix för variabelnamnet:
>
>![Exempel på miljövariabel](../../assets/set-env-variable-ui.png)

Eftersom värdena kan ändras över tid är det bäst att granska variabeln under körning och använda den för att konfigurera programmet. Använd till exempel variabeln `MAGENTO_CLOUD_RELATIONSHIPS` för att hämta miljörelaterade relationer enligt följande:

```php
<?php
/**
  * Get relationships information from cloud environment variable.
  *
  * @return mixed
  */
    protected function getRelationships()
    {
        return json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]), true);
    }
```

## Visa miljövariabler

Du kan använda kommandot `env:config:show` från [paketet `ece-tools`](../dev-tools/package-overview.md) om du vill visa en lista med variabler för den aktuella miljön.

```bash
php ./vendor/bin/ece-tools env:config:show variables
```

Exempelutdata för alternativet `variables`:

```
Magento Cloud Environment Variables:
+-----------------------------------+----------------------------------+
| Variable name                     | Value                            |
+-----------------------------------+----------------------------------+
| ADMIN_EMAIL                       | commerceadmin@company.com        |
| ADMIN_PASSWORD                    | 123123q                          |
+-----------------------------------+----------------------------------+
```
