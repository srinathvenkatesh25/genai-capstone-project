# Prompt-to-Plate: An AI-Assisted Platform for Meal Planning and Guarded Grocery-Cart Automation

*Chantrice Santiago, Kanishka Gupta, Sneha Vyas, Srinath Venkatesh*

## 1. Problem Significance

Healthy eating requires more than knowing which foods are nutritious. Users must repeatedly balance nutrition goals, preferences, allergies, budget, cooking ability, preparation time, equipment, and ingredient availability. They must then translate those decisions into meals, portion sizes, a grocery list, and suitable products. Existing applications commonly separate calorie tracking, recipes, and grocery shopping, leaving users to coordinate the workflow and repeat decisions every week.

Prompt-to-Plate reduces that burden by turning user-supplied nutrition targets and practical constraints into a checked meal plan and a reviewable grocery cart. Rather than prescribing a highly restrictive or unfamiliar diet, the system emphasizes meals that fit the user's cuisine preferences, schedule, cooking ability, pantry, and willingness to prepare food. The aim is sustainable adherence through a workable plan, not automated medical advice.

The project is AI-native where interpretation and generation are valuable, but the implemented prototype does not delegate all decisions to an LLM. AI interprets free-text requests, proposes meals, repairs failed plans, swaps disliked meals, and helps choose among retrieved grocery products. Deterministic software performs nutrition arithmetic, portion sizing, constraint validation, grocery consolidation, package calculations, browser actions, and cart verification. The user controls approval and checkout.

## 2. Prior Work and Gaps

Prior research demonstrates the feasibility of AI-supported nutrition planning but does not provide a fully reliable end-to-end food-management workflow. NutriGen combines LLMs with USDA nutrition data and produced meal plans close to calorie targets, showing the value of grounding generated plans in validated databases (Khamesian et al., 2025). Papastratis et al. (2024) combined a variational autoencoder, optimization logic, and ChatGPT to generate nutritionally appropriate weekly plans across simulated and real profiles. These systems support the feasibility of generation plus computation but do not address the complete transition from user constraints through pantry-aware grocery consolidation to a verified retail cart.

Ingredient-substitution research supports adapting recipes for allergies, nutrition goals, and availability, but methods vary in transparency, contextual awareness, and safety (Kim et al., 2025). Yang et al. (2025) showed that a two-agent nutrition-coaching workflow could identify behavioral barriers and offer personalized tactics, but coaching is distinct from operational planning and shopping.

Research on multi-agent and decomposed planning suggests that complex constraint problems benefit from specialized roles, structured state, and verification (Chen et al., 2024; Zhang et al., 2024). Shopping is especially risky: product retrieval, safety compliance, unstable preferences, seller influence, and position bias make unrestricted purchasing agents inappropriate (Tou et al., 2025; Allouah et al., 2025). The implemented prototype responds by separating generative decisions from deterministic checks, constraining browser automation, verifying the resulting cart, and requiring human review before checkout.

## 3. Implemented Technical Approach


### 3.1 Intake

The user enters a structured specification or a natural-language description. Inputs include one to seven days, daily calorie and protein targets, cuisines, weekly budget, US ZIP code, snacks, allergies, dietary restrictions, dislikes, preferred protein, maximum preparation time, effort, cooking skill, equipment, and pantry ingredients.

Intake logic resolves or flags contradictions. For example, if a user selects vegetarian and supplies chicken as the main protein, the incompatible protein preference is ignored and the user sees a notice. The system accepts nutrition targets but does not calculate or prescribe them.

### 3.2 AI Meal Planning

The planner uses an LLM to propose meals, preparation steps, equipment, cooking times, and ingredient quantities expressed as raw or dry grams. A full week is normally generated in one call to reduce quota use. Gemini is the default provider, with fallbacks across models and an optional Groq provider.

The model is not trusted to perform final nutrition arithmetic. AI output becomes a candidate plan that must pass deterministic processing.

### 3.3 Portion Solving and Nutrition Grounding

