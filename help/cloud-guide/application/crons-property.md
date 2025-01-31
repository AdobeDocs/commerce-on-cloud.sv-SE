---
title: Egenskapen Crons
description: Se exempel på hur du konfigurerar egenskapen "crons" i  [!DNL Commerce] programkonfigurationsfilen.
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1069'
ht-degree: 0%

---

# Egenskapen Crons

Adobe Commerce använder egenskapen `crons` för att schemalägga återkommande aktiviteter. Det är idealiskt för schemaläggning av en viss uppgift som ska köras vid vissa tidpunkter på dygnet. På grund av skrivskyddade miljöer kan endast ett kroniskt jobb köras åt gången på webbinstansen för Adobe Commerce i molninfrastrukturprojekt. Det är en god rutin att dela upp långvariga uppgifter i mindre uppgifter som ligger i kö. Du kan också skapa en [worker-instans](workers-property.md).

Adobe rekommenderar att du kör `crons` som [filsystemsägare](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html). Kör _inte_ `crons` som `root` eller som webbserveranvändare.

Den här konfigurationen skiljer sig från lokala distributioner av Adobe Commerce, som har flera standardcron-jobb. Se [Konfigurera cron-jobb](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html) i _Konfigurationsguiden_.

## Ställ in cron-jobb

Egenskapen `crons` beskriver processer som aktiveras enligt ett schema. Varje jobb kräver ett namn och följande alternativ:

- `spec` - Det cron-uttryck som används för schemaläggning.
- `cmd` - Det kommando som ska köras på `start` och `stop`.
- `shutdown_timeout`—(_Valfritt_) Om ett cron-jobb avbryts är detta antalet sekunder som en SIGKILL-signal skickas för att stoppa jobbet eller processen. Standardvärdet är 10 sekunder.
- `timeout`—(_Valfritt_) Den längsta tid ett cron-jobb kan köras före timeout. Standardvärdet är det högsta tillåtna värdet 86400 sekunder (24 timmar).

Som standard har varje Commerce-molnprojekt följande standardkonfiguration för `crons` i filen `.magento.app.yaml`:

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

