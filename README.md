# Prompt-to-Plate: An Agentic AI Platform for Instant Meal Planning and Grocery Automation

> Status: 🚧 Checkpoint 1 — Project Kickoff, Literature Review & Proposal

Prompt-to-Plate helps users turn their dietary goals, preferences, pantry contents, and constraints into practical meal plans, validated grocery carts, and adaptive coaching.

## Team Members & Roles

| Name | Research Topics and Responsibilities | Contact |
|---|---|---|
| Srinath Venkatesh | Researched AI-agent purchasing and shopping evaluation, including agentic e-commerce risks, product retrieval, safety compliance, position bias, and seller influence. Set up the GitHub link, led product ideation, and sourced literature for the team. | TBD |
| Chantrice Santiago | Researched LLM-based multi-agent systems and collaborative multi-constraint planning, including agent roles, task decomposition, supervision, communication, and reliability. Created the proposal document, populated GitHub file contents, and contributed to product ideation. | TBD |
| Kanishka Gupta | Researched personalized nutrition coaching and food ingredient substitution, including behavioral barriers, agentic coaching workflows, allergy-aware substitutions, transparency, and safety. Evaluated the project's technical feasibility and co-led the technical assessment during product ideation. | TBD |
| Sneha Vyas | Researched generative meal planning and nutrition recommendation, including NutriGen, validated nutrition data, generative models, optimization, and ChatGPT-supported recommendations. Contributed to product ideation and created the PowerPoint presentation slides. | TBD |

## Problem Statement & Motivation

Healthy eating requires users to repeatedly balance nutrition goals, preferences, allergies, budget, cooking ability, preparation time, ingredient availability, and physical activity. They must then turn those decisions into recipes, grocery purchases, portion sizes, and daily tracking. Existing tools typically focus on only one part of this process, forcing users to coordinate calorie trackers, recipe platforms, and grocery services. This creates decision fatigue and makes healthy changes difficult to sustain, especially for users with limited time or established eating habits.

Prompt-to-Plate addresses this gap by adapting to each user's current lifestyle and supporting realistic, incremental changes. The system requires an AI-native approach because users need to express changing goals in natural language, reconcile multiple constraints, use ingredients already available, prepare shopping-related actions, and learn from feedback over time. A fixed recommendation engine cannot easily respond to requests such as "make next week cheaper" or "use the chicken already in my refrigerator."

## Target Users & Core Tasks

**Primary user persona(s):**
- Busy adults and students who want practical meal plans but have limited time for planning, cooking, and grocery shopping.
- People who struggle to sustain diets that are drastically different from their existing lifestyles and need gradual, realistic changes that fit their routines, preferences, and constraints.

**Top 2–3 tasks they will accomplish with this system:**
1. Create and conversationally revise a weekly meal plan based on nutrition goals, preferences, allergies, schedule, cooking ability, budget, and available ingredients.
2. Optimize the plan for calories, macronutrients, cost, preparation time, ingredient reuse, package sizes, and estimated food waste.
3. Review and approve a grocery cart, then record adherence and feedback so the system can work around the user's lifestyle and gradually help them sustain healthy eating habits.

## Competitive Landscape

| Existing System/Tool | What it does | Shortcoming(s) |
|---|---|---|
| Calorie and nutrition trackers | Record meals, calories, and nutrition information | Usually require manual entry and do not create pantry-aware meal plans or grocery carts. |
| Recipe and meal-planning platforms | Recommend recipes and organize meals | Often provide limited support for simultaneous constraints such as allergies, budget, package sizes, preparation time, and ingredient reuse. |
| Grocery delivery and shopping services | Retrieve products and support online grocery purchasing | Do not usually connect product selection to validated nutrition planning, adherence tracking, or adaptive coaching. |

## Initial Concept & Value Proposition

Prompt-to-Plate uses an orchestrated multi-agent architecture to integrate the full cycle of Profile, Plan, Optimize, Shop, Eat and Track, Learn, and Adapt. A Profile Agent structures user goals, preferences, allergies, schedules, cooking ability, and pantry contents. A Meal-Planning Agent generates candidate meals grounded in verified nutrition and recipe databases. An Optimization Agent applies deterministic checks for nutrition, cost, preparation time, ingredient reuse, package sizes, and food waste. A Shopping Agent retrieves eligible products and prepares a cart, while a Coaching Agent uses adherence and feedback to recommend gradual changes that work around the user's lifestyle and help sustain healthy eating habits.

GenAI provides the natural-language interpretation, personalization, planning, and conversational revision that a fixed database cannot provide. Retrieval and trusted databases ground nutrition and product information, while deterministic validation checks constrain the generated results. Users retain control by reviewing the plan and explicitly approving the grocery cart before checkout. This approach connects meal planning, shopping, tracking, and adaptation in one workflow instead of making users coordinate separate tools.

## Milestones Roadmap

| Checkpoint | Deliverable | Target Date |
|---|---|---|
| Checkpoint 1 | Kickoff, literature review, proposal | TBD |
| Checkpoint 2 | Validate the profile-to-plan-to-cart prompt chain with synthetic profiles, deterministic nutrition checks, shopping safety cases, repeated runs, and a small user evaluation | TBD |
| Checkpoint 3 | Functional prototype with the core multi-agent workflow, conversational plan revision, validation checkpoints, and grocery-cart approval | TBD |
| Checkpoint 4 | Final system evaluation, documentation, presentation, and demonstration | TBD |

## Repository Structure

```
├── README.md               # This file — high-level project landing page
├── /literature/            # Research papers & citation bibliography
│   ├── BIBLIOGRAPHY.md     # Assigned papers and APA citations
│   ├── references.bib      # BibTeX entries for the literature
│   └── /literature-review-resources/ # Literature PDFs
├── /reflections/           # Individual reflections (1 file per student)
├── /proposal/              # Formal project proposal document
└── (GitHub Projects & Issues) # Task assignment & milestone tracking
```

See [literature/BIBLIOGRAPHY.md](literature/BIBLIOGRAPHY.md), [reflections/](reflections/), and [proposal/PROPOSAL.md](proposal/PROPOSAL.md).
