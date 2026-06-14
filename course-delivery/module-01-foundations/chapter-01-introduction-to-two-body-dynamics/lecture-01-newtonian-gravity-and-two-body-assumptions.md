# Lecture 01 — Newtonian Gravity and the Two-Body Assumptions

**Module 01: Foundations**
**Chapter 01: Introduction to Two-Body Dynamics**
**Version 1.0 | Date: 2026-06-14**

---

## Learning Objectives

By the end of this lecture, you will be able to:

1. State Newton's Law of Universal Gravitation and identify every symbol and its physical meaning.
2. Explain the four assumptions that reduce the real problem of orbital mechanics to the idealized two-body problem.
3. Derive the gravitational acceleration vector acting on a satellite from first principles.
4. Solve numerical problems involving gravitational force magnitude and direction.
5. Write a clean, type-annotated Python class that models a two-body gravitational system and integrates the equations of motion numerically.

**Primary Competency Domain**: C2 — Two-Body Dynamics
**Supporting Domain**: C1 — Mathematical Foundations

---

## Prerequisites

Before proceeding, confirm you can answer the following without looking them up:

- What is the difference between a **scalar** and a **vector**?
- What does it mean for a quantity to obey an **inverse-square law**?
- What is a **differential equation**? What does it mean to *integrate* one numerically?

If any of these feel uncertain, revisit your calculus and linear algebra foundations before continuing. This lecture assumes fluency with vector notation, Newton's second law, and basic differential calculus.

---

## Common Misconception Box

> **Misconception**: "Gravity disappears in orbit because astronauts are weightless."
>
> **Correction**: Gravity does not disappear in orbit. At the International Space Station's altitude of approximately 400 km, Earth's gravitational acceleration is still about $8.7\ \text{m/s}^2$ — roughly 88% of its sea-level value. The apparent weightlessness arises because the spacecraft and everything inside it are in free fall together: they all accelerate at the same rate toward Earth. The ISS is not escaping gravity; it is perpetually falling *around* Earth, precisely fast enough that the curved surface keeps receding beneath it. Orbital mechanics is entirely a story told by gravity.

---

## Section 1 — System Architecture: The Mental Model

Before we write a single equation, we build a mental model. This is the most important step in any physical derivation: understand the geometry and the causal chain before you abstract it.

### 1.1 The Two-Body System

Imagine two point masses — call them $m_1$ and $m_2$ — separated by a distance $r$ in otherwise empty space. Nothing else exists in this universe. The only interaction between them is gravity.

Each mass exerts a gravitational attraction on the other. By Newton's third law, these forces are equal in magnitude and opposite in direction. The two bodies therefore pull each other toward their common **center of mass**, also called the **barycenter**.

In the full two-body problem, *both* objects move in response to each other's gravity. The Earth moves in response to the Moon just as the Moon moves in response to Earth. The motion is symmetric; it is only our intuition (shaped by living on the larger body) that makes one seem stationary.

However, astrodynamics almost always invokes a powerful simplification: when $m_1 \gg m_2$ — when the central body is vastly more massive than the satellite — the center of mass is so close to $m_1$ that $m_1$ barely moves. We then treat $m_1$ as a **fixed origin** and study only the motion of $m_2$ relative to it. This is the **restricted two-body problem**, and it is the foundation of everything that follows.

### 1.2 Physical Scale

To appreciate why this simplification is excellent for most spacecraft, consider the mass ratio:

- Earth mass: $M_\oplus \approx 5.972 \times 10^{24}\ \text{kg}$
- International Space Station mass: $m_\text{ISS} \approx 4.5 \times 10^5\ \text{kg}$
- Mass ratio: $M_\oplus / m_\text{ISS} \approx 1.3 \times 10^{19}$

The ISS perturbs Earth's position by an amount proportional to its own mass relative to Earth's — roughly $10^{-19}$ of Earth's orbital radius. That displacement is approximately $6 \times 10^{-9}$ meters per orbital period. It is, for any engineering purpose, zero. The approximation is not a convenience; it is extraordinarily accurate.

### 1.3 The Causal Chain

Here is the system architecture stated as a causal chain:

1. **Positions define a separation vector**: $\vec{r} = \vec{r}_2 - \vec{r}_1$, with magnitude $r = \|\vec{r}\|$.
2. **The separation vector determines the gravitational force** on $m_2$ via Newton's Law of Gravitation.
3. **The force determines the acceleration** of $m_2$ via Newton's second law: $\vec{F} = m_2 \vec{a}$.
4. **The acceleration, integrated twice, gives the trajectory** $\vec{r}(t)$.

This chain — geometry → force → acceleration → trajectory — is the engine of all orbital mechanics. Every result in this textbook is a consequence of following this chain, sometimes in closed form, sometimes numerically.

---

## Section 2 — First-Principles Derivation

### 2.1 Newton's Law of Universal Gravitation

**Newton's Law of Universal Gravitation** states that every pair of point masses $m_1$ and $m_2$ attracts each other with a force whose magnitude is:

$$F = \frac{G m_1 m_2}{r^2}$$

where:
- $G = 6.674 \times 10^{-11}\ \text{N} \cdot \text{m}^2 \cdot \text{kg}^{-2}$ is the **universal gravitational constant**,
- $r$ is the **scalar distance** between the centers of the two masses,
- $F$ is the magnitude of the gravitational force, in Newtons.

This is a scalar equation. It tells us *how strong* the force is. To know *which direction* the force acts, we must write the **vector form**.

### 2.2 The Vector Form of the Gravitational Force

Place the central body $m_1$ at the origin. The satellite $m_2$ is at position $\vec{r}$ relative to the origin. The **unit vector** pointing from $m_2$ toward $m_1$ (i.e., in the direction of the force on $m_2$) is:

$$\hat{r} = -\frac{\vec{r}}{r}$$

The negative sign is critical: the force on $m_2$ points *toward* $m_1$, which is in the $-\vec{r}$ direction.

Therefore, the gravitational force vector acting on $m_2$ is:

$$\vec{F}_{1 \to 2} = -\frac{G m_1 m_2}{r^2} \hat{r} = -\frac{G m_1 m_2}{r^3} \vec{r}$$

where we used $\hat{r} = \vec{r}/r$, so $\hat{r}/r^2 = \vec{r}/r^3$.

