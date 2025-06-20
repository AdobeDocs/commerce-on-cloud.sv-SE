---
title: Konfigurera snabbfunktioner
description: Lär dig hur du konfigurerar snabbtjänster för ditt Adobe Commerce-projekt.
feature: Cloud, Configuration, Iaas, Cache, Security
exl-id: f9ce1e8b-4e9f-488e-8a4d-f866567c41d8
source-git-commit: 867abffd6cbed6e026c20b646ff641cc6ab40580
workflow-type: tm+mt
source-wordcount: '2063'
ht-degree: 0%

---

# Konfigurera snabbfunktioner

Det krävs snabbt för Adobe Commerce i miljöer med molnbaserad infrastruktur, staging och produktion.

Fungerar snabbt med lack för att få snabba cachningsfunktioner och ett CDN-nätverk (Content Delivery Network) för statiska resurser. Tillhandahåller snabbt även en brandvägg för webbaserade program (WAF) för att säkra din webbplats och molninfrastruktur. För att skydda din webbplats och molninfrastruktur från skadlig trafik och attacker dirigerar du all inkommande webbplatstrafik via Fast.

>[!NOTE]
>
>Snabb är inte tillgängligt i integreringsmiljöer.

Följ de här stegen för att aktivera, konfigurera och testa snabbt i webbplatsutvecklingsprocessen för att ge säker åtkomst till din webbplats.

- Få snabbt inloggningsuppgifter för mellanlagrings- och produktionsmiljöer
- Aktivera snabb CDN-cachelagring
- Ladda upp VCL-fragment snabbt
- Uppdatera DNS-konfigurationen för att dirigera trafik till tjänsten Snabbt
- Testa cachelagring snabbt

>[!NOTE]
>
>När du har aktiverat och verifierat den ursprungliga snabbkonfigurationen kan du anpassa konfigurationen. Du kan till exempel aktivera ytterligare alternativ som bildoptimering, kantmoduler och anpassad VCL-kod. Se [Anpassa cachekonfigurationen](fastly-custom-cache-configuration.md).

