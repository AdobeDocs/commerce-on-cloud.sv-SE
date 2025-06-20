---
title: Kom igång med anpassade VCL-kodfragment
description: Lär dig mer om hur du använder kodfragment för varianskontrollspråk för att anpassa konfigurationen av snabbtjänsten för Adobe Commerce.
feature: Cloud, Configuration, Services
exl-id: 90f0bea6-4365-4657-94e9-92a0fd1145fd
source-git-commit: 71fb8f5b3f32553d8b247de44fea29b1bb945584
workflow-type: tm+mt
source-wordcount: '2037'
ht-degree: 0%

---

# Komma igång med anpassad VCL

Stöd snabbt för en anpassad version av VCL (Varnish Configuration Language) för att anpassa snabbtjänstkonfigurationen efter dina behov.

Anpassade VCL-kodfragment är block med VCL-logik som lagts till i den aktiva VCL-versionen som överförts till din Adobe Commerce-webbplats. Ett anpassat VCL-fragment ändrar hur Fast caching-tjänster svarar på förfrågningstrafik. Du kan t.ex. lägga till ett anpassat VCL-kodfragment som endast tillåter begärandetrafik från angivna IP-adresser för klienten. Du kan också skapa ett fragment som blockerar trafik från webbplatser som är kända för att skicka skräppost till dina Adobe Commerce-webbplatser.

Anpassade VCL-kodfragment - genererade, kompilerade och överförda till alla fast cacheminnen - laddas och aktiveras utan serverdriftavbrott.

>[!NOTE]
>
>Innan du lägger till anpassad VCL-kod, kantordlistor och åtkomstkontrollistor i din snabbmodulskonfiguration måste du kontrollera att cachelagringstjänsten Snabbt fungerar med standardkonfigurationen. Se [Konfigurera snabbtjänster](fastly-configuration.md).

Stöd för två typer av anpassade VCL-fragment:

