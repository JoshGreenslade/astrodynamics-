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
1. $a=\mu/r^2$ produces units of $\text{m/s}^2$.
2. If $\mathbf{r}$ points from Earth to spacecraft, then gravitational acceleration is in the $+\mathbf{r}$ direction.
3. Using km for $r$ while keeping $\mu$ in m$^3$/s$^2$ is acceptable if done consistently in one line.

Quick answers to self-check:
1. Valid.
2. Invalid (gravity is inward, so opposite $\mathbf{r}$).
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
- **Standard gravitational parameter ($\mu$)**: $GM$, often known for the central body.
- **Two-body model**: system where only mutual gravity of two bodies is considered.
- **Perturbation**: effect excluded from ideal model (drag, oblateness, third bodies, thrust).

## 5) Core derivation and interpretation (20-25 min)
### 5.1 Newton’s universal gravitation (force magnitude)
\[
F = G \frac{m_1 m_2}{r^2}
\]

Where:
- $F$: force magnitude (N)
- $G$: universal gravitational constant
- $m_1,m_2$: interacting masses (kg)
- $r$: center-to-center separation distance (m)

Physical interpretation:
- inverse-square dependence: doubling distance quarters force
- symmetry: each body exerts equal and opposite gravitational force on the other
- attraction: force direction is inward along line of centers

### 5.2 Convert to acceleration form for spacecraft dynamics
Now let’s make this operational for trajectory work.
For spacecraft mass $m$ near central body mass $M$:
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
Let $\mathbf{r}$ point from central body to spacecraft.
Then gravity must point back toward the central body:
\[
\mathbf{a} = -\mu\frac{\mathbf{r}}{\|\mathbf{r}\|^3}
\]
The negative sign is not optional; it encodes inward direction.

Unit check:
- $\mu$: $\text{m}^3/\text{s}^2$
- $\mathbf{r}/\|\mathbf{r}\|^3$: 1/m$^2$
- product: $\text{m/s}^2$ (correct acceleration unit)

### 5.4 Why the denominator is $\|\mathbf{r}\|^3$ in vector form
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
So $\|\mathbf{r}\|^3$ is not mysterious; it comes from $r^2$ in magnitude times another $r$ from unit-vector normalization.

## 6) Two-body assumptions and model boundaries
### 6.1 Assumptions
1. Only two bodies interact gravitationally.
2. Bodies are point masses or spherically symmetric.
3. No non-gravitational forces (thrust, drag, SRP).
4. No aspherical harmonics (e.g., $J_2$) in this baseline model.
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
Given Earth $\mu = 3.986\times10^{14}\ \text{m}^3/\text{s}^2$, find $a$ at $r=7000\ \text{km}$.

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
Check: less than $g_0\approx9.81\ \text{m/s}^2$, physically reasonable.

### Example B — inverse-square scaling check
If radius doubles from $r$ to $2r$:
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
With $\mu=3.986\times10^{14}\ \text{m}^3/\text{s}^2$:
\[
\mathbf{a}\approx[-2.88,\ -2.88,\ 0]\ \text{m/s}^2
\]
Magnitude check:
\[
\|\mathbf{a}\|\approx4.07\ \text{m/s}^2
\]
and this equals $\mu/\|\mathbf{r}\|^2$, as expected.

## 8) Common mistakes and corrections
1. **Unit mismatch (km vs m)**
   Correction: convert all distances to meters when using SI $\mu$.
2. **Missing negative sign in vector form**
   Correction: draw direction arrow before writing final expression.
3. **Assuming model is “wrong” because it is simplified**
   Correction: describe assumptions explicitly and state intended use range.
4. **Using force formula when acceleration formula is sufficient**
   Correction: use $a=\mu/r^2$ unless force on a known mass is specifically needed.
5. **Forgetting to distinguish $\mathbf{r}$ from $\|\mathbf{r}\|$**
   Correction: write scalar and vector equations on separate lines before combining.
6. **No plausibility check after arithmetic**
   Correction: compare against nearby known values (e.g., near-Earth gravity scale, inverse-square trend).

## 9) Guided practice set (10-12 min)
1. Compute gravitational acceleration at $r=8000\ \text{km}$ around Earth.
2. A spacecraft moves from $r_1$ to $1.5r_1$. Find $a_2/a_1$.
3. Write $\mathbf{a}$ for $\mathbf{r}=[0,-r,0]$ and interpret direction.
4. List two assumptions that become weak for long-duration LEO prediction.
5. Explain in 2-3 sentences why two-body remains useful in mission pre-design.

### Guided practice solutions (attempt the practice set first, then self-check)
1. **Acceleration at $r=8000\ \text{km}$:** $r=8.0\times10^6\ \text{m}$, so
\[
a=\frac{3.986\times10^{14}}{(8.0\times10^6)^2}\approx6.23\ \text{m/s}^2
\]
2. **Ratio of accelerations at different radii:**
\[
\frac{a_2}{a_1}=\left(\frac{r_1}{1.5r_1}\right)^2=\frac{1}{2.25}\approx0.444
\]
3. **Vector acceleration for $\mathbf{r}=[0,-r,0]$:**
\[
\mathbf{a}=-\mu\frac{[0,-r,0]}{r^3}
=\left[0,\frac{\mu r}{r^3},0\right]
=\left[0,\frac{\mu}{r^2},0\right]
\]
The $y$-component is positive because $-\mu$ multiplies the vector component $-r$, and those two negatives cancel to produce $+\mu/r^2$.
This points in $+\mathbf{y}$, i.e., back toward origin if the spacecraft is at negative $y$.

