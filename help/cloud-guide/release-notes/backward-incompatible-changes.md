---
title: Bakåtkompatibla ändringar
description: Lär dig mer om bakåtkompatibilitet när du uppgraderar befintliga Cloud-projekt.
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 3f3c1036-bfd0-4c70-8309-6c5e442134cd
source-git-commit: de50fda78c28a57d76e5c0a4d5dac0f8d4d844a0
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# Bakåtkompatibla ändringar

Bakåtkompatibla ändringar kan kräva att du justerar molnkonfigurationen och processerna för befintliga Cloud-projekt när du uppgraderar till den senaste versionen av `ece-tools`-paketet eller andra Creative Suite-paket för Commerce.

## Ändringar i paketet `ece-tools`

Vissa funktioner som tidigare fanns i paketet `ece-tools` finns nu i separata paket. De här paketen är dispositionsberoenden för `ece-tools`, som installeras och uppdateras automatiskt när du installerar eller uppdaterar extraverktyg.

Den nya arkitekturen bör inte påverka installations- eller uppdateringsprocesserna. Du kan dock behöva ändra vissa kommandosyntaxer och processer när du arbetar med ditt Adobe Commerce i ett molninfrastrukturprojekt. Mer information finns i följande inkompatibla ändringar bakåt och i versionsinformationen för [Cloud Tools Suite](cloud-tools-suite.md).

### Ändringar av tjänstens versionskrav

Vi ändrade minimikravet för PHP-version från 7.0.x till 7.1.x för molnprojekt som använder `ece-tools` v2002.1.0 och senare. Om din miljökonfiguration anger PHP 7.0 ska du uppdatera [php-konfigurationen](../application/php-settings.md) i filen `.magento.app.yaml`.

>[!WARNING]
>
>På grund av ändringen av PHP-versionskraven stöder `ece-tools` 2002.1.0 bara Adobe Commerce i molninfrastrukturprojekt som kör Adobe Commerce 2.1.15 eller senare. Om projektet använder en tidigare version måste du [uppgradera](../development/commerce-version.md) innan du uppdaterar till `ece-tools` 2002.1.0.

### Ändringar av miljökonfiguration

Följande tabell innehåller information om miljövariabler och andra miljökonfigurationsfiler som har tagits bort eller tagits bort i `ece-tools` v2002.1.0.

| Objekt | Ersättning |
| -------- | ----------- |
| Variabeln `SCD_EXCLUDE_THEMES` | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| Variabeln `STATIC_CONTENT_THREADS` | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| Variabeln `DO_DEPLOY_STATIC_CONTENT` | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| Variabeln `STATIC_CONTENT_SYMLINK` | Ingen. Nu skapar bygget alltid en länk till den statiska innehållskatalogen `pub/static`. |
| `build_options.ini` fil | Använd filen [`.magento.env.yaml`](../application/configure-app-yaml.md) för att konfigurera miljövariabler för att hantera bygg- och distributionsåtgärder i alla dina miljöer.<p>Om du skapar en molnmiljö som innehåller filen `build_options.ini` misslyckas bygget. |

### Ändringar av CLI-kommandon

Följande tabell sammanfattar ändringar av CLI-kommandon i ECE-Tools v2002.1.0 som kan kräva att du uppdaterar kommandon eller skript.

| Kommando | Ersättning |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

I tidigare versioner av ECE-Tools kunde du använda kommandona `m2-ece-build` och `m2-ece-deploy` för att konfigurera distributionsnätverk i filen `.magento.app.yaml`. När du uppdaterar till v2002.1.0 bör du kontrollera konfigurationen `hooks` i filen `.magento.app.yaml` för att se om det finns föråldrade kommandon och ersätta dem om det behövs.

## Ändringar av molnkorrigeringar

