# Executable GIF Physics

## Φ → ∂Φ/∂t → Ω^ → CLA^ → Σ → Frames

**A Self-Resolving Dynamical System Based on Intent Tensor Theory**

*Continuous fields evolve, form shells, crystallize into logic gates, and emit discrete symbols.*

**Built by Armstrong Knight & Abdullah Khan | [FunnelFunction](https://funnelfunction.com)**

---

## The Complete Pipeline

```
Phase 1-2: Φ evolves     (physics)
Phase 3:   Ω^ remembers  (where shells formed)
Phase 4:   CLA^ classifies (logic gates from shells)
Phase 5:   δ_drift feeds back (memory modifies evolution)
Phase 6:   Σ emits symbols (discrete tokens from continuous dynamics)
```

This is not a drawing tool. It is a **physics simulator** that outputs GIFs while simultaneously:
- Computing nonlinear PDEs
- Tracking memory of where dynamics occurred
- Classifying shell configurations as logic gates
- Emitting discrete symbols from continuous evolution

---

## Quick Start

```
Φ₀ = bistable(ε=0.4, seed=42)
∂Φ/∂t = sigma(η=0.8, λ=0.1, μ=0.6, α=0.05)
t ∈ [0, 25], Δt = 1
canvas = 150×150
colormap = plasma
```

This runs the **full pipeline**: CSS evolution with memory, CLA^ classification, and symbol emission Σ.

---

## Architecture

### The Six Phases

| Phase | Component | What It Does |
|-------|-----------|--------------|
| **1-2** | Φ(x,y,t) | Scalar field evolves via PDEs (diffusion, waves, nonlinear dynamics) |
| **3** | Ω^(x,y,t) | Memory field accumulates where shells (high gradient regions) form |
| **4** | CLA^[i,j] | Collapse Logic Algebra classifies shell configurations as logic gates |
| **5** | δ_drift | Memory feeds back into evolution: δ_drift = ∇²Ω^ - Ω^ |
| **6** | Σ | Symbol emission: discrete tokens emerge from continuous dynamics |

### Mathematical Flow

```
Φ₀(x,y)         Initial State (what EXISTS)
    ↓
L[Φ]            Evolution Operator (how it CHANGES)  
    ↓
Φ(x,y,t)        Solution (what HAPPENS)
    ↓
Ω^(x,y,t)       Memory (what PERSISTS)
    ↓
CLA^[i,j]       Logic Gates (what COMPUTES)
    ↓
Σ               Symbols (what it SAYS)
    ↓
GIF + Log       Output (what we SEE)
```

---

## Syntax

### Full Specification

```
Φ₀ = type(param1=value, param2=value, ...)
∂Φ/∂t = operator(param1=value, ...)
t ∈ [start, end], Δt = step
canvas = width×height
colormap = name
```

### Mathematical Constants

| Symbol | Value | Usage |
|--------|-------|-------|
| π | 3.14159... | `λ=2π`, `t ∈ [0, π]` |
| τ | 2π = 6.28318... | `t ∈ [0, τ]` |
| e | 2.71828... | Euler's number |

---

## Initial Conditions (Φ₀)

### Phase 1: Basic Fields

| Type | Form | Description |
|------|------|-------------|
| `wave(A, λ, θ)` | A·sin(k·x + φ) | Plane wave |
| `gaussian(A, σ)` | A·exp(-r²/2σ²) | Localized bump |
| `ring(A, r, σ)` | A·exp(-(r-r₀)²/2σ²) | Circular ring |
| `radial(A, λ)` | A·sin(k·r) | Concentric waves |
| `spiral(A, λ, m)` | A·sin(k·r + m·θ) | m-armed spiral |
| `interference(A, λ, sep)` | A·(sin(k·r₁) + sin(k·r₂)) | Two-source pattern |
| `noise(A, seed)` | Random [0, A] | White noise |
| `constant(c)` | c | Uniform field |

### Phase 2: CSS Fields

| Type | Description | Use With |
|------|-------------|----------|
| `bistable(ε, seed, bias)` | Small noise ε·random[-1,1] | Allen-Cahn, CSS |
| `domains(n, dir, noise)` | Pre-initialized ±1 stripes | Allen-Cahn |
| `disk(r, sharpness)` | Central +1 region | Curvature flow |
| `vortex(m)` | Topological singularity | Phase dynamics |
| `turing(A, seed)` | Turing-optimized noise | Pattern formation |

---

## Evolution Operators (∂Φ/∂t)

### Phase 1: Linear Operators

| Operator | Equation | Description |
|----------|----------|-------------|
| `advect(c)` | ∂Φ/∂t = -c·∇Φ | Transport |
| `diffuse(D)` | ∂Φ/∂t = D·∇²Φ | Heat equation |
| `wave(c)` | ∂²Φ/∂t² = c²·∇²Φ | Wave equation |
| `rotate(ω)` | θ(t) = θ₀ + ω·t | Rotation |
| `expand(v)` | r(t) = r₀ + v·t | Radial expansion |
| `oscillate(ω, a)` | Φ(t) = Φ₀·(1 + a·sin(ω·t)) | Pulsing |
| `phase(ω)` | φ(t) = φ₀ + ω·t | Phase advance |

### Phase 2: Nonlinear PDEs

| Operator | Equation | Description |
|----------|----------|-------------|
| `allen_cahn(ε)` | ε²∇²Φ + Φ(1-Φ²) | Bistable coarsening |
| `ginzburg(D, α, β)` | D∇²Φ + αΦ - βΦ³ | Pattern formation |
| `cahn_hilliard(M, γ)` | M∇²(Φ³ - Φ - γ∇²Φ) | Conserved separation |
| `fisher(D, r)` | D∇²Φ + rΦ(1-Φ) | Traveling fronts |
| `swift_hohenberg(r, g)` | rΦ - (1+∇²)²Φ - gΦ³ | Wavelength selection |
| `kuramoto(ν)` | -∇²Φ - ν∇⁴Φ - \|∇Φ\|²/2 | Spatiotemporal chaos |
| `collapse(η, λ, μ)` | η∇²Φ - λ\|∇Φ\|² + μΦ(1-Φ²) | Full CSS (bounded) |

### Phase 3 & 5: Memory Operators

| Operator | Description |
|----------|-------------|
| `css(η, λ, μ, α, γ, τ)` | Full CSS with memory Ω^ and drift feedback δ_drift |

**Memory equation:** Ω^[n+1] = γ·Ω^[n] + (1-γ)·ρq·|Φ|

**Drift feedback:** δ_drift = ∇²Ω^ - Ω^

**Evolution:** ∂Φ/∂t = η∇²Φ - λ|∇Φ|² + μΦ(1-Φ²) + α·δ_drift·Φ

| Parameter | Symbol | Description |
|-----------|--------|-------------|
| η | Diffusion | Curvature spreading |
| λ | Steepening | Tension concentration |
| μ | Bistable | Reaction strength |
| α | Feedback | Memory → evolution coupling |
| γ | Persistence | How long memory lasts (0-1) |
| τ | Threshold | Gradient needed for shell detection |

### Phase 4: CLA^ Operators

| Operator | Description |
|----------|-------------|
| `cla_view(k)` | Visualize CLA states (use with `colormap=cla`) |
| `cla_css(η, μ, k)` | CSS evolution modulated by logic gates |
| `cla_count(...)` | CSS + log gate counts each frame |

**CLA^ State Classification:**

| State | Name | Color | Description |
|-------|------|-------|-------------|
| 0 | VOID | Black | No shells in neighborhood |
| 1 | PASS | Gray | Single shell direction (signal propagates) |
| 2 | AND | Red | Converging shells (conjunction) |
| 3 | OR | Blue | Diverging shells (disjunction) |
| 4 | NOT | Yellow | Enclosed by shells (negation context) |
| 5 | XOR | Magenta | Perpendicular crossing (exclusive or) |
| 6 | LATCH | White | Dense shell region (memory lock) |

### Phase 6: Symbol Emission Σ

| Operator | Description |
|----------|-------------|
| `sigma(η, λ, μ, α, γ)` | Full pipeline: CSS + memory + CLA^ + symbols |
| `sigma_view(...)` | Visualize emission hotspots |
| `sigma_collapse(...)` | Simpler collapse + symbols |
| `sigma_allen_cahn(ε)` | Allen-Cahn + symbols |

**Symbol Vocabulary:**

| Symbol | Name | Trigger |
|--------|------|---------|
| ρ | shell | New termination surfaces form |
| λ | latch | Memory lock stabilizes (N frames) |
| ∧ | and | AND gate count surges |
| ∨ | or | OR gate count surges |
| ¬ | not | NOT context emerges |
| ⊕ | xor | XOR crossing emerges |
| Ω | memory | Memory threshold crossed |
| ↓ | collapse | Field mean decreases |
| ↑ | expand | Field mean increases |
| • | pulse | Transient range spike |
| ∅ | void | Ground state expands |
| ∞ | saturate | Field hits clamp bounds |

**Example Output (Evolution Log):**
```
t = 5.0000: Φ ∈ [-0.998, 0.998] | Σ: ρ(shell) ∧(and)
t = 6.0000: Φ ∈ [-1.000, 1.000] | Σ: λ(latch)
---
═══ SYMBOL EMISSION Σ ═══
Σ Sequence: ρ∧λρ∨Ω
Total symbols: 6
Frequencies: ρ:2 ∧:1 λ:1 ∨:1 Ω:1
Patterns: "ρ∧"×2
```

---

## Colormaps

| Name | Description | Best For |
|------|-------------|----------|
| `plasma` | Blue → Purple → Orange → Yellow | General fields |
| `viridis` | Purple → Blue → Green → Yellow | Scientific |
| `thermal` | Black → Red → Yellow → White | Intensity |
| `ocean` | Deep blue → Cyan → White | Waves |
| `diverging` | Blue → White → Red | ±values |
| `neon` | Dark with bright signal | Structure |
| `grayscale` | Black → White | Simple |
| `phase` | Cyclic HSV | Periodic |
| `cla` | Discrete 7-color | CLA^ states |
| `shells` | Dark with bright shells | ρq visualization |

---

## Examples by Phase

### Phase 1: Basic Physics

```
// Propagating wave
Φ₀ = wave(A=1, λ=60, θ=0)
∂Φ/∂t = advect(c=2)
t ∈ [0, 2π], Δt = π/8
canvas = 400×400
colormap = ocean
```

```
// Rotating spiral
Φ₀ = spiral(A=1, λ=40, m=3)
∂Φ/∂t = rotate(ω=0.3)
t ∈ [0, 2π], Δt = π/8
canvas = 400×400
colormap = phase
```

### Phase 2: Nonlinear PDEs

```
// Allen-Cahn domain coarsening
Φ₀ = bistable(ε=0.5, seed=42)
∂Φ/∂t = allen_cahn(ε=1.5)
t ∈ [0, 20], Δt = 1
canvas = 200×200
colormap = diverging
```

```
// Swift-Hohenberg patterns
Φ₀ = bistable(ε=0.1, seed=2024)
∂Φ/∂t = swift_hohenberg(r=0.2, g=1)
t ∈ [0, 20], Δt = 1
canvas = 100×100
colormap = neon
```

### Phase 3 & 5: Memory + Drift

```
// Full CSS with memory feedback
Φ₀ = domains(n=4, noise=0.15)
∂Φ/∂t = css(η=0.8, λ=0.1, μ=0.5, α=0.05, γ=0.95)
t ∈ [0, 20], Δt = 1
canvas = 200×200
colormap = plasma
```

### Phase 4: CLA^ Logic Gates

```
// Visualize logic states
Φ₀ = domains(n=4, noise=0.2)
∂Φ/∂t = cla_view(k=0.8)
t ∈ [0, 10], Δt = 0.5
canvas = 200×200
colormap = cla
```

```
// Evolution modulated by gates
Φ₀ = bistable(ε=0.4, seed=42)
∂Φ/∂t = cla_css(η=0.8, μ=0.5, k=1.0)
t ∈ [0, 20], Δt = 1
canvas = 200×200
colormap = plasma
```

### Phase 6: Symbol Emission Σ

```
// Full pipeline with symbol output
Φ₀ = bistable(ε=0.4, seed=42)
∂Φ/∂t = sigma(η=0.8, λ=0.1, μ=0.6, α=0.05, γ=0.9)
t ∈ [0, 25], Δt = 1
canvas = 150×150
colormap = plasma
```

```
// Visualize emission hotspots
Φ₀ = domains(n=4, noise=0.15)
∂Φ/∂t = sigma_view(η=0.8, μ=0.5, α=0.05)
t ∈ [0, 20], Δt = 1
canvas = 200×200
colormap = thermal
```

---

## Mathematical Foundations

### Scalar Fields

A scalar field **Φ: Ω → ℝ** assigns a real number to each point in space.

**Discretization:**
```
Φ(x, y) ≈ Φᵢⱼ where x = i·Δx, y = j·Δy
```

### Differential Operators

**Gradient:**
```
∇Φ = (∂Φ/∂x, ∂Φ/∂y)
```

**Laplacian:**
```
∇²Φ = ∂²Φ/∂x² + ∂²Φ/∂y² ≈ (Φᵢ₊₁,ⱼ + Φᵢ₋₁,ⱼ + Φᵢ,ⱼ₊₁ + Φᵢ,ⱼ₋₁ - 4Φᵢⱼ) / Δx²
```

**Gradient Magnitude (Shell Detector):**
```
|∇Φ| = √((∂Φ/∂x)² + (∂Φ/∂y)²)
ρq = 1 where |∇Φ| > threshold, 0 otherwise
```

### Stability

The engine uses:
- **Substep iteration:** Large Δt broken into stable microsteps
- **Field clamping:** |Φ| ≤ clamp prevents runaway
- **Bounded bistable form:** Φ(1-Φ²) instead of Φ³

**CFL Conditions:**
- Diffusion: Δt < Δx² / (4D)
- Advection: Δt < Δx / |c|
- 4th-order: Δt < Δx⁴ / (16ν)

---

## Intent Tensor Theory Connection

| ITT Concept | Physics Implementation |
|-------------|------------------------|
| Φ (Potential) | Scalar field |
| ∇Φ (Tension) | Gradient direction |
| ρq (Shell) | High \|∇Φ\| regions |
| Ω^ (Memory) | Accumulated shell activity |
| CLA^ (Logic) | Gate classification from shells |
| Σ (Symbol) | Discrete output tokens |
| δ_drift (Feedback) | Memory → evolution coupling |

### The Self-Referential Property

When α > 0 in CSS/sigma operators:
1. Field evolves and forms shells
2. Memory accumulates at shells
3. Memory creates drift feedback
4. Drift modifies future evolution
5. **The system rewrites its own dynamics**

---

## Technical Implementation

### Stack

- **Pure JavaScript** - Zero dependencies
- **HTML5 Canvas** - Field rendering
- **Float32Array** - Numerical precision
- **LZW Compression** - GIF encoding
- **Single HTML file** - ~3700 lines

### Components

| Component | Lines | Purpose |
|-----------|-------|---------|
| ScalarField | ~400 | Field math + CLA^ methods |
| SymbolEmitter | ~300 | Σ = g(ρq, CLA^, Ω^) |
| Evolution Operators | ~1200 | 18 physics engines |
| Initial Conditions | ~300 | 10 field generators |
| Colormaps | ~150 | 10 visualization maps |
| GIF Encoder | ~400 | LZW compression |
| Parser | ~200 | Mathematical notation |
| UI | ~200 | Web interface |

### Boundary Conditions

**Periodic:** Field wraps around edges.

---

## Links

- **Live App:** [https://render-executable-gif-physics.onrender.com](https://render-executable-gif-physics.onrender.com)
- **GitHub:** [https://github.com/FunnelFunction/0.0_Executable_GIF_Physics](https://github.com/FunnelFunction/0.0_Executable_GIF_Physics)
- **FunnelFunction:** [https://funnelfunction.com](https://funnelfunction.com)
- **Intent Tensor Theory:** [https://intent-tensor-theory.com](https://intent-tensor-theory.com)
- **ITT Coding Principals:** [https://github.com/intent-tensor-theory/0.0_Coding_Principals_Intent_Tensor_Theory](https://github.com/intent-tensor-theory/0.0_Coding_Principals_Intent_Tensor_Theory)
- **Marketing Principals:** [https://github.com/FunnelFunction/0.0_git_funnelfunction_marketing_Principals](https://github.com/FunnelFunction/0.0_git_funnelfunction_marketing_Principals)

---

## Credits

### Human Authors
- **Armstrong Knight** - Architecture, ITT Mathematics
- **Abdullah Khan** - Co-founder, Business Strategy

### AI Collaborators
- **Claude (Anthropic)** - Primary development
- **ChatGPT (OpenAI)** - Ideation
- **Grok (xAI)** - Mathematical validation
- **Gemini (Google)** - Research synthesis

---

## License

MIT License - Free for commercial and personal use.

---

*The system speaks. Continuous dynamics emit discrete symbols. Σ = g(ρq, CLA^, Ω^).*
