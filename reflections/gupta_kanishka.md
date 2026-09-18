# Student Reflection - Kanishka Gupta

**Student Name:** Kanishka Gupta  
**File Name:** `gupta_kanishka.md`

# Paper 1: Behavioral Science-Informed Agentic Workflows

## Full Citation & Link

Yang, E., Garcia, T., Williams, H. G., Kumar, B., Ramé, M., Rivera, E., Ma, Y., Amar, J., Catalani, C., & Jia, Y. (2025). A behavioral science-informed agentic workflow for personalized nutrition coaching: Development and validation study. *JMIR Formative Research*, 9, e75421. https://formative.jmir.org/2025/1/e75421

## Structured Summary

This paper addresses the problem that conventional nutrition applications often fail to support sustainable dietary change because they provide static and generic recommendations without identifying the behavioral, logistical, or psychological barriers users face. To address this, the researchers developed a two-agent architecture consisting of a barrier-identification agent using motivational probing and a tactic-delivery agent grounded in behavioral science frameworks such as the Behavior Change Wheel and EAST framework. The system was evaluated using 187 simulated patient vignettes assessed by behavioral experts, along with a pilot study involving real cardiometabolic patients. The findings showed that separating barrier identification from intervention generation enabled the system to identify users' underlying obstacles with high accuracy and perform better than single-prompt approaches. Participants in the pilot study also reported that the resulting recommendations were personalized, empathetic, and actionable.

## Three Key Insights

1. **Multi-Agent Decoupling Prevents LLM Cognitive Overload:** Splitting a complex task into specialized roles can improve the quality of the overall system. One agent can focus on understanding the user's barriers while another generates an intervention based on those identified constraints.

2. **Probing Before Prescribing Reduces Drop-off:** Effective dietary interventions should first ask exploratory and open-ended questions to uncover constraints such as decision fatigue, limited cooking skills, budget limitations, or time constraints before recommending solutions.

3. **Behavioral Grounding Beats Generic Rules:** Mapping user challenges to established behavioral frameworks can help an AI system provide recommendations that address the underlying reasons for non-adherence instead of simply repeating general nutritional advice.

## Two Limitations or Risks

1. **Absence of Downstream Physical Task Execution:** The system primarily provides behavioral coaching and recommendations. Tasks such as checking pantry inventory, creating meal plans, identifying missing ingredients, and purchasing groceries still remain with the user.

2. **Cascading Error Risks Across Agent Hand-offs:** A multi-agent architecture introduces dependency between agents. If the barrier-identification agent incorrectly interprets a user's constraint, the tactic-delivery agent may generate an irrelevant or ineffective recommendation.

## One Concrete Inspiration

**Three-Tier Agentic Architecture for Grocery and Meal Automation:**  
This paper inspires the multi-agent architecture of our **Prompt-to-Plate: An Agentic AI Platform for Instant Meal Planning and Grocery Automation**. Instead of asking a single LLM to understand the user and generate the entire solution, our system can first use a conversational agent to identify constraints such as weekly budget, dietary requirements, available cooking time, and food preferences.

These verified constraints can then be passed to specialized execution agents:

- **Pantry & Ingredient Optimizer:** Analyzes available ingredients, including ingredients identified from refrigerator or pantry images, and creates meal plans that maximize ingredient reuse while reducing unnecessary purchases and food waste.
- **Grocery Cart Agent:** Converts the final meal plan into the ingredients and quantities required for grocery shopping, reducing the amount of manual planning required from the user.

This extends the paper's idea of specialized agents from behavioral coaching toward practical meal-planning and grocery automation.

---

# Paper 2: Food Ingredient Substitutions

## Full Citation & Link

Kim, H., Venkataramanan, R., & Sheth, A. (2025). *A survey on food ingredient substitutions.* arXiv preprint arXiv:2501.01958. https://arxiv.org/abs/2501.01958

## Structured Summary

This survey examines the challenge of computational food ingredient substitution, where an alternative ingredient must preserve important characteristics such as culinary function, taste, and nutritional value. The authors review existing datasets, computational approaches, and domain-specific knowledge used to identify substitution relationships between ingredients. The survey organizes existing approaches across techniques including data-driven representation learning, knowledge-based methods, and newer language-model-based approaches. A major takeaway is that ingredient substitution is highly context dependent because the same ingredient can serve different purposes depending on the recipe and cooking technique. The paper also highlights the challenge of balancing multiple requirements when recommending substitutions, including dietary needs, nutrition, availability, and culinary compatibility.

## Three Key Insights

1. **Context-Dependent Substitution Rules:** Ingredient substitutions are not always simple one-to-one replacements. An appropriate substitute depends on the cooking method, the ingredient's functional role in the recipe, and the other ingredients being used.

2. **Structured Food Knowledge Can Improve Reliability:** Combining language-based reasoning with structured information about ingredients can help systems make more reliable substitutions instead of relying entirely on an LLM's generated recommendation.

3. **Ingredient Substitution Is a Multi-Constraint Problem:** Real-world substitution requires balancing several factors simultaneously, including dietary restrictions, nutrition, cost, taste, availability, and texture.

## Two Limitations or Risks

1. **Lack of Dynamic Inventory and Pricing Awareness:** Many substitution approaches focus primarily on ingredient relationships and do not fully account for changing real-world information such as the ingredients currently available in a user's pantry, package sizes, or grocery prices.

2. **Risk of Flavor Incompatibility and User Friction:** A technically valid substitution may still be undesirable to the user because of personal taste or regional food preferences. Optimizing only for nutrition or availability could therefore reduce satisfaction with the generated meal plan.

## One Concrete Inspiration

**Progressive Substitution and Pantry Optimization Engine:**  
This paper provides useful design ideas for the ingredient optimization component of our **Prompt-to-Plate** platform.

- **Pantry-First Ingredient Swapping:** When a planned recipe requires an ingredient the user does not have, the system can first determine whether an appropriate substitute already exists in the user's pantry instead of immediately adding another item to the grocery list.

- **Progressive Dietary Adaptation:** Ingredient substitution can also support gradual dietary improvements. The system could identify alternatives that satisfy dietary or nutritional goals while remaining similar enough in flavor and function to the original ingredient.

Together, these ideas could allow Prompt-to-Plate to treat ingredient substitution as a contextual optimization problem rather than simply replacing one missing ingredient with another.
