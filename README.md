# Meal Planning Assistant – Version 2

SOEN 357 Phase 3 · The Vth Division · **Version 2 (V2)**: V1 revised with AI after usability round 1 (4 participants: 2 home cooks, 2 students).
V1 was generated from the Phase 2 high-fidelity prototype (Output C).

Sample website for learning and educational purposes only. Not for commercial use or commercial testing.
All foods, recipes and dates are fictional sample data.

**Live website:** *(add the V2 GitHub Pages link)* · V1: https://whoisyazzy.github.io/soen357-v1/#/pantry

## What changed in V2

| Finding (round 1) | Change in V2 | Checked by |
|---|---|---|
| F1 Serving size could not be changed on the meal detail page (2 of 4 needed a hint) | Serving stepper (− / field / +) on the meal detail card. Ingredient quantities, the meta line and *Why this meal* update immediately (CR3 unchanged). It sets the same Servings value as the Meals chip. | V2-02 to V2-12 |
| F2 Back button in the top-left corner was missed (1 of 4) | **Back to Meals** button at the top of the meal detail card, above the recipe title. Returns to the Meals list. The header Back button stays. | V2-01, V2-10, V2-15 |
| F3 Serving chip on Meals not noticed (2 of 4 changed Profile instead) | Covered by the detail-page stepper (F1); a value set there also shows on the Meals chip. | V2-10, V2-11 |
| F4 Leaving a recipe and returning to the Meals tab reopened the recipe (4 of 4) | Leaving the Meals tab from Search or a meal detail (by tab, *View grocery list* or browser Back) resets it; the Meals tab then opens on the Meals list. Other tabs still remember their place. | W-02, V2-13, V2-14, V2-16 |

Unchanged from V1: task flows T1–T3, calculation rules CR1–CR9, screen addresses, colours, font and
accessibility features. Details: `docs/V2/`.

## What it does

The website helps a student plan meals from what is already in their pantry. It supports the three
essential tasks from Output C:

| Task | What the user does |
|---|---|
| **T1 Manage pantry** | Add an ingredient with quantity, unit, storage location and date; edit or remove it. Items close to their date are shown as *Use soon*. |
| **T2 Select a suitable meal** | See meal suggestions that use pantry items (Use soon first), filtered by servings, maximum time and Profile allergies; search recipes; open a meal and select it. |
| **T3 Prepare grocery list** | Add a selected meal's missing ingredients to the grocery list without duplicates; change quantities, add or remove items, tick items as bought. |

Every action that changes data shows a confirmation with **Undo**, and errors explain how to fix them.

## Run it

**On the web:** open the live link above.

**On your computer (no installation):**
1. Download or clone this repository.
2. Double-click `index.html` (Chrome, Edge, Firefox or Safari). No internet connection is needed.

Or serve the folder, which matches GitHub Pages exactly:
```bash
python3 -m http.server 8357
# then open http://localhost:8357/
```

**Publish on GitHub Pages:** Settings → Pages → Source: *Deploy from a branch* → Branch `main`, folder `/ (root)` → Save. The link appears after about a minute.

## For test sessions (moderators)

Open this address before each participant (replace the start with the live link):

```
<V2 live link>/?reset&today=2026-11-05#/pantry
```

- `?reset` restores the sample data (Profile → *Reset sample data* does the same).
- `?today=2026-11-05` fixes “today” to the Output C reference date, so every participant sees the same
  dates and the expected values in `docs/V1/calculation-rules.md` apply. The page shows “Today (test date)”.
  It stays until you open `?today=off`.

Both options are removed from the address bar after loading.

### Sample inputs for the essential tasks (with the test date)

- **T1:** Pantry → Add ingredient → Name *Zucchini*, Quantity *2*, Unit *units*, Storage *Fridge*, Date type *Reminder*, Date *7 Nov 2026* → Save. Then tap Zucchini, change the quantity to *3*, Save.
- **T2:** Meals → *Zucchini and tomato rice skillet* → change Servings to *3* with the stepper on the card → Select this meal.
- **T3:** Meals → *Vegetable pasta* → Select this meal → *Add 1 missing item to grocery list* → View grocery list → change the quantity.

Without a test date, use a reminder date 2 days from today for T1.

## Simulated behaviour and limitations

- Four fictional sample recipes. Suggestions come from a fixed rule set (Use soon items first, then fewer missing items, then shorter time), not an AI model. There is no server.
- Ingredients are matched to the pantry **by name only**; quantities are not compared (1 zucchini counts as *Have* for a recipe that needs 2).
- Recipe quantities are scaled to the chosen servings and can be fractional (e.g. “1.5 can” at 3 servings).
- Items already on the grocery list keep their quantity if servings change later.
- Servings are one shared value: changing them on a meal detail page also changes the Meals chip (and the reverse).
- Saving the Profile resets the Meals filters (Servings, Max time) to the Profile values.
- Renaming an item in Edit to a name already in the pantry is not caught as a duplicate.
- Dietary preferences are stored but do not filter the sample recipes.
- Data is saved in this browser only (no accounts, no sync). Clearing browser data or using a private window starts from the sample data.
- Allergen information is from the sample recipes only. Always check product labels.
- There are no loading states because nothing is fetched.

## Project structure

```
index.html              page shell
css/styles.css          styles (colours, type and components from Output C)
fonts/                  Atkinson Hyperlegible (SIL Open Font License 1.1)
js/data.js              icons, lists, sample recipes and sample pantry
js/logic.js             calculation and validation rules (no DOM; unit-tested)
js/store.js             saved data, today's date, ?reset and ?today options
js/ui.js                helpers, toast messages, dialogs
js/router.js            screen addresses, Back/Forward, per-tab memory, unsaved-changes guard
js/screens-*.js         Pantry (T1), Meals (T2, T3), Grocery list (T3), Profile
js/app.js               start-up
tests/logic.test.js     30 calculation/validation checks:   node --test tests/logic.test.js
tests/e2e-verify.js     85 browser checks (V1 checks + V2-01 to V2-17):  npm install, npx playwright install chromium, npm start, then npm run verify
docs/V1/                V1 records (unchanged)
docs/V2/                AI-use record for V2, findings-to-changes traceability, verification results, screenshots
```

## Versions

| Version | Built from | Status |
|---|---|---|
| V1 | Output C (Phase 2) | `soen357-v1`, tag `v1.0` |
| V2 | V1 + iteration 1 findings | This repository, tag `v2.0` |
| V3 | V2 + iteration 2 findings | — |
| V4 | V3 + iteration 3 findings (final) | — |

Keep each tested version unchanged (git tag) so results stay comparable.

## Credits

Font: Atkinson Hyperlegible, Braille Institute of America, SIL Open Font License 1.1.
Generated with Claude (Anthropic); see `docs/V1/AI-generation-record.md` and `docs/V2/AI-revision-record.md`.
