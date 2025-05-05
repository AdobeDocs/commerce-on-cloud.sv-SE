---
title: Teknikstack
description: Se vilken teknologi som utgör Commerce om Cloud-infrastrukturen.
feature: Cloud, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# Teknikstack

Tänk dig Adobe Commerce i molninfrastrukturen som fem funktionella lager, vilket visas nedan:

![Molnhög](../../assets/CloudStack.svg)

1. [**Molninfrastruktur**](pro-architecture.md): Välj antingen Amazon Web Services (AWS) eller Microsoft Azure som en IaaS-grund (Infrastructure as a Service) för dina Adobe Commerce-projekt i molninfrastrukturproffsprojekt.

   Adobe analyserar rutinmässigt användningen av din virtuella datorresurs (vCPU) och allokerar automatiskt resurser för att optimera din långtidsanvändning och minska risken för att överskrida din högsta årliga daggräns för vCPU. Om du förväntar dig ökad webbplatstrafik under en viss tidsperiod måste du fortsätta att öppna en supportbiljett för att [begära en tillfällig uppstorlek](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html?lang=sv-SE).

1. [**Plattform som en tjänst**](cloud-architecture.md): Varje Adobe Commerce i molninfrastrukturprojekt tillhandahåller en plattformsintegreringsmiljö (PaaS) för utveckling, testning och integrering av tjänster.
1. [**Adobe Commerce**](../project/overview.md): Adobe Commerce i molninfrastruktur innehåller en företablerad infrastruktur som omfattar PHP, MySQL (MariaDB), Redis, [!DNL RabbitMQ] och sökmotortekniker som stöds.
1. [**Prestandaverktyg**](../monitor/new-relic-service.md): Med New Relic prestandaverktyg kan du felsöka, övervaka och hantera program och infrastruktur genom att samla in, analysera och visa data från Adobe Commerce i molninfrastrukturprojekt.
1. [**CDN (Content Delivery Network), Brandvägg för webbaserade program ([!DNL WAF]) och Bildoptimering (IO)**](../cdn/fastly.md):

   * [Snabbt CDN](../cdn/fastly.md#ddos-protection) - Tillhandahåller säkra CDN-tjänster med inbyggt skydd mot DoS-attacker (Distributed Denial of Service) som [!DNL Ping of Death], [!DNL Smurf] och andra ICMP-baserade (Internet Control Message Protocol) översvämningsattacker.
   * [Brandvägg för webbaserade program (WAF)](../cdn/fastly-waf-service.md) - WAF tjänster garanterar PCI-kompatibilitet för Adobe Commerce-butiker i produktionsmiljöer och WAF-principer som skyddar dina Adobe Commerce webbprogram från injektionsangrepp, skadlig inmatning, serveröverskridande skript, dataexfiltrering, HTTP-protokollöverträdelser och andra [[!DNL OWASP] De 10 viktigaste säkerhetshot](https://owasp.org/www-project-top-ten/).
   * [Bildoptimering (IO)](../cdn/fastly-image-optimization.md) - Tillhandahåller bildredigering och optimering i realtid för att snabba upp bildleveransen och förenkla underhåll av bildkälluppsättningar för responsiva webbprogram. I/O-funktionen avlastar snabbt bildbearbetning och storleksändring, vilket frigör servrar så att de kan bearbeta beställningar och konverteringar effektivt.

Monolitiska applikationer är resurskrävande och svåra att skala och fungerar snabbt. Med Cloud-infrastrukturen får Commerce-kunder oöverträffad tillgång till SaaS-baserade mikrotjänster som är avancerade, intelligenta och kraftfulla. Se [Program och tjänster som stöds](cloud-architecture.md#supported-software-and-services).

Använd guiden [Commerce Get Started](../../get-started/overview.md) för att konfigurera ditt nya molnprogram och börja hantera ditt [!DNL Commerce]-program i en molnbaserad miljö.
