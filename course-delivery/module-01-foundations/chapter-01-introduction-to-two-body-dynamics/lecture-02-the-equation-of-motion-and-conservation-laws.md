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

- What is the difference between `$r$` as a scalar distance and `r` as a position vector symbol?
- Why can a force that changes with position still conserve total energy?
- What is the geometric meaning of a cross product?
- What does taking a time derivative of a vector quantity tell you?

If any of these are uncertain, pause and rebuild those ideas first.

---

## Concept Vocabulary

- **Central force**: Force directed along the line between bodies, depending only on separation.
- **Specific mechanical energy**: Energy per unit mass, `$epsilon = v^2/2 - mu/r$` in the two-body model.
- **Specific angular momentum**: Angular momentum per unit mass, `$h = r x v$`.
- **Invariant**: A quantity that remains constant along ideal trajectories.
- **Conservative force**: Force derived from potential, giving path-independent work.
- **Planar motion**: Motion confined to a fixed plane determined by angular momentum direction.
- **Escape condition**: Threshold case where `$epsilon = 0$`.

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

$$ r_ddot = -(mu / r^3) r $$

where `$mu = G M$` and `$r = |r|$`.

### 2.2 Angular momentum conservation

Define specific angular momentum:

$$ h = r x v $$

Differentiate with respect to time:

$$ h_dot = v x v + r x r_ddot $$

The first term is zero. The second term is also zero because `$r_ddot$` is always parallel to `r` in a central force model. Therefore `$h_dot = 0$`, so `h` is constant.

### 2.3 Planarity

A constant angular momentum direction means the orbit remains in a fixed plane.

### 2.4 Energy conservation

Take the dot product of equation of motion with velocity `v`:

$$ v dot r_ddot = -(mu / r^3) (v dot r) $$

Left side is the time derivative of `$v^2/2$`. Right side becomes the time derivative of `$-mu/r$`. So:

$$ d/dt ( v^2/2 - mu/r ) = 0 $$

Hence:

$$ epsilon = v^2/2 - mu/r = constant $$

### 2.5 Energy sign and orbit class

| Energy sign | Meaning | Orbit class |
|---|---|---|
| `$epsilon < 0$` | Bound | Ellipse (circle is special case) |
| `$epsilon = 0$` | Escape threshold | Parabola |
| `$epsilon > 0$` | Unbound | Hyperbola |

---

## Section 3 — Scaffolded Worked Examples

### Worked Example 1 — Compute invariants from state vector

Given Earth `$mu = 398600.4418 km^3/s^2$`, state:
- `$r = [7000, 0, 0] km$`
- `$v = [0, 7.5, 1.0] km/s$`

1. `$r = 7000 km$`
2. `$v^2 = 57.25 km^2/s^2$`
3. `$epsilon = 57.25/2 - 398600.4418/7000 = -28.3179 km^2/s^2$` (bound)
4. `$h = r x v = [0, -7000, 52500] km^2/s$`

### Worked Example 2 — Local escape speed

At radius `$r = 9000 km$`, solve for `$v_esc$` with `$epsilon = 0$`:

$$ v_esc = sqrt(2 mu / r) = 9.412 km/s $$

### Worked Example 3 — Diagnose numerical drift

Propagation report over one orbit:
- Initial `$epsilon = -29.1000$`, final `$epsilon = -28.4000$`
- Initial `$|h| = 53000.0$`, final `$|h| = 52999.8$`

Interpretation: large energy drift, tiny angular momentum drift. Likely time-step sensitivity in energy exchange dynamics. Reduce step size and retest.

### Worked Example 4 — Classify trajectory from telemetry

Given `$r = 12000 km$`, `$v = 8.8 km/s$`, Earth `$mu = 398600.4418 km^3/s^2$`:

$$ epsilon = 8.8^2/2 - 398600.4418/12000 = 5.5033 km^2/s^2 $$

Since `$epsilon > 0$`, the Earth-relative trajectory is hyperbolic in the ideal model.

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

### Prompt 001

Question: State one conservation-law principle 1 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 002

Question: State one conservation-law principle 2 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 003

Question: State one conservation-law principle 3 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 004

Question: State one conservation-law principle 4 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 005

Question: State one conservation-law principle 5 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 006

