---
name: feedback-to-rules
description: "Verwerk een MARKS blok of vrije tekstfeedback op geschreven output naar abstracte, universeel toepasbare schrijfregels. Gebruik deze skill altijd wanneer een MARKS blok in de chat verschijnt en de gebruiker regels wil in plaats van (of naast) een herschrijving. Trigger ook bij zinnen als 'maak hier regels van', 'verwerk dit als regel', 'voeg dit toe aan mijn schrijfregels', of wanneer er feedbackpatronen zijn die breder toepasbaar zijn dan de specifieke tekst."
---

# Feedback-to-Rules

Verwerk feedback op een specifiek stuk naar abstracte, universeel toepasbare schrijfregels. De regels mogen nooit gebonden zijn aan het onderwerp van de tekst. Ze moeten werken voor elk stuk binnen dezelfde wereld en hetzelfde format.

---

## Kernprincipe

**Positieve regels boven verboden.**

Een goede regel sluit slechte patronen structureel uit zonder ze te benoemen. Alleen als een woord of begrip echt niet af te leiden is uit een positieve regel, voeg je een verbod toe, en dan alleen als laatste redmiddel.

**De kwaliteit van een regel hangt af van of de oorzaak correct is vastgesteld.** Dezelfde markering kan tien verschillende oorzaken hebben. Verifieer daarom altijd eerst de oorzaak voordat je een regel schrijft.

---

## Stap 1: Identificeer wereld en format

Haal uit de feedback of het gesprek:
- **Wereld**: Studio, DJ, politiek, webshop, privé, of een andere context
- **Format**: LinkedIn post, productbeschrijving, cold mail, caption, blogartikel, etc.

Als dit niet uit de context af te leiden is: stel één gerichte vraag.

Noteer ook welke context file gebruikt werd voor het schrijven van de tekst. In een project is er per sessie doorgaans één actieve context file. Die heb je nodig in stap 5.

---

## Stap 2: Lees de marks label-agnostisch

Lees het label van elke mark niet als een vaste categorie maar als een signaal. Combineer de labelnaam met de gemarkeerde tekst om de intent te begrijpen.

### Signaalsterkte per intent

| Intent | Voorbeeldlabels | Wat het levert |
|---|---|---|
| Stem / identiteit | NIET MIJN STEM, WRONG VIBE, STEM, NO | Sterkste signaal: leidt naar toonregel of stemregel |
| Herformulering | REWORK, ANDERS, FIX | Stijlpatroon: analyseer wat structureel niet klopt |
| Helderheid | VAAG, ONDUIDELIJK, WAT | Helderheidsregel: wat moet concreter en hoe |
| Letterlijke fix | REPLACE, SWAP | Laagste regelwaarde: alleen een regel als er een patroon in zit |

Onbekend label: leid de intent af uit de gemarkeerde tekst zelf. Als dat niet lukt, vraag eenmalig wat het label betekent. Noteer de uitleg als label-entry in stap 6.

---

## Stap 3: Verifieer de oorzaak, schrijf nog geen regels

Presenteer per markgroep je interpretatie van het probleem. Nog geen regels. Eerst bevestiging.

Formaat:

```
Mijn interpretatie van de marks:

[label of groepsnaam]: "[gemarkeerde tekst, kort ingekort]"
Ik lees dit als: [oorzaak in één zin, geen aanname over het onderwerp]

[volgende groep]:
Ik lees dit als: [oorzaak]

Klopt dit, of zie ik iets verkeerd?
```

Wacht op reactie. Pas als de interpretatie bevestigd is, ga je naar stap 4.

Als de gebruiker corrigeert: pas de oorzaak aan, bevestig kort, en ga daarna pas verder. Sla de correctie op als leerpunt in stap 6.

**Wat een goede interpretatie is:**
- Een oorzaak, niet een herhaling van het symptoom
- Fout: "dit woord vond je niet goed"
- Goed: "dit klinkt als een claim die de schrijver toeschrijft aan het product, niet als iets dat hij zelf constateert of ervaart"
- Meerdere marks met dezelfde oorzaak: groepeer ze en formuleer één gedeelde oorzaak

---

## Stap 4: Schrijf de regels

Na verificatie: analyseer per groep wat de bevestigde oorzaak zegt over een structureel patroon.

**Over stem en identiteit:**
- Wat zegt de afwijzing over de gewenste stem?
- Welk register, vocabulaire of constructies passen niet?
- Wat is de positieve tegenhanger: hoe schrijft deze persoon wel?

