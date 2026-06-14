# Course Curriculum Policy (Fundamentals of Astrodynamics)

## Course Purpose
The reader will build mastery in orbital mechanics, mission analysis, and applied astrodynamics problem-solving through a rigorously authored, world-class textbook grounded in the latest educational science. The textbook is organized around **competency-based progression**: the reader advances by demonstrating measurable mastery of each competency domain, not by completing a fixed number of weeks.

## Pedagogical Framework

This curriculum strictly adheres to the following evidence-based methodologies derived from cognitive science:

### 1. Cognitive Load Management and Worked Examples
When introducing mechanics with high elemental interactivity — complex multi-variable equations, multi-step algorithms, and physically coupled systems — chapters lead with step-by-step worked examples. Extraneous cognitive load is minimized before transitioning the reader to independent problem-solving. Worked examples are never shortcuts; they are the primary vehicle for establishing correct procedural schema.

### 2. Retrieval Practice and Spaced Repetition
Active recall is embedded throughout every chapter. Rather than summarizing prior chapters, each new module opens with a **Retrieval Integration** challenge that forces the reader to reconstruct foundational concepts from memory and apply them to a new context. Correct recall is always verified against an answer key before proceeding.

### 3. Interleaved Practice
Assessment sets deliberately mix problem types drawn from multiple competency domains. The reader must first identify *which* framework applies before solving. This trains discriminative competence — the ability to recognize the right tool — not just procedural fluency within a single tool.

### 4. First-Principles and Systems Thinking
Every concept is derived from foundational physical laws. No equation is presented without derivation or citation. Complex astrodynamic systems are decomposed into tractable sub-problems, and pattern-recognition exercises are used to reinforce structural similarities across orbital regimes. Computational models are built from first principles alongside the analytical derivations.

### 5. Pure Intrinsic Structure (No Gamification)
The learning progression relies entirely on clear competency objectives, rigorous assessment standards, and the intrinsic satisfaction of achieving measurable mastery. There are no points, badges, streaks, or motivational scaffolding beyond the material itself.

## Module Structure
Every lecture module follows this exact canonical sequence:

1. **System Architecture** — A high-level mental model of the physical mechanic being introduced.
2. **First-Principles Derivation** — The rigorous mathematical and physical foundation, derived from axioms.
3. **Scaffolded Worked Example** — A step-by-step solution to a complex problem, managing cognitive load before independent practice.
4. **Computational Model** — A software architecture that programmatically simulates the physical system, using explicit typing and clean object-oriented design.
5. **Tangible Application Task** — An independent engineering problem requiring the reader to construct a complete solution.
6. **Retrieval Integration** — A brief challenge combining the new concept with a mechanic from a previous module (interleaved practice).

## Competency Domains
Reader progress is tracked across six competency domains. Every module maps to at least one primary domain:

| Domain | Description |
|--------|-------------|
| **C1: Mathematical Foundations** | Vector algebra, kinematics, calculus, coordinate frames |
| **C2: Two-Body Dynamics** | Newtonian gravity, the two-body ODE, energy, angular momentum |
| **C3: Orbital Elements and Geometry** | Conic sections, classical orbital elements, state-to-element conversion |
| **C4: Time and Anomaly** | Kepler's equation, anomaly relationships, time-of-flight |
| **C5: Maneuver Design** | Impulsive transfers, delta-v budgeting, rendezvous, plane changes |
| **C6: Mission Analysis** | Trade studies, perturbations, error-aware design, mission constraints |

## Curriculum Modules

### Module 01 — Foundations

| Chapter | Lecture | Title | Primary Domain |
|---------|---------|-------|----------------|
| 01 | 01 | Newtonian Gravity and Two-Body Assumptions | C2 |
| 01 | 02 | The Equation of Motion and Conservation Laws | C2 |
| 02 | 01 | Conic Sections and Orbital Geometry | C3 |
| 02 | 02 | Vis-Viva Equation and Orbital Energy | C2, C3 |
| 03 | 01 | Kepler's Laws from First Principles | C2, C4 |
| 03 | 02 | Kepler's Equation and Time-of-Flight | C4 |

### Module 02 — Core Orbital Mechanics

| Chapter | Lecture | Title | Primary Domain |
|---------|---------|-------|----------------|
| 04 | 01 | State Vectors and Classical Orbital Elements | C3 |
| 04 | 02 | State-to-Element and Element-to-State Algorithms | C3 |
| 05 | 01 | Reference Frames: ECI, ECEF, Perifocal | C1, C3 |
| 05 | 02 | Frame Transformations and Rotation Matrices | C1, C3 |
| 06 | 01 | Perturbation Awareness: J2 and Atmospheric Drag | C6 |

### Module 03 — Maneuvers and Mission Design

| Chapter | Lecture | Title | Primary Domain |
|---------|---------|-------|----------------|
| 07 | 01 | Impulsive Maneuvers and the Rocket Equation | C5 |
| 07 | 02 | Hohmann Transfer and Bi-Elliptic Transfer | C5 |
| 08 | 01 | Orbital Plane Changes | C5 |
| 08 | 02 | Combined Maneuvers and Delta-v Optimization | C5 |
| 09 | 01 | Phasing and Rendezvous Fundamentals | C5 |
| 09 | 02 | Mission Trade Studies and Delta-v Budgets | C5, C6 |

### Module 04 — Integration and Professional Practice

| Chapter | Lecture | Title | Primary Domain |
|---------|---------|-------|----------------|
| 10 | 01 | End-to-End Mission Concept Design | C6 |
| 10 | 02 | Modeling Limits, Uncertainty, and Engineering Ethics | C6 |

## Authoring Policy
- Every chapter balances conceptual narrative, rigorous derivation, worked examples, and applied problem-solving.
- Each module must include at least one formative self-assessment and one summative checkpoint exercise set.
- Prerequisite concepts are reactivated at the opening of each module through explicit retrieval challenges (not summaries).
- All chapters employ evidence-based techniques: spaced practice, interleaving, retrieval prompts, and worked-example fading.
- Exposition strictly follows: motivation and intuition precede formalism in every section.
- Terminology is precise: **orbital motion** (revolution around a central body) is never conflated with **spacecraft attitude** (rotation of the vehicle's own hull about its center of mass).
- All code examples use explicit type annotations, clean object-oriented design, and standard dependency management.
- Chapters are considered ready only when they satisfy all criteria in `lecture-readiness-standard.md`.
