# Prompt-to-Plate: Opportunity Framing
## Evidence-Based Feature Prioritization for Checkpoint 3

**Team:** Srinath Venkatesh, Chantrice Santiago, Kanishka Gupta, Sneha Vyas  
**Date:** October 6, 2026  
**Reference:** Gonzalez et al. (2026). Toward a science of human–AI teaming for decision making: A complementarity framework. *PNAS Nexus*, *5*(3), pgag030.

---

## Executive Summary

Checkpoint 2 validation tested three AI platforms (Claude, Copilot, Gemini) across 6 meal-planning scenarios and collected 8 speed-dating interviews. The evidence reveals critical gaps in AI reliability, human-AI complementarity, and user trust. This document prioritizes Prompt-to-Plate features in four phases, grounded in the complementarity framework and validated by empirical failures and user expectations.

**Core finding:** Users care more about **trust through practicality** (accurate cooking times, ingredient reuse, budget transparency) than about nutrition optimization alone. AI excels at generating options *within constraints*, but deterministic validation and human approval gates are not optional—they are fundamental to complementarity.

---

## 1. Hypothesis Evolution

### Assumptions That Changed

#### 1.1 **Trust Is Built on Practicality, Not Accuracy Alone**
- **Original assumption:** Users would trust a system that generated nutritionally correct meal plans.
- **Finding that changed it:** Santiago's Interview 1 (UIUC MSTM): *"If something says 20 minutes and actually takes an hour, I'm not going to use it again."* Across all 8 interviews, cooking-time accuracy and ingredient count (not calorie targets) emerged as the primary trust signals.
- **Why it matters:** A perfect nutrition plan that requires 2 hours when the user expects 20 minutes will be abandoned. Trust calibration depends on matching the AI's claims to real-world execution.
- **Design implication:** Cooking time, ingredient count, and effort level must be validated against independent sources—not estimated from AI confidence.

#### 1.2 **Users Prioritize Ingredient Reuse Over Maximum Variety**
- **Original assumption:** Users want diverse meals to avoid eating the same thing twice.
- **Finding that changed it:** Gupta's Interview 5 and Santiago's Interview 3 both said: *"I'd rather have fewer ingredients and repeat things than have 20 ingredients for five meals."* This was consistent across interviews—users value **waste reduction and lower cognitive load** over novelty.
- **Why it matters:** The system can optimize for different goals. Users don't always want the AI's default (variety); they want to control the trade-off.
- **Design implication:** Expose the cost-optimization mode upfront: "Prioritize: (A) Ingredient reuse, (B) Variety, (C) Price." Let users choose.

#### 1.3 **Pantry Awareness Enables Trust in Cost Efficiency**
- **Original assumption:** Pantry information is nice-to-have context for slightly better recommendations.
- **Finding that changed it:** Venkatesh's Interview 1: *"If I already have rice and onions, I don't want the app telling me to buy more."* Not accounting for pantry inventory signals to the user that the AI doesn't understand their situation—it wastes money and breaks trust in the entire cost estimate.
- **Why it matters:** A user who notices duplicate purchases loses confidence in the system's understanding of their constraints.
- **Design implication:** Pantry input must come **before** meal planning, not after. Display what the system will reuse: "We'll use these in 7 meals. Not purchasing again."

#### 1.4 **Targeted Edits Beat Full-Plan Regeneration**
- **Original assumption:** Users can regenerate the entire weekly plan if they don't like one meal.
- **Finding that changed it:** All interviews preferred targeted swaps. Santiago's Interview 1: *"I'd want to see the whole plan and then say 'change Wednesday.' I wouldn't want to start over every time I don't like one meal."* Full regeneration feels like cognitive overload.
- **Why it matters:** Interrogation capacity is limited. Asking users to regenerate a 7-day plan for a single meal swap exceeds their willingness to engage.
- **Design implication:** Single-meal swap must change only that day's meals, recalculate nutrition for that day only, and show: "New cart changes: removed Y ingredient, added Z ingredient, net cost +$2."

