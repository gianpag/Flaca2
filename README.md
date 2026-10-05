# Plugga begrepp – flashcards

A tiny offline-friendly flashcard app for studying biology concepts (Swedish). One HTML file, no build, no server.

- **Live:** `https://<your-username>.github.io/<repo-name>/`
- **On iPhone:** open in Safari -> Share -> *Add to Home Screen*.

## Features
- Flip cards, mark *Igen* / *Kunde* (buttons or swipe left/right)
- Sessions of up to 20 cards, weakest first; missed cards come back soon
- Progress bar per deck, reverse mode (answer first), "practise all" mode
- Add or update decks by pasting text (`question | answer`), JSON, or a file
- Backup/restore of all decks and progress via clipboard
- Light/dark theme

## Starter content
*Kapitel 3 – Kroppen: rörelse, transport och försvar*
- Immunsystemet – inre försvar (pp. 186–191), 43 cards
- Transport via utsöndring (pp. 192–193), 17 cards

## Run locally
Just open `index.html` in a desktop browser, or serve the folder:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Adding new cards
In the app: **+ Lägg till eller uppdatera kortlek**, paste lines like

```
Vad är en atom? | Den minsta byggstenen i ett grundämne.
```

Use the same deck name to replace an existing deck's cards. See `docs/DECK_FORMAT.md` for all formats and card-writing guidelines.

To change the built-in starter decks, edit the `SEED` array in `index.html`. Note that already-installed copies keep their saved decks; use the in-app import to update those.

## Files
```
index.html          the whole app
CLAUDE.md           instructions for Claude Code
docs/DECK_FORMAT.md import formats + card-writing guide
docs/ROADMAP.md     ideas and next steps
```