- **Ta bort hämtade korrigeringsfiler**-Paketet `magento/magento-cloud-patches` paketerar alla korrigeringsfiler som är tillgängliga från sidan [programhämtningar](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html?lang=sv-SE) och tillämpar dem automatiskt när du distribuerar till molnet. För att förhindra korrigeringskonflikter efter uppgradering till ECE-Tools 2002.1.0 eller senare, tar du bort eventuella Adobe-patchar som du har laddat ned och lagt till i ditt projekt manuellt.

- **Uppdaterar kommandot Tillämpa korrigeringar**-Vi har flyttat kommandot för att tillämpa korrigeringar från katalogen `vendor/bin/ece-tools` till katalogen `vendor/bin/ece-patches`. Om du använder det här kommandot för att tillämpa korrigeringar manuellt använder du den nya sökvägen.

  > Tillämpa korrigeringar manuellt

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Ändringar av Cloud Docker

- **PHP-versionen måste vara som lägst PHP-version nu PHP 7.1**-Om Commerce-värden kör en tidigare version uppgraderar du till PHP v7.1 eller senare.

- **Kommandot Cloud Docker för Commerce ändras**-

   - **Uppdaterar Cloud Docker för Commerce-kommandon för Docker-byggåtgärder**-Vi har flyttat Cloud Docker för Commerce-kommandon från katalogen `vendor/bin/ece-tools` till katalogen `vendor/bin/ece-docker`. Uppdatera skript och kommandon så att de använder den nya sökvägen.

     När du har uppgraderat till `ece-tools` 2002.1.0 kan du använda följande kommando för att visa tillgängliga `ece-docker` -kommandon.

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Uppdaterar Cloud-dispositionskommandona**-Vi har ändrat namn på sökvägen till kommandofilen från `./bin/docker` till `./bin/magento-docker`. Uppdatera skript och kommandon så att de använder den nya sökvägen.

   - **Kronbehållaren ingår inte längre i standardkonfigurationen för Docker**-Now, du måste lägga till alternativet `--with-cron` i kommandot `ece-docker build:compose` för att kunna inkludera kronbehållaren i konfigurationen för Docker-miljön. Se [Hantera cron-jobb](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs) i guiden _Cloud Docker för Commerce_.

     Skript som tidigare genererade behållare med cron-jobb är nu utan cron-behållare.

   - **Med temporära behållare**-I tidigare versioner togs inte de behållare som skapades av `bin/magento-docker`-kommandoåtgärder bort, så du kunde använda dem för andra åtgärder. `magento-docker`-kommandona tar nu bort alla behållare som de skapar när kommandot har slutförts.

     Om du vill behålla en behållare som skapats med en Docker-compose-åtgärd använder du kommandot `docker-compose run` i stället för kommandot `bin/magento-docker`.

   - **När du kör kopplingar efter distribution**-Kommandot `cloud-deploy` körs inte längre kopplingar efter distribution. Använd det nya `cloud-post-deploy`-kommandot för att köra kopplingar efter distributionen. Uppdatera skripten och lägg till kommandot för att köra kopplingar efter distributionen.

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     Om du använder `docker-compose`-kommandon direkt kan du köra kommandot `docker-compose run deploy cloud-post-deploy` efter kommandot för distribution.

- **Uppdatering av databasen**-Databasbehållaren lagras nu i den `magento-db` beständiga Docker-volymen. När du uppdaterar Docker-miljön tas databasen inte längre bort automatiskt. Om det behövs kan du ta bort det manuellt med något av följande kommandon.

   - Ta bort behållaren `magento-db`:

     ```bash
     docker volume rm magento-db
     ```

   - Ta bort alla associerade volymer när du stänger av Docker-behållarna:

     ```bash
     docker-compose down -v
     ```

- **Åsidosätt filsynkroniseringsinställningar för arkiv- och säkerhetskopieringsfiler**-Arkiv- och säkerhetskopieringsfiler med följande tillägg synkroniseras inte längre när du använder dockningssynkronisering eller mutagen: SQL, GZ, ZIP och BZ2. Du kan åsidosätta standardfilsynkroniseringen för dessa filtyper genom att byta namn på filen så att den slutar med ett annat filtillägg. Till exempel: `synchronize-me.zip-backup`
