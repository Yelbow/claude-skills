---
name: memory-review
description: "Review en schoon de opgeslagen memory edits op. Gebruik deze skill altijd wanneer de gebruiker vraagt om memory te reviewen, op te schonen, een audit te doen op memory edits, of wil checken wat er allemaal in memory staat met de bedoeling iets te verwijderen, samen te voegen, of te verplaatsen. Trigger ook bij 'laten we de memories doornemen', 'memory audit', of 'split de memory edits uit'."
---

# Memory Review

Splits elke opgeslagen memory edit naar losse, kortst mogelijke feiten. Presenteer ze doorlopend genummerd, laat de gebruiker per nummer keep, delete, of move aangeven, en schrijf de wijzigingen terug naar de originele regels.

---

## Stap 1: View en splits

Roep `memory_user_edits` aan met `view`. Splits elke edit in de kortste op zichzelf staande feiten. Eén feit per zin, geen samengevoegde zinnen met "en" of komma's die twee aparte feiten verbergen.

Nummer de feiten doorlopend over alle edits heen, niet per edit opnieuw beginnend.

Houd een interne mapping bij van elk genummerd feit naar de originele edit-regel waar het uit komt. Die mapping heb je nodig in stap 6.

---

## Stap 2: Duplicaten markeren

Scan de volledige feitenlijst op inhoudelijke overlap, ook als de bewoording verschilt. Markeer duplicaten expliciet in de output, bijvoorbeeld met "*(duplicaat van X)*".

Duplicaten hoeven niet apart bevestigd te worden: die mogen er sowieso uit, ook zonder expliciete instructie.

---

## Stap 3: Presenteren en vragen

Toon de volledige genummerde lijst. Geen extra opmaak zoals headers per onderwerp, gewoon een platte genummerde lijst. Sluit af met:

> Geef per nummer aan: keep, delete, of move (en waarnaartoe).

Wacht op antwoord. Verwerk alleen wat expliciet benoemd wordt. Niet-benoemde nummers blijven ongewijzigd, ook al lijkt er een logische vervolgstap te zijn.

---

## Stap 4: Vage of onduidelijke feiten

Als een feit geen duidelijke betekenis heeft, geen context biedt, of niet te herleiden is naar een bruikbare instructie: niet gokken naar een interpretatie.

Zeg eerlijk dat de context ontbreekt en vraag of de gebruiker het zelf weet. Als de gebruiker het ook niet weet: stel voor om het feit te verwijderen.

---

## Stap 5: Ambigue interpretatie voorleggen

Bij vaag geformuleerde of voice-gedicteerde instructies waar meerdere interpretaties mogelijk zijn: leg je interpretatie expliciet voor voordat je hem doorvoert.

Formaat:

```
Ik heb [origineel fragment] geïnterpreteerd als: [interpretatie].
Klopt dat, of bedoelde je iets anders?
```

Als de gebruiker corrigeert: gebruik de correctie, niet de oorspronkelijke aanname.

---

## Stap 6: Terugschrijven naar de originele edit

Voor elke wijziging: gebruik de mapping uit stap 1 om te bepalen welke originele edit-regel het betreft.

- Pas alleen het aangewezen feit aan binnen die regel. De rest van de regel blijft intact.
- Als meerdere feiten uit dezelfde regel wijzigen: verwerk ze in één `replace` call met de volledige nieuwe tekst van die regel.
- Als alle feiten in een regel verwijderd worden: verwijder de hele regel met `remove`.
- Bij `remove`: voer removals uit van hoog naar laag regelnummer, zodat eerdere removals de nummering van latere removals niet verstoren.
- Gebruik nooit `add` voor feiten die eigenlijk een correctie zijn van een bestaand feit. Dat is een `replace`.

---

## Stap 7: Bevestigen

Na het verwerken van een batch: bevestig kort en concreet wat er veranderd is per regel, in gewone taal, geen technische tool-output herhalen.

Als er nog onbehandelde nummers open staan: meld dat expliciet en vraag of de gebruiker daar nu of later op terugkomt.

---

## Wat deze skill nooit doet

- Zelf beslissen dat iets weg mag zonder expliciete instructie, behalve duidelijke duplicaten.
- Een vage instructie invullen met een aanname zonder die voor te leggen.
- Wijzigingen toepassen op nummers die de gebruiker niet genoemd heeft.
- Memory edits direct overschrijven zonder eerst de lijst en vraag uit stap 3 te tonen.

---

## Token discipline

- Stap 1 en 2 in één pass, geen losse view-call per edit.
- Geen losse vraag per nummer: verzamel alle vragen uit stap 4 en 5 en stel ze in één bericht.
- Verwerk een bevestigde batch in zo weinig `replace`/`remove` calls als mogelijk, één per geraakte regel.
