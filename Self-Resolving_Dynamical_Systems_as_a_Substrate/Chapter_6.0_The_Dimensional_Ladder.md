# Chapter 6.0: The 3.5D Dimensional Ladder

---

## 6.1 Dimensions of Description

Physics typically operates in well-defined dimensions:
- 0D: Points, scalars, instantaneous values
- 1D: Lines, sequences, time series
- 2D: Surfaces, images, spatial fields
- 3D: Volumes, space, physical reality
- 4D: Spacetime (3 space + 1 time)

But our system introduces something that doesn't fit neatly: **parameters that govern evolution but don't exist within the evolving space**.

We call this the 3.5D structure—the "half dimension" where the Law resides.

---

## 6.2 The Dimensional Stack

Consider what we're actually computing:

**Level 0D: Scalar Values**
- Φᵢⱼ = value at a single grid point
- A real number in ℝ
- No spatial extent, no temporal extent

**Level 1D: Time Evolution**
- Φᵢⱼ(t) = value at one point over time
- A function ℝ → ℝ
- Trajectory through state space

**Level 2D: Spatial Field**
- Φ(x, y, t₀) = field at one moment
- A function ℝ² → ℝ
- Image, snapshot, configuration

**Level 3D: Field Evolution**
- Φ(x, y, t) = full spatiotemporal evolution
- A function ℝ² × ℝ → ℝ
- Movie, trajectory, history

**Level 3.5D: The Law**
- θ = (η, μ, λ, lr, β, ...) = parameters
- These determine HOW the field evolves
- They don't live in (x, y, t)—they sit *outside*

---

## 6.3 What Is the Half Dimension?

The parameters θ have a peculiar ontological status:
- They affect every point in space
- They affect every moment in time
- But they are not functions of space or time
- They are **constants that shape the dynamics**

This is like the speed of light c in physics:
- c appears in Maxwell's equations
- c affects everything
- But c is not a field, not a coordinate
- It's a *law*, not a *state*

Our θ is the same: it's the Law governing how Φ evolves.

**Definition 6.1** (The 3.5D Structure). A self-resolving system has structure:
- **3D Substrate**: Φ(x, y, t) ∈ ℝ
- **0.5D Law**: θ ∈ Θ (parameter space)

