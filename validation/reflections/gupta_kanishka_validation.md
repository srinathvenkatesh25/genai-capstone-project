# Student Reflection - Kanishka Gupta

**Student Name:** Kanishka Gupta\
**File Name:** gupta_kanishka_validation.md

## Prompting and Interview Notes

### Notes from the Prompting Study

Across our testing with Claude, Microsoft 365 Copilot, and Gemini, we
observed a consistent pattern: the models generate plausible-sounding
plans but struggle with multi-constraint validation. Claude performed
best overall, producing detailed meal plans and explanations, but even
its strongest responses contained subtle error like proposing a plan
that violates a stated time constraint without acknowledging the
conflict.

Copilot was more cautious, often declining to provide complete plans
when uncertain, which protected the user but left critical gaps. It
disclosed budget problems clearly but sometimes failed to generate the
full seven-day plan requested.

Gemini had the highest hallucination rate, inventing products and
nutritional claims. In one scenario, it proposed a \"quick curry kit\"
that does not exist at the selected retailer. This taught us that
confidence and accuracy are not correlated---the model\'s tone did not
change when it was wrong.

Across all three platforms, allergy safety and checkout boundaries were
enforced better than routine arithmetic. The prompting study revealed a
critical gap: users cannot rely on the model\'s confidence to verify
whether a plan is actually feasible.

### Notes from Speed Dating

#### Interview 1

I spoke with a graduate student who meal-preps on weekends. Their
constraint was tight: they needed meals under 30 minutes, high protein,
and low cost. They said the biggest friction was not knowing whether a
proposed plan was realistic until they started cooking.

When I asked what would make them trust the system, they said: \"Don\'t
just tell me it works. Show me you checked the time, the budget, and the
pantry.\" They were willing to approve a plan, but only after seeing
evidence that trade-offs had been made explicitly.

They did not want the system to silently downgrade quality; they wanted
to see what was being compromised and decide whether to accept it.

#### Interview 2

I also interviewed an undergraduate who uses the system for weeknight
dinners. They have less cooking experience and rely on familiar
ingredients. They said the system\'s biggest value would be \"telling me
what to buy and in what order, without me having to think.\"

But they also said: \"If something doesn\'t work out, I want to know
why---not just that it failed. Did we run out of budget? Did the timing
not work? That helps me know what to fix next time.\"

They wanted the system to be transparent about its reasoning, not just
its output.

#### Cross-Interview Takeaway

Both participants valued accuracy and transparency over convenience.
They wanted the system to generate options, but they did not trust a
plan without evidence that the system had validated it against their
real constraints. This mirrors our prompting findings: a coherent,
confident-sounding response is not the same as a correct one.

## Class-Generated Storyboard

The storyboard captures the complete workflow: from the user\'s planning
burden through AI-assisted generation and human review, to cart
verification and final human control over checkout. It shows the
decision gates and the moments where human judgment is essential.

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/a2ac3bf0-703e-40a7-b6e8-73b36309acd4" />



## One Finding That Changed (or Confirmed) My Assumption About the Proposed Scenario

The finding that most changed my assumption was how central
deterministic validation is to trust. When we built the gap analysis, I
initially focused on the gaps in AI capability---where Claude, Copilot,
and Gemini struggled. But the interviews showed me that users do not
judge the system solely by AI performance. They judge it by whether the
system checks its own work.

A user said: \"I don\'t care if the AI is perfect. I care that if
something goes wrong, the system caught it before it got to me.\" This
reframed the entire complementarity problem. The AI does not need to be
flawless; the system needs to be verifiable.

### Connection to Complementarity & Shared Mental Models (Gonzalez et al., 2026):

This connects directly to complementarity. The AI explores options and
generates proposals---capabilities humans struggle with at scale. Humans
bring judgment, constraint validation, and context. A shared mental
model emerges only when both parties understand what the other is
responsible for. In our case: the AI generates; the deterministic layer
validates; the human approves or rejects based on visible evidence, not
trust alone.

The interviews revealed that users want to see the checks. When
validation is visible---\"✓ Protein 35g,\" \"⚠ Prep time over 30
min\"---users calibrate their trust correctly. They stop assuming the
system is flawless and start treating it as a partner with specific,
transparent responsibilities. That builds a more honest shared mental
model and prevents over-reliance on AI confidence.

## Reflection

Working through the theory lens, gap analysis, and opportunity framing
taught me that this project is not primarily about AI capability---it is
about coordination and transparency. Every feature we prioritized
(targeted meal swaps, visible trade-offs, cart verification, checkout
control) traces back to a single principle: make the division of labor
between human and AI explicit and verifiable.

The gap analysis showed us where AI fails; the interviews showed us what
users actually need from those failures; the theory connected those
needs to complementarity. For Checkpoint 3, I would focus on making the
validation layer visible in the UI---not as a technical detail, but as a
trust signal. Users should see what the system checked and what it left
for them to decide. That transparency is what transforms a powerful AI
tool into a trustworthy partner.
