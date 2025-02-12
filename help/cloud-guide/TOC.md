---
user-guide-title: Användarhandbok om Commerce i molnet
user-guide-description: Lär dig hur du hanterar Adobe Commerce-programmet i molninfrastrukturen.
product: magento
feature: Cloud
source-git-commit: 4e107bb8696c3c7cb76a0a702624aeb754648f70
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 4%

---


# Commerce on Cloud Infrastructure {#user-guide}

+ [Commerce](overview.md)
+ Arkitektur {#architecture}
   + [Molninfrastruktur](architecture/cloud-architecture.md)
   + [Säkerhet](architecture/security.md)
   + [Teknikstack](architecture/tech-stack.md)
   + [Startarkitektur](architecture/starter-architecture.md)
   + [Startarbetsflöde](architecture/starter-develop-deploy-workflow.md)
   + [Pro-arkitektur](architecture/pro-architecture.md)
   + [Arbetsflöde för Pro](architecture/pro-develop-deploy-workflow.md)
   + [Skalbar arkitektur](architecture/scaled-architecture.md)
   + [Automatisk skalning](architecture/autoscaling.md)
+ [Kom igång](https://experienceleague.adobe.com/docs/commerce-on-cloud/start/overview.html)
+ Versionsinformation {#release-notes}
   + [Cloud Tools Suite](release-notes/cloud-tools-suite.md)
   + [ECE-verktygspaket](release-notes/ece-tools-package.md)
   + [Molnkorrigeringar](release-notes/cloud-patches.md)
   + [Cloud Docker-paket](release-notes/cloud-docker.md)
   + [Molnkomponenter](release-notes/cloud-components.md)
   + [Molnpaket](release-notes/cloud-packages.md)
   + [Bakåtkompatibla ändringar](release-notes/backward-incompatible-changes.md)
   + [Arkiv med veringsanteckningar](release-notes/cloud-release-archive.md)
+ Molnprojekt {#project}
   + [Projektöversikt](project/overview.md)
   + [Projektstruktur](project/file-structure.md)
   + [Användaråtkomst](project/user-access.md)
   + [Multifaktorautentisering](project/multi-factor-authentication.md)
   + [Aktivitetsström](project/activity-stream.md)
   + [Utgående e-postmeddelanden](project/outgoing-emails.md)
   + [SkickaGrid-e-posttjänst](project/sendgrid.md)
   + [Konsolgrenhantering](project/console-branches.md)
   + [Regionala IP-adresser](project/regional-ip-addresses.md)
+ Utvecklarverktyg {#dev-tools}
   + [Ökning](dev-tools/overview.md)
   + Cloud CLI {#cloud-cli}
      + [CLI - översikt](dev-tools/cloud-cli-overview.md)
      + [CLI-referens](dev-tools/cloud-cli-reference.md)
   + [Cloud Docker](dev-tools/cloud-docker.md)
   + ECE-verktyg {#ece-tools}
      + [Paketöversikt](dev-tools/package-overview.md)
      + [Engångsuppgradering för att använda ECE-verktyg](dev-tools/install-package.md)
      + [Uppdateringspaket för ECE-verktyg](dev-tools/update-package.md)
      + [CLI-referens](dev-tools/ece-tools-cli-reference.md)
      + [Felreferens](dev-tools/error-reference.md)
   + Integrationer {#integrations}
      + [Ökning](integrations/overview.md)
      + [Bitbucket](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [Hälsoaviseringar](integrations/health-notifications.md)
+ Utveckling {#develop}
   + [Ökning](development/overview.md)
   + [Autentiseringsnycklar](development/authentication-keys.md)
   + [Filialhantering i CLI](development/cli-branches.md)
   + [Säkra anslutningar](development/secure-connections.md)
   + Distribuera {#deploy}
      + [Distributionsprocess](deploy/process.md)
      + [Optimering](deploy/optimization.md)
      + [God praxis](deploy/best-practices.md)
      + [Scenariobaserad driftsättning](deploy/scenario-based.md)
      + [Driftsättning utan driftstopp](deploy/reduce-downtime.md)
      + [Statisk innehållsdistribution](deploy/static-content.md)
      + [Smarta guider](deploy/smart-wizards.md)
      + [Distribuera till mellanlagring och produktion](deploy/staging-production.md)
      + [Återställning efter komponentfel](deploy/recover-failed-deployment.md)
   + Testa {#test}
      + [Testvägledning](test/guidance.md)
      + [Loggar](test/log-locations.md)
      + [Xdebug](test/debug.md)
      + [Exempeldata](test/sample-data.md)
      + [Mellanlagring och produktion](test/staging-and-production.md)
      + [Andra mellanlagringsmiljön](test/second-staging.md)
   + [PrivateLink-tjänst](development/privatelink-service.md)
   + [Skyddsblock](development/protective-block.md)
   + [Återställningsmiljö](development/restore-environment.md)
   + Lagring {#storage}
      + [Hantera diskutrymme](storage/manage-disk-space.md)
      + [Profildatabasfrågor](storage/profile-database-queries.md)
      + [Säkerhetskopiera databasen](storage/database-dump.md)
      + [Hantering av säkerhetskopiering](storage/snapshots.md)
   + Uppgraderingar och korrigeringar {#upgrade}
      + [God praxis](development/best-practices.md)
      + [Uppgradera Commerce](development/commerce-version.md)
      + [Tillämpa patchar](development/apply-patches.md)
+ Konfiguration {#configure}
   + [Ökning](environment/overview.md)
   + Program {#app}
      + [Konfigurera programdistribution](application/configure-app-yaml.md)
      + [PHP-inställningar](application/php-settings.md)
      + Egenskaper {#properties}
         + [Programegenskaper](application/properties.md)
         + [Kroner](application/crons-property.md)
         + [Brandvägg (endast Starter)](application/firewall-property.md)
         + [Hooks](application/hooks-property.md)
         + [Variabel](application/variables-property.md)
         + [Webb](application/web-property.md)
         + [Arbetare](application/workers-property.md)
      + [Ange cache för statiska filer](application/set-cache.md)
   + Miljö {#env}
      + [Konfigurera miljödistribution](environment/configure-env-yaml.md)
      + [Variabla nivåer och alternativ](environment/variable-levels.md)
      + Åsidosätt variabler {#stage}
         + [Miljövariabler](environment/variables-intro.md)
         + [ADMIN](environment/variables-admin.md)
         + [Molnvariabler](environment/variables-cloud.md)
         + [Global](environment/variables-global.md)
         + [Bygge](environment/variables-build.md)
         + [Distribuera](environment/variables-deploy.md)
         + [Efter driftsättning](environment/variables-post-deploy.md)
      + Konfigurera meddelanden {#log}
         + [Meddelanden](environment/set-up-notifications.md)
         + [Logghanterare](environment/log-handlers.md)
   + Vägar {#routes}
      + [Konfigurera flöden](routes/routes-yaml.md)
      + [Cachning](routes/caching.md)
      + [Omdirigeringar](routes/redirects.md)
      + [SSI (Server-side includes)](routes/server-side-includes.md)
   + Tjänster {#service}
      + [Konfigurera tjänster](services/services-yaml.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [KaninMQ](services/rabbitmq.md)
      + [Redis](services/redis.md)
+ Snabba tjänster {#cdn}
   + [Ökning](cdn/fastly.md)
   + Snabbinstallation {#setup-fastly}
      + [Konfigurera snabbfunktioner](cdn/fastly-configuration.md)
      + [Anpassa cachekonfigurationen](cdn/fastly-custom-cache-configuration.md)
      + [Anpassa fel- och underhållssidor](cdn/fastly-custom-response.md)
   + [Brandvägg för webbaserade program](cdn/fastly-waf-service.md)
   + [Bildoptimering](cdn/fastly-image-optimization.md)
   + Anpassa med VCL {#custom-vcl-snippets}
      + [Kom igång](cdn/fastly-vcl-custom-snippets.md)
      + [Omdirigera begäranden till en CMS-serverdel](cdn/fastly-vcl-wordpress.md)
      + [Blockera skräppost](cdn/fastly-vcl-badreferer.md)
      + [IP TILLÅTELSELISTA](cdn/fastly-vcl-allowlist.md)
      + [IP BLOCKERINGSLISTA](cdn/fastly-vcl-blocking.md)
      + [Kringgå snabbcache](cdn/fastly-vcl-bypass-to-origin.md)
   + [Snabbt felsökning](cdn/fastly-troubleshooting.md)
+ Lagringsinställningar {#configure-store}
   + [Ökning](store/overview.md)
   + [God praxis](store/best-practices.md)
   + [Eget tema](store/custom-theme.md)
   + [Tillägg](store/extensions.md)
   + [B2B-modul](store/b2b-module.md)
   + [Flera platser](store/multiple-sites.md)
   + [Robotprogram för webbplatskarta och sökmotor](store/robots-sitemap.md)
   + [Betalningsmetoder för PayPal](store/paypal.md)
   + [Konfigurationshantering](store/store-settings.md)
+ Starta webbplatsen {#launch}
   + [Ökning](launch/overview.md)
   + [Öppna checklista](launch/checklist.md)
   + [Starta steg](launch/steps.md)
+ Övervaka plats {#monitor}
   + [Prestanda](monitor/performance.md)
   + New Relic-tjänst {#new-relic}
      + [New Relic - översikt](monitor/new-relic-service.md)
      + [Konto- och användarhantering](monitor/account-management.md)
      + Undersök prestanda {#investigate}
         + [Profiler, varningar och arbetsflöden](monitor/investigate-performance.md)
         + [Intag av data](monitor/ingest-data.md)
         + [Spåra distributioner](monitor/track-deployments.md)
      + [Logghantering](monitor/log-management.md)
