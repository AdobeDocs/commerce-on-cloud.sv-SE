---
title: Hantering av säkerhetskopiering
description: Lär dig hur du manuellt skapar och återställer en säkerhetskopia för ditt Adobe Commerce i molninfrastrukturprojekt.
feature: Cloud, Paas, Snapshots, Storage
exl-id: e73a57e7-e56c-42b4-aa7b-2960673a7b68
source-git-commit: 13cb5e3231c2173d5687aec3e4e64ecc154ee962
workflow-type: tm+mt
source-wordcount: '819'
ht-degree: 0%

---

# Hantering av säkerhetskopiering

Du kan när som helst utföra en manuell säkerhetskopiering av aktiva startmiljöer med knappen **[!UICONTROL Backup]** i [!DNL Cloud Console] eller med kommandot `magento-cloud snapshot:create`.

En säkerhetskopia eller _ögonblicksbild_ är en fullständig säkerhetskopia av miljödata som innehåller alla beständiga data från tjänster som körs (MySQL-databas) och alla filer som lagras på de monterade volymerna (var, pub/media, app/etc). Ögonblicksbilden innehåller _inte_ kod, eftersom koden redan lagras i den Git-baserade databasen. Du kan inte hämta en kopia av en ögonblicksbild.

>[!WARNING]
>
>Säkerhetskopieringar innehåller vanligtvis innehållet i monterade kataloger, inklusive offentliga webbkataloger som `pub/media`, men flytta inte säkerhetskopierade utdatafiler till offentliga webbkataloger som `pub/media` eller `pub/static`.

