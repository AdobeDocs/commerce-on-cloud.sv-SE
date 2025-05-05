---
title: ADMIN-variabler
description: Se en lista över miljövariabler som används vid installation av Adobe Commerce i molninfrastruktur.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# Administratörsvariabler

Användare som har administratörsbehörighet för Adobe Commerce i molninfrastrukturprojekt kan använda följande projektmiljövariabler för att åsidosätta konfigurationsinställningarna för administratörskontot för att få åtkomst till administratörsgränssnittet.

## Administratörsreferenser

Du kan åsidosätta administratörens inloggningsuppgifter under Commerce-installationen med ADMIN-variablerna i följande tabell.

Om du vill ändra värdena efter installationen ansluter du till miljön med SSH och använder Adobe Commerce CLI [`admin:user`-kommandot ](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=sv-SE) för att skapa eller redigera administratörens inloggningsuppgifter.

| Variabel | Standard | Beskrivning |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Licensägarens e-postadress | Ett användarnamn för den administrativa användaren med möjlighet att skapa andra användare, inklusive administrativa användare. |
| `ADMIN_EMAIL` |                             | E-postadress till administratören. Den här adressen används för att skicka meddelanden om återställning av lösenord. |
| `ADMIN_PASSWORD` |                             | Lösenord för den administrativa användaren. När projektet skapas skapas ett slumpmässigt lösenord och ett e-postmeddelande skickas till licensägaren. När projektet skapades bör licensägaren redan ha ändrat lösenordet. Kontakta licensägaren för det uppdaterade lösenordet. |
| `ADMIN_LOCALE` | `en_US` | Det standardspråk som används av administratören. |

## Admin-URL

Använd följande miljövariabel för att skydda åtkomsten till ditt administratörsgränssnitt. Om det här värdet anges åsidosätts standardwebbadressen under installationen.

`ADMIN_URL` - Den relativa URL-adressen för åtkomst till administratörsgränssnittet. Standardwebbadressen är `/admin`. Av säkerhetsskäl rekommenderar Adobe att du ändrar standardvärdet till en unik, anpassad Admin-URL som inte är enkel att gissa sig till.

### Ändra Admin-URL

Adobe rekommenderar att du ändrar miljönivåvariabeln för Admin URL efter installationen. Konfigurera den här inställningen av säkerhetsskäl innan du förgrenar dig från den klonade `master`-miljön. Alla grenar som skapats från grenen `master` ärver miljönivåvariablerna och deras värden.

Använd kommandot `magento-cloud variable:update` för att uppdatera variabelvärdet. (Kommandot `variable:set` har tagits bort och är inte tillgängligt.) I följande exempel uppdateras ADMIN_URL till `newAdmin_A8v10`:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master
```

>[!NOTE]
>
>Värdet `ADMIN_URL` accepterar bokstäver (a-z eller A-Z), siffror (0-9) och understreck (_) för en anpassad administratörssökväg. Blanksteg eller andra tecken **accepteras inte**.

**Så här ändrar du URL-adressen med[!DNL Cloud Console]**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Välj ett projekt i listan _Alla projekt_.

1. I projektöversikten markerar du miljön och klickar på konfigurationsikonen.

   ![Projektkonfiguration](../../assets/icon-configure.png){width="36"}

1. Välj fliken **Variabler**.

1. Klicka på **Skapa variabel**.

1. Ange följande:

   - **Variabelnamn** = `ADMIN_URL`
   - **värde** = Ny URL. Ange till exempel Admin URL till `magento_A8v10`.

   Som standard är `Available during runtime` och `Make inheritable` markerade.

1. Klicka på **Skapa variabel** och vänta tills distributionen har slutförts. Den här knappen visas bara när de obligatoriska fälten innehåller värden.
