---
title: Konfigurera Valkey-tjänsten
description: Lär dig hur du konfigurerar och optimerar Valkey som en serverdelscachelösning för Adobe Commerce i molninfrastruktur.
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
source-git-commit: cf2e659267445603b3f5eaf877f4eb7ac0c1b54c
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# Konfigurera Valkey-tjänsten

[Valkey](https://valkey.io) är en valfri serverdelscachelösning som ersätter `Zend Framework Zend_Cache_Backend_File`, som Adobe Commerce använder som standard.

Se [Konfigurera Valkey](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/valkey/config-valkey.html?lang=sv-SE){target="_blank"} i _Konfigurationsguiden_.

{{service-instruction}}

**Om du vill ersätta Redis med Valkey uppdaterar du konfigurationen i följande tre filer**:

1. Lägg till önskat namn och typ i filen `.magento/services.yaml`.

   ```yaml
   cache:
       type: valkey:<version>
   ```

   Om du vill ange en egen Valkey-konfiguration lägger du till en `core_config`-nyckel i `.magento/services.yaml`-filen:

   ```yaml
   cache:
       type: valkey:8.0
   ```

1. Konfigurera relationerna i filen `.magento.app.yaml`.

   ```yaml
   relationships:
       valkey: "cache:valkey"
   ```

1. Konfigurera `.magento.env.yaml` enligt följande:.

   ```yaml
    stage:
        deploy:
        VALKEY_USE_SLAVE_CONNECTION: true
        VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
   ```

1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add .magento/services.yaml .magento.app.yaml .magento.env.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [Verifiera tjänstrelationerna](services-yaml.md#service-relationships).

{{service-change-tip}}

## Använda Valkey CLI

Om du antar att din Valkey-relation har namnet `valkey` kan du komma åt den med verktyget `valkey-cli`.

1. Använd SSH för att ansluta till integreringsmiljön med Valkey installerat och konfigurerat.

1. Öppna en SSH-tunnel för en värd.

   ```bash
   valkey-cli -h valkey.internal
   ```

## Hämta installerad Valkey-version

Använd följande kommando för att installera Valkey-versionen i en integreringsmiljö:

```bash
valkey-cli -h valkey.internal info | grep version
```

Svar:

```
valkey_version:8.0.1
gcc_version:12.2.0
```

### Valkey on Pro staging and production

Använd kommandot `valkey-server` om du vill få Valkey-versionen installerad i en mellanlagrings- eller produktionsmiljö:

```bash
valkey-server -v
```

```bash
Valkey server v=8.0.1 ...
```

Använd följande kommando för att få Valkey-konfigurationen installerad i en Pro Staging- eller Production-miljö:

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Svar:

```json
"valkey" : [
    {
        "cluster" : "project-master-abc0003",
        "epoch" : 0,
        "fragment" : null,
        "host" : "valkeycache.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.cache.service._.magentosite.cloud",
        "instance_ips" : [
        "123.456.789.012"
        ],
        "ip" : "123.456.789.012",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "valkey",
        "scheme" : "valkey",
        "service" : "cache",
        "type" : "valkey:8.0",
        "username" : null
    }
]
```