## 10) Exit ticket (5-7 min)
1. Name two two-body assumptions and one practical limitation.
2. Compute $a$ at a provided radius with correct units and sign reasoning.
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
- inward direction logic in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$
- the difference between a useful model and a complete model

## 12) Extended lecture narrative for full one-hour depth
Use this section as the spoken-teaching backbone so the lecture remains explanatory, paced, and interactive for roughly one hour.

### 12.1 Conversational concept walk-through (teacher script)

#### Teaching moment 1
- Teacher: "Why does gravity point inward?"
- Student: "So what is the one-line answer?"
- Teacher: "Because the force is attractive, and attraction means the acceleration vector must point toward the central mass, not away from it."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 2
- Teacher: "Why does spacecraft mass cancel out?"
- Student: "So what is the one-line answer?"
- Teacher: "Newton's second law gives $F=ma$, and gravity gives $F=GMm/r^2$; dividing by $m$ leaves $a=GM/r^2$."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 3
- Teacher: "Why is $\mu$ so useful?"
- Student: "So what is the one-line answer?"
- Teacher: "It bundles $G$ and $M$ into one constant for a body, reducing both notation clutter and arithmetic mistakes."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 4
- Teacher: "Why do we keep writing units?"
- Student: "So what is the one-line answer?"
- Teacher: "Because most orbit errors in beginner work are unit errors, not conceptual errors."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 5
- Teacher: "Why is there a minus sign in vector gravity?"
- Student: "So what is the one-line answer?"
- Teacher: "Because $\mathbf{r}$ points outward from Earth to spacecraft, while gravity points inward from spacecraft to Earth."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 6
- Teacher: "Why does inverse-square matter physically?"
- Student: "So what is the one-line answer?"
- Teacher: "It explains why acceleration decreases quickly with distance and why high orbits feel much weaker gravity."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 7
- Teacher: "Why are assumptions not a weakness?"
- Student: "So what is the one-line answer?"
- Teacher: "Assumptions are design choices: they define what we keep so we can solve the problem and reason from it."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 8
- Teacher: "Why does two-body still matter in real missions?"
- Student: "So what is the one-line answer?"
- Teacher: "It is the first-order skeleton used before perturbations and control effects are layered in."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 9
- Teacher: "Why do we check plausibility after computing?"
- Student: "So what is the one-line answer?"
- Teacher: "Math can be syntactically correct yet physically absurd; plausibility checks protect us from silent mistakes."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 10
- Teacher: "Why separate scalar and vector equations?"
- Student: "So what is the one-line answer?"
- Teacher: "Separating them prevents sign confusion and clarifies when we are describing size versus direction."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 11
- Teacher: "Why does gravity point inward?"
- Student: "So what is the one-line answer?"
- Teacher: "Because the force is attractive, and attraction means the acceleration vector must point toward the central mass, not away from it."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 12
- Teacher: "Why does spacecraft mass cancel out?"
- Student: "So what is the one-line answer?"
- Teacher: "Newton's second law gives $F=ma$, and gravity gives $F=GMm/r^2$; dividing by $m$ leaves $a=GM/r^2$."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 13
- Teacher: "Why is $\mu$ so useful?"
- Student: "So what is the one-line answer?"
- Teacher: "It bundles $G$ and $M$ into one constant for a body, reducing both notation clutter and arithmetic mistakes."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 14
- Teacher: "Why do we keep writing units?"
- Student: "So what is the one-line answer?"
- Teacher: "Because most orbit errors in beginner work are unit errors, not conceptual errors."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 15
- Teacher: "Why is there a minus sign in vector gravity?"
- Student: "So what is the one-line answer?"
- Teacher: "Because $\mathbf{r}$ points outward from Earth to spacecraft, while gravity points inward from spacecraft to Earth."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 16
- Teacher: "Why does inverse-square matter physically?"
- Student: "So what is the one-line answer?"
- Teacher: "It explains why acceleration decreases quickly with distance and why high orbits feel much weaker gravity."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 17
- Teacher: "Why are assumptions not a weakness?"
- Student: "So what is the one-line answer?"
- Teacher: "Assumptions are design choices: they define what we keep so we can solve the problem and reason from it."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 18
- Teacher: "Why does two-body still matter in real missions?"
- Student: "So what is the one-line answer?"
- Teacher: "It is the first-order skeleton used before perturbations and control effects are layered in."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 19
- Teacher: "Why do we check plausibility after computing?"
- Student: "So what is the one-line answer?"
- Teacher: "Math can be syntactically correct yet physically absurd; plausibility checks protect us from silent mistakes."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 20
- Teacher: "Why separate scalar and vector equations?"
- Student: "So what is the one-line answer?"
- Teacher: "Separating them prevents sign confusion and clarifies when we are describing size versus direction."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 21
- Teacher: "Why does gravity point inward?"
- Student: "So what is the one-line answer?"
- Teacher: "Because the force is attractive, and attraction means the acceleration vector must point toward the central mass, not away from it."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 22
- Teacher: "Why does spacecraft mass cancel out?"
- Student: "So what is the one-line answer?"
- Teacher: "Newton's second law gives $F=ma$, and gravity gives $F=GMm/r^2$; dividing by $m$ leaves $a=GM/r^2$."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 23
- Teacher: "Why is $\mu$ so useful?"
- Student: "So what is the one-line answer?"
- Teacher: "It bundles $G$ and $M$ into one constant for a body, reducing both notation clutter and arithmetic mistakes."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 24
- Teacher: "Why do we keep writing units?"
- Student: "So what is the one-line answer?"
- Teacher: "Because most orbit errors in beginner work are unit errors, not conceptual errors."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 25
- Teacher: "Why is there a minus sign in vector gravity?"
- Student: "So what is the one-line answer?"
- Teacher: "Because $\mathbf{r}$ points outward from Earth to spacecraft, while gravity points inward from spacecraft to Earth."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 26
- Teacher: "Why does inverse-square matter physically?"
- Student: "So what is the one-line answer?"
- Teacher: "It explains why acceleration decreases quickly with distance and why high orbits feel much weaker gravity."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 27
- Teacher: "Why are assumptions not a weakness?"
- Student: "So what is the one-line answer?"
- Teacher: "Assumptions are design choices: they define what we keep so we can solve the problem and reason from it."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 28
- Teacher: "Why does two-body still matter in real missions?"
- Student: "So what is the one-line answer?"
- Teacher: "It is the first-order skeleton used before perturbations and control effects are layered in."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 29
- Teacher: "Why do we check plausibility after computing?"
- Student: "So what is the one-line answer?"
- Teacher: "Math can be syntactically correct yet physically absurd; plausibility checks protect us from silent mistakes."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 30
- Teacher: "Why separate scalar and vector equations?"
- Student: "So what is the one-line answer?"
- Teacher: "Separating them prevents sign confusion and clarifies when we are describing size versus direction."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 31
- Teacher: "Why does gravity point inward?"
- Student: "So what is the one-line answer?"
- Teacher: "Because the force is attractive, and attraction means the acceleration vector must point toward the central mass, not away from it."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 32
- Teacher: "Why does spacecraft mass cancel out?"
- Student: "So what is the one-line answer?"
- Teacher: "Newton's second law gives $F=ma$, and gravity gives $F=GMm/r^2$; dividing by $m$ leaves $a=GM/r^2$."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 33
- Teacher: "Why is $\mu$ so useful?"
- Student: "So what is the one-line answer?"
- Teacher: "It bundles $G$ and $M$ into one constant for a body, reducing both notation clutter and arithmetic mistakes."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 34
- Teacher: "Why do we keep writing units?"
- Student: "So what is the one-line answer?"
- Teacher: "Because most orbit errors in beginner work are unit errors, not conceptual errors."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 35
- Teacher: "Why is there a minus sign in vector gravity?"
- Student: "So what is the one-line answer?"
- Teacher: "Because $\mathbf{r}$ points outward from Earth to spacecraft, while gravity points inward from spacecraft to Earth."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 36
- Teacher: "Why does inverse-square matter physically?"
- Student: "So what is the one-line answer?"
- Teacher: "It explains why acceleration decreases quickly with distance and why high orbits feel much weaker gravity."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 37
- Teacher: "Why are assumptions not a weakness?"
- Student: "So what is the one-line answer?"
- Teacher: "Assumptions are design choices: they define what we keep so we can solve the problem and reason from it."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 38
- Teacher: "Why does two-body still matter in real missions?"
- Student: "So what is the one-line answer?"
- Teacher: "It is the first-order skeleton used before perturbations and control effects are layered in."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 39
- Teacher: "Why do we check plausibility after computing?"
- Student: "So what is the one-line answer?"
- Teacher: "Math can be syntactically correct yet physically absurd; plausibility checks protect us from silent mistakes."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

