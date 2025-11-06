---
title: Visa och hantera loggar
description: Förstå vilka typer av loggfiler som finns i molninfrastrukturen och var de finns.
last-substantial-update: 2023-05-23T00:00:00Z
exl-id: f0bb8830-8010-4764-ac23-d63d62dc0117
source-git-commit: afdc6f2b72d53199634faff7f30fd87ff3b31f3f
workflow-type: tm+mt
source-wordcount: '1205'
ht-degree: 0%

---

# Visa och hantera loggar

Loggar för Adobe Commerce i molninfrastrukturprojekt är användbara för felsökning av problem som rör [bygg och distribuera hooks](../application/hooks-property.md), molntjänster och Adobe Commerce-programmet.

Du kan visa loggarna från filsystemet, [!DNL Cloud Console] och `magento-cloud` CLI.

- **Filsystem** - Systemkatalogen `/var/log` innehåller loggar för alla miljöer. Katalogen `var/log/` innehåller programspecifika loggar som är unika för en viss miljö. De här katalogerna delas inte mellan noder i ett kluster. I Pro Production- och Staging-miljöer måste du kontrollera loggarna på varje nod.

- **[!DNL Cloud Console]** - Du kan se hur logginformation byggs, distribueras och distribueras i _meddelanderistan_ i miljön.

- **Cloud CLI** - Du kan visa lokala miljöloggar med kommandot `magento-cloud log` eller fjärrmiljöloggar med kommandot `magento-cloud ssh`.

## Loggplatser

Systemloggarna lagras på följande platser:

- Integrering: `/var/log/<log-name>.log`
- Proffsmellanlagring: `/var/log/platform/<project-ID>_stg/<log-name>.log`
- Proffsproduktion: `/var/log/platform/<project-ID>/<log-name>.log`

Värdet för `<project-ID>` beror på projektet och om miljön är Förproduktion eller Produktion. Med ett projekt-ID på `yw1unoukjcawe` är till exempel användaren i mellanlagringsmiljön `yw1unoukjcawe_stg` och användaren i produktionsmiljön är `yw1unoukjcawe`.

Med det exemplet är distributionsloggen: `/var/log/platform/yw1unoukjcawe_stg/deploy.log`

### Visa fjärrmiljöloggar

De flesta loggar innehåller händelser som inträffar i fjärrmiljön. För Pro finns det flera noder och varje nod har unika loggar. Använd följande för att se en lista över alla värdar:

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

Exempelsvar:

```
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**Så här visar du en lista över fjärrmiljöloggar**:

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Exempel för Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**Så här visar du en fjärrlogg**:

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Exempel för Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>I Pro Staging- och Pro Production-miljöer aktiveras automatisk loggrotation, komprimering och borttagning för loggfiler med ett fast filnamn. Varje loggfilstyp har ett roterande mönster och en livstid.
>Fullständig information om miljöns loggrotation och livslängd för komprimerade loggar finns i: `/etc/logrotate.conf` och `/etc/logrotate.d/<various>`.
>För Pro Staging- och Pro Production-miljöer måste du [skicka en Adobe Commerce Support-biljett](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) för att be om ändringar i loggrotationskonfigurationen.

>[!TIP]
>
>Loggrotation kan inte konfigureras i Pro Integration-miljöer.
>För Pro Integration måste du implementera en anpassad lösning/skript och [konfigurera ditt cron](../application/crons-property.md) så att skriptet körs efter behov.

>[!NOTE]
>
>Startprojektmiljöer har ingen loggrotation.

## Skapa och distribuera loggar

När du har gjort ändringar i din miljö kan du granska loggningen från varje krok i filen `var/log/cloud.log`. Loggen innehåller start- och stoppmeddelanden för varje krok. I följande exempel är meddelandena `Starting post-deploy.` och `Post-deploy is complete.`

Kontrollera tidsstämplarna för loggposterna, verifiera och hitta loggarna för en specifik distribution. Följande är ett komprimerat exempel på loggutdata som du kan använda för felsökning:

```
Re-deploying environment project-integration-ID
  Executing post deploy hook for service `mymagento`
    [2019-01-03 19:44:11] NOTICE: Starting post-deploy.
    [2019-01-03 19:44:11] INFO: Validating configuration
    [2019-01-03 19:44:11] INFO: End of validation
    [2019-01-03 19:44:11] INFO: Enable cron
    [2019-01-03 19:44:11] INFO: Create backup of important files.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/env.php.bak for /app/app/etc/env.php was created.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/config.php.bak for /app/app/etc/config.php was created.
    [2019-01-03 19:44:11] INFO: php ./bin/magento cache:flush --ansi --no-interaction
    [2019-01-03 19:44:32] INFO: Warming up failed: http://integration-id-project.us.magentosite.cloud/
    [2019-01-03 19:44:32] NOTICE: Post-deploy is complete.
