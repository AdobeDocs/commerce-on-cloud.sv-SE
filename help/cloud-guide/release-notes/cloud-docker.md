---
title: Cloud Docker-paket
description: Se en lista över de senaste förbättringarna av Cloud Docker-paketet.
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2025-04-07T00:00:00Z
exl-id: 95cf4f30-6bce-4bac-8e11-cfe53cac2c70
source-git-commit: 5e991f974f33b35497b09c10fde36850c6279586
workflow-type: tm+mt
source-wordcount: '3710'
ht-degree: 0%

---

# Cloud Docker-paket

Paketet [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) innehåller funktioner och dockningsavbildningar för att distribuera Adobe Commerce till en lokal molnmiljö. Versionsinformationen beskriver de senaste förbättringarna av det här paketet, som är en komponent i [Cloud Tools Suite för Commerce](cloud-tools-suite.md).

Paketet `magento/magento-cloud-docker` använder följande versionssekvens: `<major>.<minor>.<patch>`

Versionsinformationen innehåller:

- ![ny ikon](../../assets/new.svg) Nya funktioner
- ![korrigeringsikon](../../assets/fix.svg) Korrigeringar och förbättringar

<!--Add release notes below-->

## v1.4.2 {#latest}

Releasedatum: 7 april 2025

- ![ny ikon](../../assets/new.svg) **PHP 8.4** - `php-cli` 8.4- och `php-fpm` 8.4-bilder har lagts till.


## v1.4.1

Releasedatum: 6 februari 2025

- ![ny ikon](../../assets/new.svg) **PHP 8.4** - Stöd för PHP 8.4 har lagts till.


## v1.4.0

Releasedatum: 7 oktober 2024

- ![korrigeringsikon](../../assets/fix.svg) **Refererad kod** - Stödet för tidigare PHP-versioner (7.4, 7.3, 7.2) och relaterade bibliotek och bilder har tagits bort.

## v1.3.7

Releasedatum: 8 april 2024

- ![ny ikon](../../assets/new.svg) **PHP** - Stöd för bilder i PHP 8.3 och PHP 8.3 har lagts till.
- ![ny ikon](../../assets/new.svg) **Nginx** - Bild nybörjare v. 1.24 har lagts till.
- ![ny ikon](../../assets/new.svg) **OpenSearch** - Lagt till bild OpenSearch v. 2.12, 1.3.
- ![ny ikon](../../assets/new.svg) **Disposition** - Composer-versionen har uppdaterats till 2.2.23.

## v1.3.6

Releasedatum: 31 juli 2023

- ![ny ikon](../../assets/new.svg) **Lagt till ny tjänstversion** - OpenSearch 2.5.
- ![ny ikon](../../assets/new.svg) **Aktivera cacheminnet för disposition** - Nu kan du utöka Docker-konfigurationen för att aktivera rensningscache för disposition när du startar Docker-behållaren. Se [Utöka dockningskonfigurationen](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/) i guiden _Cloud Docker för Commerce_.

## v1.3.5

Releasedatum: 10 mars 2023

- ![ny ikon](../../assets/new.svg) **iconCube** - har lagts till i ikontillägget för PHP 8.1-bilden.
- ![ny ikon](../../assets/new.svg) **Lagt till nya tjänstversioner** - OpenSearch 2.3 och 2.4, PHP 8.2, lack 7.1.1.
- ![ny ikon](../../assets/new.svg) **Förbättrat stöd för PHP 8.2** - Korrigerade kompatibilitetsproblem med vissa PHP 8.2.x-versioner som stöder Commerce 2.4.6.
- ![korrigeringsikon](../../assets/fix.svg) **Problem med disposition** - Problem som uppstod efter att Composer-versionen uppdaterats i Docker-behållarna har åtgärdats.

## v1.3.4

Releasedatum: 27 oktober 2022

- ![ny ikon](../../assets/new.svg) **Lagt till nya lack-bilder** - Lagt till bilder för lack 6.5, 7.0 och 7.1.<!-- MCLOUD-7879 -->

## v1.3.3

Releasedatum: 13 september 2022

- ![ny ikon](../../assets/new.svg) **Stöd för Apple M1 (ARM64)** - Ändringar har lagts till i Docker-bilder för att aktivera stöd för Apple M1-arkitektur (ARM64).<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![korrigeringsikon](../../assets/fix.svg) **Mailhog** - Korrigerade ett fel där e-postmeddelanden inte kunde fångas upp i Mailhog-tjänsten i utvecklarläge.<!-- MCLOUD-8643 -->
- ![korrigeringsikon](../../assets/fix.svg) **init-docker.sh** - Tjänstversionernas validerare i `init-docker.sh`-skriptet har korrigerats.<!-- MCLOUD-8765 -->

## v1.3.2

Releasedatum: 31 mars 2022

- ![ny ikon](../../assets/new.svg) **Lagt till Elasticsearch 7.10-bild**<!-- MCLOUD-8548 -->

## v1.3.1

Releasedatum: 10 mars 2022

