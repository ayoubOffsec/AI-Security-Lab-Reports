# Lab Report: Detecting Adversarial Attacks

## Executive Summary

This laboratory exercise focuses on exploring, identifying, and evaluating Adversarial Machine Learning attacks targeting vision-based neural networks (specifically Convolutional Neural Networks trained on the MNIST dataset). It covers white-box threat vectors where attackers leverage internal model architecture, weights, and gradients to craft imperceptible perturbations (noise) that force models into high-confidence misclassifications.
Technical Overview & Threat Analysis

  Vulnerability Category: Adversarial Machine Learning / White-Box Gradient Exploitation

  Affected Assets: Computer Vision Models, Optical Character Recognition (OCR) systems (e.g., signature/cheque verification, identity authentication).

    Primary Threat Vectors:

        Fast Gradient Sign Method (FGSM): Single-step gradient-based perturbation.

        Basic Iterative Method (BIM): Multi-step iterative extension of FGSM.

        Projected Gradient Descent (PGD): Iterative method with random initialization and constrained projection.

  Security Impact: Complete evasion of machine learning classification controls without altering the human-perceivable context of input images.

Technical Deep-Dive: Attack Methodologies
### 1. Fast Gradient Sign Method (FGSM)

    Mechanics: Calculates the gradient of the loss function with respect to the input image and adjusts pixel values in the direction that maximizes loss.

    Characteristics: Fast execution, single-shot execution, uniform low-level noise.

    Analogy: Hitting a safe with a sledgehammer once—fast and crude.

### 2. Basic Iterative Method (BIM)

    Mechanics: Applies FGSM repeatedly with smaller step sizes and clips values at each iteration to stay within a valid perturbation boundary.

    Characteristics: Accumulates fine-grained noise, bypasses single-shot FGSM defensive filters.

    Analogy: Picking a lock iteratively—slower, precise, and controlled.

### 3. Projected Gradient Descent (PGD)

    Mechanics: Extends BIM by starting from a random point within the allowed perturbation radius before executing iterative gradient updates, followed by projection back into the valid input range.

    Characteristics: Highly chaotic noise texture, non-reproducible misclassification patterns, resilient against baseline defenses.

    Analogy: A professional team of burglars with complete blueprints—relentless and highly effective.

Practical Lab Implementation & Analysis
Environment Configuration

    Dataset: MNIST (28x28 grayscale handwritten digits).

    Model: Convolutional Neural Network (CNN) built with TensorFlow/Keras.

    Framework: CleverHans library for generating adversarial perturbations.

## Attack Detection & Visual Identification

| Attack Vector | Iteration Strategy | Noise Pattern / Visual Indicator | Detection Complexity |
|---------------|-------------------|----------------------------------|---------------------|
| **FGSM** | Single Step | Minimal pixel-level changes, uniform gradient bump | **Low** |
| **BIM** | Multi-Step (Iterative) | Accumulated directional distortion, brush-stroke effect | **Medium** |
| **PGD** | Random Start + Multi-Step | Chaotic, blurry, fog-like texture | **High** (Hardest to detect) |

## Defense & Mitigation Strategies

    Adversarial Training: Retraining the neural network using datasets augmented with FGSM, BIM, and PGD adversarial samples to harden decision boundaries.

    Input Preprocessing & Sanitization: Applying spatial smoothing, noise filtering, or squeezed feature representations to strip out gradient perturbations prior to inference.

    Robust Architecture Design: Implementing defensive distillation and randomized smoothing to reduce model gradient sensitivity.

  ## Lab Walkthrough & Solutions
### Task 1: Adversarial Examples Overview

    Q1: What adversarial attack is widely used for testing the robustness of ML models?

        Answer: PGD

    Q2: What adversarial attack spreads perturbations over multiple iterations?

        Answer: BIM

    Q3: What does FGSM stand for?

        Answer: Fast Gradient Sign Method

### Task 2: How To Identify Each Attack

    Q1: Which attack is usually the hardest to detect by an AI model?

        Answer: PGD

    Q2: Which attack has minimal pixel-level changes?

        Answer: FGSM

### Task 3: Practical Analysis & Flag Extraction

    Q1: What's the flag for BIM?

        Answer: THM{**********}

    Q2: What's the flag for FGSM?

        Answer: THM{**********}

    Q3: What's the flag for PGD?

        Answer: THM{**********}

## Key Security Takeaways

    Machine learning models are inherently vulnerable to gradient exploitation if an adversary gains white-box access or approximates model gradients.

    Single-shot defenses are insufficient against iterative and randomized attacks like BIM and PGD.

    Security auditing for AI applications must include adversarial robustness testing during the model validation lifecycle.
    
