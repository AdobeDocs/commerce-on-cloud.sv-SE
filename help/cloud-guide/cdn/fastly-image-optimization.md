---
title: Snabb bildoptimering
description: Lär dig hur du optimerar bildhanteringen och förenklar bildhanteringen för Adobe Commerce webbplats genom att aktivera och konfigurera Snabb optimering av bilder.
feature: Cloud, Configuration, Media
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# Snabb bildoptimering

Snabb bildoptimering (Fast IO) ger bildredigering i realtid och optimering för snabbare bildleverans och enklare underhåll av bildkällorna för responsiva webbapplikationer. När Fast IO har konfigurerats har den följande funktioner för bildoptimering:

- Tvinga förlustkonvertering
- Djupgående bildoptimering
- Adaptiva pixelproportioner
- Stöd för vanliga bildformat: PNG, JPEG, GIF och WebP

Innan du aktiverar och konfigurerar alternativet för snabb IO måste du konfigurera tjänsten Snabb och konfigurera origin-skydd.

Baserat på dina konfigurationsinställningar infogar kodavsnittet Fast Image Optimization (Fast IO) VCL-koden för att utföra den bildoptimering som snabbar upp produktleveransen i butiken. Det finns tre steg för att konfigurera IO-funktionen snabbt: Aktivera, Konfigurera och Verifiera.

## Aktivera snabb IO

Aktivera snabb bildoptimering (Snabb IO) från panelen Admin genom att ladda upp VCL-kodfragmentet Fast IO. Utdraget innehåller snabbkonfigurationsinstruktioner för att bearbeta alla bilder med hjälp av bildoptimerare, med hjälp av standardkonfigurationer.

**Förutsättningar:**

- Installera eller uppgradera till Snabb modulversion 1.2.62 eller senare
- [Konfigurera skärm med fast startpunkt och serverdel](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding)

**Så här aktiverar du snabb IO**:

1. Logga in som administratör på din lokala [Admin](../../get-started/onboarding.md#access-your-admin-panel) -panel.

1. Välj **Lagrar** > **Inställningar** > **Konfiguration** > **Avancerat** > **System**.

1. Expandera **Helsidescache** i den högra rutan.

1. Välj **Snabb konfiguration** > **Bildoptimering** för att ange konfigurationsinställningarna.

1. I fältet _Snabbt IO-kodfragment_ väljer du **Aktivera/inaktivera**.

1. Ladda upp IO-kodutdraget Fast:

   - Välj **Standardalternativ för I/O-konfiguration** för att öppna sidan med standardkonfigurationsalternativ för bildoptimering.
   - Välj **Överför** om du vill överföra VCL-fragmentet till servern.

## Konfigurera IO snabbt

Granska och uppdatera standardinställningarna för I/O-konfiguration för bildoptimering efter behov. Du kan till exempel ändra WebP- och JPEG-kvalitetsnivåerna för förstörande format, eller ändra formatet för visning av JPEG-bilder till _Progressiv_ eller _Baslinje_. Du kan även använda Fast IO för mer detaljrika bildoptimeringsfunktioner, som:

- Tvinga förlustkonvertering
- Djupgående bildoptimering
- Adaptiva pixelproportioner

**Så här uppdaterar du snabbt IO**:

1. Välj **Konfigurera** på sidan _Snabb konfiguration_ i fältet _Standardalternativ för I/O-konfiguration_.

   ![Visa IO-konfigurationsinställningarna snabbt](../../assets/cdn/fastly-io-default-config.png)

1. Granska och uppdatera IO-konfigurationsinställningarna snabbt på sidan _Standardkonfigurationsalternativ för bildoptimering_:

   ![Granska IO-konfigurationen snabbt](../../assets/cdn/fastly-io-config-options.png)

   - **Automatisk WebP?** - lämna standardinställningen (`Yes`) för att konvertera bilder till WebP-format i webbläsare som stöder det. Om du ändrar inställningen till **Nej** används bildfiltypen i stället för att bilden konverteras till WebP-format.

   - **Standardkvalitet för WebP (förstörande)** - lämna standardinställningen (`85`) eller skriv in komprimeringsnivån för förstörande filformaterade bilder. Du kan ange ett heltal mellan 1 och 100.

   - **Standardformatkontroller för JPEG** - låt standardinställningen (`Auto`) stå kvar, eller välj den JPEG-typ som ska användas när en bild visas. Om värdet är inställt på _Auto_ levereras bilder med den utdatatyp som matchar indatatypen fast. Välj _Baslinje_ om du vill visa bilderna rad för rad från det övre vänstra hörnet till det nedre högra hörnet. Välj _Progressiv_ om du vill visa en suddig bild som blir tydlig när den läses in.

   - **Standardkvalitet för JPEG** - lämna standardinställningen (`85`) eller skriv in komprimeringsnivån för kvalitet på förlustfilformat. Ange ett heltal mellan 1 och 100.

   - **Tillåt uppskalning?** - lämna standardinställningen (`No`), eller välj `Yes` om du vill att bilder som är större än originalkällfilen ska returneras så att de får plats med de begärda dimensionerna.

   - **Ändra storlek på filter** - behåll standardinställningen (`Lancsoz3`) eller välj ett alternativ. Den här inställningen anger vilket filter som används för att skapa en storleksändrad bild. Beroende på vilket filter som är markerat kan den storleksändrade bilden ha ett högre eller lägre antal pixlar.

      - `Lanczos3` (standard) - Ger bilden med bäst kvalitet. Det ökar möjligheten att identifiera kanter och linjära funktioner i en bild och använder _[!DNL sinc]_-omsampling för att ge bästa möjliga rekonstruktion.
      - `Lanczos2` - Använder samma filter som `Lancsoz3` men med en mindre exakt uppskattning av omsamplingsfunktionen _[!DNL sinc]_.
      - `Bicubic` - Har en naturlig skärpeeffekt när en bild blir mindre.
      - `Bilinear` - Har en naturlig utjämningseffekt när du gör en bild större.
      - `Nearest` - Har en naturlig pixelförvandlingseffekt när du ändrar storlek på pixelbilder.

1. När du har angett IO-konfigurationsinställningar för tjänsten Snabbt väljer du **Avbryt** för att återgå till inställningarna för snabb konfiguration.

1. I konfigurationsfältet _Aktivera djupbildsoptimering_ i Bildoptimering väljer du **Ja** för att aktivera djupbildsoptimering.

   ![Aktivera snabb I/O-optimering av bilder](../../assets/cdn/fastly-io-deep-image-config.png)

   Djupbildsoptimering är inaktiverat som standard. När den här funktionen är aktiverad inaktiveras den inbyggda storleksförändringsfunktionen i Adobe Commerce och storleksändringen avlastas till tjänsten Snabbt IO. Bildoptimering gäller endast produktbilder. Storleken på CMS-bilder ändras inte. Se [Snabbt dokumentation](#deep-image-optimization).

1. När du har aktiverat djupbildsoptimering aktiverar du funktionen [adaptiva pixelproportioner](#adaptive-pixel-ratios) för att generera bilder som är optimerade för användning på responsiva webbplatser.

   ![Aktivera IO-adaptiva pixelproportioner snabbt](../../assets/cdn/fastly-io-config-adaptive-pixel.png)

   - Välj **Ja** i fältet _Aktivera pixelproportioner för adaptiva enheter_.
   - Acceptera standardinställningen i fältet _Enhetspixelproportioner_ eller markera kryssrutan **Systemindata** om du vill ta bort inställningen. Välj sedan önskat förhållande. En högre inställning för pixelproportioner för enheten ger större bilder.

1. Välj **Spara konfiguration**.

### Tvinga förlustkonvertering

Som standard tvingar tjänsten Fast IO konvertering av förlustfria format som PNG, BMP eller WEBP till JPEG/WEBP-format.

Fördelen med att tvinga fram förlustkonvertering är att mindre bilder används.
Om du till exempel använder formaten JPEG eller WEBp i stället för PNG kan storleken minskas med 60 till 70 procent beroende på den kvalitetsnivå som anges i Snabb IO-konfiguration.

Beroende på vilken kvalitetsnivå som valts för bildoptimering kan du uppleva skillnader i bild. Kanal/genomskinlighet för Alpha tas till exempel bort och ersätts med en vit bakgrund, om du inte använder Djup bildoptimering som använder bakgrundsfärgen för temat.

Om du inaktiverar förlustkonvertering (`WebP Auto? = No`) ändras JPEG-bilder endast till WEBP-format för kompatibla webbläsare. Inga andra bildtyper ändras. Om den ursprungliga bilden till exempel är PNG är utdata från tjänsten Snabb IO PNG.

### Djupgående bildoptimering

Djupbildsoptimering är inaktiverat som standard. Om du aktiverar det här alternativet inaktiveras den inbyggda storleksändringen i Adobe Commerce och den avlastas helt till tjänsten Fast IO.
Den här funktionen ändrar bara storlek på _product_-bilder. Storleken på CMS-bilder ändras inte.

Om du aktiverar djupbildsoptimering läggs en bakgrundsfärgdefinition till i alla bilder som definieras i ditt tema. Detta resulterar i att WebP-bilder växlas från förlustfria WebP-bilder till förlustfria WebP-bilder. En av de största skillnaderna mellan förlustfri och förstörande är att alfakanalen tas bort från PNG-bilder, som innehåller mycket mindre bilder. Bilder med genomskinlighet kan dock se udda ut på produkt- och kampanjsidor som använder en annan bakgrund.

Följande kod representerar till exempel den ursprungliga källan för en bild från Luma-temat:

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/cache/f073062f50e48eb0f0998593e568d857/m/b/mb02-gray-0.jpg"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

När funktionen för snabb IO-djup-optimering är aktiverad skrivs den ursprungliga källkoden för bilden om enligt följande exempel:

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

### Adaptiva pixelproportioner

Funktionen Adaptiva pixelproportioner är användbar för att optimera bilder för progressiva webbprogram. Du kan leverera flera bildstorlekar och upplösningar från en bildkällfil genom att lägga till en `srcset` för varje produktbild.

När funktionen Adaptiva pixelproportioner är aktiverad levererar tjänsten Snabb IO en bild med fast bredd som kan anpassas till varierande `device-pixel-ratios`.
Tjänsten ändrar till exempel produktbilddefinitionen så som visas i följande exempel:

```html
<img class="product-image-photo"
     srcset="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=2 2x,
  https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=3 3x"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

Se `srcset` [webbläsarstöd](https://caniuse.com/#feat=srcset) och [specifikation](https://html.spec.whatwg.org/multipage/embedded-content.html#attr-img-srcset).

## Validera IO snabbt

När du har aktiverat och konfigurerat Snabbt IO validerar du konfigurationen genom att utföra hastighetstester på webbsidor med och utan Fast IO aktiverat. Granska även bilderna i din butik för att kontrollera bildstorlek och utseende.