#### 1.5 **Meta-Coordination Requires Clear Decision Rights**
- **Original assumption:** If the AI flags a conflict (e.g., "This lentil meal takes 20–25 min, but your limit is 20 min. Confirm?"), users will treat it as unresolved and wait for approval.
- **Finding that changed it:** Claude's Prompt 4 flagged the timing problem but then presented the plan as "feasible" and "meets all targets." The response generated the plan without waiting for explicit user approval of the conflict. Users expect **hard gates**: "Cannot proceed until you choose: (A) Accept overage, (B) Remove meal, (C) Increase limit."
- **Why it matters:** Escalation ambiguity erodes trust. "Ask for confirmation then proceed anyway" violates the user's role as final authority on hard constraints.
- **Design implication:** Hard gates for allergies, no-pantry status, and budget limits. No silent workarounds. Escalate unresolved conflicts; do not proceed.

---

## 2. Feature Prioritization Matrix

### Phase 1: MVP — Core Complementarity Loop

These features are **essential for demonstrating human-AI complementarity** (user + AI > either alone) and must ship together.

| Feature | Evidence | Theory | Design Justification | Priority |
|---------|----------|--------|----------------------|----------|
| **Deterministic Nutrition Validation** | Claude P1–P3: Meal totals ±5% accuracy. Copilot P2: Missing shakes, nutrition misses. Gemini P1, P2: Invented products, calorie misses. | **Reasoning (Gonzalez et al.):** "AI systems excel at systematic checks and consistency monitoring but may fail silently with high confidence." Nutrition arithmetic must be *deterministic*, not LLM-generated. | Move all nutrition calculations to Python/SQL, not LLM. Generate line items with AI; verify with code. Display: "Subtotal: $196.81 (verified)" vs. "Estimated: $196.81 (±2%)." | **Must-Have** |
| **Hard-Constraint Escalation Gates** | Claude P4: Flagged timing but claimed feasibility anyway. Gemini P4: Changed no-pantry constraint without approval. All interviews: No silent workarounds. | **Meta-Coordination (Gonzalez et al.):** "Clear decision rights and escalation points help prevent an AI-generated plan from being mistaken for an approved purchase." Hard constraints (allergies, no-pantry, budget) are user decisions, not AI suggestions. | When constraints conflict, stop: "Cannot meet: 120g protein + $75 budget + no-cook + severe nut allergy. You must re-prioritize. Options: (A) increase budget, (B) lower protein, (C) add 10 min cook time." Wait for explicit choice. | **Must-Have** |
| **Provenance Tags on All Data** | Gemini P1, P2, P5: Invented prices ($1.99 vs. $2.49), invented curry kit ($14.99, unverified nutrition). Claude P1, P2: Correctly labeled produce yields as estimated. | **Memory (Gonzalez et al.):** "AI's prodigious memory is only as useful as its accuracy, sourcing, and currency. Language models may hallucinate or present outdated information with confidence." Users cannot trust data without knowing its source. | Every price: "Confirmed from Costco (Sept 30)" vs. "Estimated ±10%." Every cooking time: "Tested: 20 min (Recipe.com, avg 5 reviews)" vs. "Estimated from similar recipes." Flag uncertain allergen data: "Incomplete allergen info—label review required." | **Must-Have** |
| **Itemized Output Before Approval** | Copilot P1: Bare cost estimate ($295.77) with no meals/ingredients listed. Claude P1–P3: Full meal table, daily totals, cart rows with prices. | **Interrogation (Gonzalez et al.):** "Teams perform best when humans actively interrogate AI recommendations rather than passively accepting them." Bare summaries prevent scrutiny. | Always provide: meal table, daily nutrition, ingredient list with quantities, cart rows with unit prices. If incomplete, say so: "Missing 3 ingredients. Confidence: 60%. Approve partial plan?" | **Must-Have** |
| **Cart-to-Meal Coverage Check** | Copilot P2, Gemini P5: Missing ingredients from cart make plans unexecutable. User has to shop again. | **Reasoning (Gonzalez et al.):** A cost-efficient plan missing ingredients is not cost-efficient—the user wastes time and money on a second trip. System's reasoning must be end-to-end: meals → all ingredients in cart → ready to execute. | Enforce: "Meal ingredients coverage: ≥95% required." If <95%, fail: "Missing ingredients for [meal list]. Options: (A) add to cart, (B) remove meals, (C) substitute." Do not present incomplete carts as ready. | **Must-Have** |