Question: State one conservation-law principle 6 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 007

Question: State one conservation-law principle 7 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 008

Question: State one conservation-law principle 8 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 009

Question: State one conservation-law principle 9 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 010

Question: State one conservation-law principle 10 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 011

Question: State one conservation-law principle 11 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 012

Question: State one conservation-law principle 12 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 013

Question: State one conservation-law principle 13 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 014

Question: State one conservation-law principle 14 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 015

Question: State one conservation-law principle 15 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 016

Question: State one conservation-law principle 16 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 017

Question: State one conservation-law principle 17 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 018

Question: State one conservation-law principle 18 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 019

Question: State one conservation-law principle 19 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 020

Question: State one conservation-law principle 20 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 021

Question: State one conservation-law principle 21 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 022

Question: State one conservation-law principle 22 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 023

Question: State one conservation-law principle 23 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 024

Question: State one conservation-law principle 24 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 025

Question: State one conservation-law principle 25 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 026

Question: State one conservation-law principle 26 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 027

Question: State one conservation-law principle 27 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 028

Question: State one conservation-law principle 28 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 029

Question: State one conservation-law principle 29 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 030

Question: State one conservation-law principle 30 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 031

Question: State one conservation-law principle 31 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 032

Question: State one conservation-law principle 32 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 033

Question: State one conservation-law principle 33 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 034

Question: State one conservation-law principle 34 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 035

Question: State one conservation-law principle 35 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 036

Question: State one conservation-law principle 36 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 037

Question: State one conservation-law principle 37 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 038

Question: State one conservation-law principle 38 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 039

Question: State one conservation-law principle 39 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 040

Question: State one conservation-law principle 40 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 041

Question: State one conservation-law principle 41 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 042

Question: State one conservation-law principle 42 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 043

Question: State one conservation-law principle 43 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 044

Question: State one conservation-law principle 44 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 045

Question: State one conservation-law principle 45 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 046

Question: State one conservation-law principle 46 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 047

Question: State one conservation-law principle 47 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 048

Question: State one conservation-law principle 48 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 049

Question: State one conservation-law principle 49 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 050

Question: State one conservation-law principle 50 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 051

Question: State one conservation-law principle 51 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 052

Question: State one conservation-law principle 52 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 053

Question: State one conservation-law principle 53 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 054

Question: State one conservation-law principle 54 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 055

Question: State one conservation-law principle 55 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 056

Question: State one conservation-law principle 56 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 057

Question: State one conservation-law principle 57 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 058

Question: State one conservation-law principle 58 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 059

Question: State one conservation-law principle 59 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 060

Question: State one conservation-law principle 60 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 061

Question: State one conservation-law principle 61 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 062

Question: State one conservation-law principle 62 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 063

Question: State one conservation-law principle 63 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 064

Question: State one conservation-law principle 64 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 065

Question: State one conservation-law principle 65 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 066

Question: State one conservation-law principle 66 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 067

Question: State one conservation-law principle 67 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 068

Question: State one conservation-law principle 68 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 069

Question: State one conservation-law principle 69 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 070

Question: State one conservation-law principle 70 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 071

Question: State one conservation-law principle 71 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 072

Question: State one conservation-law principle 72 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 073

Question: State one conservation-law principle 73 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 074

Question: State one conservation-law principle 74 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 075

Question: State one conservation-law principle 75 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 076

Question: State one conservation-law principle 76 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 077

Question: State one conservation-law principle 77 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 078

Question: State one conservation-law principle 78 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 079

Question: State one conservation-law principle 79 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 080

Question: State one conservation-law principle 80 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 081

Question: State one conservation-law principle 81 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 082

Question: State one conservation-law principle 82 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 083

Question: State one conservation-law principle 83 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 084

Question: State one conservation-law principle 84 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 085

Question: State one conservation-law principle 85 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 086

Question: State one conservation-law principle 86 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 087

Question: State one conservation-law principle 87 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 088

Question: State one conservation-law principle 88 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 089

Question: State one conservation-law principle 89 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 090

Question: State one conservation-law principle 90 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 091

Question: State one conservation-law principle 91 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 092

Question: State one conservation-law principle 92 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 093

Question: State one conservation-law principle 93 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 094

Question: State one conservation-law principle 94 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 095

