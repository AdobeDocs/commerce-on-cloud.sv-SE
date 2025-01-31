---
title: Aktivera multifaktorautentisering för SSH-åtkomst
description: Lär dig hur du hanterar autentiseringskrav för SSH-åtkomst till Adobe Commerce i molninfrastrukturmiljöer.
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Aktivera multifaktorautentisering för SSH-åtkomst

För ökad säkerhet erbjuder Adobe Commerce i molninfrastruktur multifaktorautentisering (MFA) för att hantera autentiseringskrav för SSH-åtkomst till molnmiljöer.

När MFA är aktiverat i ett projekt kräver alla användarkonton med SSH-åtkomst antingen en TFA-kod (tvåfaktorsautentisering) eller en API-token och ett SSH-certifikat för att få åtkomst till miljön.

>[!NOTE]
>
>MFA är inte aktiverat i molnprojekt som standard. Kontoägaren för Adobe Commerce i molninfrastrukturprojektet måste [skicka en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) för att kunna aktivera det. När MFA är aktiverat måste alla användare ha tvåfaktorsautentisering (TFA) aktiverad på sina Adobe Commerce-konton för molninfrastruktur för SSH-åtkomst till projektmiljöerna.

## Certifikat för SSH-åtkomst

Med MFA kan användare utbyta en OAUTH-åtkomsttoken med ett kort SSH-certifikat som genererats av Adobe Cloud Certifier API. Om användaren har rollen Admin eller Contributor, en giltig SSH-nyckel och en giltig TFA-kod eller API-token, använder Adobe Commerce i molninfrastrukturen dessa autentiseringsuppgifter för att generera det tillfälliga SSH-certifikatet. Certifikatets förfallodatum är en timme, men det uppdateras automatiskt under den aktuella sessionen.

När du har loggat in i ett projekt med MFA måste användarna använda CLI-värdet `magento-cloud` för att generera SSH-certifikatet:

```bash
magento-cloud ssh-cert:load
```

Kommandot `ssh-cert:load` genererar SSH-certifikatet och installerar det i SSH-agenten för den lokala användaren.

### Generera certifikat automatiskt vid inloggning

Du kan konfigurera din lokala miljö så att SSH-certifikatet genereras automatiskt när du autentiserar till CLI:n för `magento-cloud`.

**Så här lägger du till automatisk generering av SSH-certifikat i din `magento-cloud` CLI-konfiguration**:

1. Skapa en fil med namnet `config.yaml` i mappen `.magento-cloud` i din arbetskatalog om den inte finns på din lokala arbetsstation.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Lägg till följande konfiguration i filen `config.yaml`.

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. Använd CLI:n för `magento-cloud` för att autentisera igen:

   >Logga ut:

   ```bash
   magento-cloud logout
   ```

   >Logga in:

   ```bash
   magento-cloud login
   ```

   >Följ svaret:

   ```
   Please open the following URL in a browser and log in:
   http://127.0.0.1:5000
   
   Help:
     Leave this command running during login.
     If you need to quit, use Ctrl+C.
   
     To log in using an API token, run: magento-cloud auth:api-token-login
   
   Login information received. Verifying...
   You are logged in.
   
   Generating SSH certificate...
   A new SSH certificate has been generated.
   It will be automatically refreshed when necessary.
   The certificate is included in your SSH configuration: /Users/<user-name>/.ssh/config
   ```

## Ansluta till en miljö med SSH med TFA

