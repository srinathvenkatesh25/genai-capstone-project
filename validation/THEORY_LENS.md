# Prompt-to-Plate: Human-AI Complementarity Framework
**Team:** Srinath Venkatesh, Chantrice Santiago, Kanishka Gupta, Sneha Vyas  
**Date:** October 5, 2026  
**Reference:** Gonzalez et al. (2026). Toward a Science of Human-AI Teaming for Decision Making

---

## Theory Claim

**Our hybrid should beat human-alone and AI-alone at identifying the healthiest next step a user can realistically sustain because humans own judgment about what they'll actually adhere to, and AI owns rapid multi-constraint reconciliation and nutrition validation.**

---

## Why This Matters

Prompt-to-Plate solves a multi-objective optimization problem under uncertainty: users must balance nutrition goals (calories, macros, fiber), practical constraints (budget, cooking time, allergies, pantry inventory), cultural preferences (cuisine, familiar foods), and sustainability (will they actually eat this?).

**Human-alone limitation:** Users lack tools to explore options across multiple competing constraints simultaneously. They resort to mental heuristics, repeated trial-and-error, or abandon healthy eating goals altogether because the cognitive load is too high.

**AI-alone limitation:** Large language models can generate plausible meal plans but cannot verify nutrition accuracy, may miss allergies, cannot judge adherence probability, and lack accountability for dietary decisions. Hallucinated nutrition values or unsafe substitutions create safety and trust failures.

**Hybrid advantage:** AI rapidly explores the feasible option space within human-defined constraints; humans retain control over what matters most and verify that recommendations are safe and sustainable. Neither party could achieve realistic, constraint-satisfying outcomes alone.

---

## Complementarity Conditions

For Prompt-to-Plate to demonstrate true complementarity, **all three of these must hold:**

### 1. **Constraint Satisfaction**
- AI generates meal plans that simultaneously satisfy: calorie targets ±5%, macro targets ±10%, budget limit, cooking time limit, allergy restrictions (hard constraint), cuisine preference, and ingredient reuse ≥60%.
- **Test:** Do AI outputs violate any user-specified constraint? (If yes, AI is not trustworthy for real-world use.)

### 2. **Adherence Feasibility**
- Humans review AI-generated plans and report: "I would actually eat this" or "This is realistic for my lifestyle."
- AI does not recommend meals that conflict with the user's stated preferences, cooking ability, or willingness to prepare novel foods.
- **Test:** Do users approve the plans? Do speed-dating participants report the meals seem sustainable?

### 3. **Verification & Safety**
- Nutrition values (calories, macros, fiber, sodium, allergen warnings) are verified against trusted databases (USDA FoodData Central) before presentation to users.
- Allergies are treated as hard constraints and flagged across all ingredients and packaged products.
- Uncertainty is exposed rather than hidden (e.g., "Nutrition estimate based on USDA data" vs. no attribution).
- **Test:** Do nutrition values match independent recalculation? Are allergen misses detected?

---

## Cognitive Pillars & Role Assignment

Mapped to Gonzalez et al.'s framework:

| **Cognitive Pillar** | **AI's Role** | **Human's Role** | **Why This Pairing Works** |
|---|---|---|---|
| **Reasoning** | Generate feasible options; flag constraint violations; check nutritional accuracy; surface trade-offs (e.g., "Can't meet all constraints—pick priority") | Define values & constraints; approve or reject options; judge feasibility ("I can realistically cook this"); sign off on dietary decisions | AI explores the decision space *within* human boundaries; humans hold ethical authority and make final judgments. |
| **Attention** | Triage recipes by constraint priority; flag anomalies (ingredient unavailable, allergy detected, budget overrun); surface critical information (allergens, nutrition uncertainty) | Redirect if AI prioritized wrong; decide what matters most this week ("Make it cheaper, not healthier"); notice if important information is missing | AI handles routine vigilance & prioritization; humans decide *what deserves attention* based on context. |
| **Memory** | Store recipes, nutrition data, user profiles, and pantry inventory; learn which meals users skip, replace, or complete; track adherence patterns | Validate data accuracy ("That's not my allergy profile"); provide context ("I hated that meal"); decide if strategy should change | AI learns from user behavior over time; humans confirm whether the learning is correct and actionable. |
| **Meta-Coordination** | Execute structured plan-generation, validation, and optimization logic; maintain audit trails for safety and compliance | Design the overall team workflow (when AI proposes vs. when human decides); manage escalation (unclear constraint → ask user for clarification); define approval gates | AI ensures procedural reliability and traceability; humans manage flexibility and handle exceptions. |

---

## Design Principles That Enable Complementarity

### For Reasoning
- **Goals & Constraints:** Require users to specify calorie/macro targets, allergies, budget, cooking time, and cuisine preference upfront. AI must validate that constraints are satisfiable; if not, ask humans to re-prioritize.
- **Knowledge Infrastructure:** Nutrition data must be sourced from USDA FoodData Central or equivalent. Recipes must include preparation time, ingredient counts, and verified macros. Provenance must be transparent.
- **Error Detection:** Implement deterministic checks for budget overage, allergy presence, prep-time violations, and calorie/macro mismatches. Flag uncertainty when data is estimated.

### For Attention
- **Attention & Interrogation Orchestration:** AI proposes 3–5 meal plans; human selects one, requests changes ("Make it easier"), or rejects all and re-prioritizes constraints. Never auto-finalize plans.
- **Escalation Protocols:** If constraints are contradictory (high-protein, low-budget, no-cook), AI flags the conflict and asks human to adjust.
- **Monitoring:** Track which meals users actually prepare vs. skip; flag systematic failures (e.g., "You've skipped 4 chicken dishes—shall we remove chicken?").

