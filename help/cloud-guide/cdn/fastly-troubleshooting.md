---
title: Snabbt felsökning
description: Lär dig hur du felsöker och hanterar snabbuppdateringsmodulen och tjänsterna för Adobe Commerce.
feature: Cloud, Configuration, Cache, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%

---

# Snabbt felsökning

Använd följande information för att felsöka och hantera snabbuppdateringsmodulen för CDN för Magento 2 i dina Adobe Commerce i miljöer med molninfrastrukturprojekt. Du kan till exempel undersöka svarshuvudets värden och cachningsbeteendet för att lösa problem med snabb service och prestanda.

I Pro Production- och Staging-miljöer kan du använda [New Relic-loggar](../monitor/log-management.md) för att visa och analysera snabbdata för CDN och WAF-loggdata för att felsöka fel och prestandaproblem.

>[!NOTE]
>
>Mer information om hur du konfigurerar och konfigurerar snabbt finns i [Konfigurera snabbt](fastly.md).

## Hitta snabbt tjänst-ID

Du behöver ett snabb service-ID för att konfigurera snabbt från administratören eller för att skicka in snabba API-begäranden för avancerad snabb konfiguration och felsökning.

Om Snabbt är aktiverat i din projektmiljö kan du hämta service-ID:t från administratören. Se [Få snabbt inloggningsuppgifter](fastly-configuration.md#get-fastly-credentials).

Utvecklare och avancerade VCL-användare kan använda anpassad VCL för att hämta tjänst-ID:t med hjälp av snabbvariabeln `req.service_id`. Du kan till exempel lägga till `req.service_id` i det anpassade loggningsdirektivet i din VCL för att hämta service-ID-värdet:

```json
log {"syslog"} req.service_id {" my_logging_endpoint_name :: "}
```

Du kan använda samma VCL för produktions- och mellanlagringsmiljöer. Se [`vcl_log`](https://www.fastly.com/documentation/reference/vcl/subroutines/log/) i _Snabbt dokumentation_.

## Problem med webbplatsprestanda, rensning och cache

Använd följande lista för att identifiera och felsöka problem som rör Snabb tjänstkonfiguration för din Adobe Commerce i molninfrastrukturmiljö.

- **Butiksmenyn visas inte och fungerar inte**. Du kanske använder en länk eller en tillfällig länk direkt till den ursprungliga servern i stället för att använda den aktiva webbplatsens URL, eller så använde du `-H "host:URL"` i ett [cURL-kommando](#check-live-site-through-fastly). Om du snabbt kringgår den ursprungliga servern fungerar inte huvudmenyn och felaktiga rubriker visas som tillåter cachelagring på webbläsarsidan.

- **Övre navigering fungerar inte**. Den översta navigeringen är beroende av ESI-bearbetning (Edge Side Includes), som aktiveras när du överför de förvalda VCL-kodfragmenten för Magento fast. Om navigeringen inte fungerar kan du [överföra snabbVCL](fastly-configuration.md#upload-vcl-to-fastly) och kontrollera webbplatsen igen.

- **Geo-location/GeoIP fungerar inte**. Standardvärdet för VCL-kodfragment för Magento snabbt lägger till landskoden i URL:en. Om landskoden inte fungerar kan du [överföra snabbVCL](fastly-configuration.md#upload-vcl-to-fastly) och kontrollera webbplatsen igen.

- **Sidor cachelagras inte**. Som standard cachelagras inte sidor med sidhuvudet `Set-Cookies`. Adobe Commerce ställer in cookies även på cachebara sidor (TTL > 0). Med standardinställningen Magento-VCL raderas dessa cookies på cacheable-sidor. Om sidorna inte cachelagras kan du [överföra snabbVCL](fastly-configuration.md#upload-vcl-to-fastly) och kontrollera webbplatsen igen.

  Detta kan även inträffa om ett sidblock i en mall markeras som oåtkomligt. I så fall beror problemet troligen på att en modul från tredje part eller ett tillägg blockerar eller tar bort Adobe Commerce-huvuden. Information om hur du löser problemet finns i [X-Cache innehåller endast MISS, inget HIT](#x-cache-contains-only-miss-no-hit).

- **Rensningsbegäranden misslyckas** - Följande fel returneras snabbt när du skickar en rensningsbegäran:

  ```text
  The purge request was not processed successfully.
  ```

  Problemet kan bero på något av följande:

   - Ogiltiga snabbreferenser i snabbtjänstkonfigurationen för Adobe Commerce i molninfrastrukturens projektmiljö
   - Ogiltig kod i ett anpassat VCL-fragment

  Information om hur du löser problemet finns i [Fel vid snabb rensning av cache i molnet](https://support.magento.com/hc/en-us/articles/115001853194-Error-purging-Fastly-cache-on-Cloud-The-purge-request-was-not-processed-successfully-) i Adobe Commerce Help Center.

## 503 fel från Snabb

Om du snabbt returnerar 503 timeout-fel kontrollerar du felloggarna och felsidan 503 för att identifiera rotorsaken.

>[!NOTE]
>
>Om tidsgränsen uppnås när gruppåtgärder körs kan du [förlänga den snabba tidsgränsen för administratören](fastly-custom-cache-configuration.md#extend-fastly-timeout).

Om du får ett 503-fel bör du kontrollera felloggen för produktions- eller mellanlagringsmiljön och PHP-åtkomstloggen för att felsöka problemet.

**Kontrollera felloggarna**:

- [Fellogg](../test/log-locations.md#application-logs)

  ```text
  /var/log/platform/<project-ID>/error.log
  ```

  Den här loggen innehåller fel från programmet eller PHP-motorn, till exempel `memory_limit`- eller `max_execution_time exceeded`-fel. Om du inte hittar några snabbrelaterade fel kontrollerar du PHP-åtkomstloggen.

- PHP-åtkomstlogg

  ```text
  /var/log/platform/<project-ID>/php.access.log
  ```

  Sök i loggen efter HTTP 200-svar efter den URL som returnerade felet 503. Om du hittar 200-svaret innebär det att Adobe Commerce returnerade sidan utan fel. Det indikerar att problemet kan ha inträffat efter det intervall som överskrider det `first_byte_timeout`-värde som angetts i snabbtjänstkonfigurationen.

När ett 503-fel inträffar returnerar Snabb orsaken på fel- och underhållssidan. Du kanske inte kan se orsaken om du har lagt till kod för en [anpassad svarssida](fastly-custom-response.md). Om du vill visa orsakskoden på standardfelsidan kan du ta bort HTML-koden för den anpassade felsidan.

**Så här kontrollerar du snabbt felsidan för 503**:

{{admin-login-step}}

1. Klicka på **Lagrar** > **Inställningar** > **Konfiguration** > **Avancerat** > **System**.

1. Expandera **Helsidescache** i den högra rutan.

1. Expandera **Anpassade syntetiska sidor** i avsnittet **Snabbt konfigurering** enligt bilden nedan.

   ![Anpassad felsida 503](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. Klicka på **Ange HTML**.

1. Ta bort den anpassade koden. Du kan spara det i ett textprogram och lägga tillbaka det senare.

1. Klicka på **Överför** för att skicka dina uppdateringar snabbt.

1. Klicka på **Spara konfiguration** överst på sidan.

1. Öppna URL:en som orsakade felet 503 igen. Returnerar snabbt en felsida med orsaken som visas i följande exempel.

   ![Snabbt fel](../../assets/cdn/fastly-503-example.png)

## Apex och underdomäner som redan är kopplade till ett Fast-konto

Om API-domänen och underdomänerna för ditt Adobe Commerce i ett molninfrastrukturprojekt redan är kopplade till ett befintligt Fast-konto med ett tilldelat tjänst-ID, kan du inte starta förrän du uppdaterar din snabbkonfiguration:

- Uppdatera API- och underdomänskonfigurationen på det befintliga snabbkontot. Se [Flera snabbkonton och tilldelade domäner](fastly.md#multiple-fastly-accounts-and-assigned-domains).

- [Aktivera och konfigurera snabbt](fastly-configuration.md#enable-fastly-caching) och slutför [DNS-konfigurationen](../launch/checklist.md#update-dns-configuration-with-production-settings)

## Verifiera eller felsök snabbt tjänster

Du kan felsöka prestanda- eller cachelagringsproblem för en Adobe Commerce på en molninfrastrukturwebbplats genom att testa webbplatsens URL:er och undersöka de rubrikvärden som returneras i svaret.

### Kontrollera publicerad webbplats snabbt

Använd API:t Snabbt för att kontrollera de `Fastly-Magento-VCL-Uploaded`- och `X-Cache`-svarshuvuden som returneras från din aktiva webbplats.

API-begäranden skickas snabbt via tillägget Fast för att få svar från era ursprungliga servrar. Om svaret returnerar felaktiga huvuden testar du [origin-servrarna direkt](#bypass-fastly-cache-to-check-adobe-commerce-sites).

**Så här kontrollerar du svarsrubrikerna**:

1. Använd följande `curl`-kommando i en terminal för att testa din webbplats-URL:

   ```bash
   curl https://<live URL> -vo /dev/null -H Fastly-Debug:1
   ```

   Om du inte har angett en statisk väg eller slutfört DNS-konfigurationen för domänerna på den publicerade webbplatsen använder du flaggan `--resolve`, som åsidosätter DNS-namnmatchningen.

   ```bash
   curl -svo /dev/null --resolve '<your_hostname>:443:<IP-address-of-cache-node>' <https-URL>
   ```

   >[!NOTE]
   >
   >Om du vill använda det här kommandot med alternativet `--resolve` måste du ha TLS aktiverat med Snabbt via ett SSL/TLS-certifikat och hitta cachenodens IP-adress.

1. Verifiera [headers](#check-cache-hit-and-miss-response-headers) i svaret för att kontrollera att Fastly fungerar. Du bör se följande unika rubriker i svaret:

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

Om rubrikerna inte har rätt värden kan du läsa följande information:

- [Kontrollera VCL-överföring](#fastly-vcl-has-not-been-uploaded)

- [X-Cache innehåller bara MISS, ingen HIT](#x-cache-contains-only-miss-no-hit)

### Hoppa över snabbcachelagring för att kontrollera Adobe Commerce webbplatser

Om snabbtjänsten returnerar felaktiga rubriker kan du skapa ett VCL-kodfragment som gör att du kan skicka begäranden som kringgår snabbcachen. Se [Kringgå snabbcache](fastly-vcl-bypass-to-origin.md).

När du har lagt till VCL-fragmentet använder du cURL-kommandon för att skicka begäranden till den ursprungliga servern från den angivna IP-adressen. Kontrollera sedan om svaren innehåller fel.

### Kontrollera headers för HIT- och MISS-svar i cache

Kontrollera att det returnerade svaret innehåller följande information:

- Innehåller rubriken `X-Magento-Tags`

- Värdet för huvudet `Fastly-Module-Enabled` är antingen `Yes` eller versionsnumret för modulen Fastly for CDN Magento 2 som är installerad i projektmiljön

- [Cache-Control: max-age](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) är större än 0

- Inställningen [Pragma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.32) är `cache`

Följande utdrag från utdata från cURL-kommandot visar korrekta värden för rubrikerna `Pragma`, `X-Magento-Tags` och `Fastly-Module-Enabled`:

```
* STATE: INIT => CONNECT handle 0x600057800; line 1402 (connection #-5000)
* Rebuilt URL to: https://www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud/
* Added connection 0. The cache now contains 1 members
* Trying 192.0.2.31...
* STATE: CONNECT => WAITCONNECT handle 0x600057800; line 1455 (connection #0)

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Connected to www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud (54.229.163.31) port 443 (#0)

* STATE: WAITCONNECT => SENDPROTOCONNECT handle 0x600057800; line 1562 (connection #0)
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* ALPN, offering h2

... portion omitted for brevity ...

< Set-Cookie: mage-messages=%5B%5D; expires=Wed, 22-Nov-2017 17:39:58 GMT; Max-Age=31536000; path=/
< Pragma: cache
< Expires: Wed, 23 Nov 2016 17:39:56 GMT
< Cache-Control: max-age=86400, public, s-maxage=86400, stale-if-error=5, stale-while-revalidate=5
< X-Magento-Tags: cb_welcome_popup store cb cb_store_info_mobile cb_header_promotional_bar cb_store_info cb_discount-promo-bar cpg_2 cb_83 cb_81 cb_84 cb_85 cb_86 cb_87 cb_88 cb_89 p5646 catalog_product p5915 p6040 p6197 p6227 p7095 p6109 p6122 p6331 p7592 p7651 p7690
< Fastly-Module-Enabled: yes
< Strict-Transport-Security: max-age=31536000
    < Content-Security-Policy: upgrade-insecure-requests
    < X-Content-Type-Options: nosniff
    < X-XSS-Protection: 1; mode=block
    < X-Frame-Options: SAMEORIGIN
    < X-Platform-Server: i-dff64b52
    <
    * STATE: PERFORM => DONE handle 0x600057800; line 1955 (connection #0)
    * multi_done
      0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
    * Connection #0 to host www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud left intact
```

>[!NOTE]
>
>Detaljerad information om träffar och missar finns i [Förstå headers för HIT och MISS i cache med avskärmade tjänster](https://docs.fastly.com/guides/performance-tuning/understanding-cache-hit-and-miss-headers-with-shielded-services) i dokumentationen för snabbast.

### Åtgärda fel i svarshuvuden

I det här avsnittet ges förslag på hur du löser fel som returneras när du kontrollerar svarshuvuden med API:t Snabbt.

#### Modulen Snabbt är inte aktiverad

Om snabbmodulen inte är aktiverad (`Fastly-Module-Enabled: no`) eller om rubriken saknas [använder du SSH för att logga in](../development/secure-connections.md#connect-to-a-remote-environment) i projektet. Kör sedan följande kommando för att kontrollera modulens status.

```bash
php bin/magento module:status Fastly_Cdn
```

Baserat på den returnerade statusen kan du uppdatera snabbkonfigurationen med följande instruktioner.

- `Module does not exist` - Om modulen inte finns [installerar och konfigurerar](https://github.com/fastly/fastly-magento2/blob/master/Documentation/INSTALLATION.md) den snabba CDN-modulen för Magento 2 i en integrationsgren. Aktivera och konfigurera modulen när installationen är klar. Se [Konfigurera snabbt](fastly-configuration.md).

- `Module is disabled` - Om snabbmodulen är inaktiverad kan du aktivera miljökonfigurationen på en `integration` -gren i den lokala miljön. Flytta sedan ändringarna till Förproduktion. Se [Hantera tillägg](../store/extensions.md#install-an-extension).

  Om du använder [Configuration Management](../store/store-settings.md#configure-store) bör du kontrollera Snabb CDN-modulens status i konfigurationsfilen `app/etc/config.php` innan du skickar ändringarna till produktions- eller mellanlagringsmiljön.

  Om modulen inte är aktiverad (`Fastly_CDN => 0`) i filen `config.php` tar du bort filen och kör följande kommando för att uppdatera `config.php` med de senaste konfigurationsinställningarna.

  ```bash
  bin/magento magento-cloud:scd-dump
  ```

#### Snabb VCL har inte överförts

Om Snabbt VCL inte har överförts (`Fastly-Magento-VCL-Uploaded`: `false`) använder du alternativet *Överför VCL* i Admin för att överföra den. Se [Överför snabbt VCL-fragment](fastly-configuration.md#upload-vcl-to-fastly).

#### X-Cache innehåller bara MISS, ingen HIT

Om rubriken `X-Cache` innehåller `HIT` (`HIT, HIT` eller `HIT, MISS`) anger det att det cachelagrade innehållet returneras snabbt.

Om rubriken `X-Cache` är `MISS, MISS` och inte innehåller `HIT` kör du kommandot `curl` igen för att kontrollera att sidan inte nyligen har rensats från cachen.

Om du får samma resultat kan du använda [`curl`-kommandona](#check-live-site-through-fastly) och verifiera [svarsrubrikerna](#check-cache-hit-and-miss-response-headers):

- `Pragma` är `cache`
- `X-Magento-Tags` finns
- `Cache-Control: max-age` är större än 0

Om problemet kvarstår är det troligt att ett annat tillägg återställer dessa rubriker. Upprepa följande procedur i mellanlagringsmiljön genom att inaktivera alla tillägg och aktivera om var och en för att avgöra vilket tillägg som återställer rubrikerna. När du har identifierat tillägget som orsakar problemet måste du inaktivera det i produktionsmiljön.

**Identifiera tillägg som återställer svarshuvuden:**

{{admin-login-step}}

1. Navigera till **Lagrar** > **Inställningar** > **Konfiguration** > **Avancerat** > **Avancerat**.

1. I avsnittet *Inaktivera modulutdata* i den högra rutan hittar du alla dina tillägg och inaktiverar dem.

1. Klicka på **Spara konfiguration**.

1. Klicka på **System** > **Verktyg** > **Cachehantering**.

1. Klicka på **Rensa cachen i Magento**.

1. Utför följande steg för varje tillägg som kan orsaka problem med snabbrubriker:

   - Aktivera ett tillägg i taget, spara konfigurationen och tömma Adobe Commerce-cachen.

   - Kör [`curl`-kommandona ](#check-live-site-through-fastly) för att verifiera [svarsrubrikerna](#check-cache-hit-and-miss-response-headers).

   Upprepa den här processen för varje tillägg. Om rubrikerna för snabbsvar inte längre visas har du identifierat det tillägg som orsakar problem med Snabbt.

När du har identifierat det tillägg som återställer snabbrubriker kontaktar du tilläggsutvecklaren för ytterligare hjälp. Vi kan inte tillhandahålla korrigeringar eller uppdateringar för att få tillägg från tredje part att fungera med Snabb cachelagring.

## Snabb återställning

Om uppdateringar av anpassade VCL-kodfragment eller andra snabbkonfigurationsändringar gör att fel bryts eller returneras på en Adobe Commerce på en molninfrastrukturwebbplats, använder du kommandot [activate](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) i snabbprogrammeringsgränssnittet för att återställa till en tidigare VCL-version. Du kan inte återställa VCL-versionen från administratören.

**Så här återställer du VCL-versionen**:

1. Om du vill visa en lista över tillgängliga VCL-versioner för en tjänst kör du följande kommando

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Accept: application/json" https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version
   ```

1. Kör följande kommando för att ändra den aktiva VCL-versionen till en angiven version.

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json" -X PUT https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version/<VERSION_ID>/activate
   ```

Mer information om hur du använder API:t Snabbt för att granska och hantera VCL finns i [Hantera VCL med API:t](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
