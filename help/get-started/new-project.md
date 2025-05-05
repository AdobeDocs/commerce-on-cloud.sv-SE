---
title: Etablera Commerce i molnet
description: Lär dig hur du förbereder en teknisk rådgivare från Adobe för att tillhandahålla din Adobe Commerce i molninfrastrukturprojekt.
recommendations: noDisplay, catalog
role: Admin
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 0%

---

# Krav för etablering av Commerce i molnet

Låt oss komma igång och initiera ditt Commerce-projekt för molninfrastruktur!

Innan Adobe förser din Commerce med information om molnmiljöer rekommenderar vi att du överväger följande strategier och förbereder svaren för ditt första samråd med kontoteamet på Adobe. Använd följande avsnitt som checklista för att förbereda ditt samtal med en kundtekniker för att etablera ett molnprojekt:

## Domändefinition

**Fråga 1**: _Vilken domän eller vilka domäner tänker du använda för att starta webbplatsen?_

Förbered dig för att integrera tjänsterna Snabbt och Nginx genom att definiera dina toppnivådomäner och underdomäner för Pro Staging- och Production-miljöerna. Efter den första konfigurationen kan du bara lägga till domäner genom att skicka in en Adobe Commerce Support-biljett, så vi rekommenderar att du har din domäninformation klar.

Exempel för produktions- och mellanlagringsdomäner:

- `www.your-store.com`
- `your-store.com`
- `mcprod.your-store.com`
- `mcstaging.your-store.com`

Se [Konfigurera flera webbplatser eller butiker](../cloud-guide/store/multiple-sites.md) i guiden _Commerce om molninfrastruktur_ för mer information om flera eller unika domäner.

Se [Flera snabbkonton och tilldelade domäner](https://experienceleague.adobe.com/sv/docs/commerce-on-cloud/user-guide/cdn/fastly#multiple-fastly-accounts-and-assigned-domains){target="_blank"} om du har ett befintligt Fast-konto som länkar samma index och underdomäner som används på din Adobe Commerce-webbplats.

## Domän för transaktionell e-post

**Fråga 2**: _Vilken domän eller vilka domäner tänker du använda för transaktionsmeddelanden?_

Adobe Commerce i molnet använder proxytjänsten SendGrid Simple Mail Transfer Protocol (SMTP), som tillhandahåller utgående e-postautentisering och anseendeövervakning. SendGrid skickar transaktionsbaserade e-postmeddelanden åt dig, så det kräver domäninformation.

Exempel för SendGrid-domän: `example@your-store.com`

Mer information om transaktionsmeddelanden och domäninställningar finns i [SkickaGrid-e-posttjänsten](../cloud-guide/project/sendgrid.md) i guiden _Commerce om molninfrastruktur_ .

## Lagringsallokering

**Fråga 3**: _Hur mycket av det avtalade lagringsutrymmet tänker du allokera för filöverföring och för databas?_

Adobe Commerce i molninfrastruktur använder lagring för filstrukturen, sökindexering och databasen. Du kan dela upp lagringsutrymmet efter behov med början på 10 GB för varje partition.

Mängden lagringsutrymme som du har tecknat för ditt molnprojekt motsvarar den totala lagringen för varje miljö. Om du till exempel har köpt 50 GB lagringsutrymme har du 50 GB lagringsutrymme för varje miljö. Det finns 50 GB separat lagringsutrymme för produktion, staging och varje integrationsmiljö.

Du kan när som helst öka det avtalade lagringsutrymmet. För Pro Production- och Staging-miljöer måste du skicka in en Adobe Commerce Support-biljett för att ändra diskutrymme. En storleksökning av Pro Production- och Staging-miljöer kan bara göras med vissa intervall. Beroende på hur mycket diskutrymme du använder kan supportteamet rekommendera att du ökar diskutrymme med minst 10 GB. När lagringsökningen för Pro Staging och Production har allokerats kan den **inte** återställas.

Se [Hantera diskutrymme](../cloud-guide/storage/manage-disk-space.md) i guiden _Commerce om molninfrastruktur_.

## Molntjänstregion

**Fråga 4**: _Vilket molntjänstområde passar bäst för din närhet?_

Välj antingen Amazon Web Services (AWS) eller Microsoft Azure som en grund för din infrastruktur som en tjänst (IaaS) för dina Adobe Commerce i projekt för molninfrastrukturproffs. Varje tjänsteleverantör är verksam i flera regioner och tillhandahåller flera tillgänglighetszoner. Välj ett område som passar dig och minska risken för latens och högre kostnader.

Se [en karta över Adobe Commerce molnregioner](../cloud-guide/overview.md).

## Anslutningstjänst

**Fråga 5**: _Behöver du en PrivateLink-tjänst? I så fall, i vilken region är PrivateLink-anslutningen?_

Adobe Commerce i molninfrastrukturen stöder integrering med AWS PrivateLink eller Azure Private Link-tjänsten. Även om den här tjänsten är valfri används PrivateLink för att upprätta säker, privat kommunikation mellan molninfrastrukturmiljöer med tjänster och program som finns på externa system.

Det är viktigt att tänka på din molntjänststrategi (AWS eller Azure) så att Adobe Commerce-instansen är tillgänglig inom samma region. Se [PrivateLink-tjänsten](../cloud-guide/development/privatelink-service.md) i guiden _Commerce om molninfrastruktur_ för mer information om regional tillgänglighet.

## Start av målwebbplats

**Fråga 6**: _Vad är ditt planerade startdatum?_

För att du ska kunna starta en webbplats krävs iterativ konfiguration och testning för att webbplatsen ska kunna startas. Genom att ange ett måldatum kan du och ditt kontoteam på Adobe förbereda dig för de slutliga åtgärderna före start, som inkluderar ett anrop för att samordna de sista stegen.

Se [Översikt över startwebbplatsen](../cloud-guide/launch/overview.md) i guiden _Commerce om molninfrastruktur_ om du vill granska hela processen och hämta en kopia av startchecklistan.

>[!TIP]
>
> Ta en snabb titt på molnportalen och få tillgång till ditt nya molnprojekt.
>
>**Nästa steg**: [Onboarding to Commerce](onboarding.md)