**Annotation**: The factor $1/r^3$ looks like a third-power law, but it is not — it arises because we replaced the unit vector $\hat{r}/r^2$ with $\vec{r}/r^3$. The underlying physics is still inverse-square.

### 2.3 The Equation of Motion

Apply Newton's second law to $m_2$:

$$\vec{F}_{1 \to 2} = m_2 \ddot{\vec{r}}$$

Substituting the gravitational force:

$$m_2 \ddot{\vec{r}} = -\frac{G m_1 m_2}{r^3} \vec{r}$$

Divide both sides by $m_2$:

$$\ddot{\vec{r}} = -\frac{G m_1}{r^3} \vec{r}$$

This is the **restricted two-body equation of motion**. It is perhaps the most important equation in orbital mechanics. Notice that $m_2$ has vanished from the equation entirely — the trajectory of a satellite is independent of its own mass. A grain of sand and the ISS placed in identical initial conditions follow identical trajectories under gravity alone.

### 2.4 The Gravitational Parameter $\mu$

In practice, we never need $G$ and $M$ separately. We always need their product. We define the **gravitational parameter** (also called the **standard gravitational parameter**):

$$\mu \equiv G M$$

where $M$ is the mass of the central body. For Earth:

$$\mu_\oplus = G M_\oplus = 3.986004418 \times 10^{14}\ \text{m}^3 \cdot \text{s}^{-2} \approx 398600.4418\ \text{km}^3 \cdot \text{s}^{-2}$$

This value is known with far greater precision than either $G$ or $M_\oplus$ individually, because $\mu$ is measured directly by tracking spacecraft.

The equation of motion becomes:

$$\boxed{\ddot{\vec{r}} = -\frac{\mu}{r^3} \vec{r}}$$

This is the equation you will see on every page of this textbook, in every problem, and in every simulation. Internalize it.

### 2.5 The Four Assumptions of the Two-Body Problem

The elegance of the equation above rests on four assumptions. You must know them and be able to articulate what each one excludes:

**Assumption 1: Point Masses (or Spherically Symmetric Bodies)**
Both bodies are treated as point masses, or equivalently, as spherically symmetric bodies with uniform density shells. By Newton's shell theorem, such bodies exert gravitational forces identical to a point mass at their center. This assumption fails near highly oblate bodies (Earth is about 1/298 oblate) or at very low altitudes where the body's internal mass distribution matters.

**Assumption 2: Newtonian Gravity (Non-Relativistic Regime)**
The gravitational interaction obeys Newton's inverse-square law, not general relativity. This is an excellent approximation for most Earth-orbital and interplanetary missions. It fails near strong gravitational fields (e.g., close approaches to neutron stars) or for extremely precise measurements (GPS satellites require relativistic corrections at the level of $\sim 38\ \mu\text{s/day}$).

**Assumption 3: No External Forces**
Only the mutual gravity of the two bodies acts. There is no atmospheric drag, no solar radiation pressure, no lunar gravitational pull, no electromagnetic forces, and no thrust. In reality, all of these perturb real orbits. The two-body solution is the **unperturbed baseline** that we later correct.

**Assumption 4: Isolated System (Two Bodies Only)**
The universe contains only these two bodies. In a solar system of eight planets, dozens of moons, and a star, this is clearly not exactly true. However, for timescales shorter than roughly one orbital period, the dominant gravitational influence on a near-Earth satellite is Earth itself; all other bodies contribute corrections at the level of $10^{-5}$ or smaller. The two-body approximation is valid for initial orbit design and short-term propagation.

### 2.6 Specific Mechanical Energy

From the equation of motion, two constants of motion can be derived immediately. These are the most important conserved quantities in orbital mechanics.

The **specific mechanical energy** (energy per unit mass of the satellite) is:

$$\varepsilon = \frac{v^2}{2} - \frac{\mu}{r}$$

where $v = \|\dot{\vec{r}}\|$ is the orbital speed. This quantity is conserved along any two-body orbit. To verify conservation, compute its time derivative and confirm it equals zero along solutions to $\ddot{\vec{r}} = -\mu\vec{r}/r^3$.

**Derivation sketch**: Take the dot product of the equation of motion with $\dot{\vec{r}}$:

$$\dot{\vec{r}} \cdot \ddot{\vec{r}} = -\frac{\mu}{r^3} \dot{\vec{r}} \cdot \vec{r}$$

The left side is $\frac{d}{dt}\left(\frac{v^2}{2}\right)$. The right side simplifies using $\dot{r} = \frac{\vec{r} \cdot \dot{\vec{r}}}{r}$, giving:

$$\frac{d}{dt}\left(\frac{v^2}{2}\right) = -\frac{\mu}{r^2} \dot{r} = \frac{d}{dt}\left(\frac{\mu}{r}\right) \cdot (-1) \cdot (-1) = \frac{d}{dt}\left(\frac{\mu}{r}\right)$$

Wait — let us be careful. Since $r^{-1}$ differentiates as $-\dot{r}/r^2$:

$$\frac{d}{dt}\left(\frac{\mu}{r}\right) = -\frac{\mu \dot{r}}{r^2}$$

Therefore:

$$\frac{d}{dt}\left(\frac{v^2}{2}\right) = \frac{\mu \dot{r}}{r^2} \cdot (-1)$$

Wait, let us redo this carefully. We have:

$$\frac{d}{dt}\left(\frac{\mu}{r}\right) = \mu \cdot \frac{d}{dt}\left(r^{-1}\right) = -\frac{\mu \dot{r}}{r^2}$$

And from the equation of motion dot-producted with $\dot{\vec{r}}$:

$$\frac{d}{dt}\left(\frac{v^2}{2}\right) = -\frac{\mu}{r^3}(\vec{r} \cdot \dot{\vec{r}}) = -\frac{\mu}{r^3}(r \dot{r}) = -\frac{\mu \dot{r}}{r^2}$$

Therefore:

$$\frac{d}{dt}\left(\frac{v^2}{2}\right) = \frac{d}{dt}\left(\frac{\mu}{r}\right) \cdot (-1) \cdot (-1)$$

Hmm — let us write it cleanly:

$$\frac{d}{dt}\left(\frac{v^2}{2}\right) = -\frac{\mu \dot{r}}{r^2} = \frac{d}{dt}\left(\frac{\mu}{r}\right)$$

So:

