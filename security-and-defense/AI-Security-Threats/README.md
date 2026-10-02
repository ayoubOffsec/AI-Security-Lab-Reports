# AI Security Threats

## Overview

This room explores the security threats introduced by Artificial Intelligence and how AI can be used by both attackers and defenders.

The room focuses on four main areas:

* Vulnerabilities specific to AI models.
* Existing attacks enhanced by AI.
* Defensive applications of AI in cybersecurity.
* Secure AI adoption and security standards.

---

## Prerequisites

This room builds on the concepts covered in:

**Room 1 — The Building Blocks of AI**

Recommended knowledge:

* Artificial Intelligence
* Machine Learning
* Neural Networks
* Deep Learning
* Large Language Models
* Transformers

---

# 1. AI-Specific Vulnerabilities

AI systems introduce vulnerabilities that can arise from how models are trained, instructed, deployed, and accessed.

The room focuses on five key vulnerabilities.

---

## Prompt Injection

**Prompt Injection** occurs when an attacker crafts input that overrides or bypasses the original instructions given to an AI model.

For example, an AI assistant may have a system prompt instructing it not to reveal internal information. An attacker may attempt to manipulate the model into ignoring those instructions.

Potential consequences include:

* Disclosure of sensitive information.
* Generation of unintended content.
* Behaviour outside the intended scope.
* Manipulation of an AI agent.

**Answer:** `Prompt injection`

---

## Data Poisoning

**Data Poisoning** occurs when an attacker manipulates the data used to train an AI model.

The objective is to influence the resulting model so that it produces incorrect, biased, or otherwise undesirable outputs.

Example:

```text
Legitimate Training Data
        +
Malicious / Manipulated Data
        ↓
      Training
        ↓
Compromised Model Behaviour
```

**Answer:** `Data poisoning`

---

## Model Theft

**Model Theft** occurs when an attacker obtains unauthorised access to an AI model or reproduces its behaviour.

One technique described in the room is repeatedly querying a model's API and using the resulting outputs to train another model that attempts to replicate the original.

```text
Target Model
     ↓
Repeated Queries
     ↓
Collected Outputs
     ↓
Training
     ↓
Model Clone
```

**Answer:** `Model theft`

---

## Privacy Leakage

**Privacy Leakage** occurs when an AI model unintentionally reveals sensitive information contained within its training data.

For example, a model trained using private records could potentially reveal information about individuals under certain prompting conditions.

Sensitive data therefore needs to be properly controlled throughout the AI training pipeline.

---

## Model Drift

**Model Drift** occurs when a model's performance gradually degrades because the environment or data it was trained on changes over time.

For example, a security model trained on historical network traffic may become less effective as attackers change their techniques.

Continuous monitoring is therefore important for deployed models.

**Answer:** `Model drift`

---

# 2. MITRE ATLAS

The room introduces **MITRE ATLAS**, a framework designed specifically to map tactics and techniques used against AI systems.

It serves a similar purpose to MITRE ATT&CK, but focuses on threats targeting AI-enabled systems.

**Answer:** `ATLAS`

---

# 3. Prompt Injection Practical Exercise

The room introduces **MENTOR**, an AI assistant belonging to the fictional company Syntara Corp.

MENTOR has a system prompt defining its behaviour and information it should not reveal.

The objective is to use prompt injection techniques to manipulate the assistant into revealing its system prompt.

This demonstrates a fundamental AI security concept:

```text
System Instructions
        ↓
      AI Model
        ↑
Malicious User Input
        ↓
Instruction Conflict
        ↓
Potential Unintended Behaviour
```

The important lesson is that natural-language instructions are not equivalent to traditional security boundaries.

---

# 4. AI-Enhanced Attacks

AI does not only introduce new vulnerabilities. It can also make existing attack techniques faster, cheaper, and easier to scale.

The room focuses on three examples:

* AI-generated malware
* Deepfakes
* AI-enhanced phishing

---

## AI-Generated Malware

Generative AI can produce functional code from natural-language instructions.

This can reduce the amount of time and expertise required to create or modify malicious software.

Attackers can potentially use AI to:

* Generate code.
* Modify existing malware.
* Iterate on code quickly.
* Customise malicious functionality.

