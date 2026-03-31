---
title: ADMIN-variabler
description: Se en lista över miljövariabler som används vid installation av Adobe Commerce i molninfrastruktur.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: d2746185-bc59-4d30-a088-73df1bd2c0b2
source-git-commit: ac1b2001294ba72304fc7ad3c760872dbd73e44f
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 0%

---

# Administratörsvariabler

Användare som har administratörsbehörighet för Adobe Commerce i molninfrastrukturprojekt kan använda följande projektmiljövariabler för att åsidosätta konfigurationsinställningarna för administratörskontot för att få åtkomst till administratörsgränssnittet.

## Administratörsreferenser

Du kan åsidosätta administratörens inloggningsuppgifter under Commerce-installationen med ADMIN-variablerna i följande tabell.

Om du vill ändra värdena efter installationen ansluter du till miljön med SSH och använder Adobe Commerce CLI [`admin:user`-kommandot &#x200B;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) för att skapa eller redigera administratörens inloggningsuppgifter.

| Variabel | Standard | Beskrivning |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Användarnamn för licensägare | Ett användarnamn för den administrativa användaren med möjlighet att skapa andra användare, inklusive administrativa användare. |
| `ADMIN_FIRSTNAME` | Licensägarens förnamn | Förnamnet för den administrativa användaren. |
| `ADMIN_LASTNAME` | Licensägarens efternamn | Det administrativa användarnamnet. |
| `ADMIN_EMAIL` | E-postadress till licensägare | E-postadress till administratören. Den här adressen används för att skicka meddelanden om återställning av lösenord. |
| `ADMIN_PASSWORD` | Lösenord för licensägare | Lösenord för den administrativa användaren. När projektet skapas skapas ett slumpmässigt lösenord och ett e-postmeddelande skickas till licensägaren. När projektet skapades bör licensägaren redan ha ändrat lösenordet. Kontakta licensägaren för det uppdaterade lösenordet. |
| `ADMIN_LOCALE` | `en_US` | Det standardspråk som används av administratören. |

## Admin-URL

Använd följande miljövariabel för att skydda åtkomsten till ditt administratörsgränssnitt. Om det här värdet anges åsidosätts standardwebbadressen under installationen. I [!DNL Adobe Commerce] för molninfrastruktur måste du ange eller ändra Admin URL med variabeln `ADMIN_URL` i ([!DNL Cloud Console] eller [!DNL Cloud CLI]). Inställningen från [!DNL Admin] kan bara ändras för lokala installationer.

`ADMIN_URL` - Den relativa URL-adressen för åtkomst till administratörsgränssnittet. Standardwebbadressen är `/admin`.

### Ändra Admin-URL

Som standard är URL:en för [Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/start/admin/admin.html) inställd på *&lt;domain_name>/admin*. Av säkerhetsskäl rekommenderar Adobe att du ändrar den till en unik, anpassad administratörs-URL som inte är enkel att gissa sig till.

**I [!DNL Adobe Commerce] i molninfrastrukturen** måste du ändra Admin-URL:en med hjälp av miljövariabeln `ADMIN_URL` i ([!DNL Cloud Console] eller [!DNL Cloud CLI]). Inställningen från [!DNL Admin] kan bara ändras för lokala installationer. För lokala installationer följer du [använd en anpassad administratörs-URL](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html#use-a-custom-admin-url).

Adobe rekommenderar att du ändrar miljönivåvariabeln för Admin URL efter installationen. Konfigurera den här inställningen av säkerhetsskäl innan du förgrenar dig från den klonade `master`-miljön. Alla grenar som skapas från grenen `master` ärver miljönivåvariablerna och deras värden om du inte anger arv som false.

Använd antingen [!DNL Cloud Console] eller [!DNL Cloud CLI] för att ställa in eller uppdatera `ADMIN_URL`.

#### Alternativ A: Ändra Admin-URL med [!DNL Cloud Console]

##### Integreringsmiljö

Lägg till en ny variabel med [Cloud Console](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html):

- **Namn:** `ADMIN_URL`
- **Värde:** Din nya Admin URL (till exempel `magento_A8v10`)

- Mer information finns i [Lägga till miljövariabler](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html#configure-environment) eller [miljövariabler](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-admin.html) i utvecklardokumentationen.

##### Ange Admin-URL i [!DNL Cloud Console]

1. Logga in på [molnkonsolen](https://console.adobecommerce.com/).
2. Välj ett projekt i listan **[!UICONTROL All projects]**.
3. I projektöversikten markerar du miljön och klickar på konfigurationsikonen.
4. Välj fliken **[!UICONTROL Variables]**.
5. Klicka på **[!UICONTROL Create Variable]** (eller redigera den befintliga `ADMIN_URL`-variabeln om den finns).
6. Ange följande:
   - **Variabelnamn:** `ADMIN_URL`
   - **Värde:** Din nya administratörssökväg (till exempel `magento_A8v10`).

   Som standard är **[!UICONTROL Available during runtime]** och **[!UICONTROL Make inheritable]** markerade. Om du vill förhindra att underordnade miljöer ärver det här värdet rensar du **[!UICONTROL Make inheritable]** för den här variabeln.
7. Klicka på **[!UICONTROL Create variable]** (eller **[!UICONTROL Save]**) och vänta tills distributionen har slutförts. Knappen visas bara när de obligatoriska fälten innehåller värden.

##### När mellanlagring och produktion inte är tillgängliga i [!DNL Cloud Console]

[Skicka en supportanmälan](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket) som begär att få lägga till variabeln `ADMIN_URL` för din mellanlagrings- eller produktionsmiljö. Om Förproduktion och Förproduktion är tillgängliga från [!DNL Cloud Console] lägger du till variabeln enligt beskrivningen i [Integreringsmiljö](#integration-environment).

#### Alternativ B: Ändra Admin-URL med [!DNL Cloud CLI]

Uppdatera variabeln med kommandot `magento-cloud variable:update`. (Kommandot `variable:set` har tagits bort och är inte tillgängligt.)

I följande exempel uppdateras `master`-miljön `ADMIN_URL` till `newAdmin_A8v10` och underordnade miljöer kan inte ärva värdet:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master --inheritable false
```

- **Omdistribution:** Om du ändrar variabeln `ADMIN_URL` i [!DNL Cloud CLI] utlöses en omdistribution av miljön.
- **Arv:** Variabler kan ärvas som standard. Använd alternativet `--inheritable false` så som visas för att förhindra att värdet ärvs av underordnade miljöer. Mer information finns i [Synlighet på variabelnivå](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/variable-levels.html#visibility).

>[!NOTE]
>
>Värdet `ADMIN_URL` accepterar bokstäver (a-z, A-Z), siffror (0-9) och understreck (_). Blanksteg och andra tecken accepteras inte.
