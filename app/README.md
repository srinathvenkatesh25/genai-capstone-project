# app/: MealCart, the Checkpoint 3 working implementation

The running software for Prompt-to-Plate. Setup and run steps are in [INSTALL.md](INSTALL.md); the
prompts, model settings and reply schemas are in [prompts/](prompts/README.md); the design and safety
documents are in [../docs/](../docs/).

| Folder | What it holds |
|---|---|
| `backend/app/` | FastAPI server, LangGraph flow, planner and AI client, USDA nutrition checks, Instacart shopper |
| `frontend/` | Next.js page |
| `prompts/` | Every prompt, the model settings and the JSON schemas, generated from the code |
| `scripts/` | `login.py`, the one-time Instacart sign-in |
| `AGENTS.md` | Layout and rules for contributors (the root `tests/` folder is where its "tests/" refers) |

**Source:** a snapshot of https://github.com/srinathvenkatesh25/mealcart.ai at commit `dbc8215` (branch `v1`),
taken without secrets, databases or the browser profile. The rules in `AGENTS.md` apply, especially:
the app has no checkout path, and API keys stay in `.env`.
