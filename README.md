# Executable GIF Physics

## Φ → ∂Φ/∂t → Frames

**A Visual Physics Simulator Based on Intent Tensor Theory**

*Self-resolving dynamical systems that generate GIF animations through mathematical evolution.*

**Built by Armstrong Knight & Abdullah Khan | [FunnelFunction](https://funnelfunction.com)**

---

## The Paradigm Shift

This is not a drawing tool. It is a **physics simulator** that happens to output GIFs.

| Traditional GIF Tools | Executable GIF Physics |
|-----------------------|------------------------|
| User specifies each frame | User specifies initial conditions |
| System renders what's requested | System computes what happens |
| Static frames, manually arranged | Dynamic evolution, mathematically derived |
| User is designer | User is physicist |
| System is renderer | System is simulator |

---

## Mathematical Architecture

### The Core Equation

```
∂Φ/∂t = L[Φ]
```

Where:
- **Φ(x, y, t)** is a scalar field over space and time
- **L** is a differential operator (the "law of physics")
- **t** is time

The system computes **Φ(t)** by numerical integration, then renders each timestep as a GIF frame.

### The Computational Flow

```
Φ₀(x,y)         Initial State (what EXISTS)
    ↓
L[Φ]            Evolution Operator (how it CHANGES)  
    ↓
Φ(x,y,t)        Solution (what HAPPENS)
    ↓
colormap        Observation (what we SEE)
    ↓
GIF             Output (what we SAVE)
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

### Time Domain

The time domain determines how many frames are generated:

```
t ∈ [0, 2π], Δt = π/4
```

This generates `2π / (π/4) = 8` frames.

Alternatively:
```
frames = 8
```

---

## Initial Conditions (Φ₀)

### Wave
```
Φ₀ = wave(A=amplitude, λ=wavelength, φ=phase, θ=angle)
```

Mathematical form: **Φ₀(x, y) = A · sin(k·x·cos(θ) + k·y·sin(θ) + φ)**

Where k = 2π/λ is the wave number.

| Parameter | Description | Default |
|-----------|-------------|---------|
| A | Amplitude | 1 |
| λ | Wavelength (pixels) | 50 |
| φ | Initial phase | 0 |
| θ | Propagation angle (degrees) | 0 |

### Gaussian
```
Φ₀ = gaussian(A=amplitude, x=center_x, y=center_y, σ=width)
```

Mathematical form: **Φ₀(x, y) = A · exp(-((x-x₀)² + (y-y₀)²) / (2σ²))**

| Parameter | Description | Default |
|-----------|-------------|---------|
| A | Peak amplitude | 1 |
| x, y | Center position | canvas center |
| σ | Standard deviation (width) | 30 |

### Ring
```
Φ₀ = ring(A=amplitude, r=radius, σ=width, x=cx, y=cy)
```

Mathematical form: **Φ₀(x, y) = A · exp(-(r - r₀)² / (2σ²))**

Where r = √((x-x₀)² + (y-y₀)²)

| Parameter | Description | Default |
|-----------|-------------|---------|
| A | Peak amplitude | 1 |
| r | Ring radius | 50 |
| σ | Ring width | 10 |
| x, y | Center | canvas center |

### Radial
```
Φ₀ = radial(A=amplitude, λ=wavelength, φ=phase, x=cx, y=cy)
```

Mathematical form: **Φ₀(x, y) = A · sin(k·r + φ)**

Concentric circular waves emanating from center.

### Spiral
```
Φ₀ = spiral(A=amplitude, λ=wavelength, m=arms, φ=phase)
```

Mathematical form: **Φ₀(x, y) = A · sin(k·r + m·θ + φ)**

Where (r, θ) are polar coordinates and m is the number of spiral arms.

### Interference
```
Φ₀ = interference(A=amplitude, λ=wavelength, sep=separation)
```

Mathematical form: **Φ₀ = A·(sin(k·r₁) + sin(k·r₂))**

Two-source interference pattern.

### Checkerboard
```
Φ₀ = checkerboard(A=amplitude, size=checker_size)
```

Mathematical form: **Φ₀(x, y) = A · sign(sin(kx·x) · sin(ky·y))**

### Noise
```
Φ₀ = noise(A=amplitude, seed=random_seed)
```

Random field: **Φ₀(x, y) ~ Uniform(0, A)**

### Constant
```
Φ₀ = constant(c=value)
```

Uniform field: **Φ₀(x, y) = c**

---

## Evolution Operators (∂Φ/∂t)

### Advection
```
∂Φ/∂t = advect(c=velocity)
∂Φ/∂t = advect(cx=vx, cy=vy)
```

Mathematical form: **∂Φ/∂t = -c · ∇Φ**

Transports the field along direction c. Makes waves propagate.

| Parameter | Description | Default |
|-----------|-------------|---------|
| c | Velocity (isotropic) | 1 |
| cx, cy | Velocity components | c, 0 |

### Diffusion
```
∂Φ/∂t = diffuse(D=coefficient)
```

Mathematical form: **∂Φ/∂t = D · ∇²Φ**

The heat equation. Field spreads out and smooths over time.

| Parameter | Description | Default |
|-----------|-------------|---------|
| D | Diffusion coefficient | 0.1 |

### Wave Equation
```
∂Φ/∂t = wave(c=speed)
```

Mathematical form: **∂²Φ/∂t² = c² · ∇²Φ**

True wave dynamics with velocity field. Preserves wave structure.

### Rotation
```
∂Φ/∂t = rotate(ω=angular_velocity)
```

Mathematical form: **θ(t) = θ₀ + ω·t**

Rotates the entire field around the center.

| Parameter | Description | Default |
|-----------|-------------|---------|
| ω | Angular velocity (rad/time) | 0.1 |
| cx, cy | Rotation center | canvas center |

### Expansion
```
∂Φ/∂t = expand(v=velocity)
```

Mathematical form: **r(t) = r₀ + v·t**

Radial expansion from center. Makes patterns grow outward.

### Oscillation
```
∂Φ/∂t = oscillate(ω=frequency, a=amplitude)
```

Mathematical form: **Φ(t) = Φ₀ · (1 + a·sin(ω·t))**

Modulates field amplitude sinusoidally. Creates pulsing effect.

### Phase Shift
```
∂Φ/∂t = phase(ω=frequency)
```

Advances the phase of oscillatory initial conditions:
**φ(t) = φ₀ + ω·t**

Works with wave, radial, spiral initial conditions.

### Reaction-Diffusion
```
∂Φ/∂t = react(D=diffusion, a=threshold)
```

Mathematical form: **∂Φ/∂t = D·∇²Φ + Φ(1-Φ)(Φ-a)**

Bistable reaction-diffusion. Creates organic patterns.

### None
```
∂Φ/∂t = none()
```

Static field. No evolution.

---

## Colormaps

Maps field values Φ ∈ [min, max] to colors.

| Name | Description | Best For |
|------|-------------|----------|
| `plasma` | Blue → Purple → Orange → Yellow | General fields |
| `viridis` | Purple → Blue → Green → Yellow | Scientific data |
| `thermal` | Black → Red → Yellow → White | Temperature/intensity |
| `ocean` | Deep blue → Cyan → White | Wave fields |
| `diverging` | Blue → White → Red | Positive/negative values |
| `neon` | Dark with bright signal | Structure emphasis |
| `grayscale` | Black → White | Simple visualization |
| `phase` | Cyclic (returns to start) | Periodic quantities |

---

## Examples

### 1. Propagating Plane Wave

```
Φ₀ = wave(A=1, λ=60, θ=0)
∂Φ/∂t = advect(c=2)
t ∈ [0, 2π], Δt = π/8
canvas = 400×400
colormap = ocean
```

A sinusoidal wave moving to the right.

### 2. Expanding Ring

```
Φ₀ = ring(A=1, r=50, σ=15)
∂Φ/∂t = expand(v=0.8)
t ∈ [0, 2π], Δt = π/6
canvas = 400×400
colormap = plasma
```

A ring that grows outward from the center.

### 3. Rotating Spiral

```
Φ₀ = spiral(A=1, λ=40, m=3)
∂Φ/∂t = rotate(ω=0.3)
t ∈ [0, 2π], Δt = π/8
canvas = 400×400
colormap = phase
```

A three-armed spiral that rotates.

### 4. Heat Diffusion

```
Φ₀ = gaussian(A=1, σ=30)
∂Φ/∂t = diffuse(D=50)
t ∈ [0, 1], Δt = 0.1
canvas = 400×400
colormap = thermal
```

A hot spot spreading out over time.

### 5. Two-Source Interference

```
Φ₀ = interference(A=1, λ=30, sep=120)
∂Φ/∂t = phase(ω=0.5)
t ∈ [0, 2π], Δt = π/6
canvas = 400×400
colormap = diverging
```

Interference pattern with evolving phase.

### 6. Radial Pulse (Inward)

```
Φ₀ = radial(A=1, λ=50)
∂Φ/∂t = advect(c=-1.5)
t ∈ [0, 2π], Δt = π/8
canvas = 400×400
colormap = neon
```

Concentric rings moving inward.

### 7. Reaction-Diffusion Patterns

```
Φ₀ = noise(A=1)
∂Φ/∂t = react(D=1, a=0.3)
t ∈ [0, 5], Δt = 0.5
canvas = 200×200
colormap = viridis
```

Emergent patterns from noise.

### 8. Standing Wave (Pulsing)

```
Φ₀ = wave(A=1, λ=80)
∂Φ/∂t = oscillate(ω=2, a=0.8)
t ∈ [0, 2π], Δt = π/8
canvas = 400×400
colormap = diverging
```

A wave pattern that pulses in intensity.

### 9. Diagonal Wave

```
Φ₀ = wave(A=1, λ=50, θ=45)
∂Φ/∂t = advect(cx=1.5, cy=1.5)
t ∈ [0, 2π], Δt = π/8
canvas = 400×400
colormap = ocean
```

Wave propagating diagonally.

### 10. Rotating Radial Pattern

```
Φ₀ = radial(A=1, λ=40)
∂Φ/∂t = rotate(ω=0.5)
t ∈ [0, 2π], Δt = π/8
canvas = 400×400
colormap = plasma
```

Concentric rings that rotate (creates spiral illusion).

### 11. Checkerboard Diffusion

```
Φ₀ = checkerboard(A=1, size=30)
∂Φ/∂t = diffuse(D=20)
t ∈ [0, 2], Δt = 0.2
canvas = 400×400
colormap = grayscale
```

Sharp checkerboard blurring over time.

### 12. Multi-Arm Spiral

```
Φ₀ = spiral(A=1, λ=30, m=6)
∂Φ/∂t = rotate(ω=-0.4)
t ∈ [0, 2π], Δt = π/6
canvas = 400×400
colormap = phase
```

Six-armed spiral rotating backwards.

### 13. Gaussian Pulse Expansion

```
Φ₀ = gaussian(A=1, σ=20)
∂Φ/∂t = expand(v=1.2)
t ∈ [0, π], Δt = π/8
canvas = 400×400
colormap = thermal
```

A localized pulse that expands into a ring.

### 14. Slow Wave Evolution

```
Φ₀ = wave(A=1, λ=100)
∂Φ/∂t = advect(c=0.5)
t ∈ [0, 4π], Δt = π/4
canvas = 400×400
colormap = ocean
```

Long wavelength, slow propagation.

### 15. Rapid Oscillation

```
Φ₀ = radial(A=1, λ=60)
∂Φ/∂t = oscillate(ω=4, a=0.9)
t ∈ [0, 2π], Δt = π/16
canvas = 400×400
colormap = neon
```

Fast-pulsing radial pattern.

---

## The Mathematics in Depth

### Scalar Fields

A scalar field **Φ: Ω → ℝ** assigns a real number to each point in space.

**Discretization:** We approximate continuous space with a grid:
```
Φ(x, y) ≈ Φᵢⱼ where x = i·Δx, y = j·Δy
```

### Gradient

The gradient **∇Φ** points in the direction of steepest increase:
```
∇Φ = (∂Φ/∂x, ∂Φ/∂y)
```

Computed numerically using central differences:
```
∂Φ/∂x ≈ (Φᵢ₊₁,ⱼ - Φᵢ₋₁,ⱼ) / (2Δx)
```

### Laplacian

The Laplacian **∇²Φ** measures how different a point is from its neighbors:
```
∇²Φ = ∂²Φ/∂x² + ∂²Φ/∂y²
```

Computed numerically:
```
∇²Φ ≈ (Φᵢ₊₁,ⱼ + Φᵢ₋₁,ⱼ + Φᵢ,ⱼ₊₁ + Φᵢ,ⱼ₋₁ - 4Φᵢⱼ) / Δx²
```

### Time Stepping

We evolve the field using Euler's method:
```
Φ(t + Δt) ≈ Φ(t) + Δt · (∂Φ/∂t)
```

Each timestep produces one GIF frame.

### Stability

Numerical stability requires:
- **Diffusion:** Δt < Δx² / (4D)
- **Advection:** Δt < Δx / |c| (CFL condition)
- **Wave:** Δt < Δx / c

The engine automatically enforces these constraints.

---

## Phase 2: CSS (Collapse Sentience Simulator) Operators

Phase 2 introduces nonlinear evolution operators based on the Intent Tensor Theory mathematical foundations. These operators produce spontaneous pattern formation, domain walls, and self-organizing dynamics.

### The Collapse Genesis Stack

```
Φ → ∇Φ → ∇×F → ∇²Φ → ρq
↓     ↓      ↓       ↓      ↓
0D   1D    2D     3D    3D+
```

Each operator in Phase 2 encodes a layer of dimensional emergence.

---

### CSS Initial Conditions

#### Bistable Noise
```
Φ₀ = bistable(ε=amplitude, bias=0, seed=42)
```
Small random perturbations around the unstable equilibrium (Φ ≈ 0). Under bistable evolution, spontaneously organizes into domains of Φ ≈ ±1.

#### Domains
```
Φ₀ = domains(n=4, dir=vertical, noise=0.05)
```
Pre-initialized stripes of +1 and -1. Watch domain walls move under coarsening dynamics.

#### Disk
```
Φ₀ = disk(r=50, sharpness=5)
```
Central region of +1 surrounded by -1. Models a single coherent shell.

#### Turing Seed
```
Φ₀ = turing(A=0.5, seed=1234)
```
Noise optimized for Turing pattern formation.

#### Vortex
```
Φ₀ = vortex(m=1)
```
Topological phase singularity with winding number m.

---

### CSS Evolution Operators

#### Collapse (Full CSS)
```
∂Φ/∂t = collapse(η=1, λ=0.5, μ=1, ν=0)
```

Mathematical form: **∂Φ/∂t = η∇²Φ - λ|∇Φ|² + μΦ³ - νΦ**

ITT Mapping:
| Term | Symbol | ITT Meaning |
|------|--------|-------------|
| η∇²Φ | Δ₃/Δ₄ | Curvature spreading |
| -λ\|∇Φ\|² | Δ₁ | Tension concentration |
| +μΦ³ | Lock | Cubic bistability |
| -νΦ | Sθ | Entropic decay |

#### Allen-Cahn
```
∂Φ/∂t = allen_cahn(ε=0.5)
```

Mathematical form: **∂Φ/∂t = ε²∇²Φ + Φ(1 - Φ²)**

Bistable dynamics with stable states Φ = ±1. Domain walls form where Φ = 0 and move by mean curvature. Small domains shrink, large domains grow.

#### Ginzburg-Landau
```
∂Φ/∂t = ginzburg(D=1, α=1, β=1)
```

Mathematical form: **∂Φ/∂t = D∇²Φ + αΦ - βΦ³**

Universal equation for pattern formation near critical points. Produces stripes, labyrinths, and spot arrays.

#### Cahn-Hilliard
```
∂Φ/∂t = cahn_hilliard(M=1, γ=1)
```

Mathematical form: **∂Φ/∂t = M∇²(Φ³ - Φ - γ∇²Φ)**

Conserved phase separation. Total Φ is preserved while the field reorganizes into domains. Produces spinodal decomposition and Ostwald ripening.

#### Fisher-KPP
```
∂Φ/∂t = fisher(D=1, r=1)
```

Mathematical form: **∂Φ/∂t = D∇²Φ + rΦ(1 - Φ)**

Traveling wave fronts. Wave speed c = 2√(Dr). Models invasion of Φ=0 regions by Φ=1.

#### Swift-Hohenberg
```
∂Φ/∂t = swift_hohenberg(r=0.3, g=1)
```

Mathematical form: **∂Φ/∂t = rΦ - (1 + ∇²)²Φ - gΦ³**

Pattern selection with preferred wavelength. Produces regular stripes, hexagons, or labyrinths depending on parameters.

#### Kuramoto-Sivashinsky
```
∂Φ/∂t = kuramoto(ν=1)
```

Mathematical form: **∂Φ/∂t = -∇²Φ - ν∇⁴Φ - |∇Φ|²/2**

Spatiotemporal chaos. Produces flame-front-like patterns that never stabilize.

#### CSS with Memory
```
∂Φ/∂t = css(η=1, λ=0.2, μ=0.3, α=0.1, γ=0.9, τ=0.5)
```

Mathematical form:
```
∂Φ/∂t = η∇²Φ - λ|∇Φ|² + μΦ³ + α·δ_drift
Ω^_{n+1} = γ·Ω^_n + (1-γ)·ρq·|Φ|
δ_drift = ∇²Ω^ - Ω^
```

Full CSS with recursive memory field (Ω^). The system accumulates memory where shells form (high gradient regions) and uses that memory to modulate future evolution.

| Parameter | Symbol | Description |
|-----------|--------|-------------|
| α | Feedback | Memory influence on evolution |
| γ | Persistence | How long memory lasts (0-1) |
| τ | Threshold | Gradient needed for shell detection |

---

### CSS Examples

#### 1. Spontaneous Domain Formation
```
Φ₀ = bistable(ε=0.3, seed=42)
∂Φ/∂t = allen_cahn(ε=2)
t ∈ [0, 10], Δt = 0.5
canvas = 200×200
colormap = diverging
```

Watch random noise spontaneously organize into domains of +1 (red) and -1 (blue).

#### 2. Domain Wall Motion
```
Φ₀ = domains(n=6, dir=vertical)
∂Φ/∂t = allen_cahn(ε=1.5)
t ∈ [0, 15], Δt = 0.75
canvas = 200×200
colormap = diverging
```

Pre-initialized stripes coarsen as curved boundaries move to reduce total interface length.

#### 3. Shrinking Disk
```
Φ₀ = disk(r=80)
∂Φ/∂t = allen_cahn(ε=1)
t ∈ [0, 20], Δt = 1
canvas = 300×300
colormap = plasma
```

A circular domain shrinks due to curvature-driven dynamics. Demonstrates motion by mean curvature.

#### 4. Pattern Selection
```
Φ₀ = bistable(ε=0.05, seed=2024)
∂Φ/∂t = swift_hohenberg(r=0.5)
t ∈ [0, 30], Δt = 1.5
canvas = 150×150
colormap = neon
```

Watch a preferred wavelength emerge from noise, producing regular patterns.

#### 5. Self-Referential CSS
```
Φ₀ = domains(n=4, noise=0.2)
∂Φ/∂t = css(η=1, λ=0.2, μ=0.3, α=0.1, γ=0.9)
t ∈ [0, 12], Δt = 0.6
canvas = 200×200
colormap = plasma
```

Full CSS with memory field. The system remembers where shells formed and uses that history to guide future evolution.

---

## Intent Tensor Theory Connection

This engine implements the visual layer of ITT:

| ITT Concept | Physics Analog |
|-------------|----------------|
| Φ (Potential) | Scalar field |
| ∇Φ (Gradient) | Direction of change |
| ∇²Φ (Laplacian) | Stability/collapse |
| ∂Φ/∂t | Evolution equation |

The **self-resolving** nature: given initial conditions, the future is mathematically determined. The system discovers what happens rather than being told.

---

## Technical Implementation

### Stack

- **Pure JavaScript** - No dependencies
- **HTML5 Canvas** - Field rendering
- **Float32Array** - Numerical precision
- **LZW Compression** - GIF encoding

### Field Resolution

Default: 1 pixel = 1 grid point

For performance with large canvases, the grid can be coarser than the pixel grid.

### Boundary Conditions

**Periodic:** Field wraps around edges. A wave exiting the right enters from the left.

---

## Links

- **Live App:** [https://render-executable-gif-physics.onrender.com](https://render-executable-gif-physics.onrender.com)
- **GitHub:** [https://github.com/FunnelFunction/0.0_Executable_GIF_Physics](https://github.com/FunnelFunction/0.0_Executable_GIF_Physics)
- **Dynamic GIF Command-Line:** [https://github.com/FunnelFunction/0.0_Dynamic_GIF_Command-Line](https://github.com/FunnelFunction/0.0_Dynamic_GIF_Command-Line)
- **FunnelFunction:** [https://funnelfunction.com](https://funnelfunction.com)
- **Intent Tensor Theory:** [https://intent-tensor-theory.com](https://intent-tensor-theory.com)

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

*The future unfurls from the present through mathematical necessity.*
