# Architectural Comparison: System Control Models

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


## Task 4: Defining NorthStar Fashion's Business Problem

To ensure potential solutions target the real operational bottleneck rather than imposing unnecessary technology, we deconstruct the initial request into an evidence-based **Business Problem Statement**.

### Problem Categorization Breakdown

| Discovery Category | NorthStar Fashion Operational Context |
| :--- | :--- |
| **Symptom** | The engineer must review approximately 50 security alerts per day. |
| **Contributing Cause** | Evidence is fragmented across SIEM logs, IP reputation services, and past incident notes. |
| **Decision Difficulty** | SIEM severity levels are unreliable (false positives vs. hidden high-risk combined alerts). |
| **Operational Impact** | Manual triage consumes critical bandwidth required for infrastructure and software development. |
| **Proposed Solution (Tentative)** | *Build an AI Agent (Subject to architectural review in Task 6).* |

### Core Business Problem Statement

> **Defined Problem:**  
> NorthStar Fashion's sole engineer must manually gather, correlate, and interpret fragmented evidence across multiple security systems to triage ~50 daily alerts. Because individual SIEM risk scores can be inaccurate and contextual correlation (across timestamps and assets) is required to identify true risks, this manual triage process severely detracts from critical infrastructure and software development responsibilities.

---

### Walkthrough Questions & Answers

* **Q1: Approximately how many security alerts does NorthStar’s engineer review each day?**
  * **Answer:** `50`
* **Q2: How many total engineers handle security, infrastructure, and code responsibilities for NorthStar?**
  * **Answer:** `1`

---

> **Architectural Note for Security Leadership:**
> Framing the issue as "we need an AI Agent" focuses on technology adoption rather than operational risk. Defining the bottleneck around manual evidence gathering and contextual analysis enables us to evaluate deterministic code, AI workflows, or assistants objectively—ensuring the company does not over-engineer a simple workflow problem.


## Task 5: Mapping the Current Alert-Investigation Workflow

To evaluate where automation or LLM assistance is viable, we perform **Workflow Archaeology** to break down the manual investigation into discrete, measurable steps.

### Current-State Workflow Boundary

* **Start Boundary:** A new SIEM security alert becomes available for engineer triage.
* **End Boundary:** The engineer records the investigation conclusion and assigns a handling priority.
* **Out of Scope (Strictly Prohibited):** Containment actions (e.g., IP blocking, user isolation, firewall policy modifications).

---

### Task Breakdown Matrix

| Step # | Current Action | Input Required | Output Produced | Operational Nature |
| :--- | :--- | :--- | :--- | :--- |
| **1. Review Alert** | Read alert details & extract core facts | SIEM Alert | Extracted alert facts (IP, User, Host) | Deterministic / Rule-based |
| **2. Select Evidence Path** | Decide which evidence sources to check | Extracted alert facts | Investigation path plan | Contextual / Conditional |
| **3. Check IP Reputation** | Query external IP reputation engines | Source IP address | Reputation threat score | Deterministic / API Query |
| **4. Search History** | Query past documentation & notes | Alert facts & identifiers | Historical context records | Search & Summarization |
| **5. Find Related Alerts** | Correlate multi-event timelines | User, IP, Device, Timestamp | Event sequence timeline | Pattern Correlation |
| **6. Correlate Evidence** | Compare findings across all sources | Findings from Steps 3–5 | Synthesis assessment | High-Judgment Analysis |
| **7. Make Decision** | Apply security judgment (Deprioritize/Escalate) | Synthesis assessment | Priority / Action recommendation | High-Risk Human Decision |
| **8. Record Outcome** | Write investigation summary into logs/notes | Decision & supporting proof | Historical documentation record | Text Generation / Logging |

---

### Walkthrough Questions & Solutions

* **Q1: An alert contains an external source IP address. Which investigation step should the engineer consider?**
  * **Answer:** `Check IP Reputation`
* **Q2: An unusual-location login is followed ten minutes later by an external mail-forwarding rule for the same user. What should the engineer search for?**
  * **Answer:** `Related alerts`

---

> **Architectural Note for Security Leadership:**
> Mapping the current state demonstrates that the investigation is not a monolithic tasks. Steps 1, 3, and 5 are structured and deterministic (prime candidates for **Traditional Automation** or **AI Workflows**), while Steps 6 and 7 demand heavy contextual reasoning and human accountability.


## Interactive Simulation & Discovery Methodology Summary

Through the interactive **Agent Discovery Canvas** simulation, we performed a structured, step-by-step operational assessment to determine the true security automation needs for NorthStar Fashion. 

Rather than jumping straight to building an AI Agent, the simulation guided the evaluation through five core analytical stages:

1. **Business Problem Identification:** Deconstructed operational complaints to isolate real bottlenecks (~50 daily alerts, manual triage fatigue, SIEM severity inaccuracies) from premature technology-first assumptions.
2. **Workflow Graphing & Archaeology:** Mapped the complete 8-step investigation path from initial SIEM alert triggers to final outcome documentation, pinpointing dependencies and decision points.
3. **LLM vs. Deterministic Classification:** Evaluated each step to distinguish between rule-based tasks (log parsing, API IP checks) and tasks requiring natural language understanding (document search, contextual synthesis).
4. **Control Delegation Analysis:** Analyzed who controls the next step in each phase (Fixed Code, Human Engineer, or LLM Model) to balance operational flexibility against unpredictability and cost.
5. **Operational Boundaries & Guardrails:** Established clear system limits, strictly separating evidence aggregation/summarization from high-impact containment actions (such as blocking IPs or disabling accounts), which must remain exclusively under human control.

> **Key Discovery Outcome:**  
> The assessment successfully demonstrated that a fully autonomous **AI Agent** is unnecessary and introduces unwarranted risk. A **Hybrid AI Workflow + AI Assistant** architecture effectively resolves the operational bottleneck while preserving 100% human accountability over security decisions.

Q6: What is the lab completion flag?

    Answer: THM{*************} 
