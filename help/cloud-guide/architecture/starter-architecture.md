---
title: Startarkitektur
description: Läs mer om de miljöer som stöds av Starter-arkitekturen.
feature: Cloud, Paas
exl-id: 2f16cc60-b5f7-4331-b80e-43042a3f9b8f
source-git-commit: 2236d0b853e2f2b8d1bafcbefaa7c23ebd5d26b3
workflow-type: tm+mt
source-wordcount: '1017'
ht-degree: 0%

---

# Startarkitektur

Din Adobe Commerce på molninfrastruktur Starter-arkitektur stöder upp till **fyra** -miljöer, inklusive en `master` -miljö som innehåller den inledande projektkoden, mellanlagringsmiljön och upp till två integreringsmiljöer.

Alla miljöer finns i PaaS-behållare (Platform as a service). Behållarna distribueras inuti mycket begränsade behållare på ett rutnät med servrar. De här miljöerna är skrivskyddade och godkänner distribuerade kodändringar från grenar som har skickats från den lokala arbetsytan. Varje miljö innehåller en databas- och webbserver.

>[!NOTE]
>
>Det går inte att ändra behörigheter för skrivskyddade mappar i någon av Starter-miljöerna. Den här begränsningen skyddar programmets integritet och säkerhet. Mappbehörigheter i dessa skrivskyddade filsystem kan inte ändras - inte ens supporten kan ändra dem. Alla ändringar måste göras från en gren i den lokala utvecklingsmiljön och skickas till programmiljön. Du kan använda vilken utvecklings- och förgrenade metod du vill. Skapa en `staging`-miljö från miljön `master` när du får den första åtkomsten till ditt projekt. Skapa sedan miljön `integration` genom att förgrena från `staging`.

## Arkitektur för startmiljö

I följande diagram visas de hierarkiska relationerna mellan Starter-miljöerna.

![Högnivåvy av Starter-projekt](../../assets/starter/architecture.png)

## Produktionsmiljö

Produktionsmiljön innehåller källkoden för att distribuera Adobe Commerce till molninfrastrukturen som kör dina offentliga butiker för en och flera platser. Produktionsmiljön använder kod från grenen `master` för att konfigurera och aktivera webbservern, databasen, konfigurerade tjänster och programkoden.

Eftersom miljön `production` är skrivskyddad kan du använda miljön `integration` för att göra kodändringar, distribuera över arkitekturen från `integration` till `staging` och slutligen till miljön `production`. Se [Distribuera din butik](../deploy/staging-production.md) och [Starta webbplatsen](../launch/overview.md).

Adobe rekommenderar att du testar fullständigt i din `staging`-gren innan du går till `master`-grenen, som distribuerar till `production`-miljön.

## Mellanlagringsmiljö

Adobe rekommenderar att du skapar en gren med namnet `staging` från `master`. Branschen `staging` distribuerar kod till mellanlagringsmiljön för att tillhandahålla en förproduktionsmiljö där du kan testa kod, moduler och tillägg, betalningsgateways, leveranser, produktdata och mycket annat. Den här miljön innehåller konfigurationen för alla tjänster som matchar produktionsmiljön, inklusive Fastly, New Relic APM och sökning.

Ytterligare avsnitt i den här guiden innehåller anvisningar för slutgiltig koddistribution och testning av interaktioner på produktionsnivå i en säker mellanlagringsmiljö. För bästa prestanda och funktionstestning ska du replikera databasen till mellanlagringsmiljön.

>[!WARNING]
>
>Adobe rekommenderar att man testar alla interaktioner mellan handlare och kunder i mellanlagringsmiljön innan man driftsätter i produktionsmiljön. Se [Distribuera din butik](../deploy/staging-production.md) och [Testa distributionen](../test/staging-and-production.md).

## Integreringsmiljö

Utvecklare använder miljön `integration` för att utveckla, distribuera och testa:

- Adobe Commerce-programkod

- Egen kod

- Tillägg

- Tjänster

**Rekommenderade användningsfall:**

Integrationsmiljöer är utformade för begränsad testning och utveckling. Du kan till exempel använda integreringsmiljön för att utföra följande uppgifter:

- Se till att ändringar i CI-processer är molnkompatibla

- Testa viktiga arbetsflöden på nyckelsidor som Hem, Kategori, Produktinformationssida (PDP), Checka ut och Admin

För bästa prestanda i integreringsmiljön bör du följa dessa standarder:

- Begränsa katalogstorleken - Som referens innehåller exempeldata cirka 2 048 produkter. Minska katalogstorleken till cirka 4 000-5 000 produkter.
Om du vill kontrollera antalet produkter i katalogen kör du följande MySQL-fråga:

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- Minska antalet kundgrupper - För många kundgrupper kan påverka indexeringens prestanda och övergripande prestanda.

