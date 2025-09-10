---
source-git-commit: b151aac666510594751937e80dc3d9db4ede41b7
workflow-type: tm+mt
source-wordcount: '815'
ht-degree: 1%

---
# Adobe Commerce on Cloud Infrastructure

Den här webbplatsen innehåller den senaste utvecklardokumentationen för Commerce om molninfrastruktur.

- [Commerce on Cloud Infrastructure Guide](https://experienceleague.adobe.com/sv/docs/commerce-on-cloud/user-guide/overview)
- [Kom igång med Commerce](https://experienceleague.adobe.com/sv/docs/commerce-on-cloud/start/overview) i molninfrastrukturen

## Adobe Open Source - uppförandekod

Det här projektet har antagit [Adobe Open Source Code of Conduct](code-of-conduct.md). Mer information finns i artikeln [Contributing](contributing.md).

## Om dina bidrag till Adobe-innehåll

Se [Adobe Docs Contributor Guide](https://experienceleague.adobe.com/sv/docs/contributor/contributor-guide/introduction).

Hur du bidrar beror på vem du är och vilken typ av ändringar du vill bidra med:

### Mindre ändringar

Om du bidrar med mindre uppdateringar kan du besöka artikeln och klicka på feedbackområdet som visas längst ned i artikeln, klicka på **Detaljerade feedbackalternativ** och sedan på **Föreslå en redigering** för att gå till markeringskällfilen på GitHub. Använd GitHub-gränssnittet för att göra uppdateringar.

Mindre korrigeringar och förtydliganden som du lämnar in för dokumentation och kodexempel i den här rapporten omfattas av Adobe användarvillkor.

### Större ändringar eller nya artiklar från communitymedlemmar

Om du är en del av Adobe-communityn och vill skapa en ny artikel eller skicka större ändringar använder du fliken Problem i Git-databasen för att skicka in ett problem för att starta en konversation med dokumentationsteamet. När du har gått med på en plan måste du arbeta tillsammans med en anställd för att få in det nya innehållet genom en kombination av arbete i det offentliga och privata arkivet.

### Större förändringar för Adobe medarbetare

Om du är teknikskribent, programchef eller utvecklare för en Adobe Experience Cloud-lösning och det är ditt jobb att bidra till eller skapa tekniska artiklar, bör du använda den privata databasen på `https://git.corp.adobe.com/AdobeDocs`.

## Verktyg och inställningar

Deltagare i communityn kan använda GitHub-gränssnittet för grundläggande redigering eller förgrena rapporten för att göra större insatser.

Mer information finns i [Adobe Docs Contributor Guide](https://experienceleague.adobe.com/sv/docs/contributor/contributor-guide/introduction).

## Så här använder du kod för att formatera ämnet

Alla artiklar i den här databasen använder smaksatt GitHub-kod. Om du inte är van vid att markera något läser du:

- [Grunderna för markeringar](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [Utskrivbart kalkylblad för markering](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

## Mallar

För vissa ämnen använder vi datafiler och mallar för att generera publicerat innehåll. Användningsexempel för den här metoden är:

- Publicera stora mängder programmatiskt genererat innehåll
- En enda källa till sanning för kunder i flera system som kräver maskinläsbara filformat, som YAML, för integrering (t.ex. CLI-verktyg i molnet, tjänstkonfigurationer)

Exempel på mallat innehåll är, men är inte begränsade till, följande:

- [CLI-referens för molnet](help/templated/cloud-cli-ref.md)
- [Molnpaket](help/templated/cloud-packages.md)
- [Referens för ECE-verktyg](help/templated/ece-tools.md)
- [PHP-tillägg för molnet](help/templated/php-extensions-cloud.md)

### Generera mallat innehåll

I allmänhet behöver de flesta skribenter bara lägga till en releaseversion till tabellerna med produkttillgänglighet och systemkrav. Underhåll för allt annat mallat innehåll automatiseras eller hanteras av en dedikerad teammedlem. De här instruktionerna är avsedda för de flesta skribenter.

>**OBS!**
>
>- För att generera mallinnehåll måste du arbeta på kommandoraden i en terminal.
>- Du måste ha installerat Ruby för att köra återgivningsskriptet. Se [_jekyll/.ruby-version ] (_jekyll/.ruby-version) för den version som krävs.

Här nedan finns en beskrivning av filstrukturen för mallat innehåll:

- `_jekyll` - Innehåller teman och obligatoriska resurser
- `_jekyll/_data` - Innehåller de maskinläsbara filformat som används för att återge mallar
- `_jekyll/templated` - Innehåller HTML-baserade mallfiler som använder mallspråket Flytande
- `help/_includes/templated` - Innehåller genererade utdata för mallat innehåll i `.md`-filformat så att det kan publiceras i Experience League-avsnitt. Återgivningsskriptet skriver automatiskt genererade utdata till den här katalogen åt dig

Så här uppdaterar du mallinnehåll:

1. Öppna en datafil i katalogen `_jekyll/_data` i textredigeraren. Exempel:

   - [Cloud CLI-referens](help/templated/cloud-cli-ref.md): `_jekyll/_data/cloud-cli-ref.yaml`
   - [Molnpaket](help/templated/cloud-packages.md): `_jekyll/_data/cloud-packages.yaml`
   - [ECE-verktygsreferens](help/templated/ece-tools.md): `_jekyll/_data/ece-tools.yaml`

2. Använd den befintliga YAML-strukturen för att skapa poster.

3. Navigera till katalogen `_jekyll`.

   ```bash
   cd _jekyll
   ```

4. Generera mallinnehåll och skriv utdata till katalogen `help/_includes/templated`.

   ```bash
   bundle exec rake render
   ```

   >**OBS!** Du måste köra skriptet från katalogen `_jekyll`. Om detta är första gången du kör skriptet måste du installera Ruby-beroenden först med kommandot `bundle install`. Rita-åtgärderna tillhandahålls av `adobe-comdox-exl-rake-tasks` Gem för bättre underhåll över Adobe Commerce dokumentationsarkiv.

5. Gå tillbaka till katalogen `root`.

   ```bash
   cd ..
   ```

6. Verifiera att de förväntade `help/_includes/templated` filerna har ändrats.

   ```bash
   git status
   ```

   Utdata bör se ut ungefär så här:

   ```bash
   modified:   _data/cloud-cli-ref.yaml
   modified:   help/_includes/templated/cloud-cli-ref.md
   ```

7. Gör ändringar.

   ```bash
   git add .
   git commit -m "descriptive message of the intended commit"
   git push
   ```

Mer information om [datafiler](https://jekyllrb.com/docs/datafiles), [flytande filter](https://jekyllrb.com/docs/liquid/filters/) och andra funktioner finns i dokumentationen för Jekyll.

## Tillgängliga uppspelningsuppgifter

I den här databasen används streckuppgifter som tillhandahålls av `adobe-comdox-exl-rake-tasks` Gem. Om du vill se alla tillgängliga uppgifter kör du:

```bash
cd _jekyll
bundle exec rake --tasks
```

## Förimplementera kopplingar för bildoptimering

Den här databasen innehåller automatiska förimplementeringskopplingar som optimerar bilder innan implementering. **Alla medverkande bör aktivera dessa kopplingar** för att säkerställa konsekvent bildoptimering och minskad databasstorlek.

### Snabbinställningar

När du har klonat databasen kör du:

```bash
.githooks/setup-hooks.sh
```

### Vad krokarna gör

- Identifiera automatiskt mellanlagrade bildfiler (PNG, JPG, JPEG, GIF, SVG)
- Kör `image_optim` för att komprimera och optimera bilder
- Scenoptimerade bilder på nytt automatiskt
- Säkerställ att alla dedikerade bilder optimeras korrekt

### Fördelar

- Minskad databasstorlek
- Snabbare sidinläsning för dokumentation
- Enhetlig bildkvalitet för alla deltagare
- Ingen manuell optimering krävs

Detaljerade installationsanvisningar, felsökning och konfiguration finns i [`.githooks/README.md`](.githooks/README.md).
