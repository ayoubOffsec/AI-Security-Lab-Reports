Markdown

# Safe AI Agent Design: SOC Alert Triage Specification
> **Prerequisites:** AI Agent Discovery (NorthStar Fashion Scenario)

---

## EXECUTIVE SUMMARY

Following the **Agent Discovery** phase, which established that a fully autonomous agent creates unnecessary operational risk, this report defines the **Safe Agent Design Specification** for NorthStar Fashion. 

Before any code, LLM framework, or API integration is selected, we establish strict system boundaries, tool constraints, state management rules, and Human-in-the-Loop oversight. This prevents non-deterministic behavior, authority overreach, and unexpected state manipulation.

---

## Task 1: Introduction to Agent Design Methodology

Before implementing an AI-assisted security system, establishing a precise **Agent Design Specification** is mandatory. Agent design defines the AI's exact operational behavior, scope, tool limits, and oversight requirements *before* any code, LLM framework, or API integration is selected.

### Why Agent Design Matters (Risk vs. Flexibility)

AI agents possess dynamic execution capabilities—they select tools, evaluate context, update state, and make multi-step decisions. Without a rigid design specification, this flexibility introduces serious security risks:

* **Scope Creep & Authority Overreach:** A poorly defined agent meant only for evidence gathering might accidentally be granted permissions to modify firewall rules, close alerts, or isolate endpoints.
* **Unconstrained Execution:** Missing stop conditions can cause the model to loop indefinitely or make unverified assumptions when evidence is lacking.
* **Lack of Accountability:** Without explicit human review points, non-deterministic model outputs can directly impact operational environments.

### Core Architectural Principles

1. **Vague Specification = Operational Risk:** Ambiguity in roles or tool boundaries leads to unpredictable execution paths and authorization risks.
2. **Support over Maximum Autonomy:** The objective is **not** to create a fully autonomous agent, but rather the simplest, safest AI architecture that reliably supports human analysts.
3. **Strict Separation of Duties:** Investigation and evidence aggregation can be augmented by AI, but containment and enforcement actions (e.g., blocking IPs, disabling user accounts, modifying security controls) must remain strictly under human control (**Human-in-the-Loop**).

---

### Walkthrough Questions & Answers

* **Q1: What defines agent behaviour before implementation?**  
  * **Answer:** `Agent design`
* **Q2: What do these questions reduce before implementation begins?**  
  * **Answer:** `Ambiguity`
* **Q3: What can a vague agent design create?**  
  * **Answer:** `Risk`

---

## Task 2: Defining Agent Role, Scope, and Boundaries

Establishing clear boundaries prevents authority creep and ensures the AI system operates solely as an **Investigation-Support Agent** rather than an autonomous containment system.

### Scope Boundary Breakdown

* **Workflow Start Boundary:** A new SIEM security alert is ingested and made available for triage.
* **Workflow End Boundary:** The agent delivers a structured investigation summary and priority recommendation to the analyst.
* **Strict Out-of-Scope (Prohibited):** Any remediation, containment, or system modification (e.g., firewall adjustments, account isolation, alert closure).

---

### Responsibility Matrix (Allowed vs. Disallowed Actions)

| Category | Permitted Agent Responsibilities (Allowed) | Prohibited Agent Responsibilities (Disallowed) |
| :--- | :--- | :--- |
| **Data Access** | Read SIEM details, extract entities (IP, User, Host), query approved threat APIs. | Modify, delete, or alter evidence logs. |
| **Analysis** | Correlate timeline events, search historical incident notes. | Invent or synthesize missing/unverified evidence. |
| **Execution** | Prepare structured markdown summaries, propose triage priorities. | Block IP addresses, disable accounts, close SIEM alerts automatically. |
| **Oversight** | Request human input when encountering ambiguous evidence. | Make final security decisions or execute automated containment. |

---

### Core Design Principle

