# Prompt-to-Plate: An Agentic AI Platform for Instant Meal Planning and Grocery Automation


Prompt-to-Plate helps users turn dietary goals, preferences, pantry contents, budget, schedule, and other real-world constraints into practical weekly meal plans, validated grocery carts, and adaptive coaching.

Rather than functioning as a one-time meal generator, Prompt-to-Plate explores an adaptive workflow:

**Profile → Plan → Optimize → Shop → Eat & Track → Learn → Adapt**

---

## Team Members & Roles

| Name | Research Topics and Responsibilities | Contact |
|---|---|---|
| Srinath Venkatesh | <ul><li><strong>Checkpoint 1:</strong> Researched AI-agent purchasing and shopping evaluation, set up the GitHub repository, led product ideation, and sourced literature.</li><li><strong>Checkpoint 2:</strong> Built the interactive proof-of-concept and wrote the detailed design specification. Contributed to the prompting protocol, supported the prompting study and receipt collection, and supported the speed-dating interviews.</li></ul> | TBD |
| Chantrice Santiago | <ul><li><strong>Checkpoint 1:</strong> Researched LLM-based multi-agent systems and collaborative multi-constraint planning, created the proposal, populated repository content, and contributed to product ideation.</li><li><strong>Checkpoint 2:</strong> Contributed to the prompting protocol, ran the prompting study and saved receipts, wrote the shared theoretical discussion, and supported the speed-dating interviews.</li></ul> | TBD |
| Kanishka Gupta | <ul><li><strong>Checkpoint 1:</strong> Researched personalized nutrition coaching and ingredient substitution, evaluated technical feasibility, and co-led the technical assessment.</li><li><strong>Checkpoint 2:</strong> Read Gonzalez et al. (2026) and drafted the theory claim, then led evidence-to-theory feature prioritization after the team compared interview results. Documented hypothesis shifts, created a prioritized feature matrix with empirical and theory-linked justification for every feature, and supported the speed-dating interviews.</li></ul> | TBD |
| Sneha Vyas | <ul><li><strong>Checkpoint 1:</strong> Researched generative meal planning and nutrition recommendation, contributed to product ideation, and created the presentation slides.</li><li><strong>Checkpoint 2:</strong> Led the speed-dating interviews, built the gap-analysis matrix, and supported the speed-dating interviews.</li></ul> | TBD |

---

# Checkpoint 1 — Project Kickoff, Literature Review & Proposal

## Problem Statement & Motivation

Healthy eating requires users to repeatedly balance nutrition goals, preferences, allergies, budget, cooking ability, preparation time, ingredient availability, and physical activity. They must then turn those decisions into recipes, grocery purchases, portion sizes, and daily tracking.

Existing tools typically focus on only one part of this process, forcing users to coordinate calorie trackers, recipe platforms, and grocery services. This creates decision fatigue and makes healthy changes difficult to sustain, especially for users with limited time or established eating habits.

Prompt-to-Plate addresses this gap by adapting to each user's current lifestyle and supporting realistic, incremental changes. The system uses GenAI where natural-language understanding, personalization, planning, and conversational revision are valuable, while relying on grounded data and deterministic validation for constraints that require greater reliability.

---

## Target Users & Core Tasks

### Primary User Personas

- Busy adults and students who want practical meal plans but have limited time for planning, cooking, and grocery shopping.
- People who struggle to sustain diets that are drastically different from their existing lifestyles and need gradual, realistic changes that fit their routines, preferences, and constraints.

### Core Tasks

1. **Plan** — Create and conversationally revise a weekly meal plan based on nutrition goals, preferences, allergies, schedule, cooking ability, budget, and available ingredients.
2. **Optimize** — Evaluate the plan for nutrition, cost, preparation time, ingredient reuse, package sizes, and estimated food waste.
3. **Shop & Adapt** — Review and approve a grocery cart, then record adherence and feedback so future plans can adapt to the user's actual behavior.

---

## Competitive Landscape

| Existing System/Tool | What it does | Shortcoming(s) |
|---|---|---|
| Calorie and nutrition trackers | Record meals, calories, and nutrition information | Usually require manual entry and do not create pantry-aware meal plans or grocery carts. |
| Recipe and meal-planning platforms | Recommend recipes and organize meals | Often provide limited support for simultaneous constraints such as allergies, budget, package sizes, preparation time, and ingredient reuse. |
| Grocery delivery and shopping services | Retrieve products and support online grocery purchasing | Do not usually connect product selection to validated nutrition planning, adherence tracking, or adaptive coaching. |

Prompt-to-Plate explores whether these currently separated activities can be connected into a single adaptive workflow rather than requiring the user to coordinate multiple tools manually.

---

## System Concept

Prompt-to-Plate uses an orchestrated multi-agent architecture to support the full cycle:

**Profile → Plan → Optimize → Shop → Eat & Track → Learn → Adapt**

### Profile Agent
Structures user goals, dietary preferences, allergies, schedules, cooking ability, budget, and pantry contents.

### Meal-Planning Agent
Generates candidate meals based on the user's structured constraints and grounded recipe and nutrition information.

### Optimization & Validation Layer
Evaluates measurable constraints such as nutrition, cost, preparation time, ingredient reuse, package sizes, and estimated food waste using grounded data and deterministic checks where appropriate.

### Shopping Agent
Identifies missing ingredients, retrieves eligible products, and prepares a proposed grocery cart.

### Human-in-the-Loop
The user reviews and modifies recommendations and explicitly approves the grocery cart before any purchasing action.

### Coaching / Adaptation Agent
Uses adherence and feedback — such as meals eaten, skipped, substituted, or disliked — to inform subsequent planning cycles.

