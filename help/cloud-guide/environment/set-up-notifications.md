---
title: Konfigurera meddelanden
description: Lär dig hur du konfigurerar meddelanden för Adobe Commerce i molninfrastrukturmiljöer.
feature: Cloud, Configuration, Logs
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# Konfigurera meddelanden

Som standard skriver Adobe Commerce i molninfrastruktur build- och deploy-åtgärder till filen `app/var/log/cloud.log` i Adobe Commerce rotkatalog. Du kan också skicka loggar till ett meddelandesystem, som Slack och e-post, för att få meddelanden i realtid.

Du kan t.ex. skicka ett Slack-meddelande för att varna en grupp personer när en distribution misslyckas och fråga en fråga om vad som gick fel.

## Planmeddelanden

Innan du konfigurerar meddelanden bör du tänka på följande:

- Vilken typ av meddelanden vill du ta emot (Slack-meddelanden, e-post, båda)?
- Hur mycket information vill du se i loggarna?
- Var vill du konfigurera meddelanden (integrering, mellanlagring, produktion)?

Under den inledande utvecklingen kan du till exempel föredra e-postmeddelanden som visar detaljerade loggar om integreringsmiljön för att hjälpa dig att felsöka problem innan du distribuerar till mellanlagringsmiljön. När du är redo att distribuera till förproduktionsmiljön eller produktionsmiljön kanske du föredrar ett Slack-meddelande som innehåller mindre detaljerad information.

>[!NOTE]
>
>Konfigurationsfilen som används för att konfigurera meddelanden finns i rotkatalogen i projektkatalogen, så den gäller när du skickar ändringar till någon miljö. Om du vill anpassa meddelanden per miljö måste du ändra konfigurationsfilen innan du skickar den till den miljön.

## Konfigurera meddelanden

Så här konfigurerar du meddelanden:

1. Byt till din projektkatalog på din lokala arbetsstation.
1. I filen `.magento.env.yaml` i projektroten lägger du till inställningarna för meddelandesystemet, inklusive det rekommenderade meddelandet [Loggnivåer](log-handlers.md#log-levels).

   Om du till exempel vill konfigurera e-postkonfigurationer för både Slack _och_ använder du följande:

   ```yaml
   log:
     slack:
       token: "<your-slack-token>"
       channel: "<your-slack-channel>"
       username: "SlackHandler"
       min_level: "info"
     email:
       to: <your-email>
       from: <your-email>
       subject: "Log notification from Adobe Commerce"
       min_level: "notice"
   ```

   >[!NOTE]
   >
   >Adobe Commerce i molninfrastrukturen skickar bara e-post under driftsättningsfasen.

1. Verkställ och skicka ändringarna till fjärrservern.

   ```bash
   git -A && git commit -m "Configure build/deploy notifications"
   ```

   ```bash
   git push origin <branch-name>
   ```

### Exempel på Slack-konfiguration

I följande exempel visas en konfiguration som endast är för Slack:

```yaml
log:
  slack:
    token: "<your-slack-token>"
    channel: "<your-slack-channel>"
    username: "SlackHandler"
    min_level: "info"
```

- `token` - Din [användartoken](https://api.slack.com/docs/token-types#user) för Slack. Ditt användartoken tillåter att Adobe Commerce i molninfrastrukturen skickar meddelanden.
- `channel` - Slack-kanalens namn Adobe Commerce på molninfrastrukturen skickar meddelanden.
- `username` - Använd användarnamnet Adobe Commerce i molninfrastrukturen för att skicka meddelanden i Slack.
- `min_level` - Minsta loggnivå för meddelanden. Vi rekommenderar att du använder `info`.

### Exempel på e-postkonfiguration

I följande exempel visas en konfiguration som bara innehåller e-post:

>[!NOTE]
>
>Adobe Commerce i molninfrastrukturen skickar bara e-post under driftsättningsfasen.

```yaml
log:
  email:
    to: <your-email>
    from: <your-email>
    subject: "Log notification from Adobe Commerce"
    min_level: "notice"
```

- `to` - E-postadressen Adobe Commerce på molninfrastrukturen skickar meddelanden.
- `from` - E-postadress för att skicka meddelanden till mottagare.
- `subject` - Beskrivning av e-postmeddelandet.
- `min_level` - Minsta loggnivå för meddelanden. Vi rekommenderar att du använder `notice` eller `warning`.