The "half" indicates that θ is:
- Higher than 3D (it governs 3D)
- Less than 4D (it's not another full coordinate)
- A different *kind* of dimension

---

## 6.4 The Law as Learnable

Here's the key innovation: **the Law is not fixed**.

In traditional physics:
- Laws are discovered, not chosen
- Parameters are measured, not optimized
- The universe has ONE set of physical constants

In our system:
- Laws are specified by us
- Parameters are optimized toward goals
- We can SEARCH for the right physics

**Definition 6.2** (Meta-Learning). Meta-learning is optimization over θ:

$$\theta^* = \arg\min_{\theta} \mathcal{L}[\Phi_\theta(T), \Phi^*]$$

where Φ_θ(T) is the final field after evolving under parameters θ.

This is learning *which physics* produces the desired outcome.

---

## 6.5 The Dimensional Ladder in Full

Let's trace how information flows through dimensions:

```
Level 3.5D: θ (parameters)
    │
    │ determines
    ▼
Level 3D: Φ(x, y, t) (field evolution)
    │
    │ at each moment
    ▼
Level 2D: Φ(·, ·, t₀) (spatial snapshot)
    │
    │ at each point
    ▼
Level 0D: Φᵢⱼ (scalar value)
```

**Upward flow** (emergence):
- Scalar values aggregate into fields
- Fields evolve into histories
- Histories are shaped by laws

**Downward flow** (constraint):
- Laws constrain histories
- Histories constrain fields
- Fields determine values

---

## 6.6 The Target as 3.5D Object

What about the target Φ*? Where does it live?

The target is specified once, outside of time:
- Φ*(x, y) doesn't change during evolution
- It's compared against Φ(x, y, t) at each t
- It's a *goal*, not a *state*

**Proposition 6.1**. The target Φ* lives in the same 0.5D space as θ.

*Argument*. Both θ and Φ* are:
- Specified before evolution
- Constant during evolution
- External to the (x, y, t) dynamics
- Normative rather than descriptive (they say what *should* happen)

The target is part of the Law: it tells the field what to become.

---

## 6.7 Loss as Coupling Between Dimensions

The loss functional L[Φ, Φ*] couples the 3D dynamics to the 3.5D Law:

```
      θ (parameters)    Φ* (target)
         \                /
          \              /
           \            /
            ▼          ▼
         ┌─────────────────┐
         │  L[Φ(t), Φ*]    │
         └─────────────────┘
                 │
                 │ gradient signal
                 ▼
              Φ(t+Δt)
```

The loss creates information flow:
1. Φ evolves under θ
2. L measures distance from Φ*
3. Gradient of L modifies Φ
4. (Optionally) Gradient of L modifies θ

This is how the 3.5D structure governs the 3D dynamics: through the loss coupling.

---

## 6.8 Comparison to Other Frameworks

**Neural Networks**:
- Weights W are like our θ
- Activations are like our Φ
- Forward pass is like one evolution step
- Backprop adjusts W; our learning adjusts Φ directly

Key difference: In NNs, W is learned; Φ is intermediate. In our system, θ is fixed; Φ is learned.

**Reinforcement Learning**:
- Policy π is like our θ
- State s is like our Φ
- Reward R is like our -L
- Learning updates π; our system updates Φ

Key difference: RL optimizes over action sequences; we optimize over continuous fields.

**Physics**:
- Physical constants are like our θ
- Physical fields are like our Φ
- No loss—physics just evolves

Key difference: Physics doesn't have goals; we add goals via Φ*.

---

## 6.9 The Emergence of Meaning

The 3.5D structure suggests how meaning might emerge:

**Syntax** (3D): The field Φ has structure—patterns, boundaries, domains. This is the "form" of the computation.

**Semantics** (3.5D): The parameters θ and target Φ* give meaning to the form. A particular configuration means "success" (low L) or "failure" (high L).

Without θ and Φ*, the field just evolves. It doesn't mean anything.

With θ and Φ*, the field evolution becomes purposeful. It's *trying* to reach Φ*.

**Proposition 6.2**. Meaning arises from the coupling between levels in the dimensional ladder.

---

## 6.10 Learning the Law

Can we go further and learn θ itself?

**Definition 6.3** (Law Learning). Given a meta-loss:

$$\mathcal{M}[\theta] = \mathcal{L}[\Phi_\theta(T), \Phi^*]$$

we can optimize:

$$\theta_{n+1} = \theta_n - \eta_\theta \nabla_\theta \mathcal{M}$$

This is gradient descent in parameter space.

**Challenge**: Computing ∇_θ M requires differentiating through the entire field evolution. This is:
- Possible via adjoint methods
- Expensive (proportional to evolution length)
- The province of "differentiable physics"

**Current Implementation**: We don't learn θ automatically. We manually tune parameters. This is a frontier for future work.

---

## 6.11 The Dimension Interpretation

| Dimension | Contains | Nature | Example |
|-----------|----------|--------|---------|
| 0D | Scalar values | Static | Φᵢⱼ = 0.73 |
| 1D | Sequences | Temporal | Φ(t) at one point |
| 2D | Fields | Spatial | Φ(x,y) at one moment |
| 3D | Evolutions | Spatiotemporal | Φ(x,y,t) history |
| 3.5D | Laws | Meta | θ, Φ*, L |

The 0.5 indicates the qualitative difference: the Law is not just "another coordinate" but a *governing principle*.

---

## 6.12 Philosophical Implications

The 3.5D structure suggests a resolution to several puzzles:

**The Regress Problem**: What determines the laws? What determines those determiners?

In our system: We specify θ and Φ*. We are the 4D beings setting the 3.5D laws. The regress terminates in us.

**The Problem of Universals**: Are mathematical objects (like differential equations) real?

In our system: θ and Φ* are as real as Φ(x,y,t). They have causal power—they shape the evolution. They're not "more abstract"; they're just in a different dimension.

**The Is-Ought Gap**: How can facts determine values?

In our system: The gap is explicit. Φ(x,y,t) is the "is" (what happens). Φ* is the "ought" (what should happen). L measures the gap. The system works to close it.

---

## 6.13 Summary

| Concept | Dimension | Role |
|---------|-----------|------|
| Scalar value | 0D | Base unit |
| Field | 2D | State |
| Evolution | 3D | History |
| Parameters θ | 3.5D | Law (dynamics) |
| Target Φ* | 3.5D | Law (teleology) |
| Loss L | 3.5D | Coupling |

The "half dimension" is where purpose lives. Without it, physics just happens. With it, physics computes.

---

## Exercises

**6.1** If we allowed θ to vary in time, θ(t), what dimension would it occupy? How would this change the system?

**6.2** Design a meta-loss M[θ] that rewards θ values producing fast convergence (low L in few steps).

**6.3** In the dimensional ladder, where does the *observer* sit? (Hint: who specifies Φ*?)

**6.4** Could the target Φ* itself be learned from examples? Sketch a system that infers Φ* from input-output pairs.

**6.5** Compare the 3.5D structure to the "hyperparameters" of neural network training. What are the analogies and disanalogies?

---

*← [Chapter 5.0: Computational Complexity as Feature](Chapter_5.0_Computational_Complexity.md) | [Chapter 7.0: Symbol Emergence (Σ)](Chapter_7.0_Symbol_Emergence.md) →*
