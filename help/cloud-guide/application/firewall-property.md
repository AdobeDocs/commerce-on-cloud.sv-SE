---
title: Brandväggsegenskap
description: Se exempel på hur du konfigurerar brandväggsegenskapen i Commerce-programmets konfigurationsfil.
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# Brandväggsegenskap

>[!IMPORTANT]
>
>Endast startprojekt

För Starter-projekt lägger egenskapen `firewall` till en _utgående_ brandvägg i programmet. Den här brandväggen påverkar inte inkommande begäranden. Den definierar vilka `tcp` utgående begäranden som kan _lämna_ en Adobe Commerce-webbplats. Detta kallas för egresfiltrering. Den utgående brandväggen filtrerar det som kan gå ur - gå ut eller fly från webbplatsen. Genom att begränsa vad som kan kringgås läggs ett kraftfullt säkerhetsverktyg till på servern.

## Standardbegränsningsprinciper

Brandväggen innehåller två standardprinciper som styr utgående trafik: `allow` och `deny`. `allow`-principen _tillåter_ all utgående trafik som standard. Och principen `deny` _nekar_ all utgående trafik som standard. Men när du lägger till en regel åsidosätts standardprincipen och brandväggen blockerar **all** utgående trafik som inte tillåts av regeln.

Standardprincipen är `allow` för Starter-planer. Med den här inställningen kan du vara säker på att all utgående trafik inte blockeras förrän du lägger till regler för utgående filtrering. Standardprincipen kan anges till `deny` på begäran.

**Så här kontrollerar du standardprincipen**:

```bash
magento-cloud p:curl --project PROJECT_ID /settings | grep -i outbound
```

Om du inte har begärt `deny` för din princip ska kommandot visa din princip som `allow`:

```json
"outbound_restrictions_default_policy": "allow"
```

>[!NOTE]
>
>**Nyckelhanterare**: När du lägger till en utgående regel blockerar du all utgående trafik utom de domäner, IP-adresser eller portar som du lägger till i regeln. Därför är det viktigt att ha en fullständig utgående lista definierad och testad innan du lägger till den på produktionsplatsen.

## Brandväggsalternativ

I följande exempelkonfiguration i filen `.magento.app.yaml` visas alla `firewall`-alternativ som du kan använda för att lägga till regler för urklippsfiltreringen.

```yaml
firewall:
    outbound:
        - # Common accessed domains
            domains:
                - newrelic.com
                - fastly.com
                - magento.com
                - magentocommerce.com
                - google.com
            ports:
                - 80
                - 443
            protocol: tcp # Can be omitted from rules.

        - # Adobe Stock integration
            domains:
                - account.adobe.com
                - stock.adobe.com
                - console.adobe.io
            ports:
                - 80
                - 443

        - # Payment services
            domains:
                - braintreepayments.com
                - paypal.com
            ports:
                - 80
                - 443

        - # Shipping services
            domains:
                - ups.com
                - usps.com
                - fedex.com
                - dhl.com
            ports:
                - 80
                - 443

        - # Vertex Integrated Address Cleansing
            domains:
                - mgcsconnect.vertexsmb.com
            ports:
                - 80
                - 443

        - # New Relic networks
            ips:
                - 162.247.240.0/22 # US region accounts
                - 185.221.84.0/22 # EU region accounts
            ports:
                - 443

        - # New Relic endpoints
            domains:
                - collector.newrelic.com, # US region accounts
                - collector.eu01.nr-data.net # EU region accounts
            ports:
                - 443

        - # Fastly IP ranges
            ips:
                - 23.235.32.0/20
                - 43.249.72.0/22
                - 103.244.50.0/24
                - 103.245.222.0/23
                - 103.245.224.0/24
                - 104.156.80.0/20
                - 146.75.0.0/16
                - 151.101.0.0/16
                - 157.52.64.0/18
                - 167.82.0.0/17
                - 167.82.128.0/20
                - 167.82.160.0/20
                - 167.82.224.0/20
                - 172.111.64.0/18
                - 185.31.16.0/22
                - 199.27.72.0/21
                - 199.232.0.0/16
            ports:
                - 80
                - 443
```

## Regler för filtrering av ägg

Konfigurationer för utgående brandvägg består av regler. Du kan definiera så många regler du behöver. Kraven för reglerna är följande.

**Varje regel:**

- Måste börja med ett bindestreck (`-`). Om du lägger till en kommentar på samma rad blir det lättare att dokumentera och visuellt skilja en regel från nästa.
- Du måste definiera minst ett av följande alternativ: `domains`, `ips` eller `ports`.
- `tcp`-protokollet måste användas. Eftersom det här är standardprotokollet för alla regler kan du utesluta det från regeln.
- Kan definiera `domains` eller `ips`, men inte båda i samma regel.
- Kan innehålla `yaml` kommentarer (`#`) och radbrytningar för att organisera vilka domäner, IP-adresser och portar som tillåts.

För varje regel används följande egenskaper:

### `domains`

