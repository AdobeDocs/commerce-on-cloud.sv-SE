---
title: Blockera skräppost
description: Blockera spam från din webbplats med hjälp av Fastly Edge-ordlistan och ett anpassat VCL-kodfragment.
feature: Cloud, Configuration, Security
exl-id: 4ed47a71-7fee-4f37-a7da-3e30052004df
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# Blockera skräppost

I följande exempel visas hur du konfigurerar [Snabb Edge-ordlista](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) med ett anpassat VCL-fragment för att blockera spam från din Adobe Commerce på en molninfrastrukturwebbplats.

>[!NOTE]
>
>Vi rekommenderar att du lägger till anpassade VCL-konfigurationer i en mellanlagringsmiljö där du kan testa dem innan du kör dem mot produktionsmiljön.

**Förutsättningar:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Granska webbplatsloggarna för falska hänvisnings-URL:er och gör en lista över domäner som ska blockeras.

## Skapa en hänvisare blockeringslista

Edge-ordlistor skapar nyckelvärdepar som är tillgängliga för VCL-funktioner under VCL-fragmentbearbetning. I det här exemplet skapar du en kantordlista med en lista över referenswebbplatser som ska blockeras.

{{admin-login-step}}

1. Klicka på **Lagrar** > **Inställningar** > **Konfiguration** > **Avancerat** > **System**.

1. Expandera **Helsidescache** > **Snabb konfiguration** > **Edge-ordlistor**.

1. Skapa ordlistebehållaren:

   - Klicka på **Lägg till behållare**.

   - På sidan *Behållare* anger du ett **lexikonnamn**—`referrer_blocklist`.

   - Välj **Aktivera efter ändringen** om du vill distribuera ändringarna till den version av snabbtjänstkonfigurationen som du redigerar.

   - Klicka på **Överför** för att koppla ordlistan till din snabbtjänstkonfiguration.

1. Lägg till listan med domännamn som ska blockeras i `referrer_blocklist`-ordlistan:

   - Klicka på inställningsikonen för `referrer_blocklist`-ordlistan.

   - Lägg till och spara nyckelvärdepar i den nya ordlistan. I det här exemplet är varje **nyckel** domännamnet för en hänvisnings-URL som ska blockeras och **Värde** är `true`.

     ![Lägg till felaktiga referensordlisteobjekt](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - Klicka på **Avbryt** för att återgå till systemkonfigurationssidan.

1. Klicka på **Spara konfiguration**.

1. Uppdatera cacheminnet enligt meddelandet längst upp på sidan.

Mer information om Edge-ordlistor finns i [Skapa och använda Edge-ordlistor](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) och [anpassade VCL-kodfragment](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples) i Snabbt-dokumentationen.

## Skapa ett anpassat VCL-fragment för att blockera spam från referensen

I följande anpassade VCL-kodfragment (JSON-format) visas logiken för att kontrollera och blockera begäranden. VCL-fragmentet samlar in värddatorn för en referenswebbplats i ett sidhuvud och jämför sedan värdnamnet med listan med URL:er i `referrer_blocklist`-ordlistan. Om värdnamnet matchar blockeras begäran med ett `403 Forbidden`-fel.

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if (req.http.Referer ~ \"^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$\") {set req.http.Referer-Host = re.group.2;}if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {error 403 \"Forbidden\";}"
}
```

Innan du skapar ett fragment baserat på det här exemplet ska du granska värdena för att avgöra om du behöver göra några ändringar:

- `name` - VCL-fragmentets namn. I det här exemplet använde vi `block_bad_referrer`.

- `dynamic` - Värdet 0 anger att ett [vanligt fragment](https://docs.fastly.com/en/guides/using-regular-vcl-snippets) ska överföras till den versionshanterade VCL-listan för snabbkonfigurationen.

- `priority` - Avgör när VCL-fragmentet körs. Prioriteten är `5` för att köra kodfragmentkoden innan något av Magento VCL-standardfragmenten (`magentomodule_*`) har tilldelats en prioritet på 50. Ange prioriteten för varje anpassat fragment som är högre eller lägre än 50, beroende på när du vill att fragmentet ska köras. Fragment med lägre prioritetsnummer körs först.

- `type` - Anger en plats där fragmentet ska infogas i VCL-versionen. I det här exemplet är VCL-fragmentet ett `recv`-fragment. När fragmentet infogas i VCL-versionen läggs det till i underrutinen `vcl_recv`, nedanför den förvalda VCL-koden Fast och ovanför eventuella objekt.

- `content` - VCL-kodfragmentet som ska köras på en rad, utan radbrytningar.

När du har granskat och uppdaterat koden för din miljö använder du någon av följande metoder för att lägga till det anpassade VCL-fragmentet i din snabbtjänstkonfiguration:

- [Lägg till det anpassade VCL-fragmentet från administratören](#add-the-custom-vcl-snippet). Den här metoden rekommenderas om du har åtkomst till Admin. (Kräver [snabbversion 1.2.58](fastly-configuration.md#upgrade) eller senare.)

- Spara JSON-kodexemplet till en fil (till exempel `allowlist.json`) och [överför det med snabbprogrammeringsgränssnittet](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Använd den här metoden om du inte kan komma åt administratören.

## Lägg till anpassat VCL-fragment

{{admin-login-step}}

1. Klicka på **Lagrar** > Inställningar > **Konfiguration** > **Avancerat** > **System**.

1. Expandera **Helsidescache** > **Snabb konfiguration** > **Anpassade VCL-kodfragment**.

1. Klicka på **Skapa anpassat fragment**.

1. Lägg till VCL-fragmentvärden:

   - **Namn** — `block_bad_referrer`

   - **Typ** — `recv`

   - **Prioritet** — `5`

   - **VCL**-fragmentinnehåll —

     ```conf
     if (req.http.Referer ~ "^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$") {
       set req.http.Referer-Host = re.group.2;  
     }
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. Klicka på **Skapa**.

   ![Skapa VCL-kodfragment för anpassat referensblock](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. När sidan har lästs in på nytt klickar du på **Överför VCL till Snabbt** i avsnittet *Snabbkonfiguration*.

1. När överföringen är klar uppdaterar du cacheminnet enligt meddelandet längst upp på sidan.

Validerar snabbt den uppdaterade VCL-versionen under överföringsprocessen. Om valideringen misslyckas kan du åtgärda eventuella problem genom att redigera det anpassade VCL-fragmentet. Ladda sedan upp VCL-filen igen.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
