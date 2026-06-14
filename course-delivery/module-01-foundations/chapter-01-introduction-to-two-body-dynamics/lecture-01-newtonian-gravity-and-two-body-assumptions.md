# Module 01 · Foundations
## Chapter 01 · Introduction to Two-Body Dynamics
### Lecture 01 · Newtonian Gravity and Two-Body Assumptions

## 1) Why this lecture matters
Astrodynamics starts with a model simple enough to solve and strong enough to guide real mission design. In this lecture, we build that model: Newtonian gravity plus two-body assumptions.

## 2) Learning goals
By the end of this lecture, you should be able to:
1. State Newton’s law of gravitation and interpret every term with units.
2. Derive gravitational acceleration from force form.
3. Explain the closed two-body assumptions and when they begin to fail.
4. Check a solution for unit consistency and physical reasonableness.

## 3) Core theory
### 3.1 Newton’s law of gravitation
\[
F = G \frac{m_1 m_2}{r^2}
\]

Where:
- \(F\): gravitational force magnitude (N)
- \(G\): gravitational constant
- \(m_1, m_2\): masses
- \(r\): separation distance between centers of mass (m)

Direction: force acts along the line connecting the two masses and is attractive.

### 3.2 From force to acceleration
For a spacecraft mass \(m\) around a central body mass \(M\):
\[
F = m a = G \frac{M m}{r^2}
\Rightarrow
a = G \frac{M}{r^2}
\]

Define \(\mu = GM\) (standard gravitational parameter):
\[
a = \frac{\mu}{r^2}
\]
Vector form (direction toward the central body):
\[
\mathbf{a} = -\mu \frac{\mathbf{r}}{\|\mathbf{r}\|^3}
\]

### 3.3 Two-body assumptions
We assume:
1. Only two masses interact.
2. Bodies are treated as point masses or spherically symmetric.
3. No thrust, drag, J2, solar radiation pressure, or third-body effects.
4. Inertial reference frame for model derivation.

These assumptions are idealized but form the baseline used before adding perturbations.

## 4) Worked example
Given Earth \(\mu = 3.986\times10^{14}\ \text{m}^3/\text{s}^2\), find acceleration magnitude at \(r=7000\ \text{km}\).

Convert radius:
\[
r = 7.0\times10^6\ \text{m}
\]

Compute:
\[
a=\frac{\mu}{r^2}
=\frac{3.986\times10^{14}}{(7.0\times10^6)^2}
=\frac{3.986\times10^{14}}{4.9\times10^{13}}
\approx 8.13\ \text{m/s}^2
\]

Interpretation: magnitude is below surface gravity, as expected for orbital altitude.

## 5) Common mistakes to avoid
- Mixing km and m in the same computation.
- Dropping the minus sign in vector form (wrong acceleration direction).
- Treating assumptions as universal truth rather than model scope.

## 6) Quick concept check (used as exit ticket)
1. Name two two-body assumptions and one limitation of the model.
2. Compute \(a\) for a given \(r\) using \(a=\mu/r^2\), with correct units.

## 7) Lecture 01 completion notes
- Delivery date: 2026-06-14
- Student: JoshGreenslade
- Result summary: delivered and assessed; minor coaching needed on sign-convention notation.

## 8) Next lecture preview
Lecture 02 introduces conic geometry and connects the two-body model to orbit shape classification.