#### Teaching moment 40
- Teacher: "Why separate scalar and vector equations?"
- Student: "So what is the one-line answer?"
- Teacher: "Separating them prevents sign confusion and clarifies when we are describing size versus direction."
- Teacher: "Now say it back in your own words before we continue."
- Teacher check: Ask for one physical interpretation, not only an algebraic restatement.
- Quick board note: Keep direction arrows explicit before writing the final sign.

### 12.2 Stepwise derivation drill (slow and explicit)

#### Derivation drill set 1
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 2
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 3
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 4
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 5
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 6
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 7
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 8
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 9
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 10
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 11
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 12
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 13
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 14
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 15
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 16
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 17
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 18
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 19
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 20
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 21
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 22
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 23
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 24
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

#### Derivation drill set 25
1. Start from Newtonian gravitation in magnitude form: $F=Gm_1m_2/r^2$.
2. Specialize to central body and spacecraft: $F=GMm/r^2$.
3. Apply Newton second law to spacecraft: $F=ma$.
4. Set the two force expressions equal: $ma=GMm/r^2$.
5. Divide both sides by $m$ carefully and note that $m
eq0$.
6. Obtain acceleration magnitude: $a=GM/r^2$.
7. Define standard gravitational parameter: $\mu=GM$.
8. Rewrite compactly: $a=\mu/r^2$.
9. Move to vector form using outward position vector $\mathbf{r}$.
10. Use inward unit vector: $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
11. Combine magnitude and inward direction.
12. Result: $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
13. Run dimensional check to verify $m/s^2$.
14. Run sign check by testing on +x and -y locations.
15. State modeling assumption before applying numerically.
Reflection prompt: Which step feels purely algebraic and which step encodes physical direction?

