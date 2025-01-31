---
title: Logghanterare
description: Lär dig hur du konfigurerar logghanterare för Adobe Commerce i molninfrastruktur.
feature: Cloud, Logs, Configuration
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# Logghanterare

Du kan konfigurera logghanterare så att meddelanden skickas till en fjärrloggningsserver. En logghanterare pushar build- och deploy-loggar till andra system, på samma sätt som du skickar loggar till Slack och e-post. Du kan aktivera en _syslog_-hanterare, vilket är idealiskt för att logga meddelanden som är relaterade till maskinvara, eller en GELF-hanterare (Graylog Extended Log Format), som är idealisk för att logga meddelanden från program.

I följande exempel konfigureras båda hanterarna genom att konfigurationen läggs till i filen `.magento.env.yaml`. Information om lägsta loggningsnivå (`min_level`) finns i [Loggnivåer](#log-levels).

```yaml
log:
  syslog:
    ident: "<syslog-ident>"
    facility: 8 # https://php.net/manual/en/network.constants.php
    min_level: "info"
    logopts: <syslog-logopts>

  syslog_udp:
    host: "<syslog-host>"
    port: <syslog-port>
    facility: 8  # https://php.net/manual/en/network.constants.php
    ident: "<syslog-ident>"
    min_level: "info"

  gelf:
    min_level: "info"
    use_default_formatter: true
    additional: # Some additional information for each log message
      project: "<some-project-id>"
      app_id: "<some-app-id>"
    transport:
      http:
        host: "<http-host>"
        port: <http-port>
        path: "<http-path>"
        connection_timeout: 60
      tcp:
        host: "<tcp-host>"
        port: <tcp-port>
        connection_timeout: 60
      udp:
        host: "<udp-host>"
        port: <udp-port>
        chunk_size: 1024
```

## Loggnivåer

Loggnivåer avgör detaljnivån i meddelanden. Följande loggnivåkategorier innehåller alla loggnivåer under den. En `debug`-nivå omfattar till exempel loggning från alla nivåer, medan en `alert`-nivå bara visar varningar och nödsituationer.

- **debug** - detaljerad felsökningsinformation
- **info** - intressanta händelser, till exempel användarinloggning eller SQL-logg
- **Obs!** - normala, men viktiga händelser
- **varning** - exceptionella händelser som inte är fel, som användning av ett inaktuellt API eller dålig användning av ett API
- **error** - körningsfel som inte kräver omedelbar åtgärd
- **critical** - kritiska villkor, t.ex. en otillgänglig programkomponent eller ett oväntat undantag
- **alert** - omedelbar åtgärd krävs - till exempel att en webbplats är nere eller att databasen inte är tillgänglig - som utlöser en SMS-avisering
- **kris** - systemet kan inte användas