Question: State one conservation-law principle 95 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 096

Question: State one conservation-law principle 96 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 097

Question: State one conservation-law principle 97 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 098

Question: State one conservation-law principle 98 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 099

Question: State one conservation-law principle 99 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 100

Question: State one conservation-law principle 100 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 101

Question: State one conservation-law principle 101 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 102

Question: State one conservation-law principle 102 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 103

Question: State one conservation-law principle 103 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 104

Question: State one conservation-law principle 104 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 105

Question: State one conservation-law principle 105 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 106

Question: State one conservation-law principle 106 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 107

Question: State one conservation-law principle 107 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 108

Question: State one conservation-law principle 108 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 109

Question: State one conservation-law principle 109 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 110

Question: State one conservation-law principle 110 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 111

Question: State one conservation-law principle 111 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 112

Question: State one conservation-law principle 112 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 113

Question: State one conservation-law principle 113 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 114

Question: State one conservation-law principle 114 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 115

Question: State one conservation-law principle 115 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 116

Question: State one conservation-law principle 116 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 117

Question: State one conservation-law principle 117 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 118

Question: State one conservation-law principle 118 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 119

Question: State one conservation-law principle 119 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 120

Question: State one conservation-law principle 120 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 121

Question: State one conservation-law principle 121 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 122

Question: State one conservation-law principle 122 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 123

Question: State one conservation-law principle 123 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 124

Question: State one conservation-law principle 124 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 125

Question: State one conservation-law principle 125 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 126

Question: State one conservation-law principle 126 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 127

Question: State one conservation-law principle 127 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 128

Question: State one conservation-law principle 128 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 129

Question: State one conservation-law principle 129 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 130

Question: State one conservation-law principle 130 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 131

Question: State one conservation-law principle 131 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 132

Question: State one conservation-law principle 132 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 133

Question: State one conservation-law principle 133 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 134

Question: State one conservation-law principle 134 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 135

Question: State one conservation-law principle 135 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 136

Question: State one conservation-law principle 136 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 137

Question: State one conservation-law principle 137 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 138

Question: State one conservation-law principle 138 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 139

Question: State one conservation-law principle 139 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 140

Question: State one conservation-law principle 140 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 141

Question: State one conservation-law principle 141 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 142

Question: State one conservation-law principle 142 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 143

Question: State one conservation-law principle 143 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 144

Question: State one conservation-law principle 144 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 145

Question: State one conservation-law principle 145 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 146

Question: State one conservation-law principle 146 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 147

Question: State one conservation-law principle 147 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 148

Question: State one conservation-law principle 148 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 149

Question: State one conservation-law principle 149 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 150

Question: State one conservation-law principle 150 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 151

Question: State one conservation-law principle 151 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 152

Question: State one conservation-law principle 152 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 153

Question: State one conservation-law principle 153 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 154

Question: State one conservation-law principle 154 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 155

Question: State one conservation-law principle 155 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 156

Question: State one conservation-law principle 156 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 157

Question: State one conservation-law principle 157 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 158

Question: State one conservation-law principle 158 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 159

Question: State one conservation-law principle 159 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

### Prompt 160

Question: State one conservation-law principle 160 in the two-body model and write one equation using `$...$` delimiters.

Minimum answer structure:
- Statement
- Equation
- Physical meaning

---

## Extended Practice Set

### Exercise 01

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 02

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 03

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 04

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 05

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 06

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 07

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 08

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 09

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 10

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 11

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 12

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 13

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 14

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 15

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 16

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 17

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 18

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 19

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 20

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 21

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 22

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 23

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 24

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 25

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 26

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 27

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 28

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 29

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 30

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 31

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 32

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 33

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 34

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 35

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 36

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 37

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 38

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 39

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 40

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 41

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 42

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 43

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 44

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 45

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 46

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 47

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 48

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 49

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 50

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 51

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 52

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 53

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 54

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 55

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 56

Derive one consequence of constant angular momentum for orbital geometry.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 57

Propose a numerical experiment that separates integration error from modeling error.

Answer scaffold: setup, equations, interpretation, common pitfall.

### Exercise 58

Given `$mu$`, `$r$`, and `$v$`, compute `$epsilon$` and classify orbit type.

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
