---
title: Bästa tillvägagångssätt för butikskonfiguration
description: Läs om de effektivaste strategierna för att konfigurera din butik på Adobe Commerce i molninfrastruktur.
feature: Cloud, Best Practices
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1087'
ht-degree: 0%

---

# Bästa tillvägagångssätt för butikskonfiguration

Mer information om hur du konfigurerar din butik, webbplatser och webbplatser finns i [Adobe Commerce användarhandbok](https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html). På den här sidan hittar du bästa praxis, användbar information och riktlinjer för hur du konfigurerar butiker, webbplatser med mera med ytterligare innehåll som ska publiceras över tid och över flera versioner.

## Marknadsföringskampanjer och -kampanjer

Den här informationen är användbar för Adobe Commerce om molninfrastruktur 2.1.X och 2.2.X.

Om du vill skapa kampanjer och kampanjer skapar du alternativen och inställningarna i [Content Staging](https://experienceleague.adobe.com/docs/commerce-admin/content-design/staging/content-staging.html). Med den här funktionen kan ni skapa och förhandsgranska era kampanjer innan de publiceras för kundförsäljning. Följande information är användbar. Exakta anvisningar finns i det länkade innehållet i Adobe Commerce Användarhandbok.

_Kampanjer_ är marknadsföringshändelser för säsongsförsäljning, nya produktrader och mycket mer. Varje kampanj kan innehålla anpassade teman, innehållsblock, widgetar som styr och visar innehåll samt tillhörande kampanjer med prisregler. Eftersom en kampanj är så omfattande kan du skapa dem med ett start- och slutdatum via Content Staging.

_Kampanjer_ ger rabatter, engångserbjudanden, kuponger, förstagångsrabatter med mera. Du skapar de här kampanjerna som _Prisregler_ som anger villkoren, rabatterna och alternativen för att uppmuntra kunderna att köpa. Du kan skapa prisregler på [kundvagnen](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart.html) eller [katalogen](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rules-catalog.html), med ytterligare alternativ för banners, belöningspoäng med mera. Ni kan planera kampanjer för era kampanjer och tillämpa prisregler för större event som en ny produktlinje eller säsongsförsäljning.

Här följer tips om hur du skapar, uppdaterar och hanterar kampanjer och kampanjer:

* En kampanj kan ingå i en kampanj. En kampanj kan inte ingå i en kampanj. Ni kan ha listor med kampanjer som prisregler som kan användas flera gånger, med flera kampanjer.
* När du skapar en kampanj skapas alltid en inaktiv inledande kampanj. Den har ett startdatum men inte ett slutdatum. Du kan ignorera den här inledande kampanjen. Du kan schemalägga en ny uppdatering med rätt kampanjschema och aktivera den.
* En kampanj har ett start- och slutdatum, inte en kampanj. Schemaläggaren som visas när du skapar en kampanj konfigurerar inte start- och slutdatum för kampanjen. Det gör att du kan schemalägga en kampanj för den här kampanjen när du är på kampanjens konfigurationssida.
* Du kan inte redigera direkt i mellanlagrat innehåll. Om du måste redigera inställningar och alternativ i kampanjen redigerar du originalet eller en replik och trycker för att skriva över i mellanlagrat innehåll. Om du till exempel inte anger ett slutdatum för en kampanj måste du redigera originalet och skicka för att uppdatera.

## Avancerade priser och mellanlagrat innehåll

Den här informationen är användbar för Adobe Commerce om molninfrastruktur 2.1.X och 2.2.X.

Vanligtvis kan du ange [Avancerade priser](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/pricing/pricing-advanced.html) för produkter genom området **Produkter** > **Kataloger** i Admin. Med Mellanlagrat innehåll utför du några extra steg för att lägga till priset i en kampanj.

Så här redigerar du avancerad prissättning och uppdaterar innehållsutjämning:

1. Logga in på Admin.
1. Navigera till **Produkter** > **Katalog** och markera en produkt och redigera den.
1. Välj **Avancerade priser** på fliken Priser. Redigera priset och spara ändringarna.
1. Klicka på **Schemalägg ny uppdatering** överst på sidan.
1. Skapa en kampanj för produkten.
1. Fyll i kampanjinformationen. Ange ett start- och slutdatum och en sluttid för Schemaläggaren.
1. Spara erbjudandet. En inaktiv inledande kampanj skapas.
1. Du kan förhandsgranska om du vill granska specialpriset, kampanjnamnet, det regelbundna priset och det schemalagda datumintervallet för kampanjen.

För ytterligare steg kan du fortsätta med instruktioner med [Schemalägg ändringar för katalogens prisregler](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rule-catalog-scheduled-changes.html). Klicka på **Nästa** för att gå igenom stegen.

## Prisregler

Prisreglerna kan innehålla logik och villkor som är lika gränslösa som er marknadsföringsfantasi. Exempel på populära exempel är Köp en kostnadsfri, Köp en och få en 50 % rabatt, en 25 dollar på beställningar över 100 dollar och mycket mer.

Om du vill skapa en prisregel läser du [Adobe Commerce användarhandbok](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rules-catalog-create.html).

Här nedan visas ett exempel på hur du skapar en prisregel för rabatt endast på första ordern. För den här rabatten vill du:

* Skapa en prisregel med ett [kundsegment](https://experienceleague.adobe.com/en/docs/commerce-admin/customers/segments/customer-segment-price-rule) med ett villkor: Totalt antal order under 1
* Lägg till det här kundsegmentet som ett villkor i kundvagnsregeln
* Valfritt - Lägg till villkor och regler för att tillämpa rabatterna på specifika SKU:er eller produktkategorier för fokuserade inköp

Detta garanterar att nya kunder eller befintliga kunder som inte har gjort ett köp får rabatten endast på sin första order. Du kan skapa banners och skicka e-postkampanjer för förstagångsköp.

## Butiksvyer

Du kan konfigurera och köra flera butiker med en enda implementering av Adobe Commerce i molninfrastrukturen. Se [Konfigurera flera webbplatser eller butiker](multiple-sites.md).

För butiker som inte interagerar med varandra kan du skapa flera _webbplatser_. Varje webbplats innehåller specifika artiklar, kunddata, utcheckning och kundvagn som inte delas med andra webbplatser i Adobe Commerce.

Varje webbplats kan innehålla en eller flera _butiker_ med olika kategorier och artiklar, delade kunddata, utcheckning och kundvagn. I dessa butiker kan kunden registrera sig och handla i olika produktkataloger med en enda utcheckning.

Du kan även skapa _butiksvyer_ för olika språk, layouter och designer. Varje vy kan ha en egen domän, ett eget varumärke och språk när artiklar, kunddata, utcheckning och kundvagn delas.

Här följer några exempel som bättre förklarar:

* En webbplats med en butik och två vyer för engelska och spanska. Alla artikeldata, kunder, utcheckningar och kundvagnar delas.

  ![Butiksexempel 1](../../assets/example-store1.png)

* En enda webbplats med en butik för kvinnors kläder innehåller två vyer: en för engelska och en för spanska. I butiken för barnkläder finns en butiksvy på engelska. Alla artikeldata, kunder, utcheckningar och kundvagnar delas. Butikerna kan ha olika domäner och teman.

  ![Exempel på butik 2](../../assets/example-store2.png)

* Två webbplatser en för kläder och en för heminredning med olika kataloger och separata artiklar, kunddata och kundvagn. Varje webbplats kan ha flera butiker och vyer som delar artiklar, kunddata, utcheckning och kundvagn endast inom den webbplatsen.

  ![Butiksexempel 3](../../assets/example-store3.png)

>[!WARNING]
>
>Katalogdata utökas allt eftersom du ökar antalet webbplatser och butiker. Beroende på din projektarkitektur kan de extra butikerna leda till en längre indexeringsprocess och längre svarstider för icke-cachelagrade katalogsidor. Adobe rekommenderar att du noga övervakar webbplatsens prestanda.