- [Vanliga fragment](https://docs.fastly.com/en/guides/about-vcl-snippets) - Anpassade vanliga VCL-fragment kodas för specifika VCL-versioner. Du kan skapa, ändra och distribuera vanliga VCL-kodfragment från Admin eller Fast API.

- [Dynamiska fragment](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets) - VCL-fragment skapade med API:t Snabb. Du kan ändra och distribuera dynamiska kodfragment utan att behöva uppdatera den snabbaste VCL-versionen för tjänsten.

Vi rekommenderar att du använder anpassade VCL-kodfragment med Edge-ordlistor och ACL-listor (Access Control Lists) för att lagra data som används i din anpassade kod.

- [**Edge-ordlista**](https://docs.fastly.com/guides/edge-dictionaries/about-edge-dictionaries) - Lagrar data som nyckelvärdepar i en ordlistebehållare som kan refereras från anpassade VCL-fragment

- [**Edge ACL**](https://docs.fastly.com/guides/access-control-lists/about-acls) - Lagrar klientens IP-adressdata som definierar åtkomstkontrollistan för block eller tillåt regler som implementerats med anpassade VCL-fragment

Ordlistan och ACL-data distribueras till Edge-noder med snabb åtkomst över olika nätverksregioner. Data kan också uppdateras dynamiskt i nätverket utan att du behöver omdistribuera VCL-koden för din staging- eller produktionsmiljö.

>[!NOTE]
>
>Du kan bara lägga till anpassade VCL-kodfragment i en förproduktionsmiljö eller produktionsmiljö om du har [konfigurerat Snabba tjänster](fastly-configuration.md) för den miljön.

## Självstudiekurs

I den här självstudiekursen och exemplen visas hur du använder vanliga anpassade VCL-fragment med Edge-ordlistor och Edge ACL:er för att anpassa snabbtjänstkonfigurationen för Adobe Commerce. Mer detaljerad information finns i Snabb-dokumentationen:

- [Guide to Fastly VCL](https://docs.fastly.com/guides/vcl/guide-to-vcl) - Information om Snabb lack-implementering, Fast VCL-tillägg och resurser för att lära dig mer om Varnish och VCL.
- [Snabb VCL-referens](https://docs.fastly.com/guides/vcl/) - Detaljerad programmeringsreferens för att utveckla och felsöka snabbt anpassade VCL- och anpassade VCL-kodfragment.

Du kan skapa och hantera anpassade VCL-kodfragment från Adobe Commerce Admin eller via Fast API:

- [Adobe Commerce Admin](#manage-custom-vcl-from-admin) - Vi rekommenderar att du använder Adobe Commerce Admin för att hantera anpassade VCL-fragment eftersom det automatiserar processen att validera, överföra och tillämpa VCL-ändringarna i snabbtjänstkonfigurationen. Du kan även visa och redigera de anpassade VCL-kodfragment som lagts till i snabbtjänstkonfigurationen från Admin.

- [Snabb API](#manage-vcl-using-the-api) - Om du inte kan komma åt administratören kan du använda snabbgränssnittet för att hantera anpassade VCL-fragment. Använd till exempel API:t för att felsöka snabbtjänstkonfigurationen när platsen är nere, eller för att lägga till ett anpassat VCL-fragment. Vissa åtgärder kan bara slutföras med API:t. Du måste till exempel använda API:t för att återaktivera en äldre VCL-version eller för att visa alla VCL-kodfragment som ingår i en angiven VCL-version. Se [API-snabbreferens för VCL-fragment](#api-quick-reference-for-vcl-snippets).

### Exempel på VCL-kodfragment

I följande exempel visas det anpassade VCL-kodfragment (JSON-format) som filtrerar trafik efter klientens IP-adress:

```json
{
  "service_id": "FASTLY_SERVICE_ID",
  "version": "{Editable Version #}",
  "name": "apply_acl",
  "priority": "100",
  "dynamic": "1",
  "type": "hit",
  "content": "if ((client.ip ~ {ACLNAME}) && !req.http.Fastly-FF){ error 403; }"
}
```

>[!WARNING]
>
>I det här exemplet formateras VCL-koden som en JSON-nyttolast som kan sparas i en fil och skickas i en Fast API-begäran. Om du vill förhindra JSON-valideringsfel när du skickar fragmentet som JSON för en API-begäran använder du ett omvänt snedstreck för att undvika specialtecken i koden. Se [Använda dynamiska VCL-kodfragment](https://docs.fastly.com/vcl/vcl-snippets/) i dokumentationen för Snabbt VCL. Om du skickar VCL-kodfragmentet från administratören behöver du inte undvika specialtecken.

VCL-logiken i fältet `content` utför följande åtgärder:

- Kontrollerar den inkommande IP-adressen, `client.ip` för varje begäran

- Blockerar alla begäranden med en IP-adress som ingår i *ACLNAME* edge ACL, vilket returnerar ett `403 Forbidden`-fel

Följande tabell innehåller information om nyckeldata för anpassade VCL-fragment. Mer information finns i referensen [VCL-kodfragment](https://docs.fastly.com/api/config#api-section-snippet) i dokumentationen för Snabb.

| Värde | Beskrivning |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `API_KEY` | API-nyckeln för att komma åt ditt snabbkonto. Se [Hämta autentiseringsuppgifter](fastly-configuration.md). |
| `active` | Aktiv status för utdrag eller version. Returnerar `true` eller `false`. Om true används fragmentet eller versionen. Klona ett aktivt fragment med dess versionsnummer. |
| `content` | Det VCL-kodfragment som ska köras. Det går inte att använda alla språkfunktioner för VCL snabbt. Dessutom har Fastly tillägg med anpassade funktioner. Mer information om vilka funktioner som stöds finns i [Snabbt VCL-programmeringsreferens](https://docs.fastly.com/vcl/reference/). |
| `dynamic` | Dynamisk status för ett fragment. Returnerar `false` för [vanliga fragment](https://docs.fastly.com/en/guides/about-vcl-snippets) som ingår i den versionshanterade VCL-filen för snabbtjänstkonfigurationen. Returnerar `true` för ett [dynamiskt fragment](https://docs.fastly.com/vcl/vcl-snippets/using-dynamic-vcl-snippets/) som kan ändras och distribueras utan att en ny VCL-version krävs. |
| `number` | VCL-versionsnummer där fragmentet ingår. *Redigerbar version #* används snabbt i sina exempelvärden. Om du lägger till anpassade kodfragment från API:t inkluderar du versionsnumret i API-begäran. Om du lägger till anpassad VCL från administratören tillhandahålls versionen åt dig. |
| `priority` | Numeriskt värde från `1` till `100` som anger när den anpassade VCL-fragmentkoden körs. Fragment med lägre prioritetsvärden körs först. Om inget anges blir `priority`-värdet som standard `100`.<p>Alla anpassade VCL-kodfragment med prioritetsvärdet `5` körs omedelbart, vilket är bäst för VCL-kod som implementerar routning av begäranden (block, tillåtelselista och omdirigeringar). Prioriteten `100` är bäst för att åsidosätta VCL-standardkodfragment.<p>Alla [VCL-standardfragment](fastly-configuration.md#upload-vcl-snippets) som ingår i modulen Magento-Snabbt har `priority=50`.<ul><li>Tilldela en hög prioritet som `100` om du vill köra anpassad VCL-kod efter alla andra VCL-funktioner och åsidosätta standardkoden för VCL.</li></ul> |
| `service_id` | Snabbt service-ID för en viss staging- eller produktionsmiljö. Detta ID tilldelas när ditt projekt läggs till i Adobe Commerce i molninfrastrukturen [Snabbt tjänstkonto](fastly.md#fastly-service-account-and-credentials). |
| `type` | Anger platsen för infogning av det genererade fragmentet, t.ex. `init` (ovan) och `recv` (inom underrutiner). Mer information finns i handboken [VCL-kodfragment](https://docs.fastly.com/api/config#api-section-snippet) snabbt. |

## Hantera anpassad VCL från administratör

Du kan [lägga till anpassade VCL-fragment](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md) under *Snabbkonfiguration* > *Anpassade VCL-fragment* i Admin.

![Hantera anpassade VCL-fragment](../../assets/cdn/fastly-edit-snippets.png)

Vyn *Anpassade VCL-kodfragment* visar endast fragment som har lagts till via administratören. Om fragment läggs till med API:t Snabb använder du API:t för att [hantera dem](#manage-vcl-using-the-api).

I följande exempel visas hur du skapar och hanterar anpassade VCL-kodfragment från administratören och använder Fastly Edge-moduler och Edge-ordlistor:

- [Omdirigera begäranden till en CMS-serverdel](fastly-vcl-wordpress.md)
- [Blockera skräppost](fastly-vcl-badreferer.md)
- [Blockera skräppost](fastly-vcl-badreferer.md)
- [Anpassad VCL för IP-tillåtelselista](fastly-vcl-allowlist.md)
- [Anpassad VCL för IP-blockeringslista](fastly-vcl-blocking.md)
- [Kringgå snabbcache](fastly-vcl-bypass-to-origin.md)

## Hantera VCL med API

I följande genomgång visas hur du skapar vanliga VCL-kodfragmentfiler och lägger till dem i din snabbtjänstkonfiguration med API:t Snabb. Du kan skapa och hantera fragment från programmet *terminal*. Du behöver ingen SSH-anslutning till en viss miljö.

**Förutsättningar:**

- Konfigurera din Adobe Commerce i molninfrastrukturmiljö för Snabba tjänster. Se [Konfigurera snabbt](fastly-configuration.md).

- [Få API-autentiseringsuppgifter snabbt](fastly-configuration.md) för att autentisera begäranden till API:t Snabb. Se till att du får autentiseringsuppgifterna för rätt miljö: Förproduktion eller Produktion.

- Spara snabbt autentiseringsuppgifter som grundläggande systemvariabler som du kan använda i cURL-kommandon:

  ```bash
  export FASTLY_SERVICE_ID=<Service-ID>
  ```

  ```bash
  export FASTLY_API_TOKEN=<API-Token>
  ```

  De exporterade miljövariablerna är bara tillgängliga i den aktuella bassessionen och försvinner när du stänger terminalen. Du kan definiera om variabler genom att exportera ett nytt värde. Så här visar du listan med exporterade variabler som är relaterade till Snabbt:

  ```bash
  export | grep FASTLY
  ```

## Lägg till VCL-fragment

I den här självstudiekursen beskrivs de grundläggande stegen för att lägga till anpassade fragment med hjälp av API:t Snabb.

>[!NOTE]
>
>Mer information om hur du hanterar anpassade VCL-fragment från Adobe Commerce Admin finns i [Hantera VCL från Adobe Commerce Admin](#manage-custom-vcl-from-admin).


**Förutsättningar**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

### Steg 1: Hitta den aktiva VCL-versionen

Använd åtgärden [hämta version](https://docs.fastly.com/api/config#version_dfde9093f4eb0aa2497bbfd1d9415987) för snabbprogrammeringsgränssnittet för att hämta det aktiva versionsnumret för VCL:

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
```

Observera det aktiva versionsnumret för VCL i JSON-svaret som returneras i `number`-nyckeln, till exempel `"number": 99`. Versionsnumret behövs när du klonar VCL för redigering.

```json
{
  "testing": false,
  "locked": true,
  "number": 99,
  "active": true,
  "service_id": "872zhjyxhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

Spara det aktiva versionsnumret i en basmiljövariabel för användning i efterföljande API-begäranden:

```bash
export FASTLY_VERSION_ACTIVE=<Version>
```

### Steg 2: Klona den aktiva VCL-versionen och alla kodfragment

Innan du kan lägga till eller ändra anpassade VCL-fragment måste du skapa en kopia av den aktiva VCL-versionen för redigering. Använd åtgärden [clone](https://docs.fastly.com/api/config#version_7f4937d0663a27fbb765820d4c76c709) för snabbprogrammeringsgränssnittet:

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION_ACTIVE/clone -X PUT
```

I JSON-svaret ökas versionsnumret och nyckelvärdet *active* är `false`. Du kan ändra den nya, inaktiva VCL-versionen lokalt.

```json
{
  "testing": false,
  "locked": false,
  "number": 100,
  "active": false,
  "service_id": "vW2bLFWhhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

Spara det nya versionsnumret i en basmiljövariabel och använd det i följande kommandon:

```bash
export FASTLY_EDIT_VERSION=<Version>
```

### Steg 3: Skapa ett anpassat VCL-fragment

Skapa och spara egen VCL-kod i en JSON-fil med följande innehåll och format:

```json
{
  "name": "<name>",
  "dynamic": "0",
  "type": "<type>",
  "priority": "100",
  "content": "<code all in one line>"
}
```

Värdena är:

- `name` - VCL-fragmentets namn.

- `dynamic` - Anger om detta är ett [vanligt fragment](https://docs.fastly.com/en/guides/about-vcl-snippets) eller ett [dynamiskt fragment](https://docs.fastly.com/guides/vcl-snippets/using-dynamic-vcl-snippets).

- `type` - Anger platsen där det genererade fragmentet ska infogas, till exempel `init` (ovanför underrutiner) och `recv` (inom underrutiner). Mer information om de här värdena finns i [Snabbt VCL-fragmentobjektvärden](https://docs.fastly.com/api/config#snippet).

- `priority` - Ett värde från `1` till `100` som avgör när den anpassade VCL-fragmentkoden körs. Anpassade VCL-fragment med lägre värden körs först.

  All VCL-standardkod från snabbVCL-modulen har `priority` av `50`. Om du vill att en åtgärd ska utföras sist eller åsidosätta den förvalda VCL-koden ska du använda ett högre tal, till exempel `100`. Om du vill köra anpassad VCL-kodfragment omedelbart anger du prioriteten till ett lägre värde, till exempel `5`.

- `content` - VCL-kodfragmentet som ska köras på en rad, utan radbrytningar. Se [Exempel på anpassat VCL-fragment](#example-vcl-snippet-code).

### Steg 4: Lägg till VCL-kodfragment för snabb konfiguration

Använd snabbmeny-API:t [skapa fragment](https://docs.fastly.com/api/config#snippet_41e0e11c662d4d56adada215e707f30d) för att lägga till det anpassade VCL-fragmentet i VCL-versionen.

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/snippet -H 'Content-Type: application/json' -X POST --data @<filename.json>
```

`<filename.json>` är namnet på filen som du förberedde i föregående steg. Upprepa det här kommandot för varje VCL-fragment.

Om du får ett `500 Internal Server Error`-svar från tjänsten Snabbt kontrollerar du JSON-filsyntaxen för att vara säker på att du överför en giltig fil.

### Steg 5: Validera och aktivera anpassade VCL-fragment

När du har lagt till ett anpassat VCL-fragment infogar snabbt fragmentet i den VCL-version som du redigerar. Följ de här stegen för att validera VCL-fragmentkoden och aktivera VCL-versionen om du vill tillämpa ändringarna.

1. Använd snabbvaliderings-API:t [validera VCL-version](https://docs.fastly.com/api/config#version_97f8cf7bfd5dc2e5ea1933d94dc5a9a6) för att verifiera den uppdaterade VCL-koden.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/validate
   ```

   Om API:t Fast returnerar ett fel åtgärdar du problemet och validerar den uppdaterade VCL-versionen igen.

1. Använd snabbaktiverings-API:t [activate](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) för att aktivera den nya VCL-versionen.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/activate -X PUT
   ```

## API-snabbreferens för VCL-fragment

I dessa API-frågeexempel används exporterade miljövariabler för att tillhandahålla autentiseringsuppgifter för att autentisera med Fast. Mer information om de här kommandona finns i [Snabb API-referens](https://docs.fastly.com/api/config#vcl).

>[!NOTE]
>
>Använd de här kommandona för att hantera fragment som du har lagt till med API:t Snabbt. Om du har lagt till fragment från administratören läser du [Hantera VCL-fragment med hjälp av administratören](#manage-vcl-using-the-api).

- **Hämta aktivt VCL-versionsnummer**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
  ```

- **Visa alla vanliga VCL-fragment som är kopplade till en tjänst**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet
  ```

- **Granska ett enskilt fragment**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name>
  ```

  `<snippet_name>` är namnet på ett fragment, till exempel `my_regular_snippet`.

- **Uppdatera ett fragment**

  Ändra den [förberedda JSON-filen](#step-3-create-a-custom-vcl-snippet) och skicka följande begäran:

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -H 'Content-Type: application/json' -X PUT --data @<filename.json>
  ```

- **Ta bort ett enskilt VCL-fragment**

  Hämta en lista med fragment och använd följande `curl`-kommando med det specifika fragmentnamnet som ska tas bort:

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -X DELETE
  ```

- **Åsidosätt värden i den [standardmässiga VCL-koden](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)**

  Skapa ett fragment med uppdaterade värden och tilldela prioriteten `100`.

## Fragment som inte kan visas/ändras i Commerce Admin

Du kan inte visa eller ändra vissa fragment direkt i Commerce Admin. Till exempel [dynamiska fragment](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets). I avsnittet Anpassade VCL-fragment visas inte fragment som har lagts till av molnsupportteamet direkt på [kontrollpanelen Snabbt hantering](fastly.md#fastly-service-account-and-credentials).


**Om du vill observera de fragment som lagts till av molnsupportteamet:**

1. Gå till avsnittet **Verktyg**.

1. Klicka på **Visa alla versioner** bredvid _Versionshistorik_.

1. Klicka på ögonikonen bredvid den tillämpliga VCL-versionen för att visa de befintliga fragmenten.
