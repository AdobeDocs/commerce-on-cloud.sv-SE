---
title: Förbered för utveckling
description: Samla in inloggningsuppgifter och lär dig mer om de verktyg som finns för att konfigurera en arbetsyta för utveckling som ska användas med ditt Commerce i molninfrastrukturprojekt.
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# Förbered för utveckling

Oavsett om du är nybörjare på Commerce eller är en befintlig Commerce-ägare som går över till molninfrastrukturen, ska du göra följande för att förbereda en utvecklingsyta för ditt Cloud-projekt. Om du redan har slutfört några av dessa steg eller har en befintlig utvecklarmiljö i Adobe Commerce kan du se följande för att se förväntade resultat och fortsätta till nästa steg. Vissa konfigurationer och arbetsflöden skiljer sig från vanliga lokala installationer.

## Referenser

Innan du konfigurerar en arbetsyta bör du samla in följande nycklar och kontoåtkomst:

- **Autentiseringsnycklar (dispositionsnycklar)**

  Autentiseringsnycklar är 32-teckensautentiseringstoken som ger säker åtkomst till Adobe Commerce Composer-databasen (`repo.magento.com`) och andra Git-tjänster som krävs för programutveckling, till exempel GitHub. Ditt konto kan ha flera autentiseringsnycklar. När du konfigurerar arbetsytan börjar du med en specifik nyckel för koddatabasen. Om du inte har några nycklar kontaktar du projektägaren eller skapar [autentiseringsnycklarna](../cloud-guide/development/authentication-keys.md) själv.

- **Cloud Project-konto**

  Projektägaren bör bjuda in dig till Adobe Commerce för molninfrastrukturprojekt. När du får e-postinbjudan klickar du på länken och följer anvisningarna för att skapa ditt konto. Se [Onboarding](onboarding.md).

- **Adobe Commerce-krypteringsnyckel**

  Om du bara importerar ett befintligt system hämtar du den krypteringsnyckel som används för att skydda åtkomst och data för databasen. Mer information om den här nyckeln finns i [Lös problem med krypteringsnyckeln](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/resolve-issues-with-encryption-key.html)

## Utvecklarverktyg

- **Installera molnet-CLI**

  Installera CLI:n för `magento-cloud` så att du kan hantera molnmiljöer och köra automatiseringsåtgärder. Installationsanvisningar finns i [Cloud CLI](../cloud-guide/dev-tools/cloud-cli-overview.md).

- **Installera Docker för lokal utveckling och testning**

  Du kan också använda Docker-miljön för att emulera Commerce i molninfrastrukturen `integration` för lokal utveckling. Det finns tre viktiga komponenter: en Adobe Commerce v2-mall, Docker Compose och `ece-tools`-paket.

   - [Dockningsarkitektur och vanliga kommandon](../cloud-guide/dev-tools/cloud-docker.md)
   - [Starta Docker-utvecklingsmiljön](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
   - [ECE-verktygspaket](../cloud-guide/dev-tools/package-overview.md)

- **Integrera Git-baserade tjänster**

  Om du vill kan du integrera en Git-baserad värdtjänst, som GitHub eller GitLab, med Adobe Commerce i molninfrastrukturen. Se [Integrationer](../cloud-guide/integrations/overview.md).

## Projektkod

En säker anslutning krävs för att interagera med fjärrmiljöer. För ett nytt projekt [loggar du in på  [!DNL Cloud Console]](https://console.adobecommerce.com) och klickar på **[!UICONTROL No SSH key]**. Den här ikonen finns till höger om kommandofältet och är synlig när projektet inte innehåller någon SSH-nyckel. Se [Säkra anslutningar](../cloud-guide/development/secure-connections.md#add-an-ssh-public-key-to-your-account).

**Så här klonar du din kodbas till din lokala arbetsstation**:

1. Klicka på **[!UICONTROL code]** i [[!DNL Cloud Console]](https://console.adobecommerce.com) och välj fliken **[!UICONTROL Git]**.

   ![Klona din kod](../assets/ui-git-code.png){width="450"}

1. Kopiera det angivna `git clone ...`-kommandot.

1. Skapa och byt till din arbetskatalog i en terminal.

1. Klistra in och kör kommandot `git clone ...`.

>[!TIP]
>
>Adobe förser den ursprungliga projektmiljön med en malldatabas som innehåller paketinstruktioner för en viss version av Adobe Commerce. Granska avsnittet [projektfilstruktur](../cloud-guide/project/file-structure.md) och läs mer om viktiga projektfiler och molnmallar.