### Phase 2: Trust Calibration & Safety — Complementarity Assurance

These features enable users to trust the AI's work and defend against AI failures (the core challenge from Checkpoint 2).

| Feature | Evidence | Theory | Design Justification | Priority |
|---------|----------|--------|----------------------|----------|
| **Cooking-Time Verification** | Santiago Interview 1: "If something says 20 minutes and actually takes an hour, I'm not going to use it again." Claude P4: Lentil meals marked 20–25 min against 20-min limit. Gemini P2: Pizza claimed as feasible but took 25 min on weekday. | **Attention (Gonzalez et al.):** "Attention ensures priorities are respected" and "one trust violation can end the relationship." Cooking time is a hard constraint in daily life, not a soft preference. | Test all cooking times against trusted recipe databases (AllRecipes, NYT Cooking, Food Network). Label confidence: "Tested: 20 min" vs. "Estimated: 20–30 min (based on similar recipes, ±10% variance)." Add toggle: filter out low-confidence times. | **High Priority** |
| **Allergen Escalation (Hard Gate)** | Gemini P4: "Completely free of peanuts and tree nuts" without full ingredient data. Gemini P5: Invented curry kit with unverified allergen status, then served it anyway. All interviews: Every user expects allergen review before purchase. | **Ethical Authority (Gonzalez et al.):** "AI systems cannot hold moral agency or be held responsible for harm. Humans must remain the locus of ethical authority in consequential decisions." Allergen safety is a consequence. The AI cannot claim safety it cannot verify; doing so usurps human authority. | Hard constraint: Never claim "allergen-free" without verified ingredient list + cross-contact info. If uncertain, escalate: "This product's allergen details are incomplete. We cannot verify tree-nut safety. Escalating to you for label review. Confirm before purchase?" | **Must-Have** |
| **Locked Preferences + Constraint Hierarchy** | Venkatesh Interview 2: "If I say chicken for lunch, don't change it later just because you're optimizing the rest of the plan." Gupta Interview 7: "I'd want to be able to lock certain preferences and then edit around them." | **Meta-Coordination (Gonzalez et al.):** "Role partitioning requires that humans decide what matters most." Users should control *which* constraints are fixed vs. flexible. | Implement lock mechanism: User locks meals, proteins, budget. Display locked items prominently. Never re-optimize locked items without re-approval. Show: "You locked: [chicken for lunch]. We're re-optimizing around this. New changes: [Y, Z]. Approve?" | **High Priority** |
| **Visible Progress During Generation** | Gupta Interview 4: "Probably 10–15 seconds. If it takes longer, I'd probably just ask ChatGPT separately." Vyas Interview 8: "I'd wait up to a minute for a good weekly plan." | **Attention (Gonzalez et al.):** "If the system doesn't deliver results within the user's attention window, the user stops paying attention and goes elsewhere." Visible progress raises latency tolerance. | Target 15–20s for full plan. Show: "Checking pantry (1/4)... Generating meals (2/4)... Validating nutrition (3/4)... Building cart (4/4)." Visible progress reduces perceived wait time. | **High Priority** |
| **No-Pantry as Immutable Constraint** | Gemini P4, P5: "Assumed pantry staples." Claude P1: Correctly assumed empty pantry, showed all ingredients in cart. | **Reasoning & Accountability (Gonzalez et al.):** "Hard constraints are not suggestions; they are boundaries. Changing them without approval violates reasoning and accountability." No-pantry means "buy everything shown in the cart or meal is infeasible." | Treat as hard gate. If plan cannot be built without pantry, escalate: "Cannot meet targets without pantry staples. Options: (A) you provide pantry list, (B) increase budget, (C) lower nutrition target." Do not guess. | **High Priority** |

### Phase 3: Adherence & Personalization — Moving Toward Sustained Use

These features address the finding that generating a plan is not the hardest part; *following it over time* is.