The application retrieves nutrient values from USDA FoodData Central and caches them in SQLite. Hand-checked aliases and result-ranking rules reduce errors caused by misleading database matches or raw-versus-cooked ambiguity.

A SciPy bounded least-squares solver adjusts portions to meet daily calorie and protein targets. Calories and protein are enforced; carbohydrates and fat are computed and displayed but are not target constraints.

### 3.4 Deterministic Validation and Repair

Python validators check daily nutrition, allergies, dietary restrictions, dislikes, preparation time, realistic minimum cooking times, permitted equipment, expected meal slots, and variety. A failed check becomes a specific repair hint sent back to the LLM. The graph permits up to three repair rounds before ending with a visible failure.

This division reflects a central design principle: prompts express intent, while software verifies measurable requirements.

### 3.5 Human Review and Revision

When the plan passes validation, the system presents the weekly plan and consolidated grocery list before any retailer interaction. The user can approve, cancel, edit grocery items, remove items already available, or swap one meal. A meal swap changes only the selected meal, then re-runs portion solving and validation for that day and rebuilds the grocery list.

### 3.6 Grocery Consolidation

The consolidator combines duplicate ingredients across meals, subtracts manually entered pantry items, determines purchase quantities from required grams and package-size rules, and attaches predefined substitutes. Estimated prices are drawn from prior cart observations when available.

### 3.7 Instacart Automation and Verification

After explicit approval, Playwright drives a persistent Google Chrome profile signed into Instacart. Fixed browser steps perform navigation and cart actions; an LLM is used only to choose among products scraped from retailer search results. The system checks the saved delivery address against the submitted ZIP code, probes stores for difficult items, uses up to three stores when necessary, and pauses for human sign-in, CAPTCHA resolution, substitutions, or permission to clear an existing cart.

After adding products, the system reads each cart and compares its contents with the grocery list. The final report identifies selected products, quantities, substitutions, coverage, store subtotals, and budget variance.

The agent cannot purchase groceries. No checkout code path exists, a browser request guard blocks checkout/order/payment traffic, and users are instructed to use an account without a saved payment card. The user must review labels, allergen information, quantities, substitutions, fees, and total cost, then complete checkout manually.

### 3.8 Implementation Stack

| Layer | Implementation |
|---|---|
| User interface | Next.js single-page application |
| API | FastAPI and WebSockets |
| Workflow orchestration | LangGraph with SQLite checkpointing |
| AI | Gemini by default; model fallbacks and optional Groq |
| Nutrition source | USDA FoodData Central |
| Portion optimization | SciPy bounded least squares |
| Persistence | SQLite |
| Retail automation | Playwright with persistent Google Chrome profile |
| Safety | Human approval, browser request guard, no checkout path, and cart read-back verification |

### 3.9 Scope, Limitations, and Success Criteria

The implemented prototype focuses on user-supplied calorie and protein targets, one-to-seven-day meal planning, deterministic nutrition and constraint validation, targeted meal swaps, pantry-aware grocery consolidation, and human-approved Instacart cart filling and verification. It does not calculate nutrition goals, optimize carbohydrate or fat targets, track activity or adherence, provide behavioral coaching, recognize pantry images, maintain quantity-aware inventory, support unrestricted conversational editing, use MCP or an official retailer API, complete checkout, or support multiple users and remote access. It is also limited by its Mac and Chrome requirements, local deployment, free-model quotas, changing retailer inventory and interfaces, heuristic store selection, and incomplete package or price information.

The prototype will be considered successful if a user can enter known nutrition goals and practical constraints, receive a plan that passes the implemented checks, revise it through targeted swaps, convert it into a pantry-aware grocery list, and obtain a verified Instacart cart while retaining control over substitutions, allergen review, cost, payment, and checkout. Success also depends on whether users find the meals practical and the workflow reduces planning effort; the prototype does not claim to guarantee adherence, clinical suitability, exact final cost, or product-level allergen safety.

