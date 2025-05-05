---
title: Pro-arkitektur
description: Läs om de miljöer som stöds av Pro-arkitekturen.
feature: Cloud, Auto Scaling, Iaas, Paas, Storage
topic: Architecture
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1554'
ht-degree: 0%

---

# Pro-arkitektur

Din Adobe Commerce på molninfrastruktur Pro-arkitektur stöder flera miljöer som du kan använda för att utveckla, testa och lansera din butik.

- **Master** - Tillhandahåller en `master`-gren som distribuerats till Platform som en tjänstbehållare (PaaS).
- **Integration** - Tillhandahåller en enskild `integration`-gren för utveckling, men du kan skapa ytterligare en gren. Detta tillåter upp till två _aktiva_ grenar som distribueras till plattformar som tjänstbehållare (PaaS).
- **Förproduktion** - Tillhandahåller en enskild `staging`-gren som distribuerats till dedikerad infrastruktur som IaaS-behållare (Service).
- **Produktion** - Tillhandahåller en enskild `production`-gren som distribuerats till dedikerad infrastruktur som IaaS-behållare (Service).

I följande tabell sammanfattas skillnaderna mellan miljöerna:

|                                        | INTEGRERING | STAGNING | PRODUKTION |
| -------------------------------------- | ----------- | ----------------- | -------------------- |
| Stöder inställningshantering i [!DNL Cloud Console] | Ja | Begränsad | Begränsad |
| Stöder flera grenar | Ja | Nej (endast mellanlagring) | Nej (endast produktion) |
| Använder YAML-filer för konfiguration | Ja | Nej | Nej |
| Körs på dedikerad IaaS-maskinvara | Nej | Ja | Ja |
| Fast CDN ingår | Nej | Ja | Ja |
| Inkluderar New Relic | Nej | APM | APM + NRI |
| Automatisk säkerhetskopiering | Nej | Ja | Ja |

>[!NOTE]
>
>Adobe tillhandahåller verktyget Cloud Docker för Commerce för distribution till en lokal Cloud Docker-miljö så att du kan utveckla och testa Adobe Commerce-projekt. Se [Dockerutveckling](../dev-tools/cloud-docker.md).

## Miljöarkitektur

Ditt projekt är en enda Git-databas med tre huvudmiljögrenar: `integration`, `staging` och `production`. I följande diagram visas det hierarkiska förhållandet mellan Pro-miljöer:

![Högnivåvy av Pro-miljöarkitekturen](../../assets/pro-branch-architecture.png)

### Huvudmiljö

I Pro-projekt tillhandahåller grenen `master` en aktiv PaaS-miljö med din produktionsmiljö. Skicka alltid en kopia av produktionskoden till `master`-miljön så att du kan felsöka produktionsmiljön utan att avbryta tjänsterna.

**Caveats:**

- Skapa **inte** en gren baserat på grenen `master`. Använd integreringsmiljön för att skapa aktiva grenar för utveckling.

- Använd inte miljön `master` för utveckling, UAT eller prestandatestning

### Integreringsmiljö

Integreringsmiljön körs i en Linux-behållare (LXC) på ett rutnät med servrar som kallas PaaS. Varje miljö innehåller en webbserver och databas som testar platsen. Se [Regionala IP-adresser](../project/regional-ip-addresses.md) för en lista över IP-adresser för AWS och Azure.

**Rekommenderade användningsfall:**

Integrationsmiljöer är utformade för begränsad testning och utveckling innan ändringar görs i miljöer för testning och produktion. Du kan till exempel använda integreringsmiljön för att utföra följande uppgifter:

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

**Caveats:**

- CDN- och New Relic-tjänsterna är inte tillgängliga i en integreringsmiljö

- Integreringsmiljöns arkitektur matchar inte arkitekturen för förproduktion och produktion

- Använd inte miljön `integration` för utvecklingstestning, prestandatestning eller UAT (User accept testing)

- Använd inte miljön `integration` för att testa B2B-funktioner för Adobe Commerce

- Du kan inte återställa databasen i integreringsmiljön från databasproduktionen eller mellanlagringen

{{enhanced-integration-envs}}

### Mellanlagringsmiljö