| Feature | Evidence | Theory | Design Justification | Priority |
|---------|----------|--------|----------------------|----------|
| **Cost-Optimization Mode Selection** | Gupta & Santiago interviews: "Fewer ingredients, repeat meals" not "maximum variety." Gap Analysis: "Users prioritize ingredient reuse and waste reduction over rock-bottom prices." | **Attention (Gonzalez et al.):** "Users want the AI to focus on their priority (fewer ingredients, less waste), not on maximizing variety. The system should make this priority explicit." | Add mode selector before generation: "Prioritize: (A) Ingredient reuse (fewer unique items), (B) Variety (different meals), (C) Price (cheapest items)." If (A), enforce max N unique ingredients/week and reuse aggressively. Show: "Chicken used in 4 meals; 95% of purchased chicken will be consumed." | **Medium Priority** |
| **Targeted Meal Swap (Single-Day Rebuild)** | Santiago Interview 1: "I'd want to see the whole plan and then say 'change Wednesday.'" All interviews: Prefer targeted edits to full regeneration. | **Interrogation (Gonzalez et al.):** "Asking users to regenerate a 7-day plan for a single meal swap overwhelms interrogation capacity. Users need surgical edits." | User clicks "Replace Wednesday dinner." System generates 3 options for that day only, recalculates nutrition for Day 3, rebuilds cart to reflect change. Show: "New cart changes: [removed Y ingredient, added Z ingredient, net cost +$2]." Do not touch approved meals. | **Medium Priority** |
| **Cart-Edit Impact Display** | Gupta Interview 6: "I'd want to edit the grocery cart and have it tell me what meals are affected. That would be really useful." | **Shared Mental Models (Gonzalez et al.):** "When a user removes an ingredient from the cart, the system should show which meals lose that ingredient and suggest replacements. This builds a shared model of the plan's structure." | On cart edit: User removes Greek yogurt. System shows: "Affects: Monday breakfast (loses 15g protein), Wednesday snack (loses 20g protein). Replace with: [options]." Require re-approval of meal edit before finalizing. | **Medium Priority** |
| **Pantry-First Ingredient Swapping** | Gupta's Inspiration: "When a planned recipe requires an ingredient the user doesn't have, the system can first determine whether an appropriate substitute already exists in the user's pantry." | **Memory & Transactive Memory (Gonzalez et al.):** "The AI's memory of what the user already has is critical. Buying duplicate items wastes money and signals the AI doesn't know the user's context." | Before cart generation, ask: "You have: rice, onions, olive oil, salt, eggs." System shows: "We'll use these in: [7 meals]. Not purchasing again." Remove these from cart. At checkout: "Your pantry inventory: [rice, onions]. Final cart excludes these. Confirm?" | **Medium Priority** |

### Phase 4: Long-Term Adaptation & Learning — Full Complementarity Realization

These features support iterative improvement and adherence tracking, but come after core reliability and trust are established.

| Feature | Evidence | Theory | Design Justification | Priority |
|---------|----------|--------|----------------------|----------|
| **Weekly Adherence Tracking & Feedback Loop** | Vyas's Inspiration: "Use what the user actually buys or skips as feedback for the next plan, instead of treating the first recommendation as the final answer." Khamesian et al. (2025): Adaptation over time improves adherence. | **Memory (Gonzalez et al.):** "After each week, the system learns from user behavior over time: which meals users actually prepare vs. skip, which meals are skipped, whether they buy alternatives." This enables continuous improvement. | After each week: "You prepped 5/7 dinners. Skipped: [lentil curry, pan-fried tofu]. Should we remove these?" Learn preferences from behavior. Track adherence patterns. Adapt next week's recommendations. | **Lower Priority (Phase 4)** |
| **Wearable/Fitness Integration** | Vyas's Inspiration: "Users provide their targets; wearable data can refine them." Research gap: Current scope does not include activity tracking. | **Meta-Coordination & Continuous Learning (Gonzalez et al.):** "The system can better understand changes in lifestyle and energy demands by integrating activity data over longer-term trends." | Future work: Integrate Apple Health, Google Fit to inform macro adjustments. Do not *calculate* targets (users provide those); use activity trends to refine within user's approved range. | **Out of Scope (Phase 3+)** |
| **Progressive Dietary Adaptation** | Kim et al. (2025): "Ingredient substitution can support gradual dietary improvements." Gupta's Inspiration: "Identify alternatives that satisfy dietary/nutritional goals while remaining similar in flavor and function to the original ingredient." | **Memory (Gonzalez et al.):** "The system learns from user behavior and suggests incremental improvements rather than abrupt changes." | Over 4–8 weeks, suggest progressive substitutions: "Week 1: Keep your favorites. Week 2: Reduce portion sizes. Week 3: Add one vegetable. Week 4: Replace one processed item with lower-sodium alternative." Show change is user-approved each step. | **Lower Priority (Phase 4)** |

