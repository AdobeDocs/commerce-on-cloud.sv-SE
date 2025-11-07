---
title: Hantera användaråtkomst
description: Lär dig hur du hanterar användaråtkomst till Adobe Commerce i molninfrastrukturprojekt och -miljöer.
role: Admin
feature: Cloud, Roles/Permissions
last-substantial-update: 2023-06-27T00:00:00Z
topic: Security
exl-id: 953593de-f675-49fd-988f-f11306f67fbd
source-git-commit: c972d9f2029499cf53edc334c1d9a40b155a991d
workflow-type: tm+mt
source-wordcount: '1463'
ht-degree: 0%

---

# Hantera användaråtkomst

Adobe Commerce-projekt om molninfrastruktur använder rollbaserad åtkomst. Det finns två tillgängliga roller på projektnivå:

- **Projektadministratör** - Skriv åtkomst till alla projektmiljöer och hantera användare, push-kod och uppdatera projektinställningar. (Tidigare känd som **Superadministratör**)
- **Project Viewer** - Åtkomst endast till alla projektmiljöer.

Projektvisningsprogram kan inte utföra åtgärder i någon miljö, men du kan ge projektvisningsprogram skrivåtkomst till en viss miljötyp.

Åtkomst på miljönivå baseras på miljötypen: produktion, staging och utveckling. Om användaren _viewer_ beviljas behörighet till _development_ -miljöer innebär det att användaren kan visa **alla** utvecklingsmiljöer i projektet. I följande tabell klargörs vilka möjligheter som ges för varje behörighetsnivå:

| Behörighetsnivå | Åtkomst | SSH-åtkomst |
| ------------------ | ----------- | :----------: |
| **Admin** | Utföra administratörsuppgifter, t.ex. ändra inställningar, push-kod, utföra uppgifter och grenhantering, inklusive sammanfogning med den överordnade miljön | Ja |
| **Deltagare** | Kodning och förgreningar i miljön - det går inte att ändra inställningar eller utföra åtgärder | Ja |
| **Visningsprogram** | Åtkomst endast för visning till miljötypen | Nej |
| **Ingen åtkomst** | Ingen åtkomst till miljötypen | Nej |

{style="table-layout:auto"}

Du kan lägga till användare och tilldela roller med `magento-cloud` CLI eller [!DNL Cloud Console].

>[!BEGINSHADEBOX]

**Förutsättningar:**

