# Prompt-to-Plate prototype

**Live:** [srinathvenkatesh25.github.io/genai-capstone-project/prototype/](https://srinathvenkatesh25.github.io/genai-capstone-project/prototype/)

A clickable walk-through of Prompt-to-Plate over **one fixed sample week**: 7 days of Mexican meals at
1,850 kcal and 80 g protein a day, ending in a 14-item Kroger cart of $54.50 against an $80 budget.

**This is a fixed-scenario simulation.** The plan, the cart and the progress are all scripted from that
sample week, and changing the form does not generate a different plan. Nothing is sent anywhere, and
there's no Instacart access. It demonstrates the full target experience described in
[`DESIGN_SPEC.md`](../DESIGN_SPEC.md), independent of the phased build order in
[`OPPORTUNITY_FRAMING.md`](../validation/OPPORTUNITY_FRAMING.md). The spec's table "Target product vs
prototype" (section 0.2) lists the status of each capability.

## Open it

Use the live link, or open `index.html` directly in a browser. It works from the file system; only
the fonts come from the web.

## Using it

The dark bar at the top is outside the product:

- **Journey** jumps to a journey from the spec:

  | | Journey |
  |---|---|
  | J1 | Set targets |
  | J2 | Review and ask why |
  | J3 | Approve |
  | J4 | Shop, with pauses |
  | J5 | Check the cart and hand off |

- **Play as** picks one of the three personas. A guide in the side rail lists that persona's tasks,
  ticks each one off as you do it, and shows their success criteria at the end.
- **Failure** shows a failure flow: the cart needs a look, every AI model is busy, or Instacart
  delivers to a different ZIP.
- **Dark / Light** switches the theme.

## What to look for

| | Where |
|---|---|
| Decision rights | Approve or cancel the list, swap a meal, untick an item, answer each pause (sign-in code, existing cart, substitute), and check out yourself |
| Interrogation | Open a meal for its macros; **Why?** on an ingredient shows the USDA entry and the arithmetic; **Why this product?** in the cart; the store-ranking card; how the plan was checked |
| Trust cues | Rounding note, model-switch lines in the feed, AI usage line, need-vs-have for every item, estimate and estimated-weight badges, the verified stamp, the ZIP warning |

## What works, and what is simulated

| | In this prototype |
|---|---|
| **Works** | ZIP validation (5 digits, required); unticking grocery items updates the item count and estimate against the budget; the approval gate; per-meal and per-ingredient numbers and the **Why?** panels (real USDA values for the sample week); **Why this product?**; the persona guide; the light and dark themes |
| **Simulated** | The plan itself (always the same sample week); "Describe in words"; planning progress; the Monday-dinner swap and the weekly brown-rice change (both pre-computed with real numbers); **Simulate filling cart** (scripted progress and pauses: sign-in code, existing cart, substitute); the cart and its check; **Simulate Instacart hand-off**; the failure flows |
| **Illustrative only** | Every other form input (targets, cuisines, dislikes, allergies, restrictions, time limit, equipment, pantry) and the store-ranking numbers |

## Limits of the prototype

- The plan doesn't change with the form; the sample-scenario banner under the header says so.
- Only Monday dinner can be swapped. The other Swap buttons say so.
- The cart is fixed, so plan changes you make here aren't reflected in it. The page says so when
  this applies.
- "Verified" on the cart is simulated with sample data. In the target product it means the cart was
  read back from the store with every item covered, and nutrition computed from USDA data (see the
  spec's validation contract, section 0.3).

## Files

| File | What it is |
|---|---|
| `index.html` | The page |
| `styles.css` | Styles |
| `app.js` | The scripted walk-through |
| `data/run.json` | The sample week |
| `data/run.js` | The same data, loaded as a script so the page also works from the file system |
