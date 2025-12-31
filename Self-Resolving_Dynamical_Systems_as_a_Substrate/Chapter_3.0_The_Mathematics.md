# Chapter 3.0: The Mathematics

---

## 3.1 Overview

This chapter presents the complete mathematical formalism underlying self-resolving dynamical systems. Every equation is defined, every term is explained, and every claim is either proven or demonstrated constructively.

We proceed in layers:
1. **Base Evolution**: The CSS equation
2. **Memory System**: Ω^ accumulation
3. **Shell Detection**: ρq threshold
4. **Logic Emergence**: CLA^ classification
5. **Symbol Emission**: Σ mapping
6. **Loss Functions**: L₁, spectral, combined
7. **Learning Dynamics**: Gradient-like updates

---

## 3.2 The Collapse Sentience Simulation (CSS) Equation

**Definition 3.1** (CSS Evolution). The core evolution equation is:

$$\frac{\partial \Phi}{\partial t} = \eta \nabla^2 \Phi - \lambda |\nabla \Phi|^2 + \mu \Phi(1-\Phi^2) - \nu \Phi + \alpha \cdot \delta_{\text{drift}}$$

where:

| Term | Symbol | Meaning | Effect |
|------|--------|---------|--------|
| Diffusion | η∇²Φ | Laplacian scaled by η | Smooths spatial variations |
| Steepening | -λ\|∇Φ\|² | Gradient magnitude squared | Sharpens boundaries |
| Bistable | μΦ(1-Φ²) | Cubic reaction term | Drives toward ±1 |
| Decay | -νΦ | Linear damping | Prevents runaway growth |
| Drift | α·δ_drift | Memory feedback | Self-referential dynamics |

**Definition 3.2** (Laplacian). The Laplacian in 2D:
$$\nabla^2 \Phi = \frac{\partial^2 \Phi}{\partial x^2} + \frac{\partial^2 \Phi}{\partial y^2}$$

Discrete (5-point stencil):
$$\nabla^2 \Phi_{i,j} \approx \frac{\Phi_{i+1,j} + \Phi_{i-1,j} + \Phi_{i,j+1} + \Phi_{i,j-1} - 4\Phi_{i,j}}{\Delta x^2}$$

**Definition 3.3** (Gradient Magnitude).
$$|\nabla \Phi| = \sqrt{\left(\frac{\partial \Phi}{\partial x}\right)^2 + \left(\frac{\partial \Phi}{\partial y}\right)^2}$$

Discrete (central differences):
$$|\nabla \Phi|_{i,j} \approx \sqrt{\left(\frac{\Phi_{i+1,j} - \Phi_{i-1,j}}{2\Delta x}\right)^2 + \left(\frac{\Phi_{i,j+1} - \Phi_{i,j-1}}{2\Delta y}\right)^2}$$

---

## 3.3 Stability Analysis

The CSS equation combines multiple effects that can conflict. Stability requires careful parameter selection.

**Proposition 3.1** (Diffusion Stability). For pure diffusion ∂Φ/∂t = η∇²Φ with explicit Euler, stability requires:
$$\Delta t < \frac{\Delta x^2}{4\eta}$$

*Proof*. Von Neumann stability analysis on the discrete operator gives eigenvalues λ = 1 - 4ηΔt/Δx². Stability requires |λ| ≤ 1, hence Δt ≤ Δx²/(2η) for 1D, Δx²/(4η) for 2D. ∎

**Proposition 3.2** (Bistable Stability). For ∂Φ/∂t = μΦ(1-Φ²), fixed points are Φ* ∈ {-1, 0, +1}.
- Φ* = ±1 are stable (attractors)
- Φ* = 0 is unstable (repeller)

*Proof*. Linearize around fixed points. At Φ* = 0: dΦ/dt ≈ μΦ, unstable for μ > 0. At Φ* = ±1: dΦ/dt ≈ -2μ(Φ - Φ*), stable for μ > 0. ∎

**Proposition 3.3** (Bounded CSS). With the bistable form μΦ(1-Φ²), solutions starting in [-1.5, 1.5] remain bounded.

*Proof sketch*. The term Φ(1-Φ²) is negative for |Φ| > 1, providing restoring force. Combined with diffusion and optional clamping, fields remain bounded. ∎

---

## 3.4 Memory Accumulation (Ω^)

The memory field Ω^ tracks where "interesting" dynamics have occurred.

**Definition 3.4** (Memory Update).
$$\hat{\Omega}_{n+1} = \gamma \hat{\Omega}_n + (1-\gamma) \rho_q |\Phi|$$

where:
- γ ∈ [0, 1] is the memory persistence (typically 0.9-0.99)
- ρq is the shell indicator (see §3.5)
- |Φ| is the field magnitude

**Interpretation**: Memory accumulates where shells form (ρq = 1) and where field values are strong (|Φ| large). Old memory decays by factor γ each step.

**Proposition 3.4**. If ρq ≤ 1 and |Φ| ≤ M, then Ω^ remains bounded:
$$\hat{\Omega} \leq \frac{M}{1-\gamma}$$

