---
source-git-commit: f75dbbc9726ff21a648b1bb3d8f06cb45a5d6b08
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---
# Gör om biblioteksfiler

Den här katalogen innehåller definitioner för streckaktiviteter ordnade efter funktion. Omtagning läser automatiskt in alla `.rake` filer i den här katalogen.

## Filorganisation

### `adobe-docs-tasks.rake`

Innehåller vanliga krav, delade funktioner och icke namngivna uppgifter för Adobe Commerce i Experience League dokumentationsarkivuppgifter:

- `whatsnew` - Generera data för nyhetssammanfattning (standard: sedan senaste uppdateringen)
- `render` - Återge mallade filer och underhåll inkluderar

### `includes.rake`

Innehåller hanteringsaktiviteter som är organiserade i namnområdet `:includes`:

- `includes:maintain_relationships` - Identifiera och underhåll inkluderade relationer i markeringsfiler
- `includes:maintain_timestamps` - Lägg till/uppdatera tidsstämplar baserat på inkluderingsfiländringar
- `includes:maintain_all` - Kör båda åtgärderna i sekvens
- `includes:unused` - Sök efter oanvända inkluderingsfiler

### `images.rake`

Innehåller bildhanteringsaktiviteter organiserade i namnområdet `:images`:

- `images:optimize` - Optimera bilder i ändrade ej implementerade filer
- `images:unused` - Sök efter oanvända bilder i projektet

## Så här fungerar det

Rake identifierar och läser automatiskt in alla `.rake`-filer i katalogen `rakelib/`. Detta gör att vi kan:

1. **Ordna uppgifter efter funktion** - Gruppera relaterade uppgifter
2. **Håll den huvudsakliga återskaparfilen ren** - Fokusera på viktiga projektuppgifter
3. **Lägg enkelt till nya uppgiftsgrupper** - Skapa bara nya `.rake` filer
4. **Bevara problemskiljning** - Varje fil hanterar en specifik domän

## Lägga till nya uppgiftsgrupper

Så här lägger du till en ny grupp med relaterade uppgifter:

1. Skapa en ny `.rake`-fil i den här katalogen
2. Använd ett beskrivande namnutrymme (t.ex. `namespace :images` för bildrelaterade uppgifter)
3. Definiera dina uppgifter i namnutrymmet
4. Rake läser in dem automatiskt

## Exempelstruktur

```ruby
namespace :your_namespace do
  desc 'Description of what this task does'
  task :task_name do
    # Task implementation
  end
end
```

## Användning

Aktiviteter är tillgängliga direkt efter att de har skapats:

```bash
rake your_namespace:task_name
```

## Fördelar

- **Modularitet**: Varje fil fokuserar på ett visst område
- **Underhållbarhet**: Enklare att hitta och ändra specifika uppgiftsgrupper
- **Skalbarhet**: Det går att lägga till många uppgiftsgrupper utan att huvudåterskaparfilen behöver renderas
- **Teamutveckling**: Olika utvecklare kan arbeta med olika uppgiftsgrupper