Om ditt projekt kräver anpassade cron-jobb kan du lägga till dem i standardkonfigurationen för `crons`. Se [Skapa ett cron-jobb](#build-a-cron-job).

### `crontab`

Adobe Commerce har lagt till ett konfigurationsalternativ för automatiska kroner endast i Pro-projekt för att ge stöd för självbetjäningskonfigurationen `crons` i mellanlagrings- och produktionsmiljöerna. Om det här alternativet är aktiverat kan du använda `crontab` för att granska cron-konfigurationen. Detta är _inte_ tillgängligt med Starter-projekt.

Även om du kan använda `crontab` för att granska konfigurationen i Pro-projekt, använder inte Adobe Commerce `crontab` för att köra cron-jobb för webbplatser som distribueras i molninfrastrukturen.

**Så här granskar du cron-konfigurationen i Pro-miljöer**:

1. Använd [SSH](../development/secure-connections.md#use-an-ssh-command) för att logga in på fjärrmiljön.

1. Visa en lista över schemalagda kroniprocesser.

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >Om kommandot `crontab -l` returnerar ett `Command not found`-fel (endast i Pro Staging- och Production-miljöer) måste du [skicka en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) för att aktivera självbetjäningskonfigurationsalternativet för automatiska kopior i ditt projekt.

I följande exempel visas `crontab`-utdata för en miljö som bara har standardkonfigurationen `crons`:

```
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## Bygg ett cron-jobb

Ett cron-jobb innehåller schema- och timingspecifikationen och det kommando som ska köras vid den schemalagda tidpunkten. För Starter-miljöer och Pro `integration`-miljöer är det minsta intervallet en gång per fem minuter. För Pro Staging- och Production-miljöer är minimiintervallet en gång per minut. På Adobe Commerce i molninfrastruktur lägger du till anpassade cron-jobb i filen `.magento.app.yaml` i avsnittet `crons`. Det allmänna formatet är `spec` för schemaläggning och `cmd` för att ange det kommando eller anpassade skript som ska köras.

### Specifikation

Adobe Commerce använder ett femvärdesuttryck för en `crons`-specifikation (spec): `* * * * *`

1. Minut (0 till 59) För alla Starter- och Pro-miljöer är den minsta frekvens som stöds för kronijobb fem minuter. Du kan behöva konfigurera inställningarna i din administratör.
2. Timme (0 till 23)
3. Dag i månaden (1 till 31)
4. Månad (1 till 12)
5. Veckodag (0 till 6) (söndag till lördag; 7 är också söndag på vissa system)

Några exempel:

- `00 */3 * * *` körs var tredje timme på den första minuten (12:00, 3:00, 6:00)
- `20 */8 * * *` körs var 8:e timme vid minut 20 (12:20, 8:20, 4:20)
- `00 00 * * *` körs en gång om dagen vid midnatt
- `00 * * * 1` körs en gång i veckan på måndag vid midnatt.

>[!NOTE]
>
>Den `crons`-tid som anges i filen `.magento.app.yaml` baseras på serverns tidszon, inte på den tidszon som anges i lagringskonfigurationsvärdena i databasen.

När du bestämmer schemaläggningen bör du tänka på hur lång tid det tar att slutföra uppgiften. Om du till exempel kör ett jobb var tredje timme och aktiviteten tar 40 minuter att slutföra kan du ändra den schemalagda tidpunkten.

### Kommando

`cmd` anger det kommando eller anpassade skript som ska köras. Kommandots skriptformat kan innehålla följande:

```text
<path-to-php-binary> <project-dir>/<script-command>
```

Exempel:

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

I detta exempel är `<path-to-php-binary>` `/usr/bin/php`. Installationskatalogen, som innehåller projekt-ID:t, är `/app/abc123edf890/bin/magento` och skriptåtgärden är `export:start catalog_category_product`.

### Lägg till anpassade cron-jobb i ditt projekt

På Adobe Commerce på molninfrastrukturplattformen kan du lägga till anpassningar i avsnittet `crons` i filen [`.magento.app.yaml`](../application/configure-app-yaml.md).

>[!NOTE]
>
>För Starter-miljöer och Pro `integration`-miljöer är det minsta intervallet en gång per fem minuter. För Pro Staging- och Production-miljöer är minimiintervallet en gång per minut. Du kan inte konfigurera mer frekventa intervall än standardminimumen.

I Adobe Commerce Pro-projekt måste funktionen [för automatiska kroner](#set-up-cron-jobs) vara aktiverad i ditt projekt innan du kan lägga till anpassade kron-jobb i mellanlagrings- och produktionsmiljöer med hjälp av filen `.magento.app.yaml`. Om den här funktionen inte är aktiverad kan du [skicka en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) om du vill aktivera automatiska korrigeringar.

**Så här lägger du till anpassade cron-jobb**:

1. Redigera filen `.magento.app.yaml` i Adobe Commerce `/app`-katalogen i den lokala utvecklingsmiljön.

1. I avsnittet `crons` lägger du till din anpassning med följande format:

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   I följande exempel exporterar jobbet `productcatalog` produktkatalogen var 8:e timme, 20:e minut efter timmen.

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### Uppdatera cron-jobb

Om du vill lägga till, ta bort eller uppdatera ett anpassat jobb ändrar du konfigurationen i avsnittet `crons` i filen `.magento.app.yaml`. Testa sedan uppdateringarna i fjärrmiljön `integration` innan du överför ändringarna till mellanlagrings- och produktionsmiljöerna.

## Inaktivera cron-jobb

Du kan inaktivera cron-jobb manuellt innan du slutför underhållsåtgärder som att indexera om eller rensa cachen för att förhindra prestandaproblem. Du kan använda kommandot `ece-tools` CLI `cron:disable` för att inaktivera alla cron-jobb och stoppa alla aktiva cron-processer.

**Så här inaktiverar du cron-jobb**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Använd SSH för att logga in i fjärrmiljön.

   ```bash
   magento-cloud ssh
   ```

1. Inaktivera cron-jobb och stoppa aktiva cron-processer.

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. När du har slutfört nödvändiga underhållsåtgärder måste du aktivera cron-jobben igen.

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## Felsökning av cron-jobb

Adobe har uppdaterat Adobe Commerce om molninfrastrukturspaket för att optimera kundbearbetningen på Adobe Commerce om molninfrastrukturplattformen och för att åtgärda kronrelaterade problem. Om du får problem med seriebearbetningen bör du kontrollera att projektet använder den senaste versionen av paketet `ece-tools`. Se [Uppdatera ECE-verktyg](../dev-tools/update-package.md).

Du kan granska information om cron processing i loggfiler på programnivå för varje miljö. Se [Programloggar](../test/log-locations.md#application-logs).

Se följande Adobe Commerce supportartiklar för hjälp med felsökning av kronrelaterade problem:

- [Kronuppgifter låser uppgifter från andra grupper](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html)

- [Återställ fastnade cron-jobb manuellt i molnet](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html)
