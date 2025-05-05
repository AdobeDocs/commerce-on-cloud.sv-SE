---
title: Konfigurera flöden
description: Lär dig hur du definierar vägar för inkommande HTTPS-begäranden för Adobe Commerce i molninfrastrukturer.
feature: Cloud, Configuration, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# Konfigurera flöden

Filen `routes.yaml` i katalogen `.magento/routes.yaml` definierar vägar för din Adobe Commerce i molninfrastruktur för integrering, mellanlagring och produktion. Rutorna avgör hur programmet hanterar inkommande HTTP- och HTTPS-begäranden.

Standardfilen `routes.yaml` anger flödesmallarna för bearbetning av HTTP-begäranden som HTTPS i projekt som har en enda standarddomän och i projekt som har konfigurerats för flera domäner:

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

Använd CLI:n för `magento-cloud` för att visa en lista över konfigurerade vägar:

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Konfigurationsuppdateringar för Pro-miljöer

{{pro-self-service-warning}}

## Flödesmallar

Filen `routes.yaml` är en lista över mallvägar och deras konfigurationer. Du kan använda följande platshållare i flödesmallar:

- Platshållaren `{default}` representerar det kvalificerade domännamnet som konfigurerats som standard för projektet.

  Ett projekt med standarddomänen `example.com` och följande flödesmallar:

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  Mallarna matchar följande URL:er i en produktionsmiljö:

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- Platshållaren `{all}` representerar alla domännamn som konfigurerats för projektet.

  Ett projekt med till exempel `example.com` och `example1.com` domäner med följande vägmallar:

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  Mallarna matchar följande vägar för alla domäner i projektet:

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  Platshållaren `{all}` är användbar för projekt som konfigurerats för flera domäner. I en icke-produktionsgren ersätts `{all}` med projekt-ID och miljö-ID för varje domän.

  Om ett projekt inte har några konfigurerade domäner, som är vanliga under utvecklingen, fungerar platshållaren `{all}` på samma sätt som platshållaren `{default}`.

Adobe Commerce genererar också vägar för alla aktiva integreringsmiljöer. För integreringsmiljöer ersätts platshållaren `{default}` med följande domännamn:

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

`refactorcss`-grenen för `mswy7hzcuhcjw`-projektet i `us`-klustret har till exempel följande domän:

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>Om ditt molnprojekt har stöd för flera butiker följer du instruktionerna för vägkonfiguration för [flera webbplatser eller butiker](../store/multiple-sites.md).

### Snedstreck

Flödesdefinitioner innehåller ett avslutande snedstreck som anger en mapp eller katalog, men samma innehåll kan hanteras med eller utan ett avslutande snedstreck. Följande URL:er matchar, men kan tolkas som _två olika_ URL:er:

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>Det är bäst att använda ett avslutande snedstreck för kataloger, men oavsett vilken metod du väljer är det viktigt att **vara konsekvent** för att undvika att generera två platser.

## Cirkulationsprotokoll

Alla miljöer stöder både HTTP och HTTPS automatiskt.

- Om konfigurationen bara anger HTTP-vägen skapas HTTPS-vägar automatiskt så att platsen kan hanteras från både HTTP och HTTPS utan att omdirigeringar krävs.

  Ett projekt med standarddomänen `example.com` och följande flödesmall:

  ```text
  http://{default}/
  ```

  Den här mallen matchar följande URL:er:

  ```text
  http://example.com/
  
  https://example.com/
  ```

- Om konfigurationen bara anger HTTPS-vägen dirigeras alla HTTP-begäranden om till HTTPS.

  Ett projekt med standarddomänen `example.com` med följande flödesmall:

  ```text
  https://{default}/
  ```

  Den här mallen matchar följande URL:

  ```text
  https://example.com/
  ```

  Dessutom bearbetas följande omdirigering:

  `http://example.com/` ==> `https://example.com/`

Hantera alla sidor via TLS. För den här konfigurationen måste du konfigurera omdirigeringar för alla okrypterade begäranden till TLS-motsvarigheten med någon av följande metoder:

