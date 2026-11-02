# Omni-proxy
OmniProxy: print-ready proxy sheets for Yu-Gi-Oh!, Magic, and Pokémon. Search, add, and download an PDF with cut lines.

# OmniProxy

A single-file web app that turns a deck list into a print-ready PDF with dotted cut lines. Supports **Yu-Gi-Oh!, Magic: The Gathering, and Pokémon**. Search a card, add it to the sheet, and download. No install, no build step, no backend.

<!-- Add a screenshot here: ![OmniProxy](screenshot.png) -->

## Features

- **Deck import**: paste a text list (`3x Dark Magician`, `Dark Magician x3`), a `.ydk` file (Yu-Gi-Oh!), or a Magic Arena or Pokémon TCG Live export (`4 Lightning Bolt (M11) 149`, `4 Pikachu SVI 63`), or open a file. Optional side deck, and cards are merged into your sheet
- **Three games**: Yu-Gi-Oh! ([YGOPRODeck](https://ygoprodeck.com/api-guide/)), Magic ([Scryfall](https://scryfall.com/docs/api)), and Pokémon ([pokemontcg.io](https://docs.pokemontcg.io/)). Each game keeps its own sheet and starts at its usual card size
- **Card search** with live results from each database, so new cards appear as soon as the database adds them
- **Alt art and other printings**: every alternate artwork or printing shows up as its own tagged result (Yu-Gi-Oh! alt arts, Magic and Pokémon set and number), and you can switch a card's version right from the sheet
- **Paper sizes**: A4, Letter (short bond), Long bond, 5R and 4R photo paper, in portrait or landscape
- **Custom sizes**: enter any paper size or card size in millimetres, or pick the Poker/MTG preset (63 × 88 mm)
- **Auto grid layout** at the Yu-Gi-Oh! 59 × 86 mm card size by default (3 × 3, so 9 cards per page on A4)
- **Live preview** of every page, including empty slots
- **Dotted cut lines** between cards to guide scissors
- **Bleed** (0 to 3 mm): the card art is enlarged behind each card so a slightly off cut doesn't leave a white edge
- **Crop marks**: solid ticks outside the grid at the end of every cut line, so nothing is printed on the cards. Handy for a paper cutter or a ruler
- **Adjustable margin and gap** between cards
- **Quantity controls** per card, with extra pages added automatically
- **PDF export** in the browser with [jsPDF](https://github.com/parallax/jsPDF); duplicate cards are embedded once to keep file size down
- **Dark and light mode**, following your system theme
- Works on mobile and desktop

## Usage

1. Open `index.html` in any modern browser (or use the GitHub Pages link, if enabled).
2. Pick a game, type a card name in **Find cards**, and tap a result to add it.
3. Adjust quantity and artwork in the **Sheet** list.
4. Tweak margin, gap, and cut lines under **Layout**.
5. Tap **Download PDF** and print at **100% scale** (turn off "fit to page").

Rename `omniproxy.html` to `index.html` if you want to host it on GitHub Pages.

## Print tips

- Print at 100% or "actual size". Any scaling changes the card dimensions and they won't fit sleeves.
- Pick your paper size first. A4, Letter and Long bond fit 9 cards, 5R fits 4, and 4R fits 2 in landscape (1 in portrait). Small photo sizes need borderless printing because the margin is only 3 mm.
- Bleed of about 1 mm is usually enough. Crop marks need roughly 4 to 5 mm of free paper around the grid, so use a larger margin or smaller card count if they get shortened.
- Images are stretched to the card size you choose, so keep custom cards close to a 0.69 width-to-height ratio (59 × 86) to avoid distortion.
- Card images are about 421 × 614 px, roughly 180 dpi at 59 mm. That's good for playtesting, not for photo-quality prints.
- For sleeves, a heavier paper or a real card behind the proxy works best.

## How it works

| Part | What it does |
| --- | --- |
| `GAMES` adapters | Each game provides `search()` and `lookup()` (plus `byIds()` for `.ydk`) and returns entries with their other versions grouped together. Adding a game means writing one adapter and one `GAMES` entry |
| `computeLayout()` | Pure function that works out columns, rows, centered slot positions, and cut-line coordinates from the margin and gap. The preview and PDF both use it, so they always match |
| `paginate()` | Splits the expanded card list into pages |
| `exportPDF()` | Loads each unique image once, places cards with jsPDF, and draws the cut lines |

Everything lives in one HTML file, with a `CONFIG` object at the top for card size, page size, and proxy settings.

## Game notes

- **Magic** uses Scryfall's `large` images (672 × 936). Double-faced cards show the back face as its own tagged result.
- **Pokémon** uses pokemontcg.io, which can be slow without an API key. A free key from [dev.pokemontcg.io](https://dev.pokemontcg.io) raises the rate limit; set it in `CONFIG.pokemonApiKey`.
- Deck lists for Magic and Pokémon are matched by name, or by set and number when the export includes them. Names are case-insensitive, and Magic names use fuzzy matching.

## Image loading and CORS

Some image servers (YGOPRODeck for sure) don't send CORS headers, so browsers can't read the images directly for the PDF. On export the app tries a direct fetch first, then falls back to public image proxies ([weserv.nl](https://images.weserv.nl/) and [corsproxy.io](https://corsproxy.io/)). That means card images pass through those services during export.

If you want to avoid that, set your own proxy in `CONFIG.imageProxy`, or download and host the card images yourself. YGOPRODeck also asks that apps [store pulled data locally](https://ygoprodeck.com/api-guide/) instead of hotlinking, and rate-limits the API to 20 requests per second. Scryfall asks apps to keep to about 10 requests per second.

## Roadmap

- [ ] More games (Lorcana, One Piece, Digimon): add an adapter to `GAMES`
- [ ] Local image caching

## Disclaimer

This is an unofficial fan tool for **casual playtesting and proxy decks**. It is not affiliated with or endorsed by Konami, Wizards of the Coast, Nintendo, The Pokémon Company, YGOPRODeck, Scryfall, or pokemontcg.io. Yu-Gi-Oh!, Magic: The Gathering, and Pokémon are trademarks of their respective owners. Card images and data come from the APIs above and belong to their respective owners. Please don't use proxies in sanctioned tournaments or to sell counterfeit cards.