*Proof*. Let Ω* = M/(1-γ). If Ω^_n ≤ Ω*, then Ω^_{n+1} ≤ γΩ* + (1-γ)M = γM/(1-γ) + (1-γ)M = M(γ + (1-γ)²)/(1-γ) ≤ M/(1-γ) = Ω*. ∎

---

## 3.5 Shell Detection (ρq)

Shells are boundaries where the field changes rapidly—regions of high "tension."

**Definition 3.5** (Shell Indicator). The shell indicator is:
$$\rho_q(x,y) = \begin{cases} 1 & \text{if } |\nabla \Phi|(x,y) > \tau \\ 0 & \text{otherwise} \end{cases}$$

where τ is a threshold, typically computed from field statistics:
$$\tau = \mu_{|\nabla\Phi|} + k \cdot \sigma_{|\nabla\Phi|}$$

with k ≈ 1.0 and μ, σ being the mean and standard deviation of gradient magnitudes across the field.

**Definition 3.6** (Shell Mask). The shell mask is the array:
$$M_{\text{shell}}[i,j] = \rho_q(x_i, y_j)$$

This binary mask indicates where shells exist.

**Remark**. Shells correspond to domain boundaries in the bistable interpretation: where Φ transitions from ≈ +1 to ≈ -1 or vice versa.

---

## 3.6 Drift Feedback (δ_drift)

The drift term couples memory back into evolution:

**Definition 3.7** (Drift Feedback).
$$\delta_{\text{drift}} = \nabla^2 \hat{\Omega} - \hat{\Omega}$$

This is a "smoothed Laplacian" of memory: regions where memory is locally peaked (high Ω^ surrounded by low Ω^) generate positive drift; uniform memory regions generate zero drift.

**Interpretation**: The field "remembers" where interesting things happened and is influenced to continue exploring those regions (or avoid them, depending on the sign of α).

---

## 3.7 Collapse Logic Algebra (CLA^)

CLA^ classifies local shell configurations into discrete logic states.

**Definition 3.8** (CLA^ Classification). For each point (i, j), examine the 3×3 neighborhood of the shell mask. Classify according to:

| State | Value | Condition |
|-------|-------|-----------|
| VOID | 0 | No shells in neighborhood |
| PASS | 1 | 1-2 shells, aligned |
| AND | 2 | ≥3 shells, converging gradients |
| OR | 3 | ≥3 shells, diverging gradients |
| NOT | 4 | Enclosed by shells (on opposite sides) |
| XOR | 5 | Perpendicular shell crossing |
| LATCH | 6 | ≥7 shells (dense region) |

**Algorithm 3.1** (CLA^ State Computation):
```
function claState(i, j, shellMask):
    count = sum of shellMask in 3×3 around (i, j)
    
    if count == 0: return VOID
    if count >= 7: return LATCH
    
    (analyze gradient directions for AND/OR)
    (check enclosure for NOT)
    (check perpendicular for XOR)
    
    return PASS (default)
```

**Theorem 3.1** (CLA^ Emergence). The CLA^ states emerge from continuous field dynamics without explicit programming. They are classifications of naturally-occurring shell configurations.

*Proof*. By construction: we define CLA^ states in terms of shell mask patterns. The shell mask is defined by gradient thresholds. Gradients emerge from field evolution. Therefore CLA^ states are derived, not programmed. ∎

---

## 3.8 Loss Functions

### 3.8.1 L1 (Spatial) Loss

**Definition 3.9** (L1 Loss).
$$\mathcal{L}_1[\Phi, \Phi^*] = \sum_{i,j} |\Phi_{i,j} - \Phi^*_{i,j}|$$

**Properties**:
- L₁ = 0 iff Φ = Φ* everywhere
- L₁ is convex
- Gradient: ∂L₁/∂Φ = sign(Φ - Φ*)

### 3.8.2 Dirichlet Energy

**Definition 3.10** (Dirichlet Energy).
$$E[\Phi] = \sum_{i,j} |\nabla \Phi|^2_{i,j} \cdot \Delta x \cdot \Delta y$$

**Properties**:
- E = 0 iff Φ is constant
- E penalizes gradients (prefers smooth fields)
- The heat equation is gradient descent on E

### 3.8.3 Spectral Loss

**Definition 3.11** (2D Discrete Fourier Transform).
$$\hat{\Phi}(u,v) = \sum_{x=0}^{N-1} \sum_{y=0}^{M-1} \Phi(x,y) \cdot e^{-2\pi i (ux/N + vy/M)}$$

**Definition 3.12** (Spectral Loss).
$$\mathcal{L}_{\text{spectral}}[\Phi, \Phi^*] = ||\hat{\Phi} - \hat{\Phi}^*||_2 = \sqrt{\sum_{u,v} |\hat{\Phi}(u,v) - \hat{\Phi}^*(u,v)|^2}$$

**Properties**:
- Spectral loss weights all frequencies equally
- High-frequency targets (checkerboards) are properly weighted
- By Parseval's theorem: ||Φ - Φ*||₂ = ||F(Φ) - F(Φ*)||₂ / √(NM)

### 3.8.4 Combined Loss