```

>[!TIP]
>
>När du konfigurerar din molnmiljö kan du konfigurera [loggbaserade Slack- och e-postmeddelanden](../environment/set-up-notifications.md) för bygg- och distributionsåtgärder.

Följande loggar har en gemensam plats för alla Cloud-projekt:

- **Distributionslogg**: `var/log/cloud.log`
- **Logg för senaste distributionsfel**: `var/log/cloud.error.log`
- **Felsökningslogg**: `var/log/debug.log`
- **Undantagslogg**: `var/log/exception.log`
- **Systemlogg**: `var/log/system.log`
- **Supportlogg**: `var/log/support_report.log`
- **Rapporter**: `var/report/`

Även om filen `cloud.log` innehåller feedback från varje steg i distributionsprocessen, är loggar som skapas av distributionskroken unika för varje miljö. Den miljöspecifika distributionsloggen finns i följande kataloger:

- **Integrering med Starter och Pro**: `/var/log/deploy.log`
- **Pro Staging**: `/var/log/platform/<project-ID>_stg/deploy.log`
- **Pro Production**: `/var/log/platform/<project-ID>/deploy.log`

### Distribuera logg

Loggen för varje distribution sammanfogas med den specifika `deploy.log`-filen. Följande exempel skriver ut distributionsloggen för den aktuella miljön i terminalen:

```bash
magento-cloud log -e <environment-ID> deploy
```

Exempelsvar:

```
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### Fellogg

Fel- och varningsmeddelanden som genereras under distributionsprocessen skrivs till både `var/log/cloud.log`- och `var/log/cloud.error.log`-filerna. Felloggfilen i molnet innehåller endast fel och varningar från den senaste distributionen. En tom fil visar att distributionen lyckades utan fel.

