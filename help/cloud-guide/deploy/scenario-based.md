---
title: Scenariobaserad driftsättning
description: Lär dig hur du anpassar Adobe Commerce för driftsättning av molninfrastruktur med anpassade konfigurationsfiler.
feature: Cloud, Configuration, Deploy, Build
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# Scenariobaserad driftsättning

Med `ece-tools` 2002.1.0 och senare kan du använda den scenariobaserade distributionsfunktionen för att anpassa standarddistributionsbeteendet.
Den här funktionen använder **scenarier** och **steg** i konfigurationen:

- **Scenariokonfiguration**-Varje distributionskrok är ett *scenario* som är en XML-konfigurationsfil som beskriver sekvens- och konfigurationsparametrarna för att slutföra distributionsåtgärder. Du konfigurerar scenarierna i avsnittet `hooks` i filen `.magento.app.yaml`.

- **Stegkonfiguration**-Varje scenario använder en sekvens av *steg* som programmässigt beskriver de åtgärder som krävs för att slutföra distributionsåtgärder. Du konfigurerar stegen i en XML-baserad scenariokonfigurationsfil.

Adobe Commerce i molninfrastrukturen innehåller en uppsättning [standardscenarier](https://github.com/magento/ece-tools/tree/2002.1/scenario) och [standardsteg](https://github.com/magento/ece-tools/tree/2002.1/src/Step) i `ece-tools`-paketet. Du kan anpassa distributionsbeteendet genom att skapa anpassade XML-konfigurationsfiler som åsidosätter eller anpassar standardkonfigurationen. Du kan också använda scenarier och steg för att köra kod från anpassade moduler.

## Lägga till scenarier med hjälp av bygg- och driftsättningskopplingar

Du lägger till scenarierna för att skapa och distribuera Adobe Commerce i `hooks`-avsnittet i filen `.magento.app.yaml`. Varje krok anger vilka scenarier som ska köras under varje fas. I följande exempel visas standardscenariokonfigurationen.

> `magento.app.yaml` krokar

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>I och med versionen av `ece-tools` 2002.1.x finns det ett nytt [hooks-konfigurationsformat](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property.html?lang=sv-SE). Det gamla formatet från `ece-tools` 2002.0.x stöds fortfarande. Du måste dock uppdatera till det nya formatet för att kunna använda den scenariobaserade distributionsfunktionen.

## Granska scenariesteg

I krokkonfigurationen är varje scenario en XML-fil som innehåller steg för att köra åtgärder för att skapa, distribuera eller efterdistribuera. Filen `scenario/transfer` innehåller till exempel tre steg: `compress-static-content`, `clear-init-directory` och `backup-data`

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## Utöka ett standardscenario

I följande exempel utökas standardscenariot för distribution genom att ytterligare konfigurationsfiler för distribution läggs till i krokkonfigurationen.

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

Under distributionen sammanfogas de anpassade scenarierna med standardscenariot baserat på följande regler:

- Scenarier prioriteras baserat på deras sekvens i krokdefinitionen med det senaste scenariot som har högst prioritet.

  I exemplet har scenarierna följande prioritet:

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` (standard- eller baslinjescenario)

- Stegen i det högsta prioriterade scenariot åsidosätter steg med samma namn i de andra scenarierna. Nya steg läggs till i konfigurationen. Samma regler gäller för mer än två scenarier där varje scenario prioriteras från höger till vänster, till exempel (C → B → A).

### Ta bort standardsteg

Du tar bort steg från standardscenarier med parametern `skip`.

Om du till exempel vill hoppa över stegen `enable-maintenance-mode` och `set-production-mode` i standarddistributionsscenariot skapar du en konfigurationsfil som innehåller följande konfiguration.

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

Om du vill använda den anpassade konfigurationsfilen uppdaterar du standardfilen `.magento.app.yaml`.

> `.magento.app.yaml` med anpassat distributionsscenario

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### Ersätt standardsteg

Anpassade scenarier kan ersätta standardsteg för anpassad implementering. Om du vill göra det använder du standardstegnamnet som namn för det anpassade steget.

I [standarddistributionsscenariot] kör till exempel `enable-maintenance-mode` standardsteget standardskriptet [EnableMaintenanceMode PHP].

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

Om du vill åsidosätta det här steget skapar du en anpassad scenariokonfigurationsfil som kör ett annat skript när steget `enable-maintenance-mode` körs.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### Ändra stegprioritet

Anpassade scenarier kan ändra prioriteten för standardsteg. Följande steg ändrar prioriteten för steget `enable-maintenance-mode` från `300` till `10` så att steget körs tidigare i distributionsscenariot.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

I det här exemplet flyttas steget `enable-maintenance-mode` till början av scenariot eftersom det har lägre prioritet än alla andra steg i standardscenariot för distribution.

### Exempel: Utöka scenariot för distribution

I följande exempel anpassas [standarddistributionsscenariot] med följande ändringar:

- Ersätter steget `remove-deploy-failed-flag` med ett anpassat steg
- Hoppar över understeget `clean-redis-cache` i steget före distribution
- Hoppar över steget `unlock-cron-jobs`
- Hoppar över steget `validate-config` för att inaktivera kritiska validerare
- Lägger till ett nytt fördistributionssteg

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

Om du vill använda det här skriptet i ditt projekt lägger du till följande konfiguration i filen `.magento.app.yaml` för ditt Adobe Commerce i molninfrastrukturprojekt:

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>Du kan granska [standardscenarierna](https://github.com/magento/ece-tools/tree/2002.1/scenario) och [standardstegskonfigurationen](https://github.com/magento/ece-tools/tree/2002.1/src/Step) i GitHub-databasen `ece-tools` för att avgöra vilka scenarier och steg som ska anpassas för dina projektbygge, distributioner och efterdistribuerade uppgifter.

## Lägg till en anpassad modul för att utöka `ece-tools`

Paketet `ece-tools` innehåller standardgränssnitt för API som följer semantiska versionsstandarder. Alla API-gränssnitt har markerats med **@api**-anteckning. Du kan ersätta standard-API-implementeringen med din egen genom att skapa en anpassad modul och ändra standardkoden efter behov.

Om du vill använda den anpassade modulen med Adobe Commerce i molninfrastrukturen måste du registrera modulen i tilläggslistan för `ece-tools`-paketet. Registreringsprocessen liknar den som du använder för att registrera moduler i Adobe Commerce.

**Så här registrerar du en modul med `ece-tools` package**:

1. Skapa eller utöka filen `registration.php` i modulens rot.

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. Uppdatera avsnittet `autoload` för modulkonfigurationsfilen så att den innehåller filen `registration.php` för automatisk inläsning av modulfiler i `composer.json`.

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. Lägg till filen `config/services.xml` i modulen. Den här konfigurationen sammanfogas över `config/services.xml` från `ece-tools`-paketet.

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

Mer information om beroendeinjektion finns i [Symfony Dependency Injection](https://symfony.com/doc/current/components/dependency_injection.html).

<!-- link definitions -->

[standarddistributionsscenario]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[EnableMaintenanceMode PHP-skript]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