---

## 3. Mapping Features to Gaps (Receipt → Theory → Design)

This table shows how each prioritized feature directly addresses a complementarity gap identified in Checkpoint 2.

| Gap Identified | Feature Response | Why This Fixes Complementarity |
|---|---|---|
| **Hallucinated prices & products (Gemini P1, P2)** | Provenance tags on all data | Users regain ability to interrogate AI's source claims. "Confirmed" vs. "Estimated" labels rebuild trust in memory. |
| **Timing conflicts claimed as feasible (Claude P4)** | Hard-constraint escalation gates + cooking-time verification | Stops AI from claiming feasibility before conflicts are resolved. Escalation = meta-coordination clarity. |
| **Missing ingredients from cart (Copilot P2, Gemini P5)** | Cart-to-meal coverage check | Ensures reasoning is end-to-end. User's *actual* ability to execute (not just planned nutrition) becomes the measure of success. |
| **Ingredient disappearing without explanation (Copilot P2)** | Itemized output + cart-edit impact display | Transparency on consequences. User builds shared mental model of plan structure. |
| **Pantry items repurchased (Gemini P1, P4)** | Pantry-first input + inventory subtraction | AI's memory of user's context becomes visible and auditable. Duplicate purchases signal system doesn't understand. |
| **Unclear decision rights (All models)** | Meta-coordination clarity: locked preferences, hierarchy | Users define what can be auto-optimized vs. what requires approval. Clear role boundaries. |
| **Latency abandonment (Gupta Interview 4)** | Visible progress during generation | Attention = users stay engaged long enough to see results. |
| **One bad experience ends engagement (Santiago Interview 1 on cooking time)** | Trust calibration through accuracy validation | One verified success rebuilds confidence. One verified failure acknowledged honestly maintains calibration better than overconfidence. |
| **Users don't know what matters most (Vyas Interview 8)** | Cost-optimization mode + constraint hierarchy | Explicit prioritization lets users define "good plan" themselves, not AI's default. |

---

## 4. Evidence-Based Hypothesis for Checkpoint 3

### Complementarity Test Conditions

**Checkpoint 3 will test the hypothesis:**  
*If these Phase 1 & 2 features are implemented, then human + Prompt-to-Plate system will outperform both human-alone meal planning and AI-alone generation on measures of **constraint satisfaction, adherence likelihood, trust, and perceived effort reduction**.*

**Evaluation Framework:**

