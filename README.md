# llm-multi-agent-benchmark ⚡🤖

**Reproducible energy benchmark for comparing single-system vs LLM-based multi-agent coordination.**

This repository contains the code and materials for the course project paper **“Benchmarking Multi-Agent Coordination for Energy Planning.”**
It provides an episode-based evaluation setup built from OWID-derived country-level electricity indicators and compares single-system planners against LLM-driven multi-agent coordination strategies under identical forecasting and deterministic feasibility-aware scoring.

---

## Authors 👥

* **Altan Ulaş Zöhre**
* **Deniz Sakaroğlu**

---

## Paper 📄

**Final report (PDF):**

* `paper/paper-MAS.pdf`

---

## What this benchmark evaluates 🎯

The benchmark is designed to **separate architectural effects** (single vs multi-agent) from confounders by sharing the same data pipeline, forecasting module, and evaluator across all methods.

**Shared pipeline**

* OWID-derived episode construction (country-level annual indicators)
* Fixed train/eval split: **2000–2018** (train) and **2019–2023** (evaluation)
* Shared demand forecasting: **Random Forest (autoregressive rollout)**
* Deterministic evaluator with **soft constraints** (budget/emission/security → continuous violations → smooth score penalty)

**Reported outputs**

* Outcome metrics: **cost**, **emission**, **security**, **violations**, **score**
* Overhead metrics: **messages**, **rounds**, **vetos**, **runtime**

---

## Methods implemented 🧠

Single-system planners:

* **Deterministic Baseline** (fixed plan, no LLM)
* **Single-LLM Planner (One-shot)** (one LLM call outputs a full JSON plan)

Multi-agent planners (LLM agents for cost/emission/security):

* **LLM-MAS Candidate Search (Sequential)** (bounded deltas, deterministic best-delta selection, multi-round refinement)
* **LLM-MAS Coordinator + Security Veto** (LLM coordinator integrates proposals; deterministic security check + corrective veto)

Ablations:

* **No Veto** (coordinator without the veto safeguard)
* **No Communication** (collapsed interaction depth / single-step variant)

---

## Quickstart 🚀

### 1) Install dependencies

```bash
pip install -r requirements.txt
```

### 2) Set Groq API key 🔑

Linux/macOS:

```bash
export GROQ_API_KEY="YOUR_KEY"
```

Windows (PowerShell):

```powershell
setx GROQ_API_KEY "YOUR_KEY"
```

### 3) Run ▶️

* Recommended: run the notebook

  * `notebooks/Benchmarking Multi-Agent.ipynb`


---

## Reproducibility notes ✅

* All methods use the **same** episode builder, forecasting module, and evaluator.
* LLM outputs follow strict JSON schemas and are clipped to valid bounds.
* Multi-agent updates are bounded (delta range) to encourage conservative refinement.
* Overhead (messages/runtime/veto events) is reported to capture coordination cost.

---

## Data source 🌍

Episodes are constructed from OWID Energy indicators:

* [https://ourworldindata.org/energy](https://ourworldindata.org/energy)

---

## License 📜

MIT (recommended for course projects).
If you prefer a different license, replace `LICENSE` accordingly.
