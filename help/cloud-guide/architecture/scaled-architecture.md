---
title: Skalbar arkitektur
description: Lär dig mer om arkitekturen i de olika skikten och hur den kan anpassas efter efterfrågan.
feature: Cloud, Auto Scaling, Iaas, Logs
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Skalbar arkitektur

Molninfrastrukturen skalas efter dina resurskrav för att bli mer effektiv. Adobe Commerce i molninfrastrukturen övervakar dina program och kan justera kapaciteten för att upprätthålla stabila och förutsägbara prestanda. Att konvertera till den här arkitekturen bidrar till att minska problem som fördröjning eller stora trafiktoppar.

>[!NOTE]
>
>Den skalade arkitekturen är tillgänglig för Adobe Commerce på molninfrastrukturkonton med Pro 48-klustret eller senare.

## Split-tier architecture

Tidigare bestod Pro-arkitekturen av tre noder som var och en innehöll en fullständig teknisk stack. Nu finns det en skalbar infrastruktur som erbjuder en skiktad arkitektur med minst sex noder: tre noder för kärndatabasen och bastjänsterna samt tre noder för webbservern. Denna arkitektur ger möjlighet att skala nivåer oberoende av varandra för att uppnå en optimal balans mellan prestanda.

### Tjänstnivå

Det finns tre tjänstnoder för datalagring, cache och tjänster: **OpenSearch** eller **Elasticsearch**, **MariaDB**, **Redis** med flera. När tjänstnivån närmar sig kapaciteten är det enda sättet att skala upp serverstorleken, som att öka CPU-kraften och minnet. Kapaciteten är begränsad till storleken på noden som är tillgänglig. Eftersom databaskluster är utformat för hög tillgänglighet kan du inte skala vågrätt på ett tillförlitligt sätt med de tekniker som används.

![Skalning på tjänstnivå](../../assets/scaling-service.png)

Tänk dig ett exempel på att tjänstnodens instanstyp är _m5.2xlarge_ med 32 Gbit RAM. En tjänst, till exempel databasen, använder mycket minne (30 GB). Skalning till nästa tillgängliga instansstorlek, _m5.4xlarge_, ger 64 GB RAM, vilket fördubblar minnet och passar databasens växande behov.

Du kan optimera prestandan för tjänstnivån ytterligare genom att dirigera trafik baserat på nodtypen. Som standard är databasnoden isolerad från webbtrafiken. Du kan till exempel välja att betjäna webbtrafik på databasnoden.

### Webbnivå

Det finns tre webbnoder för bearbetning av begäranden och webbtrafik: **php-fpm** och **NGINX**. Utöver vertikal skalning genom att öka strömförsörjning och minne kan webbnivån skalas vågrätt genom att lägga till webbservrar i ett befintligt kluster när det begränsas på PHP-nivå. Se [Automatisk skalning](autoscaling.md) om du vill veta hur webbnoderna skalas automatiskt.

![Skalning på webbnivå](../../assets/scaling-web.png)

Detta kompletterar den lodräta skalförändring som tillhandahålls av tjänstnivån. När tjänstenivån skalas i storlek och i kraft för att passa en växande databas- och serviceanvändning, skalas webbnivån i storlek, effekt och instanser för att passa en ökning av processförfrågningar och högre trafikkrav.

Tänk dig ett exempel på att webbnodens instanstyp är _C5.2xlarge med åtta processorer och 16 Gbit RAM_. Antalet förfrågningar till webbplatsen ökade avsevärt. Du kan lägga till en C5.2xlarge-nod för att hantera ökningen av php-fpm-processer eller ändra varje instanstyp till _C5.4xlarge med 16 CPU och 32 Gbit RAM_ . Genom att lägga till en nod minskar risken för otillräcklig överkapacitet.

## Projektstruktur

Pro-projekt med den skalade arkitekturen har minst sex noder tillgängliga.

- 3 webbnoder c5.2xlarge (8 CPU, 16 Gbit RAM)

- 3 servicenoder m5.2xlarge (8 CPU, 32 Gbit RAM)

Varje projekt är dock unikt och kräver prestandaövervakning för att kunna analysera resurshanteringen på rätt sätt. Varje konto innehåller [New Relic-tjänsten](../monitor/new-relic-service.md) som automatiskt ansluter till programdata och prestandaanalyser för att tillhandahålla dynamisk serverövervakning. Du kan använda tjänsten New Relic för att övervaka användningen av CPU och RAM för att avgöra vilka noder som kräver ytterligare resurser. När en resurs når sin kapacitet eller du märker en prestandaförsämring som baseras på analysen, kan du skapa en begäran om att skala din infrastruktur så att den uppfyller kraven.

### SSH-åtkomst

Vissa filer och loggar, som katalogen `/app/<project-id>/var/log`, delas inte mellan noder. Varje nod har en unik SSH-åtkomst. Du kan inte använda CLI `magento-cloud` för att logga in på tjänsten eller webbnoderna, men du hittar nodadresserna i SSH-åtkomstlistan i [!DNL Cloud Console].

```bash
ssh <node>.<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
```

- `node` 1 till 3 - Adresser för åtkomst till tjänstnoderna

- `node` 4 till _n_ - Adresser för åtkomst till webbnoderna

>[!TIP]
>
>När du har loggat in kan du bekräfta server-ID:t och rollen: tjänstnoderna använder rollen _unified_ och webbnoderna använder rollen _web_.

Exempelsvar när du loggar in på en **tjänstnod** innehåller rollen _unified_:

```
 __  __                   _          ___ _             _
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/

 Welcome to Magento Cloud.

 This is server unique-server-id, role project-id:unified.

project-id@server-id:~$
```

Exempelsvar när du loggar in på en **webbnod** innehåller rollen _web_:

```
 __  __                   _          ___ _             _
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/

 Welcome to Magento Cloud.

 This is server unique-server-id, role project-id:web.

project-id@server-id:~$
```

### Loggplatser

Loggplatserna varierar något beroende på noden. En databaslogg, till exempel **MySQL-felloggen**, är tillgänglig på en tjänstnod (`/var/log/mysql/mysql-error.log`), men är inte tillgänglig på en webbnod.

Varje Pro-konto innehåller [tjänsten New Relic Logs](../monitor/new-relic-service.md) som automatiskt ansluter till loggdata från programmet för att tillhandahålla dynamisk logghantering. Sammanställda loggdata från alla noder visas i programmet New Relic Logs så att du kan felsöka prestandaproblem på specifika noder från en enda instrumentpanel.
