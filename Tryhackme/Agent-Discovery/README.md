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

## Task 3: The Imperative of Agent Discovery

**Agent Discovery** is the essential analysis conducted prior to adopting any AI technology. It ensures that solutions address root-cause operational bottlenecks rather than forcing unnecessary AI complexity onto deterministic security operations.

### Core Discovery Principles

* **Problem-First Architecture:** Technology selection must follow a clear definition of the operational bottleneck, avoiding pre-determined tool choices.
* **Repetitive ≠ Agentic:** Repetitive tasks with structured inputs and deterministic outcomes (e.g., extracting IP addresses, timestamp sorting) should be automated via standard code rather than non-deterministic LLMs.
* **The Simplicity Principle:** The most effective system design is the simplest and safest one capable of delivering the required business value.
* **Isolation of Duties:** Investigation and evidence aggregation can be augmented by AI, but enforcement actions (e.g., account suspension, IP blocking) must remain strictly under human control.

---

### Walkthrough Questions & Answers

* **Q1: A system extracts IP addresses, sorts timestamps, and follows fixed rules. Which approach fits best?**
  * **Answer:** `Traditional workflow`
* **Q2: The engineer asks for an incident comparison but personally chooses the next step. Which system type is this?**
  * **Answer:** `AI assistant`
* **Q3: The first investigation tool returns weak evidence, so the model selects another approved tool. Which system type is this?**
  * **Answer:** `AI agent`

---

> **Architectural Note for Security Leadership:**
> Skipping the discovery phase exposes organizations to significant failure modes, including automating the wrong operational bottleneck, introducing AI-driven variance into rule-based tasks, and increasing operational expenditure. Recommending a non-agent solution (such as traditional automation or a structured AI workflow) is a successful discovery outcome when it provides a simpler, safer, and more predictable security control.
