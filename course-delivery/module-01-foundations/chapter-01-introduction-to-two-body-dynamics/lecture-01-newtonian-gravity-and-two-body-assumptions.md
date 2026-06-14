# Module 01 · Foundations
## Chapter 01 · Introduction to Two-Body Dynamics
### Lecture 01 · Newtonian Gravity and Two-Body Assumptions

## 1) Why this lecture matters
Astrodynamics is built on models. The first model must be both mathematically tractable and physically meaningful. Newtonian gravity with two-body assumptions is that baseline model: it allows closed-form reasoning, supports orbit classification, and creates the reference solution that later perturbation models modify.

In practice, this lecture gives you the language and equations used throughout Stage 1:
- force and acceleration forms of gravity
- vector direction conventions
- model assumptions and scope limits
- unit-safe calculation workflow

## 2) Prerequisites and activation
Before proceeding, confirm fluency with:
1. SI base and derived units (especially N, kg, m, s).
2. Vector magnitude and direction notation.
3. Rearranging algebraic expressions with powers.

### Activation task
For each statement, mark valid/invalid and justify:
1. \(a=\mu/r^2\) produces units of m/s\(^2\).
2. If \(\mathbf{r}\) points from Earth to spacecraft, then gravitational acceleration is in the \(+\mathbf{r}\) direction.
3. Using km for \(r\) while keeping \(\mu\) in m\(^3\)/s\(^2\) is acceptable if done consistently in one line.

## 3) Learning outcomes
By the end of this lecture, you should be able to:
1. State Newton’s law of gravitation and define every symbol.
2. Convert force form to scalar acceleration form.
3. Write and interpret vector acceleration with correct sign and direction.
4. Explain each two-body assumption and identify when it breaks down.
5. Solve gravitational acceleration problems with full unit consistency.
6. Communicate model limitations without abandoning model usefulness.

## 4) Concept vocabulary
- **Central body**: dominant gravitating body in the model (e.g., Earth).
- **Spacecraft/test mass**: smaller body whose motion is modeled.
- **Standard gravitational parameter (\(\mu\))**: \(GM\), often known for the central body.
- **Two-body model**: system where only mutual gravity of two bodies is considered.
- **Perturbation**: effect excluded from ideal model (drag, oblateness, third bodies, thrust).

## 5) Core derivation and interpretation
### 5.1 Newton’s universal gravitation (force magnitude)
\[
F = G \frac{m_1 m_2}{r^2}
\]

Where:
- \(F\): force magnitude (N)
- \(G\): universal gravitational constant
- \(m_1,m_2\): interacting masses (kg)
- \(r\): center-to-center separation distance (m)

Physical interpretation:
- inverse-square dependence: doubling distance quarters force
- symmetry: each body exerts equal and opposite gravitational force on the other
- attraction: force direction is inward along line of centers

### 5.2 Convert to acceleration form for spacecraft dynamics
For spacecraft mass \(m\) near central body mass \(M\):
\[
F = ma = G\frac{Mm}{r^2}
\Rightarrow
a = G\frac{M}{r^2}
\]

Define:
\[
\mu = GM
\]
Then:
\[
a = \frac{\mu}{r^2}
\]

Why this matters:
- spacecraft mass cancels
- acceleration depends on position, not spacecraft mass
- this supports trajectory propagation from state vectors

### 5.3 Vector form and sign convention
Let \(\mathbf{r}\) point from central body to spacecraft. Then gravity points toward the central body:
\[
\mathbf{a} = -\mu\frac{\mathbf{r}}{\|\mathbf{r}\|^3}
\]
The negative sign is not optional; it encodes inward direction.

Unit check:
- \(\mu\): m\(^3\)/s\(^2\)
- \(\mathbf{r}/\|\mathbf{r}\|^3\): 1/m\(^2\)
- product: m/s\(^2\) (correct acceleration unit)

## 6) Two-body assumptions and model boundaries
### 6.1 Assumptions
1. Only two bodies interact gravitationally.
2. Bodies are point masses or spherically symmetric.
3. No non-gravitational forces (thrust, drag, SRP).
4. No aspherical harmonics (e.g., \(J_2\)) in this baseline model.
5. State is represented in an inertial frame for derivation.

### 6.2 What this excludes
- atmospheric drag in LEO
- third-body effects (Moon/Sun)
- Earth oblateness effects on long-duration orbit geometry
- finite thrust and maneuver arcs

### 6.3 Why we still use it
Even with exclusions, two-body dynamics:
- gives first-order intuition
- provides analytical structure for conics and Kepler relations
- anchors error analysis when perturbations are later added

## 7) Worked examples
### Example A — acceleration magnitude at orbital altitude
Given Earth \(\mu = 3.986\times10^{14}\ \text{m}^3/\text{s}^2\), find \(a\) at \(r=7000\ \text{km}\).

Convert:
\[
r = 7.0\times10^6\ \text{m}
\]
Compute:
\[
a=\frac{\mu}{r^2}
=\frac{3.986\times10^{14}}{(7.0\times10^6)^2}
\approx 8.13\ \text{m/s}^2
\]
Check: less than \(g_0\approx9.81\ \text{m/s}^2\), physically reasonable.

### Example B — inverse-square scaling check
If radius doubles from \(r\) to \(2r\):
\[
a(2r) = \frac{\mu}{(2r)^2}=\frac{1}{4}\frac{\mu}{r^2}=\frac{a(r)}{4}
\]
Interpretation: inverse-square behavior gives rapid drop with distance.

### Example C — vector direction sanity check
If spacecraft is on +x axis relative to Earth:
\[
\mathbf{r}=[r,0,0]
\Rightarrow
\mathbf{a}=[-\mu/r^2,0,0]
\]
Direction is toward origin, confirming sign convention.

## 8) Common mistakes and corrections
1. **Unit mismatch (km vs m)**  
   Correction: convert all distances to meters when using SI \(\mu\).
2. **Missing negative sign in vector form**  
   Correction: draw direction arrow before writing final expression.
3. **Assuming model is “wrong” because it is simplified**  
   Correction: describe assumptions explicitly and state intended use range.
4. **Using force formula when acceleration formula is sufficient**  
   Correction: use \(a=\mu/r^2\) unless force on a known mass is specifically needed.

## 9) Guided practice set
1. Compute gravitational acceleration at \(r=8000\ \text{km}\) around Earth.
2. A spacecraft moves from \(r_1\) to \(1.5r_1\). Find \(a_2/a_1\).
3. Write \(\mathbf{a}\) for \(\mathbf{r}=[0,-r,0]\) and interpret direction.
4. List two assumptions that become weak for long-duration LEO prediction.
5. Explain in 2-3 sentences why two-body remains useful in mission pre-design.

## 10) Exit ticket (administered)
1. Name two two-body assumptions and one practical limitation.
2. Compute \(a\) at a provided radius with correct units and sign reasoning.
3. State one check you would run to verify your answer is physically plausible.

### Success criteria
- equations selected correctly
- SI units used consistently
- direction/sign justified clearly
- model limitation communicated accurately

## 11) Delivery and assessment record
- Delivery date: 2026-06-14
- Student: JoshGreenslade
- Delivery mode: guided derivation + worked examples + independent checks
- Assessment result: completed; core outcomes met
- Observed gap: occasional sign-convention ambiguity in vector setup
- Intervention for Lecture 02: 5-minute sign/direction refresher before new derivations

## 12) Next lecture bridge
Lecture 02 uses this acceleration framework to build conic geometry vocabulary (ellipse, parabola, hyperbola) and connect motion state to orbit-shape interpretation.
