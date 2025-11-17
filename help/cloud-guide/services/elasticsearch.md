---
title: Konfigurera Elasticsearch-tjänst
description: Lär dig hur du aktiverar Elasticsearch-tjänsten för Adobe Commerce i molninfrastruktur.
feature: Cloud, Search, Services
exl-id: 238b9ed5-ce73-428f-9459-35de8573d5d8
source-git-commit: ef22e7b305c20148f4ee4b2c0e64e2114bf229b5
workflow-type: tm+mt
source-wordcount: '746'
ht-degree: 0%

---

# Konfigurera Elasticsearch-tjänst

[Elasticsearch](https://www.elastic.co) är en öppen källkodsprodukt som gör att du kan hämta data från alla källor, alla format och söka och visualisera dem i realtid.

{{elasticsearch-support}}

Information om Adobe Commerce version 2.4.4 och senare finns i [Konfigurera OpenSearch-tjänsten](opensearch.md).

- Elasticsearch gör snabba och avancerade sökningar i produktkatalogen
- Elasticsearch Analyzers har stöd för flera språk
- Stöder stoppord och synonymer
- Indexeringen påverkar inte kunderna förrän omindexeringsåtgärden har slutförts

>[!TIP]
>
>Adobe rekommenderar att du alltid konfigurerar Elasticsearch för ditt Adobe Commerce i molninfrastrukturprojekt, även om du tänker konfigurera ett tredjepartssökverktyg för ditt Adobe Commerce-program. När du konfigurerar Elasticsearch finns ett reservalternativ om tredjepartssökverktyget misslyckas.

{{service-instruction}}

**Aktivera Elasticsearch**:

1. För startprojekt lägger du till tjänsten `elasticsearch` i filen `.magento/services.yaml` med Elasticsearch-versionen och tilldelar diskutrymme i MB.

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   För Pro-projekt måste du skicka in en Adobe Commerce Support-biljett för att ändra Elasticsearch-versionen i mellanlagrings- och produktionsmiljöerna.

1. Ange egenskapen `relationships` i filen `.magento.app.yaml`.

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   Mer information om hur dessa ändringar påverkar dina miljöer finns i [Tjänster](services-yaml.md).

1. När distributionen är klar använder du SSH för att logga in på fjärrmiljön.

   ```bash
   magento-cloud ssh
   ```

1. Indexera om indexet för katalogsökning.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Rensa cachen.

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## Kompatibelt med Elasticsearch

När du installerar eller uppgraderar din Adobe Commerce i ett molninfrastrukturprojekt ska du alltid kontrollera om det finns kompatibilitet mellan Elasticsearch tjänstversion och [Elasticsearch PHP](https://github.com/elastic/elasticsearch-php) -klienten för Adobe Commerce.

- **Första gången du installerar** - Bekräfta att den Elasticsearch-version som anges i filen `services.yaml` är kompatibel med den Elasticsearch PHP-klient som konfigurerats för Adobe Commerce.

- **Projektuppgradering**-Kontrollera att Elasticsearch PHP-klienten i den nya programversionen är kompatibel med den Elasticsearch-tjänstversion som är installerad i molninfrastrukturen.

Tjänstversion och kompatibilitetsstöd för Adobe Commerce i molninfrastruktur avgörs av vilka versioner som distribueras i molninfrastrukturen, och skiljer sig ibland från de versioner som stöds av Adobe Commerce lokala distributioner. Se [Tjänstversioner](services-yaml.md#service-versions).

**Så här kontrollerar du kompatibiliteten för Elasticsearch-program**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Visa information om den aktiva miljön i Elasticsearch.

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. Du kan också använda SSH för att logga in på fjärrmiljön.

   ```bash
   magento-cloud ssh
   ```

1. Kontrollera Composer-paketets version för `elasticsearch/elasticsearch`.

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   Kontrollera den installerade versionen i egenskapen `versions` i svaret.

   ```
   name     : elasticsearch/elasticsearch
   descrip. : PHP Client for Elasticsearch
   keywords : client, elasticsearch, search
   versions : * v7.17.1
   type     : library
   license  : Apache License 2.0 (Apache-2.0) (OSI approved) https://spdx.org/licenses/Apache-2.0.html#licenseText
   license  : GNU Lesser General Public License v2.1 only (LGPL-2.1-only) (OSI approved) https://spdx.org/licenses/LGPL-2.1-only.html#licenseText
   homepage :
   source   : [git] git@github.com:elastic/elasticsearch-php.git f1b8918f411b837ce5f6325e829a73518fd50367
   dist     : [zip] https://api.github.com/repos/elastic/elasticsearch-php/zipball/f1b8918f411b837ce5f6325e829a73518fd50367 f1b8918f411b837ce5f6325e829a73518fd50367
   path     : ~/vendor/elasticsearch/elasticsearch
   names    : elasticsearch/elasticsearch
   ```

   Du kan också hitta Elasticsearch PHP-klientversionen i filen `composer.lock` i miljöns rotkatalog.

1. Hämta anslutningsinformationen för Elasticsearch-tjänsten från kommandoraden.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   I svaret hittar du IP-adressen för Elasticsearch tjänstslutpunkt:

   ```
   | elasticsearch:                                                                                                  |
   +------------------------------------------+----------------------------------------------------------------------+
   | username                                 | null                                                                 |
   | scheme                                   | http                                                                 |
   | service                                  | elasticsearch                                                        |
   | fragment                                 | null                                                                 |
   | ip                                       | 169.254.220.11                                                       |
   | hostname                                 | dzggu33f75wi3sd24lgwtoupxm.elasticsearch.service._.magentosite.cloud |
   | public                                   | false                                                                |
   | cluster                                  | fo3qdoxtla4j4-master-7rqtwti                                         |
   | host                                     | elasticsearch.internal                                               |
   | rel                                      | elasticsearch                                                        |
   | query                                    |                                                                      |
   | path                                     | null                                                                 |
   | password                                 | null                                                                 |
   | type                                     | elasticsearch:6.5                                                    |
   | port                                     | 9200                                                                 |
   +------------------------------------------+----------------------------------------------------------------------+
   ```

1. Hämta den installerade Elasticsearch-tjänsten `version:number` från tjänstslutpunkten.

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```json
   {
      "name" : "-AqGi9D",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "_yze6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "number" : "6.5.4",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "82a8aa7",
        "build_date" : "2019-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "7.5.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
   },
   "  tagline" : "You Know, for Search"
   }
   ```

1. Kontrollera versionskompatibiliteten mellan Elasticsearch och PHP-klienten.

   Om versionerna inte är kompatibla gör du någon av följande uppdateringar av din miljökonfiguration:

   - Ändra Elasticsearch PHP-klienten till en version som är kompatibel med Elasticsearch tjänstversion.

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - Ändra Elasticsearch-tjänstversionen i filen `services.yaml` till en version som är kompatibel med Elasticsearch PHP-klienten.

     {{pro-update-service}}

## Starta om tjänsten Elasticsearch

Om du behöver starta om tjänsten [Elasticsearch](https://www.elastic.co) måste du kontakta Adobe Commerce support.

## Ytterligare sökkonfiguration

- Som standard återskapas sökkonfigurationen för molnmiljöer varje gång du distribuerar. Du kan använda distributionsvariabeln `SEARCH_CONFIGURATION` om du vill behålla anpassade sökinställningar mellan distributioner. Se [Distribuera variabler](../environment/variables-deploy.md#search_configuration).

- När du har konfigurerat Elasticsearch-tjänsten för ditt projekt kan du testa Elasticsearch-anslutningen och anpassa Elasticsearch-inställningarna för Adobe Commerce med hjälp av administratörsgränssnittet.

### Lägg till plugin-program för Elasticsearch

Du kan också lägga till plugin-program för Elasticsearch genom att lägga till avsnittet `configuration:plugins` i Elasticsearch-tjänsten i filen `.magento/services.yaml`. Följande kod aktiverar till exempel ICU-analys och plugin-program för fonetisk analys.

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Om du använder ett plugin-program från en annan leverantör för Elastic Suite måste du [uppdatera `ece-tools` package](../dev-tools/update-package.md) till version 2002.0.19 eller senare.
När du konfigurerar Elastic Suite lägger du till konfigurationsinställningarna i variabeln `ELASTICSUITE_CONFIGURATION` för distribution. Den här konfigurationen sparar inställningarna mellan distributioner.

### Ta bort plugin-program för Elasticsearch

Om du tar bort plugin-posterna från `elasticsearch:` i `.magento/services.yaml` avinstalleras eller inaktiveras de inte på rätt sätt. Du måste indexera om dina Elasticsearch-data. Detta beteende är avsiktligt för att förhindra att data som är beroende av dessa plugin-program går förlorade eller skadas.

**Ta bort Elasticsearch-plugin-program**:

1. Ta bort Elasticsearch-pluginposterna från din `.magento/services.yaml`-fil.
1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Genomför ändringarna av `.magento/services.yaml` i din molnrepo.
1. Indexera om indexet för katalogsökning.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Rensa cachen.

   ```bash
   bin/magento cache:clean
   ```

>[!TIP]
>
>Mer information om hur du använder eller felsöker Elastic Suite-pluginprogrammet med Adobe Commerce finns i [dokumentationen för Elastic Suite](https://github.com/Smile-SA/elasticsuite).

