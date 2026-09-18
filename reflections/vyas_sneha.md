# Reflections — Sneha Vyas

## NutriGen: Personalized Meal Plan Generator Leveraging Large Language Models to Enhance Dietary and Nutritional Adherence

**Full Citation & Link:**
Khamesian, S., Arefeen, A., Carpenter, S. M., & Ghasemzadeh, H. (2025). *NutriGen: Personalized meal plan generator leveraging large language models to enhance dietary and nutritional adherence.* arXiv. https://arxiv.org/abs/2502.20601

### Structured Summary

NutriGen looks at the problem of creating meal plans that are personalized to a user's dietary preferences and calorie needs. The researchers developed a system that combines an LLM with nutrition information from the USDA database to generate these meal plans. They tested different LLMs and compared how well each model met the users' calorie targets, along with how long it took to generate the plans. The results showed that the models were generally able to create personalized meal plans with relatively small differences from the target calorie amounts, although performance varied across models. I found it interesting that the paper focuses mainly on getting the meal plan right, while the actual process of turning that plan into something the user can shop for and follow is not really addressed.

### Three Key Insights

1. One thing I took away from the paper is that LLMs can make meal planning much more flexible because users can describe their preferences in their own words instead of having to select from a fixed set of options.

2. The combination of an LLM and a nutrition database seems important. The LLM is useful for putting the plan together, but the database provides more reliable information about things like calories and nutrients.

3. The paper made me realize that generating a good meal plan is not necessarily the hardest part of the user's problem. Even if the plan is accurate, the user still has to figure out what they already have, what they need to buy, and whether they are actually going to follow the plan.

### Two Limitations or Risks

1. There is not much focus on what happens after the meal plan is generated. If a user keeps skipping certain meals, does not buy certain ingredients, or changes their preferences, the system does not seem to have a strong way of using that information to improve the next plan.

2. I also think there is a risk in relying on an LLM for personalized food recommendations. A plan can meet a calorie target and still not be appropriate for someone's particular dietary needs, so there needs to be some way of checking the recommendations rather than assuming the LLM got everything right.

### One Concrete Inspiration

For our project, I would take the idea of personalized meal planning but continue it one step further. Once the system creates a plan, it could look at what the user already has in their pantry and figure out what they actually need to buy. From there, it could create a grocery list or connect the items to a shopping cart. I also like the idea of using what the user actually buys or skips as feedback for the next plan, instead of treating the first recommendation as the final answer.

## AI Nutrition Recommendation Using a Deep Generative Model and ChatGPT

**Full Citation & Link:**
Papastratis, I., Konstantinidis, D., Daras, P., & Dimitropoulos, K. (2024). *AI nutrition recommendation using a deep generative model and ChatGPT.* *Scientific Reports, 14*, 14620. https://doi.org/10.1038/s41598-024-65438-x

### Structured Summary

This paper looks at how AI can be used to create personalized nutrition recommendations while still keeping the recommendations nutritionally accurate. The researchers combine a deep generative model, specifically a Variational Autoencoder (VAE), with ChatGPT to generate weekly meal plans based on a user's nutritional needs and preferences. They evaluate the generated plans using nutritional measures and compare them against the user's recommended daily nutrient intake. The results show that combining the generative model with ChatGPT can produce personalized meal plans while keeping the nutritional values relatively close to the target requirements. What I found most useful for our project was the way the researchers actually evaluate the nutritional quality of the generated plans instead of only looking at whether the recommendations seem reasonable.

### Three Key Insights

1. I liked the idea of using two different components for different parts of the problem. The generative model handles the nutritional side, while ChatGPT makes the recommendations easier to interact with and personalize.

2. The evaluation approach is something we could learn from. Instead of just assuming that an AI-generated meal plan is good, the paper checks the recommendations against specific nutritional targets.

3. A weekly plan gives the system more context than recommending one meal at a time. It makes it easier to think about the user's overall nutrition across several days rather than evaluating every meal separately.

### Two Limitations or Risks

1. Even if the generated plans are close to nutritional targets, this does not necessarily mean that users will actually want to eat or follow those meals. Personal preferences and practical factors can change what someone is willing or able to prepare.

2. Using ChatGPT as part of the recommendation process also introduces the possibility of inconsistent or incorrect responses. This makes it important to have some form of validation rather than relying only on the generated text.

### One Concrete Inspiration

The main idea I would take from this paper is its approach to **validating AI-generated meal plans against measurable nutrition targets**. For our project, we could use a similar checkpoint after the AI creates a plan and check things like calories and other nutritional requirements before showing the recommendation to the user. This would give us a way to measure whether our system is actually meeting the user's requirements rather than just judging the quality of the recommendations based on how they sound.
