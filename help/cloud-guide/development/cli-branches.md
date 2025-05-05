---
title: Hantera grenar med CLI
description: Lär dig hur du hanterar miljögrenarna för Adobe Commerce i molninfrastrukturen med hjälp av Cloud CLI.
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# Hantera grenar med CLI

Mer information om hur du installerar CLI:n för `magento-cloud` finns i [Cloud CLI-referensen](../dev-tools/cloud-cli-overview.md). När du har installerat CLI:n för `magento-cloud` och konfigurerat SSH-nycklar för fjärråtkomst till din molninfrastruktur kan du använda CLI-kommandon för `magento-cloud` för att hantera miljöer för dina projekt. Mer information om miljöarkitekturen finns i [Starter-arkitekturen](../architecture/starter-architecture.md) eller [Pro-arkitekturen](../architecture/pro-architecture.md).

Mer information om hur du hanterar grenar och miljöer med [!DNL Cloud Console] finns i [Hantera grenar med  [!DNL Cloud Console]](../project/console-branches.md).

## Använda CLI-kommandon

CLI-kommandona `magento-cloud` liknar Git-kommandon. Du kan använda dem för att ansluta till ditt projekt och hantera dina miljöer. Även om du kan köra kommandona från en katalog bör du köra dem från en projektkatalog. När du kör från en projektkatalog kan du utelämna parametern `-p <project-ID>`. Se [Cloud CLI-referens](../dev-tools/cloud-cli-overview.md).

## Klona projektet

I följande instruktioner används en kombination av `magento-cloud` CLI-kommandon och Git-kommandon för att klona projektet till din lokala arbetsstation. Använd kommandot `magento-cloud list` om du vill se en fullständig lista över `magento-cloud` CLI-kommandon.

>[!IMPORTANT]
>
>Vissa Git-kommandon kan inte slutföra en åtgärd i Adobe Commerce i ett molninfrastrukturprojekt. Du kan till exempel skapa en gren med ett Git-kommando, men du kan inte skapa och aktivera en ny miljö. Du måste skapa en miljö med kommandot `magento-cloud environment:branch <branch-name>` för att miljön ska bli _aktiv_. Du kan också använda [!DNL Cloud Console] för att skapa aktiva miljöer. Se [Cloud CLI-referens](../dev-tools/cloud-cli-overview.md#git-commands).

**Så här klonar du ett projekt `master` miljö**:

1. Logga in på din lokala arbetsstation med ett [ägarkonto](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=sv-SE) för filsystemet.

1. Byt till webbservern eller den virtuella värdkatalogen _docroot_.

1. Logga in med CLI:n för `magento-cloud`.

   ```bash
   magento-cloud login
   ```

1. Lista dina projekt.

   ```bash
   magento-cloud project:list
   ```

1. Klona ett projekt.

   ```bash
   magento-cloud project:get <project-ID>
   ```

   Ange ett katalognamn när du uppmanas till detta.

1. Byt till katalogen `magento2`.

1. Visa tillgängliga miljöer för projektet.

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >Kommandot `magento-cloud environment:list` visar miljöhierarkier, men det gör inte kommandot `git branch`.

1. Hämta fjärrgrenarna.

   ```bash
   git fetch origin
   ```

1. Dra in uppdaterad kod.

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>Mer information om hur du använder Git-baserade värdtjänster med Adobe Commerce om molninfrastruktur finns i [Integreringar](../integrations/overview.md).

## Skapa en gren för utveckling

När du har klonat projektet och uppdaterat Adobe Commerce administratörskonfiguration kan du skapa en gren för utvecklingen. Som tidigare nämnts måste du skapa en miljö med kommandot `magento-cloud environment:branch <branch-name>` eller [!DNL Cloud Console] för att miljön ska bli _aktiv_.

- För [Starter](../architecture/starter-develop-deploy-workflow.md#clone-and-branch) kan du skapa en gren för `staging` och sedan skapa en utvecklingsgren baserat på grenen `staging`.
- För [Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow) skapar du utvecklingsgrenar baserat på grenen `Integration`.

**Så här skapar du en utvecklingsgren**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Skapa en miljö baserad på den gren som rekommenderas för ditt projektarbetsflöde.

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. Uppdatera beroenden.

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_valfri_] Skapa en [säkerhetskopia](../storage/snapshots.md) av miljön.

### Sammanfoga en gren

När du är klar med utvecklingen sammanfogar du den här grenen med den överordnade:

1. Verkställ och push-kodsändringar:

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Sammanfoga med den överordnade miljön:

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### Ta bort en miljö

Ta bara bort en miljö om du är säker på att du inte längre behöver den. Du kan inte återställa en miljö efter att du har tagit bort den.

>[!WARNING]
>
>Du kan inte ta bort grenen `master` för något projekt.

Du måste vara projektadministratör, miljöadministratör eller kontoägare för att kunna utföra den här åtgärden. Se [Hantera användaråtkomst till molnprojekt](../project/user-access.md).

När du tar bort en miljö ställs miljön in på _inaktiv_. Koden är fortfarande tillgänglig i Git-grenen, men innehåller inte längre tjänsterna eller databasen. Om du vill ta bort miljön helt måste du även ta bort motsvarande Git-fjärrgren.

**Ta bort en miljö**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Hämta uppdateringar från fjärrservern.

   ```bash
   git fetch
   ```

1. Ta bort miljögrenen.

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   Du kan också ta bort mer än en miljö åt gången genom att lägga till flera miljö-ID:n i kommandot delete.

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. Svara på uppmaningarna att ta bort den lokala miljön och motsvarande fjärrmiljö.

   ```
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   Om du tar bort miljön placeras den i ett _inaktivt_-läge.

   ```
   Delete the remote Git branch too? [Y/n]
   ```

   Om du tar bort Git-fjärrgrenen tas miljön bort från projektet.

1. Vänta tills miljön har tagits bort.

   ```
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>Om du vill aktivera en inaktiv miljö använder du kommandot `magento-cloud environment:activate`.

## Interagera med fjärrmiljöer

När du har [konfigurerat SSH-nycklar](../development/secure-connections.md) kan du [ansluta från din lokala arbetsyta till en fjärrmiljö](../development/secure-connections.md#connect-to-a-remote-environment) och interagera med dina projekttjänster och ändra inställningarna.
