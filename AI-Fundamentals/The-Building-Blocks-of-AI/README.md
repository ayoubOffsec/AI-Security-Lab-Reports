# The Building Blocks of AI

## Overview

This room introduces the fundamental concepts behind Artificial Intelligence, Machine Learning, Deep Learning, Neural Networks, and Large Language Models (LLMs).

The goal is to understand how modern AI systems work at a high level before moving into AI security concepts and attacks.

---

## Learning Objectives

By completing this room, I learned:

* The relationship between AI, Machine Learning, Deep Learning, and LLMs.
* The main categories of Machine Learning.
* How neural networks are structured.
* The concept of overfitting.
* How Deep Learning differs from traditional Machine Learning.
* How LLMs generate text.
* The role of Transformers and Attention.
* The purpose of Backpropagation.
* The concept of Reinforcement Learning from Human Feedback (RLHF).
* Basic concepts behind AI agents.

---

## 1. Artificial Intelligence and Machine Learning

### Artificial Intelligence

Artificial Intelligence (AI) is the broader field concerned with enabling machines to perform tasks that normally require human-like intelligence, such as reasoning, problem-solving, understanding, and generation.

### Machine Learning

Machine Learning (ML) is a subfield of AI where systems learn patterns from data instead of being explicitly programmed with every rule.

A simplified ML workflow is:

```text
Data
  ↓
Training
  ↓
Model
  ↓
Evaluation
  ↓
Deployment
```

---

## 2. Overfitting

**Overfitting** occurs when a model becomes too familiar with its training data and performs poorly when presented with new, unseen data.

Instead of learning patterns that generalize well, the model effectively learns the characteristics of the training dataset too closely.

**Answer:** `Overfitting`

---

## 3. Types of Machine Learning

The room introduces four main categories.

### Supervised Learning

The model learns from labelled data where the expected output is already known.

Example:

```text
Input → Training Data → Known Output
```

Common use cases include classification and prediction.

### Unsupervised Learning

The model works with unlabelled data and attempts to discover patterns or structures within it.

Example:

```text
Unlabelled Data
      ↓
Pattern Discovery
      ↓
Clusters / Relationships
```

### Semi-Supervised Learning

Semi-supervised learning combines a small amount of labelled data with a larger amount of unlabelled data.

**Answer:** `Semi-supervised learning`

### Reinforcement Learning

In Reinforcement Learning (RL), an agent interacts with an environment and learns from rewards and penalties.

```text
Agent
  ↓
Action
  ↓
Environment
  ↓
Reward / Penalty
  ↓
Learning
```

**Answer:** `Reinforcement learning`

---

## 4. Neural Networks

A neural network is a computational model composed of interconnected nodes arranged into layers.

A simplified structure is:

```text
Input Layer
     ↓
Hidden Layers
     ↓
Output Layer
```

### Input Layer

Receives the raw input data.

### Hidden Layers

Process the input and learn increasingly complex patterns.

### Output Layer

Produces the final prediction or classification.

### Synapses

The connections between nodes have weights that influence how information is propagated through the network.

**Answer:** `Synapses`

---

## 5. Deep Learning

Deep Learning is a Machine Learning approach based on neural networks with multiple layers.

The additional layers allow the model to learn increasingly complex representations from data.

Simplified relationship:

```text
Artificial Intelligence
        ↓
Machine Learning
        ↓
Deep Learning
        ↓
Neural Networks
```

---

## 6. Large Language Models

Large Language Models (LLMs) are Deep Learning models designed to process and generate natural language.

At a simplified level, an LLM generates text by predicting the next token repeatedly:

```text
Input
  ↓
Predict next token
  ↓
Predict next token
  ↓
Predict next token
  ↓
Generated response
```

LLMs are trained using very large datasets and contain a large number of parameters.

---

## 7. Parameters

A model contains numerical **parameters** that are adjusted during training.

These parameters collectively allow the model to represent patterns learned from its training data.

The training process repeatedly modifies these parameters to improve the model's predictions.

---

## 8. Backpropagation