- En registrerad användare hos en Adobe ID. En användare måste [registrera sig för ett Adobe-konto](https://account.adobe.com) och sedan initiera sitt [Cloud-konto](https://console.adobecommerce.com) genom att gå till [https://console.adobecommerce.com](https://console.adobecommerce.com) innan du kan lägga till dem i ett Cloud-projekt.
- En användare som tilldelats rollen **Admin** kan inte hantera användare med CLI:n `magento-cloud`. Endast användare som har tilldelats rollen **Kontoägare** kan hantera användare.

>[!ENDSHADEBOX]

## Hantera användare med CLI

Använd CLI:n för `magento-cloud` för att hantera användare och integrera med automatiserade system:

- `magento-cloud user:add`-lägg till en användare i projektet
- `magento-cloud user:delete`-ta bort en användare
- `magento-cloud user:list [users]`-listprojektanvändare
- `magento-cloud user:role`-visa eller ändra användarrollen
- `magento-cloud user:update`-uppdatera användarroll i ett projekt

I följande exempel används CLI:n `magento-cloud` för att lägga till en användare, konfigurera roller, ändra projekttilldelningar och tilldela användarroller.

**Så här lägger du till en användare och tilldelar roller**:

1. Använd CLI:n för `magento-cloud` för att lägga till användaren.

   ```bash
   magento-cloud user:add
   ```

   >[!IMPORTANT]
   >
   >Användaren måste ha en Adobe ID; se [förutsättningarna](#add-users-and-manage-access).

1. Följ anvisningarna: ange användarens e-postadress, ange projekt- och miljörollerna och lägg till användaren.

   > Exempeluppmaningar

   ```
   Enter the user's email address: alice@example.com
   
   Email address: alice@example.com
   
   The user's project role can be admin (a) or viewer (v).
   
   Project role (default: viewer) [a/v]: viewer
   
   The user's environment type role(s) can be admin (a), viewer (v), contributor (c) or none (n).
   
   Role on type development (default: none) [a/v/c/n]: none
   Role on type production (default: none) [a/v/c/n]: admin
   Role on type staging (default: none) [a/v/c/n]: admin
   
   Adding the user alice@example.com to (project_id):
   Project role: viewer
     Role on type production: admin
     Role on type staging: admin
   
   Are you sure you want to add this user? [Y/n] y
   Adding the user to the project
   ```

   När du har lagt till användaren skickar Adobe ett e-postmeddelande till den angivna adressen med instruktioner om hur du får tillgång till Adobe Commerce i molninfrastrukturprojektet.

### Visa en användares projektroll

```bash
magento-cloud user:get alice@example.com
```

>Exempelsvar:

```
Current role(s) of User (alice@example.com) on Production (project_id):
  Project role: admin
```

### Lägga till en användare i flera miljöer

Så här lägger du till en användare som `viewer` i en `Production`-miljö och som `contributor` i en `Integration`-miljö:

```bash
magento-cloud user:add alice@example.com -r production:v -r integration:c
```

### Uppdatera behörigheter för användarmiljö

Så här uppdaterar du användarmiljöbehörigheter till `admin` i `Production`-miljön:

```bash
magento-cloud user:update alice@example.com -r production:a
```

## Hantera användare från [!DNL Cloud Console]

Du kan använda [[!DNL Cloud Console]](../../get-started/cloud-console.md) för att lägga till behörigheter och använda funktionen _Redigera_ för att ändra behörigheter för en befintlig användare.

>[!IMPORTANT]
>
>Användaren måste ha en Adobe ID; se [förutsättningarna](#add-users-and-manage-access).

### Lägg till en användare i projektet

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com/).

1. Välj ett projekt i listan _Alla projekt_.

1. Klicka på konfigurationsikonen i det övre högra hörnet på projektkontrollpanelen.

1. Klicka på _under_ Projektinställningar **[!UICONTROL Access]**.

1. Klicka på _i vyn_&#x200B;Åtkomst **[!UICONTROL Add]**.

1. Fyll i formuläret _[!UICONTROL Add User]_:

   - Ange användarens e-postadress.

   - **[!UICONTROL Project admin]** - Bevilja administratörsbehörighet för alla inställningar och miljötyper.

   - **[!UICONTROL Environment types and permissions]** - Bevilja åtkomst och specifika behörighetsnivåer för vissa miljötyper. _Ingen åtkomst_, _Admin_ (ändra inställningar, kör åtgärd, sammanfoga kod), _Contributor_ (push-kod) eller _Viewer_ (endast vy).

   >[!TIP]
   >
   >Endast en **projektadministratör** kan hantera användare i alla miljöer. Om du vill ge en användare åtkomst till fliken **Åtkomst** måste en annan **projektadministratör** eller **kontoägare** tilldela användaren rollen **Projektadministratör**.

1. Klicka på **[!UICONTROL Add User]**.

   >[!IMPORTANT]
   >
   >När du lägger till en användare aktiveras inte distributionen automatiskt.

1. Distribuera om alla miljöer för att tillämpa ändringarna när du har lagt till användare. När du lägger till en användare aktiveras inte distributionen automatiskt. Omdistribution är ett viktigt steg för att se till att användaren kan komma åt en miljö med SSH eller utföra administratörsuppgifter.

När du har lagt till användaren skickar Adobe ett e-postmeddelande till den angivna adressen med instruktioner om hur du får tillgång till Adobe Commerce i molninfrastrukturprojektet.

## Krav för användarautentisering

För ökad säkerhet tillhandahåller Adobe multifaktorautentisering (MFA) på projektnivå för att kräva tvåfaktorsautentisering (TFA) för SSH-åtkomst till Adobe Commerce i källkod och miljöer för molninfrastrukturprojekt. Se [Aktivera MFA för SSH](multi-factor-authentication.md).

När användningen av MFA är aktiverad i ett Adobe Commerce-projekt för molninfrastruktur måste alla användare med SSH-åtkomst till en miljö i det projektet aktivera TFA på sina Adobe Commerce-konton för molninfrastruktur. För automatiserade processer kan du skapa en datoranvändare och en API-token som autentiseras från kommandoraden.

När du har lagt till en användare i ett Cloud-projekt ber du användaren att granska säkerhetsinställningarna för kontot och lägga till följande säkerhetskonfigurationer efter behov:

- **Aktivera TFA** - Uppfyll säkerhets- och kompatibilitetsstandarder genom att konfigurera tvåfaktorsautentisering. Projekt som har konfigurerats med [MFA-tvång](multi-factor-authentication.md) kräver TFA på konton som använder SSH för att komma åt projekten.

- **Aktivera SSH-nycklar** - Användare som behöver åtkomst till Adobe Commerce i källkodsdatabaser för molninfrastruktur måste aktivera SSH-nycklar för sitt konto. Se [Säkra anslutningar](../development/secure-connections.md).

- **Skapa en API-token** - Användare måste generera en API-token som används för SSH-åtkomst till en miljö. Du behöver en token för att aktivera autentiseringsarbetsflöden för automatiserade processer.

  I projekt där MFA-tvång är aktiverat måste du använda API-token för att autentisera SSH-åtkomstbegäranden från automatiserade konton. Med denna token kan automatiserade processer kringgå autentiseringsarbetsflöden som kräver TFA.

### Aktivera TFA för molnkonton

Adobe Commerce i molninfrastrukturen stöder TFA med något av följande program:

- [Google Authenticator (Android/iPhone)](https://support.google.com/accounts/answer/1066447?hl=en)
- [Autentisera (Android/iPhone)](https://authy.com/features/)
- [FreeOTP (Android)](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp)
- [GAuth-autentiserare (Firefox OS, skrivbord, övrigt)](https://github.com/gbraad-apps/gauth)

Instruktioner för hur du installerar autentiseringsprogrammet och aktiverar TFA finns på sidan _Kontoinställningar_ i [!DNL Cloud Console].

**Så här aktiverar du TFA på ditt användarkonto**:

1. Logga in på [ditt konto](https://console.adobecommerce.com).

1. Klicka på **[!UICONTROL My Profile]** på den övre högra kontomenyn.

1. Klicka på _på fliken_ Säkerhet **[!UICONTROL Set up application]**.

1. Om du inte har något godkänt autentiseringsprogram på din mobila enhet kan du installera ett med hjälp av de länkade instruktionerna.

1. Lägg till ditt Adobe Commerce-konto i molninfrastrukturen i autentiseringsprogrammet.

   - Öppna autentiseringsprogrammet på din mobila enhet. Lägg sedan till installationskoden i programmet.

   - På sidan [!UICONTROL **[!UICONTROL TFA set up - Application]**] skriver du TFA-koden från den mobila enheten i fältet **[!UICONTROL Application verification code]**.

   - Klicka på **[!UICONTROL Verify and save]**.

     Om koden är giltig skickar Adobe ett meddelande till kontots e-postadress som bekräftar att kontot nu har TFA.

1. Valfritt. Aktivera inställningarna för _Betrodd webbläsare_ för att cachelagra autentiseringskoden i webbläsaren i 30 dagar.

   Den här konfigurationen minskar antalet autentiseringsproblem vid projektinloggning.

1. Klicka på **Spara** eller **Hoppa över**.

1. Spara återställningskoderna.

   - Kopiera och spara återställningskoderna på sidan _TFA-konfiguration - återställningskoder_ så att du kan logga in på ditt Adobe Commerce i molninfrastrukturprojekt när du inte har tillgång till din mobila enhet eller autentiseringsapplikation.

   - Kopiera återställningskoderna till en annan plats eller skriv ned dem om du skulle förlora åtkomsten till enheten eller autentiseringsprogrammet.

   - Klicka på **Spara** för att spara koderna på ditt konto så att du kan visa och hantera dem från säkerhetsinställningarna för ditt konto.

     >[!WARNING]
     >
     >Om du förlorar åtkomsten till ett konto med TFA och inte har återställningskodlistan, måste du kontakta projektadministratören eller [skicka en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) för att återställa TFA-programmet.

1. När du är klar med TFA-konfigurationen klickar du på **Spara** för att uppdatera ditt konto.

1. Autentisera din aktuella session med TFA.

   - Logga ut från ditt konto.
   - Logga in med ditt användarnamn och lösenord.
   - Ange TFA-koden för posten `accounts.magento.cloud` från autentiseringsprogrammet på din mobila enhet när du uppmanas till det.

### Hantera TFA-konfiguration och återställningskoder

Du kan hantera TFA-konfigurationen för ett Adobe Commerce-konto för molninfrastruktur i avsnittet _Säkerhet_ på sidan _Min profil_.

1. Logga in på [ditt konto](https://console.adobecommerce.com).

1. Klicka på **[!UICONTROL My Profile]** på den övre högra kontomenyn.

1. Klicka på fliken _på sidan_ Min profil **[!UICONTROL Security]**.

1. Använd de tillgängliga länkarna för att uppdatera TFA-inställningarna för ditt Adobe Commerce-konto för molninfrastruktur:

   - Inaktivera TFA
   - Återställ autentiseringsprogrammet
   - Lägga till eller ta bort betrodda webbläsare
   - Visa eller uppdatera TFA-återställningskoder på ditt konto

### Skapa en API-token

En API-token kan bytas ut mot en OAuth 2-åtkomsttoken, som sedan kan användas för att autentisera begäranden.

I projekt där användningen av MFA är aktiverad måste du ha en API-token för att aktivera SSH-åtkomst för datoranvändare och automatiserade processer.

>[!IMPORTANT]
>
>Skydda API-tokenvärden för ditt konto. Visa inte värdet i kodexempel, skärmdumpar eller osäker kommunikation mellan klient och server. Visa inte heller värdet i källkod som lagras i offentliga databaser.

**Så här skapar du en API-token**:

1. Logga in på [ditt konto](https://console.adobecommerce.com).

1. Klicka på **[!UICONTROL My Profile]** på den övre högra kontomenyn.

1. Klicka på fliken _på sidan_ Min profil **[!UICONTROL API tokens]**.

1. Klicka på **[!UICONTROL Create API token]** och ange ett namn, till exempel ett namn som matchar datoranvändaren eller den automatiska process som använder API-token.

   ![API-token](../../assets/api-token-name.png)

1. Klicka på **[!UICONTROL Create API token]**.
