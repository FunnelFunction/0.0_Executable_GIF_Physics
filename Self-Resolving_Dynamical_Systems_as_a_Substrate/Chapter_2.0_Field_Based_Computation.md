# Chapter 2.0: Field-Based Computation

---

## 2.1 The Scalar Field

**Definition 2.1** (Scalar Field). A *scalar field* is a function

$$\Phi: \Omega \times [0, T] \to \mathbb{R}$$

where Ω ⊂ ℝ² is a spatial domain and [0, T] is a time interval. For each point (x, y) ∈ Ω and time t ∈ [0, T], the field assigns a real number Φ(x, y, t).

In our implementation:
- Ω is discretized to an N × M grid
- Φ is stored as a Float32Array of length N × M
- Time is discretized into steps of size Δt

**Example 2.1**. A 200×200 field has 40,000 scalar values at each time step. Over 30 time steps, the field traces a path through a 40,000-dimensional space.

---

## 2.2 The Field as Computational State

In a von Neumann architecture, computation proceeds by:
1. Fetching an instruction
2. Decoding it
3. Executing it
4. Updating memory
5. Repeating

The state is the contents of memory. The computation is the sequence of state transitions.

In field-based computation:
1. The state is the entire field configuration Φ(·, ·, t)
2. The transition rule is a PDE: ∂Φ/∂t = F[Φ]
3. Execution is temporal evolution
4. There is no "instruction"—the dynamics are autonomous

**Proposition 2.1**. The field Φ at time t encodes the complete computational state. No external memory is required.

*Proof*. The evolution equation ∂Φ/∂t = F[Φ] is Markovian: the future depends only on the present. Therefore Φ(t) contains all information needed to compute Φ(t + Δt). ∎

---

## 2.3 Evolution as Thinking

When a field evolves under a PDE, what is it "doing"?

Consider the heat equation:
$$\frac{\partial \Phi}{\partial t} = \eta \nabla^2 \Phi$$

This equation says: "The rate of change at each point equals the curvature (Laplacian) at that point, scaled by η."

Physically: heat flows from hot to cold regions.
Computationally: sharp features smooth out; the field homogenizes.

Now consider the Allen-Cahn equation:
$$\frac{\partial \Phi}{\partial t} = \eta \nabla^2 \Phi + \mu \Phi(1 - \Phi^2)$$

The new term, μΦ(1 - Φ²), introduces bistability. The field is pushed toward Φ = +1 or Φ = -1. Combined with diffusion, this creates:
- Domains of +1 and -1
- Sharp boundaries between domains
- Boundary motion by curvature (domains coarsen over time)

This is *not* random behavior. The PDE encodes rules:
- "Smooth out small-scale variations"
- "Commit to +1 or -1"
- "Boundaries cost energy, so minimize them"

The field "thinks" by following these rules continuously.

---

## 2.4 Loss Functionals as Intent

A field evolving under physics alone has no goal. It simply obeys its dynamics.

To introduce intent, we add a *loss functional*:

**Definition 2.2** (Loss Functional). A *loss functional* is a map

$$\mathcal{L}: C(\Omega) \times C(\Omega) \to \mathbb{R}_{\geq 0}$$

that assigns a non-negative real number to pairs of field configurations. We write L[Φ, Φ*] where Φ is the current field and Φ* is the target.

**Example 2.2** (L1 Loss). The L1 loss between fields:
$$\mathcal{L}_1[\Phi, \Phi^*] = \int_\Omega |\Phi(x,y) - \Phi^*(x,y)| \, dx \, dy$$

Discrete: ||Φ - Φ*||₁ = Σᵢ |Φᵢ - Φ*ᵢ|

This measures total absolute difference. L = 0 means Φ = Φ* everywhere.

**Example 2.3** (Energy Regularization). The Dirichlet energy:
$$E[\Phi] = \int_\Omega |\nabla \Phi|^2 \, dx \, dy$$

This penalizes gradients. Low energy = smooth field.

**Example 2.4** (Combined Loss).
$$\mathcal{L}[\Phi, \Phi^*] = ||\Phi - \Phi^*||_1 + \lambda E[\Phi]$$

This balances "match the target" against "be smooth."

---

## 2.5 Loss-Driven Dynamics

With a loss functional, we can modify the dynamics to reduce it:

**Definition 2.3** (Learning Update). Given loss L, define the update:
$$\Phi_{t+\Delta t} = \Phi_t - lr \cdot \frac{\partial \mathcal{L}}{\partial \Phi} \cdot \Delta t$$

where lr is a learning rate.

For L1 loss, ∂L/∂Φ = sign(Φ - Φ*), so:
$$\Phi_{t+\Delta t} = \Phi_t - lr \cdot (\Phi_t - \Phi^*) \cdot \Delta t$$

This pushes Φ toward Φ* at each point.

**Proposition 2.2**. Under the learning update with small enough lr, L[Φ(t), Φ*] is non-increasing in t.

*Proof sketch*. The update moves Φ in the direction of steepest descent of L. For small steps, this guarantees decrease (standard gradient descent argument). ∎

