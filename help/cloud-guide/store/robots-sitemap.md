---
title: Lägg till webbplatskarta och sökrobotar
description: Lär dig hur du lägger till webbplatskartor och sökrobotar i Adobe Commerce i molninfrastruktur.
feature: Cloud, Configuration, Search, Site Navigation
exl-id: 060dc1f5-0e44-494e-9ade-00cd274e84bc
source-git-commit: 1d52481fb6874f3a9ba14b0ff4fe39dc7d564938
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 0%

---

# Lägg till webbplatskarta och sökrobotar

Ett försök att generera och skriva filen `sitemap.xml` till rotkatalogen resulterar i följande fel:

```
Please make sure that "/" is writable by the web-server.
```

Med Adobe Commerce i molninfrastruktur kan du bara skriva till specifika kataloger, som `var`, `pub/media`, `pub/static` eller `app/etc`. När du genererar filen `sitemap.xml` med hjälp av administratörspanelen måste du ange sökvägen `/media/`.

Du behöver inte generera en `robots.txt`-fil eftersom den genererar `robots.txt`-innehållet på begäran och lagrar det i databasen. Du kan visa innehållet i webbläsaren med länken `<domain.your.project>/robots.txt` eller `<domain.your.project>/robots`.

För detta krävs ECE-Tools version 2002.0.12 och senare med en uppdaterad `.magento.app.yaml`-fil. Se ett exempel på dessa regler i [magento-cloud-databasen](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49).

**Så här skapar du en `sitemap.xml`-fil i version 2.2 och senare**:

1. Gå till Admin.
1. På menyn _Marknadsföring_ klickar du på **Webbplatskarta** i avsnittet _SEO &amp; Search_ .
1. Klicka på _Lägg till platskarta_ i vyn **Platskarta**.
1. I vyn _Ny platskarta_ anger du följande värden:

   - **Filnamn**:`sitemap.xml`
   - **Sökväg**:`/media/`

1. Klicka på **Spara och generera**. Den nya webbplatskartan blir tillgänglig i rutnätet _Platskarta_.
1. Klicka på sökvägen i kolumnen _Länk för Google_.

**Så här lägger du till innehåll i `robots.txt`-filen**:

1. Gå till Admin.
1. Klicka på _Konfiguration_ i avsnittet **Design** på menyn _Innehåll_.
1. I vyn _Designkonfiguration_ klickar du på **Redigera** för webbplatsen i kolumnen _Åtgärd_ .
1. Klicka på _Sökmotorrobotar_ i vyn **Huvudwebbplats**.
1. Uppdatera **Redigera anpassad instruktion för robots.txt**-fältet.
1. Klicka på **Spara konfiguration**.
1. Verifiera `<domain.your.project>/robots.txt`-filen eller `<domain.your.project>/robots`-URL:en i webbläsaren.

>[!NOTE]
>
>Om filen `<domain.your.project>/robots.txt` genererar en `404 error`, [skickar du en Adobe Commerce-supportanmälan](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=sv-SE#submit-ticket) för att ta bort omdirigeringen från `/robots.txt` till `/media/robots.txt`.

## Skriv om med snabbVCL-kodfragment

Om du har olika domäner och behöver separata webbplatskartor kan du skapa en VCL för att dirigera till rätt platskarta. Generera filen `sitemap.xml` på Admin-panelen enligt beskrivningen ovan och skapa sedan ett anpassat Fast VCL-fragment för att hantera omdirigeringen. Se [Anpassade snabbt VCL-fragment](../cdn/fastly-vcl-custom-snippets.md).

>[!NOTE]
>
> Du kan överföra anpassade VCL-kodfragment från administratörsgränssnittet eller med hjälp av API:t Snabb. Se [Exempel på anpassade VCL-kodfragment och självstudiekurser](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code).

### Använd ett fast VCL-fragment för omdirigering

Skapa ett anpassat VCL-fragment för att skriva om sökvägen för `sitemap.xml` till `/media/sitemap.xml` med nyckelvärdepar för `type` och `content`.

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

I följande exempel visas hur du skriver om sökvägen för `robots.txt` och `sitemap.xml` till `/media/robots.txt` och `/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**Så här använder du ett fast VCL-fragment för en viss domänomdirigering**:

Skapa en `pub/media/domain_robots.txt`-fil, där domänen är `domain.com`, och använd nästa VCL-fragment:

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

VCL-fragmentet dirigerar `http://domain.com/robots.txt` och visar `pub/media/domain_robots.txt`-filen.

Om du vill konfigurera en omdirigering för `robots.txt` och `sitemap.xml` i ett enskilt fragment skapar du `pub/media/domain_robots.txt` - och `pub/media/domain_sitemap.xml` -filer, där domänen är `domain.com`, och använder nästa VCL-fragment:

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

I administratörskonfigurationen för `sitemap` måste du ange filens plats med `pub/media/` i stället för `/`.

### Konfigurera indexering med sökmotor

Om du vill aktivera `robots.txt` anpassningar i produktionen aktiverar du indexering av sökmotorer för alternativet `<environment-name>` i projektinställningarna på molnkonsolen:

- Äldre molnkonsol - URL:en följer mönstret `https://<region-id>.magento.cloud/projects/<project_id>`

  Växla inställningen [!UICONTROL Indexing by search engines] (äldre konsol) [!UICONTROL Hide from search engines] (Adobe Console) till **På**.

  ![Använd [!DNL Cloud Console] för att hantera miljöer](../../assets/robots-indexing-by-search-engine.png)

- Adobe Cloud Console - URL:en följer mönstret ``https://console.adobecommerce.com/<username>/<project_id>``

  Avmarkera inställningen [!UICONTROL Hide from search engines].

- Du kan även använda magento-cloud CLI för att uppdatera den här inställningen:

  ```bash
  magento-cloud environment:info -p <project_id> -e production restrict_robots false
  ```

>[!NOTE]
>
>- Indexering med sökmotorer kan bara aktiveras i Produktion, men inte i någon av de lägre miljöerna.
>
>- Om du använder PWA Studio och inte kan komma åt den konfigurerade `robots.txt`-filen lägger du till `robots.txt` i Tillåtelselista [Front Name](https://github.com/magento/magento2-upward-connector#front-name-allowlist) på **Store** > Configuration > **General** > **Web** > UPWARD PWA Configuration.

