#                                        Agent Foundations

## 1. Overview

This report documents the implementation and analysis of fundamental concepts used in Python-based AI agent workflows.

The assessment focused on understanding when an application actually requires agentic behaviour and how different frameworks can be used to build controlled and inspectable AI workflows.

The main technologies and concepts covered were:

* Plain Python LLM workflows
* LangChain tool calling
* Structured outputs
* Pydantic validation
* LangGraph state management
* Conditional routing and branching
* Agent failure detection and debugging

The central engineering principle throughout the assessment was to use the simplest workflow that safely solves the intended task.

---

## 2. Objectives

The assessment had several primary objectives:

* Understand what defines an AI agent.
* Differentiate fixed workflows from agentic workflows.
* Implement a basic Python LLM workflow.
* Integrate controlled tools using LangChain.
* Validate model responses using structured outputs.
* Manage workflow state using LangGraph.
* Implement conditional routing and branching.
* Identify and safely handle common agent failures.
* Select an appropriate framework based on workflow requirements.

---

## 3. AI Agent Architecture

An important distinction was made between a normal LLM workflow, automation, and an agentic workflow.

A basic LLM workflow follows a fixed execution path:

```text
Input
  ↓
Prompt
  ↓
LLM
  ↓
Output
```

An automated workflow may execute predefined functions or rules, while an agentic workflow can make runtime decisions about tool usage, state updates, and execution paths.

The increased flexibility of an agent also increases its complexity and security requirements. A system with more runtime freedom becomes more difficult to test, debug, evaluate, and secure.

### Key Observation

Not every application using an LLM needs to be implemented as an agent.

A fixed and predictable task can often be implemented more safely with standard Python code.

---

## 4. Plain Python LLM Workflow

The first workflow demonstrated a basic Python implementation without tools, state management, or branching.

The workflow performed four main operations:

1. Load a request.
2. Construct a prompt.
3. Send the prompt to the model.
4. Print and log the response.

The workflow was intentionally simple to demonstrate the underlying LLM interaction before introducing additional agent capabilities.

### Security Perspective

Reducing unnecessary components is beneficial from a security and reliability perspective.

A simple workflow has fewer:

* External dependencies
* Execution paths
* Tool interactions
* State-management issues
* Potential failure points

### Validation

The workflow was confirmed not to use tools and not to qualify as an agent because it followed a predetermined execution path.

The response style was controlled by the target audience.

---

## 5. LangChain Tool Calling

The next stage introduced controlled tool usage through LangChain.

LangChain provides abstractions for connecting an LLM with external capabilities through tools. In the assessment, the tools were designed to retrieve security-related records from controlled data sources.

Two main tools were implemented:

* `search_security_record`
* `list_security_records`

The tools validated their input before performing the requested operation.

For example, the CVE lookup function validates the supplied identifier before searching the security records.

### Tool Security

A critical security principle demonstrated here was:

> Never trust model-generated tool arguments.

The application remains responsible for:

* Validating tool input
* Restricting available tools
* Handling errors
* Controlling tool permissions
* Validating tool results

LangChain itself does not automatically make an agent secure.

---

## 6. Structured Outputs

Free-form LLM responses can be difficult for software to validate and consume reliably.

To address this, the workflow introduced structured responses using a Pydantic model.

The response schema contained fields including:

```text
request_id
summary
needs_tools
risk_level
next_action
confidence
```

The `StructuredResponse` model enforced valid values for fields such as `risk_level` and `next_action`.

For example, supported risk levels were:

```text
low
medium
high
```

and supported actions included:

```text
answer_directly
retrieve_sources
request_human_review
reject_request
```

This approach makes model output easier to validate, test, log, and pass to downstream components.

### Security Benefit

Structured validation reduces the risk of downstream components trusting malformed or unexpected model output.

An invalid value such as an unsupported risk level can be rejected before it is used by another component.

---

## 7. LangGraph State Management

The workflow was then extended using LangGraph.

LangGraph provides a graph-based approach where nodes perform specific operations and shared state carries information between them.

The workflow followed a sequence similar to:

```text
START
  ↓
load_request
  ↓
classify_request
  ↓
log_summary
  ↓
generate_answer
  ↓
END
```

The shared state included information such as:

```text
request_id
user_request
request_type
tool_results
errors
final_answer
```

State management provides visibility into what happened during execution and makes multi-step workflows easier to inspect and debug.

---

## 8. Conditional Routing and Branching

The next stage introduced conditional execution.

Instead of forcing every request through the same path, the workflow classified the request and selected an appropriate branch.

The available routes were:

```text
direct_answer
retrieve_sources
needs_human_review
reject_invalid_request
```

The routing logic mapped request classifications to these execution paths.

The resulting architecture was:

```text
                 ┌── direct_answer ───────┐
                 │                        │
START → CLASSIFY → ROUTER → retrieve_sources → FINALIZE → END
                 │                        │
                 ├── needs_human_review ──┤
                 │                        │
                 └── reject_invalid ──────┘
```

The assessment demonstrated that conditional routing makes execution decisions explicit and easier to inspect, test, and debug.

The sample requests were expected to exercise all four branches.

---

## 9. Framework Selection

The assessment compared three different implementation approaches.

| Requirement                                  | Recommended Approach |
| -------------------------------------------- | -------------------- |
| Fixed and predictable workflow               | Plain Python         |
| LLM with a small number of controlled tools  | LangChain            |
| Stateful workflow with routing and branching | LangGraph            |

