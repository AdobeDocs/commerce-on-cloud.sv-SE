---
source-git-commit: 5236f2718e5091dfc2fc140e15d4ac87a073d1d0
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---
# Förimplementera kopplingar för bildoptimering

Den här katalogen innehåller förimplementerade kopplingar som automatiskt optimerar bilder innan de implementeras i databasen.

## Vad krokarna gör

- **Identifiera** mellanlagrade bildfiler automatiskt (PNG, JPG, JPEG, GIF, SVG)
- **Kör`image_optim`** för att komprimera och optimera bilder
- **Ordna om optimerade bilder** automatiskt
- **Kontrollera att alla implementerade bilder** är korrekt optimerade

## Fördelar

- Minskad databasstorlek
- Snabbare sidinläsning för dokumentation
- Enhetlig bildkvalitet för alla deltagare
- Ingen manuell optimering krävs

## Förutsättningar

- Ruby 3.0 eller senare
- Bundler
- Git

## Inställningar

### Automatisk installation (rekommenderas)

```bash
.githooks/setup-hooks.sh
```

### Manuell konfiguration

```bash
git config core.hooksPath .githooks
chmod +x .githooks/*
```

### Slutför projektkonfiguration

1. Klona databasen:

   ```bash
   git clone <repository-url>
   cd commerce-admin.en
   ```

2. Aktivera förimplementerade kopplingar:

   ```bash
   .githooks/setup-hooks.sh
   ```

3. Installera Jekyll-beroenden:

   ```bash
   cd _jekyll
   bundle install
   ```

## Testa krokarna

1. Lägga till en bildfil i databasen
2. Stega den: `git add <image-file>`
3. Försök att genomföra: `git commit -m 'test'`
4. Haken bör automatiskt optimera bilden

### Förväntade utdata

```bash
Found 1 staged image(s). Running optimization...
Optimizing: path/to/your/image.png
Re-staged optimized image: path/to/your/image.png
Image optimization complete!
```

## Bildriktlinjer

- **PNG**: Används för skärmbilder och gränssnittselement (optimeras automatiskt)
- **SVG**: Använd för ikoner och enkel grafik (optimering inaktiverat som standard)
- **JPEG**: Används för foton (optimeras automatiskt)
- **GIF**: Används för animeringar (optimeras automatiskt)

De förimplementerade böckerna optimerar automatiskt alla bilder vid implementering.

## Manuell optimering

För manuell bildoptimering:

```bash
cd _jekyll
bundle exec rake images:optimize path=../path/to/images
```

## Konfiguration

Hokarna använder konfigurationsfilen `_jekyll/.image_optim` för att anpassa optimeringsinställningarna:

- **PNG**: Använder `advpng`, `optipng` och `pngquant`
- **JPEG**: Använder `jhead`, `jpegoptim` och `jpegtran`
- **GIF**: Använder `gifsicle`
- **SVG**: SVG-optimering är inaktiverat som standard (kan bryta komplex vektorgrafik och animeringar)

## Felsökning

### Hook körs inte

- Kontrollera krokkonfiguration: `git config core.hooksPath`
- Kontrollera att krokfilen är körbar: `chmod +x .githooks/pre-commit`
- Kontrollera att du finns i rätt databas med katalogen `_jekyll`

### Optimeringsfel

- Verifiera att `bundle install` har körts i katalogen `_jekyll`
- Kontrollera att `image_optim` och `image_optim_pack` pärlor är installerade
- Granska konfigurationsfilen `.image_optim`

### Prestandaproblem

- Justera antalet trådar i `_jekyll/.image_optim`
- Ange miljövariabeln `DEBUG=1` för detaljerad felinformation

## Så här fungerar det

1. **Pre-commit trigger**: När du kör `git commit` körs kroken automatiskt
2. **Bildidentifiering**: Söker igenom mellanlagrade filer efter bildtillägg
3. **Optimering**: Kör `image_optim` på varje mellanlagrad bild
4. **Mellanlagring**: Lägger automatiskt till optimerade bilder i mellanlagringsområdet igen
5. **Genomför fortsätter**: Om optimeringen lyckas fortsätter implementeringen normalt

## Bildformat som stöds

- **PNG** (`.png`) - förlustfri och förstörande komprimering
- **JPEG** (`.jpg`, `.jpeg`) - Förstörande komprimering med rensning av metadata
- **GIF** (`.gif`) - Animering och statisk optimering
- **SVG** (`.svg`) - Vektoroptimering (inaktiverad som standard)

## God praxis

1. **Testa kroken**: Prova att implementera en liten bild först för att se till att den fungerar
2. **Granska ändringar**: Kontrollera Git-differensen för att se optimeringsresultat
3. **Bildskärmsprestanda**: Det kan ta tid att optimera stora bilder
4. **Versionskontroll**: Anrop lagras i den här `.githooks/`-katalogen

## Support

Om du har problem med krokarna före implementering:

1. Kontrollera krokutdata för felmeddelanden
2. Kontrollera att din `image_optim`-konfiguration fungerar
3. Testa med de manuella penselåtgärderna först
4. Granska krockloggarna och konfigurationen
5. Kontrollera krokkonfigurationen: `git config core.hooksPath`
