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
