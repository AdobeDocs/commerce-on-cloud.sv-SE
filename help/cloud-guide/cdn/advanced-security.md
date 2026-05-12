---
title: Adobe Commerce Advanced Security
description: Lär dig hur Advanced Security lägger till robothantering, avancerad hastighetsbegränsning och DDoS-skydd i Layer 7 till Adobe Commerce i molninfrastrukturen.
feature: Cloud, Configuration, Security
source-git-commit: 8a7c1c297092fdf2b75d22ce99c360c85eac0495
workflow-type: tm+mt
source-wordcount: '1986'
ht-degree: 0%

---

# [!DNL Adobe Commerce Advanced Security]

[!DNL Adobe Commerce Advanced Security] är en produkt som fungerar med [!DNL Adobe Commerce on Cloud Infrastructure] för att din onlinebutik ska vara snabb, tillgänglig och säker. Detta kan hjälpa till att skydda intäkterna, minska driftstoppen och upprätthålla kundens förtroende vid trafiktoppar och automatiska attacker.

[!DNL Adobe Commerce on Cloud Infrastructure] innehåller inbyggt [Layer 3- och 4-DDoS-skydd](./fastly.md#ddos-protection) och en [Brandvägg för webbaserade program (WAF)](./fastly-waf-service.md). Under den [delade ansvarsmodellen](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility) är DoS-identifiering för Layer 7, robotskydd och proaktiv IP-blockering handlaransvar, som [!DNL Adobe Commerce Advanced Security] är utformat för att åtgärda.

[!DNL Advanced Security] utökar skyddet för butiksservrar genom säkerhetsfunktionerna i Fasttoner, som ger både hantering och avancerade hastighetsbegränsningar samt Layer 7 DDoS-skydd som en del av en enastående plattform som kombinerar skalbarhet, prestanda och säkerhet i nätverkets utkant.

>[!NOTE]
>
>[!DNL Advanced Security] är endast tillgängligt för [!DNL Adobe Commerce on Cloud Infrastructure] (PaaS)-projekt.

## Kärnfunktioner

[!DNL Adobe Commerce Advanced Security] innehåller följande extra skydd:

- **[Punkthantering](https://docs.fastly.com/products/bot-management)** - Identifierar och minskar oönskade robotaktiviteter i dina webbprogram. Tjänsten Bot Management skiljer mellan berättigade botar (sökmotorcrawler, bots för sociala medier) och skadliga, vilket ger realtidsklassificering i nätverkets utkant med möjlighet att blockera, tillåta, utmana eller begränsa trafiken.

- **[DDoS-skydd](https://docs.fastly.com/products/fastly-ddos-protection)** - Ger Layer 7 (programlager) DDoS-skydd utöver det befintliga Layer 3- och 4-skyddet som ingår i alla [!DNL Adobe Commerce on Cloud Infrastructure] -projekt. DDoS-skyddstjänsten absorberar volymetriska attacker i stor skala och garanterar kontinuerlig programtillgänglighet vid distribuerade denial of service-händelser (DDoS), vilket skyddar intäkterna under trafiktoppar.

- **[Avancerad hastighetsbegränsning](https://www.fastly.com/documentation/guides/next-gen-waf/rules/working-with-advanced-rate-limiting-rules/)** - Tillhandahåller konfigurerbara hastighetsbegränsande regler som skyddar specifika URL:er, API-slutpunkter och programresurser från missbruk. Tjänsten Advanced Rate Limiting går längre än den [grundläggande hastighetsbegränsning](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) som är tillgänglig via Fast CDN-modulen för att rikta in sig på specifika trafikmönster och attackera vektorer, vilket minskar infrastrukturens belastning och molnkostnaderna.

>[!NOTE]
>
>[!DNL Advanced Security] konfigurationer kräver för närvarande att du skickar en supportanmälan. Självbetjäningskonfiguration via administratörsgränssnittet planeras för en framtida version. Mer information finns i [Begäran [!DNL Advanced Security]](#request-advanced-security).

## Upptäckt av hot

[!DNL Advanced Security] skyddar butiker från en rad automatiska hot och hot i programlager.

![Avancerad säkerhetsposition i Adobe Commerce säkerhetsstack](../../assets/advanced-security.svg)

### Punktstyrt missbruk

- **Autentiseringsuppgifterna stoppas** - Automatiska försök görs att logga in med stulna autentiseringsuppgifter från dataintrång.
- **Kontoövertagande** - Startar som försöker få obehörig åtkomst till kundkonton.
- **Avbrott vid skapande av konto** - Automatiserad generering av falska konton för bedrägeri eller missbruk.
- **Korttestning** - Startar som testar stulna kreditkortsnummer mot din betalningsprocessor.
- **Klippning av innehåll** - Automatisk extrahering av produktdata, priser eller innehåll från din butik.
- **Inventariehoarding** - Bäddar in produkter i kundvagnar för att förhindra berättigade inköp.

### Hantering av AI-robotar

- **AI-crawlidentifiering** - Identifierar och hanterar AI-crawler som skrapar innehåll för att träna stora språkmodeller utan samtycke.
- **AI-hämtningskontroll** - Kontrollerar AI-hämtare som används i AI-sökresultat i realtid.
- **Konfigurerbara AI-robotprinciper** - Skiljer mellan verifierade och misstänkta AI-botar med konfigurerbara signaltyper för policytillämpning.

### Programskiktsattacker

- **Layer 7 DDoS-attacker** - Distribuerade attacker mot programlagret som åsidosätter inbyggda Layer 3- och 4-skydd. [!DNL Advanced Security] absorberar de volymetriska attackerna vid kanten innan de når dina ursprungliga servrar.
- **URL- och API-missbruk** - Angriper riktade URL:er eller API-slutpunkter som sprids över ett stort antal IP-adresser, där individuella IP-blockeringar inte är effektiva.
- **Cache-busting-attacker** - Begäranden med manipulerade frågeparametrar som utformats för att kringgå CDN-cachelagring och överbelasta den ursprungliga servern.

### Ytterligare funktioner

- **Dynamiska utmaningar** - Tilldelar automatiskt den optimala utmaningen till misstänkt trafik. Använder PAT-token (Private Access Tokens) för att smidigt validera en del av begäranden utan att påverka användarupplevelsen.
- **Felsökningsteknik** - Åtgärdar försök till övertagande av konto genom att returnera falsk information till angripare, vilket minskar deras attacker samtidigt som deras möjlighet att arbeta i stor skala störs.

## Välja rätt skydd

Använd följande vägledning för att avgöra om [!DNL Advanced Security] är rätt lösning för ditt butiksskydd, eller om befintliga skydd eller alternativa lösningar är lämpligare.

### När [!DNL Advanced Security] ska användas

Följande scenarier är bäst åtgärdade med [!DNL Advanced Security]:

| Scenario | Så här hjälper [!DNL Advanced Security] |
|---|---|
| Din webbplats drabbas av robotdrivna attacker, som till exempel kreditstängning, innehållskrapning eller lagervärdning | Bot Management identifierar och minskar automatiserade hot innan de når ditt program |
| Du behöver Layer 7 DDoS-skydd utöver den inbyggda Layer 3- och 4-täckningen | DDoS-skyddet absorberar programskiktsattacker som kringgår skydd på nätverksnivå |
| Specifika URL:er eller API-slutpunkter är inriktade på trafik med stora volymer som inte kan blockeras av IP | Avancerad hastighetsbegränsning ger detaljerade kontroller för specifika slutpunkter och trafikmönster |
| Du vill hantera AI-crawler och -hämtare som har åtkomst till ditt butiksinnehåll | I punkthanteringen ingår konfigurerbara principer för identifiering och tillämpning av AI-robotar |
| Du behöver en säkerhetslösning som stöds av Adobe och som är integrerad med ditt befintliga Fast CDN | [!DNL Advanced Security] körs på samma fastkantsplattform som redan betjänar din storefront |

### När befintliga skydd ska användas

Följande scenarier hanteras bäst med befintliga skydd:

| Scenario | Rekommenderad metod |
|---|---|
| En enda IP-adress eller en liten uppsättning identifierbara IP-adresser flödar din webbplats med förfrågningar | Blockera IP-adresserna med Commerce Admin eller Fast API. Använd det inbyggda [Layer 3/4 DDoS-skyddet](./fastly.md#ddos-protection) och befintliga VCL-fragment för [IP blockeringslista &#x200B;](./fastly-vcl-blocking.md). |
| Du måste blockera SQL-injektion, XSS-skriptning (cross-site scripting) eller andra hot från OWASP Top Ten | Den medföljande [WAF-tjänsten](./fastly-waf-service.md) blockerar dessa hot automatiskt. |
| Dina DDoS-attackmönster kan kontrolleras med grundläggande VCL-blockeringsregler | Använd de befintliga [anpassade VCL-fragment](./fastly-vcl-custom-snippets.md) som redan finns i Adobe Commerce. |

### När alternativa skydd ska användas

Följande scenarier är bäst åtgärdade med alternativa skydd som kan komplettera [!DNL Advanced Security]:

| Scenario | Rekommenderad metod |
|---|---|
| Du behöver göra en bedömning av bedrägeri på transaktionsnivå eller förhindra betalningsbedrägeri | Använd en särskild plattform för förebyggande av bedrägerier. [!DNL Advanced Security] skyddar på edge-nätverksnivå och utvärderar inte enskilda betalningstransaktioner. |
| Du behöver hantering av identitet och åtkomst (IAM) | Implementera en dedikerad IAM-lösning. Användarautentisering och sessionshantering behåller kundens ansvar. |
| Du behöver en statisk eller dynamisk säkerhetstestning av program (SAST/DAST) | Använd dedikerade säkerhetstestverktyg. Sårbarhetskontroll på kodnivå tillhandahålls inte. |
| Du behöver omfattande API-säkerhet utöver hastighetsbegränsningen (till exempel schemavalidering eller API-gatewayfunktioner) | Överväg en dedikerad API-säkerhetsplattform. |
| Du behöver verktyg för regelefterlevnad som PCI-skanning eller SOC-rapportering | Använd dedikerade efterlevnadshanteringsverktyg. |

>[!TIP]
>
>Om du för närvarande använder en tredjepartsleverantör av robotskydd kan du minska komplexiteten i driften genom att konsolidera till [!DNL Advanced Security] och undvika inkonsekvent säkerhetstäckning mellan olika leverantörer. Kontakta ditt Adobe-kontoteam för att utvärdera [!DNL Advanced Security] för ditt projekt.

## Placering av säkerhetsstackar

[!DNL Advanced Security] passar in i Adobe Commerce större säkerhetsarkitektur som ett extra lager av kantbaserat skydd. Det fungerar tillsammans med - och ersätter inte - de DDoS-skydd för WAF och Layer 3/4 som redan ingår i [!DNL Adobe Commerce on Cloud Infrastructure]. I följande avsnitt klargörs hur det rör sig om befintliga skydd och det ansvar som kvarstår hos kunden.

### Inkluderade skydd

[!DNL Adobe Commerce on Cloud Infrastructure] innehåller följande säkerhetsfunktioner:

- **[Brandvägg för webbaserade program (WAF)](./fastly-waf-service.md)** - Hanterat skydd mot SQL-injektioner, XSS (Cross-Site Scripting) och andra Open Web Application Security Project (OWASP) De 10 viktigaste hoten. Finns endast i produktionsmiljöer.
- **[Layer 3- och 4-DDoS-skydd](./fastly.md#ddos-protection)** - Inbyggt skydd mot nätverksnivåattacker som SYN-översvämningar, UDP-översvämningar, ICMP-baserade attacker och TCP-nivåattacker. Aktiveras automatiskt med Fast CDN.
- **[SSL-/TLS-certifikat](./fastly-configuration.md#provision-ssltls-certificates)** - Domänvaliderade krypteringscertifikat för säker HTTPS-trafik.
- **[Insvepning av ursprung](./fastly.md#origin-cloaking)** - Alla trafikvägar genom Fast säkerställs, direktåtkomst till ursprungsservrar blockeras.
- **[VCL-baserade säkerhetsfragment](./fastly-vcl-custom-snippets.md)** - VCL-regler (Custom Varnish Configuration Language) för IP-blockering, tillåtelselistning och begärandefiltrering.

### [!DNL Advanced Security]

[!DNL Advanced Security] ger ökat skydd utöver det inbyggda skyddet som ingår i [!DNL Adobe Commerce on Cloud Infrastructure], men till en extra kostnad:

- **Punkthantering** - Edge-baserad robotdetektering och -reducering med AI-robothantering.
- **Layer 7 DDoS-skydd** - DoS-absorption och försvar i programlager.
- **Avancerad hastighetsbegränsning** - Detaljerade hastighetskontroller för URL:er och API-slutpunkter.
- **Dynamiska utmaningar och Deception Technology** - Automatiserad utmaningstilldelning och reducering av kontoövertagande.

### Kundansvar

- **Bedrägeriförebyggande** - Sårning av bedrägerier på transaktionsnivå och upptäckt av betalningsbedrägerier.
- **Hantering av identitet och åtkomst** - Kundautentisering, auktorisering och sessionshantering.
- **Programsäkerhetstestning** - SAST/DAST och sårbarhetstestning.
- **Anpassade säkerhetskonfigurationer** - VCL-baserade regler, IP-tillåtelselista och blockeringslista.
- **Efterlevnadsverktyg** - PCI-skanning, SOC-kompatibilitetsrapportering och verktyg för lagstadgad revision.
- **Härdning på programnivå** - Tokenbaserad API-autentisering, frågeparameternormalisering och utformning av cachelagringsstrategi.

En fullständig översikt över säkerhetsansvar för Adobe och kunder finns i [modellen för delat ansvar](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility).

## Vanliga attackmönster och skydd

I följande tabell mappas vanliga attackmönster till lämpligt skyddslager i Adobe Commerce säkerhetsstack.

| Attackmönster | Typ | Skydd |
|---|---|---|
| En enda IP-adress eller en uppsättning identifierbara IP-adresser som skickar ett stort antal begäranden | DDoS + Bot | Blockera IP-adresser med hjälp av Commerce Admin eller Fast API. Inbyggt lager 3/4 för DDoS-skydd filtrerar trafiken i nätverkets utkant. |
| Angripanden på specifika URL:er eller API:er sprids över ett stort antal IP:er | DDoS + Bot | **[!DNL Advanced Security]**: Avancerad hastighetsbegränsning begränsar begärandavolymen per URL. Med punkthantering identifieras och blockeras distribuerad robottrafik. |
| Automatiska attacker på REST API-slutpunkter utan korrekt autentisering | Bot + DDoS | Verifiera att API-slutpunkter använder tokenbaserad autentisering. Rotera autentiseringsuppgifter om token komprometteras. **[!DNL Advanced Security]**: Avancerad hastighetsbegränsning kan skydda exponerade slutpunkter. |
| Cache-busting-attacker med manipulerade frågeparametrar | Bot + DDoS | Uteslut icke nödvändiga frågeparametrar från cachenycklar. Normalisera och begränsa frågeparametrar på programnivå. **[!DNL Advanced Security]**: Bot Management identifierar och blockerar automatiserad cachebusting-trafik. |
| SQL-injektion eller XSS-försök (cross-site scripting) | WAF | Den inkluderade [WAF-tjänsten](./fastly-waf-service.md) blockerar dessa hot automatiskt med hanterade säkerhetsregler. |

### WAF blockeringsbeteende

Följande WAF-beteende gäller för alla [!DNL Adobe Commerce on Cloud Infrastructure]-projekt, oavsett om [!DNL Advanced Security] är aktiverat eller inte. Den medföljande WAF-tjänsten använder följande blockeringsbeteende för vanliga attacksignaler:

- **SQL-injektionsbegäranden** blockeras omedelbart, även för en enda matchande begäran.
- Förfrågningar som identifieras med följande hotsignaler från en känd skadlig IP-adress blockeras omedelbart: Backdoor, Attack Tooling, CMDEXE, Log4J JNDI, Traversal och XSS.
- Begäranden från icke-skadliga IP-medlemmar som uppvisar hotsignalerna ovan blockeras när de överskrider följande tröskelvärden:

| Intervall | Tröskelvärde | Kontrollera frekvens |
|---|---|---|
| 1 minut | 50 förfrågningar | Var 20:e sekund |
| 10 minuter | 350 förfrågningar | Var 3:e minut |
| 1 timme | 1 800 förfrågningar | Var 20 minut |

## Begäran [!DNL Advanced Security]

Så här begär du [!DNL Advanced Security]:

>[!NOTE]
>
>[!DNL Advanced Security] är tillgängligt till en extra kostnad och kräver en aktiv [!DNL Adobe Commerce on Cloud Infrastructure]-prenumeration (PaaS).

1. Kontakta ditt Adobe-kontoteam eller Adobe säljare för att diskutera [!DNL Advanced Security] för ditt projekt.

1. När du har köpt [!DNL Advanced Security] [skickar du en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) med begäran om [!DNL Advanced Security] aktivering. Inkludera ditt projekt-ID för [!DNL Adobe Commerce on Cloud Infrastructure] och miljöer som kräver aktivering (till exempel produktion och mellanlagring).

1. Adobe aktiverar [!DNL Advanced Security] på tjänsten Snabbt och konfigurerar de ursprungliga skyddsprofilerna. Aktiveringen slutförs vanligtvis inom några arbetsdagar efter att biljetten skickats in.

1. Du får en bekräftelse på att [!DNL Advanced Security] är aktiv, tillsammans med information om de skydd som är aktiverade för dina miljöer.

>[!NOTE]
>
>Konfigurationsändringar för [!DNL Advanced Security] kräver för närvarande [att en supportanmälan skickas](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Självbetjäningskonfiguration via administratörsgränssnittet planeras för en framtida version.

## Begränsningar

[!DNL Advanced Security] ger skydd mot lagerframsidan för kanter. Följande funktioner är inte tillgängliga och hanteras bäst med kompletterande lösningar:

- **Sårbarhetspoäng på transaktionsnivå**—[!DNL Advanced Security] utvärderar inte enskilda betalningstransaktioner för bedrägeririsk. Använd en dedikerad plattform för förebyggande av bedrägerier för poängsättning på transaktionsnivå.
- **IAM (Identity and Access Management)**—[!DNL Advanced Security] hanterar inte användarautentisering, auktorisering eller sessionshantering. Dessa fortsätter att vara kundens ansvar.
- **Statisk och dynamisk säkerhetstestning av program (SAST/DAST)**—[!DNL Advanced Security] innehåller inte sårbarhetstestning på kodnivå eller penetrationstestning.
- **API-säkerhet** - Även om avancerad hastighetsbegränsning kan skydda API-slutpunkter från missbruk, finns inga omfattande API-säkerhetsfunktioner som schemavalidering och API-gatewayhantering.
- **Full bedrägeriförebyggande**-[!DNL Advanced Security] fokuserar på edge-layer store protection och är inte en komplett bedrägerihanteringsplattform.
- **Efterlevnadsverktygen**—[!DNL Advanced Security] tillhandahåller inte PCI-skanning, SOC-kompatibilitetsrapportering eller lagstadgade granskningsfunktioner.
