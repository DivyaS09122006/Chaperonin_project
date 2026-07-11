# CTQW-Chaperonin: Quantum Walks for Modeling Chaperonin-Assisted Protein Folding

**Status:** 🚧 Early-stage / active development 

## Overview

This project investigates whether a **continuous-time quantum walk (CTQW)** on a Markov State Model (MSM)-derived protein conformational graph — perturbed by an operator modeling GroEL-GroES ATP hydrolysis — can recover biologically observed folding pathways more faithfully than classical random walk baselines, and whether the resulting quantum dynamical features are biologically interpretable.

## Motivation

Proteins can become kinetically trapped in non-native conformations. In vivo, the GroEL-GroES chaperonin assists escape from these traps via ATP-driven conformational cycling ("iterative annealing"). Classical random walks model probability as diffusing smoothly across a conformational graph — it pools in traps and stays, since diffusion has no escape mechanism once equilibrium is reached.

A CTQW evolves complex-valued amplitude instead of real-valued probability, using the same graph Laplacian but with an imaginary term in the time-evolution operator (`expm(-iLt)` vs. `expm(-Lt)`). This introduces interference — some paths reinforce, others cancel — creating a mechanism for trap escape that classical diffusion structurally lacks. A perturbation operator, timed to the ATP hydrolysis cycle and targeted at high-retention (trap) states, is applied to model the chaperonin's active role in redirecting amplitude toward the native state.

## Method

1. **MSM construction** — Built from real MD trajectories (HP35/villin headpiece, SimTK) using PyEMMA. Nodes = conformational states, edges = transition probabilities, high self-transition = kinetic trap.
2. **Three-way dynamical comparison** — Classical random walk, plain CTQW, and CTQW + ATP-hydrolysis perturbation, all on the same graph.
3. **GNN classification** — PyTorch Geometric model trained on quantum walk probability distributions as node features, classifying on-pathway vs. trapped trajectories.
4. **SHAP interpretability** — Identifies which features/timesteps drive classification, validated against known HP35 folding intermediates.

## Scope & limitations

HP35 serves as a proof-of-concept benchmark, not an in vivo GroEL substrate. Generalization to a genuine chaperonin substrate (ACBP or rhodanese) is planned for publication-strength claims.

## Stack

- `PyEMMA`, `MDTraj` — MSM construction from MD trajectories
- `scipy` — CTQW simulation via matrix exponentiation
- `PyTorch Geometric` — GNN
- `SHAP` — interpretability