> **Boundary Golden Rule:**  
> *"The agent supports the investigation, but the human owns the decision."*  
> Any architectural component or tool choice that permits the AI system to act beyond read-only investigation support must be **rejected**.

---

### Walkthrough Questions & Solutions

* **Q1: What describes what the agent is responsible for?**
  * **Answer:** `The role`
* **Q2: What should happen to a design choice if it allows the agent to act beyond investigation support?**
  * **Answer:** `Rejected`

---

## Task 3: Defining Safe Tool Boundaries

An agent's tools define its practical execution boundaries. While prompts define context, tools dictate real-world capabilities. Tool governance is therefore a fundamental security boundary rather than just a feature selection step.

### Candidate Tool Classification

We evaluate proposed tools against the **Minimum Safe Access** principle: *Can this tool modify state, disrupt users, or alter environment security controls?*

| Proposed Tool Name | Access Type | Boundary Classification | Security Justification |
| :--- | :--- | :--- | :--- |
| `get_alert_details` | Read-Only | **Inside (Approved)** | Safely retrieves core SIEM alert metadata. |
| `check_ip_reputation` | Read-Only | **Inside (Approved)** | Queries external threat intelligence APIs without environment impact. |
| `search_previous_notes`| Read-Only | **Inside (Approved)** | Pulls historical context from analyst knowledge bases. |
| `find_related_alerts` | Read-Only | **Inside (Approved)** | Correlates user/IP timeline data across multiple events. |
| `disable_account` | Action/Write | **Outside (Rejected)** | High operational risk; could lock legitimate users out. Requires human approval. |
| `close_alert` | Action/Write | **Outside (Rejected)** | High security risk; false negatives could suppress active breaches. Requires human approval. |

---

### The Safer Alternative Pattern (Recommendation vs. Execution)

To preserve functionality without delegating critical authority, high-impact tools are replaced with **Text-Based Recommendations**:

* **Instead of `disable_account`:** System outputs: `"High risk detected. Recommend engineer evaluation for account containment."`
* **Instead of `close_alert`:** System outputs: `"Low risk scanner activity detected. Recommend engineer review prior to alert closure."`

---

### Walkthrough Questions & Solutions

* **Q1: What extends agent capability?**
  * **Answer:** `Tools`
* **Q2: Should the agent be able to modify systems (yea/nay)?**
  * **Answer:** `nay`

---

## Task 4: Managing Context, State, and Memory Architecture

To prevent data leakage, hallucinated reasoning, and unexplainable outputs, an AI agent's information architecture must distinguish clearly between **Context**, **State**, and **Memory**.

### Information Boundary Taxonomy

| Dimension | Scope & Definition | Safe Design Implementation | Dangerous / High-Risk Design |
| :--- | :--- | :--- | :--- |
| **Context** | Information visible to the model during the **current execution run**. | Strict minimal data: Current alert fields, target IP reputation, active prompt. | Dumping raw system logs, unrelated PII/employee data, or entire historical databases. |
| **State** | Structured variables tracked **across multi-step investigation phases**. | Visible state tracking: `alert_id`, `evidence_checked`, `missing_evidence`, `requires_human_review`. | Hidden execution states where analyst cannot trace *why* a decision or priority was assigned. |
| **Memory** | Knowledge retained or queried **beyond a single run**. | Controlled retrieval from approved analyst notes, tagged explicitly as *supporting context*. | Storing data indefinitely, or treating past analyst notes as automatically 100% correct. |

---

### Core Design Principle: Evidence Visibility

> **Visibility Golden Rule:**  
> *"Provide enough information to support investigation, but keep the complete evidence trail visible."*  
> Every recommendation produced by the agent must be directly traceable to current context inputs or recorded execution state variables.

---

### Walkthrough Questions & Solutions

* **Q1: What tracks workflow progress?**
  * **Answer:** `State`
* **Q2: What information is available to the model during a specific run?**
  * **Answer:** `Context`

---

## Task 5: Human Oversight, Stop Conditions, and Safe Failure

