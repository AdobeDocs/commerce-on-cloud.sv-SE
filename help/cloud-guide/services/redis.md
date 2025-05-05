---
title: Konfigurera Redis-tjänsten
description: Lär dig hur du konfigurerar och optimerar Redis som en serverdelslösning för Adobe Commerce i molninfrastrukturen.
feature: Cloud, Cache, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Konfigurera Redis-tjänsten

[Redis](https://redis.io) är en valfri serverdelscachelösning som ersätter Zend Framework Zend_Cache_Backend_File, som Adobe Commerce använder som standard.

Se [Konfigurera Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/config-redis.html?lang=sv-SE) i _Konfigurationsguiden_.

{{service-instruction}}

**Så här aktiverar du Redis**:

1. Lägg till önskat namn och typ i filen `.magento/services.yaml`.

   ```yaml
   myredis:
       type: redis:<version>
   ```

   Om du vill ha en egen Redis-konfiguration lägger du till en `core_config`-nyckel i `.magento/services.yaml`-filen:

   ```yaml
   cache:
       type: redis:<version>
   ```

1. Konfigurera relationerna i filen `.magento.app.yaml`.

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. Lägg till, implementera och push-överföra kodändringar.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [Verifiera tjänstrelationerna](services-yaml.md#service-relationships).

{{service-change-tip}}

## Använda Redis CLI

Om du antar att din Redis-relation har namnet `redis` kan du komma åt den med verktyget `redis-cli`.

1. Använd SSH för att ansluta till integreringsmiljön med Redis installerat och konfigurerat.

1. Öppna en SSH-tunnel för en värd.

   ```bash
   redis-cli -h redis.internal
   ```

## Installera Redis-versionen

Använd följande kommando för att installera Redis-versionen i en integreringsmiljö:

```bash
redis-cli -h redis.internal info | grep version
```

Exempelsvar:

```
redis_version:7.0.5
gcc_version:8.3.0
```

### Redis on Pro staging and production

Använd kommandot `redis-server` om du vill få Redis-versionen installerad i en mellanlagrings- eller produktionsmiljö:

```bash
redis-server -v
```

```
Redis server v=7.0.5 ...
```

Använd följande kommando för att få Redis-konfigurationen installerad i en Pro Staging- eller Production-miljö:

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Exempelsvar:

```json
"redis" : [
    {
        "cluster" : "project-master-123abc4",
        "fragment" : null,
        "host" : "redis.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.redis.service._.magentosite.cloud",
        "ip" : "169.254.10.10",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "redis",
        "scheme" : "redis",
        "service" : "redis",
        "type" : "redis:7.0.5",
        "username" : null
    }
]
```

## Felsökning av Redis

Se följande Adobe Commerce supportartiklar för hjälp med felsökning av Redis-problem:

- [Återvisa fördröjning av problem Administratörsinloggning eller utcheckning](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html?lang=sv-SE)
- [Implementering av utökat Redis-cache, Adobe Commerce 2.3.5+](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html?lang=sv-SE)
- [Hanterade aviseringar på Adobe Commerce: Redis Memory-varning](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html?lang=sv-SE)
- [Hanterade aviseringar på Adobe Commerce: Redis memory critical alert](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html?lang=sv-SE)
