## 🧩 Wordle with GRPO (RL + Constraint-Aware Inference)

This project explores reinforcement learning for symbolic reasoning by training a language model to play Wordle using GRPO (Group Relative Policy Optimization).

**Key idea**
* The model learns guess preferences via RL
* Exact Wordle logic is enforced at inference time using a constraint-aware reranker
* The secret word is never shown to the model
* This mirrors how real RL systems combine learned policies + symbolic rules.

---

## 🟩 Wordle (Game Overview)

Wordle is a word puzzle game where the player must guess a hidden 5-letter English word within 6 attempts.

After each guess, the game provides feedback for each letter:

* "✓" — correct letter in the correct position
* "-" — correct letter in the wrong position
* "x" — letter not present in the word

The feedback must be used to refine subsequent guesses.
The secret word is never revealed until the game ends.

The objective is to identify the secret word using logical elimination and pattern recognition within the allowed attempts.

---
## Gameplay Image (Actual)

![Game Image](/game_image.png)

---

## 🏗️ System Overview

### Model

* Base: Qwen2.5-0.5B-Instruct
* Training: GRPO (via TRL)

Objective: propose Wordle guesses conditioned on prior feedback

### Environment

* Wordle feedback (✓, -, x) generated externally
* Feedback is appended to the next prompt as text
* No hidden state or cheating

### Inference (Critical)

* The model samples multiple candidate guesses
* A constraint-aware reranker enforces:
* all known correct positions
* forbidden letters and positions
* letter count constraints
* The best valid candidate is selected

The model proposes. The environment disposes.

---

## 🔁 Gameplay Loop

* Build prompt from previous guesses + feedback
* Model generates candidate guesses
* Reranker filters invalid guesses
* Environment computes feedback using the secret
* Repeat up to 6 steps
  
---

## 🎯 Why This Matters

* Demonstrates why pure RL is insufficient for symbolic games
* Shows a clean separation between:
* learned intuition (LLM + GRPO)
* exact logic (constraints)

Reflects how real systems (e.g. AlphaGo, tool-using LLMs) work

---

## 📊 Results

* Feedback is respected across all steps
* Converges once enough correct letters are known
* Small model + small dataset → partial solve rate (expected)
  
---

## ⚠️ Important Notes

* This is not a brute-force solver
* Constraint logic is outside the model by design
* GRPO improves proposal quality, not logical correctness
  
---
