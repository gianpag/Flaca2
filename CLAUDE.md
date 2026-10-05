# CLAUDE.md

Guidance for Claude Code when working in this repo.

## What this is
A single-file flashcard web app (`index.html`) for studying Swedish school-biology concepts. It runs entirely in the browser, is hosted on GitHub Pages, and is used mainly on an iPhone (Safari, added to the Home Screen). The user-facing language is **Swedish**; code and comments are English.

Initial content: *Kapitel 3 – Kroppen: rörelse, transport och försvar*, pp. 186–193 (Immunsystemet, Transport via utsöndring), seeded as two decks in the `SEED` constant.

## Hard constraints
- **One file, no build step, no dependencies.** Everything (HTML, CSS, JS, seed cards) lives in `index.html`. Do not introduce a bundler, framework, or npm packages unless the user explicitly asks.
- **No server.** Must work as static hosting. All data lives in the browser (`localStorage`).
- **iPhone Safari first.** Test at ~390px width. Respect safe-area insets (already set on `:root`). Tap targets >= 44px. Avoid hover-only UI.
- **Do not break existing user data.** Saved state lives under `localStorage["begrepp.v1"]`. Any change to the state shape needs a migration in `loadLocal()`; never silently discard saved progress.
- Keep UI strings in Swedish and consistent in tone (informal "du").

## Architecture (all in `index.html`)
- `SEED`: array of decks, each `{id, name, group: [string, ...], src, cards: [[question, answer], ...]}`. `group` is a path of labels (e.g. `["Gretas Skol","Biologi","Kapitel 3 – Kroppen"]`) used to nest the deck under collapsible accordions on `#home`, deepest-last.
- `JP_ITEMS` + `buildJapaneseDecks()`: the ported "Kotoba"/WaniKani Japanese dataset (`Old japanese flashcard app to port to flaca2/`), compacted into `[type, level, characters, meanings, readings]` tuples and chunked into one deck per WaniKani level (`jp-l1` … `jp-l60`, "Nivå 1" … "Nivå 60") under `["Kotoba – Japanska","WaniKani-ordförråd"]`. Appended once to `state.decks` on first load that lacks a `jp-*` deck id — this is a one-time content migration, not a seed, so it reaches existing users too. `migrateJapaneseRangeDecks()` additionally re-keys progress from the older 5-level-chunk layout (`jp-l1-5` etc.) if present.
- `state`: `{updatedAt, decks: [{id, name, group, src, cards: [{q, a}]}], progress: {"<deckId>::<question>": level}, prefs: {<deckId>: {reverse, all}}}`.
- Persistence: `persist()` -> `saveLocal()` (+ optional Claude-account sync, see below). Theme is stored separately in `localStorage["begrepp.theme"]`.
- Study logic: `startStudy(deckId)` studies one deck; `startStudyMulti(deckIds, opts)` blends any number of decks into one shuffled session (used by the "Öva flera kortlekar ihop" sheet). Both funnel through `buildPool()` + `beginSession()`, building a session of up to 20 cards (each tagged with its own `deckId` so progress/`key()` still resolves correctly across decks), lowest level first (shuffled within level), unmastered cards only unless `all`. `answer(good)`: Kunde = level +1 (cap 5); Igen = level 0 and the card is re-inserted up to 3 cards later. `MASTER = 3` counts as "kan".
- Import: `parseInput()` accepts `question | answer` lines (also tab or `;`), JSON (`{name, cards:[{q,a}]}`, arrays, or `{decks:[...]}`), and a full backup (detected by `decks[0].id` + `progress`). `importDecks()` replaces cards of a deck with the same name (case-insensitive) or adds a new one.
- Views: `#home`, `#study`, plus bottom sheets `#importSheet` and `#deckSheet`. Card flip is a CSS 3D transform; swipe is a touch handler on `#stage`.

## Known quirks / tech debt
- Progress is keyed by **question text**. Editing a question resets its progress.
- `showCard()` sets `#count` twice; the first assignment is dead code and can be removed.
- Restoring a backup reports success through the error line (`"Återställt!"`) and leaves the sheet open. Should close the sheet and show a toast.
- The `connect()` block using `window.claude.use("db"/"user")` is for the claude.ai-hosted copy only. It is inert on GitHub Pages (guarded by `if(!window.claude...) return`). Leave it unless asked to remove it.
- Google Fonts are loaded from the network; the CSS has system-font fallbacks so the app still works offline.

## Working rules
- After any JS change, run a syntax check: extract the `<script>` content and run `node --check`.
- Prefer small, surgical edits over rewrites. Keep the file readable (sections are marked with `/* ---------- ... ---------- */`).
- When adding cards, follow `docs/DECK_FORMAT.md`: one fact per card, short answers, Swedish, wording close to the textbook.
- Update `docs/ROADMAP.md` when finishing or adding items.

## Deploy
Push to `main`; GitHub Pages serves the repo root (`index.html`). Verify on an iPhone in Safari after deploying, since local-only testing misses Safari-specific issues.
