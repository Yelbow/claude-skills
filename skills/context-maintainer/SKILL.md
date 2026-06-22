---
name: context-maintainer
description: "Beheert de volledige levenscyclus van context files: aanmaken, updaten op feedback, en zelf-auditen op regel-hygiëne, dead weight, voorbeelden-versheid en sectie/changelog-discipline. Gebruik deze skill altijd bij het aanmaken van een nieuwe context file, bij feedback of afgekeurde output op een bestaande file, of bij een expliciete audit-vraag. Trigger ook bij 'dit klinkt niet als mij', 'dit nooit meer', 'check mijn context files', of 'audit [bestand]'. Trigger ook stilzwijgend als lichte staleness-check vlak voordat een andere skill een context file inlaadt om content te schrijven, en passief wanneer een toon-correctie of nieuwe regel naar boven komt die relevant is voor een bestaande file, los van de actieve taak."
---

# Context Maintainer

Deze skill is de enige skill voor context files. Ze vervangt context-file-writer volledig. Aanmaken, updaten op feedback, en zelf-auditen lopen via één bestand, met één levenscyclus, niet via twee losse skills die een file aan elkaar doorgeven.

De files zelf blijven adaptief: secties worden bepaald door de wereld en context, niet door een vast template. Ze worden slimmer naarmate ze gebruikt worden, en deze skill is wat ze slim houdt.

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
5. Toon in drie woorden maximum, plus de reden erachter: voor wie geldt die toon en wanneer.
6. Wat mag absoluut nooit? Woorden, zinnen, stijlkeuzes. Koppel elk verbod aan wat er wél moet gebeuren.
7. Kroegtest: geef een voorbeeld van een zin die zakt.
8. Heb je al teksten die klopten? Zo ja, welke?

Stel ze niet allemaal tegelijk. Groepeer per thema.

**Stap 3: Stel contextspecifieke vragen**

Bepaal op basis van wereld en context welke extra secties nodig zijn. Zie de sectietabel. Stel gerichte aanvulvragen voor wat je nog niet weet.

Vraag ook: zijn er meerdere doelgroepen met elk een eigen toon? Zo ja, breng ze in kaart voordat je schrijft.

Vraag over humor alleen als de wereld er aanleiding toe geeft. AI is slecht in humor: nooit zelf een grap schrijven, alleen een tekst bouwen om een grap die de gebruiker aanlevert.

**Stap 4: Schrijf de file**

Combineer universele secties met contextspecifieke secties. Gebruik "Nog te vullen" voor lege secties.

---

## Modus 2: Bestaande file updaten op feedback

Trigger zodra de gebruiker feedback geeft, iets afkeurt of bijstuurt. Wacht niet tot er om gevraagd wordt.

**Signalen:**
- "Dit klinkt niet als mij"
- "Dit nooit meer"
- "Goed zo" of output zonder aanpassing gebruikt
- Een zin of woord wordt gecorrigeerd
- Toon- of stijlfeedback, hoe kort ook

**Stap 1: Stel gerichte vragen**

Stel alleen wat je niet zelf kunt beantwoorden uit de feedback:
- Wat klopte er precies niet?
- Was het toon, een specifieke zin, of structuur?
- Geldt dit altijd, of alleen in deze situatie?

Als de feedback zelf al duidelijk genoeg is, sla stap 1 over.

**Stap 2: Genereer een concreet update-voorstel**

Schrijf letterlijk de tekst die toegevoegd wordt. Direct kopieerbaar. Elke nieuwe regel krijgt een datum, en elk verbod krijgt een richting (niet alleen "nooit X", ook "in plaats daarvan Y").

Formaat:
```
Update voor: [bestandsnaam]
Sectie: [sectienaam]
Toevoegen:
[2026-06-21] [exacte tekst]
```

---

## Modus 3: Audit

Trigger op een expliciete vraag ("check mijn context files", "audit [bestand]", "review de stijlgidsen"). Werkt op één benoemde file per keer, geen volledige sweep, tenzij expliciet gevraagd.

Loop de checklist langs, rapporteer pass/flag per item, stel per flag een concrete fix voor. Niet zelf wijzigen voordat er akkoord is.

**Checklist:**

