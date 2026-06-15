# Lecture 01 — Conic Sections and Orbital Geometry

**Module 01: Foundations**
**Chapter 02: Conic Sections and Orbital Geometry**
**Version 0.1 | Date: 2026-06-15**

---

## Learning Objectives and Competency Mapping

By the end of this lecture, you should be able to:

1. Derive the polar conic equation for a central-force trajectory from conserved angular momentum and specific energy.
2. Classify an orbit as elliptic, parabolic, or hyperbolic from eccentricity and specific orbital energy.
3. Explain the geometric meaning of focus, directrix, semi-major axis, semi-latus rectum, and eccentricity.
4. Compute periapsis and apoapsis distances from conic parameters.
5. Build a typed computational tool that classifies trajectory geometry from state vectors.
6. Solve mixed conceptual-and-computational exam-style problems on orbital geometry.

**Primary Competency Domain**: C3 — Orbital Elements and Geometry  
**Supporting Competency Domains**: C1 — Mathematical Foundations, C2 — Two-Body Dynamics

---

## Prerequisites and Retrieval Activation

Before moving forward, answer these without notes:

- What does specific mechanical energy $\varepsilon = v^2/2 - \mu/r$ tell you in the two-body model?
- Why is specific angular momentum $\vec{h} = \vec{r} \times \vec{v}$ constant when gravity is the only force?
- What is the difference between a scalar invariant and a vector invariant?

If any answer felt shaky, revisit Module 01 Chapter 01 first. This lecture assumes you are comfortable with vectors, derivatives, and invariants.

---

## Concept Vocabulary

- **Conic section**: one of ellipse, parabola, or hyperbola.
- **Focus**: fixed point associated with a conic; in orbital mechanics, the central body is located at a focus.
- **Eccentricity ($e$)**: shape parameter of a conic.
- **Semi-major axis ($a$)**: size parameter for ellipses and hyperbolas.
- **Semi-latus rectum ($p$)**: geometric scale appearing directly in polar conic form.
- **Periapsis / Apoapsis**: nearest and farthest points from the focus along the orbit.

---

## Common Misconception Box

> **Misconception**: "Circular, elliptical, and hyperbolic trajectories are separate physics models."
>
> **Correction**: They are one family of solutions to the same two-body equation of motion,
> $$\ddot{\vec{r}} = -\frac{\mu}{r^3}\vec{r}.$$
> The geometry changes because the invariants (energy and angular momentum) change. The governing law does not.

---

## System Architecture: From Invariants to Geometry

The architecture for this chapter is:

1. Dynamics gives invariants ($\varepsilon$, $\vec{h}$).
2. Invariants determine orbit class and shape parameter $e$.
3. Shape parameters map to geometric descriptors ($a$, $p$, periapsis, apoapsis).
4. Geometry predicts reachable positions and velocity trends.

The key mental shift is this: in Chapter 01 we solved for acceleration and trajectory evolution; in Chapter 02 we interpret whole trajectory geometry from invariant snapshots.

---

## First-Principles Derivation: The Polar Conic Equation

Start from two-body motion in a plane with central focus at the origin. In polar coordinates,

$$r = \frac{p}{1 + e\cos\nu}$$

is the conic form we want to justify from dynamics, not assume.

### Step 1: Angular momentum conservation

For central forces,

$$\vec{h} = \vec{r}\times\vec{v}, \qquad h = r^2\dot{\nu} = \text{constant}.$$

### Step 2: Radial equation under inverse-square gravity

Using planar polar dynamics,

$$\ddot{r} - r\dot{\nu}^2 = -\frac{\mu}{r^2}.$$

Substitute $\dot{\nu} = h/r^2$:

$$\ddot{r} - \frac{h^2}{r^3} = -\frac{\mu}{r^2}.$$

### Step 3: Change variable to $u = 1/r$

Applying the standard substitution with independent variable $\nu$ yields

$$\frac{d^2u}{d\nu^2} + u = \frac{\mu}{h^2}.$$

### Step 4: Solve linear ODE

General solution:

$$u(\nu) = \frac{\mu}{h^2}\left(1 + e\cos(\nu-\nu_0)\right).$$

Choose periapsis-aligned reference so $\nu_0=0$:

$$u(\nu)=\frac{\mu}{h^2}(1+e\cos\nu).$$

Invert $u=1/r$:

$$r(\nu)=\frac{h^2/\mu}{1+e\cos\nu} = \frac{p}{1+e\cos\nu},$$

