# Student Reflection - Sneha Vyas

**Student Name:** Sneha Vyas
**File Name:** vyas_sneha_validation.md

---

## Prompting and Interview Notes

### Notes from the Prompting Study

- Our team ran the same six meal-planning and grocery-cart scenarios across Claude, Microsoft 365 Copilot, and Gemini. The scenarios covered typical requests, competing constraints, missing product information, allergy risks, budget pressure, and attempts to push the model toward checkout.
- **Claude** gave the most complete and well-explained plans, but it still called one plan feasible when two meals went past the stated cooking-time limit. The answer looked finished, which made the contradiction easy to miss.
- **Copilot** respected the checkout boundary and was upfront about budget problems, but it sometimes returned incomplete carts, missed nutrition targets, or made arithmetic errors. In one case it gave a cost estimate instead of the requested seven-day plan and itemized cart.
- **Gemini** handled converting an approved plan into a cart well, but during planning it invented products (including a curry kit that the retailer does not carry), left out ingredients, and reported incorrect nutrition, including in an allergy-sensitive scenario.
- Across all three tools, allergy refusals and the no-checkout rule held up better than routine math and multi-constraint tracking.
- What stood out to me most: incorrect nutrition values and invented products were presented in the same confident tone as correct ones, so the response itself gave no signal of what needed checking. This became the main thread I followed in my interviews.

**My takeaway:** The AI is useful for generating options and explaining trade-offs, but nutrition, ingredient facts, prices, and time limits need independent validation, and the user needs to be able to tell which values were checked.

---

## Notes from Speed Dating

### Interview 7: Early career professional (3–4 years experience)

**Profile:** Working professional, health-conscious, uses grocery delivery.

- They have a rough idea of what to eat but no system; they search recipes and build grocery lists by hand.
- Priorities: protein, calories, prep time, and food they actually enjoy. They did not want the app optimizing for nutrition alone.
- Nutrition accuracy matters to them, and they wanted the app to **separate what it knows from what it is estimating**.
- When requirements conflict, they want the trade-off spelled out. For example, "to stay under $75, we're changing these two ingredients."
- They want to **lock certain preferences** and edit around them.
- Automatically building the cart is fine; **placing the order is not**.
- They would wait about 30 seconds for the full plan if progress is visible.
- They would rather accept a small plan change than have the budget raised automatically.

| Dimension | Notes |
|---|---|
| Accuracy & hallucinations | Separate known facts from estimates, especially nutrition. |
| Reliability & consistency | Locked preferences must stay put during edits. |
| Latency & performance | About 30 s is acceptable if progress is visible. |
| UX friction / teaming | Don't optimize for nutrition alone; enjoyment matters. |
| Safety & guardrails | Cart can be automatic; the app must never place the order. |
| Cost & efficiency | Prefers small plan changes with stated trade-offs over a budget increase. |

### Interview 8: Mid-career professional

**Profile:** 35–45, works full-time, has a family, shops weekly.

- Meal planning is a weekly, mostly manual chore. The hard part is balancing everyone's preferences and avoiding food waste.
- Priorities: dietary restrictions first, then time, then cost. With a family, you can't optimize for one person.
- Ingredient information must be accurate, especially for allergies and dietary restrictions.
- If the system can't verify an ingredient, it should **flag it rather than guess**, because the consequence could be a health issue.
- They want a **weekly overview** rather than constant back-and-forth, and changing one meal should automatically update the grocery list.
- The AI can do most of the planning, but **anything involving an allergy, or placing the order, needs their approval**.
- They would wait up to a minute for a good weekly plan, saving 30 minutes of planning matters more than speed.
- They prioritize reusing ingredients and reducing waste, and would not compromise dietary requirements to save $5.

| Dimension | Notes |
|---|---|
| Accuracy & hallucinations | Ingredient info must be accurate for allergies and restrictions. |
| Reliability & consistency | Changing one meal should update the grocery list. |
| Latency & performance | High tolerance: up to 60 s; quality over speed. |
| UX friction / teaming | Weekly overview, little back-and-forth; must balance several family members. |
| Safety & guardrails | Flag unverified ingredients, never guess. Allergy decisions and ordering need approval. |
| Cost & efficiency | Ingredient reuse and less waste; never trade dietary requirements for savings. |

### Cross-Interview Takeaway

Both participants were comfortable letting the AI do the heavy lifting such as drafting the plan and building the cart, but both drew a hard line at placing the order. What they shared most was a demand for **honesty about certainty**: Interview 7 wanted estimates labeled as estimates, and Interview 8 wanted unverifiable ingredients flagged instead of guessed. They differed in scope and tolerance: the early-career participant plans for one person and wants fine control (locking preferences), while the mid-career participant plans for a family, wants less interaction, and will wait twice as long for a better result. Both preferred explicit, small trade-offs over silent changes or automatic budget increases. This lines up with the prompting study, where the tools presented guessed values with the same confidence as real ones.

---

## Class-Generated Storyboard

<img width="1312" height="1199" alt="image" src="https://github.com/user-attachments/assets/61ed59be-2d4a-4b7b-a66b-c66cd78416b3" />


*"From Overwhelmed to On Track"* follows a busy user on a Sunday evening who doesn't know where to start with healthy eating. She tells the AI what she needs: high-protein, vegetarian, Indian cuisine, around 1,800 calories a day, and quick to cook. She receives a weekly meal plan with calories and macro targets. She confirms the ingredient list, places the order herself, and cooks easy recipes through the week, ending with her nutrition goals met and less stress.

The storyboard captures the *ideal* experience I imagined before validation: the AI understands her request, the plan just works, and the only human step is pressing "Place Order." Looking at it after the interviews, it is also useful for showing what is missing. It has no estimated-vs-verified nutrition labels, no flag for an ingredient that can't be checked, no visible trade-off when constraints conflict, and no way to lock a preference before editing. Those gaps are the focus of the reflection below.

---

## Reflection

### One Finding That Changed (or Confirmed) My Assumption About the Proposed Scenario

My storyboard reflects my original assumption: if the AI produced a plan with the right calories and macros, the user would simply confirm and order. In other words, I assumed trust would come mostly from accuracy. My interviews showed that users care just as much about **knowing how sure the system is**. Neither participant expected perfection. Interview 7 asked the app to distinguish known facts from estimates, and Interview 8 said an unverified ingredient should be flagged, not guessed. The prompting study showed exactly why this matters: Gemini invented a product and incorrect nutrition values, and Claude declared an infeasible plan feasible, all in the same confident tone as correct answers.

**Connection to Complementarity & Shared Mental Models (Gonzalez et al., 2026):**

This is a trust-calibration problem rooted in the **memory** pillar. When the AI fills a gap in its knowledge with a plausible guess and doesn't mark it, the human has no signal for when to interrogate the output — so they either over-trust it or end up checking everything themselves. Neither outcome beats human-alone or AI-alone. Complementarity depends on the AI exposing its uncertainty so the human can direct their limited **attention** to the few items that actually need judgment.

My interviews also clarified **role partitioning** and decision rights. Both participants gave the AI planning and cart drafting, while keeping ordering for themselves; Interview 8 added allergy-related decisions to the human side. Interview 7's request to lock preferences is a way of setting fixed **goals and constraints** that the AI must work within, which keeps the shared mental model stable across edits.

Working through the validation steps shifted my view of the project from "can the AI make a good plan" to "can the user tell when the plan is trustworthy." The AI generates, a deterministic layer checks what can be checked, and the human decides on the exceptions with clear evidence in front of them.
