---
name: context-file-writer
description: "Maak nieuwe context files aan voor elke wereld en context, of update bestaande context files op basis van feedback. Gebruik deze skill altijd wanneer een nieuwe context file aangemaakt moet worden voor een wereld (Studio, webshop, DJ, politiek, privé of een andere wereld), een bestaande context file geüpdatet moet worden na feedback of afgekeurde output, of wanneer iemand zegt dat een tekst niet klopt, niet de goede toon heeft, of herschreven moet worden. Trigger ook bij zinnen als 'dit klinkt niet als mij', 'update de context file', 'voeg dit toe aan de file', of 'dit nooit meer'."
---

# Context File Writer

Deze skill maakt context files aan en houdt ze up-to-date. De files zijn adaptief: secties worden bepaald door de wereld en context, niet door een vast template. De files worden slimmer naarmate ze gebruikt worden.

Haal altijd eerst op wat al bekend is uit het gesprek en het geheugen. Stel alleen vragen over wat je zelf niet kunt weten.

---

## Modus 1: Nieuwe context file aanmaken

**Stap 1: Haal op wat al bekend is**

Kijk in het gesprek en het geheugen. Beantwoorde vragen sla je over.

**Stap 2: Stel alleen de vragen waarop het antwoord nog niet bekend is**

1. Wat is de wereld?
2. Wat is de specifieke context binnen die wereld?
3. Wie ben je in deze context? Rol, naam, positie.
4. Voor wie schrijf je? Beschrijf de mentaliteit, niet de demografie.
5. Toon in drie woorden maximum.
6. Wat mag absoluut nooit? Woorden, zinnen, stijlkeuzes.
7. Kroegtest: geef een voorbeeld van een zin die zakt.
8. Heb je al teksten die klopten? Zo ja, welke?

Stel ze niet allemaal tegelijk. Groepeer per thema.

**Stap 3: Stel contextspecifieke vragen**

Bepaal op basis van wereld en context welke extra secties nodig zijn. Zie de sectietabel. Stel gerichte aanvulvragen voor wat je nog niet weet.

Vraag ook: zijn er meerdere doelgroepen met elk een eigen toon? Zo ja, breng ze in kaart voordat je schrijft.

Vraag over humor alleen als de wereld er aanleiding toe geeft.

**Stap 4: Schrijf de file**

Combineer universele secties met contextspecifieke secties. Gebruik "Nog te vullen" voor lege secties.

---

## Modus 2: Bestaande file updaten

Trigger zodra de gebruiker feedback geeft, iets afkeurt of bijstuurt. Wacht niet tot er om gevraagd wordt.

**Signalen:**
- "Dit klinkt niet als mij"
- "Dit nooit meer"
- "Goed zo" of output zonder aanpassing gebruikt
- Een zin of woord wordt gecorrigeerd
- Toon of stijlfeedback, hoe kort ook

**Stap 1: Stel gerichte vragen**

Stel alleen wat je niet zelf kunt beantwoorden uit de feedback:
- Wat klopte er precies niet?
- Was het toon, een specifieke zin, of structuur?
- Geldt dit altijd, of alleen in deze situatie?

Als de feedback zelf al duidelijk genoeg is, sla stap 1 over.

**Stap 2: Genereer een concreet update-voorstel**

Schrijf letterlijk de tekst die toegevoegd wordt. Direct kopieerbaar.

Formaat:
```
Update voor: [bestandsnaam]
Sectie: [sectienaam]
Toevoegen:
[exacte tekst]
```

---

## Sectietabel

### Universele secties (altijd aanwezig)

| Sectie | Inhoud |
|---|---|
| Wie ben je hier | Rol, naam, positie in deze context |
| Voor wie schrijf je | Mentaliteit van de lezer, niet demografie |
| Toon | Max 3 woorden |
| Verboden formuleringen | Zinnen, woorden, stijlkeuzes die nooit mogen |
| Kroegtest | Voorbeelden van zinnen die zakken en slagen |
| Goedgekeurde voorbeelden | Teksten die klopten, met korte toelichting |
| Afgewezen voorbeelden | Teksten die niet klopten, met reden |
| Changelog | Max 5 laatste wijzigingen, oudere vallen eraf |

### Contextspecifieke secties

| Context | Extra secties |
|---|---|
| Productteksten, blogposts, paginateksten | Structuur en lengte, SEO-regels |
| Social media (Instagram, LinkedIn) | Platform, postformaat, CTA-stijl |
| Outreach, koude acquisitie | Relatietype (koud/warm), openingszin-regels, doel van het bericht |
| Offertes, voorstellen | Toonverschil formeel vs. informeel, wat altijd in een offerte staat |
| Klantenservice, reacties | Escalatieregels, wat je nooit zegt bij klachten |
| Politieke teksten, opinies | Politiek kader, wat je nooit verdedigt, hoe je bronnen gebruikt |
| Muziekpromotie, booking | Genrecontext, wat je over jezelf zegt, wat je nooit zegt |
| Privécommunicatie | Relatietype, context van het gesprek |
| Meerdere doelgroepen binnen één context | Subsecties per doelgroep met eigen toon en verboden overlap |

Voeg alleen secties toe die daadwerkelijk relevant zijn.

### Humor

Alleen toevoegen als de wereld er aanleiding toe geeft:
- AI is slecht in humor. Nooit humor schrijven tenzij de gebruiker de grap aanlevert.
- Gebruiker levert het grappige element, jij bouwt er een tekst omheen.

---

## Kroegtest

Een tekst slaagt als hij gezegd kan worden in een kroeg zonder dat iemand denkt: "dat klinkt raar." Hij zakt als hij klinkt als een folder, een AI of een marketingbureau.

---

## Self-learning principe

"Goedgekeurde voorbeelden" en "Afgewezen voorbeelden" zijn het geheugen van de file. Changelog maximaal 5 regels. Oudere wijzigingen vallen eraf.

---

## Naamgeving bestanden

Format: `context_[wereld]_[context].md`

Voorbeelden:
- `context_modernmen_webteksten.md`
- `context_studio_outreach_koud.md`
- `context_inisyel_instagram.md`
- `context_politiek_opiniestuk.md`
- `context_prive_dagelijkse_communicatie.md`
