---
title: Öppna checklista
description: Granska checklisteobjekt för att starta webbplatsen.
exl-id: efc97d4a-a9f3-49fa-b977-061282765e90
source-git-commit: ca2d94364787695398b2b8af559733fe52ec2949
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 0%

---

# Öppna checklista

Innan du distribuerar till produktionsmiljön hämtar du [startchecklistan](../../assets/adobe-commerce-cloud-prelaunch-checklist.pdf) och använder den med dessa instruktioner för att bekräfta att du har slutfört all konfiguration och testning som krävs. Se en översikt över den fullständiga distributionsprocessen för Starter och Pro på [Distribuera din butik](../deploy/staging-production.md).

## Fullständigt test i produktion

Se [Testa distributionen](../test/staging-and-production.md) för att testa alla aspekter av dina platser, butiker och miljöer. Testerna omfattar kontroll av snabbhet, UAT (User Acceptance Test) och prestandatestning.

## TLS och snabbt

Adobe tillhandahåller ett Låt&#39;s Encrypt SSL/TLS-certifikat för varje miljö. Det här certifikatet krävs för att snabbt kunna leverera säker trafik via HTTPS.

Om du vill använda det här certifikatet måste du uppdatera din DNS-konfiguration så att Adobe kan slutföra domänvalidering och tillämpa certifikatet i din miljö. Varje miljö har ett unikt certifikat som omfattar domänerna för Adobe Commerce på molninfrastrukturplatser som distribueras i den miljön. Vi rekommenderar att du slutför och konfigurerar uppdateringarna under [Snabbt konfigureringsprocessen](../cdn/fastly-configuration.md).

## Uppdatera DNS-konfiguration med produktionsinställningar

När du är redo att starta webbplatsen måste du uppdatera DNS-konfigurationen för att dirigera trafik från produktionsmiljön via tjänsten Snabbt.

**Förutsättningar:**

