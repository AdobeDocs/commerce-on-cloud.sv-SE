---
title: Ställ in PayPal-betalningsmetoder
description: Ställ in betalningsmetoder för PayPal för Adobe Commerce i molninfrastruktur.
feature: Cloud, Checkout, Payments
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---

# Ställ in PayPal-betalningsmetoder

Adobe Commerce i molninfrastrukturen erbjuder ett startverktyg som konfigurerar PayPal Express Checkout-konton direkt via Admin. Detta verktyg är tillgängligt för ECE 2.1.8 och senare. Om du vill ha bättre stöd för att gå live och testa betalningsmetoder för PayPal kan du aktivera och konfigurera ditt PayPal Express-utcheckningskonto för sandbox- eller produktionskonton.

Du kan konfigurera sandlådan eller produktionskontot i alla miljöer:

* För integrerings- och mellanlagringsmiljöer anger du autentiseringsuppgifter för sandlådan.
* I din produktionsmiljö anger du sandlådeinloggningsuppgifter för inledande testning och ersätter dem sedan med Live-produktionsuppgifter för en butik som har startats.

## PayPal-konto

Det är bäst att använda ett PayPal-handelskonto som har förberetts och konfigurerats, men du kan skapa ett konto eller uppgradera ett personligt konto via administratören.

[!DNL PayPal onboarding] stöder anslutning med följande konton:

* PayPal Business Account
* PayPal personligt konto, konverterar till ett företagskonto. Om du har ett befintligt personligt PayPal-konto kan du logga in med dessa autentiseringsuppgifter och uppgradera det här kontot till ett företagskonto när du slutför synkroniseringen.

Om du inte har något befintligt PayPal-konto skapar du ett. Ange en e-postadress för ett nytt konto. Om inget matchande PayPal-konto hittas följer du anvisningarna för att skapa ett PayPal Business-konto. Du kan också skapa ett konto direkt via [PayPal](https://www.paypal.com/us/webapps/mpp/account-selection).

### PayPal-begränsningar

PayPal har stöd för anslutning av PayPal Express Checkout för länder över hela världen, med undantag för följande begränsningar:

* Indien och Japan (framtida PayPal-uppdateringar kan stödja dessa konton)
* Israel

För Brasilien måste du ha ett befintligt PayPal-företagskonto för att kunna ansluta. Du kan inte konvertera ett befintligt personligt PayPal-konto för Brasilien under den här processen. Om du behöver ett konto [skapar du ett PayPal-företagskonto](https://www.paypal.com/us/webapps/mpp/account-selection).

## Konfigurera PayPal Express-kassan

Så här konfigurerar du PayPal Express Checkout:

1. Gå till Admin for the environment.
1. I den vänstra navigeringen väljer du **Lagrar** > **Konfiguration** och sedan **Försäljning** > **Betalningsmetoder**.
1. För PayPal väljer du **Konfigurera**. Konfigurationsfält visas i expanderbara avsnitt för Express Checkout, Adverise PayPal Credit samt Basic and Advanced settings.
1. Anslut ditt PayPal-konto. Tills kontot är anslutet inaktiveras alternativen för att aktivera. Mer information om tillgängliga och stödda konton för anslutning och begränsningar finns i [PayPal-konto](#paypal-account).

   * Om du vill ansluta ditt PayPal-Live-konto klickar du på Anslut med PayPal och följer anvisningarna. Alla kundköp som görs via en PayPal som är komplett och aktivt debiterar kunderna i en direktbutik.
   * Om du vill ansluta ditt sandlådekonto för testning klickar du på Autentiseringsuppgifter för sandlådan och följer anvisningarna. Alla kundköp som använder en sandbox PayPal är slutförda utan att aktivt debitera kunderna.

1. Konfigurera inställningarna för Express Checkout för att autentisera och använda API:t för PayPal:

   * **E-post som är associerad med PayPal-handelskonto** (valfritt) anger den e-postadress som är associerad med ditt PayPal-handelskonto. E-postmeddelandet är skiftlägeskänsligt.
   * **API-autentiseringsmetoder** som API-signatur eller API-certifikat.
   * API-användarnamn, lösenord och signatur som hämtats från ditt PayPal-konto.
   * **Sandlådeläge** Välj Ja eller Nej för att ange om inloggningsuppgifterna som du angav gäller sandlådan. Om du har angett produktionsuppgifter väljer du Nej.
   * **API:t använder Proxy**, välj Ja eller Nej om systemet använder en proxyserver för att upprätta en anslutning mellan Adobe Commerce och PayPal-betalningssystemet. Om Ja, ange proxyvärden och port.

1. Detaljerad information och steg för hur du konfigurerar ditt konto finns i [PayPal Express Checkout](https://experienceleague.adobe.com/sv/docs/commerce-admin/stores-sales/payments/paypal/paypal-express-checkout) som börjar med steg 2 Slutför de obligatoriska inställningarna.

Med kontot konfigurerat och autentiserat kan du aktivera och inaktivera betalningsalternativ för PayPal under Obligatoriska PayPal-inställningar:

* **Aktivera den här lösningen** visar betalningsmetoden PayPal för kunder via webbplatsen.
* **Aktivera sammanhangsbaserad utcheckning**
* **Aktivera PayPal-kredit** gör att kunder kan få PayPal-kredit utan extra kostnader. PayPal betalar ordern i förskott och hanterar alla återbetalningar för krediten direkt med kunden.

## PayPal-variabler

När du använder PayPal on-boarding-verktyget med Adobe Commerce i molninfrastrukturen lägger du till följande variabel i `variables:env`-avsnittet i `magento.app.yaml`-filen.

```yaml
# Environment variables
variables:
  env:
    CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
```

Om du uppgraderar till 2.2 från 2.1.8 eller senare måste du fortfarande lägga till den här variabeln.
