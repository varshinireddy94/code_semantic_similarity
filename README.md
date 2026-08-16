
IITG.ai Code Semantics Similarity Challenge — Rank 12 | 0.95783 F1
# Code Semantic Equivalence Classification

### IITG.ai Code Semantics Similarity Challenge | Kaggle Competition

A parameter-efficient LLM-based approach for predicting whether two code snippets are **semantically equivalent**, using **SmolLM-1.7B + 4-bit quantization + LoRA fine-tuning**.

> **Best Kaggle Score:** 0.95783  
> **Best Recorded Rank:** 12  
> **Best Local Validation F1:** 0.9683

---

## 📌 Overview

Code similarity is not simply about whether two programs look alike.

Two code snippets can have:

- different variable names,
- different control-flow structures,
- different formatting,
- different implementation strategies,

while still producing the same behavior.

This project tackles **code semantic equivalence classification**:

> Given two code snippets, determine whether they are semantically equivalent.

The task is formulated as a binary classification problem:

```text
Code A + Code B
       ↓
   LLM Model
       ↓
 True / False