**Definition 3.13** (Combined Loss).
$$\mathcal{L}_{\text{total}} = (1-\beta) \mathcal{L}_1 + \beta \mathcal{L}_{\text{spectral}} + \lambda E$$

where:
- β ∈ [0, 1] balances spatial vs spectral matching
- λ ≥ 0 controls smoothness regularization

**Guideline**:
- For smooth targets (disk, domains): β ≈ 0.1-0.3
- For high-frequency targets (checkerboard): β ≈ 0.5-0.8

---

## 3.9 Learning Dynamics

**Definition 3.14** (Learning Update).
$$\Phi_{t+\Delta t} = \Phi_t + \Delta t \cdot \left[ F[\Phi_t] - lr \cdot (\Phi_t - \Phi^*) \right]$$

where F[Φ] is the physics evolution and lr is the learning rate.

This combines:
1. Physics: the field evolves naturally
2. Learning: the field is nudged toward target

**Theorem 3.2** (Loss Decrease). Under combined dynamics with small lr and appropriate physics, the combined loss L_total is non-increasing on average.

*Proof sketch*. The learning term -lr·(Φ - Φ*) directly decreases L₁. The physics term may increase or decrease loss depending on the target. For targets compatible with the physics (e.g., domains under Allen-Cahn), both terms decrease loss. For incompatible targets (checkerboard under pure CSS), the learning term dominates for small enough physics coefficients. ∎

---

## 3.10 The Full System

Combining all components:

**Definition 3.15** (Complete Self-Resolving System).

**Evolution**:
$$\frac{\partial \Phi}{\partial t} = \eta \nabla^2 \Phi - \lambda |\nabla \Phi|^2 + \mu \Phi(1-\Phi^2) + \alpha \delta_{\text{drift}} - lr(\Phi - \Phi^*)$$

**Memory**:
$$\hat{\Omega}_{n+1} = \gamma \hat{\Omega}_n + (1-\gamma) \rho_q |\Phi|$$

**Shell Detection**:
$$\rho_q = \mathbf{1}_{|\nabla\Phi| > \tau}$$

**Drift**:
$$\delta_{\text{drift}} = \nabla^2 \hat{\Omega} - \hat{\Omega}$$

**Loss**:
$$\mathcal{L} = (1-\beta)||\Phi - \Phi^*||_1 + \beta ||\hat{\Phi} - \hat{\Phi}^*||_2 + \lambda E[\Phi]$$

**Convergence Criterion**:
$$\text{Resolved if } \mathcal{L} < \varepsilon \text{ or } \frac{d\mathcal{L}}{dt} \approx 0$$

---

## 3.11 Parameter Relationships

| Parameter | Range | Effect |
|-----------|-------|--------|
| η | 0.1-2.0 | Diffusion strength; higher = smoother |
| λ | 0-0.5 | Steepening; higher = sharper boundaries |
| μ | 0.1-1.0 | Bistability; higher = faster domain formation |
| α | 0-0.2 | Memory feedback; higher = more self-reference |
| γ | 0.9-0.99 | Memory persistence; higher = longer memory |
| lr | 0.01-0.2 | Learning rate; higher = faster but less stable |
| β | 0-1 | Spectral weight; higher = more frequency matching |
| Δt | 0.01-1.0 | Time step; constrained by stability |

---

## 3.12 Summary of Equations

| Equation | Formula | Purpose |
|----------|---------|---------|
| CSS | ∂Φ/∂t = η∇²Φ - λ\|∇Φ\|² + μΦ(1-Φ²) + α·δ | Field evolution |
| Memory | Ω^_{n+1} = γΩ^_n + (1-γ)ρq\|Φ\| | History tracking |
| Shells | ρq = 𝟙_{|\nabla Φ|>τ} | Boundary detection |
| Drift | δ = ∇²Ω^ - Ω^ | Memory feedback |
| L1 Loss | L₁ = \|\|Φ-Φ*\|\|₁ | Spatial matching |
| Spectral | L_s = \|\|F(Φ)-F(Φ*)\|\|₂ | Frequency matching |
| Energy | E = ∫\|∇Φ\|² | Smoothness |
| Combined | L = (1-β)L₁ + βL_s + λE | Total objective |
| Learning | Φ ← Φ - lr(Φ-Φ*)Δt | Goal convergence |

---

## Exercises

**3.1** Derive the stability condition for the Allen-Cahn equation ∂Φ/∂t = ε²∇²Φ + Φ - Φ³ using von Neumann analysis.

**3.2** Prove that the spectral loss is equivalent to L2 loss (up to scaling) using Parseval's theorem.

**3.3** For a 100×100 grid with Δx = 1, compute the Dirichlet energy of Φ(x,y) = sin(2πx/100)sin(2πy/100).

**3.4** Show that the drift feedback δ_drift = ∇²Ω^ - Ω^ has zero spatial mean if Ω^ has periodic boundary conditions.

**3.5** Design a loss functional that rewards 4-fold rotational symmetry.

---

*← [Chapter 2.0: Field-Based Computation](Chapter_2.0_Field_Based_Computation.md) | [Chapter 4.0: The Checkerboard Theorem](Chapter_4.0_The_Checkerboard_Theorem.md) →*
