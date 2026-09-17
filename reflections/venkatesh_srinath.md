# Reflections

## What Is Your AI Agent Buying? Evaluation, Implications, and Emerging Questions for Agentic E-Commerce

**Citation & Link:** Allouah, A., Besbes, O., Figueroa, J. D., Kanoria, Y., & Kumar, A. (2025). What is your AI agent buying? Evaluation, implications, and emerging questions for agentic e-commerce. arXiv. [https://arxiv.org/abs/2508.02630](https://arxiv.org/abs/2508.02630)

**Summary:**

This paper asks a question I had not really considered before starting our project: once an AI agent does the shopping instead of a person, does it actually behave like a rational buyer, or does it develop its own quirks? The authors built a framework called ACES to observe how agents built on different LLMs choose products across a range of simulated marketplace conditions. They found that agents tend to fixate on a small set of products rather than exploring the full catalog, and that these preferences are surprisingly unstable, shifting when the underlying model gets updated. The agents also show consistent position bias that differs by provider, discount sponsored listings, and respond unevenly to price, ratings, and reviews. Sellers who understand these tendencies can rewrite product descriptions to exploit the agent's preferences. The paper's overall claim is that agentic commerce runs on a different logic than human shopping, and that logic needs to be studied on its own terms rather than assumed to mirror human behavior.

**Key Insights:**

1. An AI shopping agent is not a neutral stand-in for a human shopper. It carries its own biases, such as which products it samples and how it weighs price against reviews, and those biases come from the model itself rather than from the user's stated preferences.
2. Model updates can quietly reshuffle which products an agent favors even when nothing about the user's request changed. That is a stability problem I had not thought about before, since I assumed a fixed prompt would produce fairly consistent behavior over time.
3. Sellers can adapt their listings specifically to exploit how agents parse and rank information. This means an agentic marketplace creates a new kind of optimization target that did not really exist when humans were the only ones reading product pages.

**Limitations or Risks:**

1. The study evaluates agents under simulated marketplace conditions, so it is not fully clear how these biases would play out in a live, adversarial e-commerce environment where sellers are actively trying to manipulate agent behavior in real time.
2. The paper focuses on general-purpose shopping agents and does not test scenarios involving nutrition or dietary constraints, so I cannot assume the same bias patterns would appear identically when an agent is filling a grocery cart against a meal plan rather than picking a single product.

**Inspiration:**
The finding that agents overweight certain signals unless explicitly corrected makes me want to build an audit step into our grocery cart agent, where before checkout the agent has to justify each substitution or product choice against the meal plan's actual constraints such as calories, budget, and pantry contents, rather than trusting its own internal product ranking. This borrows the paper's diagnostic lens and turns it into a built-in safeguard for our system.

## ShoppingComp: Are LLMs Really Ready for Your Shopping Cart?

**Citation & Link:** Tou, H., Zeng, Y., Li, Y., Ma, C., Li, M., Yuan, W., Zhang, H., & Jia, K. (2025). ShoppingComp: Are LLMs really ready for your shopping cart? arXiv. [https://arxiv.org/abs/2511.22978](https://arxiv.org/abs/2511.22978)

**Summary:**

This paper tackles a very practical question for our project: can current LLMs actually be trusted to build a shopping cart on someone's behalf? The authors built a benchmark called ShoppingComp made up of 145 instances and 558 scenarios, written by 35 domain experts to reflect genuine shopping situations rather than toy examples. The benchmark tests three things at once: whether the agent can retrieve the right products, whether it can produce a useful, expert level explanation of its choices, and whether it makes safe decisions in scenarios that carry real consequences. Even the strongest models tested performed poorly, with GPT-5.2 scoring around 17.76% and Gemini-3-Pro around 15.82%, a lower result than I expected going in. The authors trace these failures to specific weaknesses: agents struggle to ground themselves in open-world product information, they have trouble verifying that a choice satisfies several constraints at once, they lose consistency when evidence conflicts, and they are not reliably risk aware when a decision could cause harm. The paper reads less like a victory lap for LLM agents and more like a diagnostic report on exactly where they still fall short.

**Key Insights:**

1. Multi-constraint verification is a named, measured weakness in current shopping agents, not just something I assumed would be hard. This matters directly for us because our grocery cart has to satisfy several constraints at once, including calories, budget, pantry contents, and dietary restrictions, and this paper gives me evidence that this exact task is where models tend to fail.
2. The gap between a model sounding confident and a model actually being correct becomes very visible once you build a benchmark that checks its work against expert judgment. A cart can look reasonable in an LLM's own explanation and still be wrong in a way an expert would catch right away.
3. Safety critical decision making is treated as its own evaluated skill, separate from plain accuracy. That distinction was useful for me because it reframes safety as a capability that has to be tested directly, the same way you would test retrieval or reasoning, rather than as an afterthought.

**Limitations or Risks:**

1. The benchmark is built around general online shopping scenarios, so grocery and meal based shopping, which involves recurring purchases and household context, may surface different failure modes than the largely one-off purchases the benchmark seems to emphasize.
2. The scenarios were authored by experts rather than drawn from real user logs, so there is a chance the benchmark captures what experts think is hard rather than the failure modes that actually show up when ordinary users interact with a shopping agent.

**Inspiration:** 

Given how poorly current models handle multi-constraint verification, I think our system should not rely on a single LLM call to both build and validate the cart. A safer design would split those responsibilities: one pass proposes the cart, and a separate, more constrained pass checks the proposed cart against the meal plan's explicit constraints line by line, closer to how ShoppingComp itself evaluates agents against a checklist rather than trusting one holistic judgment.