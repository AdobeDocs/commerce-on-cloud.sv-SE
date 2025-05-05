---
title: Hantera grenar med  [!DNL Cloud Console]
description: Lär dig hur du hanterar miljögrenarna för Adobe Commerce i molninfrastrukturen med  [!DNL Cloud Console].
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# Hantera grenar med [!DNL Cloud Console]

Du kan hantera dina miljöer med [!DNL Cloud Console] eller `magento-cloud` CLI. Dina projektfiler lagras i en Git-databas. Du kan använda Git-kommandon för att hantera koden, men CLI:n för `magento-cloud` är utformad för att interagera med plattformsfunktioner, vilket Git-kommandona inte gör. Se [Git-kommandon](../dev-tools/cloud-cli-overview.md#git-commands) i CLI-avsnittet i molnet.

I det här avsnittet beskrivs hur du använder [!DNL Cloud Console] för att:

- Lägga till eller ta bort en miljö
- Synkronisera (`git pull`) från den överordnade miljön
- Koppla (`git push`) till den överordnade miljön

>[!TIP]
>
>Du kan inte skapa grenar från Pro Staging- och Production-miljöer. Du kan förgrena från grenen `master`.

## Skapa en miljö

Förgreningsstrategin använder ett gemensamt Git-arbetsflöde där du utvecklar kod och lägger till tillägg i en utvecklingsgren. Se [Starter](../architecture/starter-architecture.md) och [Pro](../architecture/starter-develop-deploy-workflow.md) - översikter över arkitekturen.

- För Starter skapar du en `staging`-gren från grenen `master` och sedan grenar från `staging` för utveckling.
- Skapa en utvecklingsgren från miljön `Integration` för Pro.

Ditt konto har stöd för ett begränsat antal ![aktiva grenar](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} (inaktiva) utvecklingsgrenar. Hantera aktiva och inaktiva grenar genom att lägga till eller ta bort en gren med endast [!DNL Cloud Console] eller molnbaserad CLI. Innan du kan ta bort en gren inaktiverar du grenen, som finns kvar i listan _Miljöer_ som _inaktiv_. Du kan återaktivera grenen senare eller så kan du [ta bort grenen](../dev-tools/cloud-cli-overview.md#) i miljöinställningarna eller använda molnet-CLI.

Om du behöver ytterligare aktiva miljöer för utveckling skickar du en [supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

**Lägga till en gren**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Välj ett projekt i listan _Alla projekt_.

1. Välj en miljö.

   >[!TIP]
   >
   >Din nya gren klonas från den här miljön. Välj en överordnad miljö som liknar den miljö du ska skapa.

1. Klicka på **[!UICONTROL Branch]**.

   ![Skapa en gren](../../assets/button-branch.png){width="150"}

1. I formuläret _Förgreningar från ..._ anger du ett förgreningsnamn.

   Miljön _name_ skiljer sig bara från miljön _ID_ om du använder blanksteg eller versaler i miljönamnet. Ett miljö-ID består av alla gemener, siffror och tillåtna symboler. Versaler i ett miljönamn konverteras till gemener i ID:t. Blanksteg i ett miljönamn konverteras till streck.

   Miljönamnet **får inte** innehålla tecken som är reserverade för ditt Linux-skal eller för reguljära uttryck. Otillåtna tecken är klammerparenteser (`{ }`), parenteser, asterisk (`*`), vinkelparenteser (`>`), et-tecken (`&`), procent (<code>%)</code>) och andra tecken.

1. Välj en **[!UICONTROL Environment type]**.

1. Klicka på **[!UICONTROL Create Branch]**.

1. Vänta medan miljön distribueras.

   Under distributionen är miljöstatusen **Pågår**. När distributionen är klar ändras statusen till en grön bock för **success**.

## Skapa inaktiv gren

Du kan inte skapa en inaktiv gren från Adobe Commerce Cloud-konsolen eller CLI. Om du vill skapa en inaktiv gren skapar du den i Git-databasen och skickar den med alternativet `environment.Parent` i kommandot.

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## Ta bort en miljö

Innan du kan ta bort en miljö måste du inaktivera den. När en miljö är inaktiv kan du ta bort den.

**Så här inaktiverar du en miljö**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Välj ett projekt i listan _Alla projekt_.

1. Välj miljö i listan _Miljö_ i navigeringsfältet.

1. Klicka på konfigurationsikonen till höger i det övre navigeringsfältet, som öppnar miljöinställningarna.

1. Bläddra nedåt till avsnittet _[!UICONTROL Deactivate environment]_&#x200B;på fliken&#x200B;_[!UICONTROL General]_, klicka på **[!UICONTROL Deactivate environment and delete data]** och följ instruktionerna.

## Synkronisera en miljö

Synkronisering av en miljö (eller gren) är samma som `git pull origin <parent>`. Du kan synkronisera uppdaterad kod från en överordnad miljö. Du kan använda den här funktionen via [!DNL Cloud Console] för alla Starter- och Pro-miljöer.

För Pro-planen kan du synkronisera från mellanlagring och produktion till din `master`-gren. Synkroniseringen hämtar och push-tar bara fram kod, inte data. Om du vill synkronisera data dumpar du databasdata och överför dem till en annan miljös databas. Se [Migrera och distribuera statiska filer och data](/help/cloud-guide/deploy/staging-production.md#migrate-static-files).

**Synkronisera en miljö**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Välj ett projekt i listan _Alla projekt_.

1. Klicka på namnet på grenen som ska synkroniseras i miljölistan.

1. Klicka på (synkronisera).

   ![Synkronisera en miljö](../../assets/button-sync.png){width="150"}

1. Markera de objekt som ska synkroniseras.

   - Ersätt data (data och filer) synkroniserar ändringar i databasen och innehållsfilerna från den överordnade grenen.
   - Sammanfoga - (kod) synkroniserar uppdaterad kod från den överordnade grenen.

   Detta skapar också ett CLI-kommando som du kan kopiera och använda.

1. Klicka på **Synkronisera**.

## Sammanfoga med överordnad miljö

Sammanfogning av en miljö (eller gren) är samma som `git push origin`. Du sammanfogar för att skicka uppdaterad kod från en miljö till dess överordnade miljö. Du kan sammanfoga koden med `master`. Du kan distribuera till mellanlagring och produktion med kommandot `merge`.

**Så här sammanfogar du med den överordnade miljön**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Välj ett projekt i listan _Alla projekt_.

1. Klicka på namnet på grenen som ska sammanfogas i miljölistan.

1. Klicka på (sammanfoga).

   ![Sammanfoga en miljö](../../assets/button-merge.png){width="150"}

1. Klicka på **Sammanfoga** och bekräfta åtgärden.

## Visa loggar

Genom [!DNL Cloud Console] kan du granska olika loggar för miljöer, inklusive historik för bygge, distribution och distribution.

För **Starter** kan du granska bygg- och distributionsloggar och distributionshistoriken. De här miljöerna innehåller grenen `master` (produktion) och alla grenar som skapats av den.

För **Pro** kan du granska följande loggar i varje miljö:

- Integration - Skapa, driftsätt och driftsättningshistorik
- Mellanlagring - Bygg loggar och driftsättningshistorik. Använd SSH för att logga in på servern för att visa distributionsloggar.
- Produktion - Bygg loggar och driftsättningshistorik. Använd SSH för att logga in på servern för att visa distributionsloggar.

**Så här visar du loggar i[!DNL Cloud Console]**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Välj ett projekt i listan _Alla projekt_.

1. Välj en miljö.

   Miljövyn innehåller en [aktivitetslista](activity-stream.md) som visar _senaste_ händelser, en post per åtgärd som gjorts, inklusive synkroniseringar, sammanfogningar, grenar, säkerhetskopieringar med mera. Klicka på **Alla** om du vill se den fullständiga distributionshistoriken.

1. Om du vill visa byggloggen väljer du länken Slutfört eller Fel per distributionspost på kontot.

>[!TIP]
>
>Klicka på ikonen **Filtrera efter** för en nedrullningsbar lista och välj den typ av meddelanden som ska visas.

## Hämta kod från en privat Git-databas

Ditt Adobe Commerce i molninfrastrukturprojekt kan innehålla kod från en privat Git-databas. Du kan till exempel ha kod för en anpassad modul eller tema i en privat rapport. Om du vill göra det måste du lägga till projektets offentliga SSH-nyckel i din privata Git-databas och uppdatera projektfilen `composer.json`.

Om du vill lägga till en distributionsnyckel i din privata GitHub-databas måste du vara administratör för den databasen. Med GitHub kan du bara använda en distributionsnyckel för en databas.

Om du föredrar att ditt projekt har åtkomst till flera databaser kan du bifoga en SSH-nyckel till ett automatiskt användarkonto. Eftersom det här kontot inte används av en människa kallas det för en [datoranvändare](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys). Lägg till datorkontot som medarbetare eller lägg till datoranvändaren i ett team med åtkomst till databaserna.

>[!INFO]
>
>Adobe rekommenderar att du lägger till och sammanfogar den här koden i dina Git-projektdatabaser. Om du inte konfigurerar anslutningen kan du få problem med bygget.

**Så här hittar du den offentliga SSH-nyckeln**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Välj ett projekt i listan _Alla projekt_.

1. Klicka på konfigurationsikonen till höger i det övre navigeringsfältet.

1. Klicka på **[!UICONTROL Deploy Key]** i _Projektinställningar_.

1. Kopiera distributionsnyckeln till Urklipp och använd den på något av följande Git-baserade sätt:

>[!BEGINTABS]

>[!TAB GitHub]

### Ange din GitHub-distributionsnyckel

På GitHub är distributionsnycklar skrivskyddade som standard.

**Så här anger du din offentliga projektnyckel som en GitHub-distributionsnyckel**:

1. Logga in som administratör i din GitHub-databas.
1. Klicka på fliken **[!UICONTROL Settings]** i databasen.

   >[!NOTE]
   >
   >Om du inte ser det här alternativet är du inte inloggad som databasadministratör och du kan inte slutföra den här uppgiften. Be din GitHub-databasadministratör att göra detta.

1. Klicka på **[!UICONTROL Deploy Keys]** på fliken _Inställningar_ i den vänstra navigeringen.
1. Klicka på **[!UICONTROL Add deploy key]**.
1. Följ anvisningarna.

Använd formatet `<user>@<host>:<.git</code>` i `composer.json` eller `ssh://<user>@<host>:<port>/<path>.git` om du använder en port som inte är standard.

>[!TAB Bitbucket]

### Ange en distributionsnyckel för Bitbucket

**Så här anger du projektets offentliga nyckel som en Bitbucket-distributionsnyckel**:

1. Logga in som administratör i Bitbucket-databasen.
1. Klicka på **[!UICONTROL Settings]** i den vänstra navigeringen.
1. Klicka på Allmänt > **[!UICONTROL Deployment Keys]**.
1. Klicka på **[!UICONTROL Add Key]**.
1. Följ anvisningarna.

>[!TAB GitLab]

### Ange din GitLab-distributionsnyckel

**Så här lägger du till den offentliga SSH-nyckeln för ditt projekt som en GitLab-distributionsnyckel**:

1. Logga in som ägare i din GitLab-databas.
1. Kontrollera att alternativet _Pipelines_ är aktiverat för ditt projekt:

   1. Utöka avsnittet **[!UICONTROL Visibility, project, features, permissions]** i projektinställningarna.
   1. Om det behövs klickar du på **[!UICONTROL Pipelines]** för att aktivera alternativet.

1. Lägg till den offentliga SSH-nyckeln i CI/CD-inställningarna.

   1. Klicka på Inställningar > **[!UICONTROL CI / CD]** i den vänstra navigeringen.
   1. Klicka på Distribuera nycklar **Utöka** för att konfigurera nyckeln.
   1. I formuläret _Distribuera nyckel_ lägger du till ett distribueringsnyckelnamn i fältet **[!UICONTROL Title]** och klistrar in den offentliga SSH-nyckeln i fältet **[!UICONTROL Key]**.
   1. Klicka på **[!UICONTROL Add Key]** för att spara konfigurationen.

>[!ENDTABS]

## Säkra miljöer och grenar

Du kan komma åt ditt projekt och dina miljöer från vilken plats som helst via en webbläsare med hjälp av [!DNL Cloud Console]. Du kan ha säkerhetsinställningar för din produktionsmiljö, dina butiker och webbplatser. I det här avsnittet får du hjälp att skydda integrerings- och mellanlagringsmiljöer för enbart utvecklare, DBA:er med mera.

>[!WARNING]
>
>**Använd INTE** följande metoder för att skydda Pro Staging- och Production-miljöer. Det här avbryter cachelagring. Använd funktionen [Blockering](../cdn/fastly-vcl-blocking.md) som finns i snabbnätverket för CDN för Adobe Commerce.

**För säkra miljöer**:

1. Logga in på [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Välj ett projekt i listan _Alla projekt_.

1. Välj en miljö och klicka på konfigurationsikonen i navigeringsfältet.

1. På fliken _Allmänt_ i miljöinställningarna klickar du på **PÅ** för **[!UICONTROL HTTP access control enabled]** för att aktivera säker åtkomst. Du kan välja mellan autentiseringsuppgifter och IP-adresser att filtrera för åtkomst.

1. Om du vill filtrera efter autentiseringsuppgifter klickar du på **[!UICONTROL Add Login]**, anger ett användarnamn och lösenord och klickar på **[!UICONTROL Add Login]** för att lägga till.

1. Om du vill filtrera efter IP-adress anger du IP-adresserna i en lista med `deny` eller `allow`. Exempel:

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. Klicka på **[!UICONTROL Save]**. Miljön distribueras om för att uppdatera säkerhet och inställningar. Adobe rekommenderar att du testar miljön när du har slutfört säkerhetsinställningarna.