- **Regel-hygiëne**: traceert elke regel naar een echte correctie, of staat er een gok of aanname in? Flag wat hypothetisch aanvoelt.
- **Dead weight**: doet recente output dit al goed zonder dat de regel genoemd wordt? Dan mag de regel weg, dat ruimte vrijmaakt voor regels die nog wel iets veranderen.
- **Negatie-koppeling**: heeft elk "nooit" een "in plaats daarvan"? Een kaal verbod laat je vastlopen op precies het moment dat het van toepassing is.
- **Voorbeelden-versheid**: is "Goedgekeurde/Afgewezen voorbeelden" nog een canonieke handvol, of een groeiende stapel? Staan er voorbeelden in die niet meer representatief zijn voor hoe er nu geschreven wordt?
- **Toon-met-reden**: staat de toon in drie woorden er los bij, of is er een zin bij over voor wie en wanneer die toon geldt?
- **Sectietabel-compliance**: staan de juiste secties erin voor dit type context? Ontbreekt er iets dat de sectietabel wel voorschrijft?
- **Changelog-discipline**: maximaal 5 regels, ouder valt eraf, elke regel heeft een datum.
- **Cross-file conflicten**: heeft deze wereld meerdere context files? Spreken hun toonregels elkaar tegen?

---

## Triggers

Drie aparte triggers, met een bewust verschillend gewicht. Een scheduled cron-job op de always-on server is bewust geen vierde trigger, dat is het upgrade-pad als de twee automatische triggers hieronder drift niet op tijd vangen.

### Handmatig

Expliciete vraag van de gebruiker. Voert de volledige Modus 3 checklist uit. Scope: vraag welke file het betreft, tot gebruik een vast patroon oplevert.

### Piggyback-on-usage (licht, automatisch)

Trigger: vlak voordat een andere skill een specifieke context file daadwerkelijk gebruikt om content te schrijven. Niet wanneer de file alleen ter referentie genoemd wordt.

Dit is geen Modus 3. Het is een snelle check, twee dingen, niet de hele checklist:

1. Is de laatste gedateerde changelog-regel oud genoeg om je af te vragen of de toon nog klopt?
2. Staan er nog open "Nog te vullen" secties die relevant zijn voor de taak die nu uitgevoerd wordt?

Stil als er niets gevonden wordt. Eén regel heads-up als er wel iets is, met een keuze, niet de volledige checklist-output:

> Heads up: [bestandsnaam] is sinds [datum] niet bijgewerkt. Eerst checken, of doorgaan met wat er is?

> Heads up: [bestandsnaam] heeft nog open "Nog te vullen" secties die relevant lijken hier. Eerst aanvullen, of doorgaan?

### Passive extraction (automatisch, los van actieve skill)

Trigger: op elk moment in een gesprek, ongeacht welke skill actief is, wanneer een toon-correctie, nieuwe regel, of reactie op output naar boven komt die relevant is voor een bestaande context file.

Werkt als Modus 2: stel een concreet update-voorstel voor, schrijf niet direct weg zonder akkoord. Voeg het toe als losse melding na het eigenlijke antwoord op de vraag van de gebruiker, onderbreek de hoofdtaak niet ervoor.

---

## Sectietabel

### Universele secties (altijd aanwezig)

| Sectie | Inhoud |
|---|---|
| Wie ben je hier | Rol, naam, positie in deze context |
| Voor wie schrijf je | Mentaliteit van de lezer, niet demografie |
| Toon | Max 3 woorden, plus de reden: voor wie en wanneer |
| Verboden formuleringen | Zinnen, woorden, stijlkeuzes die nooit mogen, elk met een "in plaats daarvan" |
| Kroegtest | Voorbeelden van zinnen die zakken en slagen |
| Goedgekeurde voorbeelden | Teksten die klopten, met korte toelichting |
| Afgewezen voorbeelden | Teksten die niet klopten, met reden |
| Changelog | Max 5 laatste wijzigingen, elke regel met datum, oudere vallen eraf |

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

---

## Kroegtest

Een tekst slaagt als hij gezegd kan worden in een kroeg zonder dat iemand denkt: "dat klinkt raar." Hij zakt als hij klinkt als een folder, een AI of een marketingbureau.

---

## Self-learning principe

"Goedgekeurde voorbeelden" en "Afgewezen voorbeelden" zijn het geheugen van de file. Changelog maximaal 5 regels, elke regel gedateerd, oudere wijzigingen vallen eraf. Modus 3 is wat dit principe afdwingt: zonder audit groeit "voorbeelden" naar een stapel en blijft "verboden" een lijst zonder reden.

---

## Naamgeving bestanden

Format: `context_[wereld]_[context].md`

Voorbeelden:
- `context_webshop_productteksten.md`
- `context_outreach_koud.md`
- `context_socials_instagram.md`
- `context_politiek_opiniestuk.md`
- `context_prive_dagelijks.md`
