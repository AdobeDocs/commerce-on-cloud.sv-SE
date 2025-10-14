---
title: Konfigurera ActiveMQ-tjänsten
description: Lär dig hur du aktiverar ActiveMQ Artemis-tjänsten för att hantera meddelandeköer för Adobe Commerce i molninfrastruktur.
feature: Cloud, Services
source-git-commit: ef22de6873b49f0fb9adfa9fc343a8d738a543e9
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# Konfigurera tjänsten [!DNL ActiveMQ]

[MQF (Message Queue Framework)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html?lang=sv-SE) är ett system i Adobe Commerce som tillåter en [modul](https://experienceleague.adobe.com/sv/docs/commerce-operations/implementation-playbook/glossary#module) att publicera meddelanden till köer. Det definierar också de konsumenter som tar emot meddelandena asynkront.

MQF kan använda [ActiveMQ Artemis](https://activemq.apache.org/components/artemis/) som meddelandeförmedlare, som tillhandahåller en skalbar plattform för att skicka och ta emot meddelanden. Den innehåller även en mekanism för att lagra olevererade meddelanden. [!DNL ActiveMQ Artemis] stöder protokollet STOMP (Streaming Text Oriented Messaging Protocol) för meddelanden.

[!DNL ActiveMQ Artemis] är tillgängligt som ett alternativ till RabbitMQ för att hantera meddelandeköer. Det är särskilt användbart när du behöver funktioner som är specifika för ActiveMQ eller vill använda STOMP-protokoll.

>[!IMPORTANT]
>
>Om du föredrar att använda en befintlig meddelandehanteringstjänst, som [!DNL ActiveMQ], i stället för att förlita dig på Adobe Commerce i molninfrastrukturen för att skapa den åt dig, använder du miljövariabeln [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) för att ansluta den till din webbplats.

{{service-instruction}}

**Så här aktiverar du ActiveMQ-objekt**:

1. Lägg till det namn, den typ och det diskvärde som krävs (i MB) till filen `.magento/services.yaml` tillsammans med den installerade ActiveMQ-versionen.

   ```yaml
   activemq-artemis:
       type: activemq-artemis:<version>
       disk: 1024
   ```

1. Konfigurera relationerna i filen `.magento.app.yaml`.

   ```yaml
   relationships:
       activemq-artemis: "activemq-artemis:activemq-artemis"
   ```

1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable ActiveMQ Artemis service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Verifiera tjänstrelationerna](services-yaml.md#service-relationships).

{{service-change-tip}}

## Anslut till ActiveMQ för felsökning

I felsökningssyfte kan du ansluta direkt till en tjänstinstans på något av följande sätt:

- Anslut från din lokala utvecklingsmiljö
- Anslut från programmet
- Anslut från ditt PHP-program

### Anslut från din lokala utvecklingsmiljö

1. Logga in på CLI och projekt för `magento-cloud`:

   ```bash
   magento-cloud login
   ```

1. Ta en titt på miljön med ActiveMQ Artemis installerat och konfigurerat.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Använd SSH för att ansluta till molnmiljön:

   ```bash
   magento-cloud ssh
   ```

1. Hämta ActiveMQ-anslutningsinformationen och inloggningsuppgifterna från variabeln [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships):

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   eller

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Leta reda på ActiveMQ-informationen i svaret. Relationsnamnet beror på hur du konfigurerade det i `.magento.app.yaml`-filen. Om du till exempel använde `activemq-artemis` som relationsnamn:

   ```json
   {
      "activemq-artemis" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "stomp",
            "port" : 61616,
            "host" : "activemq-artemis.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Aktivera lokal portvidarebefordran till ActiveMQ (om ditt projekt finns i en annan region, till exempel USA-3, EU-5 eller AP-3, ersätt ``us-3``/``eu-5``/``ap-3`` för ``us``)

   ```bash
   ssh -L <port-number>:activemq-artemis.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Ett exempel på åtkomst till ActiveMQ Artemis-webbkonsolen på `http://localhost:8161` är:

   ```bash
   ssh -L 8161:activemq-artemis.internal:8161 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   >[!NOTE]
   >
   >ActiveMQ Artemis använder port 61616 för STOMP-meddelanden och port 8161 för webbkonsolen.

1. När sessionen är öppen kan du komma åt ActiveMQ Artemis-webbkonsolen på `http://localhost:8161` med användarnamnet och lösenordet från variabeln MAGENTO_CLOUD_RELATIONSHIPS.

### Anslut från programmet

Installera ett STOMP-klientbibliotek i PHP-programmet om du vill ansluta till ActiveMQ som körs i ett program.

### Anslut från ditt PHP-program

Om du vill ansluta till ActiveMQ med ditt PHP-program lägger du till ett PHP STOMP-bibliotek i källträdet. Adobe Commerce använder STOMP-protokollet för ActiveMQ-kommunikation, och konfigurationen konfigureras automatiskt under distributionen när ActiveMQ Artemis identifieras som en konfigurerad tjänst.

## Protokollstöd

ActiveMQ Artemis i Adobe Commerce i molninfrastruktur använder protokollet STOMP (Streaming Text Oriented Messaging Protocol):

- **STOMP**: Meddelandeprotokollet som används för köåtgärder (port 61616)
- **Webbkonsol**: Hanteringsgränssnittet är tillgängligt via HTTP (port 8161)

## Skillnader från RabbitMQ

Även om både [!DNL ActiveMQ Artemis] och [!DNL RabbitMQ] fungerar som meddelandeförmedlare för Adobe Commerce finns det vissa skillnader:

- **Protokoll**: ActiveMQ Artemis använder STOMP-protokoll, medan RabbitMQ använder AMQP
- **Konfiguration**: När ActiveMQ-artemis har konfigurerats använder Adobe Commerce automatiskt STOMP-protokoll
- **Prioritet**: Om både ActiveMQ och RabbitMQ har konfigurerats har ActiveMQ företräde för STOMP-baserade åtgärder, medan AMQP-åtgärder använder RabbitMQ
- **Webbkonsol**: ActiveMQ innehåller en webbaserad hanteringskonsol för övervakning av köer och meddelanden

## Kökonfiguration

När ActiveMQ Artemis har konfigurerats som en tjänst konfigurerar Adobe Commerce automatiskt kösystemet till att använda STOMP-protokollet. Konfigurationen skrivs till filen `app/etc/env.php` under distributionen:

```php
'queue' => [
    'stomp' => [
        'host' => 'activemq-artemis.internal',
        'port' => '61616',
        'user' => 'guest',
        'password' => 'guest'
    ],
    'default_connection' => 'stomp'
]
```

Du kan åsidosätta den här konfigurationen med miljövariabeln [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) om det behövs.

