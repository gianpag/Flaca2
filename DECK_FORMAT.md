# Deck format and card-writing guide

## Plain text (easiest)
One card per line. Separator can be ` | `, `|`, a tab, or `;` (first one found on the line is used). Use ` / ` inside an answer to create a line break.

```
Vad är en epidemi? | När en infektionssjukdom sprider sig och allt fler blir sjuka.
Halsfluss: orsak och smittväg? | Streptokockbakterie. / Direktkontakt.
```

- If the first line has no separator and you leave the name field empty, it is used as the deck name (a leading `# ` is stripped).
- Lines without a separator are skipped and reported.

## JSON
```json
{ "name": "Kap 4 – Kemi",
  "cards": [ { "q": "Vad är en atom?", "a": "Den minsta byggstenen i ett grundämne." } ] }
```
Also accepted: a bare array of `{q,a}` or `[q,a]` pairs, `{ "decks": [ ... ] }` for several decks, and the keys `question/answer`, `fråga/svar`, `front/back`.

## Full backup
The "Kopiera säkerhetskopia" button copies the entire state (decks + progress + prefs) as JSON. Pasting it into the import box restores everything (after a confirmation) and replaces current data.

## Behaviour on re-import
Same deck name (case-insensitive) -> cards are replaced. Progress is kept for cards whose **question text is unchanged**; changed questions start over.

## Writing good cards
- One fact per card; keep answers to 1–2 short sentences.
- Use the textbook's own terms (e.g. *ätarceller*, *letarceller*, *minnesceller*).
- For tables (like the disease table), make one card per row: "X: orsak, smittväg, inkubationstid?"
- Ask "why/how" questions for processes, "what is" for definitions.
- Include the source in the deck name or `src` field (chapter and pages).

## Making cards from photos
Photograph textbook pages (including the "Kan du?" and "Arbeta med begrepp" boxes), give them to Claude, and ask for cards in the `question | answer` format, in Swedish, matching the book's wording. Paste the result into the app.