### 12.3 Extended worked example bank (fully narrated)

#### Example bank item 1: Low Earth reference point
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=6.800e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx8.6202\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute $a=\mu/r^2$, compare against $g_0$, and explain why the value is still substantial.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 2: Circular-orbit scale check
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=7.560e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx6.9742\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and immediately check whether direction points to Earth center in vector form.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 3: Higher circular altitude
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=9.280e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx4.6285\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and explain inverse-square drop relative to 7000 km.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 4: Mid-altitude case
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=1.240e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx2.5924\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and discuss mission-planning implication for orbital period intuition.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 5: Near GEO-radius scale
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=5.566e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx0.1287\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and interpret why station-keeping logic still requires central gravity baseline.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 6: Low Earth reference point
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=9.520e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx4.3981\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute $a=\mu/r^2$, compare against $g_0$, and explain why the value is still substantial.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 7: Circular-orbit scale check
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=7.000e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx8.1347\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and immediately check whether direction points to Earth center in vector form.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 8: Higher circular altitude
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=8.640e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx5.3396\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and explain inverse-square drop relative to 7000 km.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 9: Mid-altitude case
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=1.160e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx2.9622\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and discuss mission-planning implication for orbital period intuition.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 10: Near GEO-radius scale
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=5.228e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx0.1458\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and interpret why station-keeping logic still requires central gravity baseline.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 11: Low Earth reference point
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=8.976e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx4.9473\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute $a=\mu/r^2$, compare against $g_0$, and explain why the value is still substantial.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 12: Circular-orbit scale check
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=9.800e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx4.1504\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and immediately check whether direction points to Earth center in vector form.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 13: Higher circular altitude
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=8.000e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx6.2281\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and explain inverse-square drop relative to 7000 km.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 14: Mid-altitude case
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=1.080e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx3.4174\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and discuss mission-planning implication for orbital period intuition.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 15: Near GEO-radius scale
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=4.891e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx0.1666\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and interpret why station-keeping logic still requires central gravity baseline.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 16: Low Earth reference point
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=8.432e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx5.6063\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute $a=\mu/r^2$, compare against $g_0$, and explain why the value is still substantial.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 17: Circular-orbit scale check
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=9.240e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx4.6687\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and immediately check whether direction points to Earth center in vector form.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 18: Higher circular altitude
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=1.120e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx3.1776\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and explain inverse-square drop relative to 7000 km.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 19: Mid-altitude case
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=1.000e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx3.9860\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and discuss mission-planning implication for orbital period intuition.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 20: Near GEO-radius scale
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=4.554e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx0.1922\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and interpret why station-keeping logic still requires central gravity baseline.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 21: Low Earth reference point
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=7.888e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx6.4062\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute $a=\mu/r^2$, compare against $g_0$, and explain why the value is still substantial.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 22: Circular-orbit scale check
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=8.680e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx5.2905\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and immediately check whether direction points to Earth center in vector form.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 23: Higher circular altitude
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=1.056e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx3.5745\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and explain inverse-square drop relative to 7000 km.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 24: Mid-altitude case
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=1.400e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx2.0337\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and discuss mission-planning implication for orbital period intuition.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 25: Near GEO-radius scale
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=4.216e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx0.2242\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and interpret why station-keeping logic still requires central gravity baseline.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 26: Low Earth reference point
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=7.344e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx7.3905\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute $a=\mu/r^2$, compare against $g_0$, and explain why the value is still substantial.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 27: Circular-orbit scale check
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=8.120e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx6.0454\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and immediately check whether direction points to Earth center in vector form.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 28: Higher circular altitude
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=9.920e+06\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx4.0505\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and explain inverse-square drop relative to 7000 km.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 29: Mid-altitude case
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=1.320e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx2.2876\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and discuss mission-planning implication for orbital period intuition.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

#### Example bank item 30: Near GEO-radius scale
- Given: Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
- Radius used: $r=5.903e+07\,m$.
- Step 1: Write governing equation first: $a=\mu/r^2$.
- Step 2: Square radius before dividing so arithmetic order is explicit.
- Step 3: Substitute values to obtain $a\approx0.1144\,m/s^2$.
- Step 4: Perform reasonableness check against nearby known values.
- Interpretation: Compute acceleration and interpret why station-keeping logic still requires central gravity baseline.
- Direction reminder: acceleration vector always points toward the central body.
- Unit reminder: never mix km with SI $\mu$ unless converted first.

### 12.4 Misconception clinic (explain and repair)

