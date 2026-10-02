# AI Security & Prompt Engineering Report

## Executive Summary

This report covers two critical areas of AI security: the **supply chain and model risks** that are inherited long before deployment, and the **prompt engineering fundamentals** needed to effectively and securely interact with Large Language Models (LLMs).

Understanding both is essential: the first reveals where AI systems inherit hidden vulnerabilities, while the second equips practitioners to pilot LLMs safely, control their behaviour, and recognise the boundaries that adversaries seek to exploit.

---

# Part 1: AI Supply Chain and Model Risks

## 1.1 Unaudited and Undocumented Training Data

AI models are drawn from poorly documented, unaudited sources. This means most organizations have no reliable answer to where their training data came from, what it contained, or whether it was tampered with.

**Key Questions to Ask:**
- Where did the training data originate?
- Was the data audited or verified by a trusted third party?
- What exactly did the dataset contain (e.g., PII, copyrighted material, malicious content)?
- Could the data have been tampered with at any point in the pipeline?
- Is there a documented chain of custody for the dataset?

---

## 1.2 Leaked Credentials Baked into Model Weights

Live credentials routinely end up baked into model weights through large-scale web scraping. Once the model is deployed, these credentials **cannot be patched out**, creating a persistent and irreversible security exposure.

**Key Questions to Ask:**
- Was the training data scanned for secrets, API keys, or credentials before training?
- If credentials were found, were they removed before the model was trained?
- Can the model be retrained to remove leaked secrets, or is it permanently compromised?
- What is the exposure window if a leaked credential becomes public?
- Are there detection mechanisms in place to flag leaked secrets in future datasets?

---

## 1.3 Security Trade-offs in Model-Building Decisions

Model-building decisions such as **quantisation** and **federated learning** introduce security trade-offs that are rarely documented. As a result, organizations inherit unknown behaviour modifications alongside efficiency gains.

**Key Questions to Ask:**
- What optimizations (e.g., quantisation, pruning, distillation) were applied to this model?
- How do these optimizations affect the model's security posture and behaviour?
- Were the security implications of these decisions documented and reviewed?
- Does federated learning introduce new attack surfaces (e.g., poisoning from malicious clients)?
- Can the trade-offs be reversed or mitigated if they introduce unacceptable risk?

---

## 1.4 Inherited Risks Through Fine-Tuning

Fine-tuning a pre-trained model inherits everything beneath it: safety alignment erodes with as few as **10 adversarial examples**, and fine-tuned models are measurably **more susceptible to prompt injection** than their base counterparts.

**Key Questions to Ask:**
- What was the base model, and what is its security and safety history?
- What data was used for fine-tuning, and was it vetted?
- Could the fine-tuning process have weakened or removed safety alignments?
- How resistant is the fine-tuned model to prompt injection compared to its base?
- Has the fine-tuned model been tested against adversarial examples before deployment?

---

## 1.5 Opaque Model Weights and Voluntary Transparency

Trained model weights are fundamentally opaque. Security testing can only **sample behaviour** rather than audit it. Additionally, model cards—the primary transparency mechanism—remain voluntary, frequently incomplete, and sometimes absent entirely.

**Key Questions to Ask:**
- Is there a model card available, and is it complete and up to date?
- What behaviour has been tested, and what remains untested or unknown?
- Can the model's decision-making be explained or audited, or only sampled?
- Who is responsible for the model's behaviour once deployed?
- What transparency commitments has the provider made, and are they enforceable?

---

# Part 2: Prompt Engineering

## 2.1 Understanding Tokens

LLMs don't read text the way humans do. When you type "Hello, how are you?", the model breaks it into **tokens**, the smallest units it can understand. A token is roughly 3-4 characters, so most English words are 1-2 tokens. Common short words like "the" or "cat" become single tokens. Longer or uncommon words are split into pieces: "ChatGPT" becomes "Chat" + "GPT". The model converts each token into a unique number (an ID) and only works with these numbers to predict what comes next.

