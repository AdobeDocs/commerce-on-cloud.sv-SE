---
title: Konfigurera OpenSearch-tjänsten
description: Lär dig hur du aktiverar OpenSearch-tjänsten för Adobe Commerce i molninfrastruktur.
feature: Cloud, Search, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 0%

---

# Konfigurera OpenSearch-tjänsten

Tjänsten [OpenSearch](https://www.opensearch.org) är en öppen källkodsgaffel i Elasticsearch 7.10.2, efter licensändringarna för Elasticsearch. Se [OpenSource-projektet](https://github.com/opensearch-project) i GitHub.

{{elasticsearch-support}}

Med OpenSearch kan du hämta data från alla källor, alla format och söka och visualisera dem i realtid.

- Snabba och avancerade sökningar på produkter i produktkatalogen
- OpenSearch Analyzers stöder flera språk
- Stöder stoppord och synonymer
- Indexeringen påverkar inte kunderna förrän omindexeringsåtgärden har slutförts

{{service-instruction}}

>[!TIP]
>
>Adobe rekommenderar att du alltid konfigurerar OpenSearch för ditt Adobe Commerce i molninfrastrukturprojekt, även om du tänker konfigurera ett tredjepartssökverktyg för ditt Adobe Commerce-program. Om det inte går att konfigurera OpenSearch finns ett reservalternativ om tredjepartssökverktyget misslyckas.

**Så här aktiverar du OpenSearch**:

1. För integreringsmiljöerna Starter och Pro lägger du till tjänsten `opensearch` i filen `.magento/services.yaml` med rätt version och allokerat diskutrymme i MB. I det här fallet är version 2 lämplig. Den mindre versionen krävs inte eftersom molninfrastrukturen använder den senaste versionen av OpenSearch.

   ```yaml
   opensearch:
       type: opensearch:2
       disk: 1024
   ```

   För Pro-projekt måste du [skicka in en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) för att kunna ändra OpenSearch-versionen i mellanlagrings- och produktionsmiljöer.

1. Ange eller verifiera egenskapen `relationships` i filen `.magento.app.yaml`.

   ```yaml
   relationships:
       opensearch: "opensearch:opensearch"
   ```

1. Lägg till, implementera och push-ändra kod.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable OpenSearch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   Mer information om hur dessa ändringar påverkar dina miljöer finns i [Konfigurera tjänster](services-yaml.md).

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

## Kompatibelt med OpenSearch-program

När du installerar eller uppgraderar din Adobe Commerce i ett molninfrastrukturprojekt ska du alltid kontrollera kompatibiliteten mellan OpenSearch-tjänstversionen och [OpenSearch PHP](https://github.com/opensearch-project/opensearch-php) -klienten för Adobe Commerce.

- **Första gången du installerar** - Bekräfta att OpenSearch-versionen som anges i filen `services.yaml` är kompatibel med OpenSearch PHP-klienten som konfigurerats för Adobe Commerce.

- **Projektuppgradering**-Kontrollera att PHP-klienten för OpenSearch i den nya programversionen är kompatibel med OpenSearch-tjänstversionen som är installerad i molninfrastrukturen.

Tjänstversionen och kompatibilitetsstödet bestäms av versionerna som har testats och distribuerats i molninfrastrukturen, och skiljer sig ibland från versioner som stöds av Adobe Commerce lokala distributioner. En lista över vilka versioner som stöds finns i [Systemkrav](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) i _installationshandboken_.

**Så här verifierar du OpenSearch-programkompatibilitet**:

1. Byt till din projektkatalog på din lokala arbetsstation.

1. Visa information om OpenSearch för den aktiva miljön.

   ```bash
   magento-cloud relationships --property=opensearch
   ```

1. Du kan också använda SSH för att logga in på fjärrmiljön.

   ```bash
   magento-cloud ssh
   ```

1. Hämta anslutningsinformationen för OpenSearch-tjänsten.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   I svaret hittar du IP-adressen och porten för OpenSearch-tjänstens slutpunkt:

   ```
   +------------------------------------------+--------------------------------------------------------+
   | opensearch:                                                                                       |
   +------------------------------------------+--------------------------------------------------------+
   | username                                 | null                                                   |
   | scheme                                   | http                                                   |
   | service                                  | opensearch                                             |
   | fragment                                 | null                                                   |
   | ip                                       | 169.254.220.11                                         |
   | hostname                                 | hostf75wi3sd24l.opensearch.service._.magentosite.cloud |
   | port                                     | 9200                                                   |
   | cluster                                  | projectID-develop-4ranwui                              |
   | host                                     | opensearch.internal                                    |
   | rel                                      | opensearch                                             |
   | path                                     | null                                                   |
   | query                                    |                                                        |
   | password                                 | null                                                   |
   | type                                     | opensearch:2                                           |
   | public                                   | false                                                  |
   | host_mapped                              | false                                                  |
   ```

1. Hämta den installerade OpenSearch-tjänsten `version:number` från tjänstslutpunkten.

   ```bash
   curl -XGET <opensearch-service-endpoint-ip-address>:9200
   ```

   ```json
   {
      "name" : "opensearch.0",
      "cluster_name" : "opensearch",
      "cluster_uuid" : "_yzaae6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "distribution" : "opensearch",
        "number" : "2.5.0",
        "build_type" : "deb",
        "build_hash" : "aaaaaaa",
        "build_date" : "2023-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "9.4.2",
        "minimum_wire_compatibility_version" : "7.10.0",
        "minimum_index_compatibility_version" : "7.0.0"
   },
   "tagline" : "The OpenSearch Project: https://opensearch.org/"
   }
   ```

{{pro-update-service}}

## Starta om tjänsten OpenSearch

Om du behöver starta om tjänsten OpenSearch måste du kontakta Adobe Commerce support.

## Ytterligare sökkonfiguration

- Som standard återskapas sökkonfigurationen för molnmiljöer varje gång du distribuerar. Du kan använda distributionsvariabeln `SEARCH_CONFIGURATION` om du vill behålla anpassade sökinställningar mellan distributioner. Se [Distribuera variabler](../environment/variables-deploy.md#search_configuration).

- När du har konfigurerat OpenSearch-tjänsten för ditt projekt använder du Admin-gränssnittet för att testa OpenSearch-anslutningen och anpassa OpenSearch-inställningarna för Adobe Commerce.

### Lägg till plugin-program för OpenSearch

Du kan också lägga till plugin-program för OpenSearch genom att lägga till avsnittet `configuration:plugins` i OpenSearch-tjänsten i filen `.magento/services.yaml`. Följande kod aktiverar till exempel ICU-analys och plugin-program för fonetisk analys.

```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Mer information om plugin-program finns i [OpenSearch-projektet](https://github.com/opensearch-project).

### Ta bort plugin-program för OpenSearch

Om du tar bort plugin-posterna från avsnittet `opensearch:` i filen `.magento/services.yaml` avinstalleras eller inaktiveras inte **tjänsten**. Om du vill inaktivera tjänsten helt måste du indexera om dina OpenSearch-data efter att du har tagit bort plugin-programmen från `.magento/services.yaml`-filen. Den här designen förhindrar att data som är beroende av dessa plugin-program går förlorade eller skadas.

**Så här tar du bort OpenSearch-plugin-program**:

1. Ta bort OpenSearch-pluginposterna från filen `.magento/services.yaml`.
1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove OpenSearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Genomför `.magento/services.yaml`-ändringarna i din molndatabas.
1. Indexera om indexet för katalogsökning.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Rensa cachen.

   ```bash
   bin/magento cache:clean
   ```