#### Misconception repair 1
- Student claim: "I got a negative scalar acceleration so gravity is repulsive."
- Instructor repair: Scalar magnitude should be nonnegative; direction belongs in vector sign, not scalar size.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 2
- Student claim: "$\mathbf{r}$ and $r$ are the same thing."
- Instructor repair: $\mathbf{r}$ is a vector, while $r=\|\mathbf{r}\|$ is its scalar magnitude.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 3
- Student claim: "If assumptions are not perfect, the model is useless."
- Instructor repair: A model can be useful within scope; two-body is the baseline that later corrections build on.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 4
- Student claim: "I can skip units because the formula is standard."
- Instructor repair: Skipping units hides conversion errors; include units every substitution line.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 5
- Student claim: "The minus sign is optional if I know direction mentally."
- Instructor repair: Write the sign explicitly to avoid hidden directional mistakes in multi-axis problems.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 6
- Student claim: "I got a negative scalar acceleration so gravity is repulsive."
- Instructor repair: Scalar magnitude should be nonnegative; direction belongs in vector sign, not scalar size.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 7
- Student claim: "$\mathbf{r}$ and $r$ are the same thing."
- Instructor repair: $\mathbf{r}$ is a vector, while $r=\|\mathbf{r}\|$ is its scalar magnitude.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 8
- Student claim: "If assumptions are not perfect, the model is useless."
- Instructor repair: A model can be useful within scope; two-body is the baseline that later corrections build on.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 9
- Student claim: "I can skip units because the formula is standard."
- Instructor repair: Skipping units hides conversion errors; include units every substitution line.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 10
- Student claim: "The minus sign is optional if I know direction mentally."
- Instructor repair: Write the sign explicitly to avoid hidden directional mistakes in multi-axis problems.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 11
- Student claim: "I got a negative scalar acceleration so gravity is repulsive."
- Instructor repair: Scalar magnitude should be nonnegative; direction belongs in vector sign, not scalar size.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 12
- Student claim: "$\mathbf{r}$ and $r$ are the same thing."
- Instructor repair: $\mathbf{r}$ is a vector, while $r=\|\mathbf{r}\|$ is its scalar magnitude.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 13
- Student claim: "If assumptions are not perfect, the model is useless."
- Instructor repair: A model can be useful within scope; two-body is the baseline that later corrections build on.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 14
- Student claim: "I can skip units because the formula is standard."
- Instructor repair: Skipping units hides conversion errors; include units every substitution line.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 15
- Student claim: "The minus sign is optional if I know direction mentally."
- Instructor repair: Write the sign explicitly to avoid hidden directional mistakes in multi-axis problems.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 16
- Student claim: "I got a negative scalar acceleration so gravity is repulsive."
- Instructor repair: Scalar magnitude should be nonnegative; direction belongs in vector sign, not scalar size.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 17
- Student claim: "$\mathbf{r}$ and $r$ are the same thing."
- Instructor repair: $\mathbf{r}$ is a vector, while $r=\|\mathbf{r}\|$ is its scalar magnitude.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 18
- Student claim: "If assumptions are not perfect, the model is useless."
- Instructor repair: A model can be useful within scope; two-body is the baseline that later corrections build on.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 19
- Student claim: "I can skip units because the formula is standard."
- Instructor repair: Skipping units hides conversion errors; include units every substitution line.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 20
- Student claim: "The minus sign is optional if I know direction mentally."
- Instructor repair: Write the sign explicitly to avoid hidden directional mistakes in multi-axis problems.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 21
- Student claim: "I got a negative scalar acceleration so gravity is repulsive."
- Instructor repair: Scalar magnitude should be nonnegative; direction belongs in vector sign, not scalar size.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 22
- Student claim: "$\mathbf{r}$ and $r$ are the same thing."
- Instructor repair: $\mathbf{r}$ is a vector, while $r=\|\mathbf{r}\|$ is its scalar magnitude.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 23
- Student claim: "If assumptions are not perfect, the model is useless."
- Instructor repair: A model can be useful within scope; two-body is the baseline that later corrections build on.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 24
- Student claim: "I can skip units because the formula is standard."
- Instructor repair: Skipping units hides conversion errors; include units every substitution line.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 25
- Student claim: "The minus sign is optional if I know direction mentally."
- Instructor repair: Write the sign explicitly to avoid hidden directional mistakes in multi-axis problems.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 26
- Student claim: "I got a negative scalar acceleration so gravity is repulsive."
- Instructor repair: Scalar magnitude should be nonnegative; direction belongs in vector sign, not scalar size.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 27
- Student claim: "$\mathbf{r}$ and $r$ are the same thing."
- Instructor repair: $\mathbf{r}$ is a vector, while $r=\|\mathbf{r}\|$ is its scalar magnitude.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 28
- Student claim: "If assumptions are not perfect, the model is useless."
- Instructor repair: A model can be useful within scope; two-body is the baseline that later corrections build on.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 29
- Student claim: "I can skip units because the formula is standard."
- Instructor repair: Skipping units hides conversion errors; include units every substitution line.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 30
- Student claim: "The minus sign is optional if I know direction mentally."
- Instructor repair: Write the sign explicitly to avoid hidden directional mistakes in multi-axis problems.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 31
- Student claim: "I got a negative scalar acceleration so gravity is repulsive."
- Instructor repair: Scalar magnitude should be nonnegative; direction belongs in vector sign, not scalar size.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 32
- Student claim: "$\mathbf{r}$ and $r$ are the same thing."
- Instructor repair: $\mathbf{r}$ is a vector, while $r=\|\mathbf{r}\|$ is its scalar magnitude.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 33
- Student claim: "If assumptions are not perfect, the model is useless."
- Instructor repair: A model can be useful within scope; two-body is the baseline that later corrections build on.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 34
- Student claim: "I can skip units because the formula is standard."
- Instructor repair: Skipping units hides conversion errors; include units every substitution line.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 35
- Student claim: "The minus sign is optional if I know direction mentally."
- Instructor repair: Write the sign explicitly to avoid hidden directional mistakes in multi-axis problems.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 36
- Student claim: "I got a negative scalar acceleration so gravity is repulsive."
- Instructor repair: Scalar magnitude should be nonnegative; direction belongs in vector sign, not scalar size.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 37
- Student claim: "$\mathbf{r}$ and $r$ are the same thing."
- Instructor repair: $\mathbf{r}$ is a vector, while $r=\|\mathbf{r}\|$ is its scalar magnitude.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 38
- Student claim: "If assumptions are not perfect, the model is useless."
- Instructor repair: A model can be useful within scope; two-body is the baseline that later corrections build on.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 39
- Student claim: "I can skip units because the formula is standard."
- Instructor repair: Skipping units hides conversion errors; include units every substitution line.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