Säkerhetskopierings-/ögonblicksbildsfunktionen **gäller inte** för Pro Staging- och Production-miljöer, som får regelbundna säkerhetskopieringar för katastrofåterställning som standard. Mer information finns i [Pro Backup &amp; Disaster Recovery](../architecture/pro-architecture.md#backup-and-disaster-recovery). Till skillnad från automatiska direktsäkerhetskopieringar i Pro Staging- och Production-miljöerna är säkerhetskopieringarna **inte** automatiska. Det är _ditt_ ansvar att manuellt skapa en säkerhetskopia eller konfigurera ett cron-jobb för att regelbundet skapa en säkerhetskopia av dina integreringsmiljöer i Starter eller Pro.

## Skapa manuell säkerhetskopiering

Du kan skapa en manuell säkerhetskopia av alla aktiva Starter-miljöer och integreringPro-miljöer från [!DNL Cloud Console] eller skapa en ögonblicksbild från molnet-CLI. Du måste ha en [administratörsroll](../project/user-access.md) för miljön.

>[!NOTE]
>
>Du kan skapa en säkerhetskopia av koden direkt i Pro Production- och Staging-klustren genom att köra följande kommando i terminalen - justera den för de mappar/sökvägar som du vill inkludera/exkludera:
>
```bash
>mkdir -p var/support
>/usr/bin/nice -n 15 /bin/tar -czhf var/support/code-$(date +"%Y%m%d%H%M%p").tar.gz app bin composer.* dev lib pub/*.php pub/errors setup vendor --exclude='pub/media'
>```

**Så här skapar du en databassäkerhetskopia av Pro-miljön**:

Om du vill skapa en databasdump av en Pro-miljö, inklusive mellanlagring och produktion, kan du läsa artikeln [Skapa en databasdump](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud) i kunskapsbasen.

**Så här skapar du en säkerhetskopia av en startmiljö med[!DNL Cloud Console]**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Välj en miljö i projektnavigeringsfältet. Miljön måste vara aktiv.
1. Klicka på **[!UICONTROL Backup]** i vyn _Säkerhetskopior_. Det här alternativet är inte tillgängligt för en Pro-miljö.

   ![Säkerhetskopiera](../../assets/button-backup.png){width="150"}

**Så här skapar du en säkerhetskopia av en integreringsmiljö med[!DNL Cloud Console]**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Välj en integrerings-/utvecklingsmiljö i projektnavigeringsfältet. Miljön måste vara aktiv.
1. Välj alternativet **[!UICONTROL Backup]** i den övre högra menyn. Det här alternativet är tillgängligt för både Starter- och Pro-miljöer.
1. Klicka på knappen **[!UICONTROL Yes]**.

**Så här skapar du en ögonblicksbild med `magento-cloud` CLI**:

1. Byt till din projektkatalog på din lokala arbetsstation.
1. Kolla in miljögrenen för att ta en ögonblicksbild.
1. Skapa fixeringen.

   ```bash
   magento-cloud snapshot:create --live
   ```

   Du kan också använda kortkommandot `magento-cloud backup`. Alternativet `--live` låter miljön vara igång för att undvika driftavbrott. Ange `magento-cloud snapshot:create --help` om du vill se en fullständig lista över alternativ.

   Exempelsvar:

   ```
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. Verifiera de senaste fixeringarna.

   ```bash
   magento-cloud snapshot:list
   ```

   Listan returnerar information om ögonblicksbildens status:

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## Återställa en manuell säkerhetskopia

Du måste ha [administratörsåtkomst](../project/user-access.md) till miljön. Du har upp till **sju dagar** att _återställa_ en manuell säkerhetskopia. Koden för den aktuella Git-grenen ändras inte när du återställer en säkerhetskopia. Återställning av en säkerhetskopia på det här sättet gäller inte testmiljöer och produktionsmiljöer i Pro. Se [Pro Backup &amp; Disaster Recovery](../architecture/pro-architecture.md#backup-and-disaster-recovery).

Återställningstiden varierar beroende på databasens storlek:

- stor databas (200+ GB) kan ta 5 timmar
- en medelstor databas (150 GB) kan ta 2 1/2 timmar
- liten databas (60 GB) kan ta en timme

>[!TIP]
>
>Återställa utan säkerhetskopiering:
>
>- Information om hur du återställer till föregående kod eller tar bort tillägg i en miljö finns i [Återställa kod](#roll-back-code).
>- Information om hur du återställer en instabil miljö som _inte_ har en säkerhetskopia finns i [Återställa en miljö](../development/restore-environment.md).

**Så här återställer du en säkerhetskopia med[!DNL Cloud Console]**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Välj en miljö i projektnavigeringsfältet.
1. I vyn _Säkerhetskopior_ väljer du en säkerhetskopia i listan _Lagrade_. Säkerhetskopieringsfunktionen gäller **inte** för Pro-miljöer.
1. Klicka på **Återställ** på menyn ![Mer](../../assets/icon-more.png){width="32"} (_mer_).
1. Granska informationen för återställning från säkerhetskopia och klicka på **Ja, återställ**.

**Så här återställer du en ögonblicksbild med molnet-CLI**:

1. Byt till din projektkatalog på din lokala arbetsstation.
1. Kolla in den miljögren som ska återställas.
1. Visa alla tillgängliga ögonblicksbilder.

   ```bash
   magento-cloud snapshot:list
   ```

   Listan returnerar information om tillgängliga ögonblicksbilder:

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. Återställ en ögonblicksbild med ID för ögonblicksbild i listan.

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## Återställ en ögonblicksbild av Disaster Recovery

[Importera databasdumpen direkt från servern](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3) om du vill återställa ögonblicksbilden av Disaster Recovery i Pro-miljöer för förproduktion och produktion.

## Återställningskod

Säkerhetskopior och ögonblicksbilder innehåller _inte_ en kopia av koden. Koden lagras redan i den Git-baserade databasen, så du kan använda Git-baserade kommandon för att återställa (återställa) kod. Använd till exempel `git log --oneline` för att bläddra igenom tidigare implementeringar och använd sedan [`git revert`](https://git-scm.com/docs/git-revert) för att återställa kod från en specifik implementering.

Du kan också välja att lagra kod i en _inaktiv_-gren. Använd Git-kommandon för att skapa en gren i stället för att använda `magento-cloud`-kommandon. Läs mer om [Git-kommandon](../dev-tools/cloud-cli-overview.md#git-commands) i Cloud CLI-avsnittet.

## Relaterad information

- [Säkerhetskopiera databasen](database-dump.md)
- [Säkerhetskopiering och katastrofåterställning](../architecture/pro-architecture.md#backup-and-disaster-recovery) för Pro Production och Staging-kluster
