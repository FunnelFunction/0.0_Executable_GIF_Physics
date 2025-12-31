# Appendix A: Notation & Units

---

## A.1 Mathematical Symbols

### Primary Variables

| Symbol | Type | Domain | Description |
|--------|------|--------|-------------|
| Φ | Scalar field | ℝ² × ℝ⁺ → ℝ | Primary dynamical field |
| Φ* | Scalar field | ℝ² → ℝ | Target pattern (goal) |
| Φ₀ | Scalar field | ℝ² → ℝ | Initial condition |
| Ω^ | Scalar field | ℝ² × ℝ⁺ → ℝ⁺ | Memory accumulation field |
| ρq | Binary field | ℝ² → {0, 1} | Shell indicator |
| CLA^ | Discrete field | ℤ² → {0,...,6} | Logic algebra state |
| Σ | Symbol sequence | ℕ → V* | Symbol emission stream |

### Coordinates

| Symbol | Type | Description |
|--------|------|-------------|
| x, y | ℝ | Spatial coordinates |
| i, j | ℤ | Discrete grid indices |
| t | ℝ⁺ | Time |
| n | ℕ | Discrete time step index |
| u, v | ℝ | Frequency coordinates |
| k | ℝ² | Wave vector (k_x, k_y) |

### Parameters

| Symbol | Typical Range | Description |
|--------|---------------|-------------|
| η | 0.1 – 2.0 | Diffusion coefficient |
| λ | 0.0 – 0.5 | Steepening coefficient |
| μ | 0.1 – 1.0 | Bistable reaction strength |
| ν | 0.0 – 0.1 | Linear decay rate |
| α | 0.0 – 0.2 | Memory feedback strength |
| γ | 0.9 – 0.99 | Memory persistence |
| lr | 0.01 – 0.2 | Learning rate |
| β | 0.0 – 1.0 | Spectral loss weight |
| τ | varies | Threshold (context-dependent) |
| r | -0.5 – 0.5 | Swift-Hohenberg control parameter |
| g | 0.5 – 2.0 | Swift-Hohenberg nonlinearity |

### Discretization

| Symbol | Description |
|--------|-------------|
| N, M | Grid dimensions (points) |
| Δx, Δy | Spatial step sizes |
| Δt | Time step size |
| T | Total simulation time |
| n_frames | Number of output frames |

---

## A.2 Operators

### Differential Operators

| Notation | Definition | Description |
|----------|------------|-------------|
| ∂Φ/∂t | lim_{Δt→0} (Φ(t+Δt) - Φ(t))/Δt | Time derivative |
| ∂Φ/∂x | lim_{Δx→0} (Φ(x+Δx) - Φ(x))/Δx | Spatial x-derivative |
| ∇Φ | (∂Φ/∂x, ∂Φ/∂y) | Gradient vector |
| \|∇Φ\| | √((∂Φ/∂x)² + (∂Φ/∂y)²) | Gradient magnitude |
| ∇²Φ | ∂²Φ/∂x² + ∂²Φ/∂y² | Laplacian |
| ∇⁴Φ | ∇²(∇²Φ) | Biharmonic |

### Discrete Approximations (Second-Order)

**Laplacian (5-point stencil)**:
$$\nabla^2 \Phi_{i,j} \approx \frac{\Phi_{i+1,j} + \Phi_{i-1,j} + \Phi_{i,j+1} + \Phi_{i,j-1} - 4\Phi_{i,j}}{\Delta x^2}$$

**Gradient (central difference)**:
$$\frac{\partial \Phi}{\partial x}\bigg|_{i,j} \approx \frac{\Phi_{i+1,j} - \Phi_{i-1,j}}{2\Delta x}$$

**Time derivative (forward Euler)**:
$$\frac{\partial \Phi}{\partial t}\bigg|_n \approx \frac{\Phi_{n+1} - \Phi_n}{\Delta t}$$

### Transform Operators