**Over stijl en herformulering:**
- Wat hebben de gemarkeerde secties structureel gemeen?
- Is het zinslengte, abstractieniveau, passiviteit, formele constructies, register?
- Wat is de patroonregel die dit aanpakt voor elk stuk, niet alleen dit ene?

**Over helderheid:**
- Wat ontbreekt er concreet? Voorbeeld, getal, handeling, specificiteit?
- Wat is de structurele reden dat het vaag is, niet het symptoom?

**Over letterlijke fixes:**
- Zit er een patroon over meerdere fixes? Dan is er een regel.
- Eén losse fix zonder patroon: geen regel, wel vermelden als voorbeeld.

### Kwaliteitscriteria voor elke regel

Een regel is goed als hij:
1. **Generiek** is: toepasbaar op elk stuk in dezelfde wereld en format, niet op dit onderwerp
2. **Scherp** is: concreet genoeg om output te sturen
3. **Positief geformuleerd** is: zegt wat je doet, niet wat je vermijdt
4. **Autonoom** is: sluit het slechte patroon structureel uit zonder het te noemen

### Output formaat

```
Regels voor: [wereld] / [format]

STEM
- [wat de schrijver wel doet: toon, register, perspectief]

STIJL
- [zinslengte, abstractieniveau, constructies]

STRUCTUUR
- [opbouw, volgorde, alinea-logica]

VERBODEN (alleen als positieve regel niet volstaat)
- [specifiek woord of begrip dat buiten alle positieve regels valt]
```

Laat secties weg als er geen marks voor waren. Forceer geen categorieën.

---

## Stap 5: Conflictcheck tegen actieve context file

Controleer de nieuwe regels tegen de context file die gebruikt werd voor het schrijven van de tekst. Die staat in het project.

**Hoe je conflicten herkent:**
- Een nieuwe regel zegt iets anders dan een bestaande regel over hetzelfde aspect
- Een nieuwe regel maakt een bestaande regel overbodig of ongeldig
- Een nieuwe regel en een bestaande regel geven tegenstrijdige instructies voor hetzelfde format

**Als er geen conflict is:** vermeld dit kort. Ga door naar stap 6.

**Als er een conflict is:** presenteer elk conflict afzonderlijk in dit formaat en vraag wat voorrang heeft:

```
Conflict gevonden:

Nieuwe regel: [nieuwe regel]
Huidige regel in context file: [bestaande regel]

Wat heeft voorrang?
A. Nieuwe regel: context file aanpassen
B. Huidige regel: nieuwe feedback is een eenmalige uitzondering
```

Wacht op antwoord per conflict. Verwerk daarna:
- Bij A: markeer de regel als "context file bijwerken" en stel voor de context-file-writer skill te triggeren
- Bij B: markeer de regel als uitzondering, niet opnemen als permanente regel

Pas na alle conflicten opgelost zijn: ga naar stap 6.

---

## Stap 6: Label-register en leernotities

**Onbekend label uitgelegd gekregen:**

```
Label-register update:
[LABELNAME] = [intent in één zin]
Suggestie: voeg dit toe aan de skill.
```

**Interpretatie gecorrigeerd:**

```
Leerpunt:
Markering van type [label] op [soort tekst] betekent niet [foute aanname].
Het betekent: [gecorrigeerde oorzaak].
Suggestie: voeg dit toe aan de skill als aanscherping van stap 3.
```

---

## Stap 7: Bestemming voorstellen

Na de conflictcheck, kort en direct:

> Wil je de definitieve regels in de context file van [wereld], tijdelijk in de projectinstructies, of los bewaren?

Als de gebruiker zegt "maak er een file van" of als stap 5 een context file update vereist: trigger de context-file-writer skill met zowel de nieuwe regels als de te vervangen bestaande regels als input.

---

## Wat de regels nooit mogen bevatten

- Een verwijzing naar het onderwerp van de tekst
- Een zin die alleen werkt voor dit stuk en niet voor het volgende
- Een verbod als er een positieve tegenhanger mogelijk is
- Vage aansporingen zonder handvat

---

## Token discipline

- Verwerk alle marks in één pass. Geen iteratie per mark.
- Stap 3 is verplicht. Geen regels zonder bevestigde oorzaak.
- Stap 5 is verplicht. Geen regels opslaan zonder conflictcheck.
- Regels zijn de output. Geen herschrijving tenzij apart gevraagd.
- Maximaal acht regels totaal. Liever vier scherpe dan acht vage.
