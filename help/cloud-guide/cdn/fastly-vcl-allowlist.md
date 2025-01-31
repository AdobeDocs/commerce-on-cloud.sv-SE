---
title: Anpassad VCL för att tillåta begäranden
description: Filtrera inkommande förfrågningar och tillåt åtkomst via IP-adress för Adobe Commerce-webbplatser genom att använda en lista med Edge ACL och anpassade VCL-fragment.
feature: Cloud, Configuration, Security
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# Anpassad VCL för att tillåta begäranden

Du kan använda en lista med snabb Edge ACL med ett anpassat VCL-kodfragment för att filtrera inkommande begäranden och tillåta åtkomst via IP-adress. ACL-listan anger vilka IP-adresser som tillåts.

Skapa en tillåtelselista för att begränsa åtkomsten till mellanlagringsmiljön så att endast begäranden från angivna IP-adresser för interna utvecklare och godkända externa tjänster tillåts. Du kan också skapa en tillåtelselista för säker åtkomst till Admin i mellanlagrings- och produktionsmiljöer.

I följande exempel visas hur du använder ett anpassat VCL-fragment med en [snabbåtkomstkontrollista (ACL)](https://docs.fastly.com/guides/access-control-lists/about-acls) för att skydda åtkomsten till Admin för en Adobe Commerce-projektmiljö i molnet. När du lägger till det anpassade VCL-fragmentet i molnmiljön tillåter snabbast endast begäranden från IP-adresser som ingår i åtkomstkontrollistan.

>[!TIP]
>
>För mellanlagrings- och integreringsmiljöer som inte ska vara allmänt tillgängliga använder du alternativet för HTTP-åtkomstkontroll i [[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface) för att hantera åtkomst till hela platsen via IP-adress.

**Förutsättningar:**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Lista över IP-adresser för klienter som ska inkluderas i tillåtelselista

## Skapa Edge ACL för att tillåta klient-IP-adresser

Edge ACL:er skapar IP-adresslistor för att hantera åtkomst till din webbplats. I det här exemplet skapar du en Edge ACL och lägger till listan över IP-adresser för klienter som har åtkomst till Admin för din projektmiljö.

{{admin-login-step}}

1. Klicka på **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System**.

1. Expandera **Helsidescache** > **Snabb konfiguration** > **Edge ACL**.

1. Skapa ACL-behållaren:

   - Klicka på **Lägg till ACL**.

   - Ange ett **ACL-namn**—`allowlist` på sidan *ACL-behållare*.

   - Välj **Aktivera efter ändringen** om du vill distribuera ändringarna till den version av snabbtjänstkonfigurationen som du redigerar.

   - Klicka på **Överför** för att ansluta åtkomstkontrollistan till din snabbtjänstkonfiguration.

1. Lägg till listan över IP-adresser som kan få åtkomst till administratören:

   - Klicka på inställningsikonen för `allowlist` ACL.

   - Lägg till och spara *IP-värdet* för varje klient-IP-adress.

   - Klicka på **Avbryt** för att återgå till systemkonfigurationssidan.

1. Klicka på **Spara konfiguration**.

1. Uppdatera cacheminnet enligt meddelandet längst upp på sidan.

## Skapa ett anpassat VCL-fragment för att skydda administratörsåtkomsten

Följande anpassade VCL-kodfragment (JSON-format) visar logiken för att filtrera begäranden till administratören och tillåta åtkomst om klientens IP-adress matchar en adress i `allowlist` ACL.

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

Innan du [skapar ett anpassat fragment](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html#add-the-custom-vcl-snippet) från det här exemplet bör du kontrollera värdena för att avgöra om du behöver göra några ändringar. Ange sedan varje värde i respektive fält, till exempel `type`, i fältet Typ `content` i fältet Innehåll.

- `name` - VCL-fragmentets namn. I det här exemplet: `allowlist`.

- `priority` - Avgör när VCL-fragmentet körs. Prioriteten är `5` för att omedelbart köra och kontrollera om en Admin-begäran kommer från en tillåten IP-adress. Utdraget körs före något av de Magento VCL-standardfragment (`magentomodule_*`) som tilldelats en prioritet på 50. Ange prioriteten för varje anpassat fragment som är högre eller lägre än 50, beroende på när du vill att fragmentet ska köras. Fragment med lägre prioritetsnummer körs först.

- `type` - Anger en plats där fragmentet ska infogas i den versionshanterade VCL-koden. Denna VCL är en `recv`-fragmenttyp som lägger till fragmentkoden i `vcl_recv`-underrutinen nedanför den förvalda snabbvariga VCL-koden och ovanför eventuella objekt.

- `content` - Det VCL-kodfragment som ska köras. I det här exemplet filtrerar koden förfrågningar till administratören och tillåter åtkomst om klientens IP-adress matchar en adress i `allowlist` ACL. Om adressen inte matchar blockeras begäran med ett `403 Forbidden`-fel.

  Om URL:en för din administratör har ändrats ska du ersätta exempelvärdet `/admin` med URL:en för din miljö. Exempel: `/company-admin`.

I kodexemplet är villkoret `!req.http.Fastly-FF` viktigt när du använder [Origin Shielding](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding). Ta inte bort eller redigera den här koden.

När du har granskat och uppdaterat koden för din miljö använder du någon av följande metoder för att lägga till det anpassade VCL-fragmentet i din snabbtjänstkonfiguration:

- [Lägg till det anpassade VCL-fragmentet från administratören](#add-the-custom-vcl-snippet). Den här metoden rekommenderas om du har åtkomst till Admin. (Kräver modulen [Snabbt CDN för Magento 2 version 1.2.58](fastly-configuration.md#upgrade) eller senare.)

- Spara JSON-kodexemplet till en fil (till exempel `allowlist.json`) och [överför det med snabbprogrammeringsgränssnittet](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Använd den här metoden om du inte kan komma åt administratören.

## Lägg till anpassat VCL-fragment

{{admin-login-step}}

1. Klicka på **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System**.

1. Expandera **Helsidescache** > **Snabb konfiguration** > **Anpassade VCL-kodfragment**.

1. Klicka på **Skapa anpassat fragment**.

1. Lägg till VCL-fragmentvärden:

   - **Namn** — `allowlist`

   - **Typ** — `recv`

   - **Prioritet** — `5`

   - Lägg till **VCL**-fragmentinnehållet:

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. Klicka på **Skapa** om du vill generera VCL-utdragsfilen med namnmönstret `type_priority_name.vcl`, till exempel `recv_5_allowlist.vcl`

1. När sidan har lästs in på nytt klickar du på **Överför VCL till Snabbt** i avsnittet *Snabbkonfiguration* för att lägga till filen i snabbtjänstkonfigurationen.

1. När överföringen är klar uppdaterar du cacheminnet enligt meddelandet längst upp på sidan.

Snabbt validerar den uppdaterade versionen av VCL-koden under överföringsprocessen. Om valideringen misslyckas kan du åtgärda problemet genom att redigera det anpassade VCL-fragmentet. Ladda sedan upp VCL-filen igen.

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
