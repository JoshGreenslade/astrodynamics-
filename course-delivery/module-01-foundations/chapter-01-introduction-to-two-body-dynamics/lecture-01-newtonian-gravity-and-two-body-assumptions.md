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
This section is a complete teaching script that prioritizes explanation, interpretation, and student dialogue so the session consistently fills one hour with meaningful learning.

### 12.1 Guided teaching script: concept by concept
Use each segment in sequence; pause after every segment for student paraphrasing before moving on.

#### Segment 1: Physical picture first
- Instructor framing: Imagine tossing a ball sideways from a mountain while Earth curves away beneath it. Orbital motion is this same idea extended continuously.
- Prompt: Ask the student to sketch the path and identify where gravity acts at three points.
- Board action: Draw a side-view diagram and annotate velocity and inward acceleration arrows.
- Core takeaway: Gravity is not merely pulling down in the local sense; in orbital dynamics it is continuously bending velocity toward the planet center.
- Student checkpoint: explain why continuous sideways motion plus inward pull creates orbit, not straight-line fall.

#### Segment 2: Force versus acceleration
- Instructor framing: Students often memorize force equations but forget that trajectory propagation uses acceleration directly.
- Prompt: convert one force equation into a state-derivative statement in words.
- Board action: Write force form and acceleration form side-by-side, then circle the propagator-ready expression.
- Core takeaway: Translate every force statement into acceleration language so state update equations become immediate.
- Student checkpoint: state when force is still useful and when acceleration is the better operational quantity.

#### Segment 3: Meaning of standard gravitational parameter
- Instructor framing: When we write $\mu$, we are using a precomputed physical fingerprint of the central body.
- Prompt: what practical errors are reduced when using $\mu$ directly in computation?
- Board action: List constants for Earth and show how one symbol simplifies repeated calculations.
- Core takeaway: This avoids repeatedly carrying $G$ and $M$ and reduces arithmetic and transcription error risk.
- Student checkpoint: explain why two analysts using the same $\mu$ value are less likely to diverge numerically.

#### Segment 4: Direction discipline
- Instructor framing: Whenever a vector appears, draw an arrow before writing symbols.
- Prompt: identify outward and inward directions before any minus sign is written.
- Board action: Use coordinate axes and mark $\mathbf{r}$, then mark gravity as opposite direction.
- Core takeaway: This habit prevents sign mistakes that are otherwise hard to catch in numerical pipelines.
- Student checkpoint: explain how a pre-drawn arrow prevents sign mistakes in components.

#### Segment 5: Inverse-square intuition
- Instructor framing: If distance doubles, gravity does not halve; it drops to one quarter.
- Prompt: Ask for an everyday analogy of intensity spreading with distance.
- Board action: Show the ratio derivation line-by-line: $a(2r)=a(r)/4$.
- Core takeaway: That one fact explains much of orbital scaling behavior.
- Student checkpoint: give one mission implication of inverse-square decay with altitude.

#### Segment 6: Model scope statement
- Instructor framing: The two-body model is intentionally selective: it keeps dominant gravity and ignores smaller effects.
- Prompt: name one included effect and two excluded effects.
- Board action: Create a two-column board table: Included / Excluded.
- Core takeaway: A good engineer states what is ignored before claiming confidence in outputs.
- Student checkpoint: explain when exclusions become too large to ignore.

#### Segment 7: Units as a safety net
- Instructor framing: Most student errors are unit mismatch errors masquerading as algebra errors.
- Prompt: Ask student to find the deliberate unit bug in a prepared substitution line.
- Board action: Circle units on each symbol before entering numbers.
- Core takeaway: If radius is in km and $\mu$ is SI, conversion is mandatory before substitution.
- Student checkpoint: explain the exact failure mode when km is mixed with SI $\mu$.

#### Segment 8: Scalar and vector separation
- Instructor framing: Write magnitude equations and vector equations on separate lines.
- Prompt: classify each symbol in the derivation as scalar or vector.
- Board action: Color-code scalar terms and vector terms to make structure obvious.
- Core takeaway: This keeps $r$ and $\mathbf{r}$ conceptually distinct.
- Student checkpoint: explain why $r$ and $\mathbf{r}$ cannot be swapped in formulas.

#### Segment 9: Interpreting negative sign
- Instructor framing: In $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$, the minus sign encodes inward direction.
- Prompt: Ask student to test sign by placing spacecraft at $+x$ and then at $-y$.
- Board action: Compute component signs explicitly for both test points.
- Core takeaway: Without it, the model would predict repulsion and fail physical checks immediately.
- Student checkpoint: explain why one component can be positive while still pointing inward overall.

#### Segment 10: Reasonableness checks
- Instructor framing: After every computation, compare magnitude against a known nearby value.
- Prompt: is your result in a plausible range for near-Earth orbit?
- Board action: Write a quick reference band of expected accelerations at common radii.
- Core takeaway: At near-Earth radii, values should sit in the same order as surface gravity, not hundreds of $m/s^2$.
- Student checkpoint: explain one red flag that would trigger immediate rework.

#### Segment 11: Assumptions versus limitations
- Instructor framing: Assumptions are design choices; limitations are consequences of those choices.
- Prompt: convert one assumption into a practical limitation sentence.
- Board action: Show a traceability chain: assumption -> omitted physics -> expected error behavior.
- Core takeaway: Naming both keeps communication honest and technically precise.
- Student checkpoint: explain why documenting assumptions is part of technical honesty.

#### Segment 12: Bridge to future topics
- Instructor framing: Two-body acceleration is the seed for conic orbits and Keplerian reasoning.
- Prompt: predict one thing Lecture 02 will reuse from today.
- Board action: Map today's key equation to next lecture's conic interpretation box.
- Core takeaway: If this seed is misunderstood, later orbit-geometry lessons become fragile.
- Student checkpoint: explain why weak foundations here make conic classification harder later.

