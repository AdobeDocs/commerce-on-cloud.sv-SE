---
title: Säkra anslutningar
description: Lär dig hur du använder SSH-nycklar i ditt Adobe Commerce i molninfrastrukturprojekt och loggar in på fjärrmiljöer.
role: Developer
feature: Cloud, Security
topic: Security
exl-id: 73af13d8-7085-4ac8-9cfe-9772bc6bc112
source-git-commit: 9c0b4bea11abb2ce5644556ab3dadd361f8ff449
workflow-type: tm+mt
source-wordcount: '1010'
ht-degree: 0%

---

# Säkra anslutningar till fjärrmiljöer

Secure Shell (SSH) är ett vanligt protokoll som används för att logga in på fjärrservrar och -system på ett säkert sätt. Du kan använda SSH för att komma åt fjärrmiljöer för att hantera Adobe Commerce-programmet och komma åt fjärrmiljöloggar. Adobe har bara stöd för säkra FTP-anslutningar (sFTP) med din offentliga SSH-nyckel. FTP-anslutningar stöds inte.

## Generera ett SSH-nyckelpar

Skapa ett SSH-nyckelpar på alla datorer och arbetsytor som kräver åtkomst till projektets källkod och miljöer. Med SSH-nyckeln kan du ansluta till GitHub för att hantera källkod och ansluta till molnservrar utan att kontinuerligt behöva ange ditt användarnamn och lösenord. Mer information om hur du skapar ett SSH-nyckelpar finns i [Ansluta till GitHub med SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

- Den _publika nyckeln_ är säker att ge åtkomst till en plats, SSH och sFTP.
- Den _privata nyckeln_ förblir privat på arbetsstationen.

>[!CAUTION]
>
>**Dela aldrig din privata nyckel.** Lägg inte till den i en biljett, kopiera den till en chatt eller bifoga den till e-postmeddelanden.

## Lägg till en offentlig SSH-nyckel till ditt konto

När du har lagt till eller uppdaterat din offentliga SSH-nyckel till ditt Adobe Commerce-konto för molninfrastruktur kan du [omdistribuera alla aktiva miljöer](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-reference#environmentredeploy) på ditt konto för att installera nyckeln.

Du kan lägga till SSH-nycklar till ditt konto på något av följande sätt: Cloud CLI eller [!DNL Cloud Console].

>[!BEGINTABS]

>[!TAB CLI]

### Lägg till din SSH-nyckel med hjälp av CLI för molnet

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Logga in på ditt projekt:

   ```bash
   magento-cloud login
   ```

1. Lägg till den offentliga nyckeln.

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>Du kan visa och ta bort SSH-nycklar med kommandona `ssh-key:list` och `ssh-key:delete` i molnet.

>[!TAB Konsol]

### Lägg till din SSH-nyckel med [!DNL Cloud Console]

**Så här lägger du till en SSH-nyckel i ett nytt projekt**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Klicka på **[!UICONTROL No SSH key]**. Den här ikonen finns till höger om kommandofältet och är synlig när projektet inte innehåller någon SSH-nyckel.

1. Kopiera och klistra in innehållet i den offentliga SSH-nyckeln i fältet **Offentlig nyckel**.

1. Följ återstående instruktioner.

**Så här lägger du till en SSH-nyckel i din molnprofil**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Klicka på **Min profil** på den övre högra kontomenyn.

1. Klicka på _Lägg till offentlig nyckel_ i vyn **SSH-nycklar**.

1. I formuläret _Lägg till en SSH-nyckel_ ger du nyckeln namnet **Titel** och klistrar in den offentliga SSH-nyckeln i fältet **Nyckel**.

1. Klicka på **Spara**.

>[!ENDTABS]

## Ansluta till en fjärrmiljö

Du kan ansluta till en fjärrmiljö med `magento-cloud` CLI eller SSH-kommandot. CLI-kommandona `magento-cloud` kan bara användas i integreringsmiljöer för Starter och Pro.

### Använda CLI i molnet

**Så här loggar du in i en fjärrintegreringsmiljö**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Lista miljöerna i det projektet.

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. Använd SSH för att logga in i fjärrmiljön.

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### Använda ett SSH-kommando

[!DNL Cloud Console] innehåller en lista med kommandon för webb- och SSH-åtkomst för varje miljö.

**Så här kopierar du SSH-kommandot**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Välj ett projekt i listan _Alla projekt_.

1. Välj en miljö.

1. Klicka på **[!UICONTROL SSH]**.

1. Klicka på knappen Kopiera på fliken _SSH_ för att kopiera det fullständiga SSH-kommandot till Urklipp.

1. Öppna en terminal och klistra in SSH-kommandot för att skapa en anslutning.

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>För Pro Staging- och Production-miljöer kan SSH-kommandot se ut så här:
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

Adobe Commerce i molninfrastrukturen har stöd för åtkomst till dina miljöer med sFTP (säker FTP) med SSH-autentisering. Använd en klient som har stöd för SSH-nyckelautentisering för sFTP och använd den offentliga SSH-nyckeln. Den offentliga SSH-nyckeln måste läggas till i målmiljön. För Starter-miljöer och Pro-integreringsmiljöer kan du [lägga till den via  [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface).

Skrivskyddade sFTP-anslutningar stöds _inte_; sFTP-åtkomst tillhandahålls med behörigheten _write_ som standard.

När du konfigurerar sFTP använder du informationen från SSH-åtkomstmiljökommandot: `<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **Användarnamn**: Allt innehåll före `@` i ditt SSH-åtkomstmål.
- **Lösenord**: Du behöver inget lösenord för sFTP. sFTP-åtkomst använder SSH-nyckelautentisering.
- **Värd**: Allt innehåll efter `@` i din SSH-åtkomst.
- **Port**: 22, som är standardport för SSH.
- **Privat nyckel för SSH**: Ange platsen för din privata nyckel till sFTP-klienten om det behövs. Som standard lagras privata nycklar i katalogen `~/.ssh`.

Beroende på klienten kan ytterligare alternativ behövas för att slutföra SSH-autentiseringen för sFTP. Granska dokumentationen för den valda klienten.

För **startmiljöer och Pro-integreringsmiljöer** kan du också överväga att [lägga till en `mount`](../application/properties.md#mounts) för åtkomst till en viss katalog. Du skulle lägga till monteringen i din `.magento.app.yaml`-fil. En lista över skrivbara kataloger finns i [Projektstruktur](../project/file-structure.md). Den här monteringspunkten fungerar bara i dessa miljöer.

Om du inte har SSH-åtkomst till miljön i **Pro-miljöer för mellanlagrings- och produktionsmiljöer** måste du [skicka en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) för att begära sFTP-åtkomst och en monteringspunkt för åtkomst till den specifika mappen, t.ex. `pub/media`.

>[!NOTE]
>Om sFTP-anslutningen för Pro Staging and Production är avsedd för en _generisk_-användare som **inte** behöver läggas till [i Cloud-projektet](../project/user-access.md) måste du [skicka en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) med den bifogade **publika**-nyckeln. **Ange aldrig din privata SSH-nyckel.**

## SSH-tunnel

Du kan använda SSH-tunnel för att ansluta till en tjänst från den lokala utvecklingsmiljön som om tjänsten var lokal. Konfigurera din [SSH](#add-an-ssh-public-key-to-your-account) innan du tunnlar.

Använd ett terminalprogram för att logga in och skicka ut kommandon.

```bash
magento-cloud login
```

Kontrollera om några tunnlar är öppna med.

```bash
magento-cloud tunnel:list
```

Om du vill skapa en tunnel måste du känna till [programnamnet](../application/properties.md#name). Du kan kontrollera programnamnet med CLI:

```bash
magento-cloud apps
```

### Konfigurera SSH-tunneln

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

Om du till exempel vill öppna en tunnel till grenen `sprint5` i ett projekt med en app med namnet `mymagento` anger du

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

Exempelsvar:

```
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**Så här visar du information om tunneln**:

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### Anslut till tjänster

När du har skapat en SSH-tunnel kan du ansluta till tjänster som om de körs lokalt. Om du till exempel vill ansluta till databasen använder du följande kommando:

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```

#### Hämta MySQL-autentiseringsuppgifter

Hämta inloggningsuppgifterna för MySQL från egenskaperna `database` i miljövariabeln `$MAGENTO_CLOUD_RELATIONSHIPS`. Instruktioner om hur du hämtar information i en lokal miljö eller fjärrmiljö finns i [Tjänstrelationer](../services/services-yaml.md#service-relationships).
