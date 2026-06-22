---
name: wordpress-gutenberg
description: Helpt bij elke WordPress implementatievraag op Jelle's client-sites. Gebruik deze skill altijd bij Gutenberg/FSE vragen, block styling, theme.json, CSS-plaatsing, register_block_style, Swiper/carousel, accordeon, navigatie, plugingedrag (Twentig), of wanneer een WordPress-probleem of screenshot van een WP-site wordt gedeeld. Trigger ook bij "in WordPress", "Gutenberg", "block editor", "theme.json", "is-style", "child theme", of een client-site URL (ngtalen.nl, vondeldam.nl, dannycool.nl).
---

# WordPress / Gutenberg

Jelle bouwt client-sites Gutenberg/FSE-native. Geen page builders. Deze skill voorkomt dat dezelfde dingen elke keer opnieuw uitgelegd worden en stopt onnodige vraag-rondes.

---

## Uitgangspunten (altijd)

- Gutenberg/FSE native. Geen Elementor, geen Divi. Blijf binnen WP core blocks waar het kan.
- Child theme voor maatwerk. Favoriete thema: Olly.
- Migratie: frisse installatie op subdomein, niet staging.
- Jelle bouwt zelf en plakt CSS/PHP zelf in. Lever werkende code, geen losse fragmenten zonder plaatsing.

---

## Gedrag (dit is waar het meestal misging)

- **Begin met de implementatie, niet met vragen.** Als de oplossingsruimte smal is, geef de aanpak met aannames hardop ("ik ga ervan uit dat dit Gutenberg-native is"). Stel alleen een vraag als je echt vastloopt zonder. Open nooit met 2-3 verduidelijkingsvragen op een rechttoe-rechtaan technische vraag.
- **Als Jelle de oorzaak al benoemt, werk daarvandaan.** Hij kent zijn eigen site. Ga niet alsnog zoeken naar externe overrides of "het kan ook caching zijn" als hij de bron al heeft aangewezen.
- **Parseer losjes.** Jelle dicteert vaak. Lees de bedoeling, niet de letterlijke woorden.
- **Geen neutrale optielijstjes voor een opgelost probleem.** Geef de aanbevolen route en benoem kort de trade-off. Niet vier opties opsommen en hem laten kiezen tenzij het echt een keuze is.
- **Web search resultaten zijn niet bevestigd tot ze getest zijn.** Presenteer een gevonden oplossing niet als werkend feit. Zeg dat het getest moet worden.

---

## register_block_style()

Werkt op elke core block (list, heading, paragraph, etc.). PHP registreert de variant, CSS doet de styling via `.is-style-{naam}`.

```php
register_block_style( 'core/heading', [
    'name'  => 'gradient-brand',
    'label' => 'Gradient Brand',
]);
```

```css
.is-style-gradient-brand {
    background-image: var(--wp--preset--gradient--brand);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
}
```

Block style class zit op het hele block, niet per heading-level. Eén style werkt dus op h1/h2/h3 tegelijk.

---

## theme.json valkuil (komt vaak terug)

CSS onder `styles.blocks.core/X.css` in theme.json is een **globale** regel op élke instance van dat block. Een nieuwe `.is-style-*` class voegt daar bovenop toe maar **overschrijft die globale regel niet**. Resultaat: je ziet twee stijlen door elkaar, of de globale wint op specificiteit.

Wil je per-style controle: **verwijder eerst de globale regel** uit theme.json, dan lopen alle keuzes via de block styles.

---

## CSS plaatsen: welke laag

- **Additional CSS** (Vormgeving > Customizer): snelle site-brede override, prima voor losse fixes.
- **Child theme style.css**: onderhoudbaar, maar moet ge-enqueued zijn via `wp_enqueue_scripts`. Werkt het in de editor maar niet op de frontend, check de enqueue (of caching).
- **theme.json**: design tokens en globale block-styling.

Gebruik **eigen class-namen** (`hero-shape-bg`, `hero-shape-box`), nooit de auto-gegenereerde `wp-custom-css-{hash}` classes. Die hashes zijn niet voorspelbaar of herbruikbaar.

Editor-view komt soms niet overeen met frontend bij absolute positioning / transforms. Dat is normaal, frontend is leidend.

---

## Swiper / cb-carousel-block

- Bij `slidesPerView: 3` markeert Swiper de **meest linkse zichtbare** slide als `.swiper-slide-active`. De visuele middenslide is dus `.swiper-slide-next`. Target die voor een "featured" center-effect.
- Foto die boven de card uitsteekt: gebruik **geen** `overflow: visible` (geeft ghosting bij transitions). Gebruik `padding-top` op de slide + negatieve `margin-top` op de image-container.
- Centreren van absolute foto: `left: 50%` + `transform: translateX(-50%)`.
- Navigatiepijl-kleur: `--swiper-navigation-color`.

---

## Accordeon deep-linking (WordPress 6.9+)

- Hash-open zoekt de anchor op een block **binnen de panel-content**, niet op de heading. Zet de HTML-anchor (via Geavanceerd) op het eerste block in het panel.
- Autoclose (open er één, sluit de rest) is een toggle op het **parent Accordion block**.
- Editor-preview bug: een default-open panel sluit soms niet visueel in de editor met autoclose aan, maar werkt wel op de frontend.
- Buttons: zorg dat elke `href="#anchor"` exact matcht met de anchor in het panel.

---

## Twentig plugin

- Bullet styles via `is-style-tw-*` (bv. `is-style-tw-square`). Zelfde patroon na te bouwen met `register_block_style()` in je child theme.
- "Reverse stack order" is deprecated, vervangen door "Stack with media on bottom" voor Media & Text.
- Columns responsive order-classes (`tw-sm-order-first`, `tw-sm-order-last`) werken in de praktijk onbetrouwbaar. Eerst testen voor je het aanraadt.

---

## Pattern Overrides (synced patterns)

`core/list` en `core/list-item` zijn standaard niet geregistreerd voor Block Bindings. Een one-liner in functions.php (`block_bindings_supported_attributes` filter) laat je de **tekst** van bestaande, vaste list-items per instance overschrijven. Het kan **niet** items toevoegen of verwijderen per instance (open feature request, GitHub #67509). Voor variabel aantal items: unsynced pattern voor het lijst-deel, of een Query Loop.

---

## Kleine terugkerende fixes

- **Button-tekst breekt over twee regels**: `white-space: nowrap` of een `min-width`, of de label inkorten.
- **Te veel ruimte tussen list-items**: target de selector met `revert` (valt terug op browser-default), of hard `0` als fallback.
- **Icon block uitbreiden**: Jelle wil low-code, dus plugin boven functions.php. Officiële Icon Block plugin, Font Awesome, Feather, Bootstrap Icons voegen sets toe aan het native block.

---

## Client-sites (context)

ngtalen.nl, vondeldam.nl, dannycool.nl. Content levert de klant aan tenzij anders afgesproken.

---

## Skill updaten

Zeg: "update de wordpress-gutenberg skill met deze regel: [aanpassing]". Bij elke correctie of nieuw terugkerend patroon: stel zelf een update voor.

## Changelog

- v1: aangemaakt op basis van 14 dagen WP-chats (juni 2026). Gedrag, register_block_style, theme.json valkuil, CSS-lagen, Swiper, accordeon, Twentig, pattern overrides, kleine fixes.
