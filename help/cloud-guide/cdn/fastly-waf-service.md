---
title: Brandvägg för webbaserade program (WAF)
description: Läs om hur tjänsten Fastly WAF identifierar, loggar och blockerar trafik för skadliga förfrågningar innan den kan skada Adobe Commerce nätverk eller webbplatser.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# Brandvägg för webbaserade program (WAF)

WAF-tjänsten (web application firewall) för Adobe Commerce i molninfrastruktur tillhandahålls av Fastly och identifierar, loggar och blockerar trafik för skadliga förfrågningar innan den kan skada dina webbplatser eller ditt nätverk. Tjänsten WAF finns endast i produktionsmiljöer.

WAF-tjänsten ger följande fördelar:

- **PCI-kompatibilitet** - WAF-aktivering säkerställer att Adobe Commerce-butiker i produktionsmiljöer uppfyller PCI DSS 6.6-säkerhetskrav.
- **WAF standardpolicy** - WAF standardpolicy, som konfigurerats och underhålls av Fastly, innehåller en samling säkerhetsregler som är skräddarsydda för att skydda dina Adobe Commerce webbprogram från en mängd olika attacker, inklusive injektionsangrepp, skadliga indata, serveröverskridande skriptning (cross-site scripting), dataextrahering, HTTP-protokollöverträdelser och andra [OWASP Top Ten](https://owasp.org/www-project-top-ten/) -säkerhetshot.
- **WAF onboarding and enablement** - Adobe distribuerar och aktiverar WAF standardpolicy i din produktionsmiljö inom 2 till 3 veckor efter att etableringen är klar.
- **Drifts- och underhållssupport**—
   - Adobe och Snabbt konfigurera och hantera loggar, regler och varningar för WAF-tjänsten.
   - Adobe utreder supportärenden för WAF som blockerar legitim trafik som Priority 1-frågor.
   - Automatiserade uppgraderingar av WAF-versionen säkerställer omedelbar täckning för nya eller framtida utnyttjanden. Se [WAF underhåll och uppgraderingar](#waf-maintenance-and-updates).

>[!TIP]
>
>Mer information om hur du underhåller PCI-kompatibilitet för din Adobe Commerce i molninfrastrukturbutiker finns i [PCI-kompatibilitet](https://business.adobe.com/products/magento/pci-compliance.html).

## Aktivera WAF

Adobe aktiverar WAF-tjänsten på nya konton inom 2 till 3 veckor efter det att etableringen är klar. WAF implementeras via tjänsten Fast CDN. Du behöver inte installera eller underhålla någon maskin- eller programvara.

>[!NOTE]
>
>Innan du kan använda WAF-tjänsten måste all extern trafik till Adobe Commerce i molninfrastrukturprojekt gå via tjänsten Snabbt. Se [Konfigurera snabbt](fastly-configuration.md).

## Så här fungerar det

WAF-tjänsten integreras med Fastly och använder cachelogiken i tjänsten Fast CDN för att filtrera trafiken på de snabbt globala noderna. Vi aktiverar WAF-tjänsten i din produktionsmiljö med en WAF-standardpolicy som baseras på [ModSecurity Rules från Trustwave SpiderLabs](https://github.com/owasp-modsecurity/ModSecurity) och OWASP Top Ten security HoHoperes.

WAF-tjänsten inspekterar HTTP- och HTTPS-trafik (GETS- och POST-förfrågningar) mot WAF-regeluppsättningen och blockerar trafik som är skadlig eller inte uppfyller specifika regler. Tjänsten undersöker endast ursprunglig trafik som försöker uppdatera cachen. Därför stoppar vi de flesta attackerna på snabbcachen och skyddar era urkunder från skadliga attacker. Genom att endast bearbeta ursprungstrafik bevarar WAF-tjänsten cacheprestanda, vilket innebär att endast uppskattningsvis 1,5 millisekunder till 20 millisekunder fördröjning för varje begäran som inte cachelagrats läggs till.

## Felsöka blockerade begäranden

När WAF-tjänsten är aktiverad undersöker den all webb- och administratörstrafik mot WAF-reglerna och blockerar alla webbförfrågningar som utlöser en regel. När en begäran blockeras ser den som gjorde begäran en standardfelsida för `403 Forbidden` som innehåller ett referens-ID för blockeringshändelsen.

![WAF-felsida](../../assets/cdn/fastly-waf-403-error.png)

Du kan anpassa den här felsvarssidan från Admin. Se [Anpassa WAF svarssida](fastly-custom-response.md#customize-the-waf-error-page).

Om din Adobe Commerce-administratörssida eller butik returnerar en `403 Forbidden`-felsida som svar på en giltig URL-begäran, skickar du en [Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Kopiera referens-ID:t från felsvarssidan och klistra in det i biljettbeskrivningen.

Om du vill identifiera WAF svar på en viss begäran med New Relic kan du läsa följande:

- `Agent_response` - Anger WAF-svarskoden (`200` betyder bra och `406` betyder blockerad)
- `sigsci` taggar - Taggar begäran till en viss signalvetenskapstagg baserat på typen av begäran

## Underhåll och uppdateringar av WAF

Uppdaterar och lanserar snabbt patchar för nya CVE:er/mallade regler baserat på regeluppdateringar från kommersiella tredje parter, snabbforskning och öppna källor. Uppdaterar snabbt de publicerade reglerna till en policy efter behov eller när ändringar av reglerna är tillgängliga från deras respektive källor. Fast kan också lägga till regler som matchar de publicerade regelklasserna i WAF-instansen för alla tjänster när WAF-tjänsten har aktiverats. Uppdateringarna säkerställer omedelbar täckning för nya eller föränderliga explosioner.

Hantera uppdateringsprocessen Adobe och Snabb för att säkerställa att nya eller ändrade WAF-regler fungerar effektivt i produktionsmiljön innan uppdateringarna distribueras i blockeringsläge.

## Problem

Om du upptäcker att WAF blockerar legitima förfrågningar är dessa ofta falska positiva och måste kringgås eller ha en lösning implementerad på WAF-tjänsten. Skicka en supportanmälan och inkludera den URL som påverkas, exakta steg för att återge felet och felreferensen i textformulär (till skillnad från en skärmbild) för att undvika transkriberingsfel.

## Begränsningar

WAF standardtjänst som drivs av Fastly har inte stöd för följande funktioner:

- Skydd mot skadlig kod eller robotskydd - Överväg att använda [åtkomstkontrollistor](./fastly-vcl-allowlist.md) eller en tredjepartstjänst.
- Hastighetsbegränsning - Se [Hastighetsbegränsning](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) i dokumentationen Snabbt, eller se [Hastighetsbegränsning](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/) i säkerhetsavsnittet för _Commerce Web API_ .
- Konfigurerar en loggningsslutpunkt för kund - Se [PrivateLink-tjänsten](../development/privatelink-service.md) som ett alternativ.

Med tjänsten WAF kan du blockera eller tillåta trafik baserat på IP-adresser. Du kan lägga till åtkomstkontrollistor (ACL) och anpassade VCL-kodfragment till tjänsten Snabbt för att ange IP-adresser och VCL-logik för blockering eller tillåtelse av trafik. Se [Anpassade snabbt VCL-fragment](fastly-vcl-custom-snippets.md).

Filtrering för TCP-, UDP- eller ICMP-begäranden stöds inte av WAF-tjänsten. Den här funktionen tillhandahålls dock av det inbyggda DDoS-skyddet som ingår i tjänsten Fast CDN. Se [DDoS-skydd](fastly.md#ddos-protection).
