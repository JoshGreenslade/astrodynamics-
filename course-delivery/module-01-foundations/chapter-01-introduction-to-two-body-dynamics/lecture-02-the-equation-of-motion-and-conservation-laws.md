# Lecture 02 — The Equation of Motion and Conservation Laws

**Module 01: Foundations**
**Chapter 01: Introduction to Two-Body Dynamics**
**Version 1.0 | Date: 2026-06-15**

---

## Learning Objectives

By the end of this lecture, you will be able to:

1. Derive the restricted two-body equation of motion from Newton's laws step by step.
2. Explain why central-force dynamics imply conservation of angular momentum and planar motion.
3. Derive specific mechanical energy conservation using dot products and chain rule calculus.
4. Use the sign of energy to classify bound and unbound trajectories.
5. Build a type-annotated computational propagator and audit invariant drift.
6. Identify when conservation laws appear to fail because assumptions are broken.

**Primary Competency Domain**: C2 — Two-Body Dynamics  
**Supporting Domains**: C1 — Mathematical Foundations, C4 — Time and Anomaly

---

## Prerequisites

Before proceeding, answer from memory:

- What is the difference between `$r$` as a scalar distance and `$\vec{r}$` as a position vector?
- Why can a force that changes with position still conserve total energy?
- What is the geometric meaning of a cross product?
- What does taking a time derivative of a vector quantity tell you?

If any of these are uncertain, pause and rebuild those ideas first.

---

## Concept Vocabulary

- **Central force**: Force directed along the line between bodies, depending only on separation.
- **Specific mechanical energy**: Energy per unit mass, `$\varepsilon = v^2/2 - \mu/r$` in the two-body model.
- **Specific angular momentum**: Angular momentum per unit mass, `$\vec{h} = \vec{r} \times \vec{v}$`.
- **Invariant**: A quantity that remains constant along ideal trajectories.
- **Conservative force**: Force derived from potential, giving path-independent work.
- **Planar motion**: Motion confined to a fixed plane determined by angular momentum direction.
- **Escape condition**: Threshold case where `$\varepsilon = 0$`.

---

## Common Misconception Box

> **Misconception**: “Because gravity gets weaker with altitude, energy cannot stay constant.”
>
> **Correction**: Gravity changes with position, but it is still conservative in the ideal two-body model. Kinetic and potential energy trade off continuously while total specific mechanical energy stays constant.

---

## Section 1 — System Architecture

The equation of motion gives local acceleration. Conservation laws give global structure. You need both.

Use this causal chain:

1. State `(r, v)` determines acceleration from gravity.
2. Acceleration updates velocity.
3. Velocity updates position.
4. Derived invariants are checked to verify physical consistency.

In simulation work, invariant drift is your first warning signal. If drift grows systematically in an unforced model, numerics are likely the culprit.

---

## Section 2 — First-Principles Derivation

### 2.1 Starting equation

For a satellite orbiting a dominant central body:

$$ \ddot{\vec{r}} = -(\mu / r^3) \vec{r} $$

where `$\mu = G M$` is the standard gravitational parameter, `$\vec{r}$` is the position vector, and `$r = |\vec{r}|$` is the scalar separation.

### 2.2 Angular momentum conservation

Define specific angular momentum:

$$ \vec{h} = \vec{r} \times \vec{v} $$

Differentiate with respect to time:

$$ \dot{\vec{h}} = \vec{v} \times \vec{v} + \vec{r} \times \ddot{\vec{r}} $$

The first term is zero. The second term is also zero because `$\ddot{\vec{r}}$` is always parallel to `$\vec{r}$` in a central force model. Therefore `$\dot{\vec{h}} = 0$`, so `$\vec{h}$` is constant.

### 2.3 Planarity

A constant angular momentum direction means the orbit remains in a fixed plane.

### 2.4 Energy conservation

Take the dot product of equation of motion with velocity `$\vec{v}$`:

$$ \vec{v} \cdot \ddot{\vec{r}} = -(\mu / r^3) (\vec{v} \cdot \vec{r}) $$

Left side is the time derivative of `$v^2/2$`. Right side becomes the time derivative of `$-\mu/r$`. So:

$$ d/dt ( v^2/2 - \mu/r ) = 0 $$

Hence:

$$ \varepsilon = v^2/2 - \mu/r = constant $$

### 2.5 Energy sign and orbit class