$$\frac{d}{dt}\left(\frac{v^2}{2} - \frac{\mu}{r}\right) = 0 \implies \varepsilon = \frac{v^2}{2} - \frac{\mu}{r} = \text{constant}$$

This confirms that $\varepsilon$ is a constant of motion. Its sign determines the type of orbit:

| $\varepsilon$ | Orbit Type |
|:---:|---|
| $\varepsilon < 0$ | Ellipse (bound orbit) |
| $\varepsilon = 0$ | Parabola (escape at exactly escape velocity) |
| $\varepsilon > 0$ | Hyperbola (unbound, excess energy) |

### 2.7 Specific Angular Momentum

The **specific angular momentum vector** is:

$$\vec{h} = \vec{r} \times \dot{\vec{r}}$$

This is also conserved. To prove it, differentiate with respect to time:

$$\dot{\vec{h}} = \dot{\vec{r}} \times \dot{\vec{r}} + \vec{r} \times \ddot{\vec{r}}$$

The first term is the cross product of a vector with itself, which is always zero. The second term substitutes the equation of motion:

$$\vec{r} \times \ddot{\vec{r}} = \vec{r} \times \left(-\frac{\mu}{r^3}\vec{r}\right) = -\frac{\mu}{r^3}(\vec{r} \times \vec{r}) = \vec{0}$$

Therefore $\dot{\vec{h}} = \vec{0}$, so $\vec{h}$ is constant.

The magnitude $h = \|\vec{h}\|$ characterizes the orbit's size and shape (along with $\varepsilon$). The direction of $\vec{h}$ is perpendicular to the **orbital plane**, and because it is constant, the orbital plane is fixed in inertial space (under the two-body assumption).

---

## Section 3 — Scaffolded Worked Examples

### Worked Example 3.1 — Gravitational Force Between Earth and a Satellite

**Problem**: A satellite of mass $1200\ \text{kg}$ orbits Earth at an altitude of $500\ \text{km}$ above the surface. Compute:
(a) The gravitational force magnitude acting on the satellite.
(b) The gravitational acceleration experienced by the satellite.
(c) The percentage reduction in gravitational acceleration compared to sea level ($g_0 = 9.807\ \text{m/s}^2$).

**Given**:
- Earth radius: $R_\oplus = 6371\ \text{km} = 6.371 \times 10^6\ \text{m}$
- Altitude: $h = 500\ \text{km} = 5.00 \times 10^5\ \text{m}$
- Satellite mass: $m = 1200\ \text{kg}$
- $\mu_\oplus = 3.986004418 \times 10^{14}\ \text{m}^3 \cdot \text{s}^{-2}$
- $M_\oplus = 5.972 \times 10^{24}\ \text{kg}$, $G = 6.674 \times 10^{-11}\ \text{N m}^2 \text{kg}^{-2}$

**Step 1 — Compute the orbital radius.**

The orbital radius is measured from Earth's center:

$$r = R_\oplus + h = 6371 + 500 = 6871\ \text{km} = 6.871 \times 10^6\ \text{m}$$

**Step 2 — Compute the gravitational force magnitude.**

Using $F = G m_1 m_2 / r^2$, or equivalently $F = m \mu / r^2$:

$$F = \frac{m \mu_\oplus}{r^2} = \frac{1200 \times 3.986004418 \times 10^{14}}{(6.871 \times 10^6)^2}$$

Numerator: $1200 \times 3.986 \times 10^{14} = 4.783 \times 10^{17}\ \text{N m}^2$

Denominator: $(6.871)^2 \times 10^{12} = 47.21 \times 10^{12} = 4.721 \times 10^{13}\ \text{m}^2$

$$F = \frac{4.783 \times 10^{17}}{4.721 \times 10^{13}} = 1.013 \times 10^4\ \text{N} \approx 10{,}130\ \text{N}$$

**Step 3 — Compute the gravitational acceleration.**

$$g(r) = \frac{\mu_\oplus}{r^2} = \frac{F}{m} = \frac{10{,}130}{1200} \approx 8.441\ \text{m/s}^2$$

Alternatively, directly:

$$g(r) = \frac{3.986004418 \times 10^{14}}{(6.871 \times 10^6)^2} = \frac{3.986 \times 10^{14}}{4.721 \times 10^{13}} \approx 8.444\ \text{m/s}^2$$

**Step 4 — Percentage reduction from sea level.**

$$\text{Reduction} = \frac{g_0 - g(r)}{g_0} \times 100\% = \frac{9.807 - 8.444}{9.807} \times 100\% \approx 13.9\%$$

**Interpretation**: At 500 km altitude, the gravitational acceleration is 86.1% of its sea-level value. The common assertion that there is "no gravity in space" is physically wrong. The astronaut appears weightless not because gravity is absent, but because they and their spacecraft are in free fall together.

---

### Worked Example 3.2 — Escape Velocity from Earth's Surface

**Problem**: Derive the **escape velocity** — the minimum launch speed required to escape Earth's gravity from the surface, ignoring atmosphere and rotation.

**Approach**: We use conservation of specific mechanical energy. At the surface, the satellite has speed $v_e$ and radius $r = R_\oplus$. "Escape" means the satellite just barely reaches $r \to \infty$ with $v \to 0$.

**Step 1 — Write the energy conservation equation.**

$$\varepsilon_\text{initial} = \varepsilon_\text{final}$$

$$\frac{v_e^2}{2} - \frac{\mu}{R_\oplus} = \frac{0^2}{2} - \frac{\mu}{\infty} = 0$$

**Step 2 — Solve for $v_e$.**

$$\frac{v_e^2}{2} = \frac{\mu}{R_\oplus}$$

$$v_e = \sqrt{\frac{2\mu}{R_\oplus}}$$

**Step 3 — Substitute numerical values.**

$$v_e = \sqrt{\frac{2 \times 3.986004418 \times 10^{14}}{6.371 \times 10^6}} = \sqrt{\frac{7.972 \times 10^{14}}{6.371 \times 10^6}} = \sqrt{1.2512 \times 10^8}$$

$$v_e = 1.119 \times 10^4\ \text{m/s} = 11.19\ \text{km/s}$$

**Interpretation**: Any object launched from Earth's surface at speeds exceeding $11.19\ \text{km/s}$ (radially outward, ignoring atmosphere) will escape Earth's gravitational influence entirely. The specific mechanical energy at escape is exactly zero — neither bound nor unbound. This is the parabolic trajectory condition.

