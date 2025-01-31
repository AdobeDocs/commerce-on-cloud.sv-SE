---
title: Aktivitetsström
description: Lär dig hur du läser aktivitetsströmmen i  [!DNL Cloud Console]  eller Cloud CLI för Adobe Commerce i molninfrastrukturen.
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# Aktivitetsström

Huvudvyn för varje miljö visar en **Activity**-lista med historiska händelser som liknar en Git-logg. Listan Activity (Aktivitet) är en ström av de senaste händelserna för aktiva miljöer. Nedan följer en lista över de aktivitetstyper och deras ikoner som visas i aktivitetsströmmen:

![Aktivitetstyper](../../assets/activity-types.svg){width="500" align="center"}

## Visa loggar

Klicka på statusikonen för en aktivitet i listan Aktivitet för att visa loggen. Du kan också klicka på menyn ![Mer](../../assets/icon-more.png){width="32"} (_mer_) om du vill visa fler alternativ för att hantera aktiviteten. Nedan visas en kort logg som skapar en säkerhetskopia. Du kan [använda Cloud CLI](#activity-stream-with-cloud-cli) för att visa samma logg.

![Loggvy](../../assets/log-view.png)

## Hantera en aktivitet

Vissa aktiviteter har statusen _som kör_ eller _väntar_. Du kan agera på en aktivitet som körs, till exempel avbryta en distribution som körs. På följande flikar visas två metoder för att avbryta en aktivitet: [!DNL Cloud Console] eller Cloud CLI.

>[!BEGINTABS]

>[!TAB Konsol]

**Så här avbryter du en aktivitet i[!DNL Cloud Console]**:

Du kan agera på en aktivitet som körs genom att gå till menyn ![Mer](../../assets/icon-more.png){width="32"} (_mer_) och välja en åtgärd, till exempel `Cancel` eller `View log`. I det här exemplet väljer du alternativet **Avbryt** för att stoppa den pågående aktiviteten.

Alla aktiviteter kan inte avbrytas. Alternativet att avbryta programdistributionen visas till exempel bara under _build_ -fasen. När programmet har flyttats till _distributionsfasen_ kan du inte längre avbryta aktiviteten. Se [Distributionsprocess](../deploy/process.md) om de olika faserna.

![Avbryt aktivitet](../../assets/activity-icons/cancel-activity.png){width="450" align="center"}

Om du har en terminal som kör distributionsaktiviteten avbryts [!DNL Cloud Console] i terminalen:

![Aktiviteten avbröts i terminal](../../assets/activity-icons/activity-cancelled.png){width="300"}

>[!TAB CLI]

**Så här avbryter du en aktivitet i molnet-CLI**:

1. Identifiera de aktiviteter som körs och välj ett aktivitets-ID.

   ```bash
   magento-cloud activity:list --state=in_progress
   ```

1. Avbryt aktiviteten med aktivitets-ID:

   ```bash
   magento-cloud activity:cancel wvl5wm7s5vkhy
   ```

>[!ENDTABS]

## Filtrera aktivitetsström

Möjligheten att filtrera aktivitetslistan är användbar när du letar efter något särskilt, t.ex. en säkerhetskopia eller en sammanfogningshändelse.

**Så här filtrerar du aktivitetslistan i[!DNL Cloud Console]**:

1. Välj en miljö och välj vyn Activity **[!UICONTROL All]** för att inkludera hela händelseloggen.

1. Klicka på ![Filtrera efter](../../assets/icon-filterby.png){width="32"} och välj **[!UICONTROL Filter by]**-alternativen:

   ![Filtrera aktiviteter](../../assets/activity-filter.png)

1. Välj vyn Aktivitet **[!UICONTROL Recent]** och återställ listan.

## Visa ström med molnbaserad CLI

CLI:n `magento-cloud` tillhandahåller de flesta av funktionerna som finns i [!DNL Cloud Console]. Kommandot `activity` kan:

- `list` aktivitetsströmmen för en miljö
- `get` information om en specifik aktivitet
- visa `log` för en specifik aktivitet
- `cancel` en aktivitet

**Så här visar du aktivitetsströmmen med Cloud CLI**:

1. Visa aktiviteterna för den aktuella miljön.

   ```bash
   magento-cloud activity:list
   ```

1. Varje aktivitet har ett unikt ID. Välj ett ID i den tidigare listan och visa information om aktiviteten.

   ```bash
   magento-cloud activity:get wvl5wm7s5vkhy
   ```

1. Visa den fullständiga loggen för aktiviteten.

   ```bash
   magento-cloud activity:log wvl5wm7s5vkhy
   ```

   Exempelsvar:

   ```bash
   Activity ID: wvl5wm7s5vkhy
   Type: environment.backup
   Description: User created a backup of Master
   Created: 2023-09-08T14:03:33+00:00
   State: complete
   Log:
   Creating backup of master
   Created backup eg5pu63egt2dcojkljalzjdopa
   ```
