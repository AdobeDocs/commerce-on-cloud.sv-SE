---
title: Anpassad VCL för blockeringsbegäranden
description: Blockera inkommande begäranden via IP-adresser med hjälp av en ACL-lista (Edge Access Control List) med ett anpassat VCL-fragment.
feature: Cloud, Configuration, Security
exl-id: eb21c166-21ae-4404-85d9-c3a26137f82c
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# Anpassad VCL för blockeringsbegäranden

Du kan använda modulen Snabbt CDN för Magento 2 för att skapa en Edge ACL med en lista över IP-adresser som du vill blockera. Sedan kan du använda den listan med ett VCL-fragment för att blockera inkommande begäranden. Koden kontrollerar IP-adressen för den inkommande begäran. Om den matchar en IP-adress som ingår i ACL-listan blockerar snabbbegäran från att få åtkomst till din plats och returnerar en `403 Forbidden error`. Alla andra IP-adresser för klienter har åtkomst.

**Förutsättningar:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Lista över IP-adresser för klient som ska blockeras

## Skapa Edge ACL för att blockera IP-adresser för klienter

Du skapar en Edge ACL för att definiera listan över IP-adresser som ska blockeras. När du har skapat ACL-listan kan du använda den i ett anpassat VCL-kodfragment för att hantera åtkomsten till din mellanlagrings- eller produktionsplats.

Hantera åtkomsten till både mellanlagrings- och produktionsplatser genom att skapa Edge ACL med samma namn i båda miljöerna. VCL-fragmentkoden gäller båda miljöerna.

1. Logga in på Admin.
1. Navigera till **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System** > **Fullsidescache** > **Snabbt konfigurering**.
1. Expandera avsnittet **Edge ACL**.
1. Klicka på **Lägg till ACL** för att skapa en lista. I det här exemplet ger du listan namnet&quot;blockeringslista&quot;.
1. Ange IP-adressvärden i listan. Alla klient-IP-adresser som läggs till i den här listan blockeras och kan inte komma åt webbplatsen.
1. Du kan också markera kryssrutan **Negerad** om det behövs.

Du refererar till Edge ACL efter namn i VCL-kodfragmentkoden.

## Skapa anpassad VCL för blockeringslista

>[!NOTE]
>
>I det här exemplet visas hur avancerade användare skapar ett VCL-kodfragment för att konfigurera anpassade blockeringsregler för överföring till tjänsten Snabbt. Du kan konfigurera ett blockeringslista eller tillåtelselista baserat på land från Adobe Commerce Admin med funktionen [Blockera](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) som finns i modulen Snabbt CDN för Magento 2.

När du har definierat Edge ACL kan du använda det för att skapa VCL-kodfragmentet och blockera åtkomst till de IP-adresser som anges i ACL. Du kan använda samma VCL-kodfragment i både Staging- och Production-miljöer, men du måste överföra fragmentet till varje miljö separat.

Följande anpassade VCL-kodfragment (JSON-format) visar logiken för att blockera inkommande begäranden med en klient-IP-adress som matchar en adress i blockeringslista-ACL.

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

Innan du skapar ett fragment baserat på det här exemplet ska du granska värdena för att avgöra om du behöver göra några ändringar:

- `name`: VCL-fragmentets namn. I det här exemplet använde vi namnet `blocklist`.

- `priority`: Avgör när VCL-fragmentet körs. Prioriteten är `5` för att omedelbart köra och kontrollera om en administratörsbegäran kommer från en tillåten IP-adress. Utdraget körs innan något av Magento VCL-standardfragmenten (`magentomodule_*`) har tilldelats en prioritet på 50. Ange prioriteten för varje anpassat fragment som är högre eller lägre än 50, beroende på när du vill att fragmentet ska köras. Fragment med lägre prioritetsnummer körs först.