where

$$p = \frac{h^2}{\mu}.$$

This is the conic equation produced directly by two-body dynamics.

### Step 5: Energy-based classification

Specific energy is

$$\varepsilon = \frac{v^2}{2} - \frac{\mu}{r}.$$

For conics:

- $\varepsilon<0 \Rightarrow$ ellipse ($0\le e<1$)
- $\varepsilon=0 \Rightarrow$ parabola ($e=1$)
- $\varepsilon>0 \Rightarrow$ hyperbola ($e>1$)

and

$$e = \sqrt{1 + \frac{2\varepsilon h^2}{\mu^2}}.$$

---

## Worked Examples (Scaffolded)

### Example 1 — Orbit class from invariants

Given $\mu=398600\ \text{km}^3/\text{s}^2$, $\varepsilon=-15\ \text{km}^2/\text{s}^2$, and $h=60000\ \text{km}^2/\text{s}$:

$$e = \sqrt{1 + \frac{2(-15)(60000)^2}{(398600)^2}} \approx 0.565.$$

Since $e<1$ and $\varepsilon<0$, orbit is elliptic.

### Example 2 — Periapsis/apoapsis from $a$ and $e$

Suppose $a=12000\ \text{km}$ and $e=0.25$.

$$r_p = a(1-e)=9000\ \text{km}, \qquad r_a = a(1+e)=15000\ \text{km}.$$

Interpretation: the orbit is moderately elongated with 6,000 km radial swing.

### Example 3 — Hyperbolic geometry check

Given $\varepsilon = +6\ \text{km}^2/\text{s}^2$ and $h=90000\ \text{km}^2/\text{s}$:

$$e = \sqrt{1 + \frac{2(6)(90000)^2}{(398600)^2}} \approx 1.268.$$

Because $e>1$, this is a flyby/escape-type hyperbolic trajectory.

### Check Your Understanding

1. If $e=1$, what is the corresponding energy sign?
2. If $e=0$, where is periapsis relative to apoapsis?
3. Can $\varepsilon<0$ and $e>1$ occur simultaneously in two-body dynamics?

---

## Computational Model: State-to-Geometry Classifier

```python
from __future__ import annotations
from dataclasses import dataclass
from math import sqrt
from typing import Tuple

Vector3 = Tuple[float, float, float]


def dot(a: Vector3, b: Vector3) -> float:
    return a[0]*b[0] + a[1]*b[1] + a[2]*b[2]


def cross(a: Vector3, b: Vector3) -> Vector3:
    return (
        a[1]*b[2] - a[2]*b[1],
        a[2]*b[0] - a[0]*b[2],
        a[0]*b[1] - a[1]*b[0],
    )


def norm(v: Vector3) -> float:
    return sqrt(dot(v, v))


@dataclass(frozen=True)
class OrbitGeometry:
    energy: float
    h_mag: float
    eccentricity: float
    class_name: str


class ConicClassifier:
    def __init__(self, mu: float) -> None:
        self.mu = mu

    def classify(self, r: Vector3, v: Vector3) -> OrbitGeometry:
        r_mag = norm(r)
        v_mag = norm(v)
        h_vec = cross(r, v)
        h_mag = norm(h_vec)

        energy = 0.5 * v_mag * v_mag - self.mu / r_mag
        ecc = sqrt(1.0 + (2.0 * energy * h_mag * h_mag) / (self.mu * self.mu))

        if abs(ecc - 1.0) < 1e-6:
            class_name = "parabolic"
        elif ecc < 1.0:
            class_name = "elliptic"
        else:
            class_name = "hyperbolic"

        return OrbitGeometry(
            energy=energy,
            h_mag=h_mag,
            eccentricity=ecc,
            class_name=class_name,
        )
```

### Why this model matters

- It links raw state vectors to interpretable geometry.
- It supports mission design gates (bound-orbit vs escape-orbit checks).
- It reinforces the chapter's architecture: invariants drive geometry.

---

## Tangible Application Task

You are evaluating an Earth-observation mission handover scenario. Two candidate insertion states are proposed.

1. Compute $\varepsilon$, $h$, and $e$ for each candidate.
2. Identify whether each candidate is bound or unbound.
3. For bound case(s), compute $r_p$ and $r_a$.
4. Recommend which candidate better supports repeat ground-track revisit and justify using geometry.

Deliverable:

- A one-page engineering memo with equations, calculations, and recommendation.

Competency mapping:

