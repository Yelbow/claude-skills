---
name: project-instruction-writer
description: "Maak nieuwe projectinstructies aan of update bestaande projectinstructies. Gebruik deze skill altijd wanneer een project klaar is om projectinstructies te krijgen, wanneer context files zijn bijgekomen of veranderd en de instructies bijgewerkt moeten worden, of wanneer iemand zegt 'maak projectinstructies', 'update de projectinstructies', of 'voeg deze file toe aan het project'."
---

# Project Instruction Writer

Deze skill maakt projectinstructies aan en houdt ze up-to-date. Projectinstructies zijn kort en functioneel. Ze vertellen de agent wat het project is, welke context files er zijn en wanneer hij welke laadt. Niets meer.

Haal altijd eerst op wat al bekend is uit het gesprek en het geheugen. Stel alleen vragen over wat je zelf niet kunt weten.

---

## Modus 1: Nieuwe projectinstructies aanmaken

**Stap 1: Haal op wat al bekend is**

Haal uit het gesprek en geheugen op:
- Wat is het project?
- Welke context files bestaan er?
- Welke pijlers, contexten of situaties zijn er binnen dit project?
- Op welk signaal in de taakinstructie moet welke file geladen worden?

**Stap 2: Stel alleen wat je echt niet weet**

Vraag minimaal. Als het signaal voor een laadregel niet duidelijk is uit het gesprek, vraag dan alleen dat. Niet meer.

**Stap 3: Schrijf de projectinstructies**

Exact dit formaat. Niets toevoegen, niets uitleggen.

```
## Over [project]
[1-3 zinnen in gewone taal. Wat is het project. Welke pijlers of contexten zijn er, elk in één boldde zin.]

## Context files
Laad [bestandsnaam] als [signaal in taakinstructie].
Laad [bestandsnaam] als [signaal in taakinstructie].

## Self-learning
Als output wordt afgekeurd of bijgestuurd, genereer direct een update-voorstel voor de relevante context file in dit formaat:

Update voor: [bestandsnaam]
Sectie: [sectienaam]
Toevoegen:
[exacte tekst]
```

---

## Modus 2: Bestaande projectinstructies updaten

Gebruik deze modus als er context files zijn bijgekomen, veranderd of verwijderd.

**Stap 1: Haal op wat er veranderd is**

Kijk in het gesprek: welke file is nieuw, welke is hernoemd, welke is weggevallen?

**Stap 2: Pas alleen de relevante laadregel aan**

Genereer een concreet voorstel. Direct kopieerbaar.

Formaat:
```
Update voor: projectinstructies [projectnaam]
Wijziging:
[exacte nieuwe of aangepaste regel]
```

---

## Regels voor laadregels

- Altijd gebaseerd op een signaal dat de gebruiker zelf meegeeft in de taakinstructie
- Nooit gebaseerd op informatie die de agent niet heeft, zoals URL-structuur of bestandsinhoud
- Zo kort mogelijk: één zin per file
- Signalen zijn woorden of zinsdelen die de gebruiker natuurlijk typt, zoals "Modern Men", "blogpost", "klant", "Velvet"

---

## Voorbeeld output

```
## Over modernmenthongs.nl
modernmenthongs.nl verkoopt sexy herenondergoed en lingerie voor mannen. De shop heeft twee pijlers.

**Modern Men:** stijlvol herenondergoed, zelfverzekerd en normaal.
**Velvet:** feminiene lingerie voor mannen, welkomend en specifiek per doelgroep.

## Context files
Laad context_modernmen_webteksten.md als de taak "Modern Men" of "MM" bevat.
Laad context_velvet_webteksten.md als de taak "Velvet" bevat.
Laad context_mmt_blog.md als de taak over een blogpost gaat.
Laad context_mmt_klantenservice.md als de taak over een klant, bestelling, klacht of retour gaat.

## Self-learning
Als output wordt afgekeurd of bijgestuurd, genereer direct een update-voorstel voor de relevante context file in dit formaat:

Update voor: [bestandsnaam]
Sectie: [sectienaam]
Toevoegen:
[exacte tekst]
```