- `type`: Anger den typ av VCL-fragment som bestämmer fragmentets plats i den genererade VCL-koden. I det här exemplet använder vi `recv`, som infogar VCL-koden i underrutinen `vcl_recv`, nedanför mallens VCL och ovanför eventuella objekt. I [Snabbt VCL-fragmentreferens](https://docs.fastly.com/api/config#api-section-snippet) finns en lista med fragmenttyper.

- `content`: Det VCL-kodfragment som ska köras, som kontrollerar klientens IP-adress. Om IP-adressen finns i Edge ACL blockeras den från åtkomst med ett `403 Forbidden`-fel för hela webbplatsen. Alla andra IP-adresser för klienter har åtkomst.

När du har granskat och uppdaterat koden för din miljö använder du någon av följande metoder för att lägga till det anpassade VCL-fragmentet i din snabbtjänstkonfiguration:

- [Lägg till det anpassade VCL-fragmentet från administratören](#add-the-custom-vcl-snippet). Den här metoden rekommenderas om du har åtkomst till Admin. (Kräver [snabbversion 1.2.58](fastly-configuration.md#upgrade-fastly-module) eller senare.)

- Spara JSON-kodexemplet till en fil (till exempel `blocklist.json`) och [överför det med snabbprogrammeringsgränssnittet](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Använd den här metoden om du inte kan komma åt administratören.

## Lägg till anpassat VCL-fragment

{{admin-login-step}}

1. Klicka på **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System**.

1. Expandera **Helsidescache** > **Snabb konfiguration** > **Anpassade VCL-kodfragment**.

1. Klicka på **Skapa anpassat fragment**.

1. Lägg till VCL-fragmentvärden:

   - **Namn** — `blocklist`

   - **Typ** — `recv`

   - **Prioritet** — `5`

   - Lägg till **VCL**-fragmentinnehållet:

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. Klicka på **Skapa** om du vill generera VCL-utdragsfilen med namnmönstret `type_priority_name.vcl`, till exempel `recv_5_blocklist.vcl`

1. När sidan har lästs in på nytt klickar du på **Överför VCL till Snabbt** i avsnittet *Snabbkonfiguration* för att lägga till filen i snabbtjänstkonfigurationen.

1. Uppdatera cacheminnet enligt meddelandet högst upp på sidan när överföringen är klar.

Snabbt validerar den uppdaterade versionen av VCL-koden under överföringsprocessen. Om valideringen misslyckas kan du åtgärda problemet genom att redigera det anpassade VCL-fragmentet. Ladda sedan upp VCL-filen igen.

## Ytterligare VCL-exempel för blockeringsbegäranden

I följande exempel visas hur du blockerar begäranden med hjälp av infogade villkorssatser i stället för en ACL-lista.

>[!WARNING]
>
>I de här exemplen formateras VCL-koden som en JSON-nyttolast som kan sparas i en fil och skickas i en Fast API-begäran. Du kan skicka [VCL-fragmentet från Admin](#add-the-custom-vcl-snippet) eller som en JSON-sträng med API:t Snabb. För att förhindra valideringsfel när du använder API:t Fastly med en JSON-sträng måste du använda ett omvänt snedstreck för att undvika specialtecken.

>[!NOTE]
>Om du skickar VCL-kodfragmentet från administratören extraherar du de enskilda värdena från VCL-exempelkoden och anger dem i motsvarande fält. Exempel:
>- Namn: `<name of the VCL>`
>- Dynamisk: `<0/1>`
>- Typ: `<type>`
>- Prioritet: `<priority>`
>- Innehåll: `<content>`

Se [Använda dynamiska VCL-kodfragment](https://docs.fastly.com/vcl/vcl-snippets/) i dokumentationen för Snabbt VCL.

### Exempel på VCL-kod: Blockera efter landskod

I det här exemplet används ISO 3166-1-landskoden med två tecken för det land som är associerat med IP-adressen.

```json
{
  "name": "blockbycountrycode",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.geo.country_code == \"HK\" ) { error 405 \"Not allowed\";}"
}
```

>[!NOTE]
>
>I stället för att använda ett anpassat VCL-kodfragment kan du använda funktionen [Blockera](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) snabbt i Adobe Commerce-administratören för molninfrastruktur för att konfigurera blockering efter landskod eller en lista med landskoder.

### Exempel på VCL-kod: Blockera med huvudet för HTTP-användaragentbegäran

```json
{
  "name": "blockbyuseragent",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( req.http.User-Agent ~ \"(UCBrowser|MQQBrowser|LieBaoFast|Mb2345Browser)\" ) {error 405 \"Not allowed\";}"
}
```

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