#### Misconception repair 40
- Student claim: "The minus sign is optional if I know direction mentally."
- Instructor repair: Write the sign explicitly to avoid hidden directional mistakes in multi-axis problems.
- Verification question: Can you test this with a point on +x or -y and confirm direction?
- Transfer question: How would this mistake affect a full trajectory propagation?

### 12.5 Guided problem set with structured solutions

#### Guided problem 1
Prompt A: Compute acceleration magnitude at $r=6750\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=6.750e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx8.7484\,m/s^2$.
4. Scaling check example: for $k=1.12$, ratio $a_2/a_1=1/k^2\approx0.7972$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 2
Prompt A: Compute acceleration magnitude at $r=7000\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=7.000e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx8.1347\,m/s^2$.
4. Scaling check example: for $k=1.14$, ratio $a_2/a_1=1/k^2\approx0.7695$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 3
Prompt A: Compute acceleration magnitude at $r=7250\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=7.250e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx7.5834\,m/s^2$.
4. Scaling check example: for $k=1.16$, ratio $a_2/a_1=1/k^2\approx0.7432$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 4
Prompt A: Compute acceleration magnitude at $r=7500\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=7.500e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx7.0862\,m/s^2$.
4. Scaling check example: for $k=1.18$, ratio $a_2/a_1=1/k^2\approx0.7182$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 5
Prompt A: Compute acceleration magnitude at $r=7750\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=7.750e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx6.6364\,m/s^2$.
4. Scaling check example: for $k=1.20$, ratio $a_2/a_1=1/k^2\approx0.6944$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 6
Prompt A: Compute acceleration magnitude at $r=8000\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=8.000e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx6.2281\,m/s^2$.
4. Scaling check example: for $k=1.22$, ratio $a_2/a_1=1/k^2\approx0.6719$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 7
Prompt A: Compute acceleration magnitude at $r=8250\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=8.250e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx5.8564\,m/s^2$.
4. Scaling check example: for $k=1.24$, ratio $a_2/a_1=1/k^2\approx0.6504$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 8
Prompt A: Compute acceleration magnitude at $r=8500\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=8.500e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx5.5170\,m/s^2$.
4. Scaling check example: for $k=1.26$, ratio $a_2/a_1=1/k^2\approx0.6299$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 9
Prompt A: Compute acceleration magnitude at $r=8750\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=8.750e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx5.2062\,m/s^2$.
4. Scaling check example: for $k=1.28$, ratio $a_2/a_1=1/k^2\approx0.6104$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 10
Prompt A: Compute acceleration magnitude at $r=9000\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=9.000e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx4.9210\,m/s^2$.
4. Scaling check example: for $k=1.30$, ratio $a_2/a_1=1/k^2\approx0.5917$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 11
Prompt A: Compute acceleration magnitude at $r=9250\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=9.250e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx4.6586\,m/s^2$.
4. Scaling check example: for $k=1.32$, ratio $a_2/a_1=1/k^2\approx0.5739$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 12
Prompt A: Compute acceleration magnitude at $r=9500\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=9.500e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx4.4166\,m/s^2$.
4. Scaling check example: for $k=1.34$, ratio $a_2/a_1=1/k^2\approx0.5569$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 13
Prompt A: Compute acceleration magnitude at $r=9750\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=9.750e+06\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx4.1930\,m/s^2$.
4. Scaling check example: for $k=1.36$, ratio $a_2/a_1=1/k^2\approx0.5407$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 14
Prompt A: Compute acceleration magnitude at $r=10000\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.000e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx3.9860\,m/s^2$.
4. Scaling check example: for $k=1.38$, ratio $a_2/a_1=1/k^2\approx0.5251$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 15
Prompt A: Compute acceleration magnitude at $r=10250\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.025e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx3.7939\,m/s^2$.
4. Scaling check example: for $k=1.40$, ratio $a_2/a_1=1/k^2\approx0.5102$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 16
Prompt A: Compute acceleration magnitude at $r=10500\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.050e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx3.6154\,m/s^2$.
4. Scaling check example: for $k=1.42$, ratio $a_2/a_1=1/k^2\approx0.4959$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 17
Prompt A: Compute acceleration magnitude at $r=10750\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.075e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx3.4492\,m/s^2$.
4. Scaling check example: for $k=1.44$, ratio $a_2/a_1=1/k^2\approx0.4823$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 18
Prompt A: Compute acceleration magnitude at $r=11000\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.100e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx3.2942\,m/s^2$.
4. Scaling check example: for $k=1.46$, ratio $a_2/a_1=1/k^2\approx0.4691$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 19
Prompt A: Compute acceleration magnitude at $r=11250\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.125e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx3.1494\,m/s^2$.
4. Scaling check example: for $k=1.48$, ratio $a_2/a_1=1/k^2\approx0.4565$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 20
Prompt A: Compute acceleration magnitude at $r=11500\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.150e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx3.0140\,m/s^2$.
4. Scaling check example: for $k=1.50$, ratio $a_2/a_1=1/k^2\approx0.4444$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 21
Prompt A: Compute acceleration magnitude at $r=11750\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.175e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx2.8871\,m/s^2$.
4. Scaling check example: for $k=1.52$, ratio $a_2/a_1=1/k^2\approx0.4328$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 22
Prompt A: Compute acceleration magnitude at $r=12000\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.200e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx2.7681\,m/s^2$.
4. Scaling check example: for $k=1.54$, ratio $a_2/a_1=1/k^2\approx0.4217$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 23
Prompt A: Compute acceleration magnitude at $r=12250\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.225e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx2.6562\,m/s^2$.
4. Scaling check example: for $k=1.56$, ratio $a_2/a_1=1/k^2\approx0.4109$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 24
Prompt A: Compute acceleration magnitude at $r=12500\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.250e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx2.5510\,m/s^2$.
4. Scaling check example: for $k=1.58$, ratio $a_2/a_1=1/k^2\approx0.4006$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 25
Prompt A: Compute acceleration magnitude at $r=12750\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.275e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx2.4520\,m/s^2$.
4. Scaling check example: for $k=1.60$, ratio $a_2/a_1=1/k^2\approx0.3906$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 26
Prompt A: Compute acceleration magnitude at $r=13000\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.300e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx2.3586\,m/s^2$.
4. Scaling check example: for $k=1.62$, ratio $a_2/a_1=1/k^2\approx0.3810$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 27
Prompt A: Compute acceleration magnitude at $r=13250\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.325e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx2.2704\,m/s^2$.
4. Scaling check example: for $k=1.64$, ratio $a_2/a_1=1/k^2\approx0.3718$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 28
Prompt A: Compute acceleration magnitude at $r=13500\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.350e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx2.1871\,m/s^2$.
4. Scaling check example: for $k=1.66$, ratio $a_2/a_1=1/k^2\approx0.3629$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 29
Prompt A: Compute acceleration magnitude at $r=13750\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.375e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx2.1083\,m/s^2$.
4. Scaling check example: for $k=1.68$, ratio $a_2/a_1=1/k^2\approx0.3543$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