**Backpropagation** is used during neural-network training to adjust model parameters based on the error between the model's prediction and the expected result.

Simplified process:

```text
Input
  ↓
Prediction
  ↓
Calculate Error
  ↓
Backpropagation
  ↓
Update Parameters
  ↓
Repeat
```

**Answer:** `Backpropagation`

---

## 9. Transformers

Modern LLMs commonly use the **Transformer** architecture.

The Transformer architecture was introduced in the 2017 research paper:

> *Attention Is All You Need*

Transformers significantly changed how large-scale language models process sequences of text.

**Answer:** `Transformer neural networks`

---

## 10. Attention

The **Attention** mechanism allows Transformer models to assign different levels of importance to different tokens based on their context and relationships within a sequence.

For example, when processing a sentence containing multiple nouns and pronouns, attention helps the model determine which words are related to each other.

**Answer:** `Attention`

---

## 11. Reinforcement Learning from Human Feedback

**RLHF (Reinforcement Learning from Human Feedback)** is a technique used to refine model behaviour using human evaluations.

A simplified process is:

```text
Model Output
     ↓
Human Evaluation
     ↓
Feedback
     ↓
Model Refinement
```

Human feedback can help guide a model toward responses that better match desired behaviour.

**Answer:** `RLHF`

---

## 12. Generative AI

Generative AI refers to AI systems capable of generating new content.

Depending on the model, this can include:

* Text
* Images
* Audio
* Video
* Other types of content

LLMs are one category of Generative AI focused primarily on language.

---

## 13. AI, ML, DL and LLM Relationship

A simplified hierarchy is:

```text
Artificial Intelligence
│
└── Machine Learning
    │
    └── Deep Learning
        │
        └── Neural Networks
            │
            └── Transformer-based Models
                │
                └── Large Language Models
```

This hierarchy is useful for understanding where LLMs fit within the broader AI ecosystem.

---

# Lab Answers

| Question                                                                  | Answer                        |
| ------------------------------------------------------------------------- | ----------------------------- |
| Model becomes too familiar with training data                             | `Overfitting`                 |
| ML using a small amount of labelled data with a larger unlabelled dataset | `Semi-supervised learning`    |
| ML learning through rewards and penalties                                 | `Reinforcement learning`      |
| First neural-network layer receiving raw input                            | `Input layer`                 |
| Weighted connections between neural-network nodes                         | `Synapses`                    |
| Algorithm used to adjust parameters based on prediction error             | `Backpropagation`             |
| Neural-network architecture powering modern LLMs                          | `Transformer neural networks` |
| Mechanism assigning different importance to tokens                        | `Attention`                   |
| Human-feedback process used to refine model behaviour                     | `RLHF`                        |

> **Note:** The exported lab content available to me masks the actual TryHackMe flags, so their exact values are not included here.

---

# Key Takeaways

The most important concepts from this room are:

1. **AI** is the broad field of machine intelligence.
2. **Machine Learning** allows systems to learn patterns from data.
3. **Deep Learning** uses multi-layer neural networks.
4. **Neural Networks** process information through interconnected layers.
5. **Overfitting** occurs when a model fails to generalize to unseen data.
6. **LLMs** are large Deep Learning models designed to process and generate language.
7. **Transformers** are the architecture behind many modern LLMs.
8. **Attention** allows Transformers to model relationships between tokens.
9. **Backpropagation** helps adjust model parameters during training.
10. **RLHF** uses human feedback to influence model behaviour.

---

# Security Relevance

Understanding these concepts is important before studying AI security.

AI applications introduce technologies and attack surfaces that differ from traditional web applications. A security researcher therefore needs to understand not only how an AI application is exposed to users, but also the basic mechanics behind models, prompts, training, inference, and AI agents.

This room provides the theoretical foundation for the security-focused rooms that follow.

---

## Conclusion

This room established the fundamental building blocks of modern AI, from basic Machine Learning concepts to neural networks, Transformers, and LLMs.

The next step is to move from **understanding how AI works** to **understanding how AI systems can be attacked and secured**.
