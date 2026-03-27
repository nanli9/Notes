---
aliases: [time integration, time stepping, ODE integration, symplectic Euler, implicit Euler]
tags: [topic, numerical-methods, Simulation]
---

# Time Integration

Methods for advancing simulation state $x(t)$ forward in time given $\dot{x} = f(x, t)$.

## Course Notes

### Forward (Explicit) Euler
- $x(t + \Delta t) = x(t) + \Delta t \cdot f(x, t)$
- Issues: **inaccuracy** and **instability** (energy gain, explosion)

### Symplectic Euler (Semi-Implicit)
- $x_{n+1} = x_n + h \cdot v_n$
- $v_{n+1} = v_n + h \cdot a_n$
- Updates velocity first, then position — better energy conservation than forward Euler
- Standard for particle systems and real-time physics

### Implicit Backward Euler
- $\dot{y} = G(y)$, $y \in \mathbb{R}^k$
- $y_{n+1} = y_n + h \cdot G(y_{n+1})$ — requires solving for $y_{n+1}$ (implicit)
- Linearize: $G(y_{n+1}) \approx G(y_n) + \frac{\partial G}{\partial y}\big|_{y_n} (y_{n+1} - y_n) + O(\|y_{n+1} - y_n\|^2)$
- System: $(I - h \frac{\partial G}{\partial y}\big|_{y_n}) \cdot (y_{n+1} - y_n) = h \cdot G(y_n)$
- Requires [[Newton-Raphson method]] + [[linear solvers]] per step
- Unconditionally stable — essential for stiff systems ([[cloth simulation]], [[FEM]])

### Chain Rule for Time Derivatives
- $\dot{x} = \frac{df}{dx} \frac{\partial F}{\partial x} \dot{x} + \frac{\partial F}{\partial t}$

### External Force Fields
- Force defined on a grid → look up force at particle position
- Grid cell index: $i = \lfloor x/h \rfloor$, $j = \lfloor y/h \rfloor$
- Optimization: use $h_{\text{inv}} = 1/h$, then $i = \lfloor x \cdot h_{\text{inv}} \rfloor$ (multiply is ~6× faster than divide)
- Extract int from float using machine code for further speedup

## Papers
- [[20_Research/Papers/Simulation/Simplicits|Simplicits]]
- [[20_Research/Papers/Simulation/Incremental Potential Contact|Incremental Potential Contact]]
- [[20_Research/Papers/Simulation/Smoothed Aggregation Multigrid for Cloth Simulation|Smoothed Aggregation Multigrid for Cloth Simulation]]
