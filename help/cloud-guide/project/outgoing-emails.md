---
title: Konfigurera utgående e-post
description: Lär dig hur du aktiverar utgående e-post för Adobe Commerce i molninfrastruktur.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# Konfigurera utgående e-post

Du kan aktivera och inaktivera utgående e-post för integrerings- (och mellanlagring endast för Starter-miljöer) från [!DNL Cloud Console] eller från kommandoraden. Aktivera utgående e-postmeddelanden för att skicka tvåfaktorsautentisering eller för att återställa lösenordsmeddelanden för användare av Cloud-projekt.

Som standard är utgående e-post aktiverat i produktions- och mellanlagringsmiljöer (endast Pro). Inställningen **[!UICONTROL Enable outgoing emails]** kan dock visas som inaktiverad i miljöinställningarna, oavsett status, tills du ställer in egenskapen `enable_smtp` via [kommandoraden](#enable-emails-in-the-cli) eller [molnkonsolen](outgoing-emails.md#enable-emails-in-the-cloud-console).

Om du uppdaterar egenskapsvärdet `enable_smtp` med [kommandorad](#enable-emails-in-the-cli) ändras även inställningsvärdet [!UICONTROL Enable outgoing emails] för den här miljön på molnkonsolen.

>[!NOTE]
>
>Om du aktiverar/inaktiverar inställningen **[!UICONTROL Enable outgoing emails]** aktiveras/inaktiveras inte e-post i Pro Staging- eller Production-miljöerna.

{{redeploy-warning}}

## Aktivera e-post i molnkonsolen

Använd växlingsknappen **[!UICONTROL Outgoing emails]** i vyn _Konfigurera miljö_ om du vill aktivera eller inaktivera e-poststöd.

Om utgående e-post måste inaktiveras eller återaktiveras i Pro Production- eller Staging-miljöer kan du skicka en [Adobe Commerce Support-biljett](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Status för utgående e-post kanske inte återspeglas för Pro Staging- eller Production-miljöer i Cloud Console.

**Så här hanterar du e-postsupport från[!DNL Cloud Console]**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Välj ett projekt i listan _Alla projekt_.
1. Klicka på konfigurationsikonen i det övre högra hörnet på projektkontrollpanelen.
1. Klicka på **[!UICONTROL Environments]** och välj en specifik miljö i listan (förutom Förproduktion och produktion för Pro).
1. Om du vill aktivera eller inaktivera utgående e-post växlar du _Aktivera utgående e-post_ **På** eller **Av**.

   ![Aktivera konfiguration för utgående e-post](../../assets/outgoing-emails.png)

När du har ändrat inställningen byggs och distribueras miljön med den nya konfigurationen.

## Aktivera e-post i CLI

Du kan ändra e-postkonfigurationen för en aktiv miljö med kommandot `magento-cloud` CLI `environment:info` för att ange egenskapen `enable_smtp`. Om du aktiverar SMTP uppdateras miljövariabeln `MAGENTO_CLOUD_SMTP_HOST` med IP-adressen för SMTP-värden för att skicka e-post.

**Så här hanterar du e-poststöd från kommandoraden**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Kontrollera miljöns inställning för utgående e-post.

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. Ändra konfigurationen för e-postsupport genom att ange miljövariabeln `enable_smtp` till `true` eller `false`.

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   Vänta på att miljön ska byggas och distribueras.

1. Använd en SSH för att logga in i fjärrmiljön.

1. Verifiera att e-postmeddelandet fungerar. Skicka ett test-e-postmeddelande till en adress som du kan kontrollera.

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. Kontrollera att e-postmeddelandet har hämtats av SendGrid.

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
