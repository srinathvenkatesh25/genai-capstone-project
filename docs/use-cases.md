# Use cases and test cases

Each use case lists what the user does, what the system does, and the automated test that covers the
behaviour (in [`/tests/`](../tests/)). Tests use a scripted fake model, a fake Instacart page and an
offline USDA table, so they never call live services. The live parts are walked through in the demo.

## UC1. Plan a week and fill the cart (primary flow, live demo)

| Step | User | System | Covered by |
|---|---|---|---|
| 1 | Enters 2000 kcal, 110 g protein, ZIP, cuisine, dislikes, kitchen, pantry; clicks **Plan my week** | Creates a run; Activity shows live progress | `test_api_run.py` |
| 2 | Waits | Plans 7 days, sizes portions, validates, repairs failing days | `test_planning_graph.py`, `test_solver.py`, `test_validator.py` |
| 3 | Opens the **Meals** tab, clicks a meal | Pop-up: calories, protein, ingredients with grams, steps | manual / demo |
| 4 | Swaps a meal with a reason | New meal for that slot only, re-solved and re-checked | `test_planning_graph.py` (swap) |
| 5 | Confirms meals, unticks an item, **Confirm list and fill my Instacart cart** | Count and estimate update; the edit is sent with the approval | `test_api_run.py`, `test_consolidator.py` |
| 6 | Watches progress | Searches in 3 tabs, picks products, adds them, moves on to a second store for missing items | `test_shopper.py`, `test_instacart_page.py` |
| 7 | Reviews the cart | Per-store lines, grams needed vs in cart, subtotal vs budget; checks out in Instacart themselves | `test_shopper.py`, `test_packs.py` |

## UC2. Edge cases and error recovery (live demo)

| # | Situation | What happens | Covered by |
|---|---|---|---|
| E1 | Main protein conflicts with a diet or allergy (chicken + vegetarian) | Protein request dropped with a notice; exclusion wins | `test_planning_graph.py` |
| E2 | Day misses the calorie or protein target, or uses a disliked food | Validator names the day, meal and fix; planner repairs those days only, up to 3 rounds | `test_validator.py`, `test_planning_graph.py` |
| E3 | Model is silent or overloaded | After 45 s a second model is asked as well; first answer wins; the slow one goes to the back for 10 min | `test_llm.py` |
| E4 | Free quota used up (429), too-large request (413), out of credit (402), unusable JSON | Switches to the next model; the Activity log says which and why | `test_llm.py` |
| E5 | Run uses too many AI calls | Stops at the cap (40) with a clear message | `test_llm.py` (call-budget test) |
| E6 | Item not stocked at the first store | Tries the next shortlisted store for just those items | `test_shopper.py` |
| E7 | A product pop-up blocks a click | Pop-up dismissed; if it still fails the item becomes a note ("add it yourself") | `test_instacart_page.py` |
| E8 | Instacart asks for an email code or a CAPTCHA | Run pauses, the page asks, the answer goes straight to the browser | `test_registry.py`, `test_api_run.py` (login pause), `test_session.py` |
| E9 | Cart already has items | Asks before clearing; keeps them if the user says no | `test_shopper.py` |
| E10 | Anything tries to reach checkout, order or payment | Request aborted, including requests the guard can't inspect | `test_guard.py`, `test_session.py` (real browser) |
| E11 | Server restarts while a plan waits for approval | The approval pause survives; other unfinished runs are marked failed | `test_api_run.py`, `test_registry.py` |

## Running the tests

```bash
pip install -r app/requirements.txt
pytest -q --ignore=tests/test_session.py     # about 20 s; no network, no Chrome
pytest -q                                    # also the real-Chrome tests, about 75 s
```
