---
title: Konfigurera [!DNL Xdebug]
description: Lär dig hur du konfigurerar Xdebug-tillägget för felsökning av din Adobe Commerce när du utvecklar projekt för molninfrastruktur.
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1920'
ht-degree: 0%

---

# Konfigurera Xdebug

[!DNL Xdebug] är ett tillägg till felsökning av PHP. Även om du kan använda en integrerad utvecklingsmiljö som du väljer beskrivs hur du konfigurerar [!DNL Xdebug] och [!DNL PhpStorm] för felsökning i den lokala miljön.

>[!NOTE]
>
>Du kan konfigurera [!DNL Xdebug] så att den körs i Cloud Docker-miljön för lokal felsökning utan att ändra din projektkonfiguration för Adobe Commerce för molninfrastruktur. Se [Konfigurera Xdebug för Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).

Om du vill aktivera [!DNL Xdebug] måste du konfigurera en fil i Git-databasen, konfigurera IDE och konfigurera portvidarebefordran. Du kan konfigurera vissa inställningar i filen `magento.app.yaml`. Efter redigering kan du aktivera [!DNL Xdebug] genom att överföra Git-ändringarna i alla Starter-miljöer och Pro-integreringsmiljöer. [!DNL Xdebug] är redan tillgängligt i Pro Staging &amp; Production-miljöer.

