---
title: Anpassad VCL för att snabbt kringgå cache
description: Felsök trafiken till den ursprungliga servern genom att skapa ett anpassat VCL-kodfragment som kringgår snabbcachen.
feature: Cloud, Configuration, Cache
exl-id: 4e19d6d4-b5a1-4623-b0be-804ddc81ff3d
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Anpassad VCL för att snabbt kringgå cache

Du kan skapa ett anpassat VCL-kodfragment som kringgår snabbcachen så att du kan felsöka begärandetrafik till den ursprungliga servern. Du kan till exempel skapa ett kodfragment som avgör om platsproblem orsakas av cachelagring eller om sidhuvuden ska felsökas.

Du kan konfigurera fragmentet så att det kringgår snabb cachelagring för begäranden från en viss IP-adress eller URL.

>[!NOTE]
>
>Innan du sammanfogar en anpassad VCL-konfiguration i en produktionsmiljö måste du testa koden i mellanlagringsmiljön.

**Förutsättningar:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Om du vill kringgå snabbcachelagring baserat på IP-adress eller URL**:

{{admin-login-step}}

1. Klicka på **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System**.

1. Expandera **Helsidescache** > **Snabb konfiguration** > **Anpassade VCL-kodfragment**.

1. Klicka på **Skapa anpassat fragment**.

1. Lägg till VCL-fragmentvärden:

   - **Namn** — `bypass_fastly`

   - **Typ** — `recv`

   - **Prioritet** — `5`

   - **VCL**-fragmentinnehåll —

     Följande exempel kringgår Fastly för en specifik IP-adress:

     ```conf
     if (client.ip == "<Your IPv4 IP address>" || client.ip == "<Your IPv6 IP address>") {
       return(pass);
     }
     ```

     Följande exempel kringgår Snabbt för ett specifikt URL-mönster:

     ```conf
     if (req.url ~ "/media/feeds/GoogleShoppingHiVisNew.xml") {  return (pass);}
     ```

     Använd operatorn `==` i stället för operatorn `~` för en exakt URL-matchning. Mer information finns i [Snabbt VCL-referens].

1. Klicka på **Skapa**.

   ![Skapa VCL-fragment med snabb åsidosättning](/help/assets/cdn/fastly-create-bypass-snippet.png)

1. När sidan har lästs in på nytt klickar du på **Överför VCL till Snabbt** i avsnittet *Snabbkonfiguration*.

1. När överföringen är klar uppdaterar du cacheminnet enligt meddelandet längst upp på sidan.

   Validerar snabbt den uppdaterade VCL-versionen under överföringsprocessen. Om valideringen misslyckas kan du åtgärda eventuella problem genom att redigera det anpassade VCL-fragmentet. Ladda sedan upp VCL-filen igen.

När du har lagt till VCL-kodfragmentet kan du använda cURL-kommandon för att skicka begäranden till den ursprungliga servern från den angivna IP-adressen eller URL:en enligt följande exempel:

```bash
curl -svo /dev/null www.example.com/index.html
```

Kontrollera sedan svaret på felsökningsproblem med det ocachelagrade innehållet.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!--External link definitions-->

[Snabb VCL-referens]: https://docs.fastly.com/vcl/

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