## 4. Validation Plan

### 4.1 Checkpoint Validation Plan

Checkpoint validation will test the complete profile-to-plan-to-cart workflow with synthetic user scenarios that vary nutrition targets, cuisines, budgets, allergies, dietary restrictions, cooking time, equipment, and pantry contents. Results will be assessed for calorie and protein accuracy, constraint satisfaction, ingredient reuse, grocery-list correctness, cart coverage, substitution quality, cost variance, and the number of repairs or human interventions required.

The team will combine automated tests with manual end-to-end runs against the live AI, USDA, and Instacart services. User-oriented evaluation will examine whether the plan is practical and understandable, whether the approval and revision controls support appropriate human oversight, and whether the workflow reduces perceived planning effort while preserving trust and control.

### 4.2 Checkpoint 2 Prototype Validation Workflow

The implementation repository reports 218 automated tests. Coverage includes nutrition validation, portion solving, USDA result ranking and caching, allergen and restriction maps, cooking-time floors, grocery consolidation, package-size parsing, substitutions, run persistence, approval and swap behavior, WebSocket/API flows, browser sessions, and the checkout guard.

Live checks include seven-day plans produced with Gemini and Groq that passed the implemented validators, USDA lookups for common staples, a five-item Instacart cart filled and verified, idempotent re-runs that avoided duplicate additions, and checkout traffic blocked in a real browser session.

The prototype still requires manual end-to-end evaluation. A full live plan-to-cart run depends on external model quotas, a valid Instacart session, current inventory and prices, store availability, and the retailer's current interface. Evaluation should therefore measure both plan quality and operational completion, including:

- success in meeting calorie and protein tolerances;
- satisfaction of allergies, restrictions, dislikes, time, and equipment constraints;
- pantry removal and ingredient consolidation accuracy;
- number and quality of repair rounds or meal swaps;
- cart coverage, substitutions, and duplicate prevention;
- budget variance before taxes and fees;
- frequency of human interventions; and
- perceived practicality, clarity, trust, and reduction in planning effort.

## 5. Risk Analysis and Mitigation

- **Nutrition and medical risk:** The application does not calculate targets, diagnose conditions, or replace a dietitian. Users provide their targets. USDA grounding and deterministic checks reduce arithmetic errors, but users with medical nutrition needs require professional review.
- **Allergy risk:** Ingredient-name rules can catch known conflicts in generated meals, but retailer listings and substitutions may be incomplete or ambiguous. Product labels and allergen information require human verification before purchase or consumption.
- **Accidental purchase:** The prototype has no checkout implementation, blocks checkout/order/payment requests, stops at cart review, and recommends removing saved payment methods.
- **Automation and retailer risk:** Instacart interface changes, automation detection, CAPTCHAs, sign-in expiry, or terms may interrupt the workflow. The prototype uses one personal session at human speed, never solves CAPTCHAs, and exposes failures rather than bypassing them.
- **Privacy and security:** A local deployment limits exposure, but the system processes diet preferences, a ZIP code, pantry information, and a logged-in retail session. Secrets are held in memory during pauses rather than written to the database or checkpoints. The current local server is intentionally not exposed to other devices.
- **Hallucination and retrieval errors:** LLM outputs are treated as proposals. USDA values, user constraints, cooking times, package quantities, and cart coverage are checked in code. Unknown or mismatched ingredients trigger repair rather than silent acceptance.
- **Bias and cultural fit:** Cuisine preferences are included to support familiar food patterns and avoid assuming a single ideal diet. User testing should still examine cultural coverage, socioeconomic constraints, body-related language, and the practicality of suggested foods.
- **Cost uncertainty:** Prices change, initial estimates may depend on historical carts, and reported subtotals exclude taxes, service fees, delivery fees, and tips. Multi-store fulfillment may further increase cost.
- **Model availability:** Free Gemini and Groq quotas can delay or stop a run. The interface reports model switches and quota failures, but availability is not guaranteed.