När konfigurationen är klar kan du felsöka CLI-kommandon, webbförfrågningar och kod. Kom ihåg att alla molninfrastrukturmiljöer är skrivskyddade. Klona koden i den lokala utvecklingsmiljön för att utföra felsökningen. För Pro-miljöer för mellanlagring och produktion, se [ytterligare instruktioner](#debug-for-pro-staging-and-production) för [!DNL Xdebug].

## Krav

Om du vill köra och använda [!DNL Xdebug] behöver du SSH-URL:en för miljön. Du kan hitta informationen via [[!DNL Cloud Console]](../project/overview.md) eller [!DNL Cloud Onboarding UI].

## Konfigurera Xdebug

Så här konfigurerar du [!DNL Xdebug]:

- [Arbeta i en gren för att överföra filuppdateringar](#get-started-with-a-branch)
- [Aktivera [!DNL Xdebug] för miljöer](#enable-xdebug-in-your-environment)
- [Konfigurera PHPStorm-servern](#configure-phpstorm-server)
- [Konfigurera portvidarebefordran](#set-up-port-forwarding)

### Kom igång med en gren

Om du vill lägga till [!DNL Xdebug] rekommenderar Adobe att du arbetar i [en utvecklingsgren](../dev-tools/cloud-cli-overview.md#create-an-environment-branch).

### Aktivera Xdebug i miljön

Du kan aktivera [!DNL Xdebug] direkt i alla Starter-miljöer och Pro-integreringsmiljöer. Det här konfigurationssteget krävs inte för Pro Production &amp; Staging-miljöer. Se [Felsök för Pro Staging and Production](#debug-for-pro-staging-and-production).

>[!VIDEO](https://video.tv.adobe.com/v/3437407?learn=on)

Om du vill aktivera [!DNL Xdebug] för ditt projekt lägger du till `xdebug` i avsnittet `runtime:extensions` i filen `.magento.app.yaml`.

**Så här aktiverar du Xdebug**:

1. Öppna filen `.magento.app.yaml` i en textredigerare i din lokala terminal.

1. Lägg till `xdebug` under `extensions` i avsnittet `runtime`. Exempel:

   ```yaml
   runtime:
       extensions:
           - redis
           - xsl
           - newrelic
           - sodium
           - xdebug
   ```

1. Spara ändringarna i filen `.magento.app.yaml` och avsluta textredigeraren.

1. Lägg till, implementera och kör ändringarna för att omdistribuera miljön.

   ```bash
   git add .magento.app.yaml
   ```

   ```bash
   git commit -m "add xdebug"
   ```

   ```bash
   git push origin <environment-ID>
   ```

[!DNL Xdebug] är nu tillgängligt när den distribueras till Starter-miljöer och Pro-integreringsmiljöer. Fortsätt konfigurera IDE. Information om PhpStorm finns i [Konfigurera PhpStorm](#configure-phpstorm).

### Konfigurera PhpStorm-server

>[!VIDEO](https://video.tv.adobe.com/v/3437409?learn=on)

IDE:n [PhpStorm](https://www.jetbrains.com/phpstorm/) måste vara konfigurerad för att fungera korrekt med [!DNL Xdebug].

**Så här konfigurerar du PhpStorm så att det fungerar med Xdebug**:

1. Öppna panelen **Inställningar** i ditt PhpStorm-projekt.

   - _macOS_ - Välj **PhpStorm** > **Inställningar**.
   - _Windows/Linux_ - Välj **Arkiv** > **Inställningar**.

1. Utöka avsnittet **PHP** på panelen _Inställningar_ och klicka på **Servrar**.

1. Klicka på **+** för att lägga till en serverkonfiguration. Projektnamnet är grått högst upp.

1. [Valfritt] Konfigurera följande inställningar för den nya serverkonfigurationen. Se [Ingen felsökningsserver har konfigurerats](https://www.jetbrains.com/help/phpstorm/troubleshooting-php-debugging.html#no-debug-server-is-configured) i _PHPStorm_ -dokumentationen.

   - **Namn** - Ange samma som värdnamn. Det här värdet måste matcha värdet för variabeln `PHP_IDE_CONFIG` i [Felsök CLI-kommandon](#debug-cli-commands) för att kunna använda CLI för felsökning.
   - **Värd** - Ange värdnamnet.
   - **Port** - Ange `443`.
   - **Felsökning** - Välj `Xdebug`.

1. Välj **Använd banavbildningar**. I rutan _Fil/katalog_ visas roten för projektet för `serverName`.

1. Klicka på ikonen **Redigera** i kolumnen **Absolut sökväg på servern** och lägg till en inställning baserad på miljön.

   - Fjärrsökvägen är `/app` för alla Starter-miljöer och Pro-integreringsmiljöer.
   - För Pro Staging- och Production-miljöer:

      - Produktion: `/app/<project_code>/`
      - Mellanlagring: `/app/<project_code>_stg/`

1. Ändra porten [!DNL Xdebug] till `9000,9003` eller så kan du begränsa den till bara `9000` i panelen **PHP** > **Felsök** > **Xdebug** > **Felsök port**.

1. Klicka på **Använd**.

### Skapa konfigurationen för PHPStorm Run/Debug

Detta gör att programmet kan ha rätt felsökningsinställningar för att hantera begäran från Adobe Commerce-programmet.

>[!VIDEO](https://video.tv.adobe.com/v/3437426?learn=on)

1. Öppna PHPStorm-programmet och klicka på **[!UICONTROL Add Configuration]** i skärmens övre högra hörn.

1. Klicka på **[!UICONTROL Add new run configuration]**.

1. Välj alternativet **[!UICONTROL PHP Remote Debug]**.

   - Ange ett unikt, men identifierbart namn.
   - Markera kryssrutan [!UICONTROL Filter debug connection by IDE key]**.
   - Markera servern som du skapade i det [föregående avsnittet](#configure-phpstorm-server). Om du inte har skapat den än kan du skapa en nu, men se den delen av installationsguiden.
   - Skriv `PHPSTORM` med versaler i textfältet **[!UICONTROL IDE key(session id)]**. Vi kommer att använda detta i andra delar av installationen, så det är viktigt att vi behåller detta. Om du väljer en annan sträng måste du komma ihåg att använda den någon annanstans i installations- och konfigurationsprocessen.

1. Klicka på **[!UICONTROL Apply]** > **[!UICONTROL OK]**.

### Konfigurera portvidarebefordran

>[!VIDEO](https://video.tv.adobe.com/v/3437410?learn=on)

Mappa `XDEBUG`-anslutningen från servern till det lokala systemet. För att kunna utföra alla typer av felsökning måste du vidarebefordra port 9000 från din Adobe Commerce på molninfrastrukturservern till din lokala dator. Se något av följande avsnitt:

- [Portvidarebefordran på Mac eller UNIX](#port-forwarding-on-mac-or-unix)
- [Portvidarebefordran i Windows](#port-forwarding-on-windows)

#### Portvidarebefordran på Mac eller UNIX®

**Så här konfigurerar du portvidarebefordran på en Mac eller i en UNIX®-miljö**:

1. Öppna en terminal.

1. Använd SSH för att upprätta anslutningen.

   ```bash
   ssh -R 9000:localhost:9000 <ssh url>
   ```

   Använd alternativet `-v` (utförligt) så att när en socket är ansluten till porten som vidarebefordras visas den i terminalen.

   Om ett fel av typen&quot;Det gick inte att ansluta&quot; eller&quot;det gick inte att lyssna på porten på fjärrservern&quot; visas, kan det finnas en annan aktiv SSH-session som är beständig på servern och som upptar port 9000. Om den anslutningen inte används kan du avsluta den.

**Så här felsöker du anslutningen**:

1. Använd SSH för att logga in på fjärrintegrerings-, mellanlagrings- eller produktionsmiljön.

1. Visa en lista över SSH-sessioner: `who`

1. Visa befintliga SSH-sessioner per användare. Var noga med att inte påverka andra användare än dig själv!

   - integrering: användarnamn liknar `dd2q5ct7mhgus`
   - Mellanlagring: användarnamn liknar `dd2q5ct7mhgus_stg`
   - Produktion: användarnamn liknar `dd2q5ct7mhgus`

1. För en användarsession som är äldre än din bör du hitta pseudoterminalvärdet (PTS), till exempel `pts/0`.

1. Avsluta process-ID (PID) som motsvarar PTS-värdet.

   ```bash
   ps aux | grep ssh
   kill <PID>
   ```

   Exempelsvar:

   ```
   dd2q5ct7mhgus        5504  0.0  0.0  82612  3664 ?      S    18:45   0:00 sshd: dd2q5ct7mhgus@pts/0
   ```

   Om du vill avsluta anslutningen anger du ett avslutskommando med process-ID (PID).

   ```bash
   kill 3664
   ```

#### Portvidarebefordran i Windows

Om du vill konfigurera portvidarebefordran (SSH-tunnling) på Windows måste du konfigurera Windows-terminalprogrammet. I det här exemplet skapas en SSH-tunnel med [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). Du kan använda andra program som Cygwin. Mer information om andra program finns i leverantörsdokumentationen som medföljer dessa program.

**Så här konfigurerar du en SSH-tunnel i Windows med Putty**:

1. Hämta [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) om du inte redan har gjort det.

1. Starta Putty.

1. Klicka på **Session** i kategorirutan.

1. Ange följande information:

   - Fältet **Värdnamn (eller IP-adress)**: Ange [SSH-URL](../development/secure-connections.md#connect-to-a-remote-environment) för molnservern
   - **Port**-fält: Retur `22`

   ![Konfigurera Putty](../../assets/xdebug/putty-session.png)

1. Klicka på **Anslutning** > **SSH** > **Tunnlar** i rutan _Kategori_.

1. Ange följande information:

   - **Source-portfält**: Ange `9000`
   - **Målfält**: Ange `127.0.0.1:9000`
   - Klicka på **Fjärr**

1. Klicka på **Lägg till**.

   ![Skapa en SSH-tunnel i Putty](../../assets/xdebug/putty-tunnels.png)

1. Klicka på **Session** i rutan _Kategori_.

1. Ange ett namn för den här SSH-tunneln i fältet **Sparade sessioner**.

1. Klicka på **Spara**.

   ![Spara SSH-tunneln](../../assets/xdebug/putty-session-save.png)

1. Om du vill testa SSH-tunneln klickar du på **Läs in** och sedan på **Öppna**.

   Om ett &quot;Det går inte att ansluta&quot;-fel visas kontrollerar du följande:

   - Alla mjuka inställningar är korrekta
   - Du kör Putty på den dator där din privata Adobe Commerce på SSH-nycklar för molninfrastruktur finns

## SSH-åtkomst till Xdebug-miljöer

För att starta felsökning, utföra inställningar med mera behöver du SSH-kommandona för att komma åt miljöerna. Du kan hämta den här informationen genom [[!DNL Cloud Console]](../development/secure-connections.md#use-an-ssh-command) och ditt projektkalkylblad.

I Starter-miljöer och i Pro-integreringsmiljöer kan du använda följande `magento-cloud` CLI-kommando för SSH i dessa miljöer:

```bash
magento-cloud environment:ssh --pipe -e <environment-ID>
```

Om du vill använda [!DNL Xdebug], SSH till miljön enligt följande:

```bash
ssh -R <xdebug listen port>:<host>:<xdebug listen port> <SSH-URL>
```

Exempel:

```bash
ssh -R 9000:localhost:9000 pwga8A0bhuk7o-mybranch@ssh.us.magentosite.cloud
```

## Debug for Pro Staging and Production

>[!NOTE]
>
>I Pro Staging &amp; Production-miljöer är [!DNL Xdebug] alltid tillgängligt eftersom dessa miljöer har en särskild konfiguration för [!DNL Xdebug]. Alla normala webbbegäranden dirigeras till en dedikerad PHP-process som inte har [!DNL Xdebug]. Därför behandlas dessa begäranden normalt och påverkas inte av prestandaförsämringen när [!DNL Xdebug] läses in. När en webbförfrågan skickas som har nyckeln [!DNL Xdebug] dirigeras den till en separat PHP-process som har [!DNL Xdebug] inläst.

Om du vill använda [!DNL Xdebug] specifikt i Pro-planens miljö för mellanlagring och produktion skapar du en separat SSH-tunnel och webbsession som du bara har tillgång till. Den här användningen skiljer sig från den vanliga åtkomsten, som bara ger dig åtkomst och inte till alla användare.

Du behöver följande:

- SSH-kommandon för åtkomst till miljöer. Du kan hämta den här informationen genom [[!DNL Cloud Console]](../project/overview.md) eller [!DNL Cloud Onboarding UI].
- Värdet `xdebug_key` som anges när du konfigurerar mellanlagrings- och Pro-miljöerna.

  `xdebug_key` kan hittas genom att använda SSH för att logga in på den primära noden och köra:

  ```bash
  cat /etc/platform/*/nginx.conf | grep xdebug.sock | head -n1
  ```

**Så här konfigurerar du en SSH-tunnel till en mellanlagrings- eller produktionsmiljö**:

1. Öppna en terminal.

1. Rensa alla SSH-sessioner för varje webbnod i klustret.

   ```bash
   ssh USERNAME@CLUSTER.ent.magento.cloud 'rm /run/platform/USERNAME/xdebug.sock'
   ```

1. Konfigurera SSH-tunneln för Xdebug för varje webbnod i klustret.

   ```bash
   ssh -R /run/platform/USERNAME/xdebug.sock:localhost:9000 -N USERNAME@CLUSTER.ent.magento.cloud
   ```

>[!NOTE]
>
>Så här hämtar du rätt värde för `USERNAME@CLUSTER.ent.magento.cloud`:
>- Metod 1: magento-cloud CLI: `magento-cloud ssh --all`
>- Metod 2: Commerce Console: https://CONSOLE-URL/ENVIRONMENT, klicka på listrutan `SSH v`

**Så här startar du felsökningen med URL:en för miljön**:

Detta är en demonstration av de konfigurationer som används samt en demonstration av GET-parametern för att starta en fjärrfelsökningssession.

>[!VIDEO](https://video.tv.adobe.com/v/3437417?learn=on)

1. Aktivera fjärrfelsökning. Gå till webbplatsen i webbläsaren och lägg till följande till URL:en där `KEY` är värde för `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_START=KEY
   ```

   I det här steget anges den cookie som skickar webbläsarbegäranden till utlösaren [!DNL Xdebug].

1. Slutför felsökningen med [!DNL Xdebug].

1. När du är redo att avsluta sessionen använder du följande kommando för att ta bort cookien och avsluta felsökningen via webbläsaren där `KEY` är värde för `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_STOP=KEY
   ```

   >[!NOTE]
   >
   >`XDEBUG_SESSION_START` som skickades av `POST`-begäranden stöds inte.

## Felsöka CLI-kommandon

I det här avsnittet beskrivs CLI-kommandona för felsökning.

Så här felsöker du CLI-kommandon:

1. SSH in på den server som du vill felsöka med CLI-kommandon.

1. Skapa följande miljövariabler:

   ```bash
   export XDEBUG_CONFIG='PHPSTORM'
   ```

   ```bash
   export PHP_IDE_CONFIG="serverName=<name of the server that is configured in PHPSTORM>"
   ```

   Dessa variabler tas bort när SSH-sessionen avslutas.

1. Starta felsökning

   Kör CLI-kommandot för att felsöka i Starter-miljöer och i Pro-integreringsmiljöer.
Du kan lägga till körningsalternativ, till exempel:

   ```bash
   php -d xdebug.profiler_enable=On -d xdebug.max_nesting_level=9999 bin/magento cache:clean
   ```

   I Pro Staging- och Production-miljöer måste du ange sökvägen till PHP-konfigurationsfilen [!DNL Xdebug] när du felsöker CLI-kommandon, till exempel:

   ```bash
   php -c /etc/platform/USERNAME/php.xdebug.ini bin/magento cache:clean
   ```

## Felsöka webbförfrågningar

Följande steg hjälper dig att felsöka webbförfrågningar.

1. Klicka på **Felsök** på menyn _Tillägg_ för att aktivera.

1. Högerklicka, välj alternativmenyn och ställ in IDE-tangenten på **PHPSTORM**.

1. Installera klienten [!DNL Xdebug] i webbläsaren. Konfigurera och aktivera den.

### Exempel: Chrome-konfiguration

I det här avsnittet beskrivs hur du använder [!DNL Xdebug] i Chrome med hjälp av tillägget [!DNL Xdebug] Helper. Information om [!DNL Xdebug]-verktyg för andra webbläsare finns i webbläsardokumentationen.

**Så här använder du Xdebug Helper med Chrome**:

1. Skapa en [SSH-tunnel](#ssh-access-to-xdebug-environments) till molnservern.

1. Installera [hjälptillägget för Xdebug](https://chromewebstore.google.com/detail/eadndfjplgieldjbigjakmdgkmoaaaoc) från Chrome Store.

1. Aktivera tillägget i Chrome enligt bilden nedan.

   ![Aktivera Xdebug-tillägget i Chrome](../../assets/xdebug/enable-chrome-ext.png)

1. Högerklicka på den gröna hjälpikonen i Chrome verktygsfält i Chrome.

1. Klicka på **Alternativ** på snabbmenyn.

1. Klicka på **PhpStorm** i listan _IDE-nyckel_.

1. Klicka på **Spara**.

   ![Hjälpalternativ för Xdebug](../../assets/xdebug/helper-options.png)

1. Öppna ditt PhpStorm-projekt.

1. Klicka på ikonen **Börja lyssna** i det övre navigeringsfältet.

   Om navigeringsfältet inte visas klickar du på **Visa** > **Navigeringsfält**.

1. Dubbelklicka på den PHP-fil som ska testas i PHpStorm-navigeringsrutan.

## Felsöka lokal kod

På grund av skrivskyddade miljöer måste du hämta kod till den lokala arbetsstationen från en miljö eller en specifik Git-gren för att kunna utföra felsökning.

Det är upp till dig att välja metod. Du har följande alternativ:

- Checka ut kod från Git och kör `composer install`

  Den här metoden fungerar om inte `composer.json` refererar till paket i privata databaser som du inte har åtkomst till. Den här metoden leder till att hela Adobe Commerce-kodbasen hämtas.

- Kopiera katalogerna `vendor`, `app`, `pub`, `lib` och `setup`

  Den här metoden gör att du får all kod som du kan testa. Beroende på hur många statiska resurser du har kan det resultera i en lång överföring med en stor mängd filer.

- Kopiera endast katalogen `vendor`

  Eftersom större delen av koden finns i katalogen `vendor` resulterar den här metoden troligen i bra testning, men testar inte hela kodbasen.

**Så här komprimerar du filer och kopierar dem till den lokala datorn**:

1. Använd SSH för att logga in i fjärrmiljön.

1. Komprimera filerna.

   ```bash
   tar -czf /tmp/<file-name>.tgz <directory list>
   ```

   Om du till exempel bara vill komprimera katalogen `vendor`:

   ```bash
   tar -czf /tmp/vendor.tgz vendor
   ```

1. I din lokala miljö använder du PhpStorm för att komprimera filerna.

   ```bash
   cd <phpstorm project root dir>
   ```

   ```bash
   rsync <SSH-URL>:/tmp/<file-name>.tgz .
   ```

   ```bash
   tar xzf <file-name>.tgz
   ```
