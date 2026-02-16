# Protein-Shielded-Nanocrystal-Foundry PSNF
A reproducible proof-of-concept repo that simulates protein-templated nanocrystal growth and shielding, predicts Month-18 milestone success with ML, and uses uncertainty-guided active learning and Pareto trade-offs to prioritize the next best experiments.

# Protein-Shielded Nanocrystal Foundry (PSNF), Digital Twin, Active Learning, and Multi-Objective Design

This repository contains a proof-of-concept **digital twin** for the “Protein-Shielded Nanocrystal Foundry (PSNF)” workflow. The code simulates thousands of nanocrystal synthesis and assembly runs, then applies **machine learning** and **closed-loop active learning** to recommend the next experiments that maximize the probability of meeting **Month-18 milestone gates** while steering toward **Month-36 magnetic performance proxies**.

The goal is to demonstrate an end-to-end, evidence-driven pipeline for **manufacturing-like iteration**: define measurable success criteria, generate data, learn decision rules, quantify uncertainty, and prioritize the next experimental batch.

## What this repository does

### 1) Simulated PSNF experiments (digital twin)
Generates a synthetic dataset where each row is one experimental run across:
- **Nanoreactor choices** (protein cage / design rules).
- **Nucleation and growth controls** (chelation, redox, feed rate, pore gating, luminal charge).
- **Shielding strategies** (thin peptide, protein shell, permanent matrix, sacrificial matrix).
- **Assembly and lock-in controls** (field strength, shear, residence time, crosslink strength).

Outputs include:
- **Particle size** and **dispersity** (SD%).
- **Crystalline fraction** and **air stability** (days).
- **SAXS-like order parameter** (0–1).
- **Throughput metrics** (yield, mass of single solid).
- **Magnetic proxies** (coercivity, squareness).
- Optional **physics-inspired proxies**: packing fraction, density, Ms, Br, and BHmax (proxy).

> Important: this dataset is synthetic and built to be *physically plausible* and *numerically anchored* to ranges seen in scientific journal-indexed literature. It is not a claim about real performance.

### 2) Milestone gating (Month 18)
Defines a hard pass/fail label (`m18_pass`) requiring simultaneous satisfaction of multiple thresholds, for example:
- Yield > 10 g,
- Single solid > 1 g,
- SD% ≤ 15%,
- Crystalline fraction > 0.40,
- Order ≥ 0.70,
- Air stability ≥ 14 days.

This replicates “all conditions must hold” milestone logic.

### 3) Predictive modeling + interpretability
Trains a Random Forest model to predict probability of Month-18 success from design knobs. The repository provides:
- Cross-validation (AUC and Average Precision),
- Precision–Recall curves,
- Confusion matrices at user-defined thresholds,
- Feature importance,
- ICE curves and response surfaces,
- Ablation studies (which feature groups matter most),
- Failure-mode taxonomy (what criteria fail, stratified by design choices).

### 4) Multi-objective selection + Pareto analysis (optional)
Adds physics-inspired proxies and performs constraint-aware multi-objective selection:
- maximize BHmax proxy, squareness, coercivity, and order,
- subject to meeting Month-18 success and minimal functional constraints,
- yields a **Pareto front** of best trade-offs.

### 5) Closed-loop active learning
Simulates a realistic “learn → pick next experiments → run → update” loop:
- Starts from an initial dataset,
- Trains a model each round,
- Picks the next batch using **UCB-style acquisition** (mean + β·uncertainty),
- “Runs” the batch in the digital twin,
- Tracks improvement over rounds (learning curves),
- Compares strategies (random vs greedy vs UCB active learning).

## Repository structure

```
├── psnf_simulator.py                 # Simulation functions and milestone gating
├── psnf_analysis_baseline.py         # Distributions, relationships, baseline model
├── psnf_analysis_extended.py         # Stress tests, ICE, calibration, DOE proposals
├── psnf_physics_proxies.py           # Packing, Ms, Br, BHmax proxy, Pareto selection
├── psnf_intensive_analysis.py        # CV, PR, response surfaces, ablation, clustering, failure taxonomy
├── psnf_active_learning.py           # Closed-loop active learning loop and strategy comparisons
├── psnf_outputs/                     # Generated CSV summaries and optional plots
├── README.md
├── ModelCard.md
└── DataSheet.md
```
## Quickstart
1) Create environment
```
python -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install numpy pandas matplotlib scikit-learn
```
2) Run a baseline simulation + model
```
python psnf_analysis_baseline.py
```
3) Run intensive analysis
```
python psnf_intensive_analysis.py
```
4) Run closed-loop active learning
```
python psnf_active_learning.py
```
## Outputs are written to:
psnf_outputs/

## How to interpret outputs
	•	Histograms show whether distributions look plausible (size, SD%, BHmax proxy).
	•	Scatter plots show expected monotonic relationships (field → order, order → squareness).
	•	PR curves reflect decision-making quality when successes are rare.
	•	Ablation shows which pipeline stage dominates success (chemistry, shielding, field/flow, lock-in).
	•	Failure taxonomy identifies dominant bottlenecks (e.g., poor order vs poor stability).
	•	Active learning curves show how quickly the pipeline improves by choosing better experiments.

## Scientific grounding and numeric anchors
The simulator uses parameter ranges anchored to representative values in scientific journals for protein nanocages, magnetic nanoparticle size scales, and SAXS-accessible order in magnetic assemblies. All numeric anchoring in this repository is used to keep the synthetic dataset within plausible regimes.

## Limitations
	•	This is a synthetic dataset, not real lab data.
	•	Proxies (order, stability, BHmax) are simplified and intended for ranking and workflow demonstration.
	•	True magnetic performance requires explicit phase control, hysteresis loop measurement, and microstructure quantification.

## License
MIT 

## Citation
**Petalcorin, M.I.R.** (2026)A Protein-Shielded Nanocrystal Foundry for Air-Stable, Anisotropically Ordered Magnetic Solids, a Digital-Twin and Closed-Loop Active-Learning Blueprint for Low-Temperature Manufacturing. Github. https://github.com/mpetalcorin/Protein-Shielded-Nanocrystal-Foundry-PSNF-
