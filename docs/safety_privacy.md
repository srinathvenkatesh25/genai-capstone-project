# Safety and privacy

MealCart handles food preferences, allergies, a ZIP code and a logged-in Instacart browser session. It is a
single-user app that runs on the user's own Mac: nothing is hosted, and the only outbound traffic is to
the model endpoints, USDA and Instacart. It is **not medical advice**: targets are supplied by the user
and the app does not calculate or recommend them.

## Risks, guardrails and how each is tested

| Risk | Guardrail | Test |
|---|---|---|
| **An order is placed or money is spent** | No checkout code exists. A guard on every browser context aborts checkout, order and payment URLs and GraphQL operations, and aborts any request it cannot inspect. The user is told to use an account with no saved card and checks out themselves | `test_guard.py`, `test_session.py` (real Chrome), `test_instacart_page.py` (helper tabs are guarded too) |
| **The model gets nutrition arithmetic wrong** | The model never does arithmetic. A SciPy solver sets the grams; the validator checks calories (±5%) and protein (±10 g) from USDA data; failing days are repaired, then flagged | `test_solver.py`, `test_validator.py`, `test_planning_graph.py` |
| **An allergen, restricted food or dislike appears** | The validator rejects any ingredient that contains an excluded term, and the grocery consolidator checks again as a second lock. A requested protein that conflicts with an exclusion is dropped, with a notice | `test_validator.py`, `test_maps.py`, `test_consolidator.py`, `test_planning_graph.py` |
| **Unrealistic cooking times** | Times are raised to a minimum for slow foods (raw chicken, rice, dried beans); a meal that then exceeds the user's limit is flagged and swapped | `test_timing.py` |
| **The model invents a product** | Picks must be the number of a real search result or null. Labels the model echoes back are mapped to items, and anything unmatched is treated as not found | `test_shopper.py` |
| **Prompt injection from store pages** | Product names are passed as quoted candidate rows under a fixed system prompt that only asks for a number. The reply is parsed against a JSON schema, and the only thing a pick can do is add a listed product to the cart. The guard still blocks checkout whatever the model says | `test_shopper.py`, `test_guard.py` |
| **Wrong or incomplete cart is called ready** | The cart is read back from Instacart and compared with the list. Any item not fully covered makes the run `needs_attention`, not `cart_ready` | `test_shopper.py`, `test_packs.py` |
| **Login codes or passwords leak** | Answers pass from the page straight to the waiting browser code in memory. They are never written to the database, checkpoints or logs, and the event store keeps the question only | `test_registry.py`, `test_api_run.py` |
| **API keys leak** | Keys live only in `.env` (gitignored, never copied). No key is in code, tests, docs or a `NEXT_PUBLIC_*` variable, so none reaches the browser | Reviewed; secret scan before each commit |
| **Free-tier rate limits and outages** | Backup models, hedged requests when one is silent, a 10-minute cooldown for failing models, handling of quota (429), too-large (413), out-of-credit (402) and bad-JSON replies, and a cap of 40 AI calls per run | `test_llm.py` |
| **A pause is never answered** | Questions time out after 15 minutes and the run fails cleanly; after a server restart only plan approvals are resumed | `test_registry.py` |
| **Wrong delivery address** | The ZIP is compared with Instacart's delivery address, never used to change it; a mismatch produces a warning that stores and prices are for the other address | `test_shopper.py` |

## Privacy

- **Data stored** (in a local SQLite file, gitignored): the request (targets, preferences, allergies, ZIP,
  pantry), the plan, the grocery list, the cart report, AI call metadata (model, tokens, latency) and prices
  seen. No name, email or payment data is stored.
- **Sent to model providers:** the request and, for shopping, grocery item names and public product names.
  Free tiers may use submitted data to improve their products, so **do not enter real health conditions or
  personal details**. Targets, foods and a ZIP are all the app needs.
- **Instacart session:** kept in the app's own Chrome profile folder (`profiles/`, gitignored). Deleting the
  folder signs out.
- **Single user, local only:** the server accepts browser requests only from `localhost:3000`
  (CORS), and has no accounts. Exposing it to a network would need authentication first.

## Failure fallbacks

| Failure | What the user sees |
|---|---|
| Every model fails | The run stops with a plain message ("AI quota used up, try again later"), not a stack trace |
| A day still fails the checks after 3 repair rounds | The run stops with the failing checks listed; no plan is offered for approval |
| An item is not at any store | Listed as unavailable with substitutes; never silently dropped |
| A click on Instacart fails | The item becomes "add it yourself in Instacart"; the run continues |
| Chrome needs a login or CAPTCHA | The run pauses and asks; it never guesses |

## Known limits

- Product-level allergen and cross-contact data is **not** checked: allergen safety covers the planned
  ingredients, not the exact brand bought. The user must read labels before checking out.
- Prompt-injection defences reduce, but do not eliminate, the chance that a manipulated product name steers a pick.
  The impact is bounded to choosing among listed products.
- Free models can still be slow or unavailable; the app degrades by switching models, not by guaranteeing latency.