| Notation | Definition | Description |
|----------|------------|-------------|
| F[Φ] or Φ̂ | ∑_x ∑_y Φ(x,y) e^{-2πi(ux/N + vy/M)} | 2D DFT |
| F⁻¹[Φ̂] | (1/NM) ∑_u ∑_v Φ̂(u,v) e^{2πi(ux/N + vy/M)} | Inverse 2D DFT |
| \|F[Φ]\| | √(Re² + Im²) | Magnitude spectrum |

---

## A.3 Norms and Functionals

### Norms

| Notation | Definition | Description |
|----------|------------|-------------|
| \|\|Φ\|\|₁ | ∑_{i,j} \|Φ_{i,j}\| | L1 norm (sum of absolutes) |
| \|\|Φ\|\|₂ | √(∑_{i,j} Φ_{i,j}²) | L2 norm (Euclidean) |
| \|\|Φ\|\|_∞ | max_{i,j} \|Φ_{i,j}\| | L∞ norm (maximum) |

### Functionals

| Notation | Definition | Description |
|----------|------------|-------------|
| L₁[Φ,Φ*] | \|\|Φ - Φ*\|\|₁ | L1 loss |
| L_s[Φ,Φ*] | \|\|F[Φ] - F[Φ*]\|\|₂ | Spectral loss |
| E[Φ] | ∫\|∇Φ\|² dxdy | Dirichlet energy |
| F[Φ] | ∫(η\|∇Φ\|²/2 + μ(1-Φ²)²/4) dxdy | Ginzburg-Landau energy |
| L_total | (1-β)L₁ + βL_s + λE | Combined loss |

---

## A.4 Special Functions and Patterns

### Initial Conditions

| Name | Definition |
|------|------------|
| noise(A) | Φ(x,y) = A × uniform(-1, 1) |
| bistable(ε) | Φ(x,y) = ε × uniform(-1, 1) |
| disk(r, x₀, y₀) | Φ = +1 if \|(x,y)-(x₀,y₀)\| < r, else -1 |
| domains(n, dir) | n alternating ±1 stripes |
| checkerboard(s) | Φ = sign(sin(πx/s) sin(πy/s)) |
| wave(k) | Φ = sin(kx) sin(ky) |
| vortex(m) | Φ = tanh(r) cos(mθ) |

### Target Patterns

Same as initial conditions—any pattern can be a target.

---

## A.5 Greek Alphabet Reference

| Letter | Name | Common Use |
|--------|------|------------|
| α | alpha | Memory feedback, learning rate modifier |
| β | beta | Spectral loss weight |
| γ | gamma | Memory decay/persistence |
| δ | delta | Drift feedback, small change |
| ε | epsilon | Small perturbation, threshold |
| η | eta | Diffusion coefficient |
| θ | theta | Parameter vector, angle |
| λ | lambda | Steepening coefficient, eigenvalue |
| μ | mu | Bistable strength, mean |
| ν | nu | Decay rate |
| ρ | rho | Shell indicator (ρq) |
| σ | sigma | Standard deviation, error field |
| τ | tau | Threshold, time constant |
| Φ | Phi | Primary scalar field |
| Ω | Omega | Memory field (Ω^) |
| Σ | Sigma | Symbol emitter |

---

## A.6 Symbol Vocabulary

| Symbol | Code | Name | Condition |
|--------|------|------|-----------|
| ρ | SHELL | Shell | ∇Φ exceeds threshold |
| λ | LATCH | Latch | CLA^ locked for N frames |
| ∧ | AND | And | Converging shell gradients |
| ∨ | OR | Or | Diverging shell gradients |
| ¬ | NOT | Not | Enclosed by opposing shells |
| ⊕ | XOR | Xor | Perpendicular shell crossing |
| Ω | MEMORY | Memory | Ω^ exceeds threshold |
| ↓ | COLLAPSE | Collapse | Mean \|Φ\| decreases >10% |
| ↑ | EXPAND | Expand | Mean \|Φ\| increases >10% |
| • | PULSE | Pulse | Transient high activity |
| ∅ | VOID | Void | Zero-region expands |
| ∞ | SATURATE | Saturate | \|Φ\| hits bounds |

---

## A.7 CLA^ State Values

