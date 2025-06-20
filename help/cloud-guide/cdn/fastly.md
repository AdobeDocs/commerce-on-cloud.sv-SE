---
title: Snabb översikt över tjänster
description: Se hur de snabbaste tjänsterna som ingår i Adobe Commerce i molninfrastrukturen hjälper er att optimera och säkra leveransåtgärder för era Adobe Commerce-sajter.
feature: Cloud, Configuration, Iaas, Paas, Cache, Security, Services
exl-id: 429b6762-0b01-438b-a962-35376306895b
source-git-commit: 3cef442321120d8ca813c760d2fd0435f4961235
workflow-type: tm+mt
source-wordcount: '1443'
ht-degree: 0%

---

# Snabb översikt över tjänster

>[!WARNING]
>
>Om du vill upprätthålla PCI-kompatibilitet för Adobe Commerce-webbplatser som distribueras på molnplattformen ska du konfigurera snabbt på startgrenen, Pro Production och Pro Staging-miljöerna. Om du använder Adobe Commerce i en headlessdistribution rekommenderar vi att du använder Fast för att cachelagra GraphQL-svar. Se [Cachelagra med Snabb](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly) i *GraphQL Developer Guide*.

Tillhandahåller snabbt följande tjänster för att optimera och skydda innehållsleveranser för Adobe Commerce i molninfrastrukturprojekt. Dessa tjänster ingår utan extra kostnad i Adobe Commerce molninfrastruktur.

- **CDN (Content Delivery Network)** - Slutbaserad tjänst som cachelagrar dina webbplatssidor, resurser, CSS med mera i de serverdelsdatacenter du konfigurerar. När kunderna kommer in på er webbplats och i era butiker går förfrågningarna igenom snabbt för att läsa in cachelagrade sidor snabbare. CDN-tjänsten har följande funktioner:

