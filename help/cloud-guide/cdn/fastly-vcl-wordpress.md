---
title: Omdirigera begäranden till en CMS-serverdel
description: Lär dig hur du dirigerar om inkommande begäranden från en Adobe Commerce-butik till en separat WordPress-webbplats med hjälp av modulen Snabb.
feature: Cloud, Configuration, Routes
exl-id: ef024c68-395b-4d47-9362-a8404a93dbbe
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 0%

---

# Omdirigera begäranden till en CMS-serverdel

Omdirigera inkommande begäranden från en Adobe Commerce-butik till en separat WordPress-webbplats med Snabb Edge-modul _Annan CMS/backend-integrering_ med en Edge-ordlista. Du kan följa en liknande process för att dirigera om begäranden till andra CMS-backends.

Använd Edge-moduler snabbt för att skapa och överföra anpassad VCL-kod från administratören i stället för att skriva VCL-koden manuellt och överföra den med API:t Snabb.

>[!NOTE]
>
>Vi rekommenderar att du lägger till anpassade VCL-konfigurationer i en mellanlagringsmiljö där du kan testa dem innan du uppdaterar snabbtjänstkonfigurationen i produktionsmiljön.

**Förutsättningar**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Så här dirigerar du om begäranden från Adobe Commerce till WordPress**:

1. Aktivera snabbt Edge-moduler i mellanlagrings- eller produktionsmiljön.

   - Logga in på Admin.

   - Navigera till **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System** > **Fullsidescache** > **Snabbt konfigurering** > **Avancerad konfiguration**.

   - Ange värdet för **Edge-moduler** snabbt till **Ja**.

   - Spara konfigurationen.

1. Identifiera de URL-sökvägar som ska dirigeras om till WordPress-serverdelen.

1. Utför följande uppgifter för att konfigurera tjänsten Snabbt och skapa den anpassade VCL-koden för omdirigering av begäranden till WordPress-backend.

   - Skapa en Edge-ordlista som anger sökvägarna som ska dirigeras om från Adobe Commerce Store till serverdelen.

   - Lägg till backend-objektet för WordPress i snabbtjänstkonfigurationen och bifoga villkoret för begäran om URL-omskrivning.

   - Konfigurera Edge-modulen _Annan CMS/backend-integrering_ för att hantera URL-omskrivningar från Adobe Commerce till WordPress-serverdelen.

     Mer information finns i [Snabbt Edge-moduler - Annan CMS/Backend-integrering](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) i dokumentationen för _Snabbt CDN-modulen för Magento 2_.

1. När du har uppdaterat konfigurationen för tjänsten Snabb testar du Adobe Commerce Store för att kontrollera att de angivna URL-förfrågningarna för WordPress dirigeras om korrekt.

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
