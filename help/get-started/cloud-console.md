---
title: Logga in på  [!DNL Cloud Console]
description: Lär dig mer om infrastrukturen för  [!DNL Cloud Console] for Adobe Commerce i molnet.
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# Logga in på [!DNL Cloud Console]

[!DNL Cloud Console] innehåller interaktiva metoder för att skapa, hantera och distribuera Commerce-kod. [!DNL Cloud Console] är en modernare, användarvänlig upplevelse och lägger grunden för framtida förbättringar av gränssnittet.

[Logga in på  [!DNL Cloud Console]](https://console.adobecommerce.com) om du vill visa projektlistan.

![Projektlista](../assets/ui-allprojects-list.png)

## Funktioner

Nya eller förbättrade funktioner:

- Tydlig översikt över projekt- och miljöegenskaper
- Aktivitetsström med sorterbar historik
- Manuell hantering av säkerhetskopiering och historik för startprojekt
- Förbättrade loggvyer
- Sorterbara listor
- Enkla formulär och vägledning för att lägga till integreringar
- WCAG-kompatibilitet (Web Content Accessibility Guidelines)

![[!DNL Cloud Console]](../assets/CloudConsole.svg)

De nya eller förbättrade funktionerna är följande:

| Funktion | Förbättringar |
| -------------- | ----------------------------------- |
| [Aktivitetsström](../cloud-guide/project/activity-stream.md) | Interagera med en sorterbar lista över pågående, väntande eller historiska åtgärder. Välj en aktivitet och visa loggar eller avbryt en pågående version. |
| [Projekt- och miljööversikter](../cloud-guide/project/overview.md#project-overview) | Öppna projektet och se en översikt över projektinformation och miljölista. I miljööversikten finns mer information om miljöstatus, programåtkomst och senaste aktiviteter. |
| [Integreringsformulär](../cloud-guide/integrations/overview.md) | Använd enkla formulär och vägledning för att lägga till integreringar, som Bitbucket eller Slack. |
| [Projektlista](../cloud-guide/project/overview.md#cloud-console) | I vyn _Alla projekt_ visas alla projekt som du har behörighet att komma åt. Du kan klicka på **[!UICONTROL Show filters]** och filtrera projektlistan efter typ, region eller plan. |
| [Alternativ för variabel synlighet](../cloud-guide/environment/variable-levels.md) | Begränsa synligheten för en variabel på projektnivå eller miljönivå under bygge eller körning. |

<!-- The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | -->

## Konsolfrågor

**_Var hittar jag funktionen för ögonblicksbilder_**?

För [!DNL Starter]-projekt kallas nu funktionen för ögonblicksbilder _Säkerhetskopior_. Du kan skapa en manuell säkerhetskopia av din [!DNL Starter]-miljö från [!DNL Cloud Console] eller skapa en ögonblicksbild från Cloud CLI. Du måste ha en administratörsroll för miljön.

Välj en miljö i projektnavigeringsfältet. Miljön måste vara aktiv. Välj fliken **[!UICONTROL Backups]**. För närvarande är det här alternativet inte tillgängligt för Pro-miljöer.

**_Var finns listan över konfigurerade vägar för miljön_**?

Du hittar listan över konfigurerade vägar på fliken _Tjänster_ för en miljö.

Välj en miljö i projektnavigeringsfältet. Välj fliken **[!UICONTROL Services]**. I översikten **Router** visas de konfigurerade vägarna. Du kan för närvarande inte lägga till en väg från nya [!DNL Cloud Console].

## Menyn Konto

I det övre högra hörnet finns din kontomeny. Klicka på nedpilen för menyn och välj **[!UICONTROL My Profile]**. I vyn _Min profil_ kan du styra dina användaruppgifter och visningsinställningar, hantera [säkerhetsautentisering](../cloud-guide/project/user-access.md#user-authentication-requirements), [API-tokens](../cloud-guide/project/user-access.md#create-an-api-token) och [SSH-nycklar](../cloud-guide/development/secure-connections.md).
