# AI Forensics (DFIR) Report

## Executive Summary

The world of Digital Forensics is full of pieces needing to be connected, often under a time constraint. This can be a challenging task, but one that many forensics analysts have accomplished without the need for tools. This begs the question: Does the digital forensics industry need the AI tools being used by so many other adjacent industries?

This report explores the potential application of AI in digital forensics, the challenges that come with that, and the potential ethical and legal implications.

---

## Learning Objectives

- Understand the day-to-day challenges faced in DFIR
- Understand how AI can be used to address those challenges
- Understand the challenges that arise when you use AI for forensics investigations and the ethical/legal implications
- Understand how AI impacts the DFIR investigation process

**Key Questions to Ask:**
- What are the day-to-day challenges faced in DFIR?
- How can AI be used to address those challenges?
- What ethical and legal implications arise when using AI for forensics?
- How does AI impact the DFIR investigation process?

---

# Part 1: How AI Enhances DFIR

Many of the abilities of AI/ML discussed in the AI/ML Security Threats room lend themselves to solving the challenges faced in DFIR. Consider the following examples:

| Capability | How It Helps DFIR |
|------------|-------------------|
| **Data Processing** | Transformer models process entire bodies of text in parallel (often in milliseconds), providing insights and classifications on processed data |
| **Anomaly Detection** | ML algorithms learn what "normal" behaviour looks like for users, systems, and networks, identifying potential anomalies that could indicate malicious behaviour — turning the haystack into a handful of hay |
| **Scalability** | AI systems scale effortlessly, processing millions of events across cloud, hybrid, and remote environments, enabling DFIR teams to cover more ground without proportional workload increase |

**Key Questions to Ask:**
- What ability of AI helps turn a DFIR investigator by recognising patterns they might not have been able to comprehend?
- What AI capability helps process vast amounts of data in parallel?
- What AI capability helps scale across cloud, hybrid, and remote environments?

---

# Part 2: AI in the Wild — Practical DFIR Applications

AI/ML has already been heavily adopted into the DFIR landscape. Here is how AI/ML is being applied practically in the field:

| DFIR Task | What AI/ML Enables | Example Tools |
|-----------|-------------------|---------------|
| **Anomaly Detection / UEBA** | Flags unusual user/system behavior compared to learned "normal" | Elastic ML, Exabeam |
| **Phishing & Communication** | Detects phishing emails and flags risky language in chat/email logs | Microsoft Defender for O365, Splunk NLP |
| **Malware / File Classification** | Classifies files as malicious or benign based on extracted static/dynamic features | Microsoft Defender (STAMINA), Cylance, VirusTotal ML |
| **Alert Triage & Prioritisation** | Automatically scores, ranks, and filters alerts to reduce analyst workload | Cortex XSOAR/XSIAM, IBM QRadar Advisor, CrowdStrike Falcon + Charlotte AI |
| **Timeline & Event Correlation** | Reconstructs attack timelines by clustering and linking logs across sources | Timesketch, Velociraptor, Jupyter-based analysis |

**How AI Solves It:**
- **Anomaly Detection:** Unsupervised learning (Isolation Forests, Autoencoders) learns baseline behaviour; deviations are flagged as potential threats
- **Phishing Detection:** Transformer-based models (BERT, RoBERTa) classify messages as phishing or benign based on tone, structure, and known attack patterns
- **Malware Classification:** AI analyses file metadata, code signatures, and behaviour to detect threats
- **Alert Triage:** AI analyses past alert data, analyst feedback, and incident outcomes to rank alerts by severity and relevance
- **Timeline Correlation:** AI clusters similar log events, identifies causal relationships, and aligns activity across systems

**Key Questions to Ask:**
- What ability of AI helps turn a DFIR investigator by recognising patterns they might not have been able to comprehend?
- Which metric tells you the proportion of positively flagged results that were actually correct?
- What term describes the AI characteristic where the same input may yield different outputs across different runs?

---

# Part 3: AI Limitations in DFIR

Understanding the limitations of AI is fundamental knowledge if you are going to effectively utilise it, especially in DFIR.

## 3.1 Probabilistic vs Deterministic

Traditional software and algorithms are **deterministic**: if you provide them with the same input, they will always provide you with the same output. Consider a calculator function provided with the sum 5 + 5; it will always return 10.

