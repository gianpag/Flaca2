# Roadmap

## Next up
- [ ] Make it a real offline PWA: `manifest.webmanifest`, service worker caching `index.html`, apple-touch-icon, self-hosted fonts (or system fonts only).
- [ ] Remove the claude.ai-only `connect()` sync block (or keep behind a flag).
- [ ] Fix backup restore UX (close sheet + toast instead of the error line).
- [ ] Remove the duplicate `#count` assignment in `showCard()`.
- [ ] Add a stable card id so editing a question no longer resets its progress.

## Study experience
- [x] Swipe gesture now uses Pointer Events instead of touch-only handlers, so dragging to grade a card works with a mouse on desktop too, not just touch.
- [x] Clearer Igen/Kunde buttons: two-line labels with an icon + a short "what this does" subtitle (↺ Igen / Öva mer, ✓ Kunde / Jag kunde svaret) instead of single bare words.
- [x] English/Swedish UI language toggle (`#langBtn`, top-right of `#home`), persisted in `localStorage["begrepp.lang"]`. Translates UI chrome only — deck/card content stays as authored.
- [ ] Proper spaced repetition (due dates, intervals) instead of simple levels.
- [ ] Study-all-decks mode and per-session length setting.
- [ ] Typed-answer / multiple-choice mode (the book has "Vilka hör ihop?" matching exercises).
- [ ] Concept-map prompts for the "Arbeta med begrepp" terms.
- [ ] Stats: streaks, cards learned per day.

## Content
- [x] Group decks by subject/chapter on the home screen (collapsible `<details>` sections) so adding more decks doesn't crowd `#home`.
- [x] Nested grouping: a deck's `group` is an arbitrary path array (e.g. `["Gretas Skol","Biologi","Kapitel 3 – Kroppen"]`), rendered as nested accordions.
- [x] Port the "Kotoba" Japanese/WaniKani flashcard app (see `Old japanese flashcard app to port to flaca2/`) as decks under `Kotoba – Japanska` › `WaniKani-ordförråd`, one deck per level (Nivå 1 … 60). Uses Flaca2's simple level engine, not the original's SRS/due-dates/lessons/library/insights — see README "Known quirks" for what was intentionally dropped.
- [x] "Öva flera kortlekar ihop": pick any combination of decks from a checklist sheet and study them as one blended, shuffled session (`startStudyMulti`).
- [ ] Add next chapters as decks (photo -> cards workflow in `docs/DECK_FORMAT.md`), tagging each with its `group` path.
- [ ] Add images for diagrams (kidney, antibiotic resistance, excretory system) as inline SVG or data URIs.
- [ ] Optional deck files under `decks/*.json`, loaded at startup, so content updates don't require editing `index.html`.

## Quality
- [ ] Small Node test for `parseInput` / `importDecks`.
- [ ] Accessibility pass (VoiceOver labels, focus order in sheets).
- [ ] Download backup as a `.json` file in addition to clipboard.
