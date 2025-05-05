---
title: Öppna Commerce Admin-panelen
description: Lär dig hur du kommer åt din Commerce Admin-panel.
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 0%

---

# Öppna Commerce Admin-panelen

Användare som har administratörsbehörighet för Commerce Admin-panelen kan lägga till användare, konfigurera butikstjänster, slutföra butiksinstallation och anpassningsarbete och mycket annat.

För ett nytt projekt är det första steget efter att du har fått välkomstmeddelandet att skydda administratörsåtkomst till projektet genom att ändra lösenordet på licensägarkontot. Standardanvändarnamnet för det här kontot är licensägarens e-postadress.

Du kan skicka en begäran om lösenordsändring på något av följande sätt:

- Leta upp det välkomstmeddelande som skickas till e-postadressen till licensägaren och följ länken och ändra lösenordet.

- Kopiera butikens URL från [[!DNL Cloud Console]](../cloud-guide/project/overview.md) till en webbläsare. Lägg sedan till `/admin` i slutet av URL:en för att öppna inloggningssidan. Klicka på det **glömda lösenordet?**-länk för att skicka en begäran om lösenordsändring till e-postadressen för licensägaren.

När du har skickat begäran om lösenordsändring kan du kontrollera om det finns ett meddelande om lösenordsåterställning i e-postmeddelandet. Om du inte får e-postmeddelandet kontrollerar du din skräppostmapp.

>[!TIP]
>
>Om lösenordsåterställningen misslyckas eller du inte kan logga in på Admin-panelen, kan en användare med administratörsåtkomst ansluta till projektet med SSH och lägga till en administratörsanvändare med CLI-kommandot `admin:user:create`. Se [Skapa, redigera eller låsa upp ett administratörskonto](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=sv-SE) i _installationshandboken_.

## Övervaka webbplatshälsa

[Site-Wide Analysis Tool](https://experienceleague.adobe.com/sv/docs/commerce-operations/tools/site-wide-analysis-tool/intro) är ett proaktivt självbetjäningsverktyg och en central lagringsplats med detaljerade systeminsikter och rekommendationer för att säkerställa säkerheten och användbarheten för din Adobe Commerce-installation. Den ger prestandaövervakning, rapporter och råd i realtid dygnet runt alla dagar för att identifiera potentiella problem och bättre synlighet för webbplatsens hälsa, säkerhet och programkonfigurationer. Det minskar upplösningstiden och förbättrar webbplatsens stabilitet och prestanda. Du kan komma åt verktyget för webbplatsövergripande analys direkt från [administratörspanelen](https://experienceleague.adobe.com/sv/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel).
