---
title: New Relic Kontohantering
description: Lär dig hur du får tillgång till ditt New Relic-konto och hanterar åtkomst, integrering och verktygshantering för ditt Adobe Commerce i molninfrastrukturprojekt.
feature: Cloud, Observability
role: Admin
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '660'
ht-degree: 0%

---

# New Relic kontohantering

När Adobe tillhandahåller ditt molninfrastrukturprojekt får licensägaren ett e-postmeddelande från New Relic med uppgifter och instruktioner för hur man får åtkomst till New Relic-kontot. Om du inte fått e-postmeddelandet kan du återställa New Relic-lösenordet med hjälp av e-postadressen till licensägaren.

## Hantera användaråtkomst

>[!NOTE]
>
>Ge endast fullständig åtkomst till användare som strikt kräver åtkomst till hela funktionsuppsättningen.

**Så här får du åtkomst till användarhantering i New Relic**:

1. Logga in på ditt [New Relic-konto](https://login.newrelic.com/login).

1. Välj ditt användarnamn i den nedre vänstra navigeringen.

1. Klicka på **[!UICONTROL Administration]** och välj något av följande i listan:

   - **[!UICONTROL User management]** om du vill lägga till en användare och hantera aktiva användare och väntande inbjudningar.

   - **[!UICONTROL Access management]** om du vill hantera användargrupper, roller och konton.

Se [Användarhantering](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/) i dokumentationen för _New Relic_.

## Konfigurera New Relic för Starter-miljön

>[!NOTE]
>
>**Pro-miljöer** är förkonfigurerade för att använda New Relic-tjänster och kan hoppa över instruktioner för att aktivera och ansluta. Om New Relic APM inte är installerat i miljö för förproduktion och produktion, eller om New Relic Infrastructure inte är tillgänglig i produktionsmiljön, [skickar du en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) för att begära installation.

I Starter-miljöer måste du kontrollera filen `.magento.app.yaml` för att verifiera att sektionen `runtime` innehåller New Relic-tillägget. Om tillägget inte har konfigurerats lägger du till följande:

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### Använd licensnyckel

Om du vill ansluta en molnmiljö till New Relic lägger du till New Relic licensnyckel i miljön.

- För **Pro-projekt** lägger Adobe till licensnyckeln i produktions- och mellanlagringsmiljöerna under provisioneringsprocessen. Du kan logga in på ditt [New Relic-konto](https://login.newrelic.com/login) för att verifiera anslutningen mellan din Adobe Commerce på molninfrastruktursajten och New Relic.

- För **startprojekt** har du en licensnyckel för New Relic med stöd för upp till _tre_ miljöer. Du måste lägga till nyckeln i dina miljökonfigurationer manuellt. Startmiljöer är inte förprovisionerade för att använda New Relic-tjänsten.

I Starter-miljöer aktiverar du integreringen av New Relic genom att lägga till New Relic-licensnyckeln i miljökonfigurationen. Lägg till nyckeln i miljö för förproduktion och produktion och en annan miljö som du väljer. Endast licensnyckeln för New Relic krävs för konfigurationen. Mer information om ytterligare konfigurationsalternativ finns i avsnittet [New Relic Reporting](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html?lang=sv-SE) i _Adobe Commerce användarhandbok_.

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Inloggningsuppgifter för Adobe Commerce-kontosidan eller för den New Relic-licens som är kopplad till ditt projekt
>- [Åtkomst på administratörsnivå](../project/user-access.md) till startmiljöerna för att konfigurera
>- Autentiseringsuppgifter för åtkomst till [Admin](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html?lang=sv-SE) för miljön

**Så här konfigurerar du New Relic för Starter-miljöer**:

1. Hitta din licensnyckel för New Relic från [!DNL Cloud Console] eller molnet-CLI.

   **[!DNL Cloud Console]metod**:

   - Öppna ditt molnprojekt [på kontosidan](https://accounts.magento.cloud/user).

   - Hitta ditt projekt på fliken _Projekt_.

   - Klicka på **Visa detaljer** om du vill se information om projektinfrastrukturen.

   - Expandera avsnittet **New Relic-tjänst** om du vill visa licensnyckeln.

   - Kopiera licensnyckeln.

   **Cloud CLI-metod**:

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. Lägg till licensnyckeln för New Relic i en miljö med CLI:n för `magento-cloud`.

   - Byt till den miljö som behöver licensnyckeln.
   - Uppdatera variabelvärdet med följande `magento-cloud` CLI-kommando:

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   Du kan också lägga till det från [Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html?lang=sv-SE#step-3%3A-configure-your-store).

1. Logga in på ditt [New Relic-konto](https://login.newrelic.com/login) för att verifiera att du kan visa data från Adobe Commerce-miljön. Se [Undersök prestanda](investigate-performance.md).

### Ta bort licensnyckel

Du kan bara använda din licensnyckel för New Relic i tre aktiva miljöer. Om nyckeln används i tre miljöer måste du ta bort nyckeln från en av miljöerna innan du kan lägga till den i en annan miljö.

**Så här tar du bort en licensnyckel från en miljö**:

1. Visa miljövariabler.

   ```bash
   magento-cloud variable:list
   ```

   Exempelsvar:

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >Om du har lagt till licensnyckeln som en _projekt_ -variabel måste du ta bort den variabeln på projektnivå. En projektvariabel lägger till licensen till _varje_ miljögren som skapats, som kan förbruka eller överskrida licensgränsen. Visa projektvariabler: `magento-cloud variable:list --level project`

1. Ta bort licensvariabeln.

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```