#### Guided problem 30
Prompt A: Compute acceleration magnitude at $r=14000\,km$ around Earth.
Prompt B: If radius scales by factor $k$, explain why acceleration scales by $1/k^2$.
Prompt C: State one modeling assumption you are using.
Solution sketch:
1. Convert radius: $r=1.400e+07\,m$.
2. Apply $a=\mu/r^2$ with Earth $\mu=3.986\times10^{14}\,m^3/s^2$.
3. Computed result: $a\approx2.0337\,m/s^2$.
4. Scaling check example: for $k=1.70$, ratio $a_2/a_1=1/k^2\approx0.3460$.
5. Assumption statement: only central-body gravity is active.
6. Reflection: explain in one sentence why the result should be below surface gravity for large radii.

### 12.6 Minute-by-minute pacing map (60 minutes)

- Minute 01: Minute 00-03: Reconnect to prior knowledge and set learning targets in plain language.
- Minute 02: Minute 03-07: Activation check on units, direction, and scalar-vector distinction.
- Minute 03: Minute 07-12: Derive scalar acceleration from gravitation plus Newton second law.
- Minute 04: Minute 12-18: Transition to vector form and interpret the negative sign physically.
- Minute 05: Minute 18-23: Work first numerical example with explicit unit conversion.
- Minute 06: Minute 23-28: Run inverse-square scaling intuition check and short questions.
- Minute 07: Minute 28-35: Complete 2D vector example with component-level sign validation.
- Minute 08: Minute 35-41: Pause for misconception clinic and error-repair mini drills.
- Minute 09: Minute 41-48: Guided practice with student verbal explanation at each step.
- Minute 10: Minute 48-54: Exit ticket attempt and immediate formative feedback.
- Minute 11: Minute 54-58: Summarize assumptions, limitations, and why baseline model still matters.
- Minute 12: Minute 58-60: Bridge forward to Lecture 02 and assign focused review prompts.
- Minute 13: Minute 00-03: Reconnect to prior knowledge and set learning targets in plain language.
- Minute 14: Minute 03-07: Activation check on units, direction, and scalar-vector distinction.
- Minute 15: Minute 07-12: Derive scalar acceleration from gravitation plus Newton second law.
- Minute 16: Minute 12-18: Transition to vector form and interpret the negative sign physically.
- Minute 17: Minute 18-23: Work first numerical example with explicit unit conversion.
- Minute 18: Minute 23-28: Run inverse-square scaling intuition check and short questions.
- Minute 19: Minute 28-35: Complete 2D vector example with component-level sign validation.
- Minute 20: Minute 35-41: Pause for misconception clinic and error-repair mini drills.
- Minute 21: Minute 41-48: Guided practice with student verbal explanation at each step.
- Minute 22: Minute 48-54: Exit ticket attempt and immediate formative feedback.
- Minute 23: Minute 54-58: Summarize assumptions, limitations, and why baseline model still matters.
- Minute 24: Minute 58-60: Bridge forward to Lecture 02 and assign focused review prompts.
- Minute 25: Minute 00-03: Reconnect to prior knowledge and set learning targets in plain language.
- Minute 26: Minute 03-07: Activation check on units, direction, and scalar-vector distinction.
- Minute 27: Minute 07-12: Derive scalar acceleration from gravitation plus Newton second law.
- Minute 28: Minute 12-18: Transition to vector form and interpret the negative sign physically.
- Minute 29: Minute 18-23: Work first numerical example with explicit unit conversion.
- Minute 30: Minute 23-28: Run inverse-square scaling intuition check and short questions.
- Minute 31: Minute 28-35: Complete 2D vector example with component-level sign validation.
- Minute 32: Minute 35-41: Pause for misconception clinic and error-repair mini drills.
- Minute 33: Minute 41-48: Guided practice with student verbal explanation at each step.
- Minute 34: Minute 48-54: Exit ticket attempt and immediate formative feedback.
- Minute 35: Minute 54-58: Summarize assumptions, limitations, and why baseline model still matters.
- Minute 36: Minute 58-60: Bridge forward to Lecture 02 and assign focused review prompts.
- Minute 37: Minute 00-03: Reconnect to prior knowledge and set learning targets in plain language.
- Minute 38: Minute 03-07: Activation check on units, direction, and scalar-vector distinction.
- Minute 39: Minute 07-12: Derive scalar acceleration from gravitation plus Newton second law.
- Minute 40: Minute 12-18: Transition to vector form and interpret the negative sign physically.
- Minute 41: Minute 18-23: Work first numerical example with explicit unit conversion.
- Minute 42: Minute 23-28: Run inverse-square scaling intuition check and short questions.
- Minute 43: Minute 28-35: Complete 2D vector example with component-level sign validation.
- Minute 44: Minute 35-41: Pause for misconception clinic and error-repair mini drills.
- Minute 45: Minute 41-48: Guided practice with student verbal explanation at each step.
- Minute 46: Minute 48-54: Exit ticket attempt and immediate formative feedback.
- Minute 47: Minute 54-58: Summarize assumptions, limitations, and why baseline model still matters.
- Minute 48: Minute 58-60: Bridge forward to Lecture 02 and assign focused review prompts.
- Minute 49: Minute 00-03: Reconnect to prior knowledge and set learning targets in plain language.
- Minute 50: Minute 03-07: Activation check on units, direction, and scalar-vector distinction.
- Minute 51: Minute 07-12: Derive scalar acceleration from gravitation plus Newton second law.
- Minute 52: Minute 12-18: Transition to vector form and interpret the negative sign physically.
- Minute 53: Minute 18-23: Work first numerical example with explicit unit conversion.
- Minute 54: Minute 23-28: Run inverse-square scaling intuition check and short questions.
- Minute 55: Minute 28-35: Complete 2D vector example with component-level sign validation.
- Minute 56: Minute 35-41: Pause for misconception clinic and error-repair mini drills.
- Minute 57: Minute 41-48: Guided practice with student verbal explanation at each step.
- Minute 58: Minute 48-54: Exit ticket attempt and immediate formative feedback.
- Minute 59: Minute 54-58: Summarize assumptions, limitations, and why baseline model still matters.
- Minute 60: Minute 58-60: Bridge forward to Lecture 02 and assign focused review prompts.

### 12.7 End-of-lecture verbal recap script
Use this recap to close with understanding, not just formulas.
1. We built gravity from force to acceleration so you can compute motion directly.
2. We protected meaning by separating scalar magnitude from vector direction.
3. We used $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ as the operational statement of two-body gravity.
4. We practiced unit discipline and plausibility checks to prevent common errors.
5. We named assumptions so you know when this model is reliable and when to extend it.
6. We connected everything to next lecture, where orbit geometry grows from this acceleration law.
