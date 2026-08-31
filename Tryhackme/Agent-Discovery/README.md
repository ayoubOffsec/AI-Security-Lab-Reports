## Architectural Comparison: System Control Models

To determine the correct automation level for NorthStar Fashion, we evaluate potential architectures based on **Control Ownership** and **LLM Integration**:

| Approach | Who Controls the Next Step? | Uses an LLM? | Ideal Security Use Case |
| :--- | :--- | :--- | :--- |
| **Traditional Workflow** | Fixed Code & Rules | No | Deterministic tasks (e.g., parsing logs, IP reputation lookup via API). |
| **AI Workflow** | Fixed Code & Rules | Yes | Fixed-sequence analysis requiring Natural Language Processing (e.g., log summarization). |
| **AI Assistant** | Human Engineer | Yes | Interactive query-answering and decision-support guided step-by-step by an analyst. |
| **AI Agent** | LLM Model (within strict guardrails) | Yes | Dynamic, multi-step evidence gathering with unpredictable investigation paths. |

### Walkthrough Questions & Solutions

* **Q1: Which approach uses fixed code and rules without an LLM?**
  * **Answer:** `Traditional workflow`
* **Q2: Which approach uses an LLM inside a code-controlled sequence?**
  * **Answer:** `AI workflow`
* **Q3: Which approach responds to engineer-led questions and requests?**
  * **Answer:** `AI assistant`
* **Q4: Which approach dynamically chooses the next approved step?**
  * **Answer:** `AI agent`

> **Architectural Note for Security Leadership:**
> Following industry engineering guidance (such as Anthropic's Agentic Design Principles), complex AI Agents should only be deployed when workflow steps cannot be predicted in advance. For SOC alert triaging, starting with deterministic workflows minimizes latency, cost, and the risk of cascading agent errors.