- ![ny ikon](../../assets/new.svg) **Stöd för PHP 8.1** - Stöd för PHP 8.1 har lagts till.
- ![ny ikon](../../assets/new.svg) **OpenSearch** - Bilder av OpenSearch-versionerna 1.1 och 1.2 har lagts till.
- ![new icon](../../assets/new.svg) **Composer 2.1** - Ange disposition 2.1.x som standard i PHP 8.x-bilder.
- ![ny ikon](../../assets/new.svg) **Förbättringar av PHP-bilder**—

   - PHP 8.1-bilder har lagts till
   - Uppgraderad xDebug version 3.1.2
   - Uppgraderad xmlrpc 1.0.0RC3

- ![korrigeringsikon](../../assets/fix.svg) **Förbättringar i Elasticsearch &amp; OpenSearch** - Förbättringar i Elasticsearch- och OpenSearch-dockningsfiler - Elasticsearch 5.2-bilden har tagits bort.
- ![korrigeringsikon](../../assets/fix.svg) **Natriumtillägg** - Aktiverade tillägget `sodium` som standard i alla PHP-bilder.
- ![korrigeringsikon](../../assets/fix.svg) **Cachevolymen för Composer** - En fast sökväg för cacheminnesvolymen för Composer har cachelagrade Composer-paket.
- ![korrigeringsikon](../../assets/fix.svg) **Minnesbegränsning i nginx** - Begränsningen för minne i NGINX-bilden har åtgärdats.

## v1.3.0

Releasedatum: 25 oktober 2021

- ![korrigeringsikon](../../assets/fix.svg) **Förbättra arbetsflödet i utvecklarläget** - Tidigare behövde du ange läge i bygg- och distributionsstegen. Nu avgör alternativet `--mode` i steget `build` läget i det senare `deploy` steget. Du behöver inte längre ställa in läget efter distributionen. Se [Utvecklarläge](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/).<!-- ACMP-1086 -->
- ![korrigeringsikon](../../assets/fix.svg) **Förbättringar för skrivskyddat filsystem**—<!-- ACMP-1106 -->
   - Åtgärda problem med att starta en PHP-behållare för e-postkonfiguration.
   - Kan använda miljövariabler i INI-filer.
   - Kontrollera att PHP-startpunkterna inte behöver skrivbehörighet.
- ![korrigeringsikon](../../assets/fix.svg) **Uppdatera nod** - Uppdatera den paketerade nodversionen. När du installerar nod i PHP-CLI-bilder används nu den aktuella LTS-versionen.<!-- ACMP-1539 -->
- ![korrigeringsikon](../../assets/fix.svg) **Uppdatera Symfony** - Symfony-konfigurationsberoendena har uppdaterats så att de är kompatibla med Adobe Commerce 2.4.4.<!-- ACMP-1533 -->

## v1.2.4

Releasedatum: 29 juli 2021

