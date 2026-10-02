# AI Security Report: Supply Chain and Model Risks

## Executive Summary

This report explores how the security risks of AI systems are shaped long before a model ever reaches production. From the unaudited data that feed pre-training, to the inherited vulnerabilities carried through fine-tuning, to the simple fact that no one can look inside a trained model and see what it is actually doing, the risks stack up at every stage of the build process—long before anyone thinks to ask a security question.

With this foundation, security professionals and organizations are better equipped to ask the right questions when considering the adoption or deployment of AI: **Where did this model come from? What does it contain? And does anyone actually know?**

---

## Key Takeaways

### 1. Unaudited and Undocumented Training Data

AI models are drawn from poorly documented, unaudited sources. This means most organizations have no reliable answer to where their training data came from, what it contained, or whether it was tampered with.

**Key Questions to Ask:**
- Where did the training data originate?
- Was the data audited or verified by a trusted third party?
- What exactly did the dataset contain (e.g., PII, copyrighted material, malicious content)?
- Could the data have been tampered with at any point in the pipeline?
- Is there a documented chain of custody for the dataset?

---

### 2. Leaked Credentials Baked into Model Weights

Live credentials routinely end up baked into model weights through large-scale web scraping. Once the model is deployed, these credentials **cannot be patched out**, creating a persistent and irreversible security exposure.

**Key Questions to Ask:**
- Was the training data scanned for secrets, API keys, or credentials before training?
- If credentials were found, were they removed before the model was trained?
- Can the model be retrained or fine-tuned to remove leaked secrets, or is it permanently compromised?
- What is the exposure window if a leaked credential becomes public?
- Are there detection mechanisms in place to flag leaked secrets in future datasets?

---

### 3. Security Trade-offs in Model-Building Decisions

Model-building decisions such as **quantisation** and **federated learning** introduce security trade-offs that are rarely documented. As a result, organizations inherit unknown behaviour modifications alongside efficiency gains.

**Key Questions to Ask:**
- What optimizations (e.g., quantisation, pruning, distillation) were applied to this model?
- How do these optimizations affect the model's security posture and behaviour?
- Were the security implications of these decisions documented and reviewed?
- Does federated learning introduce new attack surfaces (e.g., poisoning from malicious clients)?
- Can the trade-offs be reversed or mitigated if they introduce unacceptable risk?

---

### 4. Inherited Risks Through Fine-Tuning

Fine-tuning a pre-trained model inherits everything beneath it: safety alignment erodes with as few as **10 adversarial examples**, and fine-tuned models are measurably **more susceptible to prompt injection** than their base counterparts.

**Key Questions to Ask:**
- What was the base model, and what is its security and safety history?
- What data was used for fine-tuning, and was it vetted?
- Could the fine-tuning process have weakened or removed safety alignments?
- How resistant is the fine-tuned model to prompt injection compared to its base?
- Has the fine-tuned model been tested against adversarial examples before deployment?

---

### 5. Opaque Model Weights and Voluntary Transparency

Trained model weights are fundamentally opaque. Security testing can only **sample behaviour** rather than audit it. Additionally, model cards—the primary transparency mechanism—remain voluntary, frequently incomplete, and sometimes absent entirely.

**Key Questions to Ask:**
- Is there a model card available, and is it complete and up to date?
- What behaviour has been tested, and what remains untested or unknown?
- Can the model's decision-making be explained or audited, or only sampled?
- Who is responsible for the model's behaviour once deployed?
- What transparency commitments has the provider made, and are they enforceable?

---

## Conclusion

The security risks of AI systems are not introduced at deployment; they are inherited from the very beginning of the build process. Understanding this supply chain is essential for any organization looking to adopt or deploy AI responsibly. Asking the right questions early—about data provenance, model composition, and transparency—is the first step toward meaningful AI security.

---

## Quick Reference: Questions Checklist

| Area | Critical Question |
|------|-------------------|
| Training Data | Where did the data come from, and was it audited? |
| Leaked Secrets | Were credentials scanned and removed before training? |
| Model-Building | What optimizations were applied, and what are their risks? |
| Fine-Tuning | Was safety alignment preserved, and is it resistant to injection? |
| Transparency | Is there a complete model card, and can behaviour be audited? |

---