Alternativet `domains` tillåter en lista med fullständigt kvalificerade domännamn (FQDN).

Om en regel definierar `domains` men inte `ports` tillåter brandväggen domänförfrågningar på alla portar.

### `ips`

Alternativet `ips` tillåter en lista med IP-adresser i CIDR-notationen. Du kan ange enskilda IP-adresser eller intervall med IP-adresser.

Om du vill ange en enda IP-adress lägger du till CIDR-prefixet `/32` i slutet av IP-adressen:

```
172.217.11.174/32  # google.com
```

Om du vill ange ett intervall med IP-adresser använder du beräknaren [IP-intervall till CIDR](https://ipaddressguide.com/cidr).

Om en regel definierar `ips` men inte `ports` tillåter brandväggen IP-begäranden på alla portar.

### `ports`

Alternativet `ports` tillåter en lista med portar mellan 1 och 65535. För de flesta regler i exemplet tillåter portarna `80` och `443` både HTTP- och HTTPS-begäranden. Men för New Relic tillåter reglerna bara åtkomst till domäner och IP-adresser på port `443`, vilket rekommenderas i New Relic-dokumentationen för [Nätverkstrafik](https://docs.newrelic.com/docs/new-relic-solutions/get-started/networks/#agents).

Om en regel bara definierar `ports` tillåter brandväggen åtkomst till alla domäner och IP-adresser för de definierade portarna.

>[!NOTE]
>
>Port `25`, den SMTP-port som ska skicka e-post, är alltid blockerad, utan undantag.

### `protocol`

Som vi nämnt är TCP standard och endast tillåtet protokoll för regler. UDP och dess portar tillåts inte. Därför kan du utelämna alternativet `protocol` från alla regler. Om du ändå vill ta med den måste du ange värdet till `tcp`, vilket visas i exempelens första regel.

## Söker efter domännamn som ska tillåtas

Använd följande kommando för att analysera serverns `dns.log`-fil och visa en lista över alla DNS-begäranden som platsen har loggat, så att du lättare kan identifiera de domäner som ska ingå i reglerna för adressfiltrering:

```shell
awk '($5 ~/query/)' /var/log/dns.log | awk '{print $6}' | sort | uniq -c | sort -rn
```

Det här kommandot visar även DNS-begäranden som har gjorts men blockerats av reglerna för filtrering av urkunder. Utdata visar inte vilka domäner som blockerades, bara de som begärdes. Utdata visar inte begäranden som gjorts med en IP-adress.

```
Example output:

97 magento.com
93 magentocommerce.com
88 google.com
70 metadata.google.internal.0
70 metadata.google.internal
65 newrelic.com
56 fastly.com
17 mcprod-0vunku5xn24ip.ap-4.magentosite.cloud
6 advancedreporting.rjmetrics.com
```

Domäner är, till skillnad från IP-adresser, vanligtvis mer specifika och säkra för offresfiltrering. Om du till exempel lägger till en IP-adress för en tjänst som använder ett CDN, tillåter du IP-adressen för CDN, som kan användas av hundratals eller tusentals andra domäner. Med en IP-adress kan du tillåta utgående åtkomst till tusentals andra servrar.

## Testa regler för gruppfiltrering

När du har samlat in och konfigurerat åtkomstregler för de domäner och IP-adresser som din webbplats behöver är det dags att trycka och testa.

Så här testar du regler för filtrering av utgångar:

1. Skapa ett gränssnittsskript med `curl` kommandon för att komma åt domänerna och IP-adresserna i reglerna. Inkludera kommandon som testar åtkomst till domäner och IP-adresser som ska blockeras.

1. Konfigurera en `post_deploy`-krok i din `.magento.app.yaml`-fil för att köra skriptet.

1. Flytta din `firewall`-konfiguration och ditt testskript till din `integration`-gren.

1. Granska `post_deploy`-utdata från dina `curl`-kommandon.

1. Förfina dina `firewall`-regler, uppdatera `curl`-skriptet, implementera, push och upprepa.

### `curl` skriptexempel

```shell
# curl-tests-for-egress-filtering.sh

# Use the -v option to display connection details

# Check domain access
curl -v newrelic.com
curl -v fastly.com
curl -v magento.com
curl -v magentocommerce.com
curl -v google.com
curl -v account.adobe.com
curl -v stock.adobe.com
curl -v console.adobe.io
curl -v braintreepayments.com
curl -v paypal.com
curl -v ups.com
curl -v usps.com
curl -v fedex.com
curl -v dhl.com
curl -v devdocs.magento.com

# Check domain denials
curl -v amazon.com
curl -v facebook.com
curl -v twitter.com

# IP address access
...

# IP address denials
...
```

### `post_deploy`-exempel

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: "php ./vendor/bin/ece-tools run scenario/deploy.xml\n"
    post_deploy: |
        set -e
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
        echo "[$(date)] post-deploy hook end"
        ./curl-tests-for-egress-filtering.sh
        echo "[$(date)] curl finished"
```
