---
title: Autentiseringsnycklar
description: Lär dig hur du använder autentiseringsnycklar i ett utvecklingsprojekt i Adobe Commerce i molninfrastruktur.
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Autentiseringsnycklar

Du måste ha en autentiseringsnyckel för att få tillgång till Adobe Commerce-databasen och för att kunna aktivera installations- och uppdateringskommandon för ditt Adobe Commerce i molninfrastrukturprojekt. Det finns två metoder för att ange autentiseringsuppgifter för Composer-autentisering.

- **autentiseringsfil** - En fil som innehåller dina Adobe Commerce [autentiseringsuppgifter](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html?lang=sv-SE) i din Adobe Commerce i molninfrastrukturens rotkatalog.
- **miljövariabel** - En miljövariabel som konfigurerar autentiseringsnycklar i ditt Adobe Commerce-infrastrukturprojekt för att förhindra oavsiktlig exponering.

>[!BEGINSHADEBOX]

**Säkerhetsanteckning**

Adobe rekommenderar att du använder metoden [för miljövariabel](#composer-auth-environment-variable) tillsammans med ditt molnprojekt för att förhindra oavsiktlig exponering av dina autentiseringsuppgifter.

Autentiseringsfilmetoden är idealisk när du använder Cloud Docker för Commerce som ett lokalt utvecklingsverktyg, men du bör inte överföra filen `auth.json` till en offentlig Git-baserad databas. Du kan lägga till filen `auth.json` i filen [`.gitignore` &#x200B;](../project/file-structure.md#ignoring-files).

>[!ENDSHADEBOX]

## Autentiseringsfil

**Så här skapar du en `auth.json` fil**:

1. Om du inte har någon `auth.json`-fil i projektets rotkatalog skapar du en.

   - Skapa en `auth.json`-fil i projektets rotkatalog med en textredigerare.
   - Kopiera innehållet i [exemplet `auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample) till den nya `auth.json`-filen.

1. Ersätt `<public-key>` och `<private-key>` med dina autentiseringsuppgifter för Adobe Commerce.

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Spara ändringarna och avsluta textredigeraren.

## Miljövariabel för Composer-autentisering

Följande metod är det bästa sättet att förhindra oavsiktlig exponering av känsliga uppgifter i en offentlig Git-baserad databas.

**Så här lägger du till autentiseringsnycklar med en miljövariabel**:

1. Klicka på konfigurationsikonen till höger om projektnavigeringen i _[!DNL Cloud Console]_.

   ![Konfigurera projekt](../../assets/icon-configure.png){width="36"}

1. Klicka på **[!UICONTROL Variables]** i listan _Projektinställningar_.

1. Klicka på **[!UICONTROL Create variable]**.

1. Ange `env:COMPOSER_AUTH` i fältet **[!UICONTROL Variable name]**.

1. I fältet _Värde_ lägger du till följande och ersätter `<public-key>` och `<private-key>` med dina autentiseringsuppgifter för Adobe Commerce:

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Markera **[!UICONTROL Available during buildtime]** och avmarkera **[!UICONTROL Available during runtime]**.

1. Klicka på **[!UICONTROL Create variable]**.

1. Ta bort filen `auth.json` från varje miljö.
