---
source-git-commit: 69b764a03e6c272498e57645dca40d29c4e79626
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---
# ece-tools

**Version**: 2002.2.7

Referensen innehåller 34 kommandon som är tillgängliga via kommandoradsverktyget `ece-tools`.
Den inledande listan genereras automatiskt med kommandot `ece-tools list` på Adobe Commerce i molninfrastrukturen.

## Allmänt

Den här referensen genereras från programmets kodbas. _Ge oss feedback_ (hitta länken i det övre högra hörnet) om du vill ändra innehållet. Information om riktlinjer för bidrag finns i [Kodavgifter](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Globala alternativ

#### `--help`, `-h`

Visa hjälp för det angivna kommandot. När inget kommando anges visas hjälpen för listkommandot

- Standard: `false`
- Accepterar inte ett värde

#### `--silent`

Skriv inget meddelande

- Standard: `false`
- Accepterar inte ett värde

#### `--quiet`, `-q`

Endast fel visas. Alla andra utdata inaktiveras

- Standard: `false`
- Accepterar inte ett värde

#### `--verbose`, `-v|-vv|-vvv`

Öka meddelandenas utförlighet: 1 för normal utskrift, 2 för mer utförlig utskrift och 3 för felsökning

- Standard: `false`
- Accepterar inte ett värde

#### `--version`, `-V`

Visa den här programversionen

- Standard: `false`
- Accepterar inte ett värde

#### `--ansi`

Tvinga (eller inaktivera) ANSI-utdata

- Accepterar inte ett värde

#### `--no-ansi`

Ignorera alternativet &quot;—ansi&quot;

- Accepterar inte ett värde

#### `--no-interaction`, `-n`

Ställ inga interaktiva frågor

- Standard: `false`
- Accepterar inte ett värde


## `_complete`

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

Internt kommando för att ge förslag på komplettering av skalet

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--shell`, `-s`

Skaltypen (&quot;bash&quot;, &quot;fish&quot;, &quot;zsh&quot;)

- Kräver ett värde

#### `--input`, `-i`

En array med indatatoken (t.ex. COMP_WORDS eller argv)

- Standard: `[]`
- Kräver ett värde

#### `--current`, `-c`

Indexvärdet för den inmatningsarray där markören finns (t.ex. COMP_CWORD)

- Kräver ett värde

#### `--api-version`, `-a`

API-versionen av det slutförda skriptet

- Kräver ett värde

#### `--symfony`, `-S`

inaktuell

- Kräver ett värde


## `build`

```bash
ece-tools build
```

Skapar program.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `completion`

```bash
ece-tools completion [--debug] [--] [<shell>]
```

Dumpa skriptet för gränssnittets slutförande

```
The completion command dumps the shell completion script required
to use shell autocompletion (currently, bash, fish, zsh completion are supported).

Static installation
-------------------

Dump the script to a global completion file and restart your shell:

    bin/ece-tools completion  | sudo tee /etc/bash_completion.d/ece-tools

Or dump the script to a local file and source it:

    bin/ece-tools completion  > completion.sh

    # source the file whenever you use the project
    source completion.sh

    # or add this line at the end of your "~/.bashrc" file:
    source /path/to/completion.sh

Dynamic installation
--------------------

Add this to the end of your shell configuration file (e.g. "~/.bashrc"):

    eval "$(/var/jenkins/workspace/gendocs-ece-tools-cli/bin/ece-tools completion )"
```

### Argument

#### `shell`

Gränssnittstypen (t.ex. &quot;bash&quot;), värdet för &quot;$SHELL&quot; env var, används om detta inte anges

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--debug`

Avsluta felsökningsloggen

- Standard: `false`
- Accepterar inte ett värde


## `db-dump`

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```

Skapar säkerhetskopiering av databaser.

### Argument

#### `databases`

Databaser för säkerhetskopiering. Tillgängliga värden: [försäljning av huvudofferter]. Om argumentvärdet inte anges skapas databassäkerhetskopior med hjälp av autentiseringsuppgifterna som lagras i miljövariabeln `MAGENTO_CLOUD_RELATIONSHIP` eller `stage.deploy.DATABASE_CONFIGURATION` i konfigurationsfilen .magento.env.yaml.

- Standard: `[]`
- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--remove-definers`, `-d`

Ta bort definitioner från databasdumpen

- Standard: `false`
- Accepterar inte ett värde

#### `--dump-directory`, `-a`

Använd alternativ katalog för att spara dumpen

- Kräver ett värde


## `deploy`

```bash
ece-tools deploy
```

Distribuerar programmet.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `help`

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```

Visa hjälp för ett kommando

```
The help command displays help for a given command:

  bin/ece-tools help list

You can also output the help in other formats by using the --format option:

  bin/ece-tools help --format=xml list

To display the list of available commands, please use the list command.
```

### Argument

#### `command_name`

Kommandonamnet

- Standard: `help`

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--format`

Utdataformatet (txt, xml, json eller md)

- Standard: `txt`
- Kräver ett värde

#### `--raw`

Hjälp för att skriva ut råformat

- Standard: `false`
- Accepterar inte ett värde


## `list`

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```

Listkommandon

```
The list command lists all commands:

  bin/ece-tools list

You can also display the commands for a specific namespace:

  bin/ece-tools list test

You can also output the information in other formats by using the --format option:

  bin/ece-tools list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  bin/ece-tools list --raw
```

### Argument

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

#### `--short`

Så här beskriver du inte kommandots argument

- Standard: `false`
- Accepterar inte ett värde


## `patch`

```bash
ece-tools patch
```

Använder anpassade patchar.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `post-deploy`

```bash
ece-tools post-deploy
```

Utför efter distributionsåtgärder.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `run`

```bash
ece-tools run <scenario>...
```

Kör scenarier.

### Argument

#### `scenario`

Scenario(er)

- Standard: `[]`
- Obligatoriskt

- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `backup:list`

```bash
ece-tools backup:list
```

Visar en lista med säkerhetskopierade filer.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `backup:restore`

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

Återställ viktiga konfigurationsfiler. Kör säkerhetskopian :list för att visa listan med säkerhetskopieringsfiler.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--force`, `-f`

Skriv över befintliga filer under återställning av en säkerhetskopia

- Standard: `false`
- Accepterar inte ett värde

#### `--file`

En specifik filåterställningssökväg

- Accepterar ett värde


## `build:generate`

```bash
ece-tools build:generate
```

Genererar alla filer som behövs för byggfasen.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `build:transfer`

```bash
ece-tools build:transfer
```

Överför genererade filer till init-katalogen.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `cloud:config:create`

```bash
ece-tools cloud:config:create <configuration>
```

Skapar en `.magento.env.yaml`-fil med den angivna variabelkonfigurationen för build, deploy och post-deploy. Skriver över befintlig `.magento.env.yaml`-fil.

### Argument

#### `configuration`

Konfiguration i JSON-format

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `cloud:config:update`

```bash
ece-tools cloud:config:update <configuration>
```

Uppdaterar den befintliga `.magento.env.yaml`-filen med den angivna konfigurationen. Skapar filen `.magento.env.yaml` om den inte finns.

### Argument

#### `configuration`

Konfiguration i JSON-format

- Obligatoriskt

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `cloud:config:validate`

```bash
ece-tools cloud:config:validate
```

Verifierar konfigurationsfilen `.magento.env.yaml`

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `config:dump`

```bash
ece-tools config:dumpdump
```

Dumpa konfigurationen för statisk innehållsdistribution.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `cron:disable`

```bash
ece-tools cron:disable
```

Inaktivera alla Magento cron-processer och avsluta alla processer som körs.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `cron:enable`

```bash
ece-tools cron:enable
```

Möjliggör Magento kundprocesser.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `cron:kill`

```bash
ece-tools cron:kill
```

Avslutar alla Magento kundprocesser.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `cron:unlock`

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

Lås upp cron-jobb som fastnat i körläge.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--job-code`

Upplås genom att krona jobbkoden.

- Standard: `[]`
- Accepterar flera värden


## `dev:generate:schema-error`

```bash
ece-tools dev:generate:schema-error
```

Skapar dist/error-codes.md från filen schema.error.yaml.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `dev:git:update-composer`

```bash
ece-tools dev:git:update-composer
```

Uppdaterar disposition för distribution från Git.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `env:config:show`

```bash
ece-tools env:config:show [<variable>...]
```

Visa miljövariabler för kodad molnkonfiguration.

### Argument

#### `variable`

Miljövariabler som ska visas, möjliga alternativ: tjänster, flöden, variabler

- Standard: `[]`
- Array

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `error:show`

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```

Visar information om fel med fel-ID eller information om alla fel från den senaste distributionen.

### Argument

#### `error-code`

Felkod: Om kommandot inte skickas visas information om alla fel från den senaste distributionen

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).