- ![ny ikon](../../assets/new.svg) **Ny `Zookeeper` container** - lade till en [Zookeeper-behållare](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#zookeeper-container) för att hantera låsproviderkonfiguration för projekt som inte har distribuerats till Adobe Commerce i molninfrastrukturen.<!--MCLOUD-8000-->

- ![ny ikon](../../assets/new.svg) **Stöd har lagts till för Composer 2.0.** - Kompositionsversion 2.0 har lagts till i Composer-konfigurationsfilen för att ge stöd för uppgraderingar från Composer 1.0 som närmar sig slutet av livscykeln.<!--MCLOUD-8003-->

## v1.2.3

Releasedatum: 14 juni 2021

- ![ny ikon](../../assets/new.svg) **Lade till PHP 8.0** - Uppdaterade PHP till version 8.0, så att du kan utnyttja alla nya funktioner och optimeringar som PHP 8.0 inkluderar.<!--MCLOUD-7941-->
- ![ny ikon](../../assets/new.svg) **Uppdaterad till lack 6.6 och Elasticsearch 7.1.2** - Följande länkar innehåller versionsinformation om [Varnish Cache 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) och Elasticsearch 7.11.2.<!--MCLOUD-7921-->
- ![ny ikon](../../assets/new.svg) **Tillägget `ioncube` för PHP 7.4-bilden** har lagts till på nytt i PHP 7.4-bilden efter att först ha undantagits från uppgraderingen av PHP 7.3 till PHP 7.4. `ioncube` *[Skickat av maskskr](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![ny ikon](../../assets/new.svg) **Lagt till ett filsynkroniseringsalternativ:`manual-native`** - Filsynkroniseringsalternativet `manual-native` ger manuell kontroll över synkroniseringen, vilket ger bästa prestanda i macOS- och Windows-miljöer. Läs om hur du använder alternativet `manual-native` i [utvecklarläget](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) och [Synkroniserar data i en Docker-utvecklingsmiljö](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/#file-synchronization-options).<!--MCLOUD-7977-->
- ![ny ikon](../../assets/new.svg) **Tog bort volymborttagningar från `up` och `down` kommandon** - alternativet `--volume` togs bort från kommandona `bin/magento-docker up` och `bin/magento-docker down` och ersattes av det nya kommandot `bin/magento-docker init` med en varning om dataförlust. Den här ändringen förhindrar dataförlust av misstag. *[Skickat av joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![korrigeringsikon](../../assets/fix.svg) **Uppdaterat `CN`-värde för det genererade certifikatet** - Det hårdkodade `CN`-värdet har tagits bort från Docker-filen. Det här värdet skapade ett certifikatfel (`NET::ERR_CERT_INVALID`) som gjorde att alternativet `--host` för kommandot `ece-docker build:compose` ignorerades.<!--MCLOUD-7934-->

## v1.2.2

Releasedatum: 20 april 2021

- ![ny ikon](../../assets/new.svg) **Uppdaterad `host.docker.internal` för att vara plattformsoberoende** - Nu kan du skapa samma Docker Compose-skript för Ubuntu, Windows och macOS. Xdebug på Ubuntu kräver inte längre en separat miljövariabel. [Korrigering har skickats av Igor Vitol](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![ny ikon](../../assets/new.svg) **Uppdaterad init-docker.sh** - `mounts`-objektet har lagts till i miljövariabeln `MAGENTO_CLOUD_APPLICATION`. [Korrigering har skickats av Chiranjeevi](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![ny ikon](../../assets/new.svg) **Uppdaterad init-docker.sh** - `init-docker.sh`-skriptet har uppdaterats med versionerna PHP 7.4 och Cloud Docker 1.2.1. [Korrigering har skickats av Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![ny ikon](../../assets/new.svg) **Natrium är aktiverat som standard** - PHP-tillägget `sodium` är aktiverat som standard i PHP Docker-bilder.<!--MCLOUD-7548-->
- ![ny ikon](../../assets/new.svg) **`custom-registry`option** - lade till ett `--custom-registry`-alternativ i `php ./vendor/bin/ece-docker build:compose`-kommandot för att använda ditt eget bildregister.<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![ny ikon](../../assets/new.svg) **Tidigare Elasticsearch-versioner har tagits bort** - Elasticsearch-versionerna 1.7 och 2.4 har tagits bort från Elasticsearch-bilder.<!--MCLOUD-7504-->
- ![ny ikon](../../assets/new.svg) **Autogenererande NGINX-certifikat** - Befintliga certifikat har tagits bort från NGINX-bilden. NGINX-certifikaten genereras nu automatiskt med varje ny distribution för förbättrad säkerhet.<!--MCLOUD-7396-->
- ![korrigeringsikon](../../assets/fix.svg) **Aktiverad`opcache.validate_timestamps`** - Aktiverade PHP-inställningen `opcache.validate_timestamps` som standard i utvecklarläge. Om du aktiverar den här inställningen åtgärdas problemet där ändringar i filsystemet inte kändes igen i Docker.<!--MCLOUD-7466-->
- ![korrigeringsikon](../../assets/fix.svg) **Korrigerad`build:custom:compose`** - Korrigerade kommandot `build:custom:compose` så att ett fel uppstod när filer inte kunde skrivas över under byggprocessen. Ett fel förhindrar situationer där `docker-compose up` kan använda fel filer.<!--MCLOUD-7457-->
- ![korrigeringsikon](../../assets/fix.svg) **Åtgärdat `--sync_engine="native"` alternativ** - Korrigerade problemet där `--sync_engine="native"` inte skulle skapa några poster för lokala mappar i `docker.composer.yml`-filen i produktionsläget (`--mode="production"`).<!--MCLOUD-7254-->
- ![korrigeringsikon](../../assets/fix.svg) **Verifieringsfel för tjänstversion** - Tjänstversioner för [!DNL RabbitMQ], Elasticsearch och andra tjänster har lagts till i egenskapen `type` i variabeln `MAGENTO_CLOUD_RELATIONSHIP`. Genom att lägga till de här versionerna i variabeln `relationships` åtgärdas de valideringsfel som uppstod under distributionsfasen.<!--MCLOUD-7572-->

## v1.2.1

Releasedatum: 21 december 2020

- ![ny ikon](../../assets/new.svg) **NGINX-kommandoalternativ** - Lagt till alternativ för byggkommandon för att ändra antalet NGINX `worker_processes` och NGINX `worker_connections` för TLS och Web services. Parametern `worker_process` behåller möjligheten att ange värdet till `auto`. Exempel: <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![ny ikon](../../assets/new.svg) **TLS-kommandoalternativ** - Lagt till build-kommandoalternativ för att skapa en konfiguration utan TLS-tjänsten. Exempel: <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![ny ikon](../../assets/new.svg) **NGINX-minnesförbrukning** - Minskar mängden minne som används av NGINX-processen för TLS och Web services.<!--MCLOUD-7259-->

- ![ny ikon](../../assets/new.svg) **Blackfire** - Inaktiverat Blackfire PHP-tillägg som standard i molnavbildningen.

- ![korrigeringsikon](../../assets/fix.svg) **PHP-FPM-behållare** - PHP-FPM-behållarens hälsokontroll har korrigerats genom att `WEB_PORT` ändrades från `80` till `8080`.<!--MCLOUD-7232-->

- ![korrigeringsikon](../../assets/fix.svg) **Ogiltig volymnamngivning** - Korrigerade ett fel med ogiltig volymnamngivning i utvecklarläge.<!--MCLOUD-7442-->

- ![korrigeringsikon](../../assets/fix.svg) **NGINX uppströms port** - Docker NGINX 1.19-bilden har uppdaterats så att port 8080 används för att undvika en oändlig slinga. [Korrigering har skickats av Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## v1.2.0

Releasedatum: 9 november 2020

- ![ny ikon](../../assets/new.svg) **Uppdateringar för behållare—**

   - ![ny ikon](../../assets/new.svg) **PHP-FPM-behållare** - Stöd för gnupg-PHP-tillägget har lagts till. [Korrigering har skickats av G Arvind från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![korrigeringsikon](../../assets/fix.svg) **Databasbehållare** - Åtgärdade hälsokontrollen för databasbehållaren genom att lägga till det obligatoriska databaslösenordet till hälsokontrollskommandot.<!--MCLOUD-7122-->

   - ![ny ikon](../../assets/new.svg) **Elasticsearch container**

      - Stöd för Elasticsearch 7.9 har lagts till för kompatibilitet med kommande Adobe Commerce-versioner.<!--MCLOUD-7190-->

      - **Elasticsearch plugin-konfiguration** - Stöd har lagts till för att använda konfigurationsinformationen för Elasticsearch-plugin-programmet från filen `services.yaml` för att generera filen `docker-compose.yaml` för en molndockningsmiljö för Commerce. Se [Elasticsearch-plugin-program](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Stöd för Elasticsearch-plugin** - Stöd har lagts till för följande Elasticsearch-plugin-program: `analysis-icu`, `analysis-phonetic`, `analysis-stempel` och `analysis-nori`. Plugin-programmen `analysis-icu` och `analysis-phonetic` installeras som standard. Du kan lägga till eller ta bort plugin-programmen `analysis-stempel` och `analysis-nori` efter behov.<!--MCLOUD-2789-->

   - ![ny ikon](../../assets/new.svg) **CLI-behållare**

      - **Kör kommandon i PHP-behållare för Docker** - Nu kan du använda Cloud Docker CLI för att köra kommandon i PHP-behållare i Docker-miljön utan att behöva installera PHP på värden. Följande kommando skapar till exempel konfigurationen: `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`. Se [Cloud Docker CLI](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli). [Korrigering har skickats av G Arvind från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - OpenSSH-klienten har lagts till i PHP CLI-behållare. Nu kan du använda Ssh-agent-vidarebefordran för Composer om filen `composer.json` innehåller privata Git-databaser som kräver att en SSH-klient använder Composer-kommandon.<!--MCLOUD-6008-->

   - ![korrigeringsikon](../../assets/fix.svg) **TLS-behållare** - Nu baseras [TLS-behållaren](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container) på `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` Docker-bilden i stället för CentOS-bilden. Den här ändringen åtgärdar problem som orsakade fel när HTTPS-begäranden skickas mellan behållare i Cloud Docker-miljön.<!--MCLOUD-6469-->

   - ![ny ikon](../../assets/new.svg) **Testbehållare** - En testbehållare för programtestning har lagts till och alternativet `--with-test` har lagts till i Docker `build:compose`-kommandot för att skapa behållaren endast vid testning i Docker-miljön. Se [programtestning](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/).<!--MCLOUD-6394-->

   - ![ny ikon](../../assets/new.svg) **FPM-XDEBUG-behållare**

      - ![ny ikon](../../assets/new.svg) **Konfigurera Xdebug i Linux** - alternativet `--set-docker-host` har lagts till i kommandot `ece-docker build:compose` för att konfigurera värdet `host.docker.internal` i Xdebug-behållaren. Det här alternativet krävs för att använda Xdebug på Linux-system. Se [Konfigurera Xdebug för Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).<!--MCLOUD-6430-->

      - ![korrigeringsikonen](../../assets/fix.svg) Åtgärdade Xdebug-variabelkonfigurationen för Docker ENTRYPOINT för att åtgärda `uninitialized "with_xdebug" variable` fel i loggarna. [Korrigering har skickats in av Florent Olivaud](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![ny ikon](../../assets/new.svg) **Konfigurationsändringar för Docker**

   - **MailHog-konfiguration** - Nu kan du använda följande `ece-docker build:compose` kommandoalternativ för att inaktivera MailHog och ange portar: `--no-mailhog`, `--mailhog-http-port` och `--mailhog-smtp-port`. Se [Konfigurera e-post](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->

   - För Cloud Docker för Commerce 1.2.0 och senare innehåller Adobe nu Docker-bilder för varje patch-version, och Docker-konfigurationsgeneratorn skapar Docker-konfigurationen med en angiven patch-version i stället för att använda den senaste. Tidigare byggde Docker-konfigurationsgeneratorn konfigurationen med den senaste korrigeringsversionen som kunde bryta Cloud Docker för Commerce-miljöer som byggts med en tidigare version.<!--MCLOUD-7093-->

   - **Ange egna bilder och versioner i anpassad Cloud Docker-konfiguration** - Uppdaterade kommandot `build:custom:compose` med alternativ för att ange anpassade bilder och versioner när en anpassad Docker-dispositionskonfigurationsfil genererades (`docker-compose.yaml`). Se [Skapa en anpassad Docker Compose-konfiguration](https://developer.adobe.com/commerce/cloud-tools/docker/configure/custom-docker-compose/). <!--MCLOUD-7089-->

   - Docker-värdkonfigurationen har uppdaterats så att port 443 exponeras för åtkomst till Adobe Commerce (`https://magento2.docker`) från alla CLI-behållare. Du kan ändra standardporten genom att lägga till alternativet `--tls-port` när du genererar Docker-konfigurationsfilen.<!--MCLOUD-6806-->

- ![korrigeringsikon](../../assets/fix.svg) Ett problem som gjorde att Cloud Docker för Commerce-bygget misslyckades om filen `app/etc/env.php` finns har åtgärdats.<!--MCLOUD-6732-->

- ![korrigeringsikon](../../assets/fix.svg) Uppdaterade byggkonfigurationen för att ersätta namngivna volymer med vanliga volymer för att förhindra problem vid distribution av Cloud Docker för Commerce i Linux eller Windows Subsystem för Linux (WSL2).<!--MCLOUD-6732-->

- ![korrigeringsikon](../../assets/fix.svg) Molndockningen för Commerce-funktionstester har uppdaterats med stöd för Composer 2.0.<!--MCLOUD-7183-->

## v1.1.2

Releasedatum: 9 september 2020

- ![ny ikon](../../assets/new.svg) Stöd för Elasticsearch 7.7 har lagts till<!--MCLOUD-6219-->

## v1.1.1

Releasedatum: 5 augusti 2020

- ![korrigeringsikon](../../assets/fix.svg) **Uppdaterad e-postkonfiguration** - Uppdaterade standardkonfigurationen för Cloud Docker för Commerce så att den stöder MailHog-tjänsten i stället för att använda SendMail. Se [Konfigurera e-post](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-5624-->

- ![korrigeringsikon](../../assets/fix.svg) PS-biblioteket återställdes till molndockningsmiljökonfigurationen för att korrigera `ps:  command not found` fel.<!--MCLOUD-6621-->

- ![korrigeringsikon](../../assets/fix.svg) Uppdaterade standardkonfigurationen för Cloud Docker för Commerce för att ta bort automatisk montering av databasingångspunkten och MariaDB-volymer för att åtgärda `Cannot create container for service db` -fel som kan uppstå när du startar Cloud Docker-miljön.

  Nu kan du konfigurera Cloud Docker-miljön så att den monterar databaskatalogerna genom att lägga till följande alternativ till kommandot `ece-docker build:compose`: `--with-entry-point` och `with-mariadb-conf`. Se [Alternativ för tjänstkonfiguration](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-6424-->

- ![ny ikon](../../assets/new.svg) **CLI-kommandouppdateringar**

| Åtgärd | Kommando |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Lägg till en startpunkt i databasbehållaren för att återställa databasen från en säkerhetskopia | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| Lägg till en MariaDB-konfigurationsvolym | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

Releasedatum: 25 juni 2020

- ![ny ikon](../../assets/new.svg) **Stöd har lagts till för prestandalösningen för den delade databasen** - Nu kan du konfigurera och distribuera en butik med prestandalösningen för den delade databasen i Cloud Docker-miljön.<!--MCLOUD-3740-->

- ![ny ikon](../../assets/new.svg) **Stöd för Adobe Commerce- och Magento Open Source-distribution** - Nu kan du använda Cloud Docker för Commerce för att distribuera en lokal utvecklingsmiljö för projekt som inte ligger på Adobe Commerce i molninfrastrukturen.<!--MCLOUD-5667-->

- ![ny ikon](../../assets/new.svg) **Stöd för Blackfire.io** - Stöd har lagts till för att använda tillägget [Blackfire.io](https://developer.adobe.com/commerce/cloud-tools/docker/test/blackfire/) för automatisk prestandatestning. [Korrigering från Adarsh Manickam från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![ny ikon](../../assets/new.svg) **Uppdateringar för behållare**

   - **Varnish** - Nu är Varnish standardcache när du distribuerar Adobe Commerce i en Cloud Docker-miljö med en version av molnprogrammallen som stöds. Se [Slutlig behållare](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container).<!--MCLOUD-2634-->

   - `--no-varnish`-alternativet för att hoppa över installationen av tjänsten Varnish lades till när du genererade konfigurationsfilen för Cloud Docker.<!--MCLOUD-2634-->

   - ![ny ikon](../../assets/new.svg) **Databas**

      - Stöd för MySQL-databasen har lagts till. Nu kan du konfigurera molndockningsmiljön med MariaDB eller MySQL. Se [Alternativ för tjänstkonfiguration](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-5691-->

      - Lagt till möjlighet att ange inställningar för ökning och förskjutning för databasreplikering när du genererar Docker-dispositionsfilen. Se [Tjänstbehållare](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers).<!--MCLOUD-5735-->

   - ![ny ikon](../../assets/new.svg) **PHP-FPM**

      - Stöd för PHP 7.4 har lagts till. [Korrigering från Mohanela Murugan från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198--> har skickats in

      - Lagt till möjlighet att kopiera en `php.ini`-fil i rotprojektkatalogen till Cloud Docker-miljön och tillämpa anpassade PHP-inställningar på PHP-FPM- och CLI-behållarna. Se [Anpassa PHP-inställningar](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#customize-php-settings). [Korrigering har skickats av Mathew Beane från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - En hälsokontroll för behållare har lagts till. [Korrigering har skickats av Visual Sampath från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![korrigeringsikon](../../assets/fix.svg) **Node.js** - Uppdaterade standardversionen av Node.js från version 8 till version 10 för att förbättra säkerheten. Node.js version 8 är inaktuell och inte längre uppdaterad med felkorrigeringar eller säkerhetspatchar. [Korrigering har skickats av Mohan Elamurgan från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![ny ikon](../../assets/new.svg) **Elasticsearch**

      - Stöd för Elasticsearch 6.8, 7.2, 7.5 och 7.6.<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860--> har lagts till

      - Lagt till möjlighet att anpassa [Elasticsearch-behållarkonfigurationen](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) när du genererar Docker compose-konfigurationsfilen.<!--MCLOUD-3059-->

      - Alternativet `--no-es` har lagts till i tjänstkonfigurationsalternativen för generering av Docker Compose-konfigurationsfilen. Använd det här alternativet om du vill hoppa över installationen av Elasticsearch-behållaren och använda MySQL-sökningen i stället. Det här alternativet stöds endast för Adobe Commerce version 2.3.5 och tidigare.<!--MCLOUD-3766-->

   - ![ny ikon](../../assets/new.svg) **FPM-XDEBUG-behållare** - Ett tjänstkonfigurationsalternativ har lagts till för att installera och konfigurera Xdebug för felsökning av PHP i molndockningsmiljön. Se [Konfigurera Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).<!--MCLOUD-4098-->

- ![ny ikon](../../assets/new.svg) **Konfigurationsändringar för Docker**

   - Hälsokontroller har lagts till för tjänstbehållarna PHP-FPM, Redis, Elasticsearch och MySQL Docker.<!--MCLOUD-3335 and MCLOUD-5856-->

   - Ändrade standardfilsynkroniseringsläget till `native` i utvecklarläge.<!--MCLOUD-3890 -->

   - Versionsinformation har lagts till i den generiska Docker-tjänstbehållarbilden när filen `docker-compose.yml` genereras.<!--MCLOUD-3878-->

   - Förbättrade möjligheter att hantera stora svar från PHP-FPM-behållaren uppströms genom att öka värdet `fastcgi_buffers` för Nginx-servern.<!--MCLOUD-5980-->

   - Förbättrade prestanda för synkronisering av mutagena filer genom att lägga till en andra synkroniseringssession för att synkronisera filer i katalogen `vendor`. Den här ändringen förhindrar att mutagen fastnar under filsynkroniseringsprocessen. [Korrigering har skickats av Mathew Beane från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![ny ikon](../../assets/new.svg) **CLI-kommandouppdateringar**

| Åtgärd | Kommando |
| -------- | --------------- |
| Rensa Redis-cache | `bin/magento-docker flush-redis` |
| Rensa lack-cache | `bin/magento-docker flush-varnish` |
| Hoppa över standardinstallation av lack | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Anpassa Elasticsearch-alternativ](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Ta bort Elasticsearch-konfiguration](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| Konfigurera DB-behållare med MySQL version 5.6 eller 5.7 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| Ange anpassad bas-URL | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Lägg till behållare för Xdebug-konfiguration](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![korrigeringsikon](../../assets/fix.svg) Konfigurationen av synkronisering av mutagenfiler har korrigerats för att förhindra att mutagen skapar inaktuella sessioner. [Korrigering har skickats av Mathew Beane från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![korrigeringsikon](../../assets/fix.svg) Ett konfigurationsproblem som orsakade syntaxfel i dispositionsloggen för Docker när PHP-FPM-behållaren startades har åtgärdats. [Korrigering skickad av Mathew Beane från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![korrigeringsikon](../../assets/fix.svg) Åtgärdade volymkonfliktsfel som ibland uppstod när flera Docker-miljöer användes. [Korrigering har skickats av G Arvind från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/168).

- ![korrigeringsikonen](../../assets/fix.svg) Korrigerade ett fel som gjorde att kommandot `ece-docker build:compose` misslyckades om konfigurationen innehöll Blackfire.io. [Korrigering har skickats av G Arvind från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![korrigeringsikon](../../assets/fix.svg) PHP CLI-bildkonfigurationen har uppdaterats för att förhindra fel av typen slut på minne som inträffar när flera paket installeras med Cloud Docker för Commerce. [Korrigering har skickats av Mohan Elamurgan från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/197).*<!--MCLOUD-5818-->

- ![korrigeringsikon](../../assets/fix.svg) Stöd har lagts till för flera MySQL-användare i Cloud Docker-miljön. I tidigare versioner misslyckades åtgärden `build:compose` om filen `magento.app.yaml` angav flera databasanvändare. [Korrigering har skickats av G Arvind från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![korrigeringsikonen](../../assets/fix.svg) Borttagen `rsyslog` från molndockningsprogrammet för Commerce PHP-behållare för att lösa kompatibilitetsproblem som orsakade varningsmeddelanden under distributionen. Cloud Docker använder inte rsyslog-verktyget.<!--MCLOUD-6173-->

## v1.0.0

Releasedatum: 5 feb 2020

- ![ny ikon](../../assets/new.svg) **Skapade ett separat paket för att leverera`Cloud Docker for Commerce`** - Flyttade källkoden för att leverera Cloud Docker för Commerce från `ece-tools`-databasen till den [nya `magento-cloud-docker`-databasen](https://github.com/magento/magento-cloud-docker) för att upprätthålla kodkvaliteten och tillhandahålla oberoende releaser. Det nya paketet är beroende av ECE-Tools v2002.1.0 och senare.

  När du uppdaterar hjälpverktygen uppdaterar du även paketet `magento/magento-cloud-docker` till version 1.0.0. Om du använde Cloud Docker för Commerce med en tidigare `ece-tools` version (2002.0.x) granskar du inkompatibiliteten [bakåt](backward-incompatible-changes.md) och uppdaterar projektet som skript, kommandon och processer efter behov.

- ![ny ikon](../../assets/new.svg) **Versionshantering har lagts till i Docker-bilderna** - Du måste nu uppdatera paketet `magento/magento-cloud-docker` för att få de uppdaterade bilderna.<!--MAGECLOUD-4737-->

- ![ny ikon](../../assets/new.svg) **Uppdateringar för behållare**—

   - ![ny ikon](../../assets/new.svg) **PHP-FPM-behållare**—

      - ![ny ikon](../../assets/new.svg) **Stöd för Node.js har lagts till** - PHP-FPM-bilden har uppdaterats för att stödja nod-, npm- och grunt-cli-funktionerna i PHP-behållaren.<!--MAGECLOUD-3953-->

      - ![ny ikon](../../assets/new.svg) **Stöd har lagts till för [jonCube](https://www.ioncube.com/)** - Docker-standardkonfigurationen har uppdaterats så att den stöder jonCube i den lokala Docker-utvecklingsmiljön.<!--MAGECLOUD-4354-->

   - ![ny ikon](../../assets/new.svg) **Webbbehållare**—

      - ![ny ikon](../../assets/new.svg) **Anpassa NGINX-konfiguration** - lade till möjligheten att montera en anpassad `nginx.conf`-fil i Cloud Docker för Commerce-miljön. Se [Webbbehållare](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#web-container).<!--MAGECLOUD-4204-->

      - ![ny ikon](../../assets/new.svg) **Autogenererade NGINX-certifikat** - Docker-konfigurationsfilen innehåller nu konfigurationen för automatisk generering av NGINX-certifikat för webbbehållaren.<!--MAGECLOUD-4258-->

   - ![ny ikon](../../assets/new.svg) **Ny Selenium-behållare** - En [Selenium-behållare](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#selenium-container) har lagts till som stöd för Adobe Commerce-programtestning med Magento Functional Testing Framework (MFTF).<!--MAGECLOUD-4040-->

   - ![ny ikon](../../assets/new.svg) **[!DNL RabbitMQ]versionstöd** - Behållarkonfigurationen för [!DNL RabbitMQ] har uppdaterats så att den stöder [!DNL RabbitMQ] version 3.8.<!--MAGECLOUD-4674-->

   - ![korrigeringsikon](../../assets/fix.svg) **Beständig databasbehållare** - `magento-db: /var/lib/mysql`-databasvolymen kvarstår nu när du har stoppat och tagit bort Docker-konfigurationen och återställer när du startar om Docker-konfigurationen. Nu måste du ta bort databasvolymen manuellt. Se [Databasbehållare].<!--MAGECLOUD-3978-->

   - ![ny ikon](../../assets/new.svg) **TLS-behållare**—

      - ![ny ikon](../../assets/new.svg) **Behållarbasbilden har uppdaterats så att den officiella bilden används** - [Molnets TLS-behållarbild](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container) baseras nu på den officiella `debian:jessie` Docker-bilden.—<!--MAGECLOUD-4163-->

      - ![ny ikon](../../assets/new.svg) **Stöd har lagts till för [Pound TLS Termination Proxy]** - [Pound configuration file](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/) innehåller följande ENV-variabler för att anpassa Docker-konfigurationen för TLS-behållaren:

         - **`TimeOut`** - Anger tidsgränsen för TTFB (Time to First Byte). Standardvärdet är 300 sekunder.

         - **`RewriteLocation`** - Avgör om Pound-proxyn skriver om platsen till begärande-URL som standard. Standardvärdet är `0` för att förhindra att omskrivningen avbryter omdirigeringar till externa webbplatser som en extern SSO-plats. [Korrigering har skickats av Sorin Sugar](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![ny ikon](../../assets/new.svg) Ökade timeoutvärdet i TLS-behållarkonfigurationen från 15 till 300 sekunder. [Korrigering skickad av Mathew Beane från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![ny ikon](../../assets/new.svg) **Varnish-behållare**—

      - ![ny ikon](../../assets/new.svg) **Behållarbasbilden har uppdaterats så att den officiella bilden används** - [Cloud lack-behållaren](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container) baseras nu på den officiella `centos` Docker-bilden.<!--MAGECLOUD-4163-->

      - ![ny ikon](../../assets/new.svg) **Förbättrad standardtimeoutkonfiguration**-Added `.first_byte_timeout` och `.between_bytes_timeout` konfiguration till behållaren Varnish. Båda timeout-värdena är som standard `300s` (5 minuter). [Korrigering skickad av Mathew Beane från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![korrigeringsikon](../../assets/fix.svg) **Hoppa över lack under Xdebug-sessioner** - Uppdaterade konfigurationen för behållaren i engelska så att den returnerar `pass` på begäranden som tas emot när Xdebug är aktiverat. I tidigare versioner gick det inte att använda Xdebug om Docker-miljön innehöll lack. [Korrigering har skickats av Mathew Beane från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![ny ikon](../../assets/new.svg) **Konfigurationsändringar för Docker**—

   - ![ny ikon](../../assets/new.svg) **Hantera monteringar och volymer för ditt projekt** - Lagt till möjlighet att hantera monteringar och volymer när du startar en Docker-miljö för lokal utveckling. Se [Dela projektdata].<!--MAGECLOUD-3248-->

   - ![ny ikon](../../assets/new.svg) **Stöd för nätverksbryggläge** - Stöd har lagts till för nätverksbryggläge för att aktivera anslutningar mellan Docker-behållare i det lokala nätverket.<!--MAGECLOUD-4165-->

   - ![ny ikon](../../assets/new.svg) **Cron-behållaren inaktiverad som standard** - För att förbättra prestanda är Cron-behållaren inte längre konfigurerad som standard när du skapar Docker-miljön. Du kan använda alternativet `--with-cron` i Docker-byggkommandot för att lägga till en Cron-behållare i miljön. Se [Hantera cron-jobb](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/).<!--MAGECLOUD-5181-->

   - ![ny ikon](../../assets/new.svg) **Stoppa synkronisering av stora säkerhetskopior** - Lagt till databasdumpar och arkivfiler - ZIP, SQL, GZ och BZ2 - i exkluderingslistan i `dist/docker-sync.yml` - och `dist/mutagen.sh`-filerna. Synkronisering av stora filer (>1 GB) kan orsaka en viss inaktivitet och säkerhetskopieringsfiler kräver normalt inte synkronisering eftersom du kan återskapa dem.<!--MAGECLOUD-3979-->

- ![ny ikon](../../assets/new.svg) **Kommandot ändras**—

   - ![korrigeringsikonen](../../assets/fix.svg) har ändrat namn på filen `./bin/docker` till `./bin/magento-docker` för att åtgärda ett problem som gjorde att vissa Docker-miljöer bröts eftersom filen `./bin/docker` skriver över befintliga binära Docker-filer. Det här är en [bakåtkompatibel ändring](backward-incompatible-changes.md) som kräver uppdateringar av skript och kommandon.<!-- MAGECLOUD-4038 -->

   - ![ny ikon](../../assets/new.svg) **Lagt till ett tjänstkonfigurationsalternativ för att visa databasporten för värden** - Använd alternativet `--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` för att visa databasporten för värden när `docker-compose.yml`-filen skapas: `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![ny ikon](../../assets/new.svg) **Nytt kommando för efterdistribution** - Tidigare kördes de efterdistribuerade kopplingar som definierats i filen `.magento.app.yaml` automatiskt efter att du distribuerat Adobe Commerce till en Cloud Docker-behållare med kommandot `cloud-deploy`. Nu måste du skapa ett separat `cloud-post-deploy`-kommando för att köra hookarna efter distributionen. Se de uppdaterade startinstruktionerna för [utvecklare](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/) och [produktion](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/production-mode/).<!--MAGECLOUD-3996-->

   - ![ny ikon](../../assets/new.svg) lade till alternativet `--rm` i `./bin/magento-docker`-kommandon för behållarna för skapande och distribution. Detta tar bort behållaren när aktiviteten har slutförts.<!--MAGECLOUD-4205-->

   - ![ny ikon](../../assets/new.svg) **Uppdateringar av `build:compose` command**—

      - ![ny ikon](../../assets/new.svg) lade till alternativet `--sync-engine="native"` i kommandot `docker-build` för att inaktivera filsynkronisering när du genererar konfigurationsfilen för Docker Compose i utvecklarläge. Använd det här alternativet när du utvecklar på Linux-system, som inte kräver filsynkronisering för lokal Docker-utveckling. Se [Synkronisera data i Docker-miljön](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![ny ikon](../../assets/new.svg) Ändrade standardinställningen för filsynkronisering från `docker-sync` till `native`. [Korrigering har skickats av Mathew Beane från Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![ny ikon](../../assets/new.svg) **Valideringsförbättringar**—

   - ![ny ikon](../../assets/new.svg) Lagt till validering i distributionsprocessen för lokala Docker-utvecklingsmiljöer för att verifiera att molnmiljökonfigurationen innehåller den krypteringsnyckel som krävs för att dekryptera databasen. Du får nu ett felmeddelande i loggen om miljökonfigurationen inte anger något värde för krypteringsnyckeln.<!--MAGECLOUD-4423-->

   - ![ny ikon](../../assets/new.svg) Lagt till en hälsokontroll för behållare i Elasticsearch-tjänsten för att säkerställa att tjänsten är klar innan du fortsätter med bygg- och distributionsbearbetningen. Om hälsokontrollen returnerar ett fel startar behållaren om automatiskt.<!--MAGECLOUD-4456-->