**Key insight**: Escape velocity depends only on $\mu$ and the initial radius. It is completely independent of the mass of the escaping object. A grain of sand and a space shuttle require the same escape speed from the same altitude.

---

### Worked Example 3.3 — Identifying the Orbit Type from Initial Conditions

**Problem**: A spacecraft is at position $\vec{r}_0 = [7000, 0, 0]^T\ \text{km}$ and velocity $\vec{v}_0 = [0, 7.5, 0]^T\ \text{km/s}$ relative to Earth's center in an inertial frame. Determine:
(a) The specific mechanical energy $\varepsilon$.
(b) The orbit type (ellipse, parabola, or hyperbola).
(c) The specific angular momentum magnitude $h$.

**Given**:
- $\mu_\oplus = 398600.4418\ \text{km}^3/\text{s}^2$
- $\vec{r}_0 = [7000, 0, 0]^T\ \text{km}$, so $r_0 = 7000\ \text{km}$
- $\vec{v}_0 = [0, 7.5, 0]^T\ \text{km/s}$, so $v_0 = 7.5\ \text{km/s}$

**Step 1 — Compute specific mechanical energy.**

$$\varepsilon = \frac{v_0^2}{2} - \frac{\mu}{r_0} = \frac{(7.5)^2}{2} - \frac{398600.4418}{7000}$$

$$= \frac{56.25}{2} - 56.943$$

$$= 28.125 - 56.943$$

$$= -28.818\ \text{km}^2/\text{s}^2$$

**Step 2 — Identify orbit type.**

Since $\varepsilon < 0$, the orbit is an **ellipse** (bound orbit). The spacecraft will not escape Earth.

**Step 3 — Compute specific angular momentum.**

$$\vec{h} = \vec{r}_0 \times \vec{v}_0 = \begin{vmatrix} \hat{x} & \hat{y} & \hat{z} \\ 7000 & 0 & 0 \\ 0 & 7.5 & 0 \end{vmatrix}$$

$$= \hat{x}(0 \cdot 0 - 0 \cdot 7.5) - \hat{y}(7000 \cdot 0 - 0 \cdot 0) + \hat{z}(7000 \cdot 7.5 - 0 \cdot 0)$$

$$= \hat{x}(0) - \hat{y}(0) + \hat{z}(52500)$$

$$\vec{h} = [0, 0, 52500]^T\ \text{km}^2/\text{s}$$

$$h = 52500\ \text{km}^2/\text{s}$$

**Interpretation**: The angular momentum vector points in the $+\hat{z}$ direction, which means the orbit lies entirely in the $xy$-plane (the equatorial plane in this example) and the spacecraft moves counter-clockwise when viewed from the $+\hat{z}$ direction (prograde motion). The conserved values $\varepsilon$ and $h$ completely characterize the orbit's geometry; we will use them to extract the semi-major axis and eccentricity in the next lecture.

---

## Section 4 — Computational Model

This section develops a clean, type-annotated Python implementation of the restricted two-body problem. The design follows standard object-oriented principles: each physical concept maps to a class or method, state is encapsulated, and computation is separated from input/output.

### 4.1 Architecture Overview

The implementation consists of three components:

1. **`CentralBody`** — Encapsulates the gravitational parameter and physical properties of the central body.
2. **`OrbitalState`** — Encapsulates the position and velocity vectors at a given epoch.
3. **`TwoBodyPropagator`** — Accepts a `CentralBody` and an `OrbitalState`, and numerically integrates the equation of motion forward in time.

### 4.2 Dependencies

The implementation uses only the Python standard library plus two widely used scientific packages:

- `numpy` — array and linear algebra operations
- `scipy` — numerical integration of differential equations

```
numpy>=1.24
scipy>=1.10
```

### 4.3 Complete Implementation

