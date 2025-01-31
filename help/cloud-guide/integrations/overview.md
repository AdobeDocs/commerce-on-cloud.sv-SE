---
title: Integrering - översikt
description: Läs mer om tredjepartsintegreringsalternativ för ditt Adobe Commerce i molninfrastrukturprojekt.
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Integrering - översikt

Integreringar är användbara när du använder externa tjänster, som Git-värdtjänster eller Slack-bots, och för att underhålla aktuella utvecklingsprocesser, som att använda funktionen pull-begäran för kodgranskning i GitHub. Du kan lägga till följande integreringar i ditt Adobe Commerce i molninfrastrukturprojekt:

![Integrationer](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**Så här lägger du till en integrering med Cloud CLI**:

Följande kommando startar interaktiva uppmaningar för att välja typ och alternativ för den nya integreringen.

```bash
magento-cloud integration:add
```

**Så här listar du de konfigurerade integreringarna för ditt projekt**:

```bash
magento-cloud integration:list
```

Exempelsvar:

```
+----------+--------------+---------------------------------------------------------------------------+
| ID       | Type         | Summary                                                                   |
+----------+--------------+---------------------------------------------------------------------------+
| <int-id> | bitbucket    | Repository: user/magento-int                                              |
|          |              | Hook URL:                                                                 |
|          |              | https://magento-url.cloud/api/projects/projectID/integrations/int-ID/hook |
| <int-id> | health.email | From: you@example.com                                                     |
|          |              | To: them@example.com                                                      |
+----------+--------------+---------------------------------------------------------------------------+
```

>[!TAB Konsol]

**Så här lägger du till en integrering med[!DNL Cloud Console]**:

1. Klicka på **[!UICONTROL Integrations]** i _Projektinställningar_.

1. Klicka på en integrationstyp eller klicka på **[!UICONTROL Add integration]**.

1. Gå igenom valet av integrationstyp och konfigurationssteg.

1. När du har lagt till integreringen visas den i listan i vyn Integrationer.

>[!ENDTABS]

## Commerce webhooks

Du kan konfigurera Commerce-webbböcker i ditt molnprojekt med den globala variabeln [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks). Commerce webbhooks skickar begäranden till en extern server som svar på händelser som genererats av Commerce. [_Webhooks-guiden_](https://developer.adobe.com/commerce/extensibility/webhooks) beskriver den här funktionen i detalj.

## Allmänna webhooks

Du kan samla in och rapportera molninfrastruktur- och databashändelser med hjälp av en anpassad webkrok-integrering till `POST` JSON-meddelanden till en _webkroks_ -URL.

**Använd följande syntax** om du vill lägga till en webkroks-URL:

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type` - Ange integrationstypen `webhook`.
- `url` - Ange webbkroks-URL som kan ta emot JSON-meddelanden.

I exempelsvaret visas en serie uppmaningar som ger möjlighet att anpassa integreringen. Om du använder standardsvaret (tomt) skickas meddelanden om alla händelser i alla miljöer i ett projekt.

Du kan anpassa integreringen för att rapportera specifika [händelser](#events-to-report), som att skicka kod till en gren. Du kan till exempel ange händelsen `environment.push` för att skicka ett meddelande när en användare skickar kod till en gren:

```
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

Du kan välja att rapportera händelser i ett `pending`-, `in_progress`- eller `complete`-läge:

```
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

Du kan även _ta med_ eller _exkludera_ meddelanden för specifika miljöer:

```
Included environments (--environments)
The environment IDs to include
Default: *
Enter comma-separated values (or leave this blank)
>

Excluded environments (--excluded-environments)
The environment IDs to exclude
Enter comma-separated values (or leave this blank)
>
```

När integreringen är klar får du en sammanfattning av värdena:

```
Created integration integration-ID (type: webhook)
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - complete                   |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Uppdatera befintlig integrering

Du kan uppdatera en befintlig integrering. Ändra till exempel lägena från `complete` till `pending` med följande:

```bash
magento-cloud integration:update --states=pending <int-id>
```

Exempelsvar:

```
Integration integration-ID (webhook) updated
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - pending                    |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Händelser som ska rapporteras

| Händelse | Beskrivning |
| ----- | :-----------|
| `environment.access.add` | En användare har beviljats åtkomst till miljön |
| `environment.access.remove` | En användare har tagits bort från miljön |
| `environment.activate` | En gren har aktiverats med en miljö |
| `environment.backup` | En användare utlöste en ögonblicksbild |
| `environment.branch` | En gren har skapats med hanteringskonsolen |
| `environment.deactivate` | En gren har inaktiverats. Koden finns fortfarande kvar, men miljön har förstörts |
| `environment.delete` | En gren har tagits bort |
| `environment.initialize` | Projektets `master`-gren initierades med en första implementering |
| `environment.merge` | En aktiv gren har sammanfogats med hanteringskonsolen eller API |
| `environment.push` | En användare skickade kod till en gren |
| `environment.restore` | En användare återställde en ögonblicksbild |
| `environment.route.create` | En väg har skapats med hanteringskonsolen |
| `environment.route.delete` | En väg har tagits bort med hanteringskonsolen |
| `environment.route.update` | En väg har ändrats med hanteringskonsolen |
| `environment.subscription.update` | Miljön `master` har ändrat storlek eftersom prenumerationen har ändrats, men det finns inga innehållsändringar |
| `environment.synchronize` | Data eller kod har återställts från den överordnade miljön i en miljö |
| `environment.update.http_access` | HTTP-åtkomstreglerna för en miljö har ändrats |
| `environment.update.restrict_robots` | Funktionen för att blockera alla robotar har aktiverats eller inaktiverats |
| `environment.update.smtp` | E-postutskick har aktiverats eller inaktiverats för en miljö |
| `environment.variable.create` | En variabel har skapats |
| `environment.variable.delete` | En variabel har tagits bort |
| `environment.variable.update` | En variabel har ändrats |
| `project.domain.create` | En domän har skapats och lagts till i projektet |
| `project.domain.delete` | En domän som är associerad med projektet har tagits bort |
| `project.domain.update` | En domän som är associerad med projektet har uppdaterats |
