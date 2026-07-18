# Exploration vs. Escape: Quantum Walks for Modeling Chaperonin-Assisted Protein Folding

**Status:** Active — core findings established, hardware validation in progress

## Overview

This project investigates whether **continuous-time quantum walks (CTQW)** on a Markov State Model (MSM)-derived protein conformational graph behave differently from classical random walks when navigating kinetic traps — and whether that difference connects mechanistically to how the GroEL-GroES chaperonin assists protein folding via ATP hydrolysis.

## Core finding

Classical diffusion and quantum walks represent two different strategies for navigating a rugged energy landscape: **exploitation vs. exploration**.

- **Classical diffusion** converges quickly to a fixed equilibrium and stops exploring. On real HP35 folding kinetics, it settles into an effective ~1.22 conformational states, permanently.
- **A quantum walk on the same landscape never settles** — it sustains ongoing exploration, reaching an effective breadth of ~3.27 states over time.

This is a categorically different dynamical behavior driven by quantum interference — not a failure to converge. On an idealized toy landscape with engineered multi-path structure, quantum interference also allows escape from a kinetic trap that classical diffusion structurally cannot achieve (exceeding classical's equilibrium ceiling repeatedly, by up to ~3x).

**Limitations:** on real HP35 kinetics, quantum's broader exploration does not clearly, preferentially target the known biologically important folding intermediate over less significant states, and quantum does not outperform classical on direct native-state occupation probability.

## Method

1. **Toy graph validation** — hand-built directed graphs with an engineered kinetic trap, used to validate the classical/quantum/perturbation pipeline in a fully inspectable setting before touching real data.
2. **Real data** — a published, peer-reviewed 12-state MSM of HP35 (villin headpiece) folding kinetics (Nagel, Sartore & Stock, 2023), used directly via its published transition timescales (Table S2) rather than reprocessing raw MD trajectories.
3. **Detailed-balance symmetrization** — real biological kinetic traps have extreme rate asymmetries (spanning 3+ orders of magnitude) that naive symmetrization destroys. We use a population-weighted (stationary-distribution-based) symmetrization, following the same methodology as the source paper, and independently validate it (our computed native-basin population, 68.3%, matches the paper's reported ~68%).
4. **Perturbation operator** (modeling ATP hydrolysis) — tested via both fixed-periodic and phase-locked firing schedules. Neither clearly outperformed plain (unperturbed) quantum interference on the toy graph — a genuine, reported negative result.

## What this isn't

This is a simulation-based study of quantum-mechanical *dynamics* applied to a real biological transition network — not a claim that protein folding is a literal quantum-coherent process, and not (yet) run on real quantum hardware. See [Roadmap](#roadmap) for the hardware validation currently in progress.

## Stack

- `scipy` — CTQW and classical diffusion simulation via matrix exponentiation
- `numpy` — graph construction, stationary distribution, detailed-balance symmetrization
- `matplotlib` — trajectory and heatmap visualization
- Real data: Nagel, Sartore & Stock (2023), *"Selecting Features for Markov Modeling: A Case Study on HP35,"* [arXiv:2303.03814](https://arxiv.org/abs/2303.03814)

## Roadmap

- [x] Toy graph: classical vs. CTQW vs. perturbed CTQW comparison
- [x] Real HP35 MSM: naive symmetrization (diagnosed as flawed — destroys real trap asymmetry)
- [x] Real HP35 MSM: detailed-balance symmetrization (validated fix)
- [x] Participation ratio (exploration) analysis
- [x] State-level interpretability check (honest, non-flattering result reported)
- [ ] Hardware validation of toy-graph result (~2 qubits, tests whether interference survives real decoherence) — **in progress**
- [ ] Generalization to a second protein (Trp-cage candidate identified; blocked on manual digitization of a figure-only transition network — not yet attempted)
- [ ] Fine-grained MSM from raw trajectory data (PyEMMA pipeline)
- [ ] GNN + SHAP on a properly-sized dataset (deferred — current 12-state system too small for meaningful trained-model evaluation)