```python
"""
two_body.py

Restricted two-body orbital mechanics simulation.

Physical model: ddot_r = -mu / r^3 * r_vec
where r_vec is the position vector from the central body to the satellite,
r = ||r_vec|| is its magnitude, and mu is the gravitational parameter.

Units: SI throughout (meters, seconds, kilograms).
"""

from __future__ import annotations

from dataclasses import dataclass, field
from typing import Final

import numpy as np
import numpy.typing as npt
from scipy.integrate import solve_ivp
from scipy.integrate._ivp.ivp import OdeResult


# ---------------------------------------------------------------------------
# Physical constants
# ---------------------------------------------------------------------------

G_SI: Final[float] = 6.674e-11  # m^3 kg^-1 s^-2, universal gravitational constant


# ---------------------------------------------------------------------------
# Data classes
# ---------------------------------------------------------------------------

@dataclass(frozen=True)
class CentralBody:
    """
    Immutable representation of a spherically symmetric central body.

    Parameters
    ----------
    name : str
        Human-readable identifier (e.g., "Earth").
    mu : float
        Standard gravitational parameter G*M, in m^3 s^-2.
    radius : float
        Mean equatorial radius, in meters.  Used only for physical reference
        (e.g., checking whether an orbit intersects the surface); it does not
        enter the equation of motion.
    """

    name: str
    mu: float       # m^3 s^-2
    radius: float   # m

    def gravitational_acceleration(
        self, r_vec: npt.NDArray[np.float64]
    ) -> npt.NDArray[np.float64]:
        """
        Compute the gravitational acceleration vector at position r_vec.

        This implements: a_vec = -mu / r^3 * r_vec

        Parameters
        ----------
        r_vec : ndarray, shape (3,)
            Position vector from the central body center to the satellite, in meters.

        Returns
        -------
        ndarray, shape (3,)
            Acceleration vector in m s^-2.
        """
        r: float = float(np.linalg.norm(r_vec))
        if r == 0.0:
            raise ValueError(
                "Position vector has zero magnitude: satellite is at the center of "
                "the central body. This is a physical singularity."
            )
        return -(self.mu / r**3) * r_vec

    def surface_gravity(self) -> float:
        """
        Compute the gravitational acceleration at the body's surface.

        Returns
        -------
        float
            Surface gravitational acceleration, in m s^-2.
        """
        return self.mu / self.radius**2

    def escape_speed(self, r: float) -> float:
        """
        Compute the escape speed from radius r.

        Parameters
        ----------
        r : float
            Radial distance from body center, in meters.

        Returns
        -------
        float
            Escape speed at radius r, in m s^-1.
        """
        return float(np.sqrt(2.0 * self.mu / r))


@dataclass
class OrbitalState:
    """
    State vector of an orbiting body at a specific epoch.

    Parameters
    ----------
    position : ndarray, shape (3,)
        Position vector in an inertial frame, in meters.
    velocity : ndarray, shape (3,)
        Velocity vector in an inertial frame, in m s^-1.
    epoch : float
        Time of the state, in seconds past an arbitrary reference epoch.
    """

    position: npt.NDArray[np.float64]
    velocity: npt.NDArray[np.float64]
    epoch: float = 0.0

    def __post_init__(self) -> None:
        self.position = np.asarray(self.position, dtype=np.float64)
        self.velocity = np.asarray(self.velocity, dtype=np.float64)
        if self.position.shape != (3,):
            raise ValueError(f"position must have shape (3,), got {self.position.shape}")
        if self.velocity.shape != (3,):
            raise ValueError(f"velocity must have shape (3,), got {self.velocity.shape}")

    @property
    def radius(self) -> float:
        """Scalar distance from the central body center, in meters."""
        return float(np.linalg.norm(self.position))

    @property
    def speed(self) -> float:
        """Scalar orbital speed, in m s^-1."""
        return float(np.linalg.norm(self.velocity))

    def specific_mechanical_energy(self, mu: float) -> float:
        """
        Compute the specific mechanical energy (energy per unit satellite mass).

        epsilon = v^2/2 - mu/r

        Parameters
        ----------
        mu : float
            Gravitational parameter of the central body, in m^3 s^-2.

        Returns
        -------
        float
            Specific mechanical energy, in m^2 s^-2.
        """
        return 0.5 * self.speed**2 - mu / self.radius

    def specific_angular_momentum(self) -> npt.NDArray[np.float64]:
        """
        Compute the specific angular momentum vector h = r x v.

        Returns
        -------
        ndarray, shape (3,)
            Specific angular momentum vector, in m^2 s^-1.
        """
        return np.cross(self.position, self.velocity)

    def to_array(self) -> npt.NDArray[np.float64]:
        """
        Flatten position and velocity into a single 6-element state array.

        Returns
        -------
        ndarray, shape (6,)
            [r_x, r_y, r_z, v_x, v_y, v_z]
        """
        return np.concatenate([self.position, self.velocity])

    @classmethod
    def from_array(
        cls, state: npt.NDArray[np.float64], epoch: float = 0.0
    ) -> "OrbitalState":
        """
        Construct an OrbitalState from a flat 6-element array.

        Parameters
        ----------
        state : ndarray, shape (6,)
            [r_x, r_y, r_z, v_x, v_y, v_z]
        epoch : float
            Epoch time, in seconds.

        Returns
        -------
        OrbitalState
        """
        return cls(
            position=state[:3].copy(),
            velocity=state[3:].copy(),
            epoch=epoch,
        )


# ---------------------------------------------------------------------------
# Propagator
# ---------------------------------------------------------------------------

@dataclass
class TwoBodyPropagator:
    """
    Numerically propagates the restricted two-body equation of motion.

    Uses scipy.integrate.solve_ivp with the RK45 adaptive integrator by default.

    Parameters
    ----------
    central_body : CentralBody
        The central attracting body.
    rtol : float
        Relative tolerance for the ODE integrator (default 1e-10).
    atol : float
        Absolute tolerance for the ODE integrator (default 1e-12).
    """

    central_body: CentralBody
    rtol: float = 1e-10
    atol: float = 1e-12

    def _equations_of_motion(
        self, t: float, state: npt.NDArray[np.float64]
    ) -> npt.NDArray[np.float64]:
        """
        Right-hand side of the two-body ODE for scipy.integrate.solve_ivp.

        State vector layout: [r_x, r_y, r_z, v_x, v_y, v_z]

        Parameters
        ----------
        t : float
            Current time (unused, but required by solve_ivp signature).
        state : ndarray, shape (6,)
            Current state vector.

        Returns
        -------
        ndarray, shape (6,)
            Time derivative: [v_x, v_y, v_z, a_x, a_y, a_z]
        """
        r_vec: npt.NDArray[np.float64] = state[:3]
        v_vec: npt.NDArray[np.float64] = state[3:]
        a_vec = self.central_body.gravitational_acceleration(r_vec)
        return np.concatenate([v_vec, a_vec])

    def propagate(
        self,
        initial_state: OrbitalState,
        duration: float,
        num_points: int = 1000,
    ) -> tuple[npt.NDArray[np.float64], list[OrbitalState]]:
        """
        Integrate the two-body equations of motion forward in time.

        Parameters
        ----------
        initial_state : OrbitalState
            Initial position and velocity at epoch.
        duration : float
            Total propagation duration, in seconds.
        num_points : int
            Number of output time points (uniformly spaced).

        Returns
        -------
        times : ndarray, shape (num_points,)
            Array of output times, in seconds.
        states : list of OrbitalState
            Propagated states at each output time.

        Raises
        ------
        RuntimeError
            If the integrator fails to reach the requested time span.
        """
        t_span = (initial_state.epoch, initial_state.epoch + duration)
        t_eval = np.linspace(t_span[0], t_span[1], num_points)
        y0 = initial_state.to_array()

        result: OdeResult = solve_ivp(
            fun=self._equations_of_motion,
            t_span=t_span,
            y0=y0,
            method="RK45",
            t_eval=t_eval,
            rtol=self.rtol,
            atol=self.atol,
            dense_output=False,
        )

        if not result.success:
            raise RuntimeError(
                f"ODE integration failed: {result.message}"
            )

        times: npt.NDArray[np.float64] = result.t
        states = [
            OrbitalState.from_array(result.y[:, i], epoch=result.t[i])
            for i in range(result.y.shape[1])
        ]
        return times, states


# ---------------------------------------------------------------------------
# Pre-configured central bodies
# ---------------------------------------------------------------------------

EARTH = CentralBody(
    name="Earth",
    mu=3.986004418e14,    # m^3 s^-2
    radius=6.371e6,       # m
)

MOON = CentralBody(
    name="Moon",
    mu=4.9048695e12,      # m^3 s^-2
    radius=1.7374e6,      # m
)

SUN = CentralBody(
    name="Sun",
    mu=1.32712440018e20,  # m^3 s^-2
    radius=6.957e8,       # m
)
```