| Dimension | Human-Alone Baseline | AI-Alone Baseline | Hybrid (Prompt-to-Plate) Success Threshold |
|---|---|---|---|
| **Constraint Satisfaction** | Users meet 60–70% of stated constraints (realistic given manual planning). | AI meets 85–90% on paper but users distrust 40% of outputs. | Hybrid meets ≥95% constraints *and users trust* ≥80% of outputs. |
| **Adherence Likelihood (User Report)** | "Would actually prepare" ~70% of meals users designed themselves. | "Would actually prepare" ~50% of AI-generated meals (realistic-sounding plans don't match lived experience). | "Would actually prepare" ≥80% of hybrid plans (practical constraints honored + user judgment integrated). |
| **Trust Calibration** | Users trust their own judgment but lack tools; high confidence, medium accuracy. | Users don't trust AI outputs; low confidence, low accuracy. | Users trust Prompt-to-Plate on routine work (nutrition checks, arithmetic), escalate to AI on judgment calls (preferences), retain authority on hard constraints. |
| **Perceived Effort Reduction** | Baseline 1.0 (100% of manual effort). | AI claims 0.3 but users spend extra time verifying = 0.7 actual. | Target 0.4–0.5 (users spend 40–50% their manual time; AI handles generation + verification; users handle judgment & approval). |

### Specific Testable Outcomes

1. **Memory failure drops by ≥80%:** Plans do not reference products outside the verified catalog; all prices grounded to source; no hallucinated allergen claims.
2. **Escalation clarity improves:** When constraints conflict (e.g., high protein + low budget), system pauses with explicit options, not silent workarounds. Users report: "I knew exactly what was happening and why I had to choose."
3. **Cooking time becomes a trust signal:** Users report cooking-time accuracy as the #1 factor in "would I use this again?" (replicates Santiago Interview 1 pattern across new sample).
4. **Single-day meal swaps reduce plan-regeneration requests by ≥50%:** Users can edit one meal without full rebuild; approval workflow becomes less cognitive burden.
5. **Cart-to-meal coverage check catches ≥95% of ingredient misses:** Users do not discover missing items after checkout; "complete cart" becomes a trust signal.
6. **Pantry awareness raises cost-estimate accuracy:** Users report: "The system actually understood what I already had" (replicates Venkatesh Interview 1 finding).

---

## 5. Out-of-Scope Decisions (Why Phase 3+ Features Wait)

The following features, though valuable, are deferred until Phase 2 complementarity is proven:

- **Activity tracking integration:** Requires user consent for health data; deferred until core meal planning is trusted.
- **Progressive dietary substitution:** Valuable for long-term adherence, but users must first trust that the *current* plan is achievable.
- **Behavioral coaching:** Yang et al. (2025) shows coaching works, but Prompt-to-Plate focuses on operational planning + shopping, not coaching. Coaching is a separate workflow.
- **Multiple-user households:** Adds coordination complexity; MVP serves single-user scenarios first.
- **Official retailer APIs:** Adds latency and complexity; heuristic store selection + manual checkout review proves the concept.

---

## 6. Conclusion: Opportunity → Checkpoint 3 Test Design

Prompt-to-Plate's opportunity is **to prove that human-AI complementarity on meal planning requires role clarity, not AI capability alone.** The prompting study showed that even high-performing models (Claude 22/24, Gemini on translation 20/24) can fail at meta-coordination and memory validation. 

**Phase 1 & 2 features are not "nice to have"—they are structural requirements for complementarity:**

- **Deterministic validation** ensures reasoning is measurable (not AI confidence).
- **Hard-constraint escalation** ensures meta-coordination is explicit (not silent workarounds).
- **Itemized output** ensures interrogation is possible (not bare summaries).
- **Provenance tags** ensure memory is auditable (not hallucinated claims).
- **Cooking-time verification** ensures attention is calibrated (not one-failure-ends-engagement).

Checkpoint 3 will test whether these features enable users to **trust the AI within bounded roles while retaining control over what matters most.**

---

## References

Gonzalez, C., Donahue, K., Goldstein, D. G., Heidari, H., Jalali, M. S., Schelble, B., Singh, A., & Woolley, A. W. (2026). Toward a science of human–AI teaming for decision making: A complementarity framework. *PNAS Nexus*, *5*(3), pgag030. https://doi.org/10.1093/pnasnexus/pgag030

Yang, E., Garcia, T., Williams, H. G., Kumar, B., Ramé, M., Rivera, E., Ma, Y., Amar, J., Catalani, C., & Jia, Y. (2025). A behavioral science-informed agentic workflow for personalized nutrition coaching: Development and validation study. *JMIR Formative Research*, 9, e75421.

Kim, H., Venkataramanan, R., & Sheth, A. (2025). A survey on food ingredient substitutions. arXiv:2501.01958.

Khamesian, S., Arefeen, A., Carpenter, S. M., & Ghasemzadeh, H. (2025). NutriGen: Personalized meal plan generator leveraging large language models to enhance dietary and nutritional adherence. arXiv:2502.20601.
