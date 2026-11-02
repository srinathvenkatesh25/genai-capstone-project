# Install and run MealCart (Prompt-to-Plate working implementation)

MealCart plans a week of meals to your calorie and protein targets, checks every day against USDA
nutrition data, and fills your Instacart cart. **It never checks out.** You do that yourself.
It runs on your own Mac for one user.

## What you need

- macOS with **Google Chrome** installed (the app drives your real Chrome)
- **Python 3.11** and **Node 20+**
- An Instacart account **with no saved payment card**
- Free API keys:
  - **Gemini** (plans the meals): https://aistudio.google.com/apikey
  - **USDA FoodData Central** (nutrition data): https://api.data.gov/signup/
  - **Groq** (optional backup AI, also used for small tasks): https://console.groq.com
  - Optional: **Cerebras** https://cloud.cerebras.ai, **DeepSeek** https://platform.deepseek.com

## Setup (once), from the repository root

```bash
cd app
python3.11 -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -r requirements.txt
cp .env.example .env              # first time only; never overwrite an existing .env
(cd frontend && npm ci)
```

Open `app/.env` and fill in `LLM_API_KEY` (your Gemini key), `USDA_API_KEY` and, optionally, `GROQ_API_KEY`.
Keys stay in `.env`, which git ignores. Never put a key anywhere else.

Sign in to Instacart once, in the app's own Chrome profile (stored in `app/profiles/`, git-ignored):

```bash
python scripts/login.py           # asks for your email, then the code Instacart sends
python scripts/login.py --check   # later: is the saved session still valid?
```

## Run

Two terminals, both starting in `app/`. Don't add `--reload` to uvicorn: it would kill a run mid-shop.

```bash
# terminal 1: the server
source .venv/bin/activate
uvicorn app.main:app --app-dir backend

# terminal 2: the page
cd frontend && npm run dev
```

Open http://localhost:3000. After changing backend code, restart terminal 1 (when no run is in progress).

## Try it (about 5 minutes)

1. Check the targets (defaults: 2,200 kcal, 140 g protein) and click **Plan my week**.
2. Open **Activity** (right edge) to watch the AI plan, software size portions, and the checks run.
3. On **Meals**, click any meal to see its calories, protein, ingredients and steps. Try **Swap this meal**.
4. **Confirm meals**, untick anything you already have, then **Confirm list and fill my Instacart cart**.
5. Watch the progress card, then review the cart (needed vs in cart, price, per-store subtotals) and check out in Instacart yourself.

## Tests

From the repository root (they use a fake AI, fake store and offline USDA table; no keys or network needed):

```bash
pip install -r app/requirements.txt
pytest -q --ignore=tests/test_session.py   # about 20 s
pytest -q                                  # adds real headless-Chrome tests, about 75 s
```

Frontend checks: `cd app/frontend && npx next typegen && npx tsc --noEmit && npx eslint .` (Next generates some types, so run `typegen` once first).

## Configuration

Everything is in `app/.env` (see `app/.env.example`). Model order, timeouts and prompts are documented in
[`prompts/`](prompts/README.md). `BROWSER_MODE` is `auto` (hidden, a window opens only to sign in),
`hidden` or `visible`.

## Troubleshooting

| What you see | What to do |
|---|---|
| "Can't reach the MealCart server" | Terminal 1 isn't running. Start it again. |
| A backend fix doesn't apply | The server doesn't reload itself. Restart terminal 1. |
| "quota used up. Switching to ..." | Normal. Free tiers cap each model; the run continues on the next one. |
| "You're not signed in to Instacart" | Run `python scripts/login.py` again. |
| "Instacart asked for a human check" | Set `BROWSER_MODE=visible`, restart the server and solve the CAPTCHA in the window. |
| "Another run is already using the Instacart browser" | Wait for it to finish, or close its Chrome window. |