An autonomous AI system must know when to cease execution. In security operations, an agent must safely halt whenever evidence is missing, tools fail, or authority limits are reached, ensuring human control remains uncompromised.

### Human Review vs. Stop Condition Boundary

| Boundary Type | Core Mandate | Operational Trigger Criteria |
| :--- | :--- | :--- |
| **Human Review Point** | `"A security engineer must evaluate and authorize this output."` | Contradictory evidence, high/critical risk alerts, potential account compromise indicators, or low confidence scores. |
| **Stop Condition** | `"The agent is strictly prohibited from executing past this node."` | Maximum retry limit reached, missing required evidence sources, API authentication failures, or out-of-scope user prompts. |

---

### Safe vs. Unsafe Behavioral Matrix

| Operational Scenario | Unsafe Behavior (Non-Compliant) | Safe Behavior (Architecturally Compliant) |
| :--- | :--- | :--- |
| **IP Reputation API Offline** | Assumes missing score means IP is benign/safe. | Marks evidence as `Unavailable` and triggers immediate **Human Review**. |
| **Conflicting Log Evidence** | Forces a deterministic guess without highlighting conflict. | Logs evidence contradiction in state and escalates to analyst. |
| **Persistent Tool Timeout** | Enters infinite retry loop, consuming API quota. | Halts execution after retry limits (e.g., 3 retries) and flags safe failure. |
| **Out-of-Scope Task Request** | Attempts to execute containment or firewall modifications. | Stops execution immediately; notifies analyst of authority boundary. |

---

### Core Design Principle: Safe Stop

> **Safe Stop Golden Rule:**  
> *"The agent may support investigation, but it must stop when evidence no longer supports safe continuation."*  
> Uncertainty is a valid, safe outcome. Revealing missing evidence and stopping execution is vastly superior to generating hallucinated confidence.

---

### Walkthrough Questions & Solutions

* **Q1: Who owns the final decision?**
  * **Answer:** `The engineer`
* **Q2: Should agents retry forever (yea/nay)?**
  * **Answer:** `nay`
* **Q3: What should missing evidence trigger?**
  * **Answer:** `Human review`

---

## Task 6: Structured Review Package and Output Validation Contract

Unstructured natural language outputs introduce verification bottlenecks. To guarantee reviewability, the agent's output must act as a rigid, schema-validated JSON contract containing explicit evidence provenance and controlled status fields.

### Controlled Value Enumerations (Enums)

To prevent the LLM from inventing non-standard or arbitrary severity labels, strict value boundaries are enforced:

```json
{
  "run_status": [
    "ready_for_review",
    "needs_human_input",
    "insufficient_evidence",
    "tool_failure",
    "out_of_scope"
  ],
  "recommended_priority": [
    "low",
    "medium",
    "high",
    "critical",
    "undetermined"
  ]
}
```
---


Walkthrough Questions & Solutions

    Q1: What must the agent use when the available evidence does not support a priority recommendation?

        Answer: undetermined

    Q2: What should not replace supporting evidence?

        Answer: Confidence

Practical Agent Specification Validation (Task 7 Overview)

In this practical challenge, we acted as the Safe Security Engineer to configure and bound an AI-assisted security agent for NorthStar Fashion.

Using the interactive Spec Builder tool, we evaluated multiple execution choices and categorized them into two distinct operational rules:

    Permitted & Safe Behaviors: Enforced boundaries that restrict the agent to read-only evidence gathering, clear state tracking, transparency regarding missing data, and mandatory human escalation whenever ambiguity arises.

    Prohibited & Unsafe Behaviors: Strictly rejected any option that granted autonomous operational authority, such as automatic account disablement, independent alert closure, guessing missing evidence, or hiding uncertainty from human analysts.

After enforcing these strict safety guardrails and submitting the specification, the design was approved without safety violations.
Walkthrough Questions & Solutions

    Q1: What is the flag revealed?

        Answer: THM{**************}
