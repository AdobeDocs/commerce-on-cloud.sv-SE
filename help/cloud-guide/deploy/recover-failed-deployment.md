---
title: Återställning efter komponentfel
description: Lär dig hur du kan återställa en komponent som inte kan distribueras korrekt i Adobe Commerce i molninfrastrukturen.
feature: Cloud, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Återställning efter komponentfel

I det här avsnittet beskrivs hur du återställer om en komponent inte kan distribueras på rätt sätt. Typiska exempel är komponenter som har beroenden som inte uppfylls av din fjärrmiljö, till exempel inkompatibla PHP-versioner.

Du kan återställa efter en misslyckad distribution på något av följande sätt:

- [Återställa en säkerhetskopia](../storage/snapshots.md#restore-a-snapshot)
- Rensa projekt och kod från tidigare ändringar och omdistribuera

## Rensa, ta bort och omdistribuera

Om du vill rensa upp från den tidigare distributionen identifierar du komponenten som lades till eller uppdaterades och tar sedan bort den. Logga först in i fjärrmiljön och rensa innehållet i katalogen `var` manuellt. Ta sedan bort komponenten från filen `composer.json` och distribuera om miljön.

**Så här rensar du `var` kataloger**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Använd SSH för att logga in i fjärrmiljön.

   ```bash
   magento-cloud ssh
   ```

1. Rensa katalogerna `var`.

   ```shell
   rm -rf var/*
   ```

1. Logga ut.

**Så här tar du bort komponenten**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Rensa cachen.

   ```bash
   composer clear-cache
   ```

1. Ta bort komponenten från filen `composer.json`.

   ```bash
   composer remove <component-name>:<version>
   ```

   Om följande meddelande visas behöver du inte göra något mer:

   ```
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. Vänta medan beroendena uppdateras.

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

Mer information om hur du återställer en miljö utan säkerhetskopiering finns i [Återställa en miljö](../development/restore-environment.md).

{{stuck-deployment-tip}}