När MFA är aktiverat i ett projekt måste du ha TFA aktiverat på ditt konto innan du kan ansluta till en fjärrmiljö med SSH. Se [Aktivera TFA](user-access.md#enable-tfa-for-cloud-accounts).

>[!BEGINSHADEBOX]

**Förutsättningar:**

För projekt som har aktiverats med MFA-användning krävs följande behörigheter och kontoinställningar för SSH-åtkomst:

- [Administratörs- eller medverkandes åtkomst till miljön](user-access.md)
- [SSH-åtkomstnyckel konfigurerad för konto](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [TFA aktiverat a conto](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**Så här ansluter du med SSH med autentiseringsuppgifter för TFA-användarkontot**:

1. Logga in på [ditt konto](https://console.adobecommerce.com).

1. Använd CLI `magento-cloud` på din lokala arbetsstation för att generera SSH-certifikatet.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Exempelsvar:

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Använd en SSH för att ansluta till fjärrmiljön.

   ```bash
   ssh abcdef7uyxabce-master-7rqtwti--mymagento@ssh.us-5.magento.cloud
   ```

   ```
    __  __                   _          ___ _             _
   |  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
   | |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
   |_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
               |___/
   
    Welcome to Magento Cloud.
   
    This is environment master-7rqtwti
    of project abcdef7uyxabce.
   
   web@mymagento.0:~$
   ```

## Hantera källkod med SSH med TFA

När du hanterar källkod för Adobe Commerce i molninfrastrukturprojekt använder du SSH för att autentisera till Git-databasen för projektet. Om ditt projekt har MFA-tillämpning aktiverad måste du generera ett SSH-certifikat innan du kan utföra kommandoradsåtgärder med Git-databasen.

**Så här ansluter du med SSH med autentiseringsuppgifter för TFA-användarkontot**:

1. Logga in på [ditt konto](https://console.adobecommerce.com) och autentisera med TFA.

   >[!NOTE]
   >
   >Om du inte har aktiverat TFA på ditt konto måste du aktivera det. Se [Aktivera TFA på molnkonton](user-access.md#enable-tfa-for-cloud-accounts).

1. Använd CLI `magento-cloud` på din lokala arbetsstation för att generera SSH-certifikatet.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Exempelsvar:

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Klona Git-databasen för din projektmiljö:

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > Exempelsvar:

   ```
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## Ansluta till en miljö med SSH med en API-token

När MFA är aktiverat i ett projekt kräver automatiserade processer som kräver SSH-åtkomst till en molnmiljö en API-token. Du kan generera token från ett Adobe Commerce-konto för molninfrastruktur med administratörs- eller medarbetaråtkomst i projektet.

Autentisering med en API-token kräver fortfarande att ett SSH-certifikat skapas. Automatiserade processer måste också automatisera genereringen av ett SSH-certifikat.

>[!BEGINSHADEBOX]

**Förutsättningar:**

- [Administratörs- eller medverkandes åtkomst till Adobe Commerce i molninfrastrukturmiljön](user-access.md)
- [Giltig API-token tillgänglig för konto](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**Så här ansluter du med SSH med en API-tokenautentiseringsuppgift**:

1. Logga in på molnprojektet med API-nyckelautentisering.

   ```bash
   magento-cloud auth:api-token
   ```

1. Ange värdet för en giltig API-token när du uppmanas till detta.

   ```
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### Exempel: automatiserat SSH-skript

Det finns två alternativ för att lagra API-token.

>[!NOTE]
>
>Om en API-token lagras autentiseras `magento-cloud` CLI automatiskt och du behöver inte utföra kommandot `magento-cloud login`.

**Alternativ 1**: Skapa en miljövariabel för lagring av API-token

Skriv token till din bash_profile

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**Alternativ 2**: Lägg till token i filen `config.yaml`

1. Skapa en fil med namnet `config.yaml` i mappen `.magento-cloud` i din arbetskatalog om den inte finns på din lokala arbetsstation.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Lägg till följande konfiguration i filen `config.yaml`.

   ```yaml
   api:
      token: <your api token>
   ```

>Exempel på basskript

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## Felsökning

Använd följande information för att lösa problem med SSH-anslutningsbegäranden på grund av autentiseringsfel som `access requires MFA` eller `permission denied`.

### Din begäran innehåller inte något giltigt certifikat

Om din begäran inte innehåller något giltigt certifikat visas ett meddelande som liknar det här:

```
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

Försök med följande felsökningsprocedurer för att lösa anslutningsproblemet:

- Verifiera kontots TFA-konfiguration
- Autentisera igen och läs sedan in certifikatet igen

**Så här verifierar du TFA-konfiguration och autentisering**:

1. Logga in på [ditt konto](https://console.adobecommerce.com).

1. Klicka på **[!UICONTROL My Profile]** på den övre högra kontomenyn.

1. Klicka på fliken **[!UICONTROL Security]** på sidan _Min profil_.

   Om TFA är aktiverat innehåller avsnittet Säkerhet alternativ för att hantera TFA-konfigurationen.

1. Om TFA inte har konfigurerats klickar du på **[!UICONTROL Set up application]** och följer instruktionerna för att aktivera den. Se [Aktivera TFA](user-access.md#enable-tfa-for-cloud-accounts).

1. Om TFA är konfigurerat försöker du autentisera igen.

**Så här autentiserar och läser du in SSH-certifikatet igen**:

1. Använd CLI:n för `magento-cloud` för att autentisera igen:

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. Läs in SSH-certifikatet igen:

   ```bash
   magento-cloud ssh-cert:load
   ```

### Åtkomst nekad

Om SSH-nyckeln saknas eller är ogiltig returnerar SSH-anslutningsbegäran ett `Permission denied (publickey)`-fel.

```
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

Åtgärda problemet genom att lägga till SSH-nyckeln till den aktuella sessionen eller uppdatera SSH-konfigurationsfilen så att dina SSH-nycklar läses in automatiskt. Se [Lägg till en offentlig SSH-nyckel](../development/secure-connections.md#add-an-ssh-public-key-to-your-account).

### Det går inte att komma åt projekt utan MFA

Om du autentiserar ett projekt med multifaktorautentisering (MFA) aktiverat kan du få följande fel vid anslutning till andra projekt som inte kräver MFA:

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

Exempelsvar:

```
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

Under SSH-certifikatgenereringen lägger CLI `magento-cloud` till ytterligare en SSH-nyckel till din lokala miljö. Nyckeln används som standard om din lokala SSH-konfiguration inte innehåller SSH-nyckeln för projektåtkomst.

**Så här lägger du till SSH-nyckeln i den lokala konfigurationen**:

1. Skapa filen `config` om den inte finns.

   ```bash
   touch ~/.ssh/config
   ```

1. Lägg till en `IdentityFile`-konfiguration.

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>Du kan ange flera SSH-nycklar genom att lägga till flera `IdentityFile`-poster i konfigurationen.
