---
title: Omdirigeringar
description: Lär dig hur du hanterar omdirigeringsregler för Adobe Commerce i molninfrastrukturprojekt.
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Omdirigeringar

Hantering av omdirigeringsregler är ett vanligt krav för webbprogram, särskilt när du inte vill förlora inkommande länkar som har ändrats eller tagits bort över tid.

I följande exempel visas hur du hanterar omdirigeringsregler på din Adobe Commerce för molninfrastrukturprojekt med konfigurationsfilen `routes.yaml`. Om omdirigeringsmetoderna som beskrivs i det här avsnittet inte fungerar för dig kan du använda cachelagringshuvuden för att göra samma sak.

{{route-placeholder}}

## Uppdateringar av Pro-miljöer

{{pro-self-service-warning}}

>[!WARNING]
>
>Om du konfigurerar flera icke-regex-omdirigeringar och omskrivningar i filen `routes.yaml` för Adobe Commerce i molninfrastrukturprojekt kan det orsaka prestandaproblem. Om din `routes.yaml`-fil är 32 kB eller större kan du avlasta din icke-regex och omdirigera den till Fast. Se [Avlasta icke-regex-omdirigeringar till Fast istället för Nginx (vägar)](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html) i _Adobe Commerce Help Center_.

## Omdirigeringar i hela flödet

Med hjälp av omdirigeringar för hela vägen kan du definiera enkla vägar med hjälp av filen `routes.yaml`. Du kan till exempel omdirigera från en huvuddomän till en `www`-underdomän enligt följande:

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## Omdirigeringar för delväg

I filen `.magento/routes.yaml` kan du lägga till regler för partiell omdirigering till befintliga flöden baserat på mönstermatchning:

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

Delvisa omdirigeringar fungerar med alla typer av vägar, inklusive flöden som betjänas direkt av programmet.

Det finns två nycklar under `redirects`:

- **förfaller** - Valfritt, anger hur lång tid omdirigeringen ska cachelagras i webbläsaren. Exempel på giltiga värden är `3600s`, `1d`, `2w`, `3m`.

- **paths** - Ett eller flera nyckelvärdepar som anger konfigurationen för omdirigeringsregler för partiell väg.

  För varje omdirigeringsregel är nyckeln ett uttryck för att filtrera sökvägar för omdirigering. Värdet är ett objekt som anger målmålet för omdirigeringen och alternativ för bearbetning av omdirigeringen.

  Objektet value har följande egenskaper:

  | Egenskap | Beskrivning |
  | ---------- | ----------- |
  | `to` | Obligatoriskt, en partiell absolut sökväg, URL med protokoll och värd, eller mönster som anger målmålet för omdirigeringsregeln. |
  | `regexp` | Valfritt, standardvärdet är `false`. Anger om sökvägsnyckeln ska tolkas som ett reguljärt PCRE-uttryck. |
  | `prefix` | Anger om omdirigeringen gäller både banan och alla dess underordnade objekt, eller bara själva banan. Standardvärdet är `true`. Det här värdet stöds inte om `regexp` är `true`. |
  | `append_suffix` | Avgör om suffixet överförs med omdirigeringen. Standardvärdet är `true`. Det här värdet stöds inte om nyckeln `regexp` är `true` eller* om nyckeln `prefix` är `false`. |
  | `code` | Anger HTTP-statuskoden. Giltiga statuskoder är [`301` (Flyttad permanent)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2), [`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3), [`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8) och [`308`](https://www.rfc-editor.org/rfc/rfc7238). Standardvärdet är `302`. |
  | `expires` | Valfritt, anger hur lång tid det tar att cachelagra omdirigeringen i webbläsaren. Standardvärdet är det `expires`-värde som definieras direkt under nyckeln `redirects`, men på den här nivån kan du finjustera cacheminnets förfallotid för enskilda partiella omdirigeringar. |

## Exempel på delvägsomdirigeringar

I följande exempel visas hur du anger delvägsomdirigeringar i filen `routes.yaml` med hjälp av olika `paths`-konfigurationsalternativ.

### Mönstermatchning för reguljära uttryck

Använd följande format för att konfigurera omdirigeringsbegäranden baserat på ett reguljärt uttryck.

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

Den här konfigurationen filtrerar sökvägar mot ett reguljärt uttryck och dirigerar om matchande begäranden till `https://example.com`. En begäran om `https://example.com/regexp/a/b/c/match` dirigeras till exempel om till `https://example.com/a/b/c`.

### Matchning av prefixmönster

Använd följande format för att konfigurera omdirigeringsbegäranden för sökvägar som börjar med ett angivet prefixmönster.

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

Den här konfigurationen fungerar så här:

- Omdirigerar begäranden som matchar mönstret `/from` till sökvägen `http://{default}/to`.

- Omdirigerar begäranden som matchar mönstret `/from/another/path` till `https://{default}/to/another/path`.

- Om du ändrar egenskapen `prefix` till `false` utlöser förfrågningar som matchar mönstret `/from` en omdirigering, men förfrågningar som matchar mönstret `/from/another/path` gör det inte.

### Matchning av suffixmönster

Använd följande format för att konfigurera omdirigeringsbegäranden som bifogar sökvägssuffixet från begäran till målmålet:

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

Den här konfigurationen fungerar så här:

- Omdirigerar begäranden som matchar mönstret `/from/path/suffix` till sökvägen `https://{default}/to`.

- Om du ändrar egenskapen `append_suffix` till `true` dirigeras förfrågningar som matchar `/from/path/suffix` om till sökvägen `https://{default}/to/path/suffix`.

### Sökvägsspecifik cachekonfiguration

Använd följande format om du vill anpassa tiden för att cachelagra en omdirigering från en viss sökväg:

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
      paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

Den här konfigurationen fungerar så här:

- Omdirigeringar från den första sökvägen (`/from`) cachas i en dag.

- Omdirigeringar från den andra sökvägen (`/here`) cachelagras i två veckor.