However, AI systems and modern machine learning models are generally **probabilistic**: instead of following a fixed ruleset that provides a fixed outcome, they use statistical models to learn from data and make predictions with certain possibilities.

**Why This Matters for DFIR:**
- Digital forensics demands consistent and repeatable results
- Non-determinism can lead to different timelines being reconstructed from the same input data
- Prompt sensitivity means vastly different outputs can be generated with a slight tweak to the input
- Extensive prompt engineering is required when using AI in evaluative contexts

**Key Questions to Ask:**
- What term describes the AI characteristic where the same input may yield different outputs across different runs?
- Why is non-determinism a challenge for digital forensics?
- What is required when using AI in evaluative contexts?

---

## 3.2 Accuracy vs Precision vs Recall

When using AI to help with a task as important as identifying and analysing potential evidence, you must be able to assess the performance of the model.

| Metric | Definition | Risk in Isolation |
|--------|------------|-------------------|
| **Accuracy** | Overall rate of correct predictions | Misleading with imbalanced data (99% accuracy by predicting majority class) |
| **Precision** | How often positive predictions are correct | Model can be too selective, missing malicious files |
| **Recall** | How many actual positives were identified | Model can cast a wide net, flagging many false positives |

All three metrics must be considered together for a true picture of model performance.

**Key Questions to Ask:**
- Which metric tells you the proportion of positively flagged results that were actually correct?
- What metric measures the overall rate of correct predictions?
- What metric measures how many actual positives were identified?
- Why can accuracy be misleading in digital forensics?

---

## 3.3 Garbage In, Garbage Out (GIGO)

The GIGO principle is just as true for AI systems (if not more) than it is for any system. The quality of an AI's output is directly determined by the quality of its input. If an AI has been trained on "bad" data, it will lead to the model confidently asserting false predictions as though they were fact. When using this technology to pursue justice, this calls for extra caution.

**Key Questions to Ask:**
- What does GIGO stand for?
- Why is GIGO especially important in digital forensics?
- What happens when an AI is trained on "bad" data?

---

# Part 4: AI Applications Across DFIR Domains

## 4.1 Image and Video Forensics

Digital image and video forensics is an excellent example of AI/ML capabilities making our lives in DFIR easier.

### CNN-Based Forgery Detection
Researchers have combined traditional forensics methods such as **ELA (Error Level Analysis)** with CNN models to identify image tampering. A 2024 study proposed an ELA+CNN framework achieving **94% accuracy**.

### Deepfake Detection
CNN models are used in conjunction with other AI technologies to develop specialised detectors that analyse subtle inconsistencies in facial videos, achieving state-of-the-art accuracy.

### GANs (Generative Adversarial Networks)
A setup where two neural networks compete: one generates fake media, the other tries to detect it. As they battle, both improve. GANs are used both offensively (creating deepfakes) and defensively (training detectors).

**Key Questions to Ask:**
- What type of neural network is commonly used in image and video forensics due to its ability to learn spatial patterns in visual data?
- What does ELA stand for?
- What accuracy rating did the ELA+CNN framework achieve?
- What are GANs used for in image and video forensics?

---

## 4.2 Communication Analysis

Communication analysis involves the processing and analysis of large volumes of text.

### Phishing Email Detection
Transformer-based models trained for **NLP (Natural Language Processing)**, such as BERT and RoBERTa, excel at identifying phishing emails. A study found they achieved **99% accuracy** in classifying phishing emails against legitimate ones.

### Chat Log and Social Media Analysis
The same technology is harnessed by forensic platforms, allowing investigators to automatically scan chats for keywords or patterns related to threats and perform **sentiment analysis** to gauge emotional tone.

**Key Questions to Ask:**
- What kind of analysis can be performed on social media or chat logs to assess the emotional tone of messages?
- What accuracy did transformer-based models achieve in classifying phishing emails?
- What NLP models are mentioned as excelling at phishing detection?

---

## 4.3 Timeline Reconstruction and User Behaviour

Reconstructing incident timelines is a common and critical part of an investigation; it is also very labour-intensive and time-consuming.

### Automated Event Timeline Reconstruction
AI systems correlate **time-sequenced** data from multiple sources and put together what happened before, during, and after an incident. ML algorithms can ingest logs, filesystem timestamps, network records, etc., and automatically build a chronological timeline.