- Begränsa användningen till en eller två samtidiga användare

- Inaktivera cron-jobb och kör manuellt efter behov

Du kan ha upp till **två** aktiva integreringsmiljöer. Du skapar en integreringsmiljö genom att skapa en gren från grenen `staging`. När du skapar en integreringsmiljö matchar miljönamnet förgreningsnamnet. En integreringsmiljö innehåller en webbserver och en databas. Det omfattar inte alla tjänster, till exempel Fast CDN och New Relic är inte tillgängliga.

Du kan ha ett obegränsat antal inaktiva grenar för kodlagring. Om du vill få åtkomst till, visa och testa en inaktiv gren måste du aktivera den

{{enhanced-integration-envs}}

## Produktions- och stagingteknologi

Produktions- och staging-miljöerna omfattar följande tekniker. Du kan ändra och konfigurera dessa tekniker med hjälp av filen [`.magento.app.yaml`](../application/configure-app-yaml.md).

- Snabbt för HTTP-cachning och CDN
- Nginx webbserver som talar till PHP-FPM, en instans med flera arbetare
- Redis-server
- Elasticsearch for catalog search for Adobe Commerce 2.2 to 2.4.3-p2
- OpenSearch - katalogsökning efter Adobe Commerce 2.3.7-p3, 2.4.3-p2 och 2.4.4 och senare
- Tryckfiltrering (utgående brandvägg)

### Tjänster

Adobe Commerce i molninfrastruktur stöder för närvarande följande tjänster: PHP, MySQL (MariaDB), Elasticsearch (Adobe Commerce 2.2 till 2.4.3-p2), OpenSearch (2.3.7-p3, 2.4.3-p2, 2.4.4 och senare), Redis och [!DNL RabbitMQ].

Varje tjänst körs i en separat, säker behållare. Behållare hanteras tillsammans i projektet. Vissa tjänster är standard, till exempel följande:

- HTTP-router (hanterar inkommande begäranden, men också cachelagring och omdirigeringar)

- PHP-programserver

- Git

- Secure Shell (SSH)

### Programversioner

Adobe Commerce i molninfrastruktur använder operativsystemet Debian GNU/Linux och NGINX-webbservern. Du kan inte uppgradera den här programvaran, men du kan konfigurera versioner för följande:

- [PHP](../application/php-settings.md)

- [MySQL](../services/mysql.md)

- [Redis](../services/redis.md)

- [KaninMQ](../services/rabbitmq.md)

- [Elasticsearch](../services/elasticsearch.md)

- [OpenSearch](../services/opensearch.md)

I testmiljöer och produktionsmiljöer använder du snabbt för CDN och cachning. Den senaste versionen av tillägget Snabbt CDN installeras när ditt projekt etableras. Du kan uppgradera tillägget för att få de senaste felkorrigeringarna och förbättringarna. Se [Snabb CDN-modul för Magento 2](https://github.com/fastly/fastly-magento2). Du har även åtkomst till [New Relic](../monitor/account-management.md) för prestandaövervakning.

Använd följande filer för att konfigurera de programversioner som du vill använda i implementeringen.

- [`.magento.app.yaml`](../application/configure-app-yaml.md)

- [`vägar.yaml`](../routes/routes-yaml.md)

- [&quot;services.yaml&quot;](../services/services-yaml.md)

### Säkerhetskopiering och katastrofåterställning

Du kan skapa en säkerhetskopia av din databas och ditt filsystem med hjälp av [!DNL Cloud Console] eller CLI. Se [Hantering av säkerhetskopiering](../storage/snapshots.md).

## Förbered för utveckling

I följande arbetsflöde sammanfattas processen för att dela ut koden, utveckla och distribuera butiken:

1. Konfigurera din lokala miljö

1. Klona grenen `master` till din lokala miljö

1. Skapa en `staging`-gren från `master`

1. Skapa grenar för utveckling från `staging`

1. Skicka kod till Git som bygger och distribuerar till en miljö för testning

I följande avsnitt finns detaljerade anvisningar och genomgångar för att utveckla, testa och driftsätta butiken:

- [Arbetsflöde för utveckling och driftsättning](starter-develop-deploy-workflow.md)

- [Dockerutveckling](../dev-tools/cloud-docker.md) (lokal utvecklingsmiljö aktiverad av Cloud Docker för Commerce)

- [Hantera grenar](../project/console-branches.md)

- [Distribuera din butik](../deploy/staging-production.md)

- [Starta webbplatsen](../launch/overview.md)