**Key Questions to Ask:**
- What is the term for the smallest units that an LLM breaks text into?
- How many characters does a token roughly represent?
- Why do different models produce different token sequences for the same sentence?

---

## 2.2 Determinism vs Nondeterminism

Ask an LLM the same question twice and you'll likely get different answers. This is **nondeterminism**: outputs vary even with identical inputs. Unlike traditional software, where the same input always produces the same output, LLMs introduce randomness when predicting the next token. No setting eliminates it entirely.

This has massive security implications: code (like malware) executes the same way every time, but a defence may work on a malicious prompt one time and fail another.

**Key Questions to Ask:**
- What is the term for outputs varying even with identical inputs?
- Why is nondeterminism a challenge for AI security defences?
- Can randomness be eliminated entirely through parameters?

---

## 2.3 Controlling the Chaos: Parameters

You can control how the probability plays out using parameters:

### Temperature: The Randomness Dial
A numerical value commonly ranging from 0.0 – 2.0 that controls how "adventurous" the model is.

| Range | Behaviour | Use Case |
|-------|-----------|----------|
| 0.0 – 0.3 | Always picks the most probable token; closest to determinism | Code generation, data extraction, factual Q&A |
| 0.7 – 1.0 | Samples from a wider distribution; more variety | Brainstorming, storytelling, marketing |
| 1.2 – 1.5 | Coherence begins to break down | Experimental use only |
| 1.5+ | Low-probability tokens dominate | Avoid for most tasks |

### Max Tokens: The Length Limiter
Caps how long the response can be. One token roughly equals 0.75 English words. Common budgets:
- Quick answers: 50 – 150 tokens
- Detailed explanations: 500 – 1000 tokens
- Full articles: 2000+ tokens

Max tokens is a ceiling, not a target.

### Top-P: The Alternative Randomness Dial
Sets a shortlist of words that together account for a cumulative probability mass (e.g., 0.9 = 90% of likely options). Adjust temperature OR top-p, but not both.

### Context Window: The Memory Limit
The model's maximum "working memory" measured in tokens. Ranges from 8k (older GPT-3.5) to 1M+ (Gemini 1.5 Pro). Exceed it and the model silently truncates earlier context.

**Key Questions to Ask:**
- What parameter would you set to 0.0 to make an LLM behave as close to deterministic as possible?
- What parameter restricts which tokens the model considers by limiting selection to a cumulative probability mass?
- What term describes the maximum working memory of an LLM, measured in tokens?
- Why is it advised to adjust temperature OR top-p, but not both?

---

## 2.4 Four Pillars of Effective Prompts

| Pillar | Description |
|--------|-------------|
| **Instruction** | The core command or action you want from the AI, expressed with a clear verb |
| **Context** | Relevant background information or scenario so the AI understands the situation |
| **Output Format** | How you want the answer to look (bullet points, JSON, table, etc.) |
| **Constraints** | Rules or limits imposed on the response (tone, forbidden topics, length) |

**Specificity vs Verbosity:** Clear, specific prompts yield better results, but overly wordy prompts confuse the model. Aim for the sweet spot: enough detail to remove ambiguity, while keeping the prompt concise.

**Key Questions to Ask:**
- Which pillar instructs the model on how the answer should be structured?
- Which pillar specifies rules or limits imposed on the model's response?
- Which pillar provides the AI with relevant background information?
- Which pillar defines the core command or action you want the AI to perform?

---

## 2.5 System vs User Prompts

| | System Prompt | User Prompt |
|---|---|---|
| **Set by** | Developer / application | End user |
| **Nature** | Immutable, constant | Dynamic, session-specific |
| **Purpose** | Establishes identity, rules, safety boundaries | Carries task-specific requests |
| **Priority** | High-priority context | Acted on within system constraints |

**The Challenge:** LLMs process everything as text. Regardless of whether something is labelled "system" or "user", the model sees a single sequence of tokens. The boundaries exist through formatting conventions and training patterns, not hard architectural barriers.

