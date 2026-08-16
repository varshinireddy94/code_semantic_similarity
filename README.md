
IITG.ai Code Semantics Similarity Challenge — Rank 13 | 0.95783 F1

# Code Semantic Equivalence Classification

### IITG.ai Code Semantics Similarity Challenge

An LLM-based approach to determine whether two code snippets are **semantically equivalent** using **SmolLM-1.7B, LoRA, and 4-bit quantization**.

🏆 **Kaggle Score:** 0.95783  
📊 **Rank:** 13  
📈 **Best Validation F1:** 0.9683

---

## 🎯 Problem

Given two code snippets, predict whether they implement equivalent behavior.

```text
Code A + Code B
      ↓
    Model
      ↓
 True / False
````

The dataset contained approximately **500K labeled code pairs**.

The key challenge was distinguishing **semantic similarity** from simple textual similarity.

---

## 🧠 Approach

### Model

* **SmolLM-1.7B**
* Causal Language Model
* 2,048-token context

### Fine-Tuning

Used **LoRA (Low-Rank Adaptation)** to efficiently fine-tune the model.

* Total parameters: ~1.73B
* Trainable parameters: ~18.1M
* Trainable fraction: ~1.05%

### Memory Optimization

Used **4-bit quantization** to reduce GPU memory usage and make training feasible on Kaggle hardware.

---

## 🔄 Classification Strategy

Instead of adding a traditional classification head, the binary task was formulated as **next-token prediction**.

The model receives:

```text
Code A
Code B
Answer:
```

and predicts:

```text
True / False
```

The prompt tokens were masked using `-100` so that the loss focused on the answer.

For inference, instead of free-form generation, the logits of the two valid tokens were directly compared:

```text
True  → token ID 3635
False → token ID 4178
```

This avoided invalid generated outputs and provided a probability for `True`.

---

## 📈 Results

| Stage                           | Validation F1 |      Kaggle |
| ------------------------------- | ------------: | ----------: |
| Initial training (~199 steps)   |        0.8916 |       ~0.91 |
| Continued training (~800 steps) |        0.9669 |     0.95634 |
| Threshold optimization          |    **0.9683** | **0.95783** |

The final threshold selected from validation was **0.32**.

### Final Result

**Kaggle Score: 0.95783 — Rank 12**

---

## 🔧 Key Experiments

* Continued training from a saved LoRA adapter after an interrupted run.
* Replaced unconstrained text generation with direct True/False logit scoring.
* Tested exact/reversed code-pair matching from the training data.
* Optimized the classification threshold for F1.
* Debugged Transformers, PEFT, gradient-checkpointing, and Kaggle training issues.

---

## 🛠️ Tech Stack

`Python` · `PyTorch` · `Transformers` · `PEFT` · `LoRA` · `BitsAndBytes` · `SmolLM-1.7B` · `Pandas` · `scikit-learn` · `Kaggle`

---

## 📌 Key Learnings

* Parameter-efficient fine-tuning makes LLM adaptation feasible under limited GPU resources.
* Semantic code equivalence requires more than surface-level similarity.
* Direct token-logit inference is preferable to unrestricted generation for binary outputs.
* Validation-based threshold optimization can improve F1.
* Training stability and experiment design are as important as model selection.

---

## 🚀 Improvements

* Better handling of code exceeding the 2,048-token context.
* Longer and more stable training.
* Full validation-set threshold optimization.
* Comparison with CodeBERT / GraphCodeBERT-style encoders.
* Model ensembling for improved generalization.

## 📂 Repository Structure

```text
  code-semantic-equivalence/
│
├── README.md
├── code_semantic_equivalence.ipynb
├── requirements.txt
│
├── results/
   └── leaderboard.png