- Primary: C3 (geometry interpretation and conic classification)
- Supporting: C1 (vector/math operations), C2 (invariant reasoning)

---

## Retrieval Integration (Interleaved)

Connect this chapter to Chapter 01:

- Chapter 01 gave the acceleration law and invariants.
- This chapter converts those invariants into geometric orbit class and shape.

Prompt:

> If two spacecraft have identical position vectors but different velocity vectors at the same instant, explain how Chapter 01 and Chapter 02 together let you predict whether one is bound and the other escaping.

---

## Supplementary Derivations and Physical Depth

### A. Relation between semi-major axis and energy

For conics with finite $a$,

$$\varepsilon = -\frac{\mu}{2a}.$$

This immediately implies:

- Ellipse: $a>0 \Rightarrow \varepsilon<0$
- Hyperbola: $a<0 \Rightarrow \varepsilon>0$ (sign convention in astrodynamics)

### B. Periapsis/apoapsis via $p$ and $e$

Set $\nu=0$ for periapsis and $\nu=\pi$ for apoapsis:

$$r_p = \frac{p}{1+e}, \qquad r_a = \frac{p}{1-e} \quad (e<1).$$

### C. Geometry sanity checks

- $e=0$ gives circle: $r=p$ constant.
- $e\to1^{-}$ gives very elongated ellipse.
- $e>1$ implies denominator can approach zero only asymptotically in reachable branch.

---

## End-of-Section Exercises

### Routine Practice

1. For $\varepsilon=-20\ \text{km}^2/\text{s}^2$ and $h=55000\ \text{km}^2/\text{s}$, compute $e$ and classify the orbit.
2. For $a=9000\ \text{km}$ and $e=0.1$, compute $r_p$ and $r_a$.
3. Show that circular motion is recovered when $e=0$ in the polar conic equation.
4. If $e=0.75$, sketch qualitative radius variation over true anomaly $\nu\in[0,2\pi]$.
5. Explain why angular momentum conservation implies planar motion.

### Stretch Problems

6. Derive $e$ from the eccentricity vector definition and show consistency with
   $$e = \sqrt{1 + \frac{2\varepsilon h^2}{\mu^2}}.$$
7. For fixed $\mu$ and fixed $h$, determine how changing energy changes conic class.
8. A mission requires periapsis above 6800 km and apoapsis below 45000 km. Give acceptable $(a,e)$ inequalities.
9. Use symbolic algebra to derive $r_p$ and $r_a$ from $r(\nu)=p/(1+e\cos\nu)$.
10. Explain, physically, why the same gravitational law yields both closed and open trajectories.

### Competency-Tagged Exam Practice

11. **[C3]** Given state vector data, compute orbit class and geometric parameters.
12. **[C1+C3]** Propagate uncertainty in velocity magnitude into uncertainty in eccentricity using first-order sensitivity.
13. **[C2+C3]** Compare two insertion burns by their resulting specific energy and geometric consequences.
14. **[C3]** Construct a decision table mapping $(\varepsilon, e)$ pairs to trajectory type and mission interpretation.
15. **[C1+C2+C3]** Critique a flawed derivation that concludes $e<0$ is physically possible.

---

## Answer Key (Selected)

1. Use
   $$e = \sqrt{1 + \frac{2\varepsilon h^2}{\mu^2}}.$$
   If computed $e<1$ and $\varepsilon<0$, orbit is elliptical.
2.
   $$r_p=a(1-e),\quad r_a=a(1+e).$$
3. Substitute $e=0$ into $r=p/(1+e\cos\nu)$ to get $r=p$ constant.
6. Start from
   $$\vec{e}=\frac{\vec{v}\times\vec{h}}{\mu}-\frac{\vec{r}}{r},$$
   then square and simplify using dot/cross identities and specific energy form.
10. Closed vs open trajectories depends on total specific energy, not a different force law.

---

## Further Exploration

- Derive the eccentricity vector in component form and connect it to periapsis direction.
- Explore how perturbations (e.g., J2) break strict conic closure.
- Implement a plotting notebook that overlays trajectory classes for varying $e$.

---

## Vocabulary Reference

- **True anomaly ($\nu$)**: angle from periapsis direction to current position vector.
- **Specific energy ($\varepsilon$)**: orbital energy per unit mass.
- **Specific angular momentum ($h$)**: angular momentum per unit mass magnitude.
- **Bound orbit**: trajectory with $\varepsilon<0$.
- **Escape orbit**: trajectory with $\varepsilon\ge 0$.