### 12.2 Deep derivation notebook (fully explained)
This notebook-style walk-through is intentionally explicit so no algebraic step is treated as "obvious."

#### Notebook block 1: Start from universal gravitation
- Write the magnitude form: $F=Gm_1m_2/r^2$.
- State each symbol verbally before proceeding.
- Confirm that force points along the line connecting body centers.
- Remind that this is an attractive interaction, not repulsive.
- Student recap: summarize this block in one sentence that includes both math and meaning.

#### Notebook block 2: Specialize to central body + spacecraft
- Use $M$ for central body and $m$ for spacecraft: $F=GMm/r^2$.
- Explain why this specialization helps mission analysis context.
- Note that center-to-center distance is required, not altitude alone.
- Ask student to identify where altitude converts to radius in practice.
- Student recap: summarize this block in one sentence that includes both math and meaning.

#### Notebook block 3: Connect to Newton second law
- Write $F=ma$ for spacecraft translational motion.
- Set equations equal: $ma=GMm/r^2$.
- Divide by $m$ and explicitly state condition $m \neq 0$.
- Obtain scalar acceleration: $a=GM/r^2$.
- Student recap: summarize this block in one sentence that includes both math and meaning.

#### Notebook block 4: Introduce $\mu$ and operational form
- Define $\mu=GM$.
- Rewrite as $a=\mu/r^2$ to streamline repeated computation.
- Explain that published planetary constants often provide $\mu$ directly.
- Emphasize that this compact form is standard in orbit determination tools.
- Student recap: summarize this block in one sentence that includes both math and meaning.

#### Notebook block 5: Convert to vector form carefully
- Use outward position vector $\mathbf{r}$ from planet center to spacecraft.
- Use inward unit direction $\hat{\mathbf{r}}_{inward}=-\mathbf{r}/\|\mathbf{r}\|$.
- Combine with scalar magnitude to obtain $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$.
- Re-check direction using a simple point on +x axis.
- Student recap: summarize this block in one sentence that includes both math and meaning.

#### Notebook block 6: Dimensional audit
- Units of $\mu$ are $m^3/s^2$.
- Units of $\mathbf{r}/\|\mathbf{r}\|^3$ are $1/m^2$.
- Product gives $m/s^2$, confirming acceleration dimensions.
- Require student to say this audit aloud before finalizing answers.
- Student recap: summarize this block in one sentence that includes both math and meaning.

#### Notebook block 7: Interpretation audit
- Mathematical validity is necessary but not sufficient.
- Ask whether value trends correctly with increasing radius.
- Ask whether direction points inward for all tested coordinate cases.
- Only then accept result as physically credible.
- Student recap: summarize this block in one sentence that includes both math and meaning.

### 12.3 Extended worked examples (new, non-redundant set)
Each example includes setup, process, interpretation, and a common-error warning.

