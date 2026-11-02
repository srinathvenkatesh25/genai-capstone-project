# Checkpoint 2 features against the working app

Every feature in [validation/OPPORTUNITY_FRAMING.md](../validation/OPPORTUNITY_FRAMING.md), checked against
the code in [`/app/`](../app/). The implemented scope is stated in
`proposal/[Updated]PROPOSAL_checkpoint-2.md`: user-supplied calorie and protein targets, one to seven days,
deterministic validation, targeted swaps, pantry-aware lists and a human-approved Instacart cart.

**Status:** ✅ implemented · 🟡 partly implemented · ⬜ not built

## Must-have

| Feature | Status | What the app does | Code and test |
|---|---|---|---|
| Deterministic nutrition validation | ✅ | Solver sets grams; validator checks calories ±5% and protein ±10 g from USDA data, plus exclusions, equipment, time and variety; failing days are repaired | `nutrition/solver.py`, `validator.py`; `test_solver.py`, `test_validator.py` |
| Hard-constraint escalation gates | 🟡 | A protein request that conflicts with an allergy or diet is dropped with a notice; implausible targets produce warnings; login, substitution and cart-clearing always ask. It does not stop and offer budget or protein trade-offs | `intake/intake.py`; `test_planning_graph.py` |
| Provenance tags on all data | 🟡 | Raised cooking times carry a note saying why; the list estimate is labelled an estimate and the cart shows Instacart's real prices. No per-price "confirmed on date" tag | `nutrition/timing.py`; `test_timing.py` |
| Itemized output before approval | ✅ | Meals with ingredients and grams, per-day and per-meal calories and macros, grocery list with quantities and estimate, all shown before the user approves | `frontend/components/`; `test_api_run.py` |
| Cart-to-meal coverage check | ✅ | Cart is read back and compared with the list; any uncovered item makes the run `needs_attention`, not `cart_ready` | `shopping/verify.py`; `test_shopper.py` |
| Allergen escalation (hard gate) | 🟡 | Allergens and restrictions are blocked in the plan and again in the list. Product-level allergen and cross-contact data is not read, so the user must check labels | `nutrition/maps.py`, `shopping/consolidator.py`; `test_maps.py`, `test_consolidator.py` |

## High priority

| Feature | Status | What the app does | Code and test |
|---|---|---|---|
| Cooking-time labelling | 🟡 | Times are raised to realistic floors and the meal note says so. No toggle to hide AI-estimated times | `nutrition/timing.py`; `test_timing.py` |
| Locked preferences and constraint hierarchy | ⬜ | Only a meal swap keeps the rest of the plan unchanged; meals cannot be locked | n/a |
| Visible progress during generation | ✅ | Live step messages, which model answered, a shopping progress card with items added and the current store | `ActivityDrawer.tsx`, `ShoppingProgress.tsx` |
| No-pantry as an immutable constraint | ✅ | Pantry is an input; with an empty pantry every ingredient is on the list | `shopping/consolidator.py`; `test_consolidator.py` |

## Medium priority

| Feature | Status | What the app does | Code and test |
|---|---|---|---|
| Cost-optimization mode selection | ⬜ | Ingredient reuse and a weekly budget are in the planner prompt and the cart reports over-budget amounts; there is no mode selector | `prompts/system_prompts.md`; `test_shopper.py` |
| Targeted meal swap | ✅ | Replace one meal with a reason; only that slot changes, then it is re-solved and re-checked | `graph/nodes.py: swap`; `test_api_run.py` |
| Cart-edit impact display | ⬜ | Unticking a list item updates the count and estimate, but does not show which meals lose it | n/a |
| Pantry-first ingredient swapping | 🟡 | Pantry items are removed from the list and allowed substitutes are attached; there is no suggestion engine that proposes a pantry substitute for a missing item | `shopping/consolidator.py`, `substitutions.yaml`; `test_consolidator.py` |

## Later phases (out of scope in the Checkpoint 2 proposal)

| Feature | Status |
|---|---|
| Weekly adherence tracking and feedback loop | ⬜ Phase 4 |
| Wearable and fitness integration | ⬜ Out of scope |
| Progressive dietary adaptation | ⬜ Phase 4 |

## Summary

Of the 14 features planned for this prototype, 6 are fully implemented, 5 partly, and 3 not built (locked
preferences, cost-optimization mode, cart-edit impact). The 3 later-phase features were already outside the
Checkpoint 2 scope. The Checkpoint 4 plan can pick up the partly built features (escalation options, price
provenance, a time-label toggle) and, if time allows, locking.
