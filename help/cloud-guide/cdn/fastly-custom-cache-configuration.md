---
title: Anpassa cachekonfigurationen
description: Lär dig hur du granskar och anpassar inställningarna för cachekonfigurationen när tjänsten Snabbt har installerats.
feature: Cloud, Configuration, Iaas, Cache
exl-id: f6901931-7b3f-40a8-9514-168c6243cc43
source-git-commit: 551a00932165dd1c0a876b8151ba14752ceac802
workflow-type: tm+mt
source-wordcount: '1953'
ht-degree: 0%

---

# Anpassa cachekonfigurationen

Granska och anpassa inställningarna för cachekonfigurationen när du har konfigurerat och testat tjänsten Snabbt i dina miljö för staging och produktion. Du kan till exempel uppdatera inställningarna för att göra det möjligt för TLS att omdirigera HTTP-begäranden till Fast, uppdatera rensningsinställningar och aktivera grundläggande autentisering för att lösenordsskydda webbplatsen under utvecklingen.

I följande avsnitt finns en översikt och instruktioner för hur du konfigurerar vissa cacheinställningar. Mer information om tillgängliga konfigurationsalternativ finns i dokumentationen för [Snabbt CDN-modul för Magento 2](https://github.com/fastly/fastly-magento2/tree/master/Documentation).

## Tvinga TLS

Tillhandahåller snabbt alternativet _Tvinga TLS_ för omdirigering av okrypterade begäranden (HTTP) till snabbt. När mellanlagrings- eller produktionsmiljön har etablerats med ett [giltigt SSL/TLS-certifikat](fastly-configuration.md#provision-ssltls-certificates) kan du uppdatera snabbkonfigurationen för butiken för att aktivera alternativet Tvinga TLS. Se handboken [Tvinga TLS snabbt](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/FORCE-TLS.md) i _Snabb CDN-modulen för Magento 2_ .

>[!NOTE]
>
>Att aktivera alternativet Force TLS rekommenderas för Adobe Commerce i molninfrastrukturbutiker.

## Utöka tidsgränsen snabbt

I snabbtjänstkonfigurationen anges en standardtidsgräns på 180 sekunder för HTTPS-begäranden till administratören. Alla förfrågningar som överskrider tidsgränsen returnerar ett 503-fel. Det innebär att du kan få 503 fel som svar på förfrågningar som kräver lång bearbetning eller när du försöker utföra gruppåtgärder.

Om du vill slutföra gruppåtgärder som tar längre tid än 3 minuter ändrar du _Admin path timeout_ value_ för att förhindra 503 fel.

>[!NOTE]
>
>Om du har angett en anpassad Admin Path-slutpunkt i fältet **Anpassad administratörssökväg** i **Lager** > **Konfiguration** > **Avancerat** > **Admin** > **Admin Bas-URL** måste du också ange [ADMIN_URL-variabeln](../environment/variables-admin.md#change-the-admin-url) i -miljön till samma värde. Om inställningarna inte är samma kommer timeout-värdet inte att fungera.
>
>Mer information om hur du utökar parametrar för snabb timeout för andra än Admin i snabbgränssnittet finns i [Öka tidsgränser för långa jobb](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-INCREASE-TIMEOUTS-LONG-JOBS.md).

**Så här utökar du den snabba tidsgränsen för administratören**:

{{admin-login-step}}

1. Klicka på **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System** och expandera **Helsidescache**.

1. Expandera _Avancerad konfiguration_ i avsnittet **Snabbt konfigurering**.

1. Ange tidsgränsen **för** Admin-sökvägen i sekunder. Värdet får inte vara längre än 10 minuter (600 sekunder).

>[!NOTE]
>
>Konfigurationsinställningen **_Admin path timeout_** styr inte timeout-värden utanför Adobe Commerce, som Fastly WAF timeout. Om du vill justera tidsgränsen för Fast WAF måste du öppna en Adobe Support-biljett och uppdatera den i tjänsten Snabbt.

1. Klicka på **Spara konfiguration** överst på sidan.

1. När sidan har lästs in igen väljer du **Överför VCL till snabbast** i avsnittet _Snabbkonfiguration_.

Hämtar snabbt Admin-sökvägen för generering av VCL-filen från konfigurationsfilen `app/etc/env.php`.

## Konfigurera rensningsalternativ

Du kan snabbt välja mellan flera olika typer av rensningsalternativ på sidan för hantering av Magento-cache, bland annat alternativ för att rensa produktkategori, produktresurser och innehåll. När det här alternativet är aktiverat letar programmet efter händelser som automatiskt tömmer cacheminnen. Om du inaktiverar ett rensningsalternativ kan du rensa bort cacheminnen manuellt när du har slutfört uppdateringarna via sidan Cachehantering.

Rensningsalternativen är:

- **Töm kategori**-Tar bort produktkategoriinnehåll (inte produktinnehåll) när du lägger till och uppdaterar en enskild produkt. Du kanske vill hålla den inaktiverad och aktivera rensning av produkten, vilket tömmer produkter och produktkategorier.
- **Töm produkt**- Tar bort allt innehåll i produkt och produktkategori när en enskild ändring sparas i en produkt. Det kan vara praktiskt att aktivera rensning av produkter för att omedelbart få uppdateringar till kunderna när de ändrar ett pris, lägger till ett produktalternativ och när produktlagret är slut.
- **Rensa CMS-sida**-Tar bort sidinnehåll när sidor uppdateras och läggs till i Adobe Commerce CMS. Du kanske till exempel vill rensa när du uppdaterar villkoren eller returprincipen. Om du sällan gör dessa ändringar kan du inaktivera automatisk rensning.
- **Mjuk tömning**-Anger ändrat innehåll till inaktivt och tömt enligt inaktuell tidpunkt. Förutom tidsförskjutningen får kunderna inaktuellt innehåll medan de snabbt uppdaterar innehållet i bakgrunden.

![Konfigurera rensningsalternativ](../../assets/cdn/fastly-purge-options.png)

**Så här konfigurerar du alternativen för snabbtömning**:

1. Expandera _Avancerad konfiguration_ i avsnittet **Snabbt konfigurering** för att visa rensningsalternativen.

1. För varje rensningsalternativ väljer du **Ja** om du vill aktivera automatisk tömning eller **Nej** om du vill inaktivera automatisk tömning.

   När du inaktiverar ett rensningsalternativ måste du manuellt rensa cacheminnet för den kategorin från sidan _Cachehantering_.

1. Klicka på **Spara konfiguration** överst på sidan.

1. När sidan har lästs in igen väljer du **Överför VCL till snabbast** i avsnittet _Snabbkonfiguration_.

Mer information finns i [Alternativ för snabb konfiguration](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/CONFIGURATION.md#further-configuration-options).

## Konfigurera GeoIP-hantering

I modulen Snabbt finns GeoIP-hantering som automatiskt omdirigerar besökare eller en lista över butiker som matchar deras landskod. Om du redan använder ett tillägg för GeoIP-hantering kan du behöva verifiera funktionerna med alternativen Snabbt.

**Så här konfigurerar du GeoIp-hantering**:

{{admin-login-step}}

1. Klicka på **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System** och expandera **Helsidescache**.

1. Expandera _Avancerad konfiguration_ i avsnittet **Snabbt konfigurering**.

1. Bläddra nedåt och välj **Yes** för att **aktivera GeoIP**. Ytterligare konfigurationsalternativ visas.

1. För GeoIP-åtgärd väljer du om besökaren automatiskt omdirigeras med **Omdirigering** eller om en lista med butiker att välja från finns i **dialogrutan**.

1. För **Landsmappning** väljer du **Lägg till** om du vill ange en landskod på två bokstäver som ska mappas med en specifik Adobe Commerce-butik från en lista.

   ![Lägg till GeoIP-landskartor](/help/assets/cdn/fastly-geo-code.png)

1. Klicka på **Spara konfiguration** överst på sidan.

1. När sidan har lästs in igen väljer du **Överför VCL till Snabbt** i avsnittet _Snabbt konfigurering_.

>[!NOTE]
>
>Den nuvarande implementeringen av Adobe Commerce Fast GeoIP-modulen stöder inte omdirigeringar mellan flera webbplatser.

Ger snabbt även en serie [geolocation-relaterade VCL-funktioner](https://developer.fastly.com/reference/vcl/variables/geolocation/) för anpassad geopositioneringskodning.

## Aktivera Edge-moduler snabbt

Edge Modules är ett flexibelt ramverk som gör det möjligt att definiera gränssnittskomponenter och tillhörande VCL-kod via en mall. Dessa moduler gör det enkelt att anpassa och utöka konfigurationen för tjänsten Snabb via användargränssnittet i stället för att använda anpassade VCL-fragment.

Med Edge-moduler kan du aktivera specifika funktioner som CORS-huvuden, omskrivningar av Cloud Sitemap och konfigurera integrering mellan din Adobe Commerce-butik och andra CMS-system eller backend-system.

Aktivera alternativet _Aktivera Edge snabbmoduler_ om du vill öppna menyn Edge Modules för att visa, konfigurera och hantera tillgängliga moduler. Se [Snabbt Edge-moduler](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULES.md) i dokumentationen för modulen Snabbt CDN.

## Konfigurera bakändar och origin-skydd

Med backend-inställningarna kan du finjustera prestanda snabbt med skärmsläckning och timeout. En _serverdel_ är en specifik plats (IP eller domän) med konfigurerad origin-skydd och timeout-inställningar för kontroll och tillhandahållande av cachelagrat innehåll.

_Ursprungsskydd_ dirigerar alla begäranden för din butik till en viss POP (Point of Presence). När en begäran tas emot söker POP efter cachelagrat innehåll och skickar det. Om den inte cachelagras fortsätter den till sköld-POP och sedan till origin-servern som cachelagrar innehållet. Sköldarna minskar trafiken direkt till origo.

Standardvärdet för Fast VCL-kod anger standardvärden för Origin-avskärmning och timeout för din Adobe Commerce på molninfrastruktursajter. I vissa fall kan du behöva ändra standardvärdena. Om du till exempel får TTFB-fel (Time to First Byte) kan du behöva justera värdet _för den första bytetidsgränsen_.

>[!NOTE]
>
>Om webbplatsen kräver funktioner som levereras via en backend-integrering som [Wordpress](fastly-vcl-wordpress.md) kan du anpassa snabbtjänstkonfigurationen för att lägga till backend-objektet och hantera omdirigeringar från din Adobe Commerce-butik till Wordpress. Mer information finns i [Snabbt Edge-moduler - Annan CMS/Backend-integrering](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) i dokumentationen för modulen Snabbt.

**Så här granskar du konfigurationen för backend-inställningarna**:

{{admin-login-step}}

1. Klicka på **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System** och expandera **Helsidescache**.

1. Expandera avsnittet **Snabb konfiguration**.

1. Expandera **Backend-inställningarna** och markera kugghjulet för att kontrollera standardbackend-objektet. En modal öppnas som visar aktuella inställningar med alternativ för att ändra dem.

   ![Ändra bakände](../../assets/cdn/fastly-backend.png)

1. Markera platsen **Sköld** (eller datacentret).

   Med standardkonfigurationen Snabbt för ditt projekt anges den plats som ligger närmast din molntjänstregion. Om du behöver ändra den väljer du en plats som ligger nära standardplatsen.

1. Ändra timeoutvärdena (i mikrosekunder) för anslutningen till skölden, tiden mellan byte och tiden för den första byten. Vi rekommenderar att du behåller standardinställningarna för timeout.

1. Du kan också välja att **aktivera serverdelen och skölden när du har redigerat eller sparat**.

1. Klicka på **Överför** för att spara ändringarna och överföra dem till snabbservrarna.

1. Välj **Spara konfiguration** i Admin.

Mer information finns i handboken [Backend-inställningar](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/Guides/BACKEND-SETTINGS.md) i dokumentationen för snabbmodulen.

## Grundläggande autentisering

Grundläggande autentisering är en funktion som skyddar alla sidor och resurser på din webbplats med ett användarnamn och lösenord.

Adobe **rekommenderar inte** att du aktiverar grundläggande autentisering i din produktionsmiljö. Du kan konfigurera den på mellanlagring för att skydda din plats under utvecklingsprocessen. Se [Grundläggande autentiseringshandbok](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md) i dokumentationen för snabbkorrigeringsmodulen.

Om du lägger till användaråtkomst och aktiverar grundläggande autentisering på mellanlagring, kan du fortfarande komma åt administratören utan att ytterligare autentiseringsuppgifter krävs.

>[!NOTE]
>
>Kontrollera **inte** [!UICONTROL Enable HTTP access control] i molnkonsolen för miljöer där Fastly är aktiverat (till exempel för förproduktionsmiljöer eller icke-aktiva produktionsmiljöer). Om åtkomstkontrollen är konfigurerad på det här sättet kan användare som tidigare haft åtkomst fortfarande komma åt webbplatsen om deras inloggningsuppgifter cachas av Fastly, även efter att åtkomsten har återkallats.

## Skapa anpassade VCL-fragment

Stöd för en anpassad version av VCL (Varnish Configuration Language) för att anpassa konfigurationen av tjänsten snabbt. Du kan till exempel tillåta, blockera eller omdirigera åtkomst för specifika användare eller IP-adresser med hjälp av VCL-kodblock med edge- och Access Control List-ordlistor (ACL).

Instruktioner om hur du skapar anpassade VCL-fragment, kantordlistor och ACL-listor finns i [Anpassade, snabbt VCL-fragment](fastly-vcl-custom-snippets.md).

>[!NOTE]
>
>Innan du lägger till anpassad VCL-kod, kantordlistor och åtkomstkontrollistor i din snabbmodulskonfiguration måste du kontrollera att cachelagringstjänsten Snabbt fungerar med standardkonfigurationen. Se [Konfigurera snabbt](fastly-configuration.md).

## Hantera domäner

För både Starter- och Pro-projekt kan du använda alternativet [!UICONTROL Domains] för att lägga till och hantera Snabb domänkonfiguration för din butik.

- För Starter-projekt går du till Project URL på fliken [!UICONTROL Domains] i [!DNL Cloud Console] för att lägga till din projekt-URL.

- För Pro-projekt skickar du en [Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) för att lägga till domänen i din molnprojektskonfiguration. Supportteamet uppdaterar även kontokonfigurationen för Adobe Commerce Fast för att lägga till domänen.

**Så här hanterar du snabb domänkonfiguration från administratören**:

{{admin-login-step}}

1. Välj **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System** och expandera **Helsidescache**.

1. Välj _Domäner_ i avsnittet Admin **Snabbt konfiguration**.

1. Klicka på **Hantera domäner** för att öppna sidan Domäner.

1. Lägg till namnen på den översta nivån och underdomänerna för butikerna i molnmiljön.

   Du kan bara ange domäner som redan har lagts till i din konfiguration för molninfrastruktur.

   ![Lägg till snabb domänkonfiguration för Starter](../../assets/cdn/fastly-starter-activate-domain.png)

1. Klicka på **Aktivera** för att uppdatera Snabb domänkonfiguration.

>[!NOTE]
>
>Om samma domän har konfigurerats på ett annat snabbkonto måste du skicka en Adobe Commerce-supportanmälan för att begära domändelegering innan du kan lägga till domänen i Adobe Commerce. Se [Flera snabbkonton och tilldelade domäner](fastly.md#multiple-fastly-accounts-and-assigned-domains).

## Aktivera underhållsläge

Använd alternativet _Underhållsläge_ om du vill tillåta administrativ åtkomst till din plats från angivna IP-adresser samtidigt som du returnerar en felsida för alla andra begäranden.

**Så här aktiverar du underhållsläge med administrativ åtkomst**:

1. Öppna avsnittet _Snabb konfiguration_ i Admin.

1. I avsnittet _Edge ACL_ uppdaterar du `maint_allow`-åtkomstkontrollistan med de administrativa IP-adresser som har åtkomst till din butik när den är i underhållsläge.

   ![Uppdatera IP-underhållsläge tillåtelselista](../../assets/cdn/fastly-maint-allowlist.png)

1. Välj _Aktivera underhållsläge_ i avsnittet **Underhållsläge**.

   När du har aktiverat underhållsläge blockeras all trafik utom förfrågningar från IP-adresserna i `maint_allowlist` ACL. Du kan uppdatera `maint_allowlist` om du vill ändra IP-adresserna i åtkomstkontrollistan.

   Detaljerade konfigurationsinstruktioner finns i [underhållslägesguiden](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/MAINTENANCE-MODE.md) i dokumentationen för Fast CDN för Magento 2-modulen.
