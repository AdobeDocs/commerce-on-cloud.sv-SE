---
title: Variabler efter distribution
description: Se en lista med miljövariabler som styr åtgärder i Adobe Commerce för efterdriftsättning av molninfrastruktur.
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# Variabler efter distribution

Följande _post-deploy_-variabler kontrollerar åtgärder i efterdistribueringsfasen och kan ärva och åsidosätta värden från [Globala variabler](variables-global.md). Infoga de här variablerna i `post-deploy`-steget i filen `.magento.env.yaml`:

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

Mer information om hur du anpassar bygg- och distributionsprocessen:

- [Distributionskonfiguration](configure-env-yaml.md)
- [Distributionsprocess](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **Standard**— `[]` (en tom array)
- **Version** - Adobe Commerce 2.1.4 och senare

Konfigurera TTFB-testning (_Time To First Byte_) för angivna sidor för att testa platsens prestanda. Ange en absolut sökvägsreferens, eller URL med protokoll och värd, för varje sida som kräver testet.

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

När du har angett vilka sidor som ska testas och verkställas, körs testet _Tid till första byte_ under fasen efter distributionen och resultat skickas för varje sökväg till molnloggen:

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

För omdirigerade sökvägar rapporterar loggen sökvägen till omdirigeringsmålet i stället för den som konfigurerats i miljövariabeln. Om du anger en ogiltig sökväg visas ett varningsmeddelande i loggen.

## `WARM_UP_CONCURRENCY`

- **Standard**—_Inte angivet_
- **Version** - Adobe Commerce 2.1.4 och senare

Ange gränsen för antalet samtidiga begäranden som ska skickas under cachevärmaråtgärder för att minska serverbelastningen. Det här värdet begränsar antalet parallella anslutningar och är användbart för miljökonfigurationer där variabeln `WARM_UP_PAGES` efter distribution anger flera sidor för cacheförinläsning.

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **Standard**— `index.php`
- **Version** - Adobe Commerce 2.1.4 och senare

Anpassa listan med sidor som används för att förhandsladda cachen på scenen `post_deploy`. Du måste konfigurera kroken efter distributionen. Se avsnittet [hooks](../application/hooks-property.md) i filen `.magento.app.yaml`.

- **enstaka sidor** - Ange en sida som ska läggas till i cachen. Du behöver inte ange standardbas-URL:en. Följande exempel cachelagrar sidan `BASE_URL/index.php`:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **flera domäner** - Visa flera URL:er. I följande exempel cachelagras sidor från två domäner:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **flera sidor** - Använd följande format för att cachelagra flera sidor enligt ett visst mönster för reguljära uttryck:

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`: Möjliga varianter `category`, `cms-page`, `product`, `store-page`
   - `pattern|url|product_sku`: Använd ett `regexp` -mönster eller en exakt matchning `url` för att filtrera URL-adresserna, eller använd en asterisk (\*) för alla sidor. Använd produktsku för entitetstypen `product`
   - `store_id|store_code`: Använd ID:t eller koden för butiken eller en asterisk (\*) för alla butiker, du kan skicka flera butiks-ID:n eller koder avgränsade med `|`

  Följande exempel cachelagrar för entitetstyperna `category` och `cms-page` baserat på dessa kriterier:
   - alla kategorisidor för butik med ID `1`
   - alla kategorisidor för butiker med koden `store1` och `store2`
   - kategorisida `cars` för butik med kod `store_en`
   - cms-sida `contact` för alla butiker
   - cms-sida `contact` för butiker med ID `1` och `2`
   - en kategorisida som innehåller `car_` och slutar med `html` för butik med ID 2
   - en kategorisida som innehåller `tires_` för butik med koden `store_gb`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  Följande exempel cachelagrar för entitetstypen `product` baserat på dessa villkor:
   - alla produkter för alla butiker (programmatiskt begränsat till 100 per butik för att undvika prestandaproblem)
   - alla produkter för butiken `store1`
   - produkter med `sku1` för alla butiker
   - produkter med `sku1` för butiker med koden `store1` och `store2`
   - produkter med `sku1`, `sku2` och `sku3` för butiker med koden `store1` och `store2`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  Följande exempel cachelagrar för entitetstypen `store-page` baserat på dessa villkor:
   - sida `/contact-us` för alla butiker
   - sida `/contact-us` för butik med ID `1`
   - sidan `/contact-us` för butiker med koden `code1` och `code2`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
