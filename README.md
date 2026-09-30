# Prompt-to-Plate: An Agentic AI Platform for Instant Meal Planning and Grocery Automation

> **Status:** 🚧 Checkpoint 2 — Prompt-Based Validation & Concept Design

Prompt-to-Plate helps users turn dietary goals, preferences, pantry contents, budget, schedule, and other real-world constraints into practical weekly meal plans, validated grocery carts, and adaptive coaching.

Rather than functioning as a one-time meal generator, Prompt-to-Plate explores an adaptive workflow:

**Profile → Plan → Optimize → Shop → Eat & Track → Learn → Adapt**

---

## Team Members & Roles

| Name | Research Topics and Responsibilities | Contact |
|---|---|---|
| Srinath Venkatesh | Researched AI-agent purchasing and shopping evaluation, including agentic e-commerce risks, product retrieval, safety compliance, position bias, and seller influence. Set up the GitHub repository, led product ideation, and sourced literature for the team. | TBD |
| Chantrice Santiago | Researched LLM-based multi-agent systems and collaborative multi-constraint planning, including agent roles, task decomposition, supervision, communication, and reliability. Created the proposal document, populated GitHub file contents, and contributed to product ideation. | TBD |
| Kanishka Gupta | Researched personalized nutrition coaching and food ingredient substitution, including behavioral barriers, agentic coaching workflows, allergy-aware substitutions, transparency, and safety. Evaluated the project's technical feasibility and co-led the technical assessment during product ideation. | TBD |
| Sneha Vyas | Researched generative meal planning and nutrition recommendation, including NutriGen, validated nutrition data, generative models, optimization, and ChatGPT-supported recommendations. Contributed to product ideation and created the PowerPoint presentation slides. | TBD |

---

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

Checkpoint 2 moves Prompt-to-Plate from an initial concept toward **evidence-based validation and design refinement**.

The goal is not to assume that the proposed architecture works. Instead, the team will test realistic scenarios, document failures, interpret those failures through a human–AI teaming and complementarity lens, and use that evidence to refine the design.

The validation process follows:

**Receipt → Theory → Design**

### Receipt
Collect evidence from prompting experiments, AI-tool outputs, and user interviews or concept evaluation.

### Theory
Interpret observed failures using the required human–AI teaming and complementarity framework, including concepts such as reasoning, memory, attention, meta-coordination, and role partitioning.

### Design
Translate the evidence and theoretical interpretation into concrete requirements, interaction changes, validation mechanisms, and prototype decisions.

---

## Checkpoint 2 Validation Focus

Prompt-to-Plate will be evaluated across several core capabilities:

### 1. Constraint Understanding
Can the system correctly interpret and maintain dietary restrictions, allergies, budget, schedule, cooking ability, preferences, and pantry context?

### 2. Multi-Constraint Meal Planning
Can the system generate practical meal plans while satisfying multiple simultaneous constraints?

### 3. Grounding & Validation
Where can GenAI reason effectively, and where are trusted data sources or deterministic checks required?

### 4. Pantry & Grocery Reasoning
Can the system distinguish between existing and missing ingredients and appropriately prepare grocery recommendations?

### 5. Human–AI Coordination
Which decisions can be delegated to AI, and where should clarification, review, or explicit human approval be required?

### 6. Behavioral Adaptation
Can user adherence and feedback meaningfully inform future planning rather than assuming perfect compliance?

### 7. Failure & Edge-Case Handling
How does the system respond to conflicting constraints, missing information, unsafe recommendations, unavailable products, or ambiguous user requests?

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
