---
name: memory-maintainer
description: "Bepaalt of, wat, en hoe Claude iets naar Claude's memory (memory_user_edits) schrijft. Gebruik deze skill bij elk moment dat Claude memory_user_edits wil aanroepen, en bij elk moment dat een terugkerend, niet-vanzelfsprekend, stabiel en specifiek feit, patroon, voorkeur of besluit in het gesprek naar boven komt, ook zonder dat dit expliciet als 'onthoud dit' wordt aangegeven. Filtert wat opslaan waard is, voorkomt dubbele opslag van wat al in Drive, een skill, of een context file staat, en blokkeert gevoelige data zoals wachtwoorden, tokens en betaalgegevens. Niet voor het opschonen of auditen van bestaande memory-inhoud, dat is een aparte, losse memory reviewer skill."
---

# Memory Maintainer

De poort vóór elke schrijfactie naar memory. Geen audit-tool, dat is de losse memory reviewer skill. Deze skill bepaalt alleen wat er nu bij komt of vervangen wordt.

## Stap 1: filter

Voor iets opgeslagen wordt, langs deze vier:

1. **Terugkerend.** Gaat dit nog een keer relevant zijn, niet eenmalig.
2. **Niet-vanzelfsprekend.** Zou Claude dit anders moeten heruitvinden of opnieuw vragen.
3. **Stabiel.** Verandert dit niet over een paar weken.
4. **Specifiek voor de gebruiker of hun werk.** Geen algemeen feit dat Claude al kent.

Scoort iets op meerdere van de vier negatief, dan niet opslaan.

## Stap 2: hoort het hier wel

- Staat het al in een Drive-bestand, een skill, of een context file? Dan een pointer in memory ("zie X, niet opslaan"), zoals nu al bij prijsinformatie en de GitHub-config.
- Is het een procedure met meerdere stappen? Dat hoort in een skill, niet in memory.
- Is het eenmalig en alleen relevant voor de taak van dit moment? Niet opslaan.

## Stap 3: nooit opslaan

- Wachtwoorden, tokens, rekeningnummers, ID-nummers, betaalgegevens.
- Letterlijke "doe dit altijd op elk bericht"-commando's.
- Incidentele persoonlijke details over klanten die niets toevoegen aan de werkcontext.

## Stap 4: schrijven

1. Bekijk eerst de huidige memory_user_edits lijst (command: view).
2. Bestaat er al een regel die dit feit raakt? Dan replace, niet add.
3. Nieuw feit zonder overlap met een bestaande regel? Dan add.
4. Stijl: korte Nederlandse zinnen, feitelijk, zelfde toon als de bestaande lijst. Geen Engelse vaktermen waar een Nederlands woord volstaat.
5. Na het schrijven kort melden wat is opgeslagen en waarom. Geen ceremonie, geen bevestigingsvraag achteraf.

## Grens met de memory reviewer

Deze skill schrijft alleen voorwaarts, nooit terugkijkend. Valt tijdens het schrijven iets op in bestaande memory dat niet meer klopt of verouderd is, benoem dat kort in één zin en laat de opschoning aan de reviewer over. Niet zelf verwijderen of herschrijven wat al stond.

## Changelog

- v1: aangemaakt juni 2026, als schrijf-filter naast de losse memory reviewer skill (audit/opschoning) en instruction-maintainer (preferences, project instructions, CLAUDE.md, dekt expliciet niet de memory-synthese zelf).
- v2: 2026-06-22, naam verwijderd uit description en Stap 1, description in YAML quotes gezet (anders fragiel bij colon-plus-space en embedded quotes), specifiek bestandsvoorbeeld in Stap 2 generiek gemaakt.