| Value | State | Description |
|-------|-------|-------------|
| 0 | VOID | No shells nearby |
| 1 | PASS | 1-2 aligned shells |
| 2 | AND | ≥3 shells, convergent |
| 3 | OR | ≥3 shells, divergent |
| 4 | NOT | Enclosed opposition |
| 5 | XOR | Perpendicular crossing |
| 6 | LATCH | Dense shell region (≥7) |

---

## A.8 Units

The system is dimensionless. All quantities are normalized:

| Quantity | Normalization |
|----------|---------------|
| Φ | [-1, 1] typical, clamped to [-1.5, 1.5] |
| x, y | [0, N-1], [0, M-1] pixels |
| t | [0, T] arbitrary time units |
| Δx, Δy | 1 (pixel units) |
| Δt | Set by user (typically 0.1-1.0) |

For physical interpretation, multiply by appropriate scales:
- Length: L = Δx × (physical length per pixel)
- Time: T = Δt × (physical time per step)
- Field: Φ_physical = Φ × (physical field scale)

---

## A.9 Complexity Notation

| Notation | Meaning |
|----------|---------|
| O(f(n)) | Asymptotic upper bound (big-O) |
| Θ(f(n)) | Asymptotic tight bound (big-Theta) |
| O(N²) | Quadratic in grid size |
| O(N⁴) | Quartic (naive 2D DFT) |
| O(N² log N) | FFT complexity |

---

## A.10 Equation Summary

**CSS Evolution**:
$$\frac{\partial \Phi}{\partial t} = \eta \nabla^2 \Phi - \lambda |\nabla \Phi|^2 + \mu \Phi(1-\Phi^2) + \alpha \delta_{\text{drift}}$$

**Memory**:
$$\hat{\Omega}_{n+1} = \gamma \hat{\Omega}_n + (1-\gamma) \rho_q |\Phi|$$

**Drift**:
$$\delta_{\text{drift}} = \nabla^2 \hat{\Omega} - \hat{\Omega}$$

**Shell**:
$$\rho_q = \mathbf{1}_{|\nabla\Phi| > \tau}$$

**Spectral Loss**:
$$\mathcal{L}_s = ||F[\Phi] - F[\Phi^*]||_2$$

**Combined Loss**:
$$\mathcal{L} = (1-\beta)||\Phi - \Phi^*||_1 + \beta \mathcal{L}_s + \lambda E[\Phi]$$

**Learning Update**:
$$\Phi_{n+1} = \Phi_n + \Delta t \cdot F[\Phi_n] - lr \cdot (\Phi_n - \Phi^*) \cdot \Delta t$$

**Swift-Hohenberg**:
$$\frac{\partial \Phi}{\partial t} = r\Phi - (1 + \nabla^2)^2 \Phi - \Phi^3$$

---

## A.11 Glossary

| Term | Definition |
|------|------------|
| **Attractor** | State toward which dynamics converge |
| **Bistable** | Having two stable equilibria |
| **Boundary condition** | Constraint at domain edges |
| **CLA^** | Collapse Logic Algebra |
| **CSS** | Collapse Sentience Simulation |
| **Diffusion** | Spreading process driven by ∇² |
| **Domain** | Contiguous region of similar Φ |
| **DFT** | Discrete Fourier Transform |
| **Energy functional** | Quantity minimized by dynamics |
| **FFT** | Fast Fourier Transform |
| **Field** | Function assigning values to space |
| **Functional** | Map from functions to numbers |
| **Gradient** | Direction of steepest increase |
| **Laplacian** | Divergence of gradient (∇²) |
| **Latch** | Frozen/locked configuration |
| **Loss** | Measure of distance from goal |
| **Memory** | Accumulated history (Ω^) |
| **PDE** | Partial Differential Equation |
| **Resolution** | Convergence to target |
| **Shell** | High-gradient boundary region |
| **Spectral** | Relating to frequency domain |
| **Substrate** | Underlying computational medium |
| **Target** | Goal configuration (Φ*) |

---

*← [Chapter 8.0: Implications for AGI](Chapter_8.0_Implications_for_AGI.md) | [Appendix B: Code Models](Appendix_B_Code_Models.md) →*