The security concern is not that malware itself is new, but that AI can reduce barriers and increase the speed of development.

---

## Deepfakes

**Deepfakes** use AI to create convincing replicas of a person's appearance, voice, or both.

This creates a significant challenge for identity verification based primarily on seeing or hearing someone.

Example scenario:

```text
Attacker
   ↓
AI-generated voice of executive
   ↓
Employee receives voice message
   ↓
Fraudulent financial request
```

Deepfakes can therefore be used in social engineering and fraud.

**Answer:** `Deepfakes`

---

## AI-Enhanced Phishing

Phishing has traditionally relied partly on human mistakes and recognisable indicators such as:

* Suspicious links.
* Urgency.
* Poor grammar.
* Unnatural language.

Generative AI can produce fluent, contextually appropriate, and highly targeted phishing messages at scale.

This makes language quality a less reliable indicator of phishing.

**Answer:** `Phishing`

---

# 5. AI as a Defensive Capability

AI can also provide significant benefits to security teams.

The room identifies four main defensive applications:

1. Analysis
2. Prediction
3. Summarisation
4. Investigation

---

## Analysis

AI can analyse large quantities of security data and identify patterns or anomalies.

Examples include:

* Network traffic.
* Authentication behaviour.
* Process activity.
* Security logs.

The room mentions **Microsoft Defender for Endpoint** as an example of a security product leveraging AI for analysis.

**Answer:** `Microsoft Defender for Endpoint`

---

## Prediction

AI models can be trained using historical attack data to identify patterns associated with future threats.

For example, a model trained on large volumes of phishing messages can identify suspicious characteristics and help automate detection.

---

## Summarisation

Security incidents can produce large quantities of information:

* Logs.
* Alerts.
* Reports.
* Threat intelligence.
* Investigation notes.

LLMs can help security teams summarise this information and extract important findings.

This can reduce the time required to understand large amounts of security data.

---

## Investigation

LLMs can be provided with raw logs and asked to explain what happened, suggest queries, or assist with incident triage.

They can also support threat hunting by helping analysts think through possible attack scenarios.

**Answer:** `Investigation`

---

# 6. Practical Defensive AI Exercises

The room introduces **AEGIS**, an AI security assistant.

The exercises demonstrate several defensive use cases.

### Log Analysis

Example log:

```text
Jun 19 03:14:22 helix-fw01 kernel: [UFW BLOCK] IN=eth0 OUT= SRC=185.220.101.47 DST=10.0.0.5 PROTO=TCP DPT=22
```

The analyst can provide the log to AEGIS and request an analysis.

### Phishing Triage

Example:

```text
From: security@helix-financial-secure.com
Subject: Urgent: Unusual sign-in detected

We detected a sign-in on your Helix Financial account from Romania.
Verify your identity immediately or access will be suspended:
https://helix-financial-secure.com/verify
```

The analyst can ask AEGIS to assess the message and identify indicators of phishing.

### Incident Summarisation

The AI assistant can summarise the events observed during an investigation into a short leadership brief.

### Threat Hunting

The analyst can ask the AI what additional threats might exist based on the evidence already collected.

---

# 7. IBM Security Findings

The room references IBM's Cost of a Data Breach research to illustrate the potential defensive value of AI.

According to the information presented in the room:

* AI-assisted teams identified and contained breaches **108 days faster**.
* Organisations adopting AI saved an average of **$2.2 million per breach**.
* The average breach cost referenced was **$4.88 million**.

### Lab Answer

**Question:** According to IBM, how many days faster does AI help identify and contain breaches?

**Answer:** `108`

---

# 8. Secure AI Adoption

Using AI defensively does not eliminate the security risks introduced by AI.

The room states that only **24% of generative AI initiatives are currently secured**, according to the IBM research referenced in the lab.

This highlights an important principle:

> AI security must be considered from the beginning of the AI lifecycle.

---

## Access Control

AI systems should restrict who can interact with models and what actions users are allowed to perform.

The room recommends:

* Strong authentication.
* Strict permissions.
* Role-Based Access Control (RBAC).
* Multi-Factor Authentication (MFA).

**Answer:** `RBAC`

---

## Privacy Protection

AI systems may process sensitive information such as:

* Patient records.
* Internal communications.
* Customer information.

