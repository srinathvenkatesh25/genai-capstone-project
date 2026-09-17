# Reflections — Chantrice Santiago

## Planning With Multi-Constraints via Collaborative Language Agents

**Full Citation & Link:**
Zhang, C., Deik, D. G. X., Li, D., Zhang, H., & Liu, Y. (2024). Planning with multi-constraints via collaborative language agents [Preprint]. arXiv.[https://arxiv.org/abs/2405.16510](https://arxiv.org/abs/2405.16510)

### Structured Summary:
**Research problem**: The paper examines the difficulty LLM agents face when planning complex real-world tasks that involve multiple local and global constraints, dependencies, and heterogeneous actions.

**Methodology**: The authors introduce Planning with Multi-Constraints (PMC), a zero-shot collaborative multi-agent framework in which a manager agent decomposes a complex task into a hierarchy of interconnected subtasks, executor agents translate those subtasks into actions, a supervisor refines them, and a deliverer produces the final result. The framework was evaluated using TravelPlanner, which tests constraint-intensive itinerary planning, and API-Bank, which tests planning and execution across tools and APIs.

**Main findings**: PMC achieved an average success rate of 42.68% on TravelPlanner, substantially outperforming the 2.92% GPT-4 baseline, and exceeded GPT-4 with ReAct on API-Bank by approximately 13.6 percentage points. The findings demonstrate that role specialization, hierarchical task decomposition, collaboration, and intermediate refinement can improve multi-constraint planning, although the current framework still requires some human input when configuring executor agents.

### Three Key Insights:
1. #### Hierarchical planning with dependency-aware supervision improves reliability
The system does not ask one model to solve every part of the problem at once. The manager focuses on overall structure, while executors focus on individual tasks. This resembles organizational delegation: a manager coordinates the project but does not personally perform every technical action.

However, it is not enough to divide a problem into separate tasks. The system must understand which tasks depend on others. If tasks are performed in the wrong order or in parallel when one requires another’s output, the system may produce incompatible results.

This is where intermediate supervision can prevent downstream failure. The supervisor allows the system to revise instructions based on earlier outputs. This is valuable because complex planning is dynamic. A result from one stage may change what another stage should do.

2. #### Stronger architecture can sometimes compensate for weaker models

The paper’s results indicate that carefully designed agent coordination can improve performance even when the planning core uses a smaller LLM. Therefore, system architecture may matter as much as model size.

3. #### Better performance is not the same as reliability
Although PMC dramatically outperformed the baseline, its TravelPlanner success rate remained below 50%. Complex constraint satisfaction is still difficult, and important outputs should not be accepted without deterministic checks or human review.

### Two Limitations or Risks:

1. **Error propagation from incorrect task decomposition**: PMC depends heavily on the manager agent correctly breaking the overall problem into subtasks and identifying their dependencies. If the manager creates an incomplete or incorrect plan or if an early executor produces inaccurate information, these errors can propagate through the remaining agents and affect the final output.

2. **Limited reliability and increased system complexity**: Although PMC substantially outperformed the baseline, it successfully completed only 42.68% of TravelPlanner tasks, showing that multi-agent planning is still unreliable for complex constraint-heavy problems. The framework also requires multiple agents, tool calls, and some human input when configuring executors, which increases cost, latency, and implementation complexity.

### One Concrete Inspiration:

A hierarchical planning workflow with intermediate supervision: Our project can use a manager agent to divide the user’s request into dependent subtasks such as meal selection, nutritional calculation, ingredient optimization, and grocery-cart creation, while specialized agents complete each stage. A supervisor or validation layer can review intermediate outputs and send a task back for revision if it violates nutritional targets, allergies, budget, cooking-time limits, or ingredient-reuse requirements before the final plan reaches the user.

## A Survey on LLM-Based Multi-Agent Systems

Chen, S., Liu, Y., Han, W., Zhang, W., & Liu, T. (2024). A survey on LLM-based multi-agent systems: Recent advances and new frontiers in application [Preprint]. arXiv.[https://arxiv.org/abs/2412.17481](https://arxiv.org/abs/2412.17481)

### Structured Summary:
**Research problem**: The paper addresses the need for an updated and comprehensive framework for understanding the rapidly expanding research on large language model-based multi-agent systems (LLM-MAS).

**Methodology**: The authors conduct a structured survey of prior studies and organize them according to three major application areas: solving complex tasks, simulating specific scenarios, and evaluating generative agents. They also identify the core components of an LLM-MAS, including agent profiles, reasoning, memory, planning, actions, communication mechanisms, environmental rules, tools, and intervention methods.

**Main findings**: The review indicates that specialized agents can collaborate through multi-stage reasoning, collective decision-making, and self-refinement to perform tasks that may be difficult for a single agent. However, these systems continue to face challenges involving communication costs, privacy disclosure, accumulated errors, hallucinations, limited long-context capabilities, inefficient scaling, and the absence of standardized evaluation benchmarks.

### Three Key Insights:
1. #### More Agents Do Not Automatically Improve Performance
Using more agents does not automatically lead to better performance. A smaller number of clearly specialized agents can sometimes be more efficient, reliable, and easier to evaluate. Each additional agent introduces communication costs, latency, and another opportunity for hallucinations, biases, or errors to enter the workflow.

2. #### The Interaction Structure Shapes System Performance
In parallel collaboration, multiple agents solve or evaluate the same task simultaneously. This can introduce diverse perspectives, reduce dependence on one response, and support comparison or consensus. However, it increases computational costs and may create false confidence when agents built on similar models repeat the same biases or errors.

In sequential collaboration, agents pass outputs from one stage to the next. This allows for clear role specialization and an organized workflow, but errors or biases introduced by an earlier agent can propagate and become amplified when downstream agents treat its output as reliable.

3. #### Multi-Agent Architecture Is a System-Design Decision
Building on the first two insights, multi-agent architecture should be treated as a system-design problem, not merely a prompting technique. The system should be designed around the task’s actual needs, with carefully defined roles, appropriate interaction structures, and independent validation checkpoints. Adding agents without a distinct purpose may only increase costs, latency, and opportunities for failure.

### Two Limitations or Risks:

1. **Error and bias propagation**: In sequential workflows, an incorrect or biased output from one agent may be accepted by downstream agents and amplified throughout the system. Parallel agents may also produce correlated errors when they rely on similar models, creating false confidence through apparent agreement.

2. **Efficiency and communication overhead**: Adding more agents increases the number of model calls, communication steps, computational costs, and processing time. Poorly designed interaction structures may make the system more complex without meaningfully improving its performance.

### One Concrete Inspiration:

Specialized agents with structured handoffs and validation checkpoints: The project can use a small set of agents, such as a Meal-Planning Agent, Optimization Agent, Shopping Agent, and Coaching Agent, with clearly defined responsibilities. Each agent would pass structured outputs to the next stage, while deterministic validation checkpoints verify nutritional targets, allergies, budget, and ingredient availability before the workflow proceeds.