- Ändra protokollet till HTTPS i filen `routes.yaml`.

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- För mellanlagrings- och produktionsmiljöer aktiverar du alternativet [Tvinga TLS snabbt](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html?lang=sv-SE) i administratörsgränssnittet. När du använder det här alternativet hanterar omdirigeringen snabbt till HTTPS, så du behöver inte uppdatera konfigurationen för `routes.yaml`.

## Flödesalternativ

Konfigurera varje väg separat med följande egenskaper:

| Egenskap | Beskrivning |
| ---------------- | ----------- |
| `type: upstream` | Serverar ett program. Dessutom har den egenskapen `upstream` som anger programmets namn (enligt definition i `.magento.app.yaml`) följt av slutpunkten `:http`. |
| `type: redirect` | Omdirigerar till en annan väg. Den följs av egenskapen `to`, som är en HTTP-omdirigering till en annan väg som identifieras av dess mall. |
| `cache:` | Kontrollerar [cachelagring för vägen](caching.md). |
| `redirects:` | Kontrollerar [omdirigeringsregler](redirects.md). |
| `ssi:` | Kontroller som aktiverar [SSI](server-side-includes.md). |

## Enkla vägar

I följande exempel dirigerar vägkonfigurationen den överordnade domänen och `www`-underdomänen till `mymagento`-programmet. Den här vägen dirigerar inte om HTTPS-begäranden.

**Exempel 1:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

I det här exemplet följer routningen av begäran följande regler:

- Servern svarar direkt på begäranden med följande URL-mönster:

  ```text
  http://example.com/path
  ```

- Servern utfärdar en _301 omdirigering_ för begäranden med följande URL-mönster:

  ```text
  http://www.example.com/mypath
  ```

  De här förfrågningarna omdirigerar till den vanliga domänen, till exempel:

  ```text
  http://example.com/mypath
  ```

I följande exempel omdirigerar vägkonfigurationen inte URL:er från www-domänen till apex-domänen. I stället hanteras förfrågningar från både www- och apex-domänen.

**Exempel 2:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## Jokerrutter

Adobe Commerce i molninfrastrukturen har stöd för jokerrutter, så du kan mappa flera underdomäner till samma program. Detta fungerar för omdirigerings- och uppströms-vägar. Du prefix vägen med en asterisk (\*). Följande dirigerar till exempel till samma program:

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

Detta fungerar som en&quot;catch-all&quot;-domän i en live-miljö.

### Routning av en domän som inte är mappad

Du kan dirigera till ett system som inte är mappat till en domän med hjälp av en punkt (`.`) för att separera underdomänen.

**Exempel:**

Ett projekt med en `add-theme`-gren dirigerar till följande URL:

```text
http://add-theme-projectID.us.magento.com/
```

Om du definierar följande flödesmall:

```text
http://www.{default}/
```

Vägen matchar följande URL:

```text
http://www.add-theme-projectID.us.magento.com/
```

Du kan infoga valfri underdomän innan punkten och flödet löses.

**Exempel:**

Definiera följande flödesmall:

```text
http://*.{default}/
```

Den här mallen löser båda följande URL:er:

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

Du kan visa ruttmönstret för icke-mappade domäner genom att upprätta en SSH-anslutning till miljön och använda CLI `magento-cloud` för att lista rutterna:

```bash
vendor/bin/ece-tools env:config:show routes

Magento Cloud Routes:
+------------------------------------------+--------------------------------------------------------------+
| Route configuration                      | Value                                                        |
+------------------------------------------+--------------------------------------------------------------+
| http://www.add-theme-projectID.us.magento.com/:                                                         |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https:/{default}/                                            |
+------------------------------------------+--------------------------------------------------------------+
| https://*.add-theme-projectID.us.magentosite.cloud/:                                                    |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https://*.{default}/                                         |
+------------------------------------------+--------------------------------------------------------------+
```

## Omdirigering och cachelagring

Som beskrivs mer ingående i [Omdirigeringar](redirects.md) kan du hantera komplexa omdirigeringsregler, till exempel _partiella omdirigeringar_, och ange regler för flödesbaserad [cachning](caching.md):

```yaml
https://www.{default}/:
    type: redirect
    to: https://{default}/
https://{default}/:
    cache:
        cookies: [""]
        default_ttl: 0
        enabled: true
        headers:
            - Accept
            - Accept-Language
    ssi:
        enabled: false
    type: upstream
    upstream: mymagento:http
```
