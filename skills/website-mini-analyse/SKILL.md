---
name: website-mini-analyse
description: Voer een gratis website-mini-analyse uit als verkooptool en genereer een professioneel MD-rapport. Gebruik deze skill altijd wanneer iemand vraagt om een website te analyseren, een gratis SEO-check te doen, een mini-analyse te maken, of wanneer een URL wordt gedeeld met de vraag om feedback op vindbaarheid, snelheid of SEO. Trigger ook bij zinnen als "kun je even naar deze site kijken", "wat vind je van deze website qua SEO", of "maak een rapportje van deze site". De skill verzamelt input, voert checks uit via beschikbare tools, vertaalt bevindingen naar omzetimpact en genereert een downloadbaar MD-rapport.
---

# Website Mini-Analyse Skill

Een gratis website-analyse die dient als verkooptool. Het doel is niet om alles op te lossen, maar om pijn zichtbaar te maken en een betaalde vervolganalyse te verkopen.

## Stap 1: Input verzamelen

Vraag de gebruiker het volgende als het nog niet bekend is:

1. **URL** van de website
2. **Wat doet deze persoon/dit bedrijf?** (product, dienst, doelgroep)
3. **Hoofd-keyword(s)**: waar zou de site voor moeten ranken?

Als de URL al is gedeeld, sla dan stap 1 over en vraag alleen wat ontbreekt.

---

## Stap 2: Checks uitvoeren

Voer de volgende checks uit via web_fetch en web_search. Documenteer ruwe bevindingen intern voor gebruik in het rapport.

### Check 1: PageSpeed (mobiel)
- Haal de PageSpeed Insights API-URL op:
  `https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url={URL}&strategy=mobile`
- Let op: performance score, LCP, CLS, FID/INP
- Noteer de score en de zwaarste knelpunten

### Check 2: Indexatie
- Zoek via web_search: `site:{domein}`
- Hoeveel pagina's zijn geïndexeerd?
- Zijn de logische pagina's (homepage, diensten, contact) aanwezig?
- Controleer ook: `{URL}/sitemap.xml` en `{URL}/robots.txt` via web_fetch

### Check 3: Keyword ranking
- Zoek via web_search op het hoofd-keyword
- Staat de site op pagina 1? Zo nee, hoe ver weg?
- Zoek ook op een long-tail variant van het keyword

### Check 4: On-page basics homepage
- Haal de homepage op via web_fetch
- Controleer:
  - Title tag: aanwezig, bevat keyword, niet te lang (max 60 tekens)?
  - Meta description: aanwezig en wervend?
  - H1: aanwezig, duidelijk, bevat keyword?
  - Afbeeldingen: zijn er alt-teksten?
  - Content: is er substantiële tekst op de homepage?

---

## Stap 3: Bevindingen framen

Voor elke bevinding gebruik je dit model:

> **Wat zien we:** [technische observatie, kort en concreet]
> **Wat betekent dit:** [uitleg in gewone taal]
> **Wat kost dit je:** [omzetimpact, zo concreet mogelijk]

### Omzetvertalingen om te gebruiken

| Bevinding | Omzetvertaling |
|---|---|
| PageSpeed mobiel onder 50 | Elke seconde extra laadtijd kost gemiddeld 7% conversie. Veel bezoekers haken af voor ze iets gezien hebben. |
| Niet op pagina 1 voor hoofd-keyword | Pagina 2 van Google krijgt minder dan 1% van de klikken. Wie je niet kan vinden, kan je ook niet inhuren. |
| Weinig pagina's geïndexeerd | Google ziet een deel van uw site niet. Die pagina's bestaan niet voor potentiële klanten. |
| Geen of zwakke title tag | De title tag is het eerste wat Google en zoekers zien. Een vage of ontbrekende title verlaagt zowel je ranking als je klikratio. |
| Dunne homepage content | Google beloont pagina's die bezoekers goed informeren. Een dunne homepage geeft weinig reden om te ranken. |
| Sitemap ontbreekt of is leeg | Zonder sitemap laat je Google zelf raden welke pagina's belangrijk zijn. Dat werkt zelden in je voordeel. |

Gebruik alleen de bevindingen die daadwerkelijk van toepassing zijn. Verzin niets.

---

## Stap 4: Rapport genereren

Genereer een MD-rapport met de onderstaande structuur. Schrijf in duidelijke, niet-technische taal. Toon is professioneel maar direct. Geen jargon zonder uitleg.

Sla het rapport op als: `mini-analyse-{domeinnaam}.md`

---

### Rapporttemplate

```markdown
# Website-analyse: {bedrijfsnaam of domeinnaam}
*Uitgevoerd op {datum}*

---

## Wat is onderzocht

Een snelle buitenkant-analyse van {URL}. We hebben gekeken naar laadsnelheid op mobiel, vindbaarheid in Google, technische basiszaken en de opbouw van de homepage. Dit is geen volledige audit, maar een eerste scan die laat zien waar de grootste kansen (en risico's) liggen.

---

## Bevindingen

### 1. {Titel bevinding}

**Wat we zien:** ...

**Wat dit betekent:** ...

**Wat dit kost:** ...

---

### 2. {Titel bevinding}

**Wat we zien:** ...

**Wat dit betekent:** ...

**Wat dit kost:** ...

---

### 3. {Titel bevinding}

*(herhaal voor elke relevante bevinding, max 4)*

---

## Dit is het topje van de ijsberg

Bovenstaande analyse is gebaseerd op wat zichtbaar is van buitenaf, zonder toegang tot Analytics of Search Console. Een volledige SEO-analyse gaat verder:

- Welke zoekwoorden leveren nu al verkeer op, en welke laten kansen liggen?
- Hoe sterk is de concurrentie voor jouw keywords?
- Welke pagina's hebben de meeste potentie en worden nu onderschat?
- Wat is de technische staat van de volledige site, inclusief interne links en crawlfouten?

Die analyse mondt uit in een concreet verbeterplan, met prioriteiten op basis van impact.

---

## Volgende stap

Wil je weten waar de echte omzetkansen liggen? Dan is een volledige analyse de logische vervolgstap. Neem contact op voor een vrijblijvend gesprek.

*[Naam] | [Website] | [E-mail of contactlink]*
```

---

## Aandachtspunten

- **Geen oplossingen geven in het rapport.** Benoem het probleem, niet de fix. De fix is het betaalde product.
- **Wees eerlijk.** Als een site het goed doet op een bepaald vlak, zeg dat dan ook. Geloofwaardigheid is het fundament.
- **Max 4 bevindingen.** Meer maakt het rapport overweldigend en minder overtuigend.
- **Altijd afsluiten met de ijsberg-alinea.** Dit is de overgang naar de verkoopgesprek.
- **Pas de CTA aan** op basis van hoe de gebruiker zichzelf heeft ingesteld (naam, website, contactmethode).