Du kan visa loggfilen med [Cloud CLI SSH](#view-remote-environment-logs) eller använda ECE-Tools för att visa felen med förslag:

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

Exempelsvar:

```
errorCode: 1001
stage: build
step: validate-config
suggestion: Please run the following commands:
1. bin/magento module:enable --all
2. git add -f app/etc/config.php
3. git commit -m 'Adding config.php'
4. git push
title: File app/etc/config.php does not exist
type: warning
---------------

errorCode: 1006
stage: build
step: validate-config
suggestion: Your application does not have the "post_deploy" hook enabled.
  In order to minimize downtime, add the following to ".magento.app.yaml":
  hooks:
      post_deploy: |
          php ./vendor/bin/ece-tools run scenario/post-deploy.xml
title: The configured state is not ideal
type: warning
```

De flesta felmeddelanden innehåller en beskrivning och förslag på åtgärd. Använd [felmeddelandereferensen för ECE-verktyg](../dev-tools/error-reference.md) för att leta upp felkoden för ytterligare vägledning. Använd [felsökaren för Adobe Commerce-distribution](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html?lang=sv-SE) om du vill ha mer information.

## Programloggar

På samma sätt som för distributionsloggar är programloggar unika för varje miljö:

| Loggfil | Integrering med Starter och Pro | Beskrivning |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **Distribuera logg** | `/var/log/deploy.log` | Aktivitet från [distributionskroken](../application/hooks-property.md). |
| **Logg efter distribution** | `/var/log/post_deploy.log` | Aktivitet från [funktionen för efterdistribution](../application/hooks-property.md). |
| **Kronlogg** | `/var/log/cron.log` | Utdata från cron-jobb. |
| **Nginx-åtkomstlogg** | `/var/log/access.log` | Vid Nginx-start, HTTP-fel för saknade kataloger och uteslutna filtyper. |
| **Inledande fellogg** | `/var/log/error.log` | Startmeddelanden är användbara vid felsökning av konfigurationsfel som är kopplade till Nginx. |
| **PHP-åtkomstlogg** | `/var/log/php.access.log` | Begäranden till PHP-tjänsten. |
| **PHP FPM-logg** | `/var/log/app.log` | |

I Pro Staging- och Production-miljöer är loggarna Deploy, Post-deploy och Cron bara tillgängliga på den första noden i klustret:

| Loggfil | Pro Staging | Pro Production |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **Distribuera logg** | Endast den första noden:<br>`/var/log/platform/<project-ID>_stg*/deploy.log` | Endast den första noden:<br>`/var/log/platform/<project-ID>/deploy.log` |
| **Logg efter distribution** | Endast den första noden:<br>`/var/log/platform/<project-ID>_stg*/post_deploy.log` | Endast den första noden:<br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **Kronlogg** | Endast den första noden:<br>`/var/log/platform/<project-ID>_stg*/cron.log` | Endast den första noden:<br>`/var/log/platform/<project-ID>/cron.log` |
| **Nginx-åtkomstlogg** | `/var/log/platform/<project-ID>_stg*/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Inledande fellogg** | `/var/log/platform/<project-ID>_stg*/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **PHP-åtkomstlogg** | `/var/log/platform/<project-ID>_stg*/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **PHP FPM-logg** | `/var/log/platform/<project-ID>_stg*/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### Arkiverade loggfiler

Programloggarna komprimeras och arkiveras en gång om dagen och sparas i **30 dagar** som standard (för Pro-kluster för mellanlagring och produktion). Loggrotation är inte tillgängligt i alla integrerings-/startmiljöer. De komprimerade loggarna namnges med ett unikt ID som motsvarar `Number of Days Ago + 1`. I Pro-produktionsmiljöer har till exempel en PHP-åtkomstlogg för 21 dagar tidigare lagrats och fått följande namn:

```
/var/log/platform/<project-ID>/php.access.log.22.gz
```

De arkiverade loggfilerna sparas alltid i den katalog där originalfilen fanns före komprimeringen.

Du kan [skicka in en supportanmälan](https://experienceleague.adobe.com/home?lang=sv-SE&support-tab=home#support) om du vill begära ändringar av loggkvarhållningsperioden eller konfigurationen för logrotate. Du kan öka kvarhållningsperioden upp till högst 365 dagar, minska den för att spara lagringskvoten eller lägga till ytterligare loggsökvägar till logrotate-konfigurationen. Dessa ändringar är tillgängliga för Pro Staging- och Production-kluster.

Om du till exempel skapar en anpassad sökväg för att lagra loggar i katalogen `var/log/mymodule` kan du begära loggrotation för den här sökvägen. Den aktuella infrastrukturen kräver emellertid konsekventa filnamn för Adobe för att konfigurera loggrotation korrekt. Adobe rekommenderar att loggnamnen är konsekventa för att undvika konfigurationsproblem.

>[!NOTE]
>
>**Distribuera**- och **Efterdistribuera**-loggfiler roteras inte och arkiveras. Hela distributionshistoriken skrivs i dessa loggfiler.

## Tjänstloggar

Eftersom varje tjänst körs i en separat behållare är inte tjänstloggarna tillgängliga i integreringsmiljön. Adobe Commerce i molninfrastruktur ger åtkomst till webbserverbehållaren endast i integreringsmiljön. Följande loggplatser är för Pro Production och Staging-miljöerna:

- **Redis-logg**: `/var/log/platform/<project-ID>*/redis-server-<project-ID>*.log`
- **Elasticsearch-logg**: `/var/log/elasticsearch/elasticsearch.log`
- **Logg för skräpinsamling för Java**: `/var/log/elasticsearch/gc.log`
- **E-postlogg**: `/var/log/mail.log`
- **MySQL-fellogg**: `/var/log/mysql/mysql-error.log`
- **MySQL långsam logg**: `/var/log/mysql/mysql-slow.log`
- **RabbitMQ-logg**: `/var/log/rabbitmq/rabbit@host1.log`

Tjänstloggarna arkiveras och sparas under olika tidsperioder, beroende på loggtyp. MySQL-loggar har till exempel den kortaste livstiden, som har tagits bort efter sju dagar.

>[!TIP]
>
>Sökvägar för loggfiler i den skalade arkitekturen beror på nodtypen. Se [Loggplatser i avsnittet Skalad arkitektur](../architecture/scaled-architecture.md#log-locations).

## Loggdata för Pro Production och Staging

I Pro Production- och Staging-miljöer använder du [New Relic logghantering](../monitor/log-management.md) som är integrerad med ditt projekt för att hantera aggregerade loggdata från alla loggar som är kopplade till ditt Adobe Commerce i molninfrastruktursprojekt.

New Relic Logs-programmet har en central kontrollpanel för logghantering för felsökning och övervakning av Adobe Commerce i molninfrastrukturproduktions- och mellanlagringsmiljöer. Kontrollpanelen ger även åtkomst till loggdata för tjänsterna Snabbt CDN, Bildoptimering och WAF (Web application firewall). Se [New Relic-tjänster](../monitor/new-relic-service.md).