#### Example E: Low circular orbit check
- Position vector: $\mathbf{r}=[6.800e+06,0.000e+00,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=6.8000e+06\,m$.
- Scalar acceleration magnitude: $a=8.6202\,m/s^2$.
- Vector acceleration: $\mathbf{a}=[-8.6202,-0.0000,-0.0000]\,m/s^2$.
- Direction check: signs must point from spacecraft location back toward origin.
- Teaching note: LEO-like magnitude sanity check and unit discipline emphasis.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example F: Medium altitude circular check
- Position vector: $\mathbf{r}=[9.000e+06,0.000e+00,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=9.0000e+06\,m$.
- Scalar acceleration magnitude: $a=4.9210\,m/s^2$.
- Vector acceleration: $\mathbf{a}=[-4.9210,-0.0000,-0.0000]\,m/s^2$.
- Direction check: signs must point from spacecraft location back toward origin.
- Teaching note: Observe expected inverse-square drop from lower altitude.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example G: Radius increase ratio method
- Given $r_1=7.000e+06\,m$ and $r_2=1.050e+07\,m$.
- Compute using both direct formula and scaling relation $a \propto 1/r^2$.
- Direct values: $a_1=8.1347\,m/s^2$, $a_2=3.6154\,m/s^2$.
- Ratio check: $a_2/a_1=0.4444$.
- Interpretation: increasing radius reduces central acceleration predictably and nonlinearly.
- Teaching note: Compute ratio directly using scaling before direct arithmetic.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example H: 2D diagonal state vector
- Position vector: $\mathbf{r}=[7.000e+06,7.000e+06,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=9.8995e+06\,m$.
- Scalar acceleration magnitude: $a=4.0673\,m/s^2$.
- Vector acceleration: $\mathbf{a}=[-2.8760,-2.8760,-0.0000]\,m/s^2$.
- Direction check: signs must point from spacecraft location back toward origin.
- Teaching note: Practice component-level sign interpretation in two dimensions.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example I: Negative y-axis position
- Position vector: $\mathbf{r}=[0.000e+00,-8.000e+06,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=8.0000e+06\,m$.
- Scalar acceleration magnitude: $a=6.2281\,m/s^2$.
- Vector acceleration: $\mathbf{a}=[-0.0000,6.2281,-0.0000]\,m/s^2$.
- Direction check: signs must point from spacecraft location back toward origin.
- Teaching note: Show why y-acceleration becomes positive at negative y position.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example J: 3D mixed-sign vector
- Position vector: $\mathbf{r}=[6.500e+06,-4.200e+06,3.000e+06]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=8.3000e+06\,m$.
- Scalar acceleration magnitude: $a=5.7860\,m/s^2$.
- Vector acceleration: $\mathbf{a}=[-4.5312,2.9279,-2.0913]\,m/s^2$.
- Direction check: signs must point from spacecraft location back toward origin.
- Teaching note: Demonstrate inward direction across three components simultaneously.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example K: High-altitude communication orbit scale
- Position vector: $\mathbf{r}=[4.216e+07,0.000e+00,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=4.2164e+07\,m$.
- Scalar acceleration magnitude: $a=0.2242\,m/s^2$.
- Vector acceleration: $\mathbf{a}=[-0.2242,-0.0000,-0.0000]\,m/s^2$.
- Direction check: signs must point from spacecraft location back toward origin.
- Teaching note: Interpret weaker gravity while still central-body dominated.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example L: Radius perturbation sensitivity
- Given $r_1=7.200e+06\,m$ and $r_2=7.600e+06\,m$.
- Compute using both direct formula and scaling relation $a \propto 1/r^2$.
- Direct values: $a_1=7.6890\,m/s^2$, $a_2=6.9010\,m/s^2$.
- Ratio check: $a_2/a_1=0.8975$.
- Interpretation: increasing radius reduces central acceleration predictably and nonlinearly.
- Teaching note: Estimate sensitivity of acceleration to modest radius change.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example M: Unit mistake detection
- Position vector: $\mathbf{r}=[7.000e+03,0.000e+00,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=7.0000e+03\,m$.
- Scalar acceleration magnitude: $a=8134693.8776\,m/s^2$.
- Vector acceleration: $\mathbf{a}=[-8134693.8776,-0.0000,-0.0000]\,m/s^2$.
- Direction check: signs must point from spacecraft location back toward origin.
- Teaching note: Demonstrate catastrophic error if km is treated as m.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example N: Scalar/vector cross-check
- Position vector: $\mathbf{r}=[1.000e+07,-2.000e+06,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=1.0198e+07\,m$.
- Scalar acceleration magnitude: $a=3.8327\,m/s^2$.
- Vector acceleration: $\mathbf{a}=[-3.7583,0.7517,-0.0000]\,m/s^2$.
- Direction check: signs must point from spacecraft location back toward origin.
- Teaching note: Verify magnitude from components equals scalar formula.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example O: Assumption stress test
- Position vector: $\mathbf{r}=[6.800e+06,0.000e+00,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=6.8000e+06\,m$.
- Scalar acceleration magnitude: $a=8.6202\,m/s^2$.
- Vector acceleration: $\mathbf{a}=[-8.6202,-0.0000,-0.0000]\,m/s^2$.
- Direction check: signs must point from spacecraft location back toward origin.
- Teaching note: Discuss drag omission implications for low orbit predictions.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

#### Example P: Mission pre-design estimate
- Position vector: $\mathbf{r}=[1.500e+07,0.000e+00,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=1.5000e+07\,m$.
- Scalar acceleration magnitude: $a=1.7716\,m/s^2$.
- Vector acceleration: $\mathbf{a}=[-1.7716,-0.0000,-0.0000]\,m/s^2$.
- Direction check: signs must point from spacecraft location back toward origin.
- Teaching note: Use quick acceleration estimate to reason about orbital pacing.
- Common-error warning: never skip unit conversion before substitution.
- Student task: explain one physical implication of this result in plain language.

### 12.4 Socratic question bank with model answers
These prompts are designed to force explanation, not rote substitution.

#### Socratic prompt 1
- Question: Why is $\mu$ preferred over repeatedly writing $GM$?
- Model answer: It compacts constants into one body-specific parameter and reduces transcription errors.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 2
- Question: How can you explain inverse-square behavior without equations?
- Model answer: Double the distance, spread the same effect over four times the spherical area, so intensity drops to a quarter.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 3
- Question: What physical mistake happens if you omit the negative sign in vector gravity?
- Model answer: You predict outward acceleration, which contradicts attractive gravity.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 4
- Question: Why is $r$ scalar while $\mathbf{r}$ is vector?
- Model answer: $r$ is distance only; $\mathbf{r}$ includes both distance and direction.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 5
- Question: How do unit mistakes usually appear in final answers?
- Model answer: Results become unrealistically large or small compared with known orbital scales.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 6
- Question: What assumption is most fragile in very low Earth orbit?
- Model answer: Ignoring atmospheric drag is often the first assumption to break.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 7
- Question: Why do we separate model assumptions from model limitations?
- Model answer: Assumptions are choices; limitations are the consequences of those choices.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 8
- Question: How do you verbally interpret $\|\mathbf{r}\|^3$ in the denominator?
- Model answer: It combines inverse-square magnitude with vector normalization.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 9
- Question: Why does spacecraft mass cancel from acceleration?
- Model answer: Because both gravitational force and inertial resistance scale with mass, leaving mass-independent acceleration.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 10
- Question: What quick plausibility check should follow every computation?
- Model answer: Compare magnitude to nearby known values and verify direction points inward.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 11
- Question: How does central-body dominance justify a first-order model?
- Model answer: Secondary effects are smaller, so the dominant term captures core trajectory behavior first.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 12
- Question: Why is center-to-center distance required instead of altitude alone?
- Model answer: Gravity depends on radial distance from the body center, not local terrain reference.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 13
- Question: What does a positive $a_y$ mean when spacecraft is at negative $y$?
- Model answer: Acceleration is pointing back toward origin, so sign reversal is physically correct.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 14
- Question: Why are vector sketches useful before algebra?
- Model answer: They anchor direction and prevent sign drift during symbolic manipulation.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 15
- Question: How do you explain why higher orbit still has gravity?
- Model answer: Gravity weakens with distance but never becomes zero at finite radius.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 16
- Question: What is the practical role of two-body before perturbations?
- Model answer: It provides baseline motion so perturbation effects can be isolated and quantified.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 17
- Question: Why should student explanations be verbal and numeric?
- Model answer: Dual explanation shows conceptual understanding, not just calculator execution.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 18
- Question: What does dimensional analysis protect against?
- Model answer: It catches structural mistakes even when arithmetic seems clean.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 19
- Question: How can you detect vector/scalar confusion in student work?
- Model answer: They substitute a vector into scalar formulas or drop directional signs inconsistently.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 20
- Question: Why is trajectory propagation naturally acceleration-based?
- Model answer: State derivatives are built from acceleration fields, not standalone force values.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 21
- Question: Why should every answer include a model scope sentence?
- Model answer: Numerical output without scope can be misapplied beyond valid conditions.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 22
- Question: How does radius sensitivity influence mission design intuition?
- Model answer: Small altitude changes can noticeably alter acceleration and orbital timing.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 23
- Question: What happens if you use km with SI $\mu$ accidentally?
- Model answer: You introduce a million-scale error in squared terms and get invalid magnitudes.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 24
- Question: Why is one worked example never enough?
- Model answer: Different geometries reveal different failure modes and sign pitfalls.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 25
- Question: How does the two-body model support communication with teammates?
- Model answer: It gives a shared baseline language for assumptions, equations, and checks.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 26
- Question: Why is “mathematically correct” not enough in engineering?
- Model answer: Physical plausibility and domain context are part of correctness.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 27
- Question: What does “inward” mean in coordinate terms?
- Model answer: Acceleration components always oppose position components toward the origin.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 28
- Question: How do you explain model extension to perturbations?
- Model answer: Start with two-body baseline, then add drag, oblateness, or third-body terms incrementally.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 29
- Question: Why do we review prerequisite algebra explicitly?
- Model answer: Weak algebra causes hidden conceptual failures later in dynamics derivations.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

#### Socratic prompt 30
- Question: What is the key takeaway sentence for this lecture?
- Model answer: Two-body gravity gives a reliable first-order acceleration law when assumptions are explicit and checks are disciplined.
- Follow-up: give one concrete coordinate or mission example to support the explanation.

### 12.5 Long-form guided practice with varied contexts
Each task is intentionally different in framing so practice develops transfer skill, not pattern matching.

#### Practice task 1: LEO baseline computation
- Given radius: $r=7.000e+06\,m$.
- Computed acceleration magnitude: $a=8.1347\,m/s^2$.
- Include one explicit unit-conversion line before the final substitution line.
- Add one plausibility sentence comparing this value to a nearby known scale.
- Explanation target: Compute acceleration and compare with surface gravity in one sentence.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 2: Slight altitude raise
- Given radius: $r=7.600e+06\,m$.
- Computed acceleration magnitude: $a=6.9010\,m/s^2$.
- Include one explicit unit-conversion line before the final substitution line.
- Add one plausibility sentence comparing this value to a nearby known scale.
- Explanation target: Explain why the drop from the baseline is nonlinear, not linear.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 3: MEO transition intuition
- Given radius: $r=1.200e+07\,m$.
- Computed acceleration magnitude: $a=2.7681\,m/s^2$.
- Include one explicit unit-conversion line before the final substitution line.
- Add one plausibility sentence comparing this value to a nearby known scale.
- Explanation target: Describe one mission-planning implication of weaker central acceleration.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 4: Near-GEO scale check
- Given radius: $r=4.216e+07\,m$.
- Computed acceleration magnitude: $a=0.2242\,m/s^2$.
- Include one explicit unit-conversion line before the final substitution line.
- Add one plausibility sentence comparing this value to a nearby known scale.
- Explanation target: State why gravity is weaker yet still central to orbital motion.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 5: Negative-y coordinate vector check
- Position vector: $\mathbf{r}=[0.000e+00,-9.000e+06,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=9.0000e+06\,m$.
- Scalar magnitude cross-check: $a=4.9210\,m/s^2$.
- Vector result: $\mathbf{a}=[-0.0000,4.9210,-0.0000]\,m/s^2$.
- Explanation target: Explain sign of $a_y$ using geometry, not memorized rules.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 6: Mixed-sign 3D vector check
- Position vector: $\mathbf{r}=[8.000e+06,-3.000e+06,2.000e+06]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=8.7750e+06\,m$.
- Scalar magnitude cross-check: $a=5.1766\,m/s^2$.
- Vector result: $\mathbf{a}=[-4.7194,1.7698,-1.1799]\,m/s^2$.
- Explanation target: Interpret each component sign physically.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 7: Unit-conversion failure analysis
- Given radius value 7000.0. Treat this as km first, then convert correctly to meters.
- Computed acceleration magnitude: $a=8.1347\,m/s^2$.
- Include one explicit unit-conversion line before the final substitution line.
- Add one plausibility sentence comparing this value to a nearby known scale.
- Explanation target: Show what goes wrong if km is not converted before substitution.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 8: Radius ratio method
- Given radii $r_1=7.000e+06\,m$ and $r_2=1.400e+07\,m$.
- Direct values: $a_1=8.1347\,m/s^2$, $a_2=2.0337\,m/s^2$.
- Ratio: $a_2/a_1=0.2500$.
- Also compute with scaling law $a \propto 1/r^2$ and confirm agreement.
- Explanation target: Compute $a_2/a_1$ by scaling before direct arithmetic.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 9: Radius sensitivity estimate
- Given radii $r_1=8.000e+06\,m$ and $r_2=8.800e+06\,m$.
- Direct values: $a_1=6.2281\,m/s^2$, $a_2=5.1472\,m/s^2$.
- Ratio: $a_2/a_1=0.8264$.
- Also compute with scaling law $a \propto 1/r^2$ and confirm agreement.
- Explanation target: Estimate percent acceleration change and interpret it.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 10: Assumption stress case
- Given radius: $r=6.800e+06\,m$.
- Computed acceleration magnitude: $a=8.6202\,m/s^2$.
- Include one explicit unit-conversion line before the final substitution line.
- Add one plausibility sentence comparing this value to a nearby known scale.
- Explanation target: List two perturbations ignored by two-body in this regime.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 11: High-radius transfer staging
- Given radius: $r=2.000e+07\,m$.
- Computed acceleration magnitude: $a=0.9965\,m/s^2$.
- Include one explicit unit-conversion line before the final substitution line.
- Add one plausibility sentence comparing this value to a nearby known scale.
- Explanation target: Explain what this acceleration scale suggests about orbital pacing.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 12: Cross-check magnitude from vector
- Position vector: $\mathbf{r}=[1.000e+07,-4.000e+06,0.000e+00]\,m$.
- Radius magnitude: $\|\mathbf{r}\|=1.0770e+07\,m$.
- Scalar magnitude cross-check: $a=3.4362\,m/s^2$.
- Vector result: $\mathbf{a}=[-3.1904,1.2762,-0.0000]\,m/s^2$.
- Explanation target: Verify vector magnitude matches scalar formula result.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 13: Concept-only prompt
- Given radius: $r=9.000e+06\,m$.
- Computed acceleration magnitude: $a=4.9210\,m/s^2$.
- Include one explicit unit-conversion line before the final substitution line.
- Add one plausibility sentence comparing this value to a nearby known scale.
- Explanation target: Write no arithmetic first: explain expected trend, then compute.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 14: Communication prompt
- Given radius: $r=1.100e+07\,m$.
- Computed acceleration magnitude: $a=3.2942\,m/s^2$.
- Include one explicit unit-conversion line before the final substitution line.
- Add one plausibility sentence comparing this value to a nearby known scale.
- Explanation target: Summarize your result as if briefing a mission systems engineer.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

#### Practice task 15: Bridge prompt
- Given radius: $r=1.500e+07\,m$.
- Computed acceleration magnitude: $a=1.7716\,m/s^2$.
- Include one explicit unit-conversion line before the final substitution line.
- Add one plausibility sentence comparing this value to a nearby known scale.
- Explanation target: Explain how this acceleration insight prepares for conic-section interpretation.
- Assumption target: explicitly state what is neglected in the two-body model.
- Communication target: state your final answer in plain language plus equation language.

### 12.6 One-hour pacing guide (sequential, non-repeating)
This minute plan is strictly sequential and maps to a single 60-minute teaching block.

- Minute 01: Set lesson objective and relevance to mission analysis.
- Minute 02: Activate prerequisites with one quick unit question.
- Minute 03: Activate prerequisites with one vector-direction question.
- Minute 04: Clarify scalar-versus-vector notation before derivation.
- Minute 05: Write universal gravitation equation and define all symbols.
- Minute 06: Specialize to central-body plus spacecraft context.
- Minute 07: Apply Newton second law and isolate acceleration.
- Minute 08: Introduce $\mu$ and discuss why practitioners use it.
- Minute 09: Run first dimensional check aloud.
- Minute 10: Transition to vector direction and sign convention.
- Minute 11: Derive $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ step by step.
- Minute 12: Pause for student paraphrase of the derivation.
- Minute 13: Solve first scalar example together.
- Minute 14: Check result against expected near-Earth scale.
- Minute 15: Discuss inverse-square scaling with a radius-doubling thought experiment.
- Minute 16: Solve second scalar example independently.
- Minute 17: Debrief common arithmetic and unit slips.
- Minute 18: Start 2D vector example with coordinate sketch.
- Minute 19: Compute radius magnitude from components.
- Minute 20: Compute vector acceleration components.
- Minute 21: Interpret component signs physically.
- Minute 22: Cross-check vector magnitude against scalar formula.
- Minute 23: Run quick misconception poll.
- Minute 24: Repair misconception about missing minus sign.
- Minute 25: Repair misconception about $r$ versus $\mathbf{r}$.
- Minute 26: Repair misconception about assumptions and usefulness.
- Minute 27: Begin guided practice task 1 with full support.
- Minute 28: Review guided practice task 1 reasoning steps.
- Minute 29: Begin guided practice task 2 with reduced support.
- Minute 30: Ask student to explain each equation choice aloud.
- Minute 31: Begin guided practice task 3 independently.
- Minute 32: Review task 3 with focus on units.
- Minute 33: Start Socratic question round: meaning of $\mu$.
- Minute 34: Socratic round: meaning of inverse-square scaling.
- Minute 35: Socratic round: directional interpretation checks.
- Minute 36: Socratic round: assumption and limitation articulation.
- Minute 37: Present high-altitude example and interpret trend.
- Minute 38: Present negative-axis vector example and sign logic.
- Minute 39: Present mixed-sign 3D example and geometric interpretation.
- Minute 40: Summarize example patterns and transfer principles.
- Minute 41: Launch exit ticket question 1 (assumptions and limitation).
- Minute 42: Launch exit ticket question 2 (numerical acceleration).
- Minute 43: Launch exit ticket question 3 (plausibility check).
- Minute 44: Collect and review first responses.
- Minute 45: Provide corrective feedback on frequent issue #1.
- Minute 46: Provide corrective feedback on frequent issue #2.
- Minute 47: Ask student to revise one response live.
- Minute 48: Confirm revised response meets success criteria.
- Minute 49: Tie today's work to conic-section vocabulary coming next.
- Minute 50: Highlight what must be retained before Lecture 02.
- Minute 51: Restate key equation and interpretation one final time.
- Minute 52: Restate key assumption set one final time.
- Minute 53: Restate key error-check workflow one final time.
- Minute 54: Invite final conceptual question from student.
- Minute 55: Answer final conceptual question with diagram.
- Minute 56: Document one personalized remediation target.
- Minute 57: Document one personalized strength to build on.
- Minute 58: Assign short pre-lecture review for next class.
- Minute 59: Close with confidence check: "Can you explain gravity model without notes?"
- Minute 60: Confirm readiness transition to Lecture 02.

### 12.7 Closing synthesis paragraph (spoken)
Today you built a usable gravity model, not just a memorized formula list. You derived acceleration from force, tracked direction correctly with vectors, validated dimensions, and practiced realistic checks that engineers use to trust results. You also learned to treat assumptions as explicit model boundaries rather than hidden weaknesses. That combination of mathematics plus interpretation is exactly what we need before stepping into conic geometry in Lecture 02.

### 12.8 Additional explanatory notes for depth and continuity
Use this compact reference to choose the right reinforcement move for the specific learning gap you observe.

| Focus area | Reinforcement strategy | Evidence of mastery |
| --- | --- | --- |
| Unit conversion workflow | Require a full substitution line with units on every symbol before arithmetic. | Student can explain why each unit cancels and where conversion occurs. |
| Vector direction reasoning | Have the student draw $\mathbf{r}$ and $\mathbf{a}$ arrows before writing signs. | Signs in component form are consistent with the geometry sketch. |
| Inverse-square intuition | Use ratio reasoning first, then confirm with direct computation. | Student predicts trend correctly before calculator use. |
| Model scope communication | Ask for one sentence starting with "This result is valid when..." | Student states assumptions without prompting. |
| Assumption-limitation linkage | Trace one assumption to one likely error source in real flight conditions. | Student can name both assumption and consequence clearly. |
| Plausibility checks | Compare result against a nearby known orbital acceleration benchmark. | Student flags implausible magnitudes independently. |
| Coordinate sign interpretation | Evaluate one +x case and one -y case side by side. | Student explains why opposite positions flip component signs. |
| Scalar-vector separation | Color-code scalar and vector symbols in the derivation. | Student no longer interchanges $r$ and $\mathbf{r}$. |
| Derivation storytelling | Require each algebra step to be accompanied by a physical statement. | Student can narrate derivation without skipping meaning. |
| Mission-design context | Ask what the computed acceleration implies for orbit behavior or planning. | Student links numbers to practical mission reasoning. |
| Error prevention habits | Introduce a three-check routine: units, sign, plausibility. | Student uses the routine without instructor reminder. |
| Perturbation extension readiness | Ask which neglected force would be added first for a given regime. | Student selects plausible next-order correction. |

### 12.9 Extended oral-exam question bank (unique prompts)
Use these prompts to deepen explanation quality while preserving the same core model.

1. Explain why the minus sign in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ is physically required for a near-circular LEO case. Then provide one sentence describing the operational consequence.
2. Explain why $\mu$ notation improves reliability in repeated calculations for a medium-altitude transfer estimate. Then state one assumption that must remain true.
3. Explain why inverse-square scaling changes acceleration faster than many students expect for a high-altitude communication-orbit scenario. Then show one plausibility check you would run.
4. Explain why a vector sketch can prevent algebraic sign errors for a negative-y coordinate test point. Then identify one common mistake and how to avoid it.
5. Explain why dimensional analysis should be performed before accepting a numeric result for a mixed-sign 3D state vector. Then connect your explanation to what Lecture 02 will need.
6. Explain why two-body assumptions are useful without being universally valid for a quick mission pre-design briefing. Then provide one sentence describing the operational consequence.
7. Explain why radius must be center-to-center instead of altitude-alone in formulas for a student who forgot unit conversion. Then state one assumption that must remain true.
8. Explain why a result can be numerically neat yet physically implausible for a student who dropped the vector sign. Then show one plausibility check you would run.
9. Explain why scalar and vector equations should be written separately during derivation for an answer that seems too large. Then identify one common mistake and how to avoid it.
10. Explain why model limitations should be reported alongside computed values for a hand-calculation cross-check. Then connect your explanation to what Lecture 02 will need.
11. Demonstrate how the minus sign in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ is physically required for a near-circular LEO case. Then provide one sentence describing the operational consequence.
12. Demonstrate how $\mu$ notation improves reliability in repeated calculations for a medium-altitude transfer estimate. Then state one assumption that must remain true.
13. Demonstrate how inverse-square scaling changes acceleration faster than many students expect for a high-altitude communication-orbit scenario. Then show one plausibility check you would run.
14. Demonstrate how a vector sketch can prevent algebraic sign errors for a negative-y coordinate test point. Then identify one common mistake and how to avoid it.
15. Demonstrate how dimensional analysis should be performed before accepting a numeric result for a mixed-sign 3D state vector. Then connect your explanation to what Lecture 02 will need.
16. Demonstrate how two-body assumptions are useful without being universally valid for a quick mission pre-design briefing. Then provide one sentence describing the operational consequence.
17. Demonstrate how radius must be center-to-center instead of altitude-alone in formulas for a student who forgot unit conversion. Then state one assumption that must remain true.
18. Demonstrate how a result can be numerically neat yet physically implausible for a student who dropped the vector sign. Then show one plausibility check you would run.
19. Demonstrate how scalar and vector equations should be written separately during derivation for an answer that seems too large. Then identify one common mistake and how to avoid it.
20. Demonstrate how model limitations should be reported alongside computed values for a hand-calculation cross-check. Then connect your explanation to what Lecture 02 will need.
21. Defend the claim that the minus sign in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ is physically required for a near-circular LEO case. Then provide one sentence describing the operational consequence.
22. Defend the claim that $\mu$ notation improves reliability in repeated calculations for a medium-altitude transfer estimate. Then state one assumption that must remain true.
23. Defend the claim that inverse-square scaling changes acceleration faster than many students expect for a high-altitude communication-orbit scenario. Then show one plausibility check you would run.
24. Defend the claim that a vector sketch can prevent algebraic sign errors for a negative-y coordinate test point. Then identify one common mistake and how to avoid it.
25. Defend the claim that dimensional analysis should be performed before accepting a numeric result for a mixed-sign 3D state vector. Then connect your explanation to what Lecture 02 will need.
26. Defend the claim that two-body assumptions are useful without being universally valid for a quick mission pre-design briefing. Then provide one sentence describing the operational consequence.
27. Defend the claim that radius must be center-to-center instead of altitude-alone in formulas for a student who forgot unit conversion. Then state one assumption that must remain true.
28. Defend the claim that a result can be numerically neat yet physically implausible for a student who dropped the vector sign. Then show one plausibility check you would run.
29. Defend the claim that scalar and vector equations should be written separately during derivation for an answer that seems too large. Then identify one common mistake and how to avoid it.
30. Defend the claim that model limitations should be reported alongside computed values for a hand-calculation cross-check. Then connect your explanation to what Lecture 02 will need.
31. Diagnose what happens when the minus sign in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ is physically required for a near-circular LEO case. Then provide one sentence describing the operational consequence.
32. Diagnose what happens when $\mu$ notation improves reliability in repeated calculations for a medium-altitude transfer estimate. Then state one assumption that must remain true.
33. Diagnose what happens when inverse-square scaling changes acceleration faster than many students expect for a high-altitude communication-orbit scenario. Then show one plausibility check you would run.
34. Diagnose what happens when a vector sketch can prevent algebraic sign errors for a negative-y coordinate test point. Then identify one common mistake and how to avoid it.
35. Diagnose what happens when dimensional analysis should be performed before accepting a numeric result for a mixed-sign 3D state vector. Then connect your explanation to what Lecture 02 will need.
36. Diagnose what happens when two-body assumptions are useful without being universally valid for a quick mission pre-design briefing. Then provide one sentence describing the operational consequence.
37. Diagnose what happens when radius must be center-to-center instead of altitude-alone in formulas for a student who forgot unit conversion. Then state one assumption that must remain true.
38. Diagnose what happens when a result can be numerically neat yet physically implausible for a student who dropped the vector sign. Then show one plausibility check you would run.
39. Diagnose what happens when scalar and vector equations should be written separately during derivation for an answer that seems too large. Then identify one common mistake and how to avoid it.
40. Diagnose what happens when model limitations should be reported alongside computed values for a hand-calculation cross-check. Then connect your explanation to what Lecture 02 will need.
41. Compare and contrast how the minus sign in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ is physically required for a near-circular LEO case. Then provide one sentence describing the operational consequence.
42. Compare and contrast how $\mu$ notation improves reliability in repeated calculations for a medium-altitude transfer estimate. Then state one assumption that must remain true.
43. Compare and contrast how inverse-square scaling changes acceleration faster than many students expect for a high-altitude communication-orbit scenario. Then show one plausibility check you would run.
44. Compare and contrast how a vector sketch can prevent algebraic sign errors for a negative-y coordinate test point. Then identify one common mistake and how to avoid it.
45. Compare and contrast how dimensional analysis should be performed before accepting a numeric result for a mixed-sign 3D state vector. Then connect your explanation to what Lecture 02 will need.
46. Compare and contrast how two-body assumptions are useful without being universally valid for a quick mission pre-design briefing. Then provide one sentence describing the operational consequence.
47. Compare and contrast how radius must be center-to-center instead of altitude-alone in formulas for a student who forgot unit conversion. Then state one assumption that must remain true.
48. Compare and contrast how a result can be numerically neat yet physically implausible for a student who dropped the vector sign. Then show one plausibility check you would run.
49. Compare and contrast how scalar and vector equations should be written separately during derivation for an answer that seems too large. Then identify one common mistake and how to avoid it.
50. Compare and contrast how model limitations should be reported alongside computed values for a hand-calculation cross-check. Then connect your explanation to what Lecture 02 will need.
51. Predict the outcome if the minus sign in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ is physically required for a near-circular LEO case. Then provide one sentence describing the operational consequence.
52. Predict the outcome if $\mu$ notation improves reliability in repeated calculations for a medium-altitude transfer estimate. Then state one assumption that must remain true.
53. Predict the outcome if inverse-square scaling changes acceleration faster than many students expect for a high-altitude communication-orbit scenario. Then show one plausibility check you would run.
54. Predict the outcome if a vector sketch can prevent algebraic sign errors for a negative-y coordinate test point. Then identify one common mistake and how to avoid it.
55. Predict the outcome if dimensional analysis should be performed before accepting a numeric result for a mixed-sign 3D state vector. Then connect your explanation to what Lecture 02 will need.
56. Predict the outcome if two-body assumptions are useful without being universally valid for a quick mission pre-design briefing. Then provide one sentence describing the operational consequence.
57. Predict the outcome if radius must be center-to-center instead of altitude-alone in formulas for a student who forgot unit conversion. Then state one assumption that must remain true.
58. Predict the outcome if a result can be numerically neat yet physically implausible for a student who dropped the vector sign. Then show one plausibility check you would run.
59. Predict the outcome if scalar and vector equations should be written separately during derivation for an answer that seems too large. Then identify one common mistake and how to avoid it.
60. Predict the outcome if model limitations should be reported alongside computed values for a hand-calculation cross-check. Then connect your explanation to what Lecture 02 will need.
61. Justify why the minus sign in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ is physically required for a near-circular LEO case. Then provide one sentence describing the operational consequence.
62. Justify why $\mu$ notation improves reliability in repeated calculations for a medium-altitude transfer estimate. Then state one assumption that must remain true.
63. Justify why inverse-square scaling changes acceleration faster than many students expect for a high-altitude communication-orbit scenario. Then show one plausibility check you would run.
64. Justify why a vector sketch can prevent algebraic sign errors for a negative-y coordinate test point. Then identify one common mistake and how to avoid it.
65. Justify why dimensional analysis should be performed before accepting a numeric result for a mixed-sign 3D state vector. Then connect your explanation to what Lecture 02 will need.
66. Justify why two-body assumptions are useful without being universally valid for a quick mission pre-design briefing. Then provide one sentence describing the operational consequence.
67. Justify why radius must be center-to-center instead of altitude-alone in formulas for a student who forgot unit conversion. Then state one assumption that must remain true.
68. Justify why a result can be numerically neat yet physically implausible for a student who dropped the vector sign. Then show one plausibility check you would run.
69. Justify why scalar and vector equations should be written separately during derivation for an answer that seems too large. Then identify one common mistake and how to avoid it.
70. Justify why model limitations should be reported alongside computed values for a hand-calculation cross-check. Then connect your explanation to what Lecture 02 will need.
71. Translate into plain language why the minus sign in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ is physically required for a near-circular LEO case. Then provide one sentence describing the operational consequence.
72. Translate into plain language why $\mu$ notation improves reliability in repeated calculations for a medium-altitude transfer estimate. Then state one assumption that must remain true.
73. Translate into plain language why inverse-square scaling changes acceleration faster than many students expect for a high-altitude communication-orbit scenario. Then show one plausibility check you would run.
74. Translate into plain language why a vector sketch can prevent algebraic sign errors for a negative-y coordinate test point. Then identify one common mistake and how to avoid it.
75. Translate into plain language why dimensional analysis should be performed before accepting a numeric result for a mixed-sign 3D state vector. Then connect your explanation to what Lecture 02 will need.
76. Translate into plain language why two-body assumptions are useful without being universally valid for a quick mission pre-design briefing. Then provide one sentence describing the operational consequence.
77. Translate into plain language why radius must be center-to-center instead of altitude-alone in formulas for a student who forgot unit conversion. Then state one assumption that must remain true.
78. Translate into plain language why a result can be numerically neat yet physically implausible for a student who dropped the vector sign. Then show one plausibility check you would run.
79. Translate into plain language why scalar and vector equations should be written separately during derivation for an answer that seems too large. Then identify one common mistake and how to avoid it.
80. Translate into plain language why model limitations should be reported alongside computed values for a hand-calculation cross-check. Then connect your explanation to what Lecture 02 will need.
81. Show step-by-step why the minus sign in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ is physically required for a near-circular LEO case. Then provide one sentence describing the operational consequence.
82. Show step-by-step why $\mu$ notation improves reliability in repeated calculations for a medium-altitude transfer estimate. Then state one assumption that must remain true.
83. Show step-by-step why inverse-square scaling changes acceleration faster than many students expect for a high-altitude communication-orbit scenario. Then show one plausibility check you would run.
84. Show step-by-step why a vector sketch can prevent algebraic sign errors for a negative-y coordinate test point. Then identify one common mistake and how to avoid it.
85. Show step-by-step why dimensional analysis should be performed before accepting a numeric result for a mixed-sign 3D state vector. Then connect your explanation to what Lecture 02 will need.
86. Show step-by-step why two-body assumptions are useful without being universally valid for a quick mission pre-design briefing. Then provide one sentence describing the operational consequence.
87. Show step-by-step why radius must be center-to-center instead of altitude-alone in formulas for a student who forgot unit conversion. Then state one assumption that must remain true.
88. Show step-by-step why a result can be numerically neat yet physically implausible for a student who dropped the vector sign. Then show one plausibility check you would run.
89. Show step-by-step why scalar and vector equations should be written separately during derivation for an answer that seems too large. Then identify one common mistake and how to avoid it.
90. Show step-by-step why model limitations should be reported alongside computed values for a hand-calculation cross-check. Then connect your explanation to what Lecture 02 will need.
91. Argue from first principles that the minus sign in $\mathbf{a}=-\mu\mathbf{r}/\|\mathbf{r}\|^3$ is physically required for a near-circular LEO case. Then provide one sentence describing the operational consequence.
92. Argue from first principles that $\mu$ notation improves reliability in repeated calculations for a medium-altitude transfer estimate. Then state one assumption that must remain true.
93. Argue from first principles that inverse-square scaling changes acceleration faster than many students expect for a high-altitude communication-orbit scenario. Then show one plausibility check you would run.
94. Argue from first principles that a vector sketch can prevent algebraic sign errors for a negative-y coordinate test point. Then identify one common mistake and how to avoid it.
95. Argue from first principles that dimensional analysis should be performed before accepting a numeric result for a mixed-sign 3D state vector. Then connect your explanation to what Lecture 02 will need.
96. Argue from first principles that two-body assumptions are useful without being universally valid for a quick mission pre-design briefing. Then provide one sentence describing the operational consequence.
97. Argue from first principles that radius must be center-to-center instead of altitude-alone in formulas for a student who forgot unit conversion. Then state one assumption that must remain true.
98. Argue from first principles that a result can be numerically neat yet physically implausible for a student who dropped the vector sign. Then show one plausibility check you would run.
99. Argue from first principles that scalar and vector equations should be written separately during derivation for an answer that seems too large. Then identify one common mistake and how to avoid it.
100. Argue from first principles that model limitations should be reported alongside computed values for a hand-calculation cross-check. Then connect your explanation to what Lecture 02 will need.
