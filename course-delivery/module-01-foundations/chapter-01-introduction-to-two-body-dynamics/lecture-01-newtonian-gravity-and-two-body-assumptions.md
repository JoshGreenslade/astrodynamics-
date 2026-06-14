# Module 01 · Foundations
## Chapter 01 · Introduction to Two-Body Dynamics
### Lecture 01 · Newtonian Gravity and Two-Body Assumptions

## Estimated teaching time
60-70 minutes (including live checks, questions, and guided practice).

## 1) Opening: what we are doing and why it matters (5-7 min)
Today we are building the first mental model that makes orbital motion predictable.  
Think of this as our "clean-room physics" model: simple enough to solve, rich enough to be genuinely useful.

By the end of the hour, you should be comfortable saying:
- "I can write gravity in force form and acceleration form."
- "I can keep the sign and direction correct in vector form."
- "I know exactly what the two-body model includes and what it leaves out."
- "I can solve a gravitational acceleration problem quickly and safely."

We will move from intuition to equations, then back to interpretation, so the math always has physical meaning.

## 2) Prerequisites and activation
Before we dive in, quickly confirm fluency with:
1. SI base and derived units (especially N, kg, m, s).
2. Vector magnitude and direction notation.
3. Rearranging algebraic expressions with powers.

### Activation task
For each statement, mark valid/invalid and justify in one sentence:
1. \(a=\mu/r^2\) produces units of \(\text{m/s}^2\).
2. If \(\mathbf{r}\) points from Earth to spacecraft, then gravitational acceleration is in the \(+\mathbf{r}\) direction.
3. Using km for \(r\) while keeping \(\mu\) in m\(^3\)/s\(^2\) is acceptable if done consistently in one line.

Quick answers to self-check:
1. Valid.  
2. Invalid (gravity is inward, so opposite \(\mathbf{r}\)).  
3. Invalid unless units are converted before substitution.

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

## 5) Core derivation and interpretation (20-25 min)
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
Now let’s make this operational for trajectory work.  
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
Let \(\mathbf{r}\) point from central body to spacecraft.  
Then gravity must point back toward the central body:
\[
\mathbf{a} = -\mu\frac{\mathbf{r}}{\|\mathbf{r}\|^3}
\]
The negative sign is not optional; it encodes inward direction.

Unit check:
- $\(\mu\): m\(^3\)/s\(^2\)$
- \(\mathbf{r}/\|\mathbf{r}\|^3\): 1/m\(^2\)
- product: \(\text{m/s}^2\) (correct acceleration unit)

### 5.4 Why the denominator is \(\|\mathbf{r}\|^3\) in vector form
This is a common sticking point, so let’s unpack it.

Start with:
\[
\mathbf{a}=a\hat{\mathbf{r}}_{\text{inward}}
\]
The magnitude is:
\[
a=\frac{\mu}{r^2}
\]
The inward unit vector is:
\[
\hat{\mathbf{r}}_{\text{inward}}=-\frac{\mathbf{r}}{\|\mathbf{r}\|}
\]
Combine them:
\[
\mathbf{a}=\frac{\mu}{r^2}\left(-\frac{\mathbf{r}}{\|\mathbf{r}\|}\right)
=-\mu\frac{\mathbf{r}}{\|\mathbf{r}\|^3}
\]
So \(\|\mathbf{r}\|^3\) is not mysterious; it comes from \(r^2\) in magnitude times another \(r\) from unit-vector normalization.

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

## 7) Worked examples (15-18 min)
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

### Example D — full 2D vector computation
Let:
\[
\mathbf{r}=[7000,\ 7000,\ 0]\ \text{km}
\]
Convert to meters:
\[
\mathbf{r}=[7.0\times10^6,\ 7.0\times10^6,\ 0]\ \text{m}
\]
Compute radius:
\[
\|\mathbf{r}\|=\sqrt{(7.0\times10^6)^2+(7.0\times10^6)^2}
\approx 9.899\times10^6\ \text{m}
\]
Then:
\[
\mathbf{a}=-\mu\frac{\mathbf{r}}{\|\mathbf{r}\|^3}
\]
With \(\mu=3.986\times10^{14}\ \text{m}^3/\text{s}^2\):
\[
\mathbf{a}\approx[-2.88,\ -2.88,\ 0]\ \text{m/s}^2
\]
Magnitude check:
\[
\|\mathbf{a}\|\approx4.07\ \text{m/s}^2
\]
and this equals \(\mu/\|\mathbf{r}\|^2\), as expected.

## 8) Common mistakes and corrections
1. **Unit mismatch (km vs m)**  
   Correction: convert all distances to meters when using SI \(\mu\).
2. **Missing negative sign in vector form**  
   Correction: draw direction arrow before writing final expression.
3. **Assuming model is “wrong” because it is simplified**  
   Correction: describe assumptions explicitly and state intended use range.
4. **Using force formula when acceleration formula is sufficient**  
   Correction: use \(a=\mu/r^2\) unless force on a known mass is specifically needed.
5. **Forgetting to distinguish \(\mathbf{r}\) from \(\|\mathbf{r}\|\)**  
   Correction: write scalar and vector equations on separate lines before combining.
6. **No plausibility check after arithmetic**  
   Correction: compare against nearby known values (e.g., near-Earth gravity scale, inverse-square trend).

## 9) Guided practice set (10-12 min)
1. Compute gravitational acceleration at \(r=8000\ \text{km}\) around Earth.
2. A spacecraft moves from \(r_1\) to \(1.5r_1\). Find \(a_2/a_1\).
3. Write \(\mathbf{a}\) for \(\mathbf{r}=[0,-r,0]\) and interpret direction.
4. List two assumptions that become weak for long-duration LEO prediction.
5. Explain in 2-3 sentences why two-body remains useful in mission pre-design.

### Guided practice solutions (attempt the practice set first, then self-check)
1. **Acceleration at \(r=8000\ \text{km}\):** \(r=8.0\times10^6\ \text{m}\), so
\[
a=\frac{3.986\times10^{14}}{(8.0\times10^6)^2}\approx6.23\ \text{m/s}^2
\]
2. **Ratio of accelerations at different radii:**
\[
\frac{a_2}{a_1}=\left(\frac{r_1}{1.5r_1}\right)^2=\frac{1}{2.25}\approx0.444
\]
3. **Vector acceleration for \(\mathbf{r}=[0,-r,0]\):**
\[
\mathbf{a}=-\mu\frac{[0,-r,0]}{r^3}
=\left[0,\frac{\mu r}{r^3},0\right]
=\left[0,\frac{\mu}{r^2},0\right]
\]
The \(y\)-component is positive because the leading negative from \(-\mu\) multiplies the negative \(y\)-position value \((-r)\), and those two negatives cancel.
This points in \(+\mathbf{y}\), i.e., back toward origin if the spacecraft is at negative \(y\).

## 10) Exit ticket (5-7 min)
1. Name two two-body assumptions and one practical limitation.
2. Compute \(a\) at a provided radius with correct units and sign reasoning.
3. State one check you would run to verify your answer is physically plausible.

### Success criteria
- equations selected correctly
- SI units used consistently
- direction/sign justified clearly
- model limitation communicated accurately

## 11) Next lecture bridge (2-3 min)
Lecture 02 uses this acceleration framework to build conic geometry vocabulary (ellipse, parabola, hyperbola) and connect motion state to orbit-shape interpretation.

Before Lecture 02, quickly review:
- unit consistency workflow
- inward direction logic in \(\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3\)
- the difference between a useful model and a complete model
