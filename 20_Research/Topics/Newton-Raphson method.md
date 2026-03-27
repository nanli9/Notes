---
aliases: [Newton-Raphson, Newton's method, nonlinear solver]
tags: [topic, numerical-methods]
---

# Newton-Raphson Method

Iterative root-finding algorithm for nonlinear systems $f(x) = 0$, $x \in \mathbb{R}^n$, $f: \mathbb{R}^n \to \mathbb{R}^n$.

## Course Notes

### Root Finding
- Jacobian: $J = \frac{\partial f}{\partial x} \in \mathbb{R}^{n \times n}$
- Iteration: $J \cdot \Delta x_n = -f(x_n)$, then $x_{n+1} = x_n + \Delta x_n$
- Sequence: $x_0, x_1, x_2, \ldots, x_n$

### Optimization (Minimization)
- Goal: $\min g(x)$
- Gradient: $f(x) := \frac{\partial g}{\partial x}$
- Hessian: $H(x) = \frac{\partial f}{\partial x} = \frac{\partial^2 g}{\partial x^2} \in \mathbb{R}^{n \times n}$
- Newton step: $H(x_n) \cdot \Delta x_n = -(\nabla g)(x_n)$
- Update: $\Delta x_n = -H(x_n)^{-1} (\nabla g)(x_n)$

### Line Search with Backtracking
- $x_{n+1} = x_n + \alpha \cdot \Delta x_n$, $\alpha \in [0, 1]$
- Condition: $g(x_n) > g(x_{n+1})$ (sufficient decrease)
- Ensures convergence even when full Newton step overshoots

### Connection to Simulation
- Used in implicit [[time integration]] for [[FEM]], [[cloth simulation]], [[IPC]]
- Each physics time step requires solving a nonlinear system → Newton iterations
- Inner loop: solve [[linear solvers|linear system]] per Newton step
