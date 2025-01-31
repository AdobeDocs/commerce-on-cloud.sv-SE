---
title: Smarta guider
description: Lär dig hur du använder smarta guider för att utvärdera om ditt Adobe Commerce-projekt för molninfrastruktur följer bästa praxis för driftsättning.
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Smarta guider

De smarta guiderna kan hjälpa dig att avgöra om din molnkonfiguration följer vedertagna standarder. De tillgängliga guiderna hjälper dig med följande konfigurationer:

- Idealiskt läge för minimal driftsättning
- Belastningsbalanskonfiguration för databas och Redis
- Statisk innehållsdistribution (SCD) för on-demand, byggfasen eller driftsättningsfasen

Var och en av de smarta guidekommandona ger ett verifieringssvar och, om tillämpligt, en rekommendation för rätt konfiguration.

| Kommando | Beskrivning |
| ------- | ------------|
| `wizard:ideal-state` | Kontrollera att SCD finns på scenen _build_, att variabeln `SKIP_HTML_MINIFICATION` är `true` och att funktionen post_deploy har konfigurerats i molnmiljön. Ej till lokal utvecklingsmiljö. |
| `wizard:master-slave` | Kontrollera att variabeln `REDIS_USE_SLAVE_CONNECTION` och variabeln `MYSQL_USE_SLAVE_CONNECTION` är `true`. |
| `wizard:scd-on-demand` | Kontrollera att den globala miljövariabeln `SCD_ON_DEMAND` är `true`. |
| `wizard:scd-on-build` | Kontrollera att den globala miljövariabeln `SCD_ON_DEMAND` är `false` och miljövariabeln `SKIP_SCD` är `false` för scenen _build_. Verifierar att filen `config.php` innehåller information för arkiv, butiksgrupper och webbplatser. |
| `wizard:scd-on-deploy` | Kontrollera att den globala miljövariabeln `SCD_ON_DEMAND` är `false` och miljövariabeln `SKIP_SCD` är `false` för scenen _deploy_. Verifierar att filen `config.php` inte innehåller _NOT_ med listan över butiker, butiksgrupper och webbplatser med relaterad information. |

Du kan till exempel kontrollera att konfigurationen aktiverar funktionen för on demand-SCD:

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

En lyckad konfiguration returnerar:

```
SCD on-demand is enabled
```

En misslyckad konfiguration returnerar:

```
SCD on-demand is disabled
```

## Verifiera en idealisk konfiguration

Konfigurationen _idealisk_ för ditt Cloud-projekt hjälper till att minimera driftsättningsdriftavbrott genom att värma cachen och generera statiskt innehåll när användaren begär det. Den här guiden körs automatiskt under distributionsprocessen. Om ditt moln inte är konfigurerat för det här _idealiska läget_ får du ett meddelande som liknar följande:

```
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

Beroende på utdata måste du göra följande korrigeringar i konfigurationen:

1. Aktivera miniatyrvariabeln Skip HTML.

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. Konfigurera postdriftsättningskroken.

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. Skjut upp kodändringarna och kör testet igen. När konfigurationen är _idealisk_ får du följande meddelande.

   ```
   Ideal state is configured
   ```