| Energy sign | Meaning | Orbit class |
|---|---|---|
| `$\varepsilon < 0$` | Bound | Ellipse (circle is special case) |
| `$\varepsilon = 0$` | Escape threshold | Parabola |
| `$\varepsilon > 0$` | Unbound | Hyperbola |

---

## Section 3 — Scaffolded Worked Examples

### Worked Example 1 — Compute invariants from state vector

Given Earth `$\mu = 398600.4418 km^3/s^2$`, state:
- `$r = [7000, 0, 0] km$`
- `$v = [0, 7.5, 1.0] km/s$`

1. `$r = 7000 km$`
2. `$v^2 = 57.25 km^2/s^2$`
3. `$\varepsilon = 57.25/2 - 398600.4418/7000 = -28.3179 km^2/s^2$` (bound)
4. `$\vec{h} = \vec{r} \times \vec{v} = [0, -7000, 52500] km^2/s$`

### Worked Example 2 — Local escape speed

At radius `$r = 9000 km$`, solve for `$v_esc$` with `$\varepsilon = 0$`:

$$ v_esc = \sqrt{2 \mu / r} = 9.412 km/s $$

### Worked Example 3 — Diagnose numerical drift

Propagation report over one orbit:
- Initial `$\varepsilon = -29.1000$`, final `$\varepsilon = -28.4000$`
- Initial `$|\vec{h}| = 53000.0$`, final `$|\vec{h}| = 52999.8$`

Interpretation: large energy drift, tiny angular momentum drift. Likely time-step sensitivity in energy exchange dynamics. Reduce step size and retest.

### Worked Example 4 — Classify trajectory from telemetry

Given `$r = 12000 km$`, `$v = 8.8 km/s$`, Earth `$\mu = 398600.4418 km^3/s^2$`:

$$ \varepsilon = 8.8^2/2 - 398600.4418/12000 = 5.5033 km^2/s^2 $$

Since `$\varepsilon > 0$`, the Earth-relative trajectory is hyperbolic in the ideal model.

---

## Section 4 — Computational Model

```python
from __future__ import annotations
from dataclasses import dataclass
import numpy as np
from numpy.typing import NDArray

Vector = NDArray[np.float64]

@dataclass(frozen=True)
class State:
    r: Vector  # km
    v: Vector  # km/s

@dataclass(frozen=True)
class Invariants:
    energy: float
    h_norm: float

class TwoBodyModel:
    def __init__(self, mu_km3_s2: float) -> None:
        self.mu = float(mu_km3_s2)

    def acceleration(self, r: Vector) -> Vector:
        r_norm = float(np.linalg.norm(r))
        return -(self.mu / r_norm**3) * r

    def invariants(self, state: State) -> Invariants:
        r_norm = float(np.linalg.norm(state.r))
        v2 = float(state.v @ state.v)
        energy = 0.5 * v2 - self.mu / r_norm
        h_norm = float(np.linalg.norm(np.cross(state.r, state.v)))
        return Invariants(energy=energy, h_norm=h_norm)

    def rk4_step(self, state: State, dt: float) -> State:
        def deriv(s: State) -> tuple[Vector, Vector]:
            a = self.acceleration(s.r)
            return s.v, a

        k1_r, k1_v = deriv(state)
        s2 = State(r=state.r + 0.5 * dt * k1_r, v=state.v + 0.5 * dt * k1_v)
        k2_r, k2_v = deriv(s2)
        s3 = State(r=state.r + 0.5 * dt * k2_r, v=state.v + 0.5 * dt * k2_v)
        k3_r, k3_v = deriv(s3)
        s4 = State(r=state.r + dt * k3_r, v=state.v + dt * k3_v)
        k4_r, k4_v = deriv(s4)

        r_next = state.r + (dt / 6.0) * (k1_r + 2*k2_r + 2*k3_r + k4_r)
        v_next = state.v + (dt / 6.0) * (k1_v + 2*k2_v + 2*k3_v + k4_v)
        return State(r=r_next, v=v_next)

```

Validation checklist:
- [ ] Circular-orbit sanity check
- [ ] One-period state return tolerance
- [ ] Relative drift checks for energy and angular momentum
- [ ] Step-size convergence test

---

## Section 5 — Tangible Application Task

Design and validate a two-body propagation experiment for a low Earth elliptical orbit.

Required outputs:
1. Initial state and assumptions
2. Integration settings and rationale
3. Energy and angular momentum drift summary
4. Engineering judgment on whether errors are acceptable
5. One improvement proposal for the next iteration

Self-assessment rubric (0 to 3 each): physical consistency, mathematical correctness, numerical evidence quality, diagnostic depth, communication clarity.