Plain Python is appropriate when the task is deterministic.

LangChain becomes useful when the model needs access to a small collection of controlled tools.

LangGraph is more appropriate when explicit state, branching, routing, retries, approval points, or multi-step orchestration are required.

The key takeaway is that framework selection should be based on workflow requirements rather than simply choosing the most advanced technology.

---

## 10. Common Agent Failure Modes

The final stage focused on identifying common failures that can occur in agentic workflows.

The main failure categories were:

1. Invalid tool input
2. Missing state updates
3. Incorrect routing decisions
4. Weak structured-output validation
5. Hallucinated tool usage

These failures can be particularly problematic because an agent may produce an apparently reasonable response even though the underlying workflow behaved incorrectly.

---

## 11. Invalid Tool Input

The first failure involved sending malformed input to a security-record lookup tool.

A defensive implementation rejected the invalid CVE identifier rather than attempting to process it as a legitimate record.

The expected safe behaviour was:

```text
found = False
error = <validation error>
```

This demonstrates the importance of validating model-generated input before passing it to application logic.

### Security Impact

Without validation, malformed or hallucinated tool arguments could cause:

* Incorrect data retrieval
* Unexpected application behaviour
* Errors
* Misleading security conclusions

---

## 12. Missing State Updates

The second failure involved a workflow node calculating information but failing to return the updated state.

The broken implementation calculated the request classification but returned an empty dictionary.

As a result, downstream nodes continued to receive stale state.

The corrected implementation returned the relevant fields so they could be merged into the shared state.

This illustrates an important principle for stateful workflows:

> A calculated value is useless to downstream components if it is not persisted into workflow state.

---

## 13. Incorrect Routing

The third failure involved a router that always selected the `direct_answer` branch regardless of request content.

A safer implementation used the request classifier to determine whether a request should:

* Be answered directly
* Retrieve additional sources
* Require human review
* Be rejected

This prevents risky requests from accidentally being processed through an inappropriate branch.

The assessment specifically demonstrated that requests containing indicators such as attempting to bypass validation or fabricate citations should be routed to human review rather than automatically answered.

---

## 14. Hallucinated Tool Usage

The final failure involved a model-generated response claiming that a tool had been used even though no corresponding tool call existed in the execution log.

For example, a response could claim:

```text
I checked the security record...
```

without any actual tool invocation.

The detection mechanism compared the response against the tool-call log.

A tool should only be referenced in the final response when a corresponding execution record exists.

The corrected workflow performed the real tool call, recorded it, and generated the response using the actual returned data.

### Security Impact

This type of failure can create false evidence and misleading investigation results.

In security-oriented AI workflows, claims about retrieved evidence should therefore be tied to verifiable execution records.

---

## 15. Security Considerations

Several security principles were demonstrated throughout the assessment.

### Input Validation

All model-generated tool arguments should be validated before execution.

### Tool Restriction

Agents should only have access to explicitly approved tools.

### Output Validation

Model responses should be validated against a defined schema before being trusted downstream.

### State Integrity

Workflow state should be updated consistently so later stages operate on accurate information.

### Routing Controls

Requests should be routed according to deterministic rules and validated classifications.

### Execution Verification

The application should distinguish between what the model claims happened and what actually happened.

### Human Oversight

Requests that are risky, ambiguous, or outside the workflow's safe boundaries should be routed for human review.

---

## 16. Assessment Results

The workflow successfully demonstrated the following capabilities:

| Area                  | Result    |
| --------------------- | --------- |
| Basic LLM workflow    | Completed |
| Tool calling          | Completed |
| Tool input validation | Completed |
| Structured outputs    | Completed |
| Pydantic validation   | Completed |
| LangGraph state       | Completed |
| Conditional routing   | Completed |
| Branching             | Completed |
| Failure detection     | Completed |
| Failure remediation   | Completed |

The debugging stage confirmed four major failure modes as detected and fixed:

* Invalid tool input
* Missing state update
* Wrong routing decision
* Hallucinated tool usage

---

## 17. Key Findings

The assessment demonstrated that AI-agent security depends heavily on the surrounding workflow rather than only the underlying language model.

The most important findings were:

1. **Not every LLM application requires an agent.**
2. **Tool arguments must be treated as untrusted input.**
3. **Structured outputs improve validation and reliability.**
4. **Shared state is essential for multi-step workflows.**
5. **Explicit routing makes agent behaviour easier to inspect.**
6. **Tool usage should be verifiable through execution logs.**
7. **Human review provides an important safety boundary for risky requests.**
8. **Additional framework complexity should only be introduced when required.**

---

## 18. Conclusion

The assessment provided a practical foundation for designing controlled Python-based AI agents.

The progression from a basic Python workflow to LangChain tool calling and finally LangGraph stateful and branching workflows demonstrated how additional capabilities also introduce additional security and engineering responsibilities.

A reliable agent should therefore not be designed around maximum autonomy. Instead, it should use the minimum level of complexity required to safely accomplish its task.

The final principle can be summarized as:

```text
Simple task
    ↓
Plain Python

LLM + controlled tools
    ↓
LangChain

State + routing + branching + orchestration
    ↓
LangGraph
```

The overall objective is not to build the most complex agent possible, but to build a workflow that is **controlled, observable, testable, and safe**.