- **Cachehantering** - Cachelagra dina webbplatssidor, resurser, CSS med mera i datacenter som du konfigurerat för att minska bandbreddsbelastningen och kostnaderna

   - Använd [snabbt anpassade VCL-fragment](fastly-vcl-custom-snippets.md) (kompatibla med engelska 2.1) för att ändra hur cachning svarar på begäranden

   - Konfigurera [Stöd för GeoIP-tjänster](fastly-custom-cache-configuration.md#configure-geoip-handling)

   - [Tvinga okrypterade begäranden till TLS](fastly-custom-cache-configuration.md#force-tls)

   - [Anpassa inställningar för snabb timeout](fastly-custom-cache-configuration.md#extend-fastly-timeout) för att förhindra 503 svar på gruppåtgärdsbegäranden

   - Skapa [anpassade felsvarssidor](fastly-custom-response.md)

- **Säkerhet** - När du har aktiverat snabbtjänster för Adobe Commerce-webbplatser finns ytterligare säkerhetsfunktioner tillgängliga för att skydda dina webbplatser och ditt nätverk:

   - [Brandvägg för webbaserade program](fastly-waf-service.md) (WAF) - Brandvägg för hanterade webbprogram som tillhandahåller PCI-kompatibelt skydd för att blockera skadlig trafik innan den kan skada din produktion Adobe Commerce på webbplatser och nätverk i molnet. Tjänsten WAF finns endast i Pro- och Starter Production-miljöer.

   - [DoS-skydd (Distributed Denial of Service)](#ddos-protection) - Inbyggt DDoS-skydd mot vanliga attacker som Ping of Kill, Smurf-attacker och andra ICMP-baserade översvämningsattacker.

   - [SSL-/TLS-certifikat](fastly-configuration.md#provision-ssltls-certificates) - Snabbtjänsten kräver ett SSL-/TLS-certifikat för att kunna hantera säker trafik över HTTPS.

     Adobe Commerce tillhandahåller ett domänvaliderat Låt oss kryptera SSL-/TLS-certifikat för varje mellanlagrings- och produktionsmiljö. Adobe Commerce slutför domänvalidering och certifikatetablering under processen för snabb installation.

- **Insvepning av ursprung** - Förhindrar att trafik kringgår Fastly WAF och döljer IP-adresserna för dina ursprungliga servrar för att skydda dem mot direkt åtkomst och DDoS-attacker.

  Insvepning av ursprung är aktiverat som standard på Adobe Commerce i projekt för molninfrastruktur och Pro Production. Om du vill aktivera ursprungsinsvepning på Adobe Commerce i startproduktionsprojekt för molninfrastruktur skickar du en [Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Om du har trafik som inte kräver cachelagring kan du anpassa snabbtjänstkonfigurationen så att begäranden kan [kringgå snabbcachen](fastly-vcl-bypass-to-origin.md).

- **[Bildoptimering](fastly-image-optimization.md)** - Avlastar bildbearbetning och storleksändring från tjänsten Snabbt så att servrar kan bearbeta beställningar och konverteringar mer effektivt.

- **[Snabbt CDN- och WAF-loggar](../monitor/new-relic-service.md#new-relic-log-management)** - För Adobe Commerce i molninfrastrukturprojekt kan du använda New Relic Logs-tjänsten för att granska och analysera Fast CDN- och WAF-loggdata.

## Snabb CDN-modul för Magento 2

Snabba tjänster för Adobe Commerce i molninfrastruktur använder modulen [Snabbt CDN för Magento 2] som är installerad i följande miljöer: Pro Staging and Production, Starter Production (`master` grenen).

Vid första etableringen eller uppgraderingen av ditt Adobe Commerce-projekt installerar Adobe den senaste versionen av snabbuppdateringsmodulen i dina stagnings- och produktionsmiljöer. När modulen uppdateras snabbt får du meddelanden i Admin för dina miljöer. Adobe rekommenderar att du uppdaterar dina miljöer så att de använder den senaste versionen. Se [Uppgradera snabbt](fastly-configuration.md#upgrade-the-fastly-module).

## Snabb service av konto och autentiseringsuppgifter

Adobe Commerce i molninfrastrukturprojekt har inget dedikerat Fast-konto. Snabbtjänsten hanteras i ett centralt konto som är registrerat hos Adobe, och kontrollpanelen för hantering är bara tillgänglig för molnsupportteamet.

I stället har varje mellanlagrings- och produktionsmiljö unika snabbinloggningsuppgifter (API-token och tjänst-ID) för att konfigurera och hantera snabbtjänster från Commerce Admin. API:t Fast finns för avancerad hantering av tjänsten Fast, som kräver att autentiseringsuppgifterna används för att skicka dessa begäranden.

Under etableringen lägger Adobe till ditt projekt till snabbtjänstkontot för Adobe Commerce i molninfrastrukturen och lägger till snabbinloggningsuppgifterna i konfigurationen för mellanlagrings- och produktionsmiljöer. Se [Få snabbt inloggningsuppgifter](fastly-configuration.md#get-fastly-credentials).

### Ändra snabbt API-token

Skicka en Adobe Commerce Support-biljett för att utfärda en ny snabb API-tokenautentiseringsuppgift [om valideringen misslyckas/har upphört](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/error-when-validating-fastly-credentials) eller om du tror att den har komprometterats.

När du får en ny token uppdaterar du din förproduktionsmiljö så att den använder den nya token.

**Så här ändrar du autentiseringsuppgifter för snabb API-token**:

1. [Skicka in en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) och begär nya Snabb-API-autentiseringsuppgifter.

   Inkludera ditt Adobe Commerce i projekt-ID för molninfrastruktur och miljöer som kräver nya autentiseringsuppgifter.

1. När du har tagit emot den nya API-token uppdaterar du API-tokenvärdet i konfigurationen [Snabba autentiseringsuppgifter](fastly-configuration.md#test-the-fastly-credentials) i Admin eller från [[!DNL Cloud Console] miljövariablerna](../project/overview.md#configure-environment).

1. [Testa de nya autentiseringsuppgifterna](fastly-configuration.md#test-the-fastly-credentials).

1. När du har uppdaterat autentiseringsuppgifterna skickar du en Adobe Commerce Support-biljett för att ta bort den gamla API-token.

### Flera snabbkonton och tilldelade domäner

Du kan bara snabbt tilldela en apex-domän och associerade underdomäner till en snabbast-tjänst och ett konto. Om du har ett befintligt Fast-konto som länkar samma index och underdomäner som används för din Adobe Commerce-webbplats har du följande alternativ:

- Ta bort API:er och underdomäner från det befintliga kontot innan du begär snabb inloggning för Adobe Commerce i miljöer med molninfrastrukturprojekt. Se [Arbeta med domäner] i dokumentationen Snabbt.

  Använd det här alternativet om du vill länka apex-domänen och alla underdomäner till snabbtjänstkontot för Adobe Commerce i molninfrastrukturen.

- Skicka en Adobe Commerce-supportanmälan för att begära domändelegering så att API:er och underdomäner kan länkas till olika konton.

  Använd det här alternativet om du har en apex-domän som har flera underdomäner för Adobe Commerce- och icke-Adobe Commerce-webbplatser och du vill länka dessa underdomäner till olika Fastly-konton.

#### Begär domändelegering

*Scenario 1:*

Den överordnade domänen (`testweb.com` och `www.testweb.com`) är länkad till ett befintligt Fast-konto. Du har ett Adobe Commerce i molninfrastrukturprojekt konfigurerat med följande underdomäner: `mcstaging.testweb.com` och `mcprod.testweb.com`. Du vill inte flytta apex-domänen till snabbtjänstkontot för Adobe Commerce i molninfrastrukturen.

Skicka en [snabbsupportbiljett] med en begäran om att underdomänerna ska delegeras från det befintliga snabbkontot till snabbkontot för Adobe Commerce i molninfrastrukturen. Inkludera ditt Adobe Commerce projekt-ID i biljetten.

När delegeringen är klar kan dina projektunderdomäner läggas till i snabbtjänstkontot för Adobe Commerce i molninfrastrukturen. Se [Få snabbt inloggningsuppgifter](fastly-configuration.md#get-fastly-credentials).

*Scenario 2:*

Den överordnade domänen (`testweb.com` och `www.testweb.com`) är länkad till Adobe Commerce på kontot för snabb molninfrastruktur. Du vill hantera snabbtjänster för underdomänerna `service.testweb.com` och `product-updates.testweb.com` från ett annat snabbkonto.

Skicka en Adobe Commerce Support-biljett med en begäran om att underdomänerna ska delegeras från Adobe Commerce på kontot för molninfrastruktur Snabbt till kontot Snabbt. Inkludera tjänst-ID:t för snabbkontot i biljetten.

## DDoS-skydd

DDOS-skyddet är inbyggt i tjänsten Fast CDN. När du har aktiverat Snabba tjänster för dina Adobe Commerce-sajter filtrerar du snabbt all webb- och administratörstrafik för att upptäcka och blockera potentiella attacker.

- För attacker mot lager 3 eller 4 filtrerar tjänsten Snabbt bort trafik baserat på port och protokoll, och undersöker bara HTTP- eller HTTPS-begäranden. ICMP, UDP och andra nätverksinitierade attacker släpps vid vår nätverkskant. Detta omfattar speglingar och amplifieringsattacker, som använder UDP-tjänster som SSDP eller NTP. Genom att tillhandahålla denna skyddsnivå kan vi effektivt blockera flera vanliga attacker som ping of Döden, Smurf-attacker och andra ICMP-baserade översvämningar.

  Hanterar snabbt attacker på TCP-nivå i cache-lagret. Denna strategi ger den skala och kontext per klient som krävs för att hantera en SYN-översvämningsattack och dess många varianter, inklusive TCP-stacken, resursattacker och TLS-attacker inom Fastly-system.

- Skyddar dig snabbt mot Layer 7-attacker. Om din butik har prestandaproblem och du misstänker en Layer 7 DDoS-attack skickar du en Adobe Commerce Support-anmälan. Adobe kan skapa och tillämpa anpassade regler på tjänsten Snabbt för att undersöka och filtrera bort skadliga förfrågningar baserat på huvud, nyttolast eller en kombination av attribut som identifierar attacktrafiken. Se [Leta efter DDoS-attacker] och [Blockera skadlig trafik] i *Adobe Commerce Help Center*.

<!--Link definitions-->

[Caching with Fastly]: https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly

[Söka efter DDoS-attacker]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli.html

[Snabb CDN-modul för Magento 2]: https://github.com/fastly/fastly-magento2

[Snabbt supportärende]: https://docs.fastly.com/products/support-description-and-sla#support-requests

[Blockera skadlig trafik]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level.html

[Arbeta med domäner]: https://docs.fastly.com/en/guides/working-with-domains
