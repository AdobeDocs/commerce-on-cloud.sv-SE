---
title: Konfigurera flera webbplatser eller butiker
description: Lär dig hur du konfigurerar flera webbplatser eller butiker för Adobe Commerce i molninfrastrukturen.
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 773d8d64-d235-4c2b-87e9-aadbf8471b2c
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# Konfigurera flera webbplatser eller butiker

Du kan konfigurera Adobe Commerce så att det finns flera webbplatser eller butiker, till exempel en engelsk butik, en fransk butik och en tysk butik. Se [Förstå webbplatser, butiker och butiksvyer](best-practices.md#store-views).

>[!WARNING]
>
>Katalogdata utökas allt eftersom du ökar antalet webbplatser och butiker. Beroende på din projektarkitektur kan de extra butikerna leda till en längre indexeringsprocess och längre svarstider för icke-cachelagrade katalogsidor. Adobe rekommenderar att du noga övervakar webbplatsens prestanda.

Hur du konfigurerar flera arkiv beror på om du väljer att använda unika eller delade domäner.

Flera butiker med unika domäner:

```
https://first.store.com/
https://second.store.com/
```

Flera butiker med samma domän:

```
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>Om du vill lägga till en butiksvy i webbplatsens bas-URL behöver du inte skapa flera kataloger. Se [Lägg till lagringskoden i bas-URL:en](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html?lang=sv-SE) i _konfigurationshandboken_.

## Lägg till domäner

Anpassade domäner kan läggas till i Pro Staging och valfri produktionsmiljö. De kan inte läggas till i integreringsmiljöer.

Hur du lägger till en domän beror på typen av molnkonto:

- För Pro Staging and Production

  Lägg till den nya domänen i Snabb, se [Hantera domäner](../cdn/fastly-custom-cache-configuration.md#manage-domains) eller öppna en supportbiljett för att begära hjälp. Dessutom måste du [skicka en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) för att begära att nya domäner läggs till i ett kluster.

- Endast för startproduktion

  Lägg till den nya domänen i Snabb, se [Hantera domäner](../cdn/fastly-custom-cache-configuration.md#manage-domains) eller [Skicka en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) för att få hjälp. Dessutom måste du lägga till den nya domänen på fliken **Domäner** i [!DNL Cloud Console]: `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## Konfigurera lokal installation

Mer information om hur du konfigurerar din lokala installation så att du kan använda flera butiker finns i [Flera webbplatser eller butiker][config-multiweb] i _Konfigurationshandboken_.

När du har skapat och testat den lokala installationen för att kunna använda flera butiker måste du förbereda din integreringsmiljö:

1. **Konfigurera vägar eller platser** - ange hur inkommande URL:er hanteras av Adobe Commerce

   - [Vägar för separata domäner](#configure-routes-for-separate-domains)
   - [Platser för delade domäner](#configure-locations-for-shared-domains)

1. **Konfigurera webbplatser, butiker och butiksvyer** - konfigurera med användargränssnittet i Adobe Commerce Admin
1. **Ändra variabler** - ange värden för variablerna `MAGE_RUN_TYPE` och `MAGE_RUN_CODE` i filen `magento-vars.php`
1. **Distribuera och testa miljöer** - distribuera och testa grenen `integration`

>[!TIP]
>
>Du kan använda en lokal miljö för att konfigurera flera webbplatser eller butiker. Se anvisningarna för Cloud Docker om hur du [konfigurerar flera webbplatser eller butiker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites).

### Konfigurationsuppdateringar för Pro-miljöer

{{pro-self-service-warning}}

### Konfigurera vägar för separata domäner

Rutorna definierar hur inkommande URL:er ska behandlas. Flera arkiv med unika domäner kräver att du definierar varje domän i filen `routes.yaml`. Hur du konfigurerar vägar beror på hur du vill att webbplatsen ska fungera.

**Så här konfigurerar du vägar i en integreringsmiljö**:

1. Öppna filen `.magento/routes.yaml` i en textredigerare på din lokala arbetsstation.

1. Definiera domänen och underdomänerna. Värdet `mymagento` uppströms är samma värde som egenskapen name i filen `.magento.app.yaml`.

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. Spara ändringarna i filen `routes.yaml`.

1. Fortsätt till [Konfigurera webbplatser, butiker och butiksvyer](#set-up-websites-stores-and-store-views).

### Konfigurera platser för delade domäner

Där routningskonfigurationen definierar hur URL-adresserna behandlas, definierar egenskapen `web` i filen `.magento.app.yaml` hur ditt program exponeras för webben. Webb _platser_ tillåter mer granularitet för inkommande begäranden. Om din domän till exempel är `store.com` kan du använda `/first` (standardplats) och `/second` för begäranden till två olika butiker som delar en domän.

**Så här konfigurerar du en ny webbplats**:

1. Skapa ett alias för roten (`/`). I det här exemplet är aliaset `&app` på rad 3.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. Skapa en vidarekoppling för webbplatsen (`/website`) och referera till roten med hjälp av aliaset från föregående steg.

   Aliaset tillåter `website` att komma åt värden från rotplatsen. I det här exemplet finns webbplatsen `passthru` på rad 21.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**Så här konfigurerar du en plats med en annan katalog**:

1. Skapa ett alias för roten (`/`) och för de statiska (`/static`) platserna.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. Skapa en underkatalog för webbplatsen under katalogen `pub`: `pub/<website>`

1. Kopiera filen `pub/index.php` till katalogen `pub/<website>` och uppdatera sökvägen `bootstrap` (`/../../app/bootstrap.php`).

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. Skapa en vidarekoppling för filen `index.php`.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. Verkställ och skicka de ändrade filerna.

   - `pub/<website>/index.php` (Om den här filen finns i `.gitignore` kan push-funktionen kräva alternativet force.)
   - `.magento.app.yaml`

### Konfigurera webbplatser, butiker och butiksvyer

Konfigurera dina _webbplatser_, **Store** och **Store-vyer** i **administratörsgränssnittet**. Se [Konfigurera flera webbplatser, butiker och butiksvyer i Admin](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html?lang=sv-SE) i _Konfigurationshandboken_.

Det är viktigt att du använder samma namn och kod för dina webbplatser, butiker och butiksvyer från din administratör när du konfigurerar din lokala installation. Du behöver dessa värden när du uppdaterar filen `magento-vars.php`.

### Ändra variabler

I stället för att konfigurera en virtuell NGINX-värd skickar du variablerna `MAGE_RUN_CODE` och `MAGE_RUN_TYPE` med hjälp av filen `magento-vars.php` i projektets rotkatalog.

**Så här skickar du variabler med `magento-vars.php` file**:

1. Öppna filen `magento-vars.php` i en textredigerare.

   [standardfilen `magento-vars.php`](https://github.com/magento/magento-cloud/blob/master/magento-vars.php) ska se ut så här:

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. Flytta det kommenterade `if`-blocket så att det är _efter_ `function`-blocket och inte längre kommenteras.

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. Ersätt följande värden i blocket `if (isHttpHost("example.com"))`:
   - `example.com` - med bas-URL:en för din _webbplats_
   - `default` - med den unika KODEN för din _webbplats_ eller _butiksvy_
   - `store` - med ett av följande värden:
      - `website` - läs in _webbplatsen_ i butiken
      - `store` - läs in en _butiksvy_ i butiken

   För flera platser som använder unika domäner:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   För flera webbplatser med samma domän måste du kontrollera _host_ och _URI_:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. Spara ändringarna i filen `magento-vars.php`.

### Distribuera och testa på integreringsservern

Gör ändringar i er Adobe Commerce i molninfrastruktursintegreringsmiljö och testa er webbplats.

1. Lägg till, implementera och skicka kodändringar till fjärrgrenen.

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. Vänta tills distributionen är klar.

1. Efter distributionen öppnar du din Store-URL i en webbläsare.

   Använd formatet `http://<magento-run-code>.<site-URL>` med en unik domän

   Exempel: `http://french.master-name-projectID.us.magentosite.cloud/`

   Använd formatet `http://<site-URL>/<magento-run-code>` med en delad domän

   Exempel: `http://master-name-projectID.us.magentosite.cloud/french/`

1. Testa webbplatsen noggrant och sammanfoga koden till grenen `integration` för ytterligare distribution.

## Distribuera till mellanlagring och produktion

Följ distributionsprocessen för [distribution till Förproduktion och produktion](../deploy/staging-production.md). I Starter- och Pro-miljöer använder du [!DNL Cloud Console] för att skicka kod mellan miljöer.

Adobe rekommenderar att du testar fullt ut i mellanlagringsmiljön innan du går vidare till produktionsmiljön. Gör kodändringar i integreringsmiljön och börja distribuera i olika miljöer igen.

<!-- link definitions -->

[config-multiweb]: https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html?lang=sv-SE
