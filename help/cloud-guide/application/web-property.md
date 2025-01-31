---
title: Webbegenskap
description: Se exempel på hur du konfigurerar egenskapen web i  [!DNL Commerce] programkonfigurationsfilen.
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# Webbegenskap

Egenskapen `web` definierar hur ditt program exponeras för webben (i HTTP), avgör hur webbprogrammet visar innehåll och styr hur programbehållaren svarar på inkommande begäranden genom att ange regler på varje plats _block_. Ett block representerar en absolut sökväg med ett snedstreck (`/`).

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

Du kan finjustera din `locations`-konfiguration med följande nyckelvärden för varje `locations` -block:

| Attribut | Beskrivning |
| :--- | :--- |
| `allow` | Servera filer som inte matchar reglerna. Standardvärde = `true` |
| `expires` | Ange antalet sekunder som innehållet ska cachelagras i webbläsaren. Den här nyckeln aktiverar rubrikerna `cache-control` och `expires` för statiskt innehåll. Om det här värdet inte anges inkluderas inte direktivet `expires` och de resulterande rubrikerna när statiska innehållsfiler hanteras. Ett negativt 1 (`-1`)-värde resulterar i ingen cachelagring och är standardvärdet. Du kan uttrycka tidsvärdet med följande enheter: `ms` (millisekunder), `s` (sekunder), `m` (minuter), `h` (timmar), `d` (dagar), `w` (veckor), `M` (månader, 30d) eller `y` (år, 365d) |
| `headers` | Ange anpassade rubriker, till exempel `X-Frame-Options`, för statiskt innehåll som hanteras från den här platsen. |
| `index` | Visa en lista över statiska filer som kan användas för programmet, till exempel filen `index.html`. Den här nyckeln förväntar sig en samling. Detta fungerar bara om åtkomst till filen eller filerna tillåts av `allow`- eller `rules`-tangenten för den här platsen. |
| `rules` | Ange åsidosättningar för en plats. Använd ett reguljärt uttryck för att matcha en begäran. Om en inkommande begäran matchar regeln åsidosätts den vanliga hanteringen av begäran av nycklarna som används i regeln. |
| `passthru` | Ange den URL som används om en statisk fil eller PHP-fil inte kan hittas. Vanligtvis är den här URL-adressen den främre kontrollenheten för dina program, till exempel `/index.php` eller `/app.php`. |
| `root` | Ange sökvägen i förhållande till roten för programmet som visas på webben. Den offentliga katalogen (plats &quot;/&quot;) för ett Cloud-projekt är som standard inställd på &quot;pub&quot;. |
| `scripts` | Tillåt inläsning av skript på den här platsen. Ange värdet till `true` om du vill tillåta skript. |

Standardkonfigurationen tillåter följande:

- Från rotsökvägen (`/`) går det bara att komma åt webb och media
- Från sökvägarna `~/pub/static` och `~/pub/media` kan du komma åt alla filer

I följande exempel visas standardkonfigurationen i filen `.magento.app.yaml` för en uppsättning webbtillgängliga platser som är associerade med en post i egenskapen [`mounts`: ](properties.md#mounts)

```yaml
 # The configuration of app when it is exposed to the web.
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
            root: "pub"
            # The front-controller script to send non-static requests to.
            passthru: "/index.php"
            index:
                - index.php
            expires: -1
            scripts: true
            allow: false
            rules:
                \.(css|js|map|hbs|gif|jpe?g|png|tiff|wbmp|ico|jng|bmp|svgz|midi?|mp?ga|mp2|mp3|m4a|ra|weba|3gpp?|mp4|mpe?g|mpe|ogv|mov|webm|flv|mng|asx|asf|wmv|avi|ogx|swf|jar|ttf|eot|woff|otf|html?)$:
                    allow: true
                ^/sitemap(.*)\.xml$:
                    passthru: "/media/sitemap$1.xml"
        "/media":
            root: "pub/media"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/get.php"
        "/static":
            root: "pub/static"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/front-static.php"
            rules:
                ^/static/version\d+/(?<resource>.*)$:
                    passthru: "/static/$resource"
```

>[!NOTE]
>
>I det här exemplet visas standardwebbkonfigurationen för ett molnprojekt som har konfigurerats för att stödja en enda domän. För ett projekt som kräver stöd för flera webbplatser eller arkiv måste konfigurationen `web` konfigureras så att den stöder delade domäner. Se [Konfigurera platser för delade domäner](../store/multiple-sites.md#configure-locations-for-shared-domains).
