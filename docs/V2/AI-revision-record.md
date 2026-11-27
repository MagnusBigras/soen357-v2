# AI-use record: iteration 1 revision (V1 → V2)

For the Phase 3 AI-use appendix. Keeps what the AI was given, what it produced and what the team
changed by hand separate. No participant names or identifying data were shared with the AI: only the
anonymised round 1 findings (participant categories and counts).

## Tool

| Item | Value |
|---|---|
| Tool | Claude (claude.ai, agent session with file and browser tools) |
| Model | Claude Opus 5.5 (`claude-opus-5-5`) |
| Date | 27 Nov 2026 |
| Session operator | *(team member)* |

## Inputs given to the AI

| Input | What it supplied |
|---|---|
| V1 repository (`soen357-v1-main.zip`, tag `v1.0`) | Code, tests, calculation rules, V1 records |
| Phase 3 report template (`Report_template.docx`) | Report structure and Sections 1–3 |
| Round 1 findings F1–F4 and the three requested fixes (prompt below) | What to change |

## Prompt (verbatim)

> Attached is the first iteration of our iterative meal planning website. A testing session was conducted with 4 participants, two of which were home cooks and 2 were students.
>
> Observed findings:
>
> * F1: Within the meal's details page, users are unable to modify serving size. During the test session 2 out 4 of the participants needed a hint to actually complete the task and modify the serving size. They had to leave the page and edit it through the Meal page.
> * F2: One participant had a difficult back button while on the meal details page. It currently sits on the top left corner of the page, rather than within the card, making it easy to miss. This resulted in them being stuck on a recipe they did not want to cook.
> * F3: Half the participants did not notice the serving chip on the Meals page, instead they changed their default serving size within their profile.
> * F4: All of the participants noticed that while on the meals detail page, navigating to another tab then back to the Meals tab returns to the recipe, rather than back to the list. Making getting back to the list difficult.
>
> Fixes:
>
> * Add a serving stepper that allows users to modify serving sizes from the meal details page. Modifying the serving should update the quantity of ingredients immediately.
> * Add a clear back to Meals button at the top of the meal details card, near the recipe title. This should return users back to the meals page.
> * When the user is on a meals-sub screen, such as the search or a recipe details page, if they navigate away and return, they should be brought back to the main Meals page.
>
> Constraints:
>
> * Keep the task flows, calculation rules CR1–CR9, routes, colours, font and accessibility features from V1.

## How the AI interpreted the prompt

1. Change only what F1–F4 call for; everything else stays as in V1 so round 2 results stay comparable.
2. The detail-page stepper changes the **same** Servings value as the Meals chip (`S.filters.servings`).
   This keeps CR3 exactly (shown quantity = recipe quantity × servings ÷ 2), means a missing item added
   from the detail page uses the servings shown there, and also answers F3: a value set on the recipe
   shows on the Meals chip. Range and message are V1's (1–12, “Enter servings from 1 to 12”).
3. “Immediately”: a valid value updates the ingredient list, the meta line and *Why this meal* on the
   same page, without a page change or Apply button. Invalid values show the V1 message and leave the
   last valid quantities on screen. A screen-reader status announces “Quantities updated for N servings”.
4. *Back to Meals* always goes to the Meals list (`#/meals`), also when the recipe was opened from
   Search; the header Back button is kept, so V1's “Back to Search” route still exists.
5. F4 is fixed for every way of leaving the Meals tab (tab bar, *View grocery list*, browser Back).
   Pantry, Grocery list and Profile keep V1's per-tab memory. Re-tapping the Meals tab while on a
   recipe still behaves as in V1 (not part of the findings).
6. No new routes, colours or fonts. New controls reuse V1 components (`.btn-secondary`, `.stepper`,
   `.panel`, error and help styles) with 44 px targets and the V1 focus ring.

## Outputs used

| Output | Files |
|---|---|
| Website changes | `js/screens-meals.js` (detail card, stepper, Back to Meals), `js/router.js` (`backToMeals`, Meals tab reset), `css/styles.css` (V2 block), `js/data.js` (`APP_VERSION = "V2"`), `js/store.js` and `js/router.js` (storage keys `mpa.v2.*`), `index.html` (description) |
| Tests | `tests/e2e-verify.js`: W-02 updated for F4; V2-01 to V2-17 added; output to `docs/V2/` |
| Documentation | `README.md`, `docs/V2/*` |

The storage keys were renamed (`mpa.v1.*` → `mpa.v2.*`) so V1 and V2 on the same GitHub Pages
domain do not share saved data during test sessions.

## Verification before freezing V2

- `node --test tests/logic.test.js`: 30 of 30 pass (calculation rules unchanged).
- `node tests/e2e-verify.js`: 85 of 85 pass (all V1 checks, W-02 updated for F4, V2-01 to V2-17).
- Screenshots checked by eye at 375 px, 1280 px and 130 % display size (`screenshots/V2_*`).

## Student modifications

*(Record any manual changes the team makes after this session.)*