**Key Questions to Ask:**
- What type of prompt is developer-defined, persistent, and remains constant across all sessions?
- What is the term for the intended order of priority between system and user instructions?
- Why is the instruction hierarchy considered a probabilistic security boundary rather than a guaranteed one?

---

## 2.6 Prompting Techniques

### The Shot Spectrum
- **Zero-shot:** No examples, relies entirely on pre-trained knowledge
- **One-shot:** A single example to clarify expectations
- **Few-shot:** 2-5 examples so the model recognises patterns (in-context learning)

### Chain-of-Thought (CoT)
Introduced by Google researchers in 2022, CoT asks models to break down complex tasks into intermediate steps. Add "Let's think step by step" for Zero-shot CoT.

### Prompt Templates
Standardised prompt structures for recurring tasks. Templates ensure consistency, reduce cognitive load, and bake in best practices.

**When to Use:**
| Technique | Best For |
|-----------|----------|
| Zero-shot | Simple, well-defined tasks |
| One-shot | Format clarification |
| Few-shot | Complex patterns, edge cases |
| Chain-of-Thought | Multi-step reasoning, security analysis |
| Templates | Repeatable tasks, team standardisation |

**Key Questions to Ask:**
- What is the term for the prompting technique introduced by Google researchers in 2022?
- What prompting technique involves providing no examples?
- What prompting technique involves saving and reusing a standardised prompt structure?
- What simple phrase can be added to trigger Zero-shot Chain-of-Thought reasoning?

---

# Part 3: Key Takeaways Summary

## AI Supply Chain Risks
1. AI is drawn from poorly documented, unaudited sources
2. Live credentials end up baked into model weights and cannot be patched out
3. Quantisation and federated learning introduce undocumented security trade-offs
4. Fine-tuning inherits vulnerabilities; safety erodes with as few as 10 adversarial examples
5. Model weights are opaque; model cards remain voluntary and often incomplete

## Prompt Engineering Fundamentals
1. Effective prompts follow four pillars: instruction, context, output format, constraints
2. System prompts set persistent rules; user prompts provide task-specific queries
3. Tokens are the fundamental units LLMs process (roughly 3-4 characters each)
4. LLMs are nondeterministic by nature; identical inputs can produce different outputs
5. Parameters like temperature, max tokens, and top-p control model behaviour
6. Techniques like few-shot, Chain-of-Thought, and templates level up prompting

---

## Quick Reference: Questions Checklist

| Area | Critical Question |
|------|-------------------|
| Training Data | Where did the data come from, and was it audited? |
| Leaked Secrets | Were credentials scanned and removed before training? |
| Model-Building | What optimizations were applied, and what are their risks? |
| Fine-Tuning | Was safety alignment preserved, and is it resistant to injection? |
| Transparency | Is there a complete model card, and can behaviour be audited? |
| Tokens | What are the smallest units an LLM processes? |
| Determinism | Can identical inputs produce different outputs? |
| Parameters | How do temperature, max tokens, and top-p control behaviour? |
| Prompt Pillars | What are the four pillars of effective prompts? |
| System vs User | Where is the boundary between system and user instructions? |
| Techniques | When should you use zero-shot, few-shot, or CoT? |

---

## Conclusion

The security risks of AI systems are not introduced at deployment; they are inherited from the very beginning of the build process. Understanding this supply chain is essential for any organization looking to adopt or deploy AI responsibly.

Equally important is understanding how to interact with these systems effectively. Prompt engineering is not just about getting better answers—it's about understanding the probabilistic, nondeterministic nature of LLMs and the soft boundaries that separate trusted instructions from untrusted input.

Together, these two areas form the foundation for meaningful AI security: knowing where your model came from, what it contains, and how to pilot it safely.

---

*Report generated based on room takeaways regarding AI supply chain, model security, and prompt engineering fundamentals.*
