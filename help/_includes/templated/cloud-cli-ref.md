---
source-git-commit: 9166b44ae53e8cfc6b8022730a6b91406ba696c0
workflow-type: tm+mt
source-wordcount: '13341'
ht-degree: 0%

---
# magento-cloud (Adobe Commerce on cloud infrastructure)

<!-- The template to render with above values -->

**Version**: 1.46.1

Referensen innehåller 119 kommandon som är tillgängliga via kommandoradsverktyget `magento-cloud`.
Den inledande listan genereras automatiskt med kommandot `magento-cloud list` på Adobe Commerce i molninfrastrukturen.

## Allmänt

Den här referensen genereras från programmets kodbas. Om du vill ändra innehållet kan du uppdatera källkoden för motsvarande kommandoimplementering i databasen [codebase](https://github.com/magento/magento-cloud-cli) och skicka ändringarna för granskning. Ett annat sätt är att _ge oss feedback_ (hitta länken uppe till höger). Information om riktlinjer för bidrag finns i [Kodavgifter](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Globala alternativ

#### `--help`, `-h`

Visa det här hjälpmeddelandet

- Standard: `false`
- Accepterar inte ett värde

#### `--verbose`, `-v|-vv|-vvv`

Öka meddelandenas exakthet

- Standard: `false`
- Accepterar inte ett värde

#### `--version`, `-V`

Visa den här programversionen

- Standard: `false`
- Accepterar inte ett värde

#### `--yes`, `-y`

Svara&quot;ja&quot; på bekräftelsefrågor; acceptera standardvärdet för andra frågor; inaktivera interaktion

- Standard: `false`
- Accepterar inte ett värde

#### `--no-interaction`

Ställ inga interaktiva frågor. Acceptera standardvärdena. Motsvarar användning av miljövariabeln: MAGENTO_CLOUD_CLI_NO_INTERACTION=1

- Standard: `false`
- Accepterar inte ett värde


## `clear-cache`

```bash
magento-cloud cc
```

Rensa CLI-cachen

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `decode`

```bash
magento-cloud decode [-P|--property PROPERTY] [--] <value>
```

Avkoda en kodad sträng som MAGENTO_CLOUD_VARIABLES

### Argument

#### `value`

Variabelvärdet som ska avkodas

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Den egenskap som ska visas i variabeln

- Kräver ett värde


## `docs`

```bash
magento-cloud docs [--browser BROWSER] [--pipe] [--] [<search>]...
```

Öppna onlinedokumentationen

### Argument

#### `search`

Sökord

- Standard: `[]`
- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--browser`

Webbläsaren som ska användas för att öppna URL-adressen. Ange 0 som ingen.

- Kräver ett värde

#### `--pipe`

Skriv ut URL-adressen som ska stoppas.

- Standard: `false`
- Accepterar inte ett värde


## `help`

```bash
magento-cloud help [--format FORMAT] [--raw] [--] [<command_name>]
```

Visar hjälp för ett kommando

```
The help command displays help for a given command:

  magento-cloud help list

You can also output the help in other formats by using the --format option:

  magento-cloud help --format=json list

To display the list of available commands, please use the list command.
```

### Argument

#### `command_name`

Kommandonamnet

- Standard: `help`

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--format`

Utdataformatet (txt, json eller md)

- Standard: `txt`
- Kräver ett värde

#### `--raw`

Hjälp för att skriva ut råformat

- Standard: `false`
- Accepterar inte ett värde


## `list`

```bash
magento-cloud list [--raw] [--format FORMAT] [--all] [--] [<namespace>]
```

Visar kommandon

```
The list command lists all commands:

  magento-cloud list

You can also display the commands for a specific namespace:

  magento-cloud list project

You can also output the information in other formats by using the --format option:

  magento-cloud list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  magento-cloud list --raw
```

### Argument

#### `command`

Kommandot som ska köras

- Obligatoriskt


#### `namespace`

Namnutrymmets namn

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--raw`

Utdata för kommandolista

- Standard: `false`
- Accepterar inte ett värde

#### `--format`

Utdataformatet (txt, xml, json eller md)

- Standard: `txt`
- Kräver ett värde

#### `--all`

Visa alla kommandon, även dolda

- Standard: `false`
- Accepterar inte ett värde


## `multi`

```bash
magento-cloud multi [-p|--projects PROJECTS] [--continue] [--sort SORT] [--reverse] [--] <cmd> (<cmd>)...
```

Kör ett kommando i flera projekt

### Argument

#### `cmd`

Kommandot som ska köras

- Standard: `[]`
- Obligatoriskt

- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--projects`, `-p`

En lista med projekt-ID:n, avgränsade med kommatecken och/eller blanksteg

- Kräver ett värde

#### `--continue`

Fortsätt köra kommandon även om ett undantag påträffas

- Standard: `false`
- Accepterar inte ett värde

#### `--sort`

En egenskap som listan med projektalternativ ska sorteras efter

- Standard: `title`
- Kräver ett värde

#### `--reverse`

Invertera ordningen för projektalternativ

- Standard: `false`
- Accepterar inte ett värde


## `web`

```bash
magento-cloud web [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Öppna projektet i webbgränssnittet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--browser`

Webbläsaren som ska användas för att öppna URL-adressen. Ange 0 som ingen.

- Kräver ett värde

#### `--pipe`

Skriv ut URL-adressen som ska stoppas.

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `activity:cancel`

```bash
magento-cloud activity:cancel [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Avbryt en aktivitet

### Argument

#### `id`

Aktivitets-ID. Standardvärdet är den senaste avbrutna aktiviteten.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--type`, `-t`

Filtrera efter typ (när du väljer en standardaktivitet). Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg. Tecknen % och * kan användas som jokertecken för typen, t.ex. &#39;%var%&#39; för att välja variabelrelaterade aktiviteter.

- Standard: `[]`
- Kräver ett värde

#### `--exclude-type`, `-x`

Exkludera efter typ (när du väljer en standardaktivitet). Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg. Tecknen % och * kan användas som jokertecken för att utesluta typer.

- Standard: `[]`
- Kräver ett värde

#### `--all`, `-a`

Kontrollera senaste aktiviteter i alla miljöer (när du väljer en standardaktivitet)

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `activity:get`

```bash
magento-cloud activity:get [-P|--property PROPERTY] [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<id>]
```

Visa detaljerad information om en enskild aktivitet

### Argument

#### `id`

Aktivitets-ID. Standardvärdet är den senaste aktiviteten.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Egenskapen som ska visas

- Kräver ett värde

#### `--type`, `-t`

Filtrera efter typ (när du väljer en standardaktivitet). Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg. Tecknen % och * kan användas som jokertecken för typen, t.ex. &#39;%var%&#39; för att välja variabelrelaterade aktiviteter.

- Standard: `[]`
- Kräver ett värde

#### `--exclude-type`, `-x`

Exkludera efter typ (när du väljer en standardaktivitet). Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg. Tecknen % och * kan användas som jokertecken för att utesluta typer.

- Standard: `[]`
- Kräver ett värde

#### `--state`

Filtrera efter läge (när du väljer en standardaktivitet): in_progress, pending, complete eller canceled. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--result`

Filtrera efter resultat (när du väljer en standardaktivitet): slutförd eller misslyckad

- Kräver ett värde

#### `--incomplete`, `-i`

Inkludera endast ofullständiga aktiviteter (när du väljer en standardaktivitet). Detta är en förkortning av —state=in_progress,väntande

- Standard: `false`
- Accepterar inte ett värde

#### `--all`, `-a`

Kontrollera senaste aktiviteter i alla miljöer (när du väljer en standardaktivitet)

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde


## `activity:list`

```bash
magento-cloud activities [-t|--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Hämta en lista över aktiviteter för en miljö eller ett projekt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--type`, `-t`

Filtrera aktiviteter efter typ Värden kan delas upp med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg. Den första delen av aktivitetsnamnet kan utelämnas, t.ex. &quot;cron&quot; kan välja aktiviteter av typen &quot;environment.cron&quot;. Tecknen % och * kan användas som jokertecken, t.ex. &#39;%var%&#39; för att välja variabelrelaterade aktiviteter.

- Standard: `[]`
- Kräver ett värde

#### `--exclude-type`, `-x`

Uteslut aktiviteter efter typ. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg. Den första delen av aktivitetsnamnet kan utelämnas, t.ex. kan &quot;cron&quot; utesluta aktiviteter av typen &quot;environment.cron&quot;. Tecknen % och * kan användas som jokertecken för att utesluta typer.

- Standard: `[]`
- Kräver ett värde

#### `--limit`

Begränsa antalet resultat som visas

- Standard: `10`
- Kräver ett värde

#### `--start`

Endast aktiviteter som skapats före detta datum listas

- Kräver ett värde

#### `--state`

Filtrera aktiviteter efter tillstånd: in_progress, pending, complete eller canceled. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--result`

Filtrera aktiviteter efter resultat: lyckade eller misslyckade

- Kräver ett värde

#### `--incomplete`, `-i`

Endast lista ofullständiga aktiviteter

- Standard: `false`
- Accepterar inte ett värde

#### `--all`, `-a`

Visa aktiviteter i alla miljöer

- Standard: `false`
- Accepterar inte ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: id*, created*, description*, progress*, state*, result*, complete, environment, type (* = default columns). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `activity:log`

```bash
magento-cloud activity:log [--refresh REFRESH] [-t|--timestamps] [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--state STATE] [--result RESULT] [-i|--incomplete] [-a|--all] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Visa loggen för en aktivitet

### Argument

#### `id`

Aktivitets-ID. Standardvärdet är den senaste aktiviteten.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--refresh`

Intervall för aktivitetsuppdatering (sekunder). Ange 0 om du vill inaktivera uppdatering.

- Standard: `3`
- Kräver ett värde

#### `--timestamps`, `-t`

Visa en tidsstämpel bredvid varje meddelande

- Standard: `false`
- Accepterar inte ett värde

#### `--type`

Filtrera efter typ (när du väljer en standardaktivitet). Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg. Tecknen % och * kan användas som jokertecken för typen, t.ex. &#39;%var%&#39; för att välja variabelrelaterade aktiviteter.

- Standard: `[]`
- Kräver ett värde

#### `--exclude-type`, `-x`

Exkludera efter typ (när du väljer en standardaktivitet). Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg. Tecknen % och * kan användas som jokertecken för att utesluta typer.

- Standard: `[]`
- Kräver ett värde

#### `--state`

Filtrera efter läge (när du väljer en standardaktivitet): in_progress, pending, complete eller canceled. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--result`

Filtrera efter resultat (när du väljer en standardaktivitet): slutförd eller misslyckad

- Kräver ett värde

#### `--incomplete`, `-i`

Inkludera endast ofullständiga aktiviteter (när du väljer en standardaktivitet). Detta är en förkortning av —state=in_progress,väntande

- Standard: `false`
- Accepterar inte ett värde

#### `--all`, `-a`

Kontrollera senaste aktiviteter i alla miljöer (när du väljer en standardaktivitet)

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `app:config-get`

```bash
magento-cloud app:config-get [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

Visa konfigurationen för ett program

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Den konfigurationsegenskap som ska visas

- Kräver ett värde

#### `--refresh`

Om cachen ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--identity-file`, `-i`

[Inaktuellt alternativ, används inte längre]

- Kräver ett värde


## `app:list`

```bash
magento-cloud apps [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Visa program i projektet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--refresh`

Om cachen ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--pipe`

Skriv endast ut en lista med programnamn

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: namn*, typ*, disk, sökväg, storlek (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde


## `auth:api-token-login`

```bash
magento-cloud auth:api-token-login
```

Logga in på Magento Cloud med en API-token

```
Use this command to log in to your Magento Cloud account using an API token.

You can create an account at:
    https://business.adobe.com/products/magento/magento-commerce.html

If you have an account, but you do not already have an API token, you can create one here:
    https://accounts.magento.cloud/user/api-tokens

Alternatively, to log in to the CLI with a browser, run:
    magento-cloud auth:browser-login
```

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `auth:browser-login`

```bash
magento-cloud login [-f|--force] [--browser BROWSER] [--pipe]
```

Logga in på Magento Cloud via en webbläsare

```
Use this command to log in to the Magento Cloud CLI using a web browser.

It launches a temporary local website which redirects you to log in if
necessary, and then captures the resulting authorization code.

Your system's default browser will be used. You can override this using the
--browser option.

Alternatively, to log in using an API token (without a browser), run:
magento-cloud auth:api-token-login

To authenticate non-interactively, configure an API token using the
MAGENTO_CLOUD_CLI_TOKEN environment variable.
```

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--force`, `-f`

Logga in igen, även om du redan är inloggad

- Standard: `false`
- Accepterar inte ett värde

#### `--browser`

Webbläsaren som ska användas för att öppna URL-adressen. Ange 0 som ingen.

- Kräver ett värde

#### `--pipe`

Skriv ut URL-adressen som ska stoppas.

- Standard: `false`
- Accepterar inte ett värde


## `auth:info`

```bash
magento-cloud auth:info [--no-auto-login] [-P|--property PROPERTY] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--] [<property>]
```

Visa din kontoinformation

### Argument

#### `property`

Kontoegenskapen som ska visas

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--no-auto-login`

Hoppar över automatisk inloggning. Inget skrivs ut om det inte loggas in och avslutningskoden blir 0, förutsatt att inga andra fel inträffar.

- Standard: `false`
- Accepterar inte ett värde

#### `--property`, `-P`

Den kontoegenskap som ska visas (alternativ syntax)

- Kräver ett värde

#### `--refresh`

Om cachen ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde


## `auth:logout`

```bash
magento-cloud logout [-a|--all] [--other]
```

Logga ut från Magento Cloud

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--all`, `-a`

Logga ut från alla lokala sessioner

- Standard: `false`
- Accepterar inte ett värde

#### `--other`

Logga ut från andra lokala sessioner

- Standard: `false`
- Accepterar inte ett värde


## `blackfire:setup`

```bash
magento-cloud blackfire:setup [--server_id SERVER_ID] [--server_token SERVER_TOKEN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Konfigurera Blackfire.io-integrering för projektet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--server_id`

Server-ID

- Kräver ett värde

#### `--server_token`

Servertoken

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `certificate:add`

```bash
magento-cloud certificate:add [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Lägg till ett SSL-certifikat i projektet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--cert`

Sökvägen till certifikatfilen

- Kräver ett värde

#### `--key`

Sökvägen till certifikatets privata nyckelfil

- Kräver ett värde

#### `--chain`

Sökvägen till certifikatkedjefilen

- Standard: `[]`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `certificate:delete`

```bash
magento-cloud certificate:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <id>
```

Ta bort ett certifikat från projektet

### Argument

#### `id`

Certifikat-ID (eller början av det)

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `certificate:get`

```bash
magento-cloud certificate:get [-P|--property PROPERTY] [--date-fmt DATE-FMT] [-p|--project PROJECT] [--] <id>
```

Visa ett certifikat

### Argument

#### `id`

Certifikat-ID (eller början av det)

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Certifikategenskapen som ska visas

- Kräver ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde


## `certificate:list`

```bash
magento-cloud certificates [--domain DOMAIN] [--exclude-domain EXCLUDE-DOMAIN] [--issuer ISSUER] [--only-auto] [--no-auto] [--ignore-expiry] [--only-expired] [--no-expired] [--pipe-domains] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Visa projektcertifikat

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--domain`

Filtrera efter domännamn (skiftlägesokänslig sökning)

- Kräver ett värde

#### `--exclude-domain`

Uteslut certifikat, matcha efter domännamn (skiftlägesokänslig sökning)

- Kräver ett värde

#### `--issuer`

Filtrera efter utfärdare

- Kräver ett värde

#### `--only-auto`

Visa endast automatiskt tilldelade certifikat

- Standard: `false`
- Accepterar inte ett värde

#### `--no-auto`

Visa endast manuellt tillagda certifikat

- Standard: `false`
- Accepterar inte ett värde

#### `--ignore-expiry`

Visa både utgångna och ej utgångna certifikat

- Standard: `false`
- Accepterar inte ett värde

#### `--only-expired`

Visa endast utgångna certifikat

- Standard: `false`
- Accepterar inte ett värde

#### `--no-expired`

Visa endast certifikat som inte har upphört att gälla (standard)

- Standard: `false`
- Accepterar inte ett värde

#### `--pipe-domains`

Returnera endast en lista med domännamn som omfattas av certifikaten

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: skapad, domäner, förfallodatum, id, utfärdare. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde


## `commit:get`

```bash
magento-cloud commit:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<commit>]
```

Visa implementeringsinformation

### Argument

#### `commit`

Verkställ SHA. Detta kan även acceptera &quot;HEAD&quot; och cirkumflex (^) eller tilde (~)-suffix för överordnade implementeringar.

- Standard: `HEAD`

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Den implementeringsegenskap som ska visas.

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde


## `commit:list`

```bash
magento-cloud commits [--limit LIMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<commit>]
```

Listimplementeringar

### Argument

#### `commit`

Den första Git-implementeringen av SHA. Detta kan även acceptera &quot;HEAD&quot; och cirkumflex (^) eller tilde (~)-suffix för överordnade implementeringar.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--limit`

Antalet implementeringar som ska visas.

- Standard: `10`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: författare, datum, SHA, sammanfattning. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde


## `db:dump`

```bash
magento-cloud db:dump [--schema SCHEMA] [-f|--file FILE] [-d|--directory DIRECTORY] [-z|--gzip] [-t|--timestamp] [-o|--stdout] [--table TABLE] [--exclude-table EXCLUDE-TABLE] [--schema-only] [--charset CHARSET] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

Skapa en lokal dump av fjärrdatabasen

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--schema`

Schemat som ska dumpas. Uteslut att använda standardschemat (vanligen&quot;main&quot;).

- Kräver ett värde

#### `--file`, `-f`

Ett anpassat filnamn för dumpen

- Kräver ett värde

#### `--directory`, `-d`

En anpassad katalog för dumpen

- Kräver ett värde

#### `--gzip`, `-z`

Komprimera dumpen med gzip

- Standard: `false`
- Accepterar inte ett värde

#### `--timestamp`, `-t`

Lägg till en tidsstämpel i dumpfilnamnet

- Standard: `false`
- Accepterar inte ett värde

#### `--stdout`, `-o`

Utdata till STDOUT i stället för en fil

- Standard: `false`
- Accepterar inte ett värde

#### `--table`

Tabell som ska inkluderas

- Standard: `[]`
- Kräver ett värde

#### `--exclude-table`

Tabell(er) som ska uteslutas

- Standard: `[]`
- Kräver ett värde

#### `--schema-only`

Dumpa endast scheman, inga data

- Standard: `false`
- Accepterar inte ett värde

#### `--charset`

Teckenuppsättningens kodning för dumpen

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--relationship`, `-r`

Den tjänstrelation som ska användas

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `db:size`

```bash
magento-cloud db:size [-B|--bytes] [-C|--cleanup] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE]
```

Beräkna diskanvändningen för en databas

```
This is an estimate of the database disk usage. The real size on disk is usually higher because of overhead.

To see more accurate disk usage, run: magento-cloud disk
```

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--bytes`, `-B`

Visa storlekar i byte.

- Standard: `false`
- Accepterar inte ett värde

#### `--cleanup`, `-C`

Kontrollera om tabeller kan rensas och visa rekommendationer (endast InnoDb).

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--relationship`, `-r`

Den tjänstrelation som ska användas

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: max, percent_used, used. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `db:sql`

```bash
magento-cloud sql [--raw] [--schema SCHEMA] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [--] [<query>]
```

Kör SQL på fjärrdatabasen

### Argument

#### `query`

En SQL-sats som ska köras

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--raw`

Producera råa, icke-tabulära utdata

- Standard: `false`
- Accepterar inte ett värde

#### `--schema`

Schemat som ska användas. Uteslut att använda standardschemat (vanligen&quot;main&quot;). Skicka en tom sträng för att inte använda något schema.

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--relationship`, `-r`

Den tjänstrelation som ska användas

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `domain:add`

```bash
magento-cloud domain:add [--cert CERT] [--key KEY] [--chain CHAIN] [--attach ATTACH] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Lägg till en ny domän i projektet

### Argument

#### `name`

Domännamnet

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--cert`

Sökvägen till certifikatfilen för domänen

- Kräver ett värde

#### `--key`

Sökvägen till den privata nyckelfilen för det angivna certifikatet.

- Kräver ett värde

#### `--chain`

Sökvägen till certifikatkedjefilen eller -filerna för det angivna certifikatet

- Standard: `[]`
- Kräver ett värde

#### `--attach`

Den produktionsdomän som den här ersätter i miljöns vägar. Krävs för icke-produktionsmiljöer.

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `domain:delete`

```bash
magento-cloud domain:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Ta bort en domän från projektet

### Argument

#### `name`

Domännamnet

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `domain:get`

```bash
magento-cloud domain:get [-P|--property PROPERTY] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<name>]
```

Visa detaljerad information om en domän

### Argument

#### `name`

Domännamnet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Domänegenskapen som ska visas

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `domain:list`

```bash
magento-cloud domains [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Hämta en lista över alla domäner

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: name*, ssl*, created_at*, registered_name, replace_for, type, updated_at (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `domain:update`

```bash
magento-cloud domain:update [--cert CERT] [--key KEY] [--chain CHAIN] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Uppdatera en domän

### Argument

#### `name`

Domännamnet

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--cert`

Sökvägen till certifikatfilen för domänen

- Kräver ett värde

#### `--key`

Sökvägen till den privata nyckelfilen för det angivna certifikatet.

- Kräver ett värde

#### `--chain`

Sökvägen till certifikatkedjefilen eller -filerna för det angivna certifikatet

- Standard: `[]`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:activate`

```bash
magento-cloud environment:activate [--parent PARENT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

Aktivera en miljö

### Argument

#### `environment`

Den eller de miljöer som ska aktiveras

- Standard: `[]`
- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--parent`

Ange en ny överordnad miljö innan du aktiverar

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:branch`

```bash
magento-cloud branch [--title TITLE] [--type TYPE] [--no-clone-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>] [<parent>]
```

Gren och miljön

### Argument

#### `id`

Den nya miljöns ID (filialnamn)


#### `parent`

Den nya miljöns överordnade

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--title`

Den nya miljöns titel

- Kräver ett värde

#### `--type`

Den nya miljöns typ

- Kräver ett värde

#### `--no-clone-parent`

Klona inte den överordnade miljöns data

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:checkout`

```bash
magento-cloud checkout [-i|--identity-file IDENTITY-FILE] [--] [<id>]
```

Kolla in en miljö

### Argument

#### `id`

ID för miljön som ska checkas ut. Exempel: &quot;sprint2&quot;

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `environment:delete`

```bash
magento-cloud environment:delete [--delete-branch] [--no-delete-branch] [--type TYPE] [-t|--only-type ONLY-TYPE] [--exclude EXCLUDE] [--exclude-type EXCLUDE-TYPE] [--inactive] [--merged] [--allow-delete-parent] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]...
```

Ta bort en eller flera miljöer

```
When a Magento Cloud environment is deleted, it will become "inactive": it will
exist only as a Git branch, containing code but no services, databases nor
files.

This command allows you to delete environments as well as their Git branches.
```

### Argument

#### `environment`

Den eller de miljöer som ska tas bort. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--delete-branch`

Ta bort Git-grenar för inaktiva miljöer, utan bekräftelse

- Standard: `false`
- Accepterar inte ett värde

#### `--no-delete-branch`

Ta inte bort Git-grenar (inaktiva miljöer)

- Standard: `false`
- Accepterar inte ett värde

#### `--type`

Ta bort alla miljöer av en typ (som läggs till andra markerade) Värden kan delas upp med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--only-type`, `-t`

Endast borttagningsmiljöer av en viss typ Värden kan delas upp med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--exclude`

Miljö(er) som inte ska tas bort. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--exclude-type`

Miljötyper som inte ska tas bort kan delas upp med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--inactive`

Ta bort alla inaktiva miljöer (lägga till andra markerade)

- Standard: `false`
- Accepterar inte ett värde

#### `--merged`

Ta bort alla sammanfogade miljöer (lägga till andra markerade)

- Standard: `false`
- Accepterar inte ett värde

#### `--allow-delete-parent`

Tillåt att miljöer med underordnade tas bort

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:http-access`

```bash
magento-cloud httpaccess [--access ACCESS] [--auth AUTH] [--enabled ENABLED] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Uppdatera HTTP-åtkomstinställningar för en miljö

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--access`

Åtkomstbegränsning i formatet &quot;permission:address&quot;. Använd 0 för att rensa alla adresser.

- Standard: `[]`
- Kräver ett värde

#### `--auth`

HTTP Basic-autentiseringsuppgifter i formatet &quot;username:password&quot;. Använd 0 för att rensa alla autentiseringsuppgifter.

- Standard: `[]`
- Kräver ett värde

#### `--enabled`

Anger om åtkomstkontroll ska aktiveras: 1 för att aktivera, 0 för att inaktivera

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:info`

```bash
magento-cloud environment:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

Läs eller ange egenskaper för en miljö

### Argument

#### `property`

Egenskapens namn


#### `value`

Ange ett nytt värde för egenskapen

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--refresh`

Om cachen ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:init`

```bash
magento-cloud environment:init [--profile PROFILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <url>
```

Initiera en miljö från en offentlig Git-databas

### Argument

#### `url`

En URL till en Git-databas

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--profile`

Profilens namn

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:list`

```bash
magento-cloud environments [-I|--no-inactive] [--pipe] [--refresh REFRESH] [--sort SORT] [--reverse] [--type TYPE] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Få en lista över miljöer

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--no-inactive`, `-I`

Visa inte inaktiva miljöer

- Standard: `false`
- Accepterar inte ett värde

#### `--pipe`

Skriv ut en enkel lista över miljö-ID:n.

- Standard: `false`
- Accepterar inte ett värde

#### `--refresh`

Anger om listan ska uppdateras.

- Standard: `1`
- Kräver ett värde

#### `--sort`

En egenskap som ska sorteras efter

- Standard: `title`
- Kräver ett värde

#### `--reverse`

Sortera i omvänd ordning (fallande)

- Standard: `false`
- Accepterar inte ett värde

#### `--type`

Filtrera listan efter miljötyp(er). Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: id*, title*, status*, type*, created, machine_name, updated (* = default columns). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde


## `environment:logs`

```bash
magento-cloud log [--lines LINES] [--tail] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [--] [<type>]
```

Läs en miljös loggar

### Argument

#### `type`

Loggtypen, t.ex. &quot;access&quot; eller &quot;error&quot;

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--lines`

Antalet rader som ska visas

- Standard: `100`
- Kräver ett värde

#### `--tail`

Avsluta loggen kontinuerligt

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--worker`

Ett arbetarnamn

- Kräver ett värde

#### `--instance`, `-I`

Ett instans-ID

- Kräver ett värde


## `environment:merge`

```bash
magento-cloud merge [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

Sammanfoga en miljö

```
This command will initiate a Git merge of the specified environment into its parent environment.
```

### Argument

#### `environment`

Den miljö som ska sammanfogas

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:pause`

```bash
magento-cloud environment:pause [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Pausa en miljö

```
Pausing an environment helps to reduce resource consumption and carbon emissions.

The environment will be unavailable until it is resumed. No data will be lost.
```

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:push`

```bash
magento-cloud push [--target TARGET] [-f|--force] [--force-with-lease] [-u|--set-upstream] [--activate] [--parent PARENT] [--type TYPE] [--no-clone-parent] [-W|--no-wait] [--wait] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-i|--identity-file IDENTITY-FILE] [--] [<source>]
```

Kodning i en miljö

### Argument

#### `source`

Källreferens: ett grennamn eller en implementerad hash

- Standard: `HEAD`

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--target`

Målgrenens namn. Standardvärdet är den aktuella grenen.

- Kräver ett värde

#### `--force`, `-f`

Tillåt icke-snabba framåtriktade uppdateringar

- Standard: `false`
- Accepterar inte ett värde

#### `--force-with-lease`

Tillåt icke-snabba framåtriktade uppdateringar om fjärrspårningsgrenen är uppdaterad

- Standard: `false`
- Accepterar inte ett värde

#### `--set-upstream`, `-u`

Ange målmiljön som den överordnade för källgrenen. Detta anger även målprojektet som fjärrplats för den lokala databasen.

- Standard: `false`
- Accepterar inte ett värde

#### `--activate`

Aktivera miljön innan du trycker

- Standard: `false`
- Accepterar inte ett värde

#### `--parent`

Ange den nya miljön som överordnad (används endast med —activate)

- Kräver ett värde

#### `--type`

Ange miljötypen (används endast med —activate )

- Kräver ett värde

#### `--no-clone-parent`

Klona inte den överordnade grenens data (används endast med —activate)

- Standard: `false`
- Accepterar inte ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `environment:redeploy`

```bash
magento-cloud redeploy [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Distribuera om en miljö

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:relationships`

```bash
magento-cloud relationships [-P|--property PROPERTY] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<environment>]
```

Visa en miljös relationer

### Argument

#### `environment`

Miljön

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Den relationsegenskap som ska visas

- Kräver ett värde

#### `--refresh`

Om relationerna ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `environment:resume`

```bash
magento-cloud environment:resume [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait]
```

Återuppta en pausad miljö

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:scp`

```bash
magento-cloud scp [-r|--recursive] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<files>]...
```

Kopiera filer till och från en miljö med scp

### Argument

#### `files`

Filer att kopiera. Använd prefixet remote: för att definiera fjärrplatser.

- Standard: `[]`
- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--recursive`, `-r`

Kopiera hela kataloger rekursivt

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--worker`

Ett arbetarnamn

- Kräver ett värde

#### `--instance`, `-I`

Ett instans-ID

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `environment:ssh`

```bash
magento-cloud ssh [--pipe] [--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE] [--] [<cmd>]...
```

SSH till den aktuella miljön

### Argument

#### `cmd`

Ett kommando som körs i miljön.

- Standard: `[]`
- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--pipe`

Skriv bara ut SSH-URL:en.

- Standard: `false`
- Accepterar inte ett värde

#### `--all`

Skriv ut alla SSH-URL:er (för alla program).

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--worker`

Ett arbetarnamn

- Kräver ett värde

#### `--instance`, `-I`

Ett instans-ID

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `environment:synchronize`

```bash
magento-cloud sync [--rebase] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<synchronize>]...
```

Synkronisera en miljös kod och/eller data från dess överordnade

```
This command synchronizes to a child environment from its parent environment.

Synchronizing "code" means there will be a Git merge from the parent to the
child. Synchronizing "data" means that all files in all services (including
static files, databases, logs, search indices, etc.) will be copied from the
parent to the child.
```

### Argument

#### `synchronize`

Synkronisera: &quot;kod&quot;, &quot;data&quot; eller både och

- Standard: `[]`
- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--rebase`

Synkronisera kod genom att basera om i stället för att sammanfoga

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `environment:url`

```bash
magento-cloud url [-1|--primary] [--browser BROWSER] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Hämta offentliga URL:er för en miljö

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--primary`, `-1`

Returnera bara URL:en för den primära vägen

- Standard: `false`
- Accepterar inte ett värde

#### `--browser`

Webbläsaren som ska användas för att öppna URL-adressen. Ange 0 som ingen.

- Kräver ett värde

#### `--pipe`

Skriv ut URL-adressen som ska stoppas.

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `environment:xdebug`

```bash
magento-cloud xdebug [--port PORT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Öppna en tunnel för Xdebug i miljön

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--port`

Den lokala porten

- Standard: `9000`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--worker`

Ett arbetarnamn

- Kräver ett värde

#### `--instance`, `-I`

Ett instans-ID

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `integration:activity:get`

```bash
magento-cloud integration:activity:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [--] [<integration>] [<activity>]
```

Visa detaljerad information om en enskild integreringsaktivitet

### Argument

#### `integration`

Ett integrerings-ID. Lämna tomt om du vill välja från en lista.


#### `activity`

Aktivitets-ID. Som standard används den senaste integreringsaktiviteten.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Egenskapen som ska visas

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

[Inaktuellt alternativ, används inte]

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde


## `integration:activity:list`

```bash
magento-cloud int:act [--type TYPE] [-x|--exclude-type EXCLUDE-TYPE] [--limit LIMIT] [--start START] [--state STATE] [--result RESULT] [-i|--incomplete] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<id>]
```

Få en lista över aktiviteter för en integrering

### Argument

#### `id`

Ett integrerings-ID. Lämna tomt om du vill välja från en lista.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--type`

Filtrera aktiviteter efter typ. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--exclude-type`, `-x`

Uteslut aktiviteter efter typ. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg. Tecknen % och * kan användas som jokertecken för att utesluta typer.

- Standard: `[]`
- Kräver ett värde

#### `--limit`

Begränsa antalet resultat som visas

- Standard: `10`
- Kräver ett värde

#### `--start`

Endast aktiviteter som skapats före detta datum listas

- Kräver ett värde

#### `--state`

Filtrera aktiviteter efter stat. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--result`

Filtrera aktiviteter efter resultat

- Kräver ett värde

#### `--incomplete`, `-i`

Endast lista ofullständiga aktiviteter

- Standard: `false`
- Accepterar inte ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: id*, created*, description*, type*, state*, result*, complete (* = default columns). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

[Inaktuellt alternativ, används inte]

- Kräver ett värde


## `integration:activity:log`

```bash
magento-cloud integration:activity:log [-t|--timestamps] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<integration>] [<activity>]
```

Visa loggen för en integreringsaktivitet

### Argument

#### `integration`

Ett integrerings-ID. Lämna tomt om du vill välja från en lista.


#### `activity`

Aktivitets-ID. Som standard används den senaste integreringsaktiviteten.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--timestamps`, `-t`

Visa en tidsstämpel bredvid varje meddelande

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

[Inaktuellt alternativ, används inte]

- Kräver ett värde


## `integration:add`

```bash
magento-cloud integration:add [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait]
```

Lägg till en integrering i projektet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--type`

Integrationstypen (&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webkrok&#39;, &#39;health.email&#39;, &#39;health.pageruty&#39;, &#39;health.slack&#39;, &#39;health.webkrok&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumologic&#39;, &#39;syslog&#39;)

- Kräver ett värde

#### `--base-url`

Bas-URL:en för serverinstallationen

- Kräver ett värde

#### `--bitbucket-url`

Bas-URL:en för Bitbucket Server-installationen

- Kräver ett värde

#### `--username`

Användarnamnet för Bitbucket Server

- Kräver ett värde

#### `--token`

En token för autentisering eller åtkomst för integreringen

- Kräver ett värde

#### `--key`

En Bitbucket OAuth-konsumentnyckel

- Kräver ett värde

#### `--secret`

En Bitbucket OAuth-konsumenthemlighet

- Kräver ett värde

#### `--license-key`

Licensnyckeln för New Relic Logs

- Kräver ett värde

#### `--server-project`

Projektet (t.ex. &#39;namespace/repo&#39;)

- Kräver ett värde

#### `--repository`

Den databas som ska spåras (t.ex. &#39;ägare/databas&#39;)

- Kräver ett värde

#### `--build-merge-requests`

GitLab: skapa sammanfogningsbegäranden som miljöer

- Standard: `true`
- Kräver ett värde

#### `--build-pull-requests`

Bygg upp alla förfrågningar som en miljö

- Standard: `true`
- Kräver ett värde

#### `--build-draft-pull-requests`

Bygg släppbegäranden

- Standard: `true`
- Kräver ett värde

#### `--build-pull-requests-post-merge`

Skapa pull-begäranden baserat på deras status efter sammanfogning

- Standard: `false`
- Kräver ett värde

#### `--build-wip-merge-requests`

GitLab: skapa WIP-sammanslagningsbegäranden

- Standard: `true`
- Kräver ett värde

#### `--merge-requests-clone-parent-data`

GitLab: klondata för sammanslagningsbegäranden

- Standard: `true`
- Kräver ett värde

#### `--pull-requests-clone-parent-data`

Klona den överordnade miljöns data för pull-begäranden

- Standard: `true`
- Kräver ett värde

#### `--resync-pull-requests`

Synkronisera miljödata för pull-begäran på nytt vid varje bygge

- Standard: `false`
- Kräver ett värde

#### `--fetch-branches`

Hämta alla grenar från fjärrkontrollen (som inaktiva miljöer)

- Standard: `true`
- Kräver ett värde

#### `--prune-branches`

Ta bort grenar som inte finns på fjärrkontrollen

- Standard: `true`
- Kräver ett värde

#### `--resources-init`

De resurser som ska användas när en ny tjänst initieras (&#39;minimum&#39;, &#39;default&#39;, &#39;manual&#39;, &#39;parent&#39;)

- Kräver ett värde

#### `--url`

Integreringens URL- eller API-slutpunkt

- Kräver ett värde

#### `--shared-key`

Webkrok: den delade hemliga nyckeln för JWS

- Kräver ett värde

#### `--file`

Namnet på en lokal fil som innehåller skriptet som ska överföras

- Kräver ett värde

#### `--events`

En lista över händelser som ska hanteras, t.ex. environment.push

- Standard: `*`
- Kräver ett värde

#### `--states`

En lista över lägen att agera på, t.ex. väntande, in_progress, slutfört

- Standard: `complete`
- Kräver ett värde

#### `--environments`

De miljö-ID som ska inkluderas

- Standard: `*`
- Kräver ett värde

#### `--excluded-environments`

De miljö-ID:n som ska uteslutas

- Standard: `[]`
- Kräver ett värde

#### `--from-address`

[Valfritt] Anpassad från-adress för varningsmeddelanden

- Kräver ett värde

#### `--recipients`

Mottagarens e-postadress(er)

- Standard: `[]`
- Kräver ett värde

#### `--channel`

Slack

- Kräver ett värde

#### `--routing-key`

PagerDuty-routningsnyckeln

- Kräver ett värde

#### `--category`

Kategorin Sumo Logic, som används för filtrering

- Kräver ett värde

#### `--index`

Splunk-index

- Kräver ett värde

#### `--sourcetype`

Källtypen för Splunk-händelsen

- Kräver ett värde

#### `--protocol`

Syslog-transportprotokoll (&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- Standard: `tls`
- Kräver ett värde

#### `--syslog-host`

Syslog relay/collector host

- Kräver ett värde

#### `--syslog-port`

Syslog-relä/insamlarport

- Kräver ett värde

#### `--facility`

Syslog-funktion

- Standard: `1`
- Kräver ett värde

#### `--message-format`

Syslog-meddelandeformat (&#39;rfc3164&#39; eller &#39;rfc5424&#39;)

- Standard: `rfc5424`
- Kräver ett värde

#### `--auth-mode`

Autentiseringsläge (prefix eller Structure_data)

- Standard: `prefix`
- Kräver ett värde

#### `--auth-token`

Autentiseringstoken

- Kräver ett värde

#### `--verify-tls`

Om HTTPS-certifikatverifiering ska vara aktiverat (rekommenderas)

- Standard: `true`
- Kräver ett värde

#### `--header`

HTTP-huvud(n) som ska användas i POST-begäranden. Avgränsa namn och värden med kolon (:).

- Standard: `[]`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `integration:delete`

```bash
magento-cloud integration:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

Ta bort en integrering från ett projekt

### Argument

#### `id`

Integrations-ID. Lämna tomt om du vill välja från en lista.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `integration:get`

```bash
magento-cloud integration:get [-P|--property [PROPERTY]] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<id>]
```

Visa information om en integrering

### Argument

#### `id`

Ett integrerings-ID. Lämna tomt om du vill välja från en lista.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Den integrationsegenskap som ska visas

- Accepterar ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde


## `integration:list`

```bash
magento-cloud integrations [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Visa en lista över projektintegrering(ar)

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: id, sammanfattning, typ. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde


## `integration:update`

```bash
magento-cloud integration:update [--type TYPE] [--base-url BASE-URL] [--bitbucket-url BITBUCKET-URL] [--username USERNAME] [--token TOKEN] [--key KEY] [--secret SECRET] [--license-key LICENSE-KEY] [--server-project SERVER-PROJECT] [--repository REPOSITORY] [--build-merge-requests BUILD-MERGE-REQUESTS] [--build-pull-requests BUILD-PULL-REQUESTS] [--build-draft-pull-requests BUILD-DRAFT-PULL-REQUESTS] [--build-pull-requests-post-merge BUILD-PULL-REQUESTS-POST-MERGE] [--build-wip-merge-requests BUILD-WIP-MERGE-REQUESTS] [--merge-requests-clone-parent-data MERGE-REQUESTS-CLONE-PARENT-DATA] [--pull-requests-clone-parent-data PULL-REQUESTS-CLONE-PARENT-DATA] [--resync-pull-requests RESYNC-PULL-REQUESTS] [--fetch-branches FETCH-BRANCHES] [--prune-branches PRUNE-BRANCHES] [--resources-init RESOURCES-INIT] [--url URL] [--shared-key SHARED-KEY] [--file FILE] [--events EVENTS] [--states STATES] [--environments ENVIRONMENTS] [--excluded-environments EXCLUDED-ENVIRONMENTS] [--from-address FROM-ADDRESS] [--recipients RECIPIENTS] [--channel CHANNEL] [--routing-key ROUTING-KEY] [--category CATEGORY] [--index INDEX] [--sourcetype SOURCETYPE] [--protocol PROTOCOL] [--syslog-host SYSLOG-HOST] [--syslog-port SYSLOG-PORT] [--facility FACILITY] [--message-format MESSAGE-FORMAT] [--auth-mode AUTH-MODE] [--auth-token AUTH-TOKEN] [--verify-tls VERIFY-TLS] [--header HEADER] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<id>]
```

Uppdatera en integrering

### Argument

#### `id`

ID för den integrering som ska uppdateras

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--type`

Integrationstypen (&#39;bitbucket&#39;, &#39;bitbucket_server&#39;, &#39;github&#39;, &#39;gitlab&#39;, &#39;webkrok&#39;, &#39;health.email&#39;, &#39;health.pageruty&#39;, &#39;health.slack&#39;, &#39;health.webkrok&#39;, &#39;httplog&#39;, &#39;script&#39;, &#39;newrelic&#39;, &#39;splunk&#39;, &#39;sumologic&#39;, &#39;syslog&#39;)

- Kräver ett värde

#### `--base-url`

Bas-URL:en för serverinstallationen

- Kräver ett värde

#### `--bitbucket-url`

Bas-URL:en för Bitbucket Server-installationen

- Kräver ett värde

#### `--username`

Användarnamnet för Bitbucket Server

- Kräver ett värde

#### `--token`

En token för autentisering eller åtkomst för integreringen

- Kräver ett värde

#### `--key`

En Bitbucket OAuth-konsumentnyckel

- Kräver ett värde

#### `--secret`

En Bitbucket OAuth-konsumenthemlighet

- Kräver ett värde

#### `--license-key`

Licensnyckeln för New Relic Logs

- Kräver ett värde

#### `--server-project`

Projektet (t.ex. &#39;namespace/repo&#39;)

- Kräver ett värde

#### `--repository`

Den databas som ska spåras (t.ex. &#39;ägare/databas&#39;)

- Kräver ett värde

#### `--build-merge-requests`

GitLab: skapa sammanfogningsbegäranden som miljöer

- Standard: `true`
- Kräver ett värde

#### `--build-pull-requests`

Bygg upp alla förfrågningar som en miljö

- Standard: `true`
- Kräver ett värde

#### `--build-draft-pull-requests`

Bygg släppbegäranden

- Standard: `true`
- Kräver ett värde

#### `--build-pull-requests-post-merge`

Skapa pull-begäranden baserat på deras status efter sammanfogning

- Standard: `false`
- Kräver ett värde

#### `--build-wip-merge-requests`

GitLab: skapa WIP-sammanslagningsbegäranden

- Standard: `true`
- Kräver ett värde

#### `--merge-requests-clone-parent-data`

GitLab: klondata för sammanslagningsbegäranden

- Standard: `true`
- Kräver ett värde

#### `--pull-requests-clone-parent-data`

Klona den överordnade miljöns data för pull-begäranden

- Standard: `true`
- Kräver ett värde

#### `--resync-pull-requests`

Synkronisera miljödata för pull-begäran på nytt vid varje bygge

- Standard: `false`
- Kräver ett värde

#### `--fetch-branches`

Hämta alla grenar från fjärrkontrollen (som inaktiva miljöer)

- Standard: `true`
- Kräver ett värde

#### `--prune-branches`

Ta bort grenar som inte finns på fjärrkontrollen

- Standard: `true`
- Kräver ett värde

#### `--resources-init`

De resurser som ska användas när en ny tjänst initieras (&#39;minimum&#39;, &#39;default&#39;, &#39;manual&#39;, &#39;parent&#39;)

- Kräver ett värde

#### `--url`

Integreringens URL- eller API-slutpunkt

- Kräver ett värde

#### `--shared-key`

Webkrok: den delade hemliga nyckeln för JWS

- Kräver ett värde

#### `--file`

Namnet på en lokal fil som innehåller skriptet som ska överföras

- Kräver ett värde

#### `--events`

En lista över händelser som ska hanteras, t.ex. environment.push

- Standard: `*`
- Kräver ett värde

#### `--states`

En lista över lägen att agera på, t.ex. väntande, in_progress, slutfört

- Standard: `complete`
- Kräver ett värde

#### `--environments`

De miljö-ID som ska inkluderas

- Standard: `*`
- Kräver ett värde

#### `--excluded-environments`

De miljö-ID:n som ska uteslutas

- Standard: `[]`
- Kräver ett värde

#### `--from-address`

[Valfritt] Anpassad från-adress för varningsmeddelanden

- Kräver ett värde

#### `--recipients`

Mottagarens e-postadress(er)

- Standard: `[]`
- Kräver ett värde

#### `--channel`

Slack

- Kräver ett värde

#### `--routing-key`

PagerDuty-routningsnyckeln

- Kräver ett värde

#### `--category`

Kategorin Sumo Logic, som används för filtrering

- Kräver ett värde

#### `--index`

Splunk-index

- Kräver ett värde

#### `--sourcetype`

Källtypen för Splunk-händelsen

- Kräver ett värde

#### `--protocol`

Syslog-transportprotokoll (&#39;tcp&#39;, &#39;udp&#39;, &#39;tls&#39;)

- Standard: `tls`
- Kräver ett värde

#### `--syslog-host`

Syslog relay/collector host

- Kräver ett värde

#### `--syslog-port`

Syslog-relä/insamlarport

- Kräver ett värde

#### `--facility`

Syslog-funktion

- Standard: `1`
- Kräver ett värde

#### `--message-format`

Syslog-meddelandeformat (&#39;rfc3164&#39; eller &#39;rfc5424&#39;)

- Standard: `rfc5424`
- Kräver ett värde

#### `--auth-mode`

Autentiseringsläge (prefix eller Structure_data)

- Standard: `prefix`
- Kräver ett värde

#### `--auth-token`

Autentiseringstoken

- Kräver ett värde

#### `--verify-tls`

Om HTTPS-certifikatverifiering ska vara aktiverat (rekommenderas)

- Standard: `true`
- Kräver ett värde

#### `--header`

HTTP-huvud(n) som ska användas i POST-begäranden. Avgränsa namn och värden med kolon (:).

- Standard: `[]`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `integration:validate`

```bash
magento-cloud integration:validate [-p|--project PROJECT] [--] [<id>]
```

Validera en befintlig integrering

```
This command allows you to check whether an integration is valid.

An exit code of 0 means the integration is valid, while 4 means it is invalid.
Any other exit code indicates an unexpected error.

Integrations are validated automatically on creation and on update. However,
because they involve external resources, it is possible for a valid integration
to become invalid. For example, an access token may be revoked, or an external
repository may be deleted.
```

### Argument

#### `id`

Ett integrerings-ID. Lämna tomt om du vill välja från en lista.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde


## `local:build`

```bash
magento-cloud build [-a|--abslinks] [-s|--source SOURCE] [-d|--destination DESTINATION] [-c|--copy] [--clone] [--run-deploy-hooks] [--no-clean] [--no-archive] [--no-backup] [--no-cache] [--no-build-hooks] [--no-deps] [--working-copy] [--concurrency CONCURRENCY] [--lock] [--] [<app>]...
```

Bygg det aktuella projektet lokalt

### Argument

#### `app`

Ange vilka program som ska byggas

- Standard: `[]`
- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--abslinks`, `-a`

Använd absoluta länkar

- Standard: `false`
- Accepterar inte ett värde

#### `--source`, `-s`

Källkatalogen. Standardvärdet är projektets aktuella rot.

- Kräver ett värde

#### `--destination`, `-d`

Det mål som webbroten för varje program ska länkas till. Standard: _www

- Kräver ett värde

#### `--copy`, `-c`

Kopiera till en byggkatalog i stället för att länka från källan

- Standard: `false`
- Accepterar inte ett värde

#### `--clone`

Använd Git för att klona den aktuella HEAD-filen till byggkatalogen

- Standard: `false`
- Accepterar inte ett värde

#### `--run-deploy-hooks`

Kör hookar för driftsättning och/eller post_deploy

- Standard: `false`
- Accepterar inte ett värde

#### `--no-clean`

Ta inte bort gamla byggen

- Standard: `false`
- Accepterar inte ett värde

#### `--no-archive`

Skapa eller använd inte ett byggarkiv

- Standard: `false`
- Accepterar inte ett värde

#### `--no-backup`

Säkerhetskopiera inte föregående version

- Standard: `false`
- Accepterar inte ett värde

#### `--no-cache`

Inaktivera cachelagring

- Standard: `false`
- Accepterar inte ett värde

#### `--no-build-hooks`

Kör inte kopplingar efter bygget

- Standard: `false`
- Accepterar inte ett värde

#### `--no-deps`

Installera inte byggberoenden lokalt

- Standard: `false`
- Accepterar inte ett värde

#### `--working-copy`

Dra: använd git för att klona en databas för varje Drupal-modul i stället för att bara hämta en version

- Standard: `false`
- Accepterar inte ett värde

#### `--concurrency`

Dra: ange antalet samtidiga projekt som ska bearbetas samtidigt

- Standard: `4`
- Kräver ett värde

#### `--lock`

Dra: skapa eller uppdatera en låsfil (endast tillgänglig med Drush version 7+)

- Standard: `false`
- Accepterar inte ett värde


## `local:dir`

```bash
magento-cloud dir [<subdir>]
```

Hitta den lokala projektroten

### Argument

#### `subdir`

Den underkatalog som ska hittas (&#39;local&#39;, &#39;web&#39; eller &#39;shared&#39;)

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `metrics:all`

```bash
magento-cloud metrics [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA Show CPU, disk- och minnesstatistik för en miljö

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--bytes`, `-B`

Visa storlekar i byte

- Standard: `false`
- Accepterar inte ett värde

#### `--range`, `-r`

Tidsintervall. Mätvärden läses in för den här varaktigheten till sluttiden (—to). Du kan ange enheter: timmar (h), minuter (m) eller sekunder (s). Minst 5 m, högst 8 h (beroende på projektet), standard 10 m.

- Kräver ett värde

#### `--interval`, `-i`

Tidsintervallet. Standardvärdet är en division av intervallet. Du kan ange enheter: timmar (h), minuter (m) eller sekunder (s). Minst 1 m.

- Kräver ett värde

#### `--to`

Sluttiden. Standardvärdet är nu.

- Kräver ett värde

#### `--latest`, `-1`

Visa endast den senaste enskilda datapunkten

- Standard: `false`
- Accepterar inte ett värde

#### `--service`, `-s`

Filtrera efter tjänst- eller programnamn Tecknen % eller * kan användas som jokertecken.

- Standard: `[]`
- Kräver ett värde

#### `--type`

Filtrera efter tjänsttyp (if —service tillhandahålls inte). Versionen är inte obligatorisk. Tecknen % och * kan användas som jokertecken.

- Standard: `[]`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: tidsstämpel*, tjänst*, cpu_percent*, mem_percent*, disc_percent*, tmp_disk_percent*, cpu_limit, cpu_used, disc_limit, disc_used, inodes_limit, inodes_percent, inodes_used, mem_limit, mem_used, tmp_disk_limit, tmp_disk_used, tmmmmmmtmp_used p_inodes_limit, tmp_inodes_percent, tmp_inodes_used, type (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde


## `metrics:cpu`

```bash
magento-cloud cpu [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA Show CPU Usage of an environment

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--range`, `-r`

Tidsintervall. Mätvärden läses in för den här varaktigheten till sluttiden (—to). Du kan ange enheter: timmar (h), minuter (m) eller sekunder (s). Minst 5 m, högst 8 h (beroende på projektet), standard 10 m.

- Kräver ett värde

#### `--interval`, `-i`

Tidsintervallet. Standardvärdet är en division av intervallet. Du kan ange enheter: timmar (h), minuter (m) eller sekunder (s). Minst 1 m.

- Kräver ett värde

#### `--to`

Sluttiden. Standardvärdet är nu.

- Kräver ett värde

#### `--latest`, `-1`

Visa endast den senaste enskilda datapunkten

- Standard: `false`
- Accepterar inte ett värde

#### `--service`, `-s`

Filtrera efter tjänst- eller programnamn Tecknen % eller * kan användas som jokertecken.

- Standard: `[]`
- Kräver ett värde

#### `--type`

Filtrera efter tjänsttyp (if —service tillhandahålls inte). Versionen är inte obligatorisk. Tecknen % och * kan användas som jokertecken.

- Standard: `[]`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: tidsstämpel*, tjänst*, använd*, gräns*, procent*, typ (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde


## `metrics:disk-usage`

```bash
magento-cloud disk [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [--tmp] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Visa diskanvändning i en miljö

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--bytes`, `-B`

Visa storlekar i byte

- Standard: `false`
- Accepterar inte ett värde

#### `--range`, `-r`

Tidsintervall. Mätvärden läses in för den här varaktigheten till sluttiden (—to). Du kan ange enheter: timmar (h), minuter (m) eller sekunder (s). Minst 5 m, högst 8 h (beroende på projektet), standard 10 m.

- Kräver ett värde

#### `--interval`, `-i`

Tidsintervallet. Standardvärdet är en division av intervallet. Du kan ange enheter: timmar (h), minuter (m) eller sekunder (s). Minst 1 m.

- Kräver ett värde

#### `--to`

Sluttiden. Standardvärdet är nu.

- Kräver ett värde

#### `--latest`, `-1`

Visa endast den senaste enskilda datapunkten

- Standard: `false`
- Accepterar inte ett värde

#### `--service`, `-s`

Filtrera efter tjänst- eller programnamn Tecknen % eller * kan användas som jokertecken.

- Standard: `[]`
- Kräver ett värde

#### `--type`

Filtrera efter tjänsttyp (if —service tillhandahålls inte). Versionen är inte obligatorisk. Tecknen % och * kan användas som jokertecken.

- Standard: `[]`
- Kräver ett värde

#### `--tmp`

Rapportera tillfällig diskanvändning (visar kolumner: tidsstämpel, tjänst, tmp_used, tmp_limit, tmp_percent, tmp_ipercent)

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: tidsstämpel*, tjänst*, använd*, gräns*, procent*, ipercent*, tmp_percent*, ilimit, iused, tmp_ilimit, tmp_ipercent, tmp_iused, tmp_limit, tmp_used, type (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde


## `metrics:memory`

```bash
magento-cloud mem [-B|--bytes] [-r|--range RANGE] [-i|--interval INTERVAL] [--to TO] [-1|--latest] [-s|--service SERVICE] [--type TYPE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

BETA Visa minnesanvändning i en miljö

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--bytes`, `-B`

Visa storlekar i byte

- Standard: `false`
- Accepterar inte ett värde

#### `--range`, `-r`

Tidsintervall. Mätvärden läses in för den här varaktigheten till sluttiden (—to). Du kan ange enheter: timmar (h), minuter (m) eller sekunder (s). Minst 5 m, högst 8 h (beroende på projektet), standard 10 m.

- Kräver ett värde

#### `--interval`, `-i`

Tidsintervallet. Standardvärdet är en division av intervallet. Du kan ange enheter: timmar (h), minuter (m) eller sekunder (s). Minst 1 m.

- Kräver ett värde

#### `--to`

Sluttiden. Standardvärdet är nu.

- Kräver ett värde

#### `--latest`, `-1`

Visa endast den senaste enskilda datapunkten

- Standard: `false`
- Accepterar inte ett värde

#### `--service`, `-s`

Filtrera efter tjänst- eller programnamn Tecknen % eller * kan användas som jokertecken.

- Standard: `[]`
- Kräver ett värde

#### `--type`

Filtrera efter tjänsttyp (if —service tillhandahålls inte). Versionen är inte obligatorisk. Tecknen % och * kan användas som jokertecken.

- Standard: `[]`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: tidsstämpel*, tjänst*, använd*, gräns*, procent*, typ (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde


## `mount:download`

```bash
magento-cloud mount:download [-a|--all] [-m|--mount MOUNT] [--target TARGET] [--source-path] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Hämta filer från en montering med hjälp av synkronisering

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--all`, `-a`

Hämta från alla monteringar

- Standard: `false`
- Accepterar inte ett värde

#### `--mount`, `-m`

Monteringen (som en apprelativ sökväg)

- Kräver ett värde

#### `--target`

Katalogen som filerna ska hämtas till. Om —all används läggs monteringssökvägen till

- Kräver ett värde

#### `--source-path`

Använd monteringens källsökväg (i stället för monteringssökvägen) som en underkatalog till målet, när —all används

- Standard: `false`
- Accepterar inte ett värde

#### `--delete`

Om överflödiga filer ska tas bort i målkatalogen

- Standard: `false`
- Accepterar inte ett värde

#### `--exclude`

Fil(er) som ska uteslutas från nedladdningen (mönster)

- Standard: `[]`
- Kräver ett värde

#### `--include`

Fil(er) som inte ska uteslutas (mönster)

- Standard: `[]`
- Kräver ett värde

#### `--refresh`

Om cachen ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--worker`

Ett arbetarnamn

- Kräver ett värde

#### `--instance`, `-I`

Ett instans-ID

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `mount:list`

```bash
magento-cloud mounts [--paths] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Få en lista över monteringar

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--paths`

Utdata endast för monteringsbanor (en per rad)

- Standard: `false`
- Accepterar inte ett värde

#### `--refresh`

Om cachen ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: definition, sökväg. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--worker`

Ett arbetarnamn

- Kräver ett värde

#### `--instance`, `-I`

Ett instans-ID

- Kräver ett värde


## `mount:size`

```bash
magento-cloud mount:size [-B|--bytes] [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE]
```

Kontrollera diskanvändning av monteringar

```
Use this command to check the disk size and usage for an application's mounts.

Mounts are directories mounted into the application from a persistent, writable
filesystem. They are configured in the mounts key in the application configuration.

The filesystem's total size is determined by the disk key in the same file.
```

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--bytes`, `-B`

Visa storlekar i byte

- Standard: `false`
- Accepterar inte ett värde

#### `--refresh`

Uppdatera cachen

- Standard: `false`
- Accepterar inte ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: available, max, monts, percent_used, sizes, used. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--worker`

Ett arbetarnamn

- Kräver ett värde

#### `--instance`, `-I`

Ett instans-ID

- Kräver ett värde


## `mount:upload`

```bash
magento-cloud mount:upload [--source SOURCE] [-m|--mount MOUNT] [--delete] [--exclude EXCLUDE] [--include INCLUDE] [--refresh] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-I|--instance INSTANCE] [-i|--identity-file IDENTITY-FILE]
```

Överför filer till en plats med hjälp av synkronisering

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--source`

En katalog som innehåller filer som ska överföras

- Kräver ett värde

#### `--mount`, `-m`

Monteringen (som en apprelativ sökväg)

- Kräver ett värde

#### `--delete`

Om överflödiga filer ska tas bort i monteringen

- Standard: `false`
- Accepterar inte ett värde

#### `--exclude`

Fil(er) som ska uteslutas från överföringen (mönster)

- Standard: `[]`
- Kräver ett värde

#### `--include`

Fil(er) som inte ska uteslutas (mönster)

- Standard: `[]`
- Kräver ett värde

#### `--refresh`

Om cachen ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--worker`

Ett arbetarnamn

- Kräver ett värde

#### `--instance`, `-I`

Ett instans-ID

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `operation:list`

```bash
magento-cloud ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Körningsåtgärder för Beta List i en miljö

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--full`

Begränsa inte längden på det kommando som ska visas. Standardgränsen är 24 rader.

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--worker`

Ett arbetarnamn

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: service*, name*, start*, role, stop (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde


## `operation:run`

```bash
magento-cloud operation:run [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--worker WORKER] [-W|--no-wait] [--wait] [--] [<operation>]
```

BETA Köra en åtgärd i miljön

### Argument

#### `operation`

Åtgärdsnamnet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--worker`

Ett arbetarnamn

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `project:clear-build-cache`

```bash
magento-cloud project:clear-build-cache [-p|--project PROJECT]
```

Rensa ett projekts byggcache

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde


## `project:get`

```bash
magento-cloud get [-e|--environment ENVIRONMENT] [--depth DEPTH] [--build] [-p|--project PROJECT] [-i|--identity-file IDENTITY-FILE] [--] [<project>] [<directory>]
```

Klona ett projekt lokalt

### Argument

#### `project`

Projekt-ID


#### `directory`

Katalogen som ska klonas. Standardvärdet är projekttiteln

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--environment`, `-e`

Det miljö-ID som ska klonas. Standardvärdet är projektets standardinställning eller den första tillgängliga miljön

- Kräver ett värde

#### `--depth`

Skapa en grund klon: begränsa antalet implementeringar i historiken

- Kräver ett värde

#### `--build`

Bygg projektet efter kloning

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `project:info`

```bash
magento-cloud project:info [--refresh] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<property>] [<value>]
```

Läsa eller ange egenskaper för ett projekt

### Argument

#### `property`

Egenskapens namn


#### `value`

Ange ett nytt värde för egenskapen

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--refresh`

Om cachen ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `project:list`

```bash
magento-cloud projects [--pipe] [--region REGION] [--title TITLE] [--my] [--refresh REFRESH] [--sort SORT] [--reverse] [--page PAGE] [-c|--count COUNT] [--format FORMAT] [--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT]
```

Hämta en lista över alla aktiva projekt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--pipe`

Skriv ut en enkel lista med projekt-ID:n. Inaktiverar sidnumrering.

- Standard: `false`
- Accepterar inte ett värde

#### `--region`

Filtrera efter region (exakt matchning)

- Kräver ett värde

#### `--title`

Filtrera efter titel (skiftlägesokänslig sökning)

- Kräver ett värde

#### `--my`

Visa endast de projekt du äger

- Standard: `false`
- Accepterar inte ett värde

#### `--refresh`

Om listan ska uppdateras

- Standard: `1`
- Kräver ett värde

#### `--sort`

En egenskap som ska sorteras efter

- Standard: `title`
- Kräver ett värde

#### `--reverse`

Sortera i omvänd ordning (fallande)

- Standard: `false`
- Accepterar inte ett värde

#### `--page`

Sidnummer. Detta aktiverar sidnumrering, trots konfiguration eller —count. Ignoreras om —pipe har angetts.

- Kräver ett värde

#### `--count`, `-c`

Antalet projekt som ska visas per sida. Använd 0 för att inaktivera sidnumrering. Ignoreras om —page har angetts.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`

Kolumner att visa. Tillgängliga kolumner: id*, title*, region*, created_at, organization_id, organization_label, organization_name, status (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde


## `project:set-remote`

```bash
magento-cloud set-remote [<project>]
```

Ange fjärrprojektet för den aktuella Git-databasen

### Argument

#### `project`

Projekt-ID

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `repo:cat`

```bash
magento-cloud repo:cat [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] <path>
```

Läsa en fil i projektdatabasen

### Argument

#### `path`

Sökvägen till filen

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--commit`, `-c`

Verkställ SHA. Detta kan även acceptera &quot;HEAD&quot; och cirkumflex (^) eller tilde (~)-suffix för överordnade implementeringar.

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `repo:ls`

```bash
magento-cloud repo:ls [-d|--directories] [-f|--files] [--git-style] [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

Visa filer i projektdatabasen

### Argument

#### `path`

Sökvägen till en underkatalog

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--directories`, `-d`

Visa endast kataloger

- Standard: `false`
- Accepterar inte ett värde

#### `--files`, `-f`

Visa endast filer

- Standard: `false`
- Accepterar inte ett värde

#### `--git-style`

Formatutdata som liknar &quot;git ls-tree&quot;

- Standard: `false`
- Accepterar inte ett värde

#### `--commit`, `-c`

Verkställ SHA. Detta kan även acceptera &quot;HEAD&quot; och cirkumflex (^) eller tilde (~)-suffix för överordnade implementeringar.

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `repo:read`

```bash
magento-cloud read [-c|--commit COMMIT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<path>]
```

Läsa en katalog eller fil i projektdatabasen

### Argument

#### `path`

Sökvägen till katalogen eller filen

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--commit`, `-c`

Verkställ SHA. Detta kan även acceptera &quot;HEAD&quot; och cirkumflex (^) eller tilde (~)-suffix för överordnade implementeringar.

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `route:get`

```bash
magento-cloud route:get [--id ID] [-1|--primary] [-P|--property PROPERTY] [--refresh] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE] [--] [<route>]
```

Visa detaljerad information om ett flöde

### Argument

#### `route`

Vägets ursprungliga URL

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--id`

Ett rutt-ID att välja

- Kräver ett värde

#### `--primary`, `-1`

Välj primärt flöde

- Standard: `false`
- Accepterar inte ett värde

#### `--property`, `-P`

Egenskapen som ska visas

- Kräver ett värde

#### `--refresh`

Kringgå cacheminnet för vägar

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

[Inaktuellt alternativ, används inte längre]

- Kräver ett värde

#### `--identity-file`, `-i`

[Inaktuellt alternativ, används inte längre]

- Kräver ett värde


## `route:list`

```bash
magento-cloud routes [--refresh] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--] [<environment>]
```

Visa alla vägar för en miljö

### Argument

#### `environment`

Miljö-ID

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--refresh`

Kringgå cacheminnet för vägar

- Standard: `false`
- Accepterar inte ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: flöde*, typ*, till*, url (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `self:install`

```bash
magento-cloud self:install [--shell-type SHELL-TYPE]
```

Installera eller uppdatera CLI-konfigurationsfiler

```
This command automatically installs shell configuration for the Magento Cloud CLI,
adding autocompletion support and handy aliases. Bash and ZSH are supported.
```

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--shell-type`

Gränssnittstypen för automatisk komplettering (bash eller zsh)

- Kräver ett värde


## `self:update`

```bash
magento-cloud update [--no-major] [--unstable] [--manifest MANIFEST] [--current-version CURRENT-VERSION] [--timeout TIMEOUT]
```

Uppdatera CLI till den senaste versionen

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--no-major`

Uppdatera endast mellan mindre versioner eller korrigeringsversioner

- Standard: `false`
- Accepterar inte ett värde

#### `--unstable`

Uppdatera till en ny instabil version, om en sådan finns

- Standard: `false`
- Accepterar inte ett värde

#### `--manifest`

Åsidosätt manifestfilens plats

- Kräver ett värde

#### `--current-version`

Åsidosätt den aktuella versionen

- Kräver ett värde

#### `--timeout`

En timeout för versionskontrollen

- Standard: `30`
- Kräver ett värde


## `service:list`

```bash
magento-cloud services [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Visa tjänster i projektet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--refresh`

Om cachen ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--pipe`

Skriv endast ut en lista med tjänstnamn

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: disk, namn, storlek, typ. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde


## `service:mongo:dump`

```bash
magento-cloud mongodump [-c|--collection COLLECTION] [-z|--gzip] [-o|--stdout] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Skapa en binär arkivdump av data från MongoDB

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--collection`, `-c`

Samlingen som ska dumpas

- Kräver ett värde

#### `--gzip`, `-z`

Komprimera dumpen med gzip

- Standard: `false`
- Accepterar inte ett värde

#### `--stdout`, `-o`

Utdata till STDOUT i stället för en fil

- Standard: `false`
- Accepterar inte ett värde

#### `--relationship`, `-r`

Den tjänstrelation som ska användas

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde


## `service:mongo:export`

```bash
magento-cloud mongoexport [-c|--collection COLLECTION] [--jsonArray] [--type TYPE] [-f|--fields FIELDS] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Exportera data från MongoDB

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--collection`, `-c`

Samlingen som ska exporteras

- Kräver ett värde

#### `--jsonArray`

Exportera data som en enda JSON-array

- Standard: `false`
- Accepterar inte ett värde

#### `--type`

Exporttyp, t.ex. &quot;csv&quot;

- Kräver ett värde

#### `--fields`, `-f`

De fält som ska exporteras

- Standard: `[]`
- Kräver ett värde

#### `--relationship`, `-r`

Den tjänstrelation som ska användas

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde


## `service:mongo:restore`

```bash
magento-cloud mongorestore [-c|--collection COLLECTION] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Återställa en binär arkivdump av data till MongoDB

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--collection`, `-c`

Samlingen som ska återställas

- Kräver ett värde

#### `--relationship`, `-r`

Den tjänstrelation som ska användas

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde


## `service:mongo:shell`

```bash
magento-cloud mongo [--eval EVAL] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Använd MongoDB-skalet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--eval`

Skicka ett JavaScript-fragment till skalet

- Kräver ett värde

#### `--relationship`, `-r`

Den tjänstrelation som ska användas

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde


## `service:redis-cli`

```bash
magento-cloud redis [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--] [<args>]
```

Åtkomst till Redis CLI

### Argument

#### `args`

Argument som ska läggas till i Redis-kommandot

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--relationship`, `-r`

Den tjänstrelation som ska användas

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde


## `snapshot:create`

```bash
magento-cloud backup [--live] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<environment>]
```

Ta en ögonblicksbild av en miljö

### Argument

#### `environment`

Miljön

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--live`

Live-säkerhetskopiering: stoppa inte miljön. Om detta anges kommer miljön att vara igång och öppen för anslutningar under säkerhetskopieringen. Detta minskar driftstoppen och riskerar att data säkerhetskopieras i ett inkonsekvent tillstånd.

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `snapshot:delete`

```bash
magento-cloud snapshot:delete [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<id>]
```

Ta bort en ögonblicksbild av miljön

### Argument

#### `id`

ID för ögonblicksbilden. Krävs i icke-interaktivt läge.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `snapshot:get`

```bash
magento-cloud snapshot:get [-P|--property PROPERTY] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--date-fmt DATE-FMT] [--] [<id>]
```

Visa en ögonblicksbild av en miljö

### Argument

#### `id`

ID för ögonblicksbilden. Standardvärdet är den senaste.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Egenskapen som ska visas.

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde


## `snapshot:list`

```bash
magento-cloud snapshots [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [--date-fmt DATE-FMT] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Visa tillgängliga ögonblicksbilder av en miljö

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `snapshot:restore`

```bash
magento-cloud snapshot:restore [--target TARGET] [--branch-from BRANCH-FROM] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<snapshot>]
```

Återställ en ögonblicksbild av miljön

### Argument

#### `snapshot`

Namnet på ögonblicksbilden. Standardvärdet är den senaste

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--target`

Den miljö som ska återställas till. Standardvärdet är ögonblicksbildens aktuella miljö

- Kräver ett värde

#### `--branch-from`

Om —target inte finns än, anger detta den överordnade för den nya miljön

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `source-operation:list`

```bash
magento-cloud source-ops [--full] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Visa källåtgärder i en miljö

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--full`

Begränsa inte längden på det kommando som ska visas. Standardgränsen är 24 rader.

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: app, command, operation. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde


## `source-operation:run`

```bash
magento-cloud source-operation:run [--variable VARIABLE] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<operation>]
```

Köra en källåtgärd

### Argument

#### `operation`

Åtgärdsnamnet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--variable`

En variabel som ska anges under åtgärden, i formatet typ:name=value

- Standard: `[]`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `ssh-cert:load`

```bash
magento-cloud ssh-cert:load [--refresh-only] [--new] [--new-key]
```

Generera ett SSH-certifikat

```
This command checks if a valid SSH certificate is present, and generates a
new one if necessary.

Certificates allow you to make SSH connections without having previously
uploaded a public key. They are more secure than keys and they allow for
other features.

Normally the certificate is loaded automatically during login, or when
making an SSH connection. So this command is seldom needed.

If you want to set up certificates without login and without an SSH-related
command, for example if you are writing a script that uses an API token via
an environment variable, then you would probably want to run this command
explicitly. For unattended scripts, remember to turn off interaction via
--yes or the MAGENTO_CLOUD_CLI_NO_INTERACTION environment variable.
```

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--refresh-only`

Uppdatera bara certifikatet om det behövs (skriv inte SSH-konfigurationen)

- Standard: `false`
- Accepterar inte ett värde

#### `--new`

Tvinga certifikatet att uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--new-key`

[Inaktuell] Använd —ny i stället

- Standard: `false`
- Accepterar inte ett värde


## `ssh-key:add`

```bash
magento-cloud ssh-key:add [--name NAME] [--] [<path>]
```

Lägg till en ny SSH-nyckel

```
This command lets you add an SSH key to your account. It can generate a key using OpenSSH.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Argument

#### `path`

Sökvägen till en befintlig offentlig SSH-nyckel

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--name`

Ett namn som identifierar nyckeln

- Kräver ett värde


## `ssh-key:delete`

```bash
magento-cloud ssh-key:delete [<id>]
```

Ta bort en SSH-nyckel

```
This command lets you delete SSH keys from your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Argument

#### `id`

ID:t för SSH-nyckeln som ska tas bort

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `ssh-key:list`

```bash
magento-cloud ssh-keys [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Hämta en lista med SSH-nycklar i ditt konto

```
This command lets you list SSH keys in your account.

Notice:
SSH keys are no longer needed by default, as SSH certificates are supported.
Certificates offer more security than keys.

To load or check your SSH certificate, run: magento-cloud ssh-cert:load
```

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: id*, title*, path*, fingeravtryck (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde


## `subscription:info`

```bash
magento-cloud subscription:info [-s|--id ID] [--date-fmt DATE-FMT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [--] [<property>] [<value>]
```

Läs eller ändra prenumerationsegenskaper

### Argument

#### `property`

Egenskapens namn


#### `value`

Ange ett nytt värde för egenskapen

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--id`, `-s`

Prenumerations-ID

- Kräver ett värde

#### `--date-fmt`

Datumformatet (som en PHP-datumformatsträng)

- Standard: `c`
- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde


## `tunnel:close`

```bash
magento-cloud tunnel:close [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Stäng SSH-tunnlar

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--all`, `-a`

Stäng alla tunnlar

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde


## `tunnel:info`

```bash
magento-cloud tunnel:info [-P|--property PROPERTY] [-c|--encode] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP]
```

Visa relationsinformation för SSH-tunnlar

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Den relationsegenskap som ska visas

- Kräver ett värde

#### `--encode`, `-c`

Utdata som base64-kodad JSON

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde


## `tunnel:list`

```bash
magento-cloud tunnels [-a|--all] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Lista SSH-tunnlar

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--all`, `-a`

Visa alla tunnlar

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: port*, projekt*, miljö*, app*, relation*, url (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde


## `tunnel:open`

```bash
magento-cloud tunnel:open [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-i|--identity-file IDENTITY-FILE]
```

Öppna SSH-tunnlar i en apps relationer

```
This command opens SSH tunnels to all of the relationships of an application.

Connections can then be made to the application's services as if they were
local, for example a local MySQL client can be used, or the Solr web
administration endpoint can be accessed through a local browser.

This command requires the posix and pcntl PHP extensions (as multiple
background CLI processes are created to keep the SSH tunnels open). The
tunnel:single command can be used on systems without these
extensions.
```

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--gateway-ports`, `-g`

Tillåt att fjärrvärdar ansluter till lokala vidarebefordrade portar

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `tunnel:single`

```bash
magento-cloud tunnel:single [--port PORT] [-g|--gateway-ports] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-A|--app APP] [-r|--relationship RELATIONSHIP] [-i|--identity-file IDENTITY-FILE]
```

Öppna en SSH-tunnel till en apprelation

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--port`

Den lokala porten

- Kräver ett värde

#### `--gateway-ports`, `-g`

Tillåt att fjärrvärdar ansluter till lokala vidarebefordrade portar

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--app`, `-A`

Namnet på fjärrprogrammet

- Kräver ett värde

#### `--relationship`, `-r`

Den tjänstrelation som ska användas

- Kräver ett värde

#### `--identity-file`, `-i`

En SSH-identitet (privat nyckel) som ska användas

- Kräver ett värde


## `user:add`

```bash
magento-cloud user:add [-r|--role ROLE] [--force-invite] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

Lägg till en användare i projektet

### Argument

#### `email`

Användarens e-postadress

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--role`, `-r`

Användarens projektroll (admin eller viewer) eller miljötypsroll (t.ex. &#39;staging:contributor&#39; eller &#39;production:viewer&#39;). Om du vill ta bort en användare från en miljötyp anger du rollen som none. Tecknen % och * kan användas som jokertecken för miljötypen, t.ex. %:viewer, för att ge användaren rollen &#39;viewer&#39; för alla typer. Rollen kan förkortas, t.ex. &#39;production:v&#39;.

- Standard: `[]`
- Kräver ett värde

#### `--force-invite`

Skicka en inbjudan, även om en inbjudan redan har skickats

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `user:delete`

```bash
magento-cloud user:delete [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] <email>
```

Ta bort en användare från projektet

### Argument

#### `email`

Användarens e-postadress

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `user:get`

```bash
magento-cloud user:get [-l|--level LEVEL] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [-r|--role ROLE] [--] [<email>]
```

Visa en användares roll

### Argument

#### `email`

Användarens e-postadress

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--level`, `-l`

Rollnivån (&#39;project&#39; eller &#39;environment&#39;)

- Kräver ett värde

#### `--pipe`

Utdata för rollen till stdout (efter att du har gjort några ändringar)

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde

#### `--role`, `-r`

[Inaktuell: använd användare:uppdatera för att ändra en användares roll(er)]

- Kräver ett värde


## `user:list`

```bash
magento-cloud users [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT]
```

Visa projektanvändare

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: e-post*, namn*, roll*, id*, beviljad_at, uppdaterad_at (* = standardkolumner). Tecknet&quot;+&quot; kan användas som platshållare för standardkolumnerna. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde


## `user:update`

```bash
magento-cloud user:update [-r|--role ROLE] [-p|--project PROJECT] [-W|--no-wait] [--wait] [--] [<email>]
```

Uppdatera användarroller i ett projekt

### Argument

#### `email`

Användarens e-postadress

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--role`, `-r`

Användarens projektroll (admin eller viewer) eller miljötypsroll (t.ex. &#39;staging:contributor&#39; eller &#39;production:viewer&#39;). Om du vill ta bort en användare från en miljötyp anger du rollen som none. Tecknen % och * kan användas som jokertecken för miljötypen, t.ex. %:viewer, för att ge användaren rollen &#39;viewer&#39; för alla typer. Rollen kan förkortas, t.ex. &#39;production:v&#39;.

- Standard: `[]`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `variable:create`

```bash
magento-cloud variable:create [-u|--update] [-l|--level LEVEL] [--name NAME] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--prefix PREFIX] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] [<name>]
```

Skapa en variabel

### Argument

#### `name`

Variabelnamnet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--update`, `-u`

Uppdatera variabeln om den redan finns

- Standard: `false`
- Accepterar inte ett värde

#### `--level`, `-l`

Den nivå som variabeln (&#39;project&#39; eller &#39;environment&#39;) ska anges på

- Kräver ett värde

#### `--name`

Variabelnamnet

- Kräver ett värde

#### `--value`

Variabelns värde

- Kräver ett värde

#### `--json`

Om variabelvärdet är JSON-formaterat

- Standard: `false`
- Kräver ett värde

#### `--sensitive`

Om variabelvärdet är känsligt

- Standard: `false`
- Kräver ett värde

#### `--prefix`

Variabelnamnets prefix som kan avgöra dess typ, t.ex. &#39;env&#39;. Gäller endast om namnet inte redan innehåller ett prefix. (t.ex. &#39;none&#39; eller &#39;env:&#39;)

- Standard: `none`
- Kräver ett värde

#### `--enabled`

Om variabeln ska aktiveras i miljön

- Standard: `true`
- Kräver ett värde

#### `--inheritable`

Om variabeln kan ärvas av underordnade miljöer

- Standard: `true`
- Kräver ett värde

#### `--visible-build`

Om variabeln ska vara synlig vid byggtillfället

- Kräver ett värde

#### `--visible-runtime`

Om variabeln ska vara synlig vid körning

- Standard: `true`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `variable:delete`

```bash
magento-cloud variable:delete [-l|--level LEVEL] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Ta bort en variabel

### Argument

#### `name`

Variabelnamnet

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--level`, `-l`

Variabelnivån (&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; eller &#39;e&#39;)

- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `variable:get`

```bash
magento-cloud vget [-P|--property PROPERTY] [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--pipe] [--] [<name>]
```

Visa en variabel

### Argument

#### `name`

Variabelns namn

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--property`, `-P`

Visa en enskild variabelegenskap

- Kräver ett värde

#### `--level`, `-l`

Variabelnivån (&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; eller &#39;e&#39;)

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--pipe`

[Föråldrat alternativ] Skriv ut endast variabelvärdet

- Standard: `false`
- Accepterar inte ett värde


## `variable:list`

```bash
magento-cloud variables [-l|--level LEVEL] [--format FORMAT] [-c|--columns COLUMNS] [--no-header] [-p|--project PROJECT] [-e|--environment ENVIRONMENT]
```

Listvariabler

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--level`, `-l`

Variabelnivån (&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; eller &#39;e&#39;)

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: is_enabled, level, name, value. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde


## `variable:update`

```bash
magento-cloud variable:update [--allow-no-change] [-l|--level LEVEL] [--value VALUE] [--json JSON] [--sensitive SENSITIVE] [--enabled ENABLED] [--inheritable INHERITABLE] [--visible-build VISIBLE-BUILD] [--visible-runtime VISIBLE-RUNTIME] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [-W|--no-wait] [--wait] [--] <name>
```

Uppdatera en variabel

### Argument

#### `name`

Variabelnamnet

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--allow-no-change`

Returnera lyckade (noll avslutningskod) om inga ändringar har angetts

- Standard: `false`
- Accepterar inte ett värde

#### `--level`, `-l`

Variabelnivån (&#39;project&#39;, &#39;environment&#39;, &#39;p&#39; eller &#39;e&#39;)

- Kräver ett värde

#### `--value`

Variabelns värde

- Kräver ett värde

#### `--json`

Om variabelvärdet är JSON-formaterat

- Standard: `false`
- Kräver ett värde

#### `--sensitive`

Om variabelvärdet är känsligt

- Standard: `false`
- Kräver ett värde

#### `--enabled`

Om variabeln ska aktiveras i miljön

- Standard: `true`
- Kräver ett värde

#### `--inheritable`

Om variabeln kan ärvas av underordnade miljöer

- Standard: `true`
- Kräver ett värde

#### `--visible-build`

Om variabeln ska vara synlig vid byggtillfället

- Kräver ett värde

#### `--visible-runtime`

Om variabeln ska vara synlig vid körning

- Standard: `true`
- Kräver ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--no-wait`, `-W`

Vänta inte tills åtgärden har slutförts

- Standard: `false`
- Accepterar inte ett värde

#### `--wait`

Vänta tills åtgärden har slutförts (standard)

- Standard: `false`
- Accepterar inte ett värde


## `worker:list`

```bash
magento-cloud workers [--refresh] [--pipe] [-p|--project PROJECT] [-e|--environment ENVIRONMENT] [--format FORMAT] [-c|--columns COLUMNS] [--no-header]
```

Hämta en lista över alla distribuerade arbetare

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--refresh`

Om cachen ska uppdateras

- Standard: `false`
- Accepterar inte ett värde

#### `--pipe`

Skriv endast ut en lista med arbetarnamn

- Standard: `false`
- Accepterar inte ett värde

#### `--project`, `-p`

Projekt-ID eller URL

- Kräver ett värde

#### `--environment`, `-e`

Händelse-ID. Använd &quot;.&quot; för att välja projektets standardmiljö.

- Kräver ett värde

#### `--format`

Utdataformatet: table, csv, tsv eller plain

- Standard: `table`
- Kräver ett värde

#### `--columns`, `-c`

Kolumner att visa. Tillgängliga kolumner: kommandon, namn, typ. Tecknen % och * kan användas som jokertecken. Värdena kan delas med kommatecken (t.ex. &quot;a,b,c&quot;) och/eller blanksteg.

- Standard: `[]`
- Kräver ett värde

#### `--no-header`

Skriv inte ut tabellrubriken

- Standard: `false`
- Accepterar inte ett värde