---

## 2.6 Combined Evolution: Physics + Learning

The key innovation is *combining* physical dynamics with learning:

$$\frac{\partial \Phi}{\partial t} = \underbrace{\eta \nabla^2 \Phi + \mu \Phi(1-\Phi^2)}_{\text{physics}} - \underbrace{lr \cdot (\Phi - \Phi^*)}_{\text{learning}}$$

The physics provides:
- Smoothing (diffusion)
- Domain formation (bistability)
- Natural structure emergence

The learning provides:
- Goal direction
- Convergence guarantee
- Target specificity

Neither alone is sufficient:
- Physics alone: converges to energy minimizer, ignoring target
- Learning alone: converges to target, but may be unphysical (noisy, unstable)

Together: converges to target *via physical dynamics*.

---

## 2.7 Convergence as Understanding

**Definition 2.4** (Resolution). A field *resolves* to target Φ* if:
$$\lim_{t \to \infty} \mathcal{L}[\Phi(t), \Phi^*] = 0$$

**Definition 2.5** (ε-Resolution). A field *ε-resolves* to Φ* in time T if:
$$\mathcal{L}[\Phi(T), \Phi^*] < \varepsilon$$

When a field resolves to a target, we say it has "understood" the target in a specific sense: it has found a configuration that minimizes the loss.

This is not semantic understanding. The field doesn't "know" what a checkerboard "means." But it has achieved the operational definition of the goal: low loss.

**Remark**. This is analogous to how a neural network "understands" its training data: by achieving low loss. We make no stronger claims.

---

## 2.8 The Computational Medium

What makes a scalar field a valid computational medium?

**Criterion 1: State Richness**
A 200×200 field has 40,000 degrees of freedom. This is comparable to a small neural network (though organized differently).

**Criterion 2: Dynamic Expressivity**
PDEs can implement diverse transformations: smoothing, sharpening, pattern formation, wave propagation, reaction-diffusion.

**Criterion 3: Loss Sensitivity**
Any differentiable functional can be defined over fields. This allows arbitrary goals.

**Criterion 4: Physical Realizability**
Field dynamics can be implemented in physical substrates: chemical reactions, optical systems, electronic circuits.

**Criterion 5: Computational Universality?**
This is an open question. We do not claim Turing completeness for arbitrary field systems. But we demonstrate sufficient expressivity for pattern resolution tasks.

---

## 2.9 What Fields Cannot (Easily) Do

Field-based computation is not a panacea. Some tasks are awkward:

**Sequential Logic**: Fields evolve in parallel. Strict sequencing ("do A, then B, then C") requires careful encoding.

**Symbolic Manipulation**: Operating on discrete symbols (parsing, compilation, theorem proving) is unnatural for continuous fields.

**Large State**: A field stores N² values. For N = 1000, this is 1 million numbers. Symbolic systems can store sparse representations more efficiently.

**Discrete Outputs**: If the goal is a discrete answer ("yes/no", "cat/dog"), the field must be decoded. This reintroduces discretization.

We don't claim fields are better for all computation—only that they offer an alternative for certain problems where symbolic methods struggle.

---

## 2.10 The Pipeline

Our full system implements this pipeline:

```
Φ₀ (initial condition)
    ↓
∂Φ/∂t = F[Φ] (physics evolution)
    ↓
L[Φ, Φ*] (loss computation)
    ↓
Φ ← Φ - lr·∇L (learning update)
    ↓
(repeat until convergence)
    ↓
Φ_final ≈ Φ* (resolution)
```

Each component is mathematically defined, computationally implemented, and empirically tested.

---

## 2.11 Summary

| Concept | Symbol | Meaning |
|---------|--------|---------|
| Field | Φ(x,y,t) | Computational state |
| Evolution | ∂Φ/∂t = F[Φ] | State transition rule |
| Target | Φ* | Goal configuration |
| Loss | L[Φ,Φ*] | Distance from goal |
| Learning | Φ ← Φ - lr·∇L | Goal-directed update |
| Resolution | L → 0 | Goal achievement |

Field-based computation replaces:
- Instructions → PDEs
- Memory → Field configuration
- Output → Converged state
- Training → Loss specification

---

## Exercises

**2.1** Compute the L1 loss between Φ(x,y) = sin(x)sin(y) and Φ*(x,y) = 0 over Ω = [0, π]².

**2.2** For the heat equation ∂Φ/∂t = ∇²Φ, show that the Dirichlet energy E[Φ] = ∫|∇Φ|² is non-increasing in time.

**2.3** Design a loss functional that rewards symmetry: L = 0 if Φ(x,y) = Φ(-x,y) everywhere.

**2.4** If we discretize Ω into N×N points and evolve for T/Δt time steps, how many floating-point operations does naive evolution require? How does this scale with N?

---

*← [Chapter 1.0: The Problem with Symbolic AI](Chapter_1.0_The_Problem_with_Symbolic_AI.md) | [Chapter 3.0: The Mathematics](Chapter_3.0_The_Mathematics.md) →*