Mellanlagringsmiljön är en miljö för närproduktion som du kan använda för att testa platsen. Den här miljön, som finns på dedikerad IaaS-maskinvara, innehåller alla tjänster, som Fastly CDN, New Relic APM och sökning.

**Rekommenderade användningsfall:**

Miljön matchar produktionsarkitekturen och är utformad för UAT, innehållstaging och slutlig granskning innan funktioner skickas till miljön `production`. Du kan till exempel använda miljön `staging` för att utföra följande åtgärder:

- Regressionstestning mot produktionsdata

- Prestandatestning med snabb cachelagring aktiverat

- Testa nya byggen i stället för att laga i produktionen

- UAT-testning för nya byggen

- Testa B2B för Adobe Commerce

- Anpassa cron-konfiguration och testa cron-jobb

Se [Arbetsflöde för distribution](pro-develop-deploy-workflow.md#deployment-workflow) och [Testa distribution](../test/staging-and-production.md).

**Caveats:**

- När du har startat produktionsplatsen använder du i första hand testmiljön för att testa korrigeringar för produktionskritiska buggfixar.

- Du kan inte skapa en gren från grenen `staging`. Du kan istället överföra kodändringar från grenen `integration` till grenen `staging`.

{{second-staging}}

### Produktionsmiljö

Produktionsmiljön kör dina offentliga butiker för en och flera platser. Den här miljön använder dedikerad IaaS-maskinvara med redundanta noder med hög tillgänglighet för kontinuerlig åtkomst och failover-skydd för dina kunder. I produktionsmiljön ingår alla tjänster i mellanlagringsmiljön, plus tjänsten [New Relic Infrastructure (NRI)](../monitor/new-relic-service.md#new-relic-infrastructure) som automatiskt ansluter till programdata och prestandaanalyser för att tillhandahålla dynamisk serverövervakning.

**Caveat:**

Du kan inte skapa en gren från grenen `production`. Du kan istället överföra kodändringar från grenen `staging` till grenen `production`.

### Production Technology stack

Produktionsmiljön har tre virtuella datorer (VM) bakom en elastisk belastningsutjämnare som hanteras av en HAProxy per VM. Varje virtuell dator innehåller följande tekniker:

- **Snabbt CDN** - HTTP-cachelagring och CDN

- **NGINX** - webbserver som använder PHP-FPM, en instans med flera arbetare

- **GlusterFS** - filserver för hantering av alla statiska fildistributioner och synkronisering med fyra kataloguppsättningar:

   - `var`
   - `pub/media`
   - `pub/static`
   - `app/etc`

- **Redis** - en server per VM med endast en aktiv och de andra två som repliker

- **Elasticsearch** - sök efter Adobe Commerce om molninfrastruktur 2.2 till 2.4.3-p2

- **OpenSearch** - sök efter Adobe Commerce i molninfrastruktur 2.3.7-p3, 2.4.3-p2, 2.4.4 och senare

- **Galera** - databaskluster med en MariaDB MySQL-databas per nod med en inställning för automatisk ökning på tre för unika ID:n i alla databaser

Följande bild visar vilka tekniker som används i produktionsmiljön:

![Produktionsteknikstack](../../assets/az-stack-diagram.png)

## Redundanta maskinvara

I stället för att köra en traditionell, aktiv-passiv `master` eller en primär-sekundär konfiguration kör Adobe Commerce i molninfrastrukturen en _redundant arkitektur_ där alla tre instanserna accepterar läsningar och skrivningar. Denna arkitektur erbjuder inga driftavbrott vid skalning och ger garanterad transaktionsintegritet.

På grund av den unika, redundanta maskinvaran kan Adobe tillhandahålla tre gateway-servrar. De flesta externa tjänster gör att du kan lägga till flera IP-adresser till en tillåtelselista, så att det inte är något problem att ha fler än en fast IP-adress. De tre gatewayarna mappas till de tre servrarna i produktionsmiljöklustret och behåller statiska IP-adresser. Den är helt överflödig och mycket tillgänglig på alla nivåer:

- DNS
- CDN (Content Delivery Network)
- Elastisk belastningsutjämnare (ELB)
- Tre serverkluster som omfattar alla Adobe Commerce-tjänster, inklusive databas- och webbservern

## Säkerhetskopiering och katastrofåterställning

Adobe Commerce i molninfrastruktur använder en arkitektur med hög tillgänglighet som replikerar varje Pro-projekt på tre separata AWS- eller Azure-tillgänglighetszoner, där varje zon har ett separat datacenter. Förutom den här redundansen får Pro-testnings- och produktionsmiljöer regelbundna, livesäkerhetskopieringar som är utformade för att användas vid _katastrofala fel_.

**Automatiska säkerhetskopieringar** innehåller beständiga data från alla tjänster som körs, till exempel MySQL-databasen och filer som lagras på de monterade volymerna. Säkerhetskopiorna sparas i krypterad Elastic Block Storage (EBS) i samma region som produktionsmiljön. De automatiska säkerhetskopiorna är inte tillgängliga för allmänheten eftersom de lagras i ett separat system.

>[!NOTE]
>
>De monterade volymerna innehåller/refererar endast till [skrivbara monteringar](https://experienceleague.adobe.com/sv/docs/commerce-on-cloud/user-guide/configure/app/properties/properties#mounts) och kommer inte att innehålla hela din `app/`-katalog. Liksom för de andra filerna skapas/genereras de av [bygg- och distributionsprocessen](https://experienceleague.adobe.com/sv/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow), och du måste också kontrollera din Git-databas för att se om det finns återstående filer.

{{pro-backups}}

Du kan skapa en **manuell säkerhetskopia** av databasen för mellanlagrings- och produktionsmiljöer med CLI-kommandon. Se [Säkerhetskopiera databasen](../storage/database-dump.md). För `integration`-miljöer rekommenderar Adobe att du skapar en säkerhetskopia som ett första steg efter att du har öppnat ditt Adobe Commerce i ett molninfrastrukturprojekt och innan du gör några större ändringar. Se [Hantering av säkerhetskopiering](../storage/snapshots.md).

### Mål för återställningspunkt

Kontakta Adobe Customer Success Manager om du vill ha information om när återställningspunkten ska användas för den senaste säkerhetskopieringen. Hur ofta säkerhetskopieringar ska göras beror på schemat för säkerhetskopiering av din plan och hur många ändringar som ska skrivas till lagringstjänsten.

### Bevarandeprincip

Adobe behåller automatiska säkerhetskopieringar enligt följande datalagringspolicy:

| Tidsperiod | Princip för kvarhållning av säkerhetskopia |
| ------------------ | ----------------------- |
| Dag 1 till 3 | En säkerhetskopia per timme |
| Dagar 4 till 7 | En säkerhetskopiering per dag |
| Vecka 2 till 6 | En säkerhetskopia per vecka |
| Vecka 8 till 12 | En säkerhetskopiering varannan vecka |
| Månad 3 till 5 | En säkerhetskopia per månad |

Den här principen kan variera beroende på din molninfrastrukturplan.

### Mål för återställningstid

RTO beror på lagringens storlek. Stora EBS-volymer tar längre tid att återställa. Återställningstiden kan variera beroende på databasens storlek. Kontakta din kundansvarige på Adobe om du vill ha mer information.

## Pro, klusterskalning

Pro-klusterstorleken och _beräkna_-konfigurationerna varierar beroende på den valda molnprovidern (AWS, Azure), regionen och tjänstberoendena. Adobe molninfrastruktur kan skala Pro-kluster för att passa trafikens förväntningar och servicekraven när efterfrågan förändras.

Den redundanta arkitekturen gör att molninfrastrukturen i Adobe kan byggas ut utan driftstopp. Vid uppskalning roterar var och en av de tre instanserna till uppgraderingskapacitet utan att påverka webbplatsens funktion. Du kan till exempel lägga till extra webbservrar i ett befintligt kluster om gränsen är på PHP-nivå i stället för på databasnivå. Detta ger _vågrät skalning_ som komplement till den lodräta skalningen som tillhandahålls av extra processorer på databasnivån. Se [Skalad arkitektur](scaled-architecture.md).

Om du förväntar dig en avsevärd ökning av trafiken för en händelse eller av någon annan anledning kan du begära en tillfällig kapacitetsökning. Se [Så här begär du en tillfällig storlek](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html?lang=sv-SE) i _Commerce Help Center_.
