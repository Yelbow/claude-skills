---
name: gmail-mcp
description: Gebruik deze skill altijd wanneer je een e-mail draft aanmaakt, beantwoordt, of verstuurt via de Gmail MCP tool. Triggers: elke keer dat je Gmail:create_draft aanroept, wanneer je een mail klaarzet voor de gebruiker, wanneer je replyt op een bestaande mail, of wanneer je een nieuw mailbericht opstelt. Gebruik ook wanneer de gebruiker zegt "zet een mail klaar", "stuur een mail", "beantwoord die mail", of iets vergelijkbaars. Bij twijfel: gebruik de skill.
---

# Gmail MCP Skill

## Doel
Elke mail die via Gmail MCP wordt klaargezet volgt vaste regels voor afzenderadres, afsluiting en reply-gedrag.

---

## E-mailadressen en afsluitingen

| Context | Van-adres | Afsluiting |
|---|---|---|
| Modern Men Thongs (webshop) | info@modernmenthongs.nl | Jelle van MMT |
| Studio Zonder Meer (zakelijk) | info@studiozondermeer.nl | Jelle Pieterse \| Studio Zonder Meer |
| Privé | pieterse96@gmail.com | Groeten, Jelle |

Bepaal het juiste adres op basis van de context van het gesprek. Bij twijfel: vraag het de gebruiker.

---

## Gedragsregels

### 1. Altijd vermelden
Na elke draft altijd kort melden:
- Op welk adres de draft klaarstaat
- Of het een reply of nieuwe mail is

Voorbeeld: "Draft staat klaar als reply vanuit info@modernmenthongs.nl."

### 2. Reply boven nieuwe mail
Gebruik altijd `replyToMessageId` en `threadId` als het een reactie op een bestaande mail is. Haal deze IDs op via `Gmail:get_thread` als ze nog niet bekend zijn.

### 3. Afsluiting
Gebruik altijd de juiste afsluiting zoals in de tabel hierboven. Nooit "Met vriendelijke groet" zonder naam, nooit generiek afsluiten.

### 4. From-adres beperking
De Gmail MCP ondersteunt geen `from` parameter. Dit betekent dat het from-adres altijd het gekoppelde Gmail-account is (pieterse96@gmail.com). Vertel de gebruiker na elke draft welk adres hij handmatig moet aanpassen voor verzending, tenzij het toch al het juiste adres is.

### 5. Toon
Volg de toon van de relevante context file als die beschikbaar is. Geen context file? Dan: kort, direct, menselijk. Geen corporate taal. Geen overdreven excuses.

---

## Workflow per draft

1. Bepaal context: welk adres hoort hier bij?
2. Haal `threadId` en `replyToMessageId` op als het een reply is
3. Schrijf de mail met de juiste afsluiting
4. Zet klaar via `Gmail:create_draft` met `replyToMessageId` en `threadId` waar van toepassing
5. Meld aan de gebruiker: adres, type (reply/nieuw), en of hij het from-adres handmatig moet aanpassen

---

## Voorbeeld melding na draft

> "Draft staat klaar als reply vanuit info@modernmenthongs.nl. Let op: Gmail verstuurt dit vanaf je gekoppelde account (pieterse96@gmail.com). Pas het Van-adres handmatig aan voor je verstuurt."

Of als het al het juiste adres is:

> "Draft staat klaar als nieuwe mail vanuit pieterse96@gmail.com."