- [Konfigurera och testa snabbt i utvecklingsmiljön](../cdn/fastly-configuration.md#)

- Konfigurationen av produktionsmiljön har uppdaterats med alla nödvändiga domäner

  Vanligtvis samarbetar du med din kundtekniska rådgivare för att lägga till alla toppnivådomäner och underdomäner som krävs för dina butiker. [Skicka en Adobe Commerce-supportanmälan](https://support.magento.com/hc/en-us/articles/360019088251) om du vill lägga till eller ändra domänerna för din produktionsmiljö. Vänta på bekräftelse på att projektkonfigurationen har uppdaterats.

  I Starter-projekt måste du lägga till domänerna i projektet. Se [Hantera domäner](../cdn/fastly-custom-cache-configuration.md#manage-domains).

- SSL/TLS-certifikat för dina produktionsmiljöer.

  Om du lade till ACME-utmaningsposter för dina produktionsdomäner under snabbkonfigurationsprocessen, överför Adobe SSL-/TLS-certifikatet till din produktionsmiljö automatiskt när du uppdaterar DNS-konfigurationen för att dirigera trafik till tjänsten Snabbt. Om du inte har företablerat certifikatet eller om du har uppdaterat dina domäner måste Adobe slutföra domänvalidering och tillhandahålla certifikatet, vilket kan ta upp till 12 timmar.

### Så här uppdaterar du DNS-konfigurationen för webbplatsstart:

1. Uppdatera följande DNS-konfiguration för produktionsplatsen:

   - Ange alla nödvändiga omdirigeringar, särskilt om du migrerar från en befintlig plats

   - Ange zonens rotresurspost som adress till värdnamnet

   - Sänk värdet för TTL (Time-to-Live) för att uppdatera DNS-informationen så att den pekar ut kunderna till rätt produktionsbutik

     Vi rekommenderar ett betydligt lägre TTL-värde när vi byter DNS-post. Detta värde anger för DNS hur länge DNS-posten ska cachelagras. När den är förkortad uppdateras DNS snabbare. Du kan till exempel ändra TTL-värdet från tre dagar till 10 minuter när du uppdaterar platsen. Observera att om TTL-värdet förkortas ökar belastningen på DNS-infrastrukturen. Återställ det tidigare, högre värdet efter att webbplatsen startats.


1. Lägg till CNAME-poster för att peka underdomänerna för din produktionsmiljö mot snabbtjänsten `prod.magentocloud.map.fastly.net`, till exempel:

   | Domän eller underdomän | CNAME |
   | ----------------------- | -------------------------------- |
   | `www.<domain-name>.com` | prod.magentocloud.map.fastly.net |
   | `mystore.<domain-name>.com` | prod.magentocloud.map.fastly.net |

1. Om det behövs lägger du till A- och AAAA-poster för att mappa den överordnade domänen (`<domain-name>.com`) till följande fasta IP-adresser:

   | Apex-domän | ANAME | AAANAME |
   | --------------- | ----------------- | -------- |
   | `<domain-name>.com` | `151.101.1.124` | 2a04:4e42:200::380 |
   | `<domain-name>.com` | `151.101.65.124` | 2a04:4e42:400::380 |
   | `<domain-name>.com` | `151.101.129.124` | 2a04:4e42:600::380 |
   | `<domain-name>.com` | `151.101.193.124` | 2a04:4e42::380 |

>[!IMPORTANT]
>
>DNS-instruktionerna i [RFC1034](https://www.rfc-editor.org/rfc/rfc1912) (**section 2.4**) anger att:
>_En CNAME-post får inte finnas parallellt med andra data. Om suzy.podunk.xx är ett alias för use.podunk.xx kan du alltså inte ha en MX-post för suzy.podunk.edu, en A-post eller till och med en TXT-post._
>
>Därför bör DNS-poster vara av typen `CNAME` för underdomäner och typen `A` för API-domäner (rotdomäner). Om du ignorerar den här regeln kan det leda till avbrott i e-posttjänsten eller DNS-spridningen eftersom du inte längre kan lägga till andra poster, som MX eller NS. Vissa DNS-leverantörer kan kringgå detta genom att använda interna anpassningar, men om du följer standarden säkerställs stabilitet och flexibilitet (till exempel byte av DNS-providern).

1. Uppdatera bas-URL:en.

   - Använd SSH för att logga in i produktionsmiljön.

     ```bash
     magento-cloud ssh -e production
     ```

   - Använd CLI för att ändra bas-URL för din butik.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://www.<domain-name>.com/"
     ```

   **OBS!**: Du kan även uppdatera bas-URL:en från administratören. Se [Lagra URL:er](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html?lang=sv-SE) i _Adobe Commerce Store och Köp Experience Guide_.

1. Vänta några minuter tills webbplatsen har uppdaterats.

1. Testa webbplatsen.

## Verifiera produktionskonfiguration

Gör en sista pass för att validera produktionskonfigurationen för en eller flera butiker. Du kan uppdatera konfigurationen i produktionsmiljön. Om inställningarna är skrivskyddade kan du behöva öppna en SSH-anslutning och använda CLI-kommandon för att ändra konfigurationen eller göra konfigurationsändringar i den lokala miljön. När du är klar med uppdateringarna kan du distribuera ändringarna till förproduktionsmiljöer.

Följande är rekommenderade ändringar och kontroller:

- [Utgående e-posttestning har slutförts](../project/outgoing-emails.md)

- [Säker konfiguration för administratörsautentiseringsuppgifter och bas-admin-URL](https://experienceleague.adobe.com/sv/docs/commerce-admin/systems/security/security-admin)

- [Optimera alla bilder för webben](../cdn/fastly-image-optimization.md)

- [Kontrollera miniinställningar för HTML, JavaScript och CSS](../deploy/static-content.md)

## Verifiera snabb cachelagring

- Testa och verifiera att cachelagring av fast cachning fungerar korrekt på produktionsplatsen. Detaljerade tester och kontroller finns i [Snabba tester](../test/staging-and-production.md#check-fastly-caching).

- [Kontrollera att den senaste versionen av Snabbt CDN Module för Commerce är installerad i din produktionsmiljö](../cdn/fastly-configuration.md#upgrade-the-fastly-module)

- [Kontrollera att den senaste versionen av den snabbaste VCL-koden har överförts](../cdn/fastly-configuration.md#upload-vcl-to-fastly)

## Prestandatestning

Vi rekommenderar att du granskar alternativen för [Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) som en del av beredskapen inför starten.

Du kan också testa med följande alternativ från tredje part:

- [Belägring](https://www.joedog.org/siege-home/): Trafikformnings- och testningsprogramvara som gör att din butik når gränsen. Besök webbplatsen med ett konfigurerbart antal simulerade klienter. Siege har stöd för grundläggande autentisering, cookies, HTTP, HTTPS och FTP-protokoll.

- [Jmeter](https://jmeter.apache.org/): Utmärkt inläsningstestning som hjälper till att mäta prestanda för spikad trafik, till exempel för blixtförsäljning. Skapa anpassade tester som körs mot din plats.

- [New Relic](https://support.newrelic.com/s/) (tillhandahålls): Hjälper dig att hitta processer och områden på webbplatsen som ger långsam prestanda med spårad tid per åtgärd, som överföring av data, frågor, Redis med mera.

- [WebPageTest](https://www.webpagetest.org/) och [Passning](https://www.pingdom.com/): Realtidsanalys av webbplatssidorna läses in med olika ursprungsplatser. Riket kan kosta en avgift. WebPageTest är ett kostnadsfritt verktyg.

## Säkerhetskonfiguration

- [Konfigurera säkerhetsgenomsökning](overview.md#set-up-the-security-scan-tool)

- [Säker konfiguration för administratörsanvändare](https://experienceleague.adobe.com/sv/docs/commerce-admin/systems/security/security-admin)

- [Säker konfiguration för Admin URL](https://experienceleague.adobe.com/sv/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url)

- [Ta bort användare som inte längre använder Adobe Commerce i molninfrastrukturprojektet](../project/user-access.md)

- [Konfigurera tvåfaktorsautentisering](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/)

## Prestandaövervakning

Du kan använda New Relic tjänster för prestandaövervakning i Pro- och Starter-miljöer. På Pro-plankonton tillhandahåller vi hanterade aviseringar för Adobe Commerce aviseringspolicy för att övervaka program- och infrastrukturprestanda med New Relic APM och infrastrukturagenter. Mer information om hur du använder de här tjänsterna finns i [Övervaka prestanda med hanterade aviseringar](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts).

### Nästa steg

[Starta steg](steps.md)
