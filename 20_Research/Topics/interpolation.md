---
aliases: [interpolation, bilinear interpolation, trilinear interpolation]
tags: [topic, numerical-methods]
---

# Interpolation

Estimating values between known data points. Fundamental to grid-based simulation, texture sampling, and force field lookup.

## Course Notes

### Linear Interpolation (1D)
- Two values $A_0, A_1$, parameter $\alpha \in [0,1]$
- $A = (1-\alpha) A_0 + \alpha A_1$

### Bilinear Interpolation (2D)
- Four corners: $A_{00}, A_{10}, A_{11}, A_{01}$, parameters $\alpha, \beta \in [0,1]$
- $A = (1-\alpha)(1-\beta) A_{00} + \alpha(1-\beta) A_{10} + \alpha\beta A_{11} + (1-\alpha)\beta A_{01}$

### Trilinear Interpolation (3D) — Convex Combination
- Eight corners $A_{ijk}$, parameters $\alpha, \beta, \gamma \in [0,1]$
- $A = (1-\alpha)(1-\beta)(1-\gamma) A_{000} + \alpha(1-\beta)(1-\gamma) A_{100} + \alpha\beta(1-\gamma) A_{110} + (1+\alpha)\beta(1-\gamma) A_{010} + (1-\alpha)(1-\beta)\gamma A_{001} + \alpha(1-\beta)\gamma A_{101} + \alpha\beta\gamma A_{111} + (1-\alpha)\beta\gamma A_{011}$
- All weights sum to 1 → convex combination → result stays within range of corner values
- Used in: grid force field lookup, [[fluid simulation]] velocity interpolation, volume rendering
