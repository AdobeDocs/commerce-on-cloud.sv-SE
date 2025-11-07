---
title: Konfigurera tjänsten RabbitMQ
description: Lär dig hur du aktiverar tjänsten RabbitMQ för att hantera meddelandeköer för Adobe Commerce i molninfrastruktur.
feature: Cloud, Services
exl-id: 64af1dfa-e3f0-4404-a352-659ca47c1121
source-git-commit: 76a9721767cbd4328347311cc308810f0f7914c0
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Konfigurera tjänsten [!DNL RabbitMQ]

[MQF (Message Queue Framework)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html?lang=sv-SE) är ett system i Adobe Commerce som tillåter en [modul](https://experienceleague.adobe.com/sv/docs/commerce-operations/implementation-playbook/glossary#module) att publicera meddelanden till köer. Det definierar också de konsumenter som tar emot meddelandena asynkront.

MQF använder [RabbitMQ](https://www.rabbitmq.com/) som meddelandeförmedlare, som tillhandahåller en skalbar plattform för att skicka och ta emot meddelanden. Den innehåller även en mekanism för att lagra olevererade meddelanden. [!DNL RabbitMQ] baseras på specifikationen Advanced Message Queuing Protocol (AMQP) 0.9.1.

>[!NOTE]
>
>Adobe Commerce i molninfrastrukturen stöder även [ActiveMQ Artemis](activemq.md) som en alternativ meddelandekötjänst med STOMP-protokollet.

>[!IMPORTANT]
>
>Om du föredrar att använda en befintlig AMQP-baserad tjänst, som [!DNL RabbitMQ], i stället för att förlita dig på Adobe Commerce i molninfrastrukturen för att skapa den åt dig, använder du miljövariabeln [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) för att ansluta den till webbplatsen.

{{service-instruction}}

**Så här aktiverar du RabbitMQ**:

1. Lägg till det namn, den typ och det diskvärde som krävs (i MB) till filen `.magento/services.yaml` tillsammans med den installerade versionen av RabbitMQ.

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. Konfigurera relationerna i filen `.magento.app.yaml`.

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Verifiera tjänstrelationerna](services-yaml.md#service-relationships).

{{service-change-tip}}

## Anslut till RabbitMQ för felsökning

I felsökningssyfte är det praktiskt att ansluta direkt till en tjänstinstans på något av följande sätt:

- Anslut från din lokala utvecklingsmiljö
- Anslut från programmet
- Anslut från ditt PHP-program

### Anslut från din lokala utvecklingsmiljö

1. Logga in på CLI och projekt för `magento-cloud`:

   ```bash
   magento-cloud login
   ```

1. Kolla in miljön med RabbitMQ installerad och konfigurerad.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Använd SSH för att ansluta till molnmiljön:

   ```bash
   magento-cloud ssh
   ```

1. Hämta RabbitMQ-anslutningsinformationen och inloggningsuppgifterna från variabeln [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships):

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   eller

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   I svaret kan du hitta informationen om RabbitMQ, till exempel:

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Aktivera lokal portvidarebefordran till RabbitMQ (om ditt projekt finns i en annan region, till exempel USA-3, EU-5 eller AP-3, ersätt ``us-3``/``eu-5``/``ap-3`` för ``us``)

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Ett exempel på åtkomst till webbgränssnittet för RabbitMQ-hantering på `http://localhost:15672` är:

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. När sessionen är öppen kan du starta en valfri RabbitMQ-klient från din lokala arbetsstation som är konfigurerad att ansluta till `localhost:<portnumber>` med portnumret, användarnamnet och lösenordsinformationen från variabeln MAGENTO_CLOUD_RELATIONSHIPS.

### Anslut från programmet

Om du vill ansluta till RabbitMQ som körs i ett program installerar du en klient, till exempel [amqp-utils](https://github.com/dougbarth/amqp-utils), som ett projektberoende i `.magento.app.yaml`-filen.

Exempel:

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

När du loggar in på PHP-behållaren anger du ett `amqp-`-kommando som är tillgängligt för att hantera köerna.

### Anslut från ditt PHP-program

Om du vill ansluta till RabbitMQ med ditt PHP-program lägger du till ett PHP-bibliotek i källträdet.

## Felsökning av tjänsten [!DNL RabbitMQ]

Se [Det går inte att ansluta till RabbitMQ i Adobe Commerce Cloud](https://experienceleague.adobe.com/sv/docs/experience-cloud-kcs/kbarticles/ka-27688).

## Uppgraderar tjänsten [!DNL RabbitMQ]

Uppgraderingsinstruktioner finns i [Ändra tjänstversion](https://experienceleague.adobe.com/sv/docs/commerce-on-cloud/user-guide/configure/service/services-yaml#change-service-version).