Under etableringen lägger Adobe till ditt projekt i [snabbtjänstkontot](fastly.md#fastly-service-account-and-credentials) för Adobe Commerce i molninfrastrukturen och skapar snabbkontoautentiseringsuppgifter för Starter `master` - och Pro Staging- och Production-miljöerna. Varje miljö har unika referenser.

Du behöver snabbinloggningsuppgifterna för att konfigurera Fast CDN-tjänster från Adobe Commerce Admin och för att skicka in Fast API-begäranden.

## Snabb åtkomst till Admin Dashboard

Med Adobe Commerce i molninfrastruktur kan du inte komma åt snabbadministratörsmodulen direkt.

Du måste använda Adobe Commerce Admin för att granska och uppdatera snabbkonfigurationen för dina miljöer. Om du inte kan lösa ett problem med hjälp av snabbfunktionerna i Admin skickar du en [Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

## Få inloggningsuppgifter snabbt

Använd följande metoder för att hitta och spara snabb service-ID och API-token för din miljö:

**Så här visar du dina snabbuppgifter**:

>[!NOTE]
>
>Dela inte din API-token på supportärenden, allmänna forum eller någon offentlig plats. Dessutom kan du aldrig implementera API-token i koddatabaser. Databaser får bara innehålla oföränderliga filer utan känslig information.
>
>Adobe Commerce Support har redan tillgång till de nödvändiga nycklarna, så du behöver inte ange din API-token när du söker hjälp.
>
>Om din API-token någonsin delas offentligt eller kopplas till en supportanmälan kommer den att anses vara komprometterad. I sådana fall måste Adobe generera en ny token.
>
>Relaterat: [Fel vid validering av snabbinloggningsuppgifterna](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/error-when-validating-fastly-credentials#solution)

Metoden för att visa inloggningsuppgifter skiljer sig åt för Pro- och Starter-projekt.

- IaaS-monterad delad katalog - I Pro-projekt använder du SSH för att ansluta till servern och hämta snabbinloggningsuppgifterna från filen `/mnt/shared/fastly_tokens.txt`. För mellanlagrings- och produktionsmiljöer finns unika autentiseringsuppgifter. Du måste hämta autentiseringsuppgifterna för varje miljö.

- Lokal arbetsyta - Använd `magento-cloud` CLI från kommandoraden för att [lista och granska](../environment/variables-cloud.md#viewing-environment-variables) snabbt systemvariabler.

  ```bash
  magento-cloud variable:get -e <environment-ID>
  ```

- [!DNL Cloud Console] - Kontrollera följande miljövariabler i [miljökonfigurationen](../project/overview.md#configure-environment).

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_API_KEY`

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_SERVICE_ID`

>[!NOTE]
>
>Om du inte hittar de snabba inloggningsuppgifterna för förproduktionsmiljö eller produktionsmiljö kontaktar du Adobe Customer Technical Advisor (CTA).

## Aktivera snabb cachelagring

Du behöver följande komponenter för att aktivera och konfigurera snabbfunktioner:

- Senaste versionen av [Snabbt CDN för Magento 2-modulen](fastly.md#fastly-cdn-module-for-magento-2) som är installerad i miljö för förproduktion och produktion. Se [Uppgradera snabbt](#upgrade-the-fastly-module).

- [Autentiseringsuppgifter snabbt](#get-fastly-credentials) för Adobe Commerce i molninfrastrukturerna Staging- och Production-miljöer

**Så här aktiverar du snabb CDN-cachelagring i mellanlagring och produktion**:

{{admin-login-step}}

1. Klicka på **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System** och expandera **Helsidescache**.

   ![Expandera och markera snabbt](../../assets/cdn/fastly-menu.png)

1. I avsnittet _Cachelagring av program_ tar du bort markeringen från **Använd systemvärde** och väljer sedan **Snabbt CDN** i listrutan.

   ![Välj snabbt](../../assets/cdn/fastly-enable-admin.png)

1. Expandera **Snabb konfiguration** och [välj cachelagringsalternativ](https://github.com/fastly/fastly-magento2/blob/master/Documentation/CONFIGURATION.md#configure-the-module).

1. När du har konfigurerat cachelagringsalternativen klickar du på **Spara konfiguration** överst på sidan.

1. Rensa cacheminnet enligt meddelandet.

1. Fortsätt konfigurera snabbt genom att gå tillbaka till **Lager** > **Inställningar** > **Konfiguration** > **Avancerat** > **System** > **Snabbt konfigurering**.

### Testa autentiseringsuppgifter snabbt

1. Gå till **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System** > **Snabbt konfigurering** i Admin.

1. Lägg till värdena **Snabbt service-ID** och **API-token** för din projektmiljö om det behövs.

   ![Administratör för snabb inloggning](../../assets/cdn/fastly-credentials-admin-ui.png)

   >[!NOTE]
   >
   >Välj inte länken för att skapa en API-token snabbt. Använd i stället de [snabba autentiseringsuppgifterna (Service ID och API-token) som tillhandahålls av Adobe](#get-fastly-credentials).

1. Klicka på **Testa autentiseringsuppgifter**.

1. Om testet lyckas klickar du på **Spara konfiguration** och rensar sedan cachen.

   Om testet misslyckas kontrollerar du att rätt Service ID- och API-tokenvärden matchar autentiseringsuppgifterna för den aktuella miljön.

   Om testet misslyckas igen skickar du en Adobe Commerce supportanmälan eller kontaktar din Adobe-kontorepresentant. För Pro-projekt ska du inkludera URL:er för dina produktions- och mellanlagringswebbplatser. Inkludera URL:er för din `Master`- och mellanlagringsplats för startprojekt.

>[!NOTE]
>
>Instruktioner om hur du ändrar snabbt API-tokenreferenser för en mellanlagrings- eller produktionsmiljö finns i [Ändra snabbt autentiseringsuppgifter](fastly.md#change-fastly-api-token).

### Ladda upp VCL snabbt

När du har aktiverat modulen Snabbt överför du [VCL-standardkoden](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets) till snabbservrarna. Den här koden innehåller en serie VCL-kodfragment som anger konfigurationsinställningarna för att aktivera cachelagring och andra Fast CDN-tjänster för din Adobe Commerce i molninfrastruktur.

>[!NOTE]
>
>Tjänster för snabb cachning fungerar inte förrän du har slutfört den initiala överföringen av den Fast VCL-koden till Adobe Commerce Staging and Production.

**Så här överför du den fasta VCL**:

1. I avsnittet _Snabbkonfiguration_ klickar du på **Överför VCL till Snabbt** enligt bilden nedan.

   ![Överför en Magento VCL till snabbast](../../assets/cdn/fastly-upload-vcl-admin.png)

1. När överföringen är klar uppdaterar du cacheminnet enligt meddelandet längst upp på sidan.

## Tillhandahåll SSL-/TLS-certifikat

Adobe tillhandahåller ett domänvaliderat SSL-/TLS-certifikat (Let&#39;s Encrypt SSL/TLS) för säker HTTPS-trafik snabbt. Adobe tillhandahåller ett certifikat för varje Pro Production-, Staging- och Starter Production-miljö för att skydda alla domäner i den miljön. Mer information om det angivna certifikatet finns i [Adobe SSL-certifikat (TLS) för Adobe Commerce om molninfrastruktur](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq.html).

>[!NOTE]
>
>Du kan ange ett eget TLS- eller SSL-certifikat i stället för att använda det krypteringscertifikat som tillhandahålls av Adobe. Den här processen kräver dock ytterligare arbete för att kunna konfigurera och underhålla. Om du vill välja det här alternativet skickar du en Adobe Commerce supportanmälan eller arbetar med Adobe för att lägga till anpassade värdbaserade certifikat till din Adobe Commerce i molninfrastrukturmiljöer.

För att aktivera SSL-/TLS-certifikaten för Adobe Commerce-miljöer utför Adobe automatisering följande steg:

- Validerar domänägarskap
- Tillhandahåller ett Let&#39;s Encrypt SSL/TLS-certifikat som omfattar angivna övre nivåer och underdomäner för dina butiker
- Överför certifikatet till molnmiljön när webbplatsen är aktiv

Den här automatiseringen kräver att du uppdaterar DNS-konfigurationen för din plats för att kunna tillhandahålla domänverifieringsinformation. Använd **en** av följande metoder:

- **DNS-validering**-Uppdatera din DNS-konfiguration med CNAME-poster som pekar på tjänsten Snabbt för livewebbplatser
- **ACME-utmaning CNAME-poster**-Uppdatera din DNS-konfiguration med ACME-utmaning CNAME-poster som tillhandahålls av Adobe för varje domän i din miljö

>[!TIP]
>
>Om du har en produktionsdomän som inte är aktiv använder du ACME-utmanings-CNAME-posterna för domänvalidering. Om du lägger till posterna i DNS-konfigurationen tidigt kan Adobe tillhandahålla SSL-/TLS-certifikatet med rätt domäner innan webbplatsen startas. Innan du startar produktionen måste du ersätta platshållarposterna med CNAME-posterna från Adobe.

När domänvalideringen är klar tillhandahåller Adobe certifikatet för kryptering av TLS/SSL och överför det till miljöer med aktiv förproduktion eller produktion. Den här processen kan ta upp till 12 timmar. Vi rekommenderar att du slutför DNS-konfigurationsuppdateringarna flera dagar i förväg för att undvika förseningar i webbplatsutvecklingen och webbplatsens start.

## Uppdatera DNS-konfiguration med utvecklingsinställningar

Under den första snabbinstallationsprocessen kan du använda följande URL:er för att konfigurera och testa Snabb cachelagring i mellanlagrings- och produktionsmiljöer:

- Proffsens produktion:

   - `mcprod.<your-domain>.com`
   - `mcstaging.<your-domain>.com`

- Endast för startproduktion:

   - `mcprod.<your-domain>.com`

Dessa standardförproduktionsadresser är tillgängliga när projektet har etablerats. Värdet för `"your-domain"` är det domännamn du angav under introduktionsprocessen.

>[!NOTE]
>
>Du kan inte ange en anpassad domän för en icke-produktionsmiljö i Starter-projekt.

Uppdatera din DNS-konfiguration om du vill dirigera trafik från dina webbutiks-URL:er till tjänsten Snabbt. När du uppdaterar konfigurationen etablerar Adobe automatiskt de SSL-/TLS-certifikat som krävs och överför dem till dina molnmiljöer. Den här provisioneringen kan ta upp till 12 timmar.

>[!NOTE]
>
>När du är redo att starta din produktionsplats måste du uppdatera DNS-konfigurationen igen så att den pekar på produktionsdomänerna mot tjänsten Snabbt och slutföra ytterligare konfigurationsåtgärder. Se [Startchecklista](../launch/checklist.md).

**Förutsättningar:**

- Aktivera modulen Snabbt.
- Ladda upp standardkoden för VCL snabbt.
- Tillhandahåll en lista över de översta domänerna och underdomänerna för varje miljö till Adobe, eller skicka en Adobe Commerce supportanmälan.
- Vänta på bekräftelse på att de angivna domänerna har lagts till i dina molnmiljöer.
- I Starter-projekt lägger du till domänerna i din snabbtjänstkonfiguration. Se [Hantera domäner](fastly-custom-cache-configuration.md#manage-domains).
- Om du vill ha information om hur du uppdaterar DNS-konfigurationen kan du fråga din [DNS-registrator](https://lookup.icann.org/) om du vill ha rätt metod för domäntjänsten.

**Så här uppdaterar du DNS-konfigurationen för utveckling**:

1. Peka förproduktions-URL:er till tjänsten Snabbt genom att lägga till CNAME-poster: `prod.magentocloud.map.fastly.net`, till exempel:

   | Domän eller underdomän | CNAME |
   |---------------------------|----------------------------------|
   | mcprod.your-domain.com | prod.magentocloud.map.fastly.net |
   | mcstaging.your-domain.com | prod.magentocloud.map.fastly.net |

   När CNAME-posterna är live tillhandahåller Adobe certifikat och överför SSL-/TLS-certifikaten.

   >[!NOTE]
   >
   >Om du tänker använda en domän (`your-domain.com`) för produktionsplatsen måste du konfigurera DNS-adressposter (A-poster) så att de pekar på IP-adresserna för snabbservern. Se [Uppdatera DNS-konfiguration med produktionsinställningar](../launch/checklist.md#to-update-dns-configuration-for-site-launch).


1. Lägg till ACME-utmanings-CNAME-poster för domänvalidering och företablering av Production SSL/TLS-certifikat, till exempel:

   | Domän eller underdomän | CNAME |
   |-------------------------------------------|-------------------------------------------|
   | _acme-challenge.your-domain.com | 0123456789abcdef.validation.magento.cloud |
   | _acme-challenge.www.your-domain.com | 9573186429stuvwx.validation.magento.com |
   | _acme-challenge.mystore.your-domain.com | 1234567898zxywvu.validation.magento.cloud |
   | _acme-challenge.subdomain.your-domain.com | 1098765743lmnopq.validation.magento.cloud |

   >[!NOTE]
   >
   >ACME-utmaningsposterna i det här exemplet är platshållare som inte är avsedda att tillhandahålla dina Adobe Commerce mellanlagrings- och produktionsplatser. Kontakta Adobe för att få rätt information om ACME-utmaningsposten för ditt projekt.

   När du har lagt till CNAME-posterna validerar Adobe domänerna och tillhandahåller SSL-/TLS-certifikatet för miljön. När du uppdaterar DNS-konfigurationen för att dirigera trafik från dessa domäner till tjänsten Snabbt, överför Adobe certifikatet till miljön.

1. Uppdatera Adobe Commerce Bas-URL.

   - Använd SSH för att logga in i produktionsmiljön.

     ```bash
     magento-cloud ssh
     ```

   - Använd CLI i molnet för att ändra bas-URL:en för din butik.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://mcstaging.your-domain.com/"
     ```

   >[!NOTE]
   >
   >Som ett alternativ till molnbaserad CLI kan du uppdatera bas-URL:en från [administratören](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html)

1. Starta om webbläsaren.

1. Testa webbplatsen.

## Testa cachelagring snabbt

När du har slutfört ändringarna av DNS-konfigurationen använder du kommandoradsverktyget [cURL](https://curl.se/) för att kontrollera att snabbcachen fungerar.

**Så här kontrollerar du svarsrubrikerna**:

1. Använd följande `curl`-kommando i en terminal för att testa din webbplats-URL:

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 https://<live-URL>
   ```

   Om du inte har angett en statisk väg eller slutfört DNS-konfigurationen för domänerna på den publicerade webbplatsen använder du flaggan `--resolve`, som åsidosätter DNS-namnmatchningen.

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 --resolve <live-URL-hostname>:443:<live-IP-address>
   ```

1. Verifiera [headers](fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers) i svaret för att kontrollera att Fastly fungerar. Du bör se följande unika rubriker i svaret:

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

Om rubrikerna inte har rätt värden läser du [Åtgärda fel i svarsrubrikerna](fastly-troubleshooting.md#curl) för felsökning.

## Uppgradera modulen Snabbt

Uppdaterar snabbt CDN för Magento 2-modulen för att åtgärda problem, öka prestandan och tillhandahålla nya funktioner.
Vi rekommenderar att du uppdaterar modulen Snabbt i dina miljö för förproduktion och produktion till den [senaste versionen](https://github.com/fastly/fastly-magento2/blob/master/VERSION).

När du har uppdaterat modulen måste du överföra VCL-koden för att ändringarna ska gälla för snabbtjänstkonfigurationen.

>[!WARNING]
>
> Om du har anpassat standardkoden för Fast VCL med en anpassad version skrivs ändringarna över när du uppgraderar modulen Snabbt. Om du har lagt till anpassade VCL-fragment med unika namn bevaras dessa ändringar under uppgraderingsprocessen. Som en god praxis bör du uppgradera mellanlagringsmiljön och validera ändringarna innan du tillämpar ändringarna i produktionsmiljön.

**Så här kontrollerar du vilken version av Fast CDN-modulen som används i Magento 2**:

1. Byt till rotkatalogen i molnmiljön.

1. Använd Composer för att kontrollera den installerade versionen.

   ```bash
   composer show *fastly*
   ```

1. Om den [senaste versionen](https://github.com/fastly/fastly-magento2/releases) inte är installerad slutför du stegen för att uppgradera snabbmodulen.

**Så här uppgraderar du snabbmodulen**:

1. Använd följande modulinformation i din lokala integreringsmiljö för att [uppgradera snabbmodulen](../store/extensions.md#upgrade-an-extension).

   ```text
   module name: fastly/magento2
   repository: https://github.com/fastly/fastly-magento2.git
   ```

1. Skicka dina uppdateringar till mellanlagringsmiljön.

1. Logga in på Admin for your Staging environment för att [överföra VCL-koden](#upload-vcl-to-fastly).

1. [Verifiera snabbtjänster](fastly-troubleshooting.md#verify-or-debug-fastly-services) på webbplatsen för Adobe Commerce mellanlagring.

När du har verifierat Snabba tjänster på mellanlagringsplatsen upprepar du uppgraderingsprocessen i produktionsmiljön.

>[!TIP]
>
> Om du har problem med Snabba tjänster i dina Adobe Commerce-miljöer kan du läsa [Adobe Commerce Snabbt felsökning](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/magento-fastly-troubleshooter.html).