#### `--json`, `-j`

Används för att få resultat i JSON-format

- Standard: `false`
- Accepterar inte ett värde


## `module:refresh`

```bash
ece-tools module:refresh
```

Uppdaterar konfigurationen för att aktivera nya moduler.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `schema:generate`

```bash
ece-tools schema:generate
```

Skapar schemafilen *.dist.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `wizard:ideal-state`

```bash
ece-tools wizard:ideal-state
```

Verifierar det idealiska konfigurationstillståndet.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `wizard:master-slave`

```bash
ece-tools wizard:master-slave
```

Verifierar konfigurationen av master-slave.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `wizard:scd-on-build`

```bash
ece-tools wizard:scd-on-build
```

Verifierar SCD vid konfiguration av bygge.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `wizard:scd-on-demand`

```bash
ece-tools wizard:scd-on-demand
```

Verifierar konfiguration av SCD vid behov.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `wizard:scd-on-deploy`

```bash
ece-tools wizard:scd-on-deploy
```

Verifierar SCD vid distributionskonfiguration.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).


## `wizard:split-db-state`

```bash
ece-tools wizard:split-db-state
```

Verifierar möjligheten att dela DB och om DB redan delats eller inte.

### Alternativ

Information om globala alternativ finns i [Globala alternativ](#global-options).
