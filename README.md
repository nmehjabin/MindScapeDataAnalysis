This project conducts a follow-up analysis of MindScape [ 22], an
AI-supported journaling system that combines mobile sensing with
large language models to promote student well-being. Building on
the original feasibility study, we examine how contextual (Weeks
1-6) and generic (Weeks 7-8) prompts influence four behavioral do-
mains: physical activity, social interaction, digital habits, and sleep,
using per-user week labeling to account for staggered start dates.
# AI Behavioral Journaling System — Code Pipeline

## Overview
Pipeline for analyzing AI-generated journaling prompts, behavioral intentions, and passive sensing outcomes from the MindScape longitudinal study.

---

## Notebooks

### `task2.ipynb`
Merges raw journal data with week labels and behavioral domain categories.

- **Input:** Raw journal entries, week label file
- **Output:** `journals_with_week_labels.csv`

---

### `task3_1_fixed.ipynb`
Extracts behavioral signals across three time windows using **z-score normalized** composite features.

- **Input:** `journals_with_week_labels.csv`, passive sensing features
- **Key operations:**
  - Z-score normalizes features before averaging into a domain composite
  - Computes `signal_before`, `signal_after`, `signal_change`, `behavioral_improvement` per window
- **Output:**
  - `prompts_with_signals_task3_1day.csv`
  - `prompts_with_signals_task3_3day.csv`
  - `prompts_with_signals_task3_7day.csv`

> ⚠️ Z-score normalization is applied **before** composite averaging to prevent any single feature from dominating the signal.

---

### `task4.ipynb`
Classifies each unique journal prompt as `REFLECTIVE` or `REDIRECTIVE` using an LLM.

- **Input:** Unique journal prompts
- **Model:** `llama-3.3-70b-versatile` via Groq API
- **Output:** `prompts_with_classifications_task4.csv`

---

### `task5_2.ipynb`
Classifies journal responses to `REDIRECTIVE` prompts as `INTENTION` or `REFLECTION` using an LLM.

- **Input:** `prompts_with_classifications_task4.csv` (REDIRECTIVE entries only)
- **Model:** `llama-3.3-70b-versatile` via Groq API
- **Output:** `prompts_with_response_task4_3day.csv`

> ✅ `response_type_v2` is the **authoritative classification column** — use this, not `response_type`.

---

### `textual_analysis_v2.ipynb`
Linguistic analysis comparing improved vs. non-improved journal entries.

- **Input:** `prompts_with_response_task4_3day.csv` + signal outputs
- **Key features analyzed:** Word count, type-token ratio (TTR), VADER sentiment, future tense, agency markers
- **Output:** Statistical comparisons and visualizations

---

### `signal_analysis2_fixed.ipynb`
Core statistical analysis. Unit of analysis: **one row = one journal entry × one domain**.

- **Input:** `prompts_with_signals_task3_3day.csv`, response classifications
- **Improvement flag:** `behavioral_improvement` (yes/no) from `task3_1_fixed` output, using z-score normalized composite with domain-specific direction
- **Output:** Main results tables and figures

---

### `debugging.ipynb`
Sandbox for testing and debugging LLM response type classifications.

- Not part of the main analysis pipeline

---

## Pipeline Order

task2 → task3_1_fixed → task4 → task5_2 -> signal_analysis2_fixed
                                        -> textual_analysis_v2