---

## GenAI + Deterministic Design

Prompt-to-Plate does **not** assume that an LLM should perform every task.

### GenAI Responsibilities

- Natural-language understanding
- Constraint interpretation
- Personalized generation
- Meal-plan reasoning
- Conversational revision
- Contextual adaptation

### Grounded / Deterministic Responsibilities

- Nutrition validation
- Dietary and allergy constraint checks
- Budget and cost validation
- Pantry and ingredient calculations
- Ingredient reuse
- Package-size reasoning
- Other measurable optimization constraints

### Human Responsibilities

- Clarifying ambiguous information
- Reviewing recommendations
- Modifying proposed plans and carts
- Approving grocery actions
- Providing adherence and preference feedback

The intended design therefore combines **AI reasoning, grounded computation, and human control** rather than relying on unrestricted LLM generation.

---

# Checkpoint 2 — Prompt-Based Validation & Concept Design

Checkpoint 2 moves Prompt-to-Plate from an initial concept toward **evidence-based validation, design refinement, and an interactive proof-of-concept**. The team completed a theory-tagged prompting protocol with typical, edge, and failure scenarios; tested Claude, Copilot, and Gemini; and saved both raw outputs and structured evaluations.

The study found that the models generally preserved allergy safeguards, disclosed budget conflicts, and left checkout to the user, but they were inconsistent at nutrition arithmetic, simultaneous constraint satisfaction, ingredient-to-cart coverage, feasibility judgments, and handling missing product data. These findings support the prototype's division of work: GenAI proposes and explains, deterministic software validates measurable constraints, and people approve plans, substitutions, and shopping actions.

The validation process follows:

**Receipt → Theory → Design**

### Receipt
Prompting receipts and scored evaluations are stored in `validation/transcripts/`; speed-dating interviews add user evidence about practicality, trust, and control.

### Theory
Observed failures are interpreted through human–AI complementarity, especially role partitioning, verification, attention, and meta-coordination.

### Design
Evidence is translated into prioritized requirements, deterministic validation, visible uncertainty, targeted revision, and explicit human approval before shopping or checkout.

---

## Checkpoint 2 Validation Focus

Current evidence and continued prototype evaluation focus on these capabilities:

### 1. Constraint Understanding
Models usually recognized explicit safety constraints, but performance weakened as nutrition, budget, variety, preparation time, and inventory requirements accumulated.

### 2. Multi-Constraint Meal Planning
Generated plans sometimes contradicted their own feasibility claims or missed nutrition and repetition limits, showing the need for independent checks.

### 3. Grounding & Validation
Nutrition totals, budget arithmetic, product availability, and ingredient coverage require grounded data and deterministic validation rather than model self-report.

### 4. Pantry & Grocery Reasoning
Tests exposed missing ingredients, unverifiable serving quantities, and cart totals that did not consistently match itemized products.

### 5. Human–AI Coordination
Models generally respected the no-checkout boundary; substitutions, ambiguous allergen data, budget changes, and final carts still require explicit human review.

### 6. Behavioral Adaptation
Interview findings are being used to prioritize practical revision controls and lifestyle fit; long-term adherence learning remains outside the current prototype.

### 7. Failure & Edge-Case Handling
Safety refusals were often strong, but uncertainty handling varied: some outputs refused unsupported products while others invented missing product details or produced internally inconsistent calculations.

---

## Checkpoint 2 Deliverables

| Deliverable | Purpose |
|---|---|
| `validation/PROMPTING_PROTOCOL.md` | Theory-tagged test scenarios, prompts, and evaluation criteria |
| `validation/transcripts/` | Sanitized outputs and prompting receipts from tested AI tools |
| `validation/GAP_ANALYSIS.md` | Empirical failures, theoretical interpretations, and resulting design implications |
| `validation/THEORY_LENS.md` | Shared human–AI complementarity discussion |
| `validation/OPPORTUNITY_FRAMING.md` | Evidence-based and prioritized design requirements |
| `DESIGN_SPEC.md` | Updated user journeys, system flows, and interaction specifications |
| `prototype/` | Interactive clickthrough or sandbox prototype |
| `validation/reflections/` | Individual validation reflections and required storyboard material |

---

## Milestones Roadmap

| Checkpoint | Deliverable | Status |
|---|---|---|
| Checkpoint 1 | Project kickoff, literature review, and proposal | ✅ Completed |
| Checkpoint 2 | Prompt-based validation, theoretical analysis, concept refinement, design specification, and prototype | 🚧 In Progress |
| Checkpoint 3 | Functional prototype with refined agentic workflow and validation mechanisms | ⏳ Upcoming |
| Checkpoint 4 | Final system evaluation, documentation, presentation, and demonstration | ⏳ Upcoming |

---

## Repository Structure

```text
├── README.md
│
├── literature/
│   ├── BIBLIOGRAPHY.md
│   ├── references.bib
│   └── literature-review-resources/
│
├── proposal/
│   └── PROPOSAL.md
│
├── reflections/
│   └── individual Checkpoint 1 reflections
│
├── storyboard/
│   └── user journey and concept storyboard materials
│
├── validation/
│   ├── PROMPTING_PROTOCOL.md
│   │
│   ├── transcripts/
│   │   ├── tool1_outputs.md
│   │   └── tool2_outputs.md
│   │
│   ├── reflections/
│   │   └── individual Checkpoint 2 validation reflections
│   │
│   ├── GAP_ANALYSIS.md
│   ├── THEORY_LENS.md
│   └── OPPORTUNITY_FRAMING.md
│
├── DESIGN_SPEC.md
│
└── prototype/
    └── Checkpoint 2 prototype materials