### For Memory
- **Transactive Memory:** System maintains "who knows what" (which recipes suit this user, which products are in their pantry, which meal swaps they prefer). Users can correct the system.
- **Continuous Learning:** After each week, update preference profile and meal recommendations based on adherence, user feedback, and substitutions made.
- **Auditability:** Store decision records so users and designers can trace why a meal was recommended or why a product was selected.

### For Meta-Coordination
- **Role Partitioning:** AI controls meal-generation logic and optimization; human controls approval gates and final purchasing authority.
- **Training & Evaluation:** Measure not just plan accuracy but user satisfaction, adherence likelihood, and safety (allergens caught, budget respected).

---

## What We're Testing (Checkpoints 2–3)

### Prompting Study (Checkpoint 2)
- **Typical scenarios:** Can AI generate constraint-satisfying plans? Do plans match verified nutrition data?
- **Edge scenarios:** What happens when constraints conflict? Does AI flag impossibilities?
- **Failure scenarios:** Can AI miss an allergy? Exceed a budget? Ignore a cooking-time limit? (These test whether the AI is trustworthy for real deployment.)

### Speed-Dating Interviews
- **Accuracy & hallucinations:** Do users trust the nutrition information?
- **Reliability & consistency:** Would users expect similar plans from identical inputs?
- **UX friction:** Which constraints are burdensome to enter? When should humans review/override AI?
- **Safety & guardrails:** Are users concerned about allergies, nutrition, or substitutions?
- **Adherence feasibility:** Would users actually prepare the recommended meals?

### Hypothesis
**If complementarity conditions hold, then:**
- AI-generated plans satisfy ≥95% of constraints.
- Users report ≥80% of recommended meals are realistic/sustainable.
- Nutrition values match independent verification ≥95%.
- Allergens are correctly identified 100% of the time.
- Users approve plans without modification ≥70% of the time (indicating good AI understanding of preferences).
- Users report time savings and reduced cognitive burden compared to manual meal planning.

**If complementarity breaks down, then:**
- Plans violate constraints (budget overage, allergy present, prep time infeasible).
- Nutrition values are hallucinated or incorrect.
- Users report low adherence ("I would never eat this").
- Allergens are missed.
- Users distrust the system and manually verify everything (negating AI benefit).

---

## Theoretical Grounding

This claim is grounded in Gonzalez et al.'s (2026) framework:

1. **Complementarity:** Our hybrid outperforms human-alone and AI-alone because each party's strengths offset the other's weaknesses. Humans provide contextual judgment and accountability; AI provides rapid optimization and verification.

2. **Cognitive Foundations:** Reasoning (multi-objective problem-solving), Attention (prioritization & monitoring), and Memory (learning from user behavior) are the pillars. Meal planning is fundamentally a reasoning task; attention ensures priorities are respected; memory enables improvement over time.

3. **Sociotechnical Factors:** Trust calibration (users must see why AI chose a plan), shared mental models (users must understand what AI can and cannot do), and role clarity (who decides what) are critical for success.

4. **Design Principles:** Our platform implements goal definition (users specify constraints), knowledge infrastructure (verified nutrition data), attention orchestration (AI proposes, human decides), role partitioning (AI optimizes, human approves), and training/evaluation (iterative improvement).

---

## Team Reflections

**What each team member brought to this claim:**

- **Srinath Venkatesh** (shopping & AI-agent purchasing): Emphasized that AI shopping agents fail when unsupervised. Highlighted the need for human approval before checkout and verification of product selection.

- **Chantrice Santiago** (multi-agent LLM systems): Stressed that multi-agent workflows only succeed if roles are clear and escalation protocols are explicit. Identified reasoning failures when constraints conflict.

- **Kanishka Gupta** (nutrition & ingredient substitution): Anchored the claim in nutrition science. Emphasized that adherence is the limiting factor—a perfect plan users won't follow is useless.

- **Sneha Vyas** (generative meal planning): Highlighted that users need 3–5 options, not 1 recommendation. Stressed the importance of learning from weekly adherence patterns.

---

## Slide 2 Talking Points (60 seconds)

**Title:** "Human-AI Complementarity in Prompt-to-Plate"

**Narrative:**
> Meal planning is a multi-constraint optimization problem. Users must balance nutrition goals, budget, cooking time, allergies, cuisine preferences, and sustainability all at once. That's too much for either humans or AI to solve alone.
>
> Humans excel at judgment—"Will I actually eat this?" and "Does this fit my life?"—but struggle with simultaneous constraint optimization.
>
> AI excels at rapid exploration and verification—generating options and catching allergy violations—but cannot judge adherence or hold ethical responsibility.
>
> Our hybrid team works because AI explores options *within* human-defined boundaries, and humans retain control over what matters most. Neither party could achieve realistic, sustainable outcomes alone.

**Key Visual:** Three columns:
1. **Human-Alone:** "Slow, inconsistent, high cognitive load"
2. **AI-Alone:** "Fast, but unsafe; may miss allergies; users don't trust it"
3. **Hybrid (Prompt-to-Plate):** "Fast + safe + sustainable + trusted; humans in control"

---

## References

Gonzalez, C., Donahue, K., Goldstein, D. G., Heidari, H., Jalali, M. S., Schelble, B., Singh, A., & Woolley, A. W. (2026). Toward a science of human–AI teaming for decision making: A complementarity framework. *PNAS Nexus*, *5*(3), pgag030. https://doi.org/10.1093/pnasnexus/pgag030
