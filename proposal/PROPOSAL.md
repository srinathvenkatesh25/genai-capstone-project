# Prompt-to-Plate: An Agentic AI Platform for Instant Meal Planning and Grocery Automation

*Chantrice Santiago, *Kanishka Gupta, *Sneha Vyas, *Srinath Venkatesh

---

## 1. Problem Significance

Healthy eating requires more than knowing which foods are nutritious. Users must repeatedly balance nutritional goals, preferences, allergies, budget, cooking ability, preparation time, ingredient availability, and physical activity. They must then convert those decisions into recipes, grocery purchases, portion sizes, and daily tracking. Existing applications usually address only one part of this process, forcing users to coordinate separate calorie trackers, recipe platforms, and grocery services. This burden is especially significant for users with limited time, established eating habits, or low willingness to adopt an entirely new diet. Rather than prescribing drastic dietary changes that may be difficult to sustain, the application will adapt to each user’s current lifestyle and introduce realistic, incremental changes that support long-term consistency.

This problem requires an AI-native solution because the system must interpret natural-language preferences, reconcile multiple changing constraints, execute shopping-related tasks, and learn from user behavior over time. A fixed recommendation engine cannot easily respond to requests such as “make next week cheaper,” “use the chicken already in my refrigerator,” or “introduce healthier Asian meals gradually.” The proposed platform will integrate the full cycle of Profile → Plan → Optimize → Shop → Eat & Track → Learn → Adapt, reducing both decision fatigue and manual work.

## 2. Prior Work & Gaps

Prior research demonstrates the feasibility of AI-supported nutrition planning but does not provide an end-to-end adaptive food-management system. NutriGen combines LLMs with USDA nutrition data and produced meal plans close to calorie targets, showing the value of grounding generative outputs in validated databases (Khamesian et al., 2025). Similarly, Papastratis et al. (2024) combined a variational autoencoder, optimization logic, and ChatGPT to generate nutritionally appropriate weekly plans across thousands of simulated and real profiles. However, neither system connects meal planning to pantry-aware ingredient reuse, grocery-cart creation, adherence tracking, and weekly adaptation.

Ingredient substitution research supports adapting recipes for allergies, nutritional goals, and ingredient availability, but existing methods vary in transparency, contextual awareness, and safety (Kim et al., 2025). Meanwhile, Yang et al. (2025) demonstrated that a two-agent nutrition-coaching workflow could identify users’ behavioral barriers in more than 90% of evaluated cases and deliver personalized tactics. Its focus, however, is coaching rather than operational meal planning and shopping.

Multi-agent research supports decomposing the platform into specialized roles with distinct profiles, memory, planning, communication, and actions (Chen et al., 2024). This decomposition is important because multi-constraint planning remains difficult for a single model. A collaborative-agent planning method achieved a 42.68% success rate on TravelPlanner, compared with 2.92% for a baseline GPT-4 approach (Zhang et al., 2024). Shopping remains particularly risky: ShoppingComp found that leading models achieved only 17.76% product-retrieval F1 and 35.42% safety compliance in complex shopping scenarios (Tou et al., 2025). Purchasing agents may also exhibit position bias, unstable preferences, and sensitivity to seller-controlled descriptions and platform endorsements (Allouah et al., 2025). Therefore, autonomous grocery purchasing requires constrained decision logic, source verification, auditing, and human approval.

## 3. Proposed Technical Approach

The platform will use an orchestrated multi-agent architecture:

1. A Profile Agent structures user goals, preferences, allergies, budget, schedule, cooking ability, and pantry contents.
2. A Meal-Planning Agent generates candidate weekly meals grounded in verified nutrition and recipe databases.
3. An Optimization Agent uses deterministic constraint checks to balance calories, macronutrients, cost, preparation time, ingredient reuse, package sizes, and estimated waste.
4. A Shopping Agent retrieves eligible products and prepares a cart while treating allergies, budget, and user exclusions as hard constraints.
5. A Coaching Agent reviews adherence, skipped meals, substitutions, hunger, spending, and user feedback to recommend gradual, behaviorally informed changes.

Users will review the proposed plan, modify it conversationally, and explicitly approve the grocery cart before checkout. After meals, users can confirm “ate as planned” or record a change. The system will use this feedback to update future plans rather than interpreting nonadherence as failure.

## 4. Checkpoint 2 Validation Plan

Checkpoint 2 will test the core profile-to-plan-to-cart prompt chain using synthetic user profiles representing different budgets, cultures, dietary patterns, allergies, cooking abilities, and pantry inventories. Each output will be scored for nutritional accuracy, constraint satisfaction, ingredient reuse, estimated cost, food waste, and explanation quality.

Nutrition values will be recalculated independently using a trusted database rather than accepted from the LLM. Agent performance will be compared with a single-prompt baseline to determine whether decomposition improves constraint satisfaction. Shopping tests will include unavailable products, misleading product descriptions, sponsored placements, unsafe substitutions, and conflicting constraints. Repeated runs will measure output stability. A small user evaluation will assess plan relevance, practicality, clarity, and willingness to follow the recommendations.

## 5. Risk Analysis & Mitigation

- **Privacy / data security:** Health profiles, eating behavior, and pantry images are sensitive data. The platform will minimize collection, obtain explicit consent, encrypt stored and transmitted data, restrict access, and allow deletion. Images will be processed only for user-approved food identification.
- **Bias / toxicity:** Bias and toxicity testing will cover cultural foods, body sizes, socioeconomic constraints, disabilities, and eating patterns. The interface will avoid moralizing labels, extreme calorie recommendations, and stigmatizing language. Agent decisions will be logged with their sources and constraint checks so failures can be audited and corrected.
- **Safety:** Hallucinated nutrient values, unsafe substitutions, or missed allergens could cause harm. The system will therefore validate nutrition and product information against trusted databases, enforce allergies as non-negotiable rules, flag uncertain matches, and require approval before purchases. It will not diagnose conditions or replace a registered dietitian. Users with medical nutrition needs will be advised to obtain professional review.
- **Hallucination handling:** Hallucinated nutrient values, unsafe substitutions, or missed allergens could cause harm. The system will therefore validate nutrition and product information against trusted databases, enforce allergies as non-negotiable rules, flag uncertain matches, and require approval before purchases.
