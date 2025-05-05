---
title: Cachning
description: Lär dig hur du aktiverar cachelagring för din Adobe Commerce i molnmiljöer.
feature: Cloud, Cache, Routes
exl-id: e73c36d6-9a58-45c0-9220-86074c1f46f0
source-git-commit: a1ed2818cbaf5adf8b673df0ee9b9218e6f700a2
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 0%

---

# Cachning

Du kan aktivera cachelagring i din projektmiljö för molninfrastruktur. Om du inaktiverar cachelagring visas filerna direkt i Adobe Commerce.

{{route-placeholder}}

## Konfigurera cachelagring

Aktivera cachelagring för programmet genom att konfigurera cacheregler i filen `.magento/routes.yaml` enligt följande:

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## Flödesbaserad cachelagring

Aktivera detaljerad cachelagring genom att ange cachelagringsregler för flera rutter separat, vilket visas i följande exempel:

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

Exemplet ovan cachelagrar följande vägar:

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

Och följande vägar är **inte** cachelagrade:

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>Reguljära uttryck i vägar stöds **inte**.

## Cachevaraktighet

Cachevaraktigheten bestäms av svarshuvudets värde `Cache-Control`. Om inget `Cache-Control`-huvud finns i svaret används `default_ttl`-nyckeln.

## Cachenyckel

För att bestämma hur ett svar ska cachelagras, skapar Adobe Commerce en cachenyckel som är beroende av flera faktorer och lagrar det svar som är associerat med den här nyckeln. När en begäran levereras med samma cachenyckel återanvänds svaret. Dess syfte liknar HTTP [`Vary`-huvudet ](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44).

Med parametrarna `headers` och `cookies` kan du ändra den här cachenyckeln.

Standardvärdet för de här tangenterna är följande:

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## Cacheattribut

### `enabled`

Aktivera cache för den här vägen när värdet är `true`. Inaktivera cache för den här vägen när värdet är `false`.

### `headers`

Definierar vilka värden cachenyckeln måste vara beroende av.

Om till exempel nyckeln `headers` är följande:

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

Adobe Commerce cachelagrar sedan olika svar för varje värde i HTTP-huvudet `Accept`.

### `cookies`

Nyckeln `cookies` definierar vilka värden cachenyckeln måste vara beroende av.

Exempel:

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

Cachenyckeln beror på värdet för cookien `value` i begäran.

Det finns ett specialfall om nyckeln `cookies` har värdet `["*"]`. Detta värde innebär att alla förfrågningar med en cookie-fil kringgår cacheminnet. Det här är standardvärdet.

>[!NOTE]
>
>Du kan inte använda jokertecken i cookie-namnet. Använd ett exakt cookie-namn eller matcha alla cookies med en asterisk (`*`). `SESS*` eller `~SESS` är till exempel för närvarande **inte** giltiga värden.

Cookies har följande begränsningar:

- Det finns högst **50 cookies** i systemet. Annars genereras ett `Unable to send the cookie. Maximum number of cookies would be exceeded`-undantag. Om du vill öka antalet cookies till 200 använder du [MDVA-12304-korrigeringen](https://experienceleague.adobe.com/docs/commerce-operations/tools/quality-patches-tool/release-notes.html?lang=sv-SE) med [kvalitetskorrigeringsverktyget](https://experienceleague.adobe.com/sv/docs/commerce-learn/tutorials/tools/quality-patch-tool).
- En maximal cookie-storlek är **4096 byte**. Annars genereras ett `Unable to send the cookie. Size of '%name' is %size bytes`-undantag.

### `default_ttl`

Om svaret inte har något `Cache-Control`-huvud används `default_ttl`-tangenten för att definiera cachevaraktigheten, i sekunder. Standardvärdet är `0`, vilket betyder att inget cachelagras.