---

## Section 6 — Retrieval Integration

Answer briefly from memory:
1. Why does satellite mass cancel from acceleration but not from force?
2. Why does a central force imply planar motion?
3. How do you derive energy conservation from the equation of motion?
4. Why is invariant monitoring essential in numerical orbit propagation?

---

## Check Your Understanding Bank

Use this bank for spaced retrieval over multiple sessions.

The prompts intentionally cycle core conservation themes with spaced repetition so the same concepts are revisited after short intervals.

### Prompt 001

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 002

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 003

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 004

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 005

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 006

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 007

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 008

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 009

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 010

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 011

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 012

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 013

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 014

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 015

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 016

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 017

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 018

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 019

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 020

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 021

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 022

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 023

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 024

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 025

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 026

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 027

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 028

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 029

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 030

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 031

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 032

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 033

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 034

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 035

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 036

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 037

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 038

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 039

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 040

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 041

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 042

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 043

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 044

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 045

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 046

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 047

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 048

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 049

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 050

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 051

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 052

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 053

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 054

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 055

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 056

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 057

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 058

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 059

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 060

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 061

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 062

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 063

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 064

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 065

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 066

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 067

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 068

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 069

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 070

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 071

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 072

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 073

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 074

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 075

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 076

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 077

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 078

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 079

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 080

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 081

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 082

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 083

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 084

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 085

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 086

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 087

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 088

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 089

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 090

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 091

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 092

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 093

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 094

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 095

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 096

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 097

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 098

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 099

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 100

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 101

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 102

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 103

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 104

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 105

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 106

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 107

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 108

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 109

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 110

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 111

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 112

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 113

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 114

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 115

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 116

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 117

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 118

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 119

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 120

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 121

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 122

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 123

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 124

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 125

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 126

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 127

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 128

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 129

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 130

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 131

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 132

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 133

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 134

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 135

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 136

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 137

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 138

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 139

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 140

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 141

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 142

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 143

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 144

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 145

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 146

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 147

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 148

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 149

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 150

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 151

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 152

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 153

Question: Explain `energy conservation` in one paragraph, and include the equation `$\varepsilon = v^2/2 - mu/r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 154

Question: Explain `angular momentum conservation` in one paragraph, and include the equation `$h = r \times v$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 155

Question: Explain `central-force directionality` in one paragraph, and include the equation `$\ddot{r} = -(mu/r^3) r$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 156

Question: Explain `orbit classification by energy sign` in one paragraph, and include the equation `$\varepsilon < 0, =0, >0$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 157

Question: Explain `planar motion implication` in one paragraph, and include the equation `$h$ direction is constant` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 158

Question: Explain `escape-speed reasoning` in one paragraph, and include the equation `$v_{esc} = \sqrt{2\mu/r}$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 159

Question: Explain `invariant drift diagnostics` in one paragraph, and include the equation `$Delta \varepsilon$ and $Delta |h|$` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---

### Prompt 160

Question: Explain `numerical step-size sensitivity` in one paragraph, and include the equation `$dt \downarrow$ gives lower drift` with symbol definitions.

Minimum answer structure:
- Statement of principle
- Equation and symbol meanings
- One engineering interpretation
- One failure mode when assumptions break

---


## Extended Practice Set

### Exercise 01

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 02

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 03

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 04

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 05

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 06

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 07

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 08

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 09

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 10

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 11

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 12

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 13

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 14

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 15

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 16

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 17

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 18

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 19

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 20

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 21

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 22

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 23

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 24

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 25

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 26

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 27

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 28

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 29

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 30

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 31

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 32

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 33

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 34

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 35

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 36

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 37

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 38

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 39

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 40

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 41

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 42

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 43

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 44

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 45

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 46

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 47

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 48

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 49

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 50

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 51

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 52

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 53

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 54

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 55

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 56

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 57

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 58

Given `$\mu$`, `$r$`, and `$v$`, compute `$\varepsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 59

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 60

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

---

## Further Exploration

- Show the link between angular momentum conservation and areal velocity.
- Compare RK4 and symplectic leapfrog on long-horizon invariant drift.
- Add a small drag term and identify which conservation statements break first.
- Derive vis-viva from conservation laws and conic geometry.

---

## Closing Summary

You now have the core invariants that make orbital mechanics tractable: specific energy and specific angular momentum. In the ideal two-body model, these are non-negotiable constraints. Treat them as your physical compass when deriving formulas, classifying trajectories, and validating code.

**End of Lecture 02**
