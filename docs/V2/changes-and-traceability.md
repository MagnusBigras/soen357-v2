# V1 → V2 changes and traceability

Round 1: 4 participants (2 home cooks, 2 students), V1 with `?reset&today=2026-11-05`.
Check IDs refer to `verification-results.md`.

## Findings, priority and changes

Priority: **High** = blocked or needed help on an essential task, or affected most participants;
**Medium** = slowed participants, workaround found; **Low** = cosmetic.

| Finding | Participants | Task | Priority | Change in V2 | Code | Checked by |
|---|---|---|---|---|---|---|
| F1 Serving size cannot be changed on the meal detail page; participants left the page to change it on Meals | 2 of 4 needed a hint (assisted success) | T2, T3 (correctness) | High | Serving stepper on the meal detail card; quantities update immediately | `SCREENS.detail`, `ingList`, `whyList` | V2-02 to V2-12, V2-17 |
| F4 Leaving a recipe and returning to Meals reopens the recipe, not the list | 4 of 4 | T2 (workflow) | High | Meals tab reopens on the Meals list after leaving it from Search or a recipe | `show()` in `router.js` | W-02, V2-13, V2-14, V2-16 |
| F3 Serving chip on the Meals page not noticed; Profile default changed instead | 2 of 4 | T2 (visibility) | Medium | Addressed through F1: servings can be changed where quantities are read, and the value is shared with the Meals chip | as F1 | V2-10, V2-11 |
| F2 Header Back button (top left) easy to miss; participant stuck on an unwanted recipe | 1 of 4 | T2 (visibility, recovery) | Medium | *Back to Meals* button at the top of the detail card, above the title; returns to the Meals list | `SCREENS.detail`, `backToMeals()` | V2-01, V2-10, V2-15, V2-17 |

## What was kept from V1 (constraints)

| Kept | How it was checked |
|---|---|
| Task flows T1–T3, messages, states S1–S15 | All V1 browser checks still pass (T1-*, T2-*, T3-*, P-*, W-*) |
| Calculation rules CR1–CR9 | `tests/logic.test.js` 30/30; V2-03, V2-06, V2-09, V2-11, V2-12 compare detail-page quantities with CR3 expected values (C9–C12) |
| Routes (`#/pantry`, `#/meals`, `#/meals/search`, `#/meals/recipe/{id}`, …) | No route added or renamed; W-06 to W-08 |
| Colours, font | No new colour values; new controls use V1 components and variables |
| Accessibility | 44 px targets, visible labels, V1 focus ring, `aria-invalid` + error text, live status for quantity updates, focus kept on stepper buttons (V2-05), no overflow at 130 % (V2-17) |

## Expected values for the new stepper (CR3, reference date 5 Nov 2026)

| Recipe | Servings | Expected | Check |
|---|---|---|---|
| Skillet | 3 | Zucchini 3 units, Rice 225 g, Canned tomatoes 1.5 can, Olive oil 22.5 ml | V2-03 |
| Skillet | 1 | 1 unit, 75 g, 0.5 can, 7.5 ml | V2-06 |
| Skillet | 4 | 4 units, 300 g, 2 can, 30 ml | V2-09 |
| Pasta | 4 | Pasta 400 g on detail and on the grocery list | V2-11, V2-12 |
| any | 0 or 13 | “Enter servings from 1 to 12”; quantities unchanged | V2-07, V2-08 |

## Known limitations carried into round 2

- Servings are one shared value; changing them on a recipe also changes the Meals chip.
- Grocery items already added keep their quantity if servings change later (V1 limitation).
- Re-tapping the Meals tab while on a recipe re-shows the recipe (not reported in round 1).
- The Meals page Servings chip itself was not redesigned; round 2 should check whether F3 recurs.