### 4.4 Design Notes

**Why `frozen=True` on `CentralBody`?** A central body's physical properties never change during a simulation. Making the dataclass frozen (immutable) prevents accidental mutation and communicates intent clearly.

**Why are tolerances set to $10^{-10}$ and $10^{-12}$?** The default `solve_ivp` tolerances ($10^{-3}$, $10^{-6}$) are adequate for rough engineering estimates but will cause orbit energy to drift noticeably over multiple periods. High-fidelity astrodynamics simulations require tighter tolerances to conserve the energy constant $\varepsilon$.

**Energy conservation as a verification test**: Because $\varepsilon$ is theoretically constant, you can monitor its value throughout a numerical propagation and use its drift as a direct measure of numerical error. This is demonstrated in the Application Task below.

---

## Section 5 — Tangible Application Task

### Task 5.1 — Low Earth Orbit Propagation and Energy Audit

**Objective**: Propagate a satellite in a circular Low Earth Orbit (LEO) for exactly one orbital period and verify conservation of specific mechanical energy to within a relative tolerance of $10^{-8}$.

**Setup**:
A satellite is placed in a circular orbit at altitude $h = 400\ \text{km}$ above Earth's surface.

**Part A — Analytical Preparation (do this by hand before writing code)**

1. Compute the orbital radius $r$ in meters.
2. Compute the circular orbital speed $v_c$ at this radius. Recall that for a circular orbit, gravitational acceleration equals centripetal acceleration:

$$\frac{\mu}{r^2} = \frac{v_c^2}{r} \implies v_c = \sqrt{\frac{\mu}{r}}$$

3. Compute the orbital period $T$ using Kepler's third law (which you have not yet derived — accept it here as a fact to be derived in Lecture 02):

$$T = 2\pi \sqrt{\frac{r^3}{\mu}}$$

4. Compute the initial specific mechanical energy $\varepsilon_0$.

**Part B — Computational Implementation**

Using the `two_body.py` module above:

1. Construct an `OrbitalState` with the satellite at $\vec{r}_0 = [r, 0, 0]^T\ \text{m}$ and $\vec{v}_0 = [0, v_c, 0]^T\ \text{m/s}$, with `epoch=0.0`.
2. Construct a `TwoBodyPropagator` with `central_body=EARTH`.
3. Propagate for exactly one orbital period $T$.
4. At every output time step, compute $\varepsilon(t) = v(t)^2/2 - \mu/r(t)$.
5. Compute the maximum relative drift:

$$\delta\varepsilon = \max_t \left|\frac{\varepsilon(t) - \varepsilon_0}{\varepsilon_0}\right|$$

6. Report whether $\delta\varepsilon < 10^{-8}$. If it is not, investigate whether tighter integrator tolerances resolve the issue.

**Part C — Physical Interpretation**

Answer the following in prose (two to four sentences each):

1. After propagation, does the satellite return to its exact initial position and velocity? If not, what might explain any discrepancy?
2. Your propagation uses 1000 output points but the integrator takes far more internal steps. What is the difference between output points and internal integration steps, and why does it matter for accuracy?
3. A classmate claims that using `rtol=1e-3` (the scipy default) is "good enough" for orbit propagation. Compute the energy drift with this loose tolerance and compare it to the tight-tolerance result. What does this tell you about the appropriate choice of numerical tolerances in orbital mechanics?

---

## Section 6 — Retrieval Integration

*This section is a retrieval challenge. Before looking at any notes, attempt each part from memory. The goal is to reconstruct the reasoning, not just produce an answer.*

### Challenge 6.1 — Connecting Gravity to Circular Speed

Without consulting any equation in this lecture, derive the circular orbital speed $v_c$ at radius $r$ from the central body. You know:
- The two-body equation of motion $\ddot{\vec{r}} = -\mu \vec{r}/r^3$.
- The definition of centripetal acceleration for circular motion.

Write out your derivation step by step, then compare it to the result in Task 5.1, Part A.

### Challenge 6.2 — Orbit Type Identification Under Perturbation

A satellite is in a circular orbit at $r_0 = 8000\ \text{km}$ from Earth's center. An engine firing instantaneously adds $\Delta v = +1.5\ \text{km/s}$ in the direction of motion (**tangential** to the orbit). Without computing the full trajectory:

1. Compute the specific mechanical energy after the burn.
2. Classify the resulting orbit.
3. Explain in one sentence why the direction of the burn (tangential vs. radial) determines the orbit's shape even if the speed increase is identical.

**Hint**: You will need $v_c$ at $r_0 = 8000\ \text{km}$ and $\mu_\oplus = 398600.4418\ \text{km}^3/\text{s}^2$. Compute $v_c$ before applying $\Delta v$.

*(The concept of impulsive burns will be developed rigorously in Module 03. Here, you are using your foundational energy knowledge to reason about orbit changes before you have the full maneuver framework — a taste of the interleaved thinking this textbook develops.)*

---

## Section 7 — Supplementary Derivations and Physical Depth

This section is for readers who want to go deeper before proceeding. It covers the full two-body (non-restricted) problem and introduces the concept of relative motion, making explicit why the restricted problem is not merely an approximation of convenience but the exact solution in a specific coordinate frame.

### 7.1 The Full Two-Body Problem

In the unrestricted two-body problem, *neither* body is fixed. Let:
- $\vec{r}_1$ = position of body 1 (mass $M$) in an inertial frame
- $\vec{r}_2$ = position of body 2 (mass $m$) in an inertial frame

Newton's second law for each body:

$$M \ddot{\vec{r}}_1 = +\frac{G M m}{|\vec{r}_2 - \vec{r}_1|^3}(\vec{r}_2 - \vec{r}_1)$$

$$m \ddot{\vec{r}}_2 = -\frac{G M m}{|\vec{r}_2 - \vec{r}_1|^3}(\vec{r}_2 - \vec{r}_1)$$

The forces are equal and opposite, as required by Newton's third law.

**Step 1 — Center of mass motion.** Add the two equations:

$$M \ddot{\vec{r}}_1 + m \ddot{\vec{r}}_2 = \vec{0}$$

Define the center of mass position:

$$\vec{R}_\text{cm} = \frac{M \vec{r}_1 + m \vec{r}_2}{M + m}$$

Then:

$$(M + m)\ddot{\vec{R}}_\text{cm} = \vec{0} \implies \ddot{\vec{R}}_\text{cm} = \vec{0}$$

The center of mass moves at **constant velocity** (or remains at rest) — there are no external forces. This is conservation of linear momentum for an isolated system. We can always choose an inertial frame in which $\dot{\vec{R}}_\text{cm} = \vec{0}$, placing the origin at the barycenter.

**Step 2 — Relative motion.** Define the relative position vector:

$$\vec{r} \equiv \vec{r}_2 - \vec{r}_1$$

Subtract the equation of motion of body 1 (divided by $M$) from the equation of motion of body 2 (divided by $m$):

$$\ddot{\vec{r}}_2 - \ddot{\vec{r}}_1 = -\frac{G(M + m)}{r^3}\vec{r}$$

$$\boxed{\ddot{\vec{r}} = -\frac{G(M + m)}{r^3}\vec{r}}$$

This is the **exact** equation of motion for the relative position vector $\vec{r} = \vec{r}_2 - \vec{r}_1$. It has exactly the same form as the restricted two-body equation of motion, with the substitution:

$$\mu_\text{two-body} = G(M + m)$$

In the restricted limit where $m \ll M$, we have $G(M + m) \approx GM = \mu$, recovering the restricted form. The correction is of order $m/M$, which for a spacecraft orbiting Earth is $\sim 10^{-19}$ — completely negligible.

**Conclusion**: The restricted two-body problem is not merely an approximation. It is the exact equation of motion in the center-of-mass frame, with $\mu$ replaced by the exact $G(M+m)$. The "approximation" $G(M+m) \approx GM$ is separately applied when $m \ll M$ — but the structural form of the equation is exact.

### 7.2 Gravitational Potential Energy and the Potential Function

The gravitational force is **conservative** — it can be derived from a scalar **potential function** $U(\vec{r})$:

$$\vec{F} = -\nabla U$$

For two-body gravity, with $m_2 = m$ (satellite mass):

$$U = -\frac{G M m}{r} = -\frac{\mu m}{r}$$

The specific (per-unit-mass) gravitational potential is:

$$V = -\frac{\mu}{r}$$

so the force per unit mass (gravitational acceleration) is:

$$\vec{a} = -\nabla V = -\nabla\left(-\frac{\mu}{r}\right) = -\frac{\mu}{r^3}\vec{r}$$

(using $\nabla(1/r) = -\vec{r}/r^3$), confirming the equation of motion.

The specific mechanical energy is the sum of kinetic and potential specific energies:

$$\varepsilon = \frac{v^2}{2} + V = \frac{v^2}{2} - \frac{\mu}{r}$$

Because the gravitational force is conservative, this quantity is conserved along any solution of the equations of motion. The potential function perspective will become important when we introduce orbital perturbations (Module 02, Chapter 06), where non-gravitational forces are characterized by their non-conservative nature — they cause $\varepsilon$ to change, which is the physical mechanism by which drag lowers orbits.

### 7.3 Dimensional Analysis and the Scales of Orbital Mechanics

Good physical intuition begins with understanding the scales of a problem. The two-body equation of motion $\ddot{\vec{r}} = -\mu\vec{r}/r^3$ contains one parameter: $\mu$. By dimensional analysis:

$$[\mu] = \frac{[\text{length}]^3}{[\text{time}]^2}$$

A natural length scale is $r_0$ (the initial orbital radius) and the natural time scale is $\tau = \sqrt{r_0^3/\mu}$. This time scale appears in Kepler's third law as $T = 2\pi\tau$. In non-dimensional form (with $r' = r/r_0$ and $t' = t/\tau$):

$$\frac{d^2 \vec{r}'}{dt'^2} = -\frac{\vec{r}'}{r'^3}$$

This equation has **no free parameters**. Every two-body orbit is geometrically similar; they differ only in scale and period. This is a profound structural result: two orbits with the same shape (eccentricity) are related by simple scaling of length and time, regardless of whether they orbit Earth, Mars, or the Sun.

### 7.4 Physical Intuition: Why Does Orbital Speed Decrease with Altitude?

This question trips up many newcomers. Consider two circular orbits: one at $r_1$ (low) and one at $r_2 > r_1$ (high). The circular speeds are:

$$v_1 = \sqrt{\frac{\mu}{r_1}}, \quad v_2 = \sqrt{\frac{\mu}{r_2}}$$

Since $r_2 > r_1$, we have $v_2 < v_1$. The higher orbit is *slower*. This seems counterintuitive — shouldn't a faster launch put you in a higher orbit?

The resolution: a faster launch initially *increases* your orbit's energy (and thus raises the apoapsis), but in a circular orbit, the required speed is set by the balance between gravity and the centripetal requirement. At a larger radius, gravity is weaker, so less centripetal acceleration is needed, so a lower speed suffices.

This creates the famous **orbital mechanics inversion**: to move from a low orbit to a high orbit, you fire your engine to speed up — but your final speed in the higher orbit is lower than your initial speed in the lower orbit. You spend delta-v to achieve a slower final state. The energy you add goes into gravitational potential energy, not kinetic energy. We will quantify this precisely in Module 03.

---

## End-of-Section Exercises

### Routine Practice

**Exercise 1.1**: Compute the gravitational acceleration on the surface of the Moon. Use $\mu_\text{Moon} = 4.9049 \times 10^{12}\ \text{m}^3/\text{s}^2$ and $R_\text{Moon} = 1737.4\ \text{km}$.

**Exercise 1.2**: A spacecraft is at $r = 42{,}164\ \text{km}$ from Earth's center (geostationary altitude). Compute:
(a) The gravitational acceleration.
(b) The escape speed from this altitude.
(c) The specific mechanical energy if the spacecraft has speed $v = 3.075\ \text{km/s}$ at this radius.

**Exercise 1.3**: Verify that $\mu_\oplus = G M_\oplus$ using $G = 6.674 \times 10^{-11}\ \text{N m}^2 \text{kg}^{-2}$ and $M_\oplus = 5.972 \times 10^{24}\ \text{kg}$. Compare your result to the tabulated value $\mu_\oplus = 3.986004418 \times 10^{14}\ \text{m}^3/\text{s}^2$. Discuss the precision discrepancy.