Training data should therefore be:

* Audited.
* Minimized where possible.
* Properly protected.
* Encrypted.

The training pipeline should treat sensitive AI data as a security-sensitive asset.

---

## AI Security Standards

The room introduces **ISO/IEC 27090** as a standard providing guidance for identifying and mitigating security threats specific to AI systems.

**Answer:** `ISO/IEC 27090`

---

## Model Monitoring

Monitoring deployed AI models is both a performance and security requirement.

Monitoring can help detect:

* Unexpected behaviour.
* Anomalous outputs.
* Statistical drift.
* Potential attacks.

The room also mentions explainability tools such as:

* **SHAP**
* **LIME**

These tools can help security teams better understand model behaviour.

---

# 9. Key Questions and Answers

| Question                                                      | Answer                            |
| ------------------------------------------------------------- | --------------------------------- |
| MITRE framework for AI threats                                | `ATLAS`                           |
| Vulnerability where user input overrides model instructions   | `Prompt injection`                |
| Attack involving manipulated training data                    | `Data poisoning`                  |
| Attack involving repeated API queries to create a model clone | `Model theft`                     |
| Gradual degradation as the environment changes                | `Model drift`                     |
| AI technique used to replicate someone's voice or appearance  | `Deepfakes`                       |
| Common initial access method enhanced by AI-generated content | `Phishing`                        |
| Microsoft security product mentioned in the room              | `Microsoft Defender for Endpoint` |
| Defensive capability involving LLM analysis of raw logs       | `Investigation`                   |
| Faster breach identification and containment                  | `108 days`                        |
| Percentage of generative AI initiatives described as secured  | `24%`                             |
| Recommended access-control model                              | `RBAC`                            |
| AI security standard mentioned                                | `ISO/IEC 27090`                   |

---

# 10. AI Security Threat Model

The concepts from the room can be organised into three broad areas:

```text
                    AI Security
                        │
        ┌───────────────┼────────────────┐
        │               │                │
        ▼               ▼                ▼
 AI-Specific       AI-Enhanced       Defensive AI
 Vulnerabilities     Attacks
        │               │                │
        ├─ Prompt       ├─ Malware      ├─ Analysis
        │  Injection    │               ├─ Prediction
        ├─ Data         ├─ Deepfakes    ├─ Summarisation
        │  Poisoning    │               └─ Investigation
        ├─ Model Theft  └─ Phishing
        ├─ Privacy
        │  Leakage
        └─ Model Drift
```

---

# 11. Key Takeaways

The main concepts from this room are:

1. AI introduces security vulnerabilities that are different from traditional software vulnerabilities.
2. Prompt Injection can manipulate an AI model into ignoring its intended instructions.
3. Data Poisoning targets the data used to train models.
4. Model Theft can involve reproducing a model's behaviour through repeated queries.
5. Privacy Leakage can expose sensitive information contained in training data.
6. Model Drift can reduce model performance as the surrounding environment changes.
7. MITRE ATLAS provides a framework for understanding threats against AI systems.
8. AI can enhance existing attacks such as malware generation, deepfakes, and phishing.
9. AI can also support defenders through analysis, prediction, summarisation, and investigation.
10. AI systems must be secured throughout their lifecycle.
11. Access control, privacy protection, standards, and model monitoring are important parts of AI security.

---

# 12. Security Perspective

The most important transition in this room is from **AI fundamentals** to **AI security**.

Traditional applications are generally governed by explicit program logic. AI systems introduce additional behaviour influenced by models, training data, prompts, context, and model outputs.

This creates new security questions:

* Can an attacker manipulate the model's instructions?
* Can training data be poisoned?
* Can sensitive training information be extracted?
* Can the model itself be stolen or replicated?
* Can model behaviour degrade over time?
* Can existing attacks become more effective through AI?
* Can AI safely be used as part of a defensive workflow?

These questions form the foundation for more advanced AI security testing.

---

# Conclusion

This room demonstrates that AI is both a new attack surface and a defensive capability.

From a security perspective, understanding AI vulnerabilities requires knowledge of how models are trained, how they process instructions, how they interact with data, and how they are deployed.

The next stage is to move from identifying AI security threats to understanding how these vulnerabilities can be practically tested and exploited.