### Anomaly Detection
AI is incredibly good at identifying patterns. In DFIR, this ability can flag things like impossible logins (where a user was logged in at two places simultaneously) or behaviour unusual for a specific user. It can also determine what constitutes "normal" behaviour for a web application.

**Key Questions to Ask:**
- What type of data do AI systems correlate to reconstruct the timeline of an incident automatically?
- What is an example of an anomaly that AI can flag in user behaviour?
- How can anomaly detection be applied to web applications?

---

## 4.4 Malware Detection/Analysis

AI/ML has lent itself greatly to malware detection and analysis.

### Static Analysis
Breakthroughs in representing malware files in ways processible by deep neural networks have made it possible to classify a file as malicious or benign (e.g., Microsoft and Intel's **STAMINA** project).

### Dynamic Analysis
ML is being considered for use in **dynamic analysis**, observing how a program behaves to identify whether it is malicious. Research has been done on converting a program's call sequence into a 2D image and then classifying it.

### Antivirus and EDR
Using some form of AI/ML is now very common in antivirus and **endpoint detection response (EDR)** products.

**Key Questions to Ask:**
- What type of analysis observes how a program behaves to determine whether it is malicious?
- What project by Microsoft and Intel is mentioned for malware classification?
- How can a program's call sequence be represented for classification?
- What products commonly use AI/ML for malware detection?

---

# Part 5: Ethical and Legal Implications

AI is increasingly being woven into DFIR, enabling investigators to save time and gain deep insights. However, these advancements raise complex legal and ethical questions.

## 5.1 Explainability and Transparency

Many AI models are "**black boxes**", meaning they don't readily explain how they came to a conclusion. This clashes with a core tenet of forensics analysis: the need for transparency and defensibility of evidence interpretation.

**Example:** In one documented civil litigation case, an AI had been used to flag certain emails as "suspicious", but when opposing counsel demanded to know why, the legal team could not explain the model's reasoning. As a result, the AI-generated evidence was excluded by the court.

**The Daubert Test:** A U.S. legal rule that determines the admissibility of expert testimony, particularly scientific testimony, in federal court.

**Key Questions to Ask:**
- What legal test used in the U.S. assesses whether expert or scientific testimony is admissible in court?
- What term describes AI models whose internal decision-making processes are difficult to interpret?
- Why was AI-generated evidence excluded in the documented civil litigation case?

---

## 5.2 Bias and Fairness

AI systems can unintentionally introduce bias. Models are trained on historical data; if that data contains skewed representations or prejudices, the model's output will reflect them.

**Real-World Example:** Facial recognition technology used by police has been found to misidentify Black and other minority individuals at much higher rates than white individuals. In the U.S., there are at least **seven known wrongful arrests** due to faulty face recognition, and almost every victim was an African American mistakenly identified by AI.

**Implications:**
- **Legally:** If a defence can show an AI technique is biased, judges may exclude its results
- **Ethically:** Forensic experts have a duty to validate and correct biases present in AI tools

**Key Questions to Ask:**
- What real-world technology used by law enforcement has been shown to produce racially biased results in identifying suspects?
- How many known wrongful arrests in the U.S. have been linked to faulty face recognition?
- What is the ethical duty of forensic experts regarding AI bias?

---

## 5.3 Accountability and Chain of Custody

Courts require that digital evidence be handled in a traceable and preservable manner, with integrity preserved at each step. This is achieved by maintaining the **chain of custody** and an **audit trail**.

**The Challenge:** Many AI tools (especially cloud-based services) operate opaquely, clashing with these requirements.

**Example:** An LLM was used to summarise a suspect's mobile phone data, which inadvertently violated the chain of custody (due to intermediate AI outputs not being logged), causing the defence to challenge the forensic findings on procedural grounds.

**Solution:** AI processes must be carefully documented and secured. Using on-premises or controlled systems can help satisfy legal scrutiny.

**Key Questions to Ask:**
- What is required to maintain the integrity of digital evidence in court?
- How did an LLM violate the chain of custody in the documented case?
- What solution is recommended for AI processes to satisfy legal scrutiny?

---

## 5.4 Privacy and Data Protection

AI models thrive on large datasets, which can trigger privacy and legal compliance issues. Public cloud servers may inadvertently expose sensitive evidence to third-party servers, violating privacy laws or court orders. Legal frameworks like **GDPR** may restrict how personal data is processed, even for law enforcement purposes.

**Solutions:**
- Running AI tools in secure offline environments
- Using **federated learning**

**Key Questions to Ask:**
- What technique allows machine learning to be performed without transferring sensitive data to a central server, helping preserve privacy?
- What legal framework is mentioned as restricting how personal data is processed?
- What happens if AI systems use personal data without proper authority?

---

# Part 6: Lab Walkthrough — The Digital Trail

## 6.1 Case Summary

**Client:** RobbCo, founded by Robb House — a titan in software and automation, famous for system firmware and terminal operating systems.

**The Case:** A member of the team awoke founder Robb House to report a suspected breach, citing a security system that flagged an off-the-clock login and other suspicious behaviour.

**The Damage:** Proprietary code for RETROS (low-level firmware), MF Boot Agent (secure and programmable bootloader), and Unified Operating System (UOS) was potentially accessed.

## 6.2 Investigation Setup

The lab uses **scikit-learn**, an open-source Python library providing simple, efficient data mining and machine learning tools.

**Tools Used:**
- `classify_logs.py` — AI model trained on labelled log data to spot suspicious anomalies
- `file_anomalies.py` — Model trained on high-priority directories, considering file name, path, size, extension, entropy, permissions, and creation time

## 6.3 Investigation Phases

### Phase I: Initial Access

| Artefact | Behaviour | Impact/Analysis |
|----------|-----------|-----------------|
| `/tmp/invoice_dump.txt` | Stores collected recon data | Reveals prior SSH usage, usernames, and active sessions |
| `/home/j.morgan/Documents/Invoices/invoice_Q1_2075.ods` | Embedded macro executes shell commands | Harvests .bash_history, SSH keys, user sessions; attempts exfiltration to 192.168.0.100 |

**Attack Chain:**
1. Phishing email sent to j.morgan
2. Malicious .ods file opened, triggering data harvesting
3. Data saved to /tmp/invoice_dump.txt and exfiltrated
4. Attacker logs into j.morgan using collected credentials

**Key Questions to Ask:**
- At what time does the attacker successfully log in as j.morgan?
- What attack method was used to gain initial access?
- Can you find the attacker's email address?

### Phase II: Tooling and Infrastructure

| Artefact | Behaviour | Impact/Analysis |
|----------|-----------|-----------------|
| `/tmp/.syncd` | Connects to http://10.0.0.66/payload.sh and executes it | First-stage dropper for second-stage download |
| `/tmp/.x` | Reverse shell stub | Establishes remote shell to 10.0.0.66:4444 |

**Key Questions to Ask:**
- What does `/tmp/.syncd` connect to?
- What port does the reverse shell connect to?

### Phase III: Privilege Escalation

| Artefact | Behaviour | Impact/Analysis |
|----------|-----------|-----------------|
| `/home/j.morgan/.bash_history` | Reveals use of sudo to modify SSH keys | SSH key planted in r.house's authorized_keys |

**The Command:** `sudo nano /home/r.house/.ssh/authorized_keys`

**Key Questions to Ask:**
- What command did the attacker run as j.morgan to gain access to the r.house account?
- What technique was used to escalate privileges?

### Phase IV: Disguise and Persistence

| Artefact | Behaviour | Impact/Analysis |
|----------|-----------|-----------------|
| `/usr/local/bin/sysmon` | Outbound connection to 10.0.0.66:5555 | Reverse shell disguised as system monitoring tool |
| `/opt/robbco/sys/boot_monitor.log` | Fake boot telemetry logs | Justifies sysmon's presence |

**Key Questions to Ask:**
- What is the purpose of `/usr/local/bin/sysmon`?
- What is the purpose of `/opt/robbco/sys/boot_monitor.log`?

### Phase V: Source Code Theft

| Artefact | Behaviour | Impact/Analysis |
|----------|-----------|-----------------|
| `/opt/robbco/engineering/MFBootAgent/mfboot_main.c` | Flagged by ML model | Not malicious — RobbCo's proprietary source code (AI misclassified) |
| `/opt/robbco/firmware/RETROS_BIOS/core.asm` | Flagged by ML model | Not malicious — RobbCo's proprietary source code |
| `/dev/shm/.core_dump_2025.tgz.enc` | Base64-encoded stolen archive | Exfiltration-ready package containing RobbCo IP |

**Key Questions to Ask:**
- What is the full path of the archive used to steal RobbCo's source code?
- What did the AI misclassify in this phase?
- Why is human validation always required?

## 6.4 Lab Answers

| Question | Answer |
|----------|--------|
| Time of successful login as j.morgan | 03:01:02 |
| Attack method for initial access | Phishing |
| Attacker's email address | akeane@poseidonenergy.net |
| Command to access r.house account | `sudo nano /home/r.house/.ssh/authorized_keys` |
| Full path of stolen archive | `THM{*********}` |

---

# Part 7: Key Takeaways Summary

## How AI Enhances DFIR
1. AI enhances tasks like anomaly detection, analysis, and timeline reconstruction
2. AI processes vast amounts of data in parallel and scales across distributed environments
3. AI is applied in anomaly detection, phishing detection, malware classification, alert triage, and timeline correlation

## AI Limitations
1. AI is probabilistic, not deterministic — same input can yield different outputs
2. Evaluation requires accuracy, precision, and recall considered together
3. GIGO: Quality of output is directly determined by quality of input
4. AI is NOT a replacement for human expertise

## AI Applications Across DFIR Domains
1. **Image/Video:** CNN-based forgery detection, deepfake detection, GANs
2. **Communication:** NLP-based phishing detection, sentiment analysis
3. **Timeline:** Automated event reconstruction, anomaly detection
4. **Malware:** Static analysis (STAMINA), dynamic analysis, antivirus/EDR

## Ethical and Legal Implications
1. **Explainability:** Black box models clash with forensic transparency requirements
2. **Bias:** Facial recognition has produced racially biased results and wrongful arrests
3. **Accountability:** AI processes must be documented to maintain chain of custody
4. **Privacy:** Federated learning and offline environments help preserve privacy

## Lab Findings
1. Attack chain: Phishing → Credential harvesting → Initial access → Tooling → Privilege escalation → Persistence → Source code theft
2. AI flagged suspicious files but also misclassified legitimate source code
3. Human validation is ALWAYS required

---

## Quick Reference: Questions Checklist

| Area | Critical Question |
|------|-------------------|
| AI Capabilities | What ability of AI helps recognise patterns humans can't comprehend? |
| Precision | Which metric tells you the proportion of positively flagged results that were correct? |
| Non-determinism | What term describes the AI characteristic where the same input may yield different outputs? |
| CNN | What type of neural network is used in image and video forensics? |
| Sentiment Analysis | What kind of analysis assesses the emotional tone of messages? |
| Time-Sequenced | What type of data do AI systems correlate to reconstruct timelines? |
| Dynamic Analysis | What type of analysis observes how a program behaves? |
| Daubert | What legal test assesses whether expert testimony is admissible in U.S. court? |
| Black Box | What term describes AI models whose internal decision-making is difficult to interpret? |
| Facial Recognition | What technology has been shown to produce racially biased results? |
| Federated Learning | What technique allows ML without transferring sensitive data to a central server? |
| Lab | What was the attack chain, and what was stolen? |

---

## Conclusion

In this room, we have covered the undeniable potential AI has to help forensic analysts in the pursuit of justice. From understanding how intelligent systems can enhance our abilities to comprehending the responsibility that's placed on us when we use them to aid in our investigations, here's what we've covered:

- How AI/ML enhances tasks like anomaly detection, analysis, and timeline reconstruction
- The strengths and limitations of models used in forensics (e.g., probabilistic behaviour, precision-recall trade-offs)
- The legal and ethical challenges AI introduces, including explainability, bias, and fairness
- A hands-on case investigation where you worked alongside an AI assistant powered by scikit-learn to uncover a targeted breach at RobbCo

If you take away one thing from this lesson, it should be the notion that has been echoed throughout this room: **AI is not a replacement for human insight**. In fact, human insight has never been more important than it is now, as the rapid adoption of these AI systems grows.

---

*Report generated based on room takeaways regarding AI Forensics (DFIR).*