**Exercise 1.4**: Write the gravitational force vector $\vec{F}$ on a satellite at $\vec{r} = [5000, 3000, 2000]^T\ \text{km}$ from Earth's center, where the satellite has mass $m = 800\ \text{kg}$. Give your answer in Newtons as a vector.

### Stretch Problems

**Exercise 1.5 (Proof)**: Starting from the equation of motion $\ddot{\vec{r}} = -\mu \vec{r}/r^3$, prove that the orbital plane is fixed in inertial space. Your proof must invoke the conservation of $\vec{h}$ and explain what "orbital plane is fixed" means geometrically.

**Exercise 1.6 (Design)**: You want to place a weather satellite in a circular orbit with an orbital period of exactly 90 minutes. What orbital altitude above Earth's surface is required? Verify that this altitude is above Earth's atmosphere (assume $\sim 120\ \text{km}$ as the atmosphere-space boundary).

**Exercise 1.7 (Computational)**: Modify the `TwoBodyPropagator` to add an event-detection capability that halts integration when the satellite reaches periapsis (the closest point in the orbit). Use `scipy.integrate.solve_ivp`'s `events` argument. Test it with an elliptical orbit of your choice. *Hint*: Periapsis occurs when the radial velocity $\dot{r} = \vec{r} \cdot \dot{\vec{r}} / r$ changes sign from negative to positive.*

### Self-Assessment Rubric

| Exercise | Correct Setup | Correct Computation | Units Correct | Physical Interpretation |
|----------|:---:|:---:|:---:|:---:|
| 1.1 | 3 pts | 3 pts | 2 pts | 2 pts |
| 1.2 | 3 pts | 6 pts | 2 pts | 4 pts |
| 1.3 | 2 pts | 3 pts | 1 pts | 4 pts |
| 1.4 | 3 pts | 4 pts | 3 pts | — |
| 1.5 | 5 pts | 5 pts | — | 5 pts |
| 1.6 | 4 pts | 4 pts | 2 pts | 5 pts |
| 1.7 | 5 pts | 5 pts | — | 5 pts |

A score of 80% or above on this set indicates readiness to proceed to Lecture 02. If you score below 80% on any individual exercise, review the corresponding derivation in Section 2 before continuing.

---

## Answer Key (Selected)

**Exercise 1.1**:
$$g_\text{Moon} = \frac{\mu_\text{Moon}}{R_\text{Moon}^2} = \frac{4.9049 \times 10^{12}}{(1.7374 \times 10^6)^2} = \frac{4.9049 \times 10^{12}}{3.0186 \times 10^{12}} \approx 1.625\ \text{m/s}^2$$

This is approximately $1/6$ of Earth's surface gravity, consistent with the well-known fact that the Moon's gravity is about 16.5% of Earth's.

**Exercise 1.2 (a)**:
$$g = \frac{\mu}{r^2} = \frac{3.986 \times 10^{14}}{(4.2164 \times 10^7)^2} = \frac{3.986 \times 10^{14}}{1.7778 \times 10^{15}} \approx 0.2242\ \text{m/s}^2$$

This is about 2.3% of surface gravity — a dramatic reduction at geostationary altitude.

**Exercise 1.2 (c)**:
$$\varepsilon = \frac{(3.075)^2}{2} - \frac{398600.4418}{42164} = \frac{9.456}{2} - 9.452 = 4.728 - 9.452 = -4.724\ \text{km}^2/\text{s}^2$$

The negative energy confirms a bound orbit. The small magnitude (compared to LEO's $\sim -29\ \text{km}^2/\text{s}^2$) reflects that GEO is much closer to the escape condition — a satellite at GEO needs relatively little additional velocity to escape Earth entirely.

---

## Further Exploration

- **Feynman's Lost Lecture** (David Goodstein and Judith Goodstein, 1996): A geometric derivation of elliptical orbits from first principles, following Newton's original approach in the Principia. Recommended for readers who want to understand orbital mechanics as Newton discovered it, before the machinery of calculus was fully developed.
- **Battin, R. H., *An Introduction to the Mathematics and Methods of Astrodynamics*, AIAA, 1999** (Chapter 1–2): The definitive reference treatment of the two-body problem. Rigorous, complete, and demanding.
- **Prussing, J. E., and Conway, B. A., *Orbital Mechanics*, Oxford University Press**: An accessible and mathematically clean treatment aimed at aerospace engineering students. Excellent complement to this textbook.
- **NIST CODATA values for fundamental constants** (https://physics.nist.gov/cuu/Constants/): The authoritative source for $G$, $M_\oplus$, and $\mu_\oplus$ values used throughout this text.

---

## Vocabulary Reference

| Term | Symbol | Definition |
|------|--------|------------|
| **Gravitational parameter** | $\mu$ | Product $GM$ of the central body; determines all orbital characteristics |
| **Specific mechanical energy** | $\varepsilon$ | Energy per unit mass: $v^2/2 - \mu/r$; conserved in two-body motion |
| **Specific angular momentum** | $\vec{h}$ | $\vec{r} \times \dot{\vec{r}}$; conserved in two-body motion; defines orbital plane |
| **Escape velocity** | $v_e$ | Minimum speed to escape the gravitational field: $\sqrt{2\mu/r}$ |
| **Circular orbital speed** | $v_c$ | Speed for a circular orbit at radius $r$: $\sqrt{\mu/r}$ |
| **Barycenter** | — | The center of mass of a two-body system |
| **Epoch** | $t_0$ | A reference time at which the orbital state is specified |
| **Orbital plane** | — | The plane containing $\vec{r}$ and $\dot{\vec{r}}$; fixed in inertial space for the two-body problem |
| **Point mass approximation** | — | Assumption that extended bodies behave gravitationally as though all mass is concentrated at their center |

---

*End of Lecture 01 — Module 01, Chapter 01*
*Next: Lecture 02 — The Equation of Motion and Conservation Laws*

---

> **Version tag**: v1.0 | **Date**: 2026-06-14
> **Competency domains assessed**: C1 (Mathematical Foundations), C2 (Two-Body Dynamics)
> **Prerequisites satisfied**: Newton's second law, vector algebra, basic differential calculus
> **Estimated reading time**: 60–75 minutes for active reading with worked examples
