---
title: Molninfrastrukturprojekt
description: Läs en översikt om Adobe Commerce i molninfrastrukturen [!DNL Cloud Console]  och lär dig hur du får åtkomst till kontoinställningarna.
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 8eed04c7-6469-45a4-aa89-dc594c977264
source-git-commit: 00b1b6578c226a304697963d17ba349ea17da260
workflow-type: tm+mt
source-wordcount: '1003'
ht-degree: 0%

---

# Molninfrastrukturprojekt

Adobe Commerce i molninfrastrukturprojektet innehåller all kod i Git-grenar, associerade miljöer och skript för att distribuera [!DNL Commerce]-programmet. Miljöer innehåller tjänster som stöder programmet [!DNL Commerce], inklusive en databas, webbserver och cachelagringsserver.

Adobe tillhandahåller ett [!DNL Cloud Console]- och utvecklingsverktyg för att hantera alla aspekter av ditt projekt fullt ut. Som kontoägare har du fullständig åtkomst till alla miljöer.

## [!DNL Cloud Console]

[!DNL Cloud Console] innehåller interaktiva metoder för att skapa, hantera och distribuera Commerce-kod i ett användarvänligt format. [Logga in på  [!DNL Cloud Console]](https://console.adobecommerce.com) om du vill visa projektlistan. Du kan bara se projekt som du har behörighet att använda som administratör eller för särskilda miljötyper. Om du är Adobe Solutions Partner kan du se flera projekt för kunder som du stöder.

>[!TIP]
>
>Om du inte ser några projekt måste du kontakta den [kontoägare eller projektadministratör](../project/user-access.md) som är associerad med projektet och begära åtkomst. För förstagångsanvändare, se [introduktionsämnet](../../get-started/onboarding.md#cloud-console) i guiden _Kom igång_.

I vyn _Alla projekt_ visas alla projekt som du har behörighet att komma åt. Du kan klicka på **[!UICONTROL Show filters]** och filtrera projektlistan efter typ, region eller plan.

![Projektlista](../../assets/ui-allprojects-list.png)

### Projektöversikt

Om du väljer ett projekt i listan _Alla projekt_ öppnas projektöversikten. I projektöversikten visas alltid ett projektnavigeringsfält, som innehåller en miljöväljare och en konfigurationsknapp:

![Projektnavigering](../../assets/project-nav.png)

I projektöversikten visas en sammanfattning av projektinformationen i förhandsvisningsområdet, så länge du inte har valt någon miljö:

- Projektnamn
- Region, projekt-ID
- Planera, allokerad lagring, miljöer, användare
- Lagra URL med knappen **[!UICONTROL Set a custom domain]**

Och i huvudprojektöversikten:

- Miljövyn visar en lista eller trädvy över ![aktiv gren](../../assets/icon-active.png){width="32"} (aktiv) och ![inaktiv gren](../../assets/icon-inactive.png){width="32"} (inaktiv).
- [Aktivitetsströmmen](activity-stream.md) visar pågående, väntande och senaste aktiviteter för projektet.
<!-- - Apps & Services—Shows a topology of service containers -->

För **startprojekt** finns det en hierarki med grenar som börjar från `master` (produktion). Alla grenar som du skapar visas som underordnade från grenen `master`. Adobe rekommenderar att du skapar en `staging`-gren och sedan skapar en `integration`-gren för utveckling. Se [Startarkitektur](../architecture/starter-architecture.md).

För **Pro** finns det en hierarki med grenar från `production` till `staging` till `integration`. Ikonen ![Dedikerad ikon](../../assets/icon-dedicated.png){width="32"} anger att grenen distribuerar till en dedikerad miljö. Alla grenar som du skapar visas som underordnade grenen `integration`. Se [Pro-arkitektur](../architecture/pro-architecture.md).

![Proffsmiljölista](../../assets/pro-environments.png)

### Miljööversikt

Om du väljer en miljö i projektnavigeringsfältet ändras översikten och navigeringsfältet så att det fokuserar på den valda miljön. Navigeringsfältet innehåller förgreningskontroller (förgrening, sammanfogning och synkronisering) och en konfigurationsknapp:

![Miljö vald](../../assets/environment-selected.png)

I miljööversikten visas en sammanfattning av miljöinformationen i förhandsvisningsområdet:

- Miljönamn, typ
- Region, projekt-ID
- Datum och tid för senaste aktivitet, inklusive säkerhetskopiering
- HTTP-åtkomst och sökmotorstatus
- Datornamn som tilldelats miljön
- Miljöstatus (aktiv eller inaktiv)
- Lagra URL med knappen **[!UICONTROL Set a custom domain]**

Och i huvudmiljööversikten:

- [Aktivitetsström](activity-stream.md) utgör huvudmiljööversikten och visar aktiviteter som körs, väntar och nyligen har utförts för den valda miljön.
<!-- - Services tab shows and Apps & Services menu, including overview and configuration tabs for each service. -->
- [Fliken Säkerhetskopior](../storage/snapshots.md#create-a-manual-backup) innehåller en lista över lagrade säkerhetskopieringar, historik över säkerhetskopieringsåtgärder och säkerhetskopieringsknappen.

### Åtkomstbutik

Varje aktiv miljö har en bakgrund. Välj en miljö i den översta navigeringen och klicka på URL-adressen i miljööversikten. Det finns även en **[!UICONTROL URLs]**-lista till höger ovanför aktivitetslistan.

Webbåtkomstadressen kan innehålla följande:

```
https://<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud/
```

- **Unikt ID** = 7 slumpmässiga alfanumeriska tecken
- **Projekt-ID** = projekt-ID med 13 tecken
- **Region** = AWS- eller Azure-regionnamn, se [Regionala IP-adresser](regional-ip-addresses.md)

Proproduktion- och mellanlagringsmiljöerna innehåller tre noder som du kommer åt via följande länkar:

- URL för belastningsutjämnare:

   - `http[s]://<your-domain>.c.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.c.<project-ID>.ent.magento.cloud`

- Direktåtkomst till en av de tre redundanta servrarna:

   - `http[s]://<your-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`
   - `http[s]://<your-staging-domain>.{1|2|3}.<project-ID>.ent.magento.cloud`

  Produktions-URL:en används av leveransnätverket (CDN).

## Inställningar

Öppna panelen _Inställningar_ genom att klicka på ikonen ![Konfigurera projekt](../../assets/icon-configure.png){width="36"} (konfigurera) till höger i projektnavigeringen.

### Projektinställningar

**[!UICONTROL Project Settings]** utökar en meny med kontroller på projektnivå för att hantera användare, variabler och mycket annat:

| Alternativ | Beskrivning |
|--------------|-------------------------------------------------------------------------------------------------------------------------------|
| Allmänt | Hantera tidszonen för användning med schemaläggning av säkerhetskopieringar eller underhåll. |
| Åtkomst | Hantera [användaråtkomst](user-access.md) för projekt- och miljötyper. |
| Certifikat | Visa en lista över SSL-certifikat som är associerade med projektet. |
| Distribuera nyckel | Lägg till och visa den offentliga nyckeln i projektkoddatabasen. |
| Domäner | Lägg till ett domännamn i projektet. Se [Hantera domäner](../cdn/fastly-custom-cache-configuration.md#manage-domains). |
| Integreringar | Lägg till och hantera [integreringar](../integrations/overview.md), till exempel hälsomeddelanden och webbböcker. |
| Variabel | Lägg till [projektnivåvariabler](../environment/variable-levels.md) som är tillgängliga vid bygge och körning i alla miljöer. |

{style="table-layout:auto"}

### Miljöinställningar

Klicka på **[!UICONTROL Environments]** och välj en specifik miljö i listan för kontroller för att hantera platsinställningar, miljövariabler och mycket annat:

| Alternativ | Beskrivning |
| --------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Allmänt | Konfigurera visningsnamn, miljötyp och överordnad miljö.<br>Växla mellan olika miljöinställningar: |
|           | **Aktivera utgående e-post**: Skicka [utgående e-post](outgoing-emails.md) från miljön med protokollet SMTP. |
|           | **Dölj från sökmotorer**: Blockera sökmotorindexerare och crawlningar från webbplatsen. |
|           | **HTTP-åtkomstkontroll**: Aktivera säkerhetskonfiguration för [!DNL Cloud Console] med en åtkomstkontroll för inloggning och IP-adresser. |
|           | Status är `active` eller `inactive`. Det mesta av ditt arbete är i en aktiv miljö. Du kan inaktivera eller ta bort miljön. |
| Variabel | Visa, skapa och hantera [miljönivåvariabler](../environment/variable-levels.md) som är tillgängliga vid körning. |
| Domäner | Visa en lista med [konfigurerade vägar](../routes/routes-yaml.md). |

{style="table-layout:auto"}

>[!WARNING]
>
>**Använd INTE** HTTP-åtkomstkontrollsmetoden för att skydda Pro Staging- och Production-miljöer. Det här avbryter cachelagring. Använd i stället funktionen [Blockera](../cdn/fastly-vcl-blocking.md) som finns i snabbnätverket för CDN för Adobe Commerce för att blockera åtkomst, eller implementera åtkomstkontroll med [Snabbt grundläggande autentisering](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md).

## Snabbt och New Relic

Ditt projekt innehåller [Fast](../cdn/fastly.md) och [New Relic](../monitor/new-relic-service.md). Projektinformationen visar information för din projektplan och viktiga licenser och tokens för dessa integreringar. Endast licensägaren har initial åtkomst till autentiseringsuppgifterna och tjänsterna. Ge de här autentiseringsuppgifterna till tekniska resurser och utvecklarresurser efter behov.

- [Snabbt](https://www.fastly.com/) tillhandahåller innehållsleverans (CDN), bildoptimering och säkerhetstjänster (DDoS och WAF) för Adobe Commerce i molninfrastrukturprojekt. Se [Få snabbt inloggningsuppgifter](../cdn/fastly-configuration.md#get-fastly-credentials).

- [New Relic](../monitor/new-relic-service.md) innehåller programmått och prestandainformation för miljö för förproduktion och produktion.

Använd [Cloud CLI](../dev-tools/cloud-cli-overview.md) för att granska dina integreringstoken, ID:n med mera:

```bash
magento-cloud subscription:info services
```
