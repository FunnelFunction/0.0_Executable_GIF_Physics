# Appendix B: Code Models

---

## B.1 Overview

This appendix provides implementation reference for the key algorithms. All code is in JavaScript, matching the browser-based implementation.

**Source**: Full implementation at [github.com/FunnelFunction/0.0_Executable_GIF_Physics](https://github.com/FunnelFunction/0.0_Executable_GIF_Physics)

---

## B.2 ScalarField Class

The core data structure:

```javascript
class ScalarField {
  constructor(nx, ny) {
    this.nx = nx;        // Width in pixels
    this.ny = ny;        // Height in pixels
    this.data = new Float32Array(nx * ny);  // Flat array storage
  }
  
  // Index conversion
  idx(i, j) {
    // Periodic boundary conditions
    const ii = ((i % this.nx) + this.nx) % this.nx;
    const jj = ((j % this.ny) + this.ny) % this.ny;
    return ii + jj * this.nx;
  }
  
  // Access
  get(i, j) { return this.data[this.idx(i, j)]; }
  set(i, j, v) { this.data[this.idx(i, j)] = v; }
  
  // Copy
  clone() {
    const copy = new ScalarField(this.nx, this.ny);
    copy.data.set(this.data);
    return copy;
  }
}
```

---

## B.3 Differential Operators

### Laplacian (5-Point Stencil)

```javascript
laplacian(i, j) {
  const center = this.get(i, j);
  const left   = this.get(i - 1, j);
  const right  = this.get(i + 1, j);
  const up     = this.get(i, j - 1);
  const down   = this.get(i, j + 1);
  
  // Assumes Δx = Δy = 1
  return left + right + up + down - 4 * center;
}
```

### Gradient Magnitude

```javascript
gradientMagnitude(i, j) {
  const dx = (this.get(i + 1, j) - this.get(i - 1, j)) / 2;
  const dy = (this.get(i, j + 1) - this.get(i, j - 1)) / 2;
  return Math.sqrt(dx * dx + dy * dy);
}
```

### Gradient Squared (|∇Φ|²)

```javascript
gradientSquared(i, j) {
  const dx = (this.get(i + 1, j) - this.get(i - 1, j)) / 2;
  const dy = (this.get(i, j + 1) - this.get(i, j - 1)) / 2;
  return dx * dx + dy * dy;
}
```

---

## B.4 CSS Evolution

The core evolution step:

```javascript
function cssStep(field, dt, params) {
  const { η, λ, μ, ν, α } = params;
  const newField = field.clone();
  
  for (let j = 0; j < field.ny; j++) {
    for (let i = 0; i < field.nx; i++) {
      const phi = field.get(i, j);
      
      // Diffusion
      const lap = field.laplacian(i, j);
      const diffusion = η * lap;
      
      // Steepening
      const gradSq = field.gradientSquared(i, j);
      const steepen = -λ * gradSq;
      
      // Bistable
      const bistable = μ * phi * (1 - phi * phi);
      
      // Decay
      const decay = -ν * phi;
      
      // Total rate of change
      const dPhidt = diffusion + steepen + bistable + decay;
      
      // Euler step
      let newPhi = phi + dt * dPhidt;
      
      // Clamp to prevent runaway
      newPhi = Math.max(-1.5, Math.min(1.5, newPhi));
      
      newField.set(i, j, newPhi);
    }
  }
  
  field.data.set(newField.data);
}
```

---

## B.5 Memory System

### Memory Update (Ω^)

```javascript
function updateMemory(omega, field, shellMask, gamma) {
  for (let k = 0; k < omega.data.length; k++) {
    const shell = shellMask[k] ? 1 : 0;
    const magnitude = Math.abs(field.data[k]);
    
    // Exponential moving average
    omega.data[k] = gamma * omega.data[k] + (1 - gamma) * shell * magnitude;
  }
}
```

### Shell Detection (ρq)

```javascript
function computeShellMask(field, threshold) {
  const mask = new Uint8Array(field.data.length);
  
  // Compute gradient statistics
  let sum = 0, sumSq = 0, count = 0;
  for (let j = 0; j < field.ny; j++) {
    for (let i = 0; i < field.nx; i++) {
      const grad = field.gradientMagnitude(i, j);
      sum += grad;
      sumSq += grad * grad;
      count++;
    }
  }
  
  const mean = sum / count;
  const variance = sumSq / count - mean * mean;
  const std = Math.sqrt(Math.max(0, variance));
  
  // Adaptive threshold
  const tau = mean + threshold * std;
  
  // Apply threshold
  for (let j = 0; j < field.ny; j++) {
    for (let i = 0; i < field.nx; i++) {
      const grad = field.gradientMagnitude(i, j);
      mask[i + j * field.nx] = grad > tau ? 1 : 0;
    }
  }
  
  return { mask, threshold: tau };
}
```

---

## B.6 CLA^ Classification

```javascript
const CLA_STATES = {
  VOID: 0, PASS: 1, AND: 2, OR: 3, NOT: 4, XOR: 5, LATCH: 6
};

function computeCLA(field, shellMask) {
  const cla = new Uint8Array(field.data.length);
  
  for (let j = 0; j < field.ny; j++) {
    for (let i = 0; i < field.nx; i++) {
      const k = i + j * field.nx;
      
      // Count shells in 3×3 neighborhood
      let shellCount = 0;
      for (let dj = -1; dj <= 1; dj++) {
        for (let di = -1; di <= 1; di++) {
          const ni = (i + di + field.nx) % field.nx;
          const nj = (j + dj + field.ny) % field.ny;
          if (shellMask[ni + nj * field.nx]) shellCount++;
        }
      }
      
      // Basic classification
      if (shellCount === 0) {
        cla[k] = CLA_STATES.VOID;
      } else if (shellCount >= 7) {
        cla[k] = CLA_STATES.LATCH;
      } else if (shellCount >= 3) {
        // Would analyze gradients for AND/OR/XOR
        cla[k] = CLA_STATES.AND;  // Simplified
      } else {
        cla[k] = CLA_STATES.PASS;
      }
    }
  }
  
  return cla;
}
```

---

## B.7 Loss Functions

### L1 Loss

```javascript
function l1Loss(field, target) {
  let sum = 0;
  for (let k = 0; k < field.data.length; k++) {
    sum += Math.abs(field.data[k] - target.data[k]);
  }
  return sum;
}
```

### Dirichlet Energy

```javascript
function dirichletEnergy(field) {
  let energy = 0;
  for (let j = 0; j < field.ny; j++) {
    for (let i = 0; i < field.nx; i++) {
      energy += field.gradientSquared(i, j);
    }
  }
  return energy;
}
```

### 2D DFT (Naive)

```javascript
function dft2d(field) {
  const N = field.nx, M = field.ny;
  const magnitude = new Float32Array(N * M);
  
  for (let v = 0; v < M; v++) {
    for (let u = 0; u < N; u++) {
      let sumReal = 0, sumImag = 0;
      
      for (let y = 0; y < M; y++) {
        for (let x = 0; x < N; x++) {
          const phi = field.get(x, y);
          const angle = -2 * Math.PI * (u * x / N + v * y / M);
          sumReal += phi * Math.cos(angle);
          sumImag += phi * Math.sin(angle);
        }
      }
      
      magnitude[u + v * N] = Math.sqrt(sumReal * sumReal + sumImag * sumImag);
    }
  }
  
  return magnitude;
}
```

### Spectral Loss

```javascript
function spectralLoss(field, target) {
  const specField = dft2d(field);
  const specTarget = dft2d(target);
  
  let sumSq = 0;
  for (let k = 0; k < specField.length; k++) {
    const diff = specField[k] - specTarget[k];
    sumSq += diff * diff;
  }
  
  return Math.sqrt(sumSq);
}
```

### Combined Loss

```javascript
function combinedLoss(field, target, beta, lambda) {
  const spatial = l1Loss(field, target);
  const spectral = spectralLoss(field, target);
  const energy = dirichletEnergy(field);
  
  // Normalize spectral to similar scale
  const spectralNorm = spectral / Math.sqrt(field.data.length);
  
  const total = (1 - beta) * spatial + beta * spectralNorm + lambda * energy;
  
  return { total, spatial, spectral, spectralNorm, energy };
}
```

---

## B.8 Learning Update

```javascript
function learningStep(field, target, lr, dt) {
  for (let k = 0; k < field.data.length; k++) {
    const error = field.data[k] - target.data[k];
    field.data[k] -= lr * error * dt;
  }
}
```

---

## B.9 Swift-Hohenberg Evolution

```javascript
function swiftHohenbergStep(field, dt, r, g) {
  const newField = field.clone();
  
  for (let j = 0; j < field.ny; j++) {
    for (let i = 0; i < field.nx; i++) {
      const phi = field.get(i, j);
      const lap = field.laplacian(i, j);
      const biharmonic = field.biharmonic(i, j);  // Need to implement
      
      // ∂Φ/∂t = rΦ - (1 + ∇²)²Φ - gΦ³
      // (1 + ∇²)²Φ = Φ + 2∇²Φ + ∇⁴Φ
      const linearOp = phi + 2 * lap + biharmonic;
      const dPhidt = r * phi - linearOp - g * phi * phi * phi;
      
      newField.set(i, j, phi + dt * dPhidt);
    }
  }
  
  field.data.set(newField.data);
}

// Biharmonic (9-point stencil approximation)
biharmonic(i, j) {
  // ∇⁴Φ = ∇²(∇²Φ)
  const lap_center = this.laplacian(i, j);
  const lap_left   = this.laplacian(i - 1, j);
  const lap_right  = this.laplacian(i + 1, j);
  const lap_up     = this.laplacian(i, j - 1);
  const lap_down   = this.laplacian(i, j + 1);
  
  return lap_left + lap_right + lap_up + lap_down - 4 * lap_center;
}
```

---

## B.10 Symbol Emission

```javascript
function emitSymbols(field, prevField, omega, shellMask, cla, thresholds) {
  const symbols = [];
  
  // Shell formation
  const shellCount = shellMask.reduce((a, b) => a + b, 0);
  const prevShellCount = thresholds.prevShellCount || 0;
  if (shellCount > prevShellCount * 1.1) {
    symbols.push('ρ');
  }
  thresholds.prevShellCount = shellCount;
  
  // Memory peak
  const maxOmega = Math.max(...omega.data);
  if (maxOmega > thresholds.memoryThreshold && 
      !thresholds.memoryPeaked) {
    symbols.push('Ω');
    thresholds.memoryPeaked = true;
  }
  
  // Logic gates
  const claHist = new Array(7).fill(0);
  for (const state of cla) claHist[state]++;
  
  if (claHist[CLA_STATES.AND] > thresholds.gateThreshold) {
    symbols.push('∧');
  }
  if (claHist[CLA_STATES.OR] > thresholds.gateThreshold) {
    symbols.push('∨');
  }
  if (claHist[CLA_STATES.LATCH] > thresholds.latchThreshold) {
    symbols.push('λ');
  }
  
  // Global dynamics
  const meanAbs = field.data.reduce((a, b) => a + Math.abs(b), 0) / field.data.length;
  const prevMeanAbs = prevField.data.reduce((a, b) => a + Math.abs(b), 0) / prevField.data.length;
  
  if (meanAbs < prevMeanAbs * 0.9) {
    symbols.push('↓');
  }
  if (meanAbs > prevMeanAbs * 1.1) {
    symbols.push('↑');
  }
  
  return symbols;
}
```

---

## B.11 Initial Condition Generators

```javascript
const INITIAL_CONDITIONS = {
  noise: (field, params) => {
    const A = params.A ?? 0.5;
    const seed = params.seed ?? Date.now();
    const rng = seededRandom(seed);
    
    for (let k = 0; k < field.data.length; k++) {
      field.data[k] = A * (2 * rng() - 1);
    }
  },
  
  bistable: (field, params) => {
    const ε = params.ε ?? 0.3;
    const seed = params.seed ?? Date.now();
    const rng = seededRandom(seed);
    
    for (let k = 0; k < field.data.length; k++) {
      field.data[k] = ε * (2 * rng() - 1);
    }
  },
  
  disk: (field, params) => {
    const r = params.r ?? 50;
    const cx = params.x ?? field.nx / 2;
    const cy = params.y ?? field.ny / 2;
    const sharpness = params.sharpness ?? 5;
    
    for (let j = 0; j < field.ny; j++) {
      for (let i = 0; i < field.nx; i++) {
        const dist = Math.sqrt((i - cx) ** 2 + (j - cy) ** 2);
        field.set(i, j, -Math.tanh(sharpness * (dist - r)));
      }
    }
  },
  
  checkerboard: (field, params) => {
    const size = params.size ?? 20;
    
    for (let j = 0; j < field.ny; j++) {
      for (let i = 0; i < field.nx; i++) {
        const xi = Math.floor(i / size);
        const yi = Math.floor(j / size);
        field.set(i, j, ((xi + yi) % 2 === 0) ? 1 : -1);
      }
    }
  },
  
  domains: (field, params) => {
    const n = params.n ?? 3;
    const dir = params.dir ?? 'vertical';
    const noise = params.noise ?? 0;
    const rng = seededRandom(params.seed ?? Date.now());
    
    for (let j = 0; j < field.ny; j++) {
      for (let i = 0; i < field.nx; i++) {
        const coord = (dir === 'vertical') ? i : j;
        const size = (dir === 'vertical') ? field.nx : field.ny;
        const domainIdx = Math.floor(coord / (size / n));
        const base = (domainIdx % 2 === 0) ? 1 : -1;
        field.set(i, j, base + noise * (2 * rng() - 1));
      }
    }
  }
};
```

---

## B.12 Simulation Runner

```javascript
async function runSimulation(spec) {
  const { nx, ny } = spec.canvas;
  const { start, end, dt } = spec.time;
  
  // Initialize
  const field = new ScalarField(nx, ny);
  INITIAL_CONDITIONS[spec.initial.type](field, spec.initial.params);
  
  const target = spec.target ? new ScalarField(nx, ny) : null;
  if (target) {
    INITIAL_CONDITIONS[spec.target.type](target, spec.target.params);
  }
  
  const omega = new ScalarField(nx, ny);
  const frames = [];
  const log = [];
  
  // Evolution loop
  for (let t = start; t <= end; t += dt) {
    // Compute derived quantities
    const { mask: shellMask } = computeShellMask(field, 1.0);
    const cla = computeCLA(field, shellMask);
    
    // Log state
    const minPhi = Math.min(...field.data);
    const maxPhi = Math.max(...field.data);
    let logLine = `t = ${t.toFixed(4)}: Φ ∈ [${minPhi.toFixed(3)}, ${maxPhi.toFixed(3)}]`;
    
    if (target) {
      const loss = combinedLoss(field, target, spec.beta ?? 0.3, spec.lambda ?? 0.01);
      logLine += ` | L = ${loss.total.toFixed(2)}`;
    }
    
    log.push(logLine);
    
    // Store frame
    frames.push(field.clone());
    
    // Evolve
    EVOLUTION_OPERATORS[spec.evolution.type](field, dt, spec.evolution.params, t, null, target);
    
    // Update memory
    updateMemory(omega, field, shellMask, 0.95);
  }
  
  return { frames, log };
}
```

---

## B.13 FFT Implementation (Optimized)

For production, use this Cooley-Tukey FFT:

```javascript
// Radix-2 FFT (requires N to be power of 2)
function fft(real, imag) {
  const N = real.length;
  
  // Bit-reversal permutation
  for (let i = 0, j = 0; i < N; i++) {
    if (j > i) {
      [real[i], real[j]] = [real[j], real[i]];
      [imag[i], imag[j]] = [imag[j], imag[i]];
    }
    let m = N >> 1;
    while (m >= 1 && j >= m) {
      j -= m;
      m >>= 1;
    }
    j += m;
  }
  
  // Cooley-Tukey
  for (let size = 2; size <= N; size <<= 1) {
    const halfSize = size >> 1;
    const step = Math.PI / halfSize;
    
    for (let i = 0; i < N; i += size) {
      for (let j = 0, angle = 0; j < halfSize; j++, angle += step) {
        const cos = Math.cos(angle);
        const sin = -Math.sin(angle);
        
        const k = i + j;
        const l = k + halfSize;
        
        const tr = real[l] * cos - imag[l] * sin;
        const ti = real[l] * sin + imag[l] * cos;
        
        real[l] = real[k] - tr;
        imag[l] = imag[k] - ti;
        real[k] += tr;
        imag[k] += ti;
      }
    }
  }
}

// 2D FFT via row-column decomposition
function fft2d(field) {
  // Assumes field.nx and field.ny are powers of 2
  const N = field.nx, M = field.ny;
  const real = new Float32Array(N * M);
  const imag = new Float32Array(N * M);
  
  // Copy field to real part
  real.set(field.data);
  
  // FFT rows
  for (let j = 0; j < M; j++) {
    const rowReal = real.subarray(j * N, (j + 1) * N);
    const rowImag = imag.subarray(j * N, (j + 1) * N);
    fft(rowReal, rowImag);
  }
  
  // FFT columns
  const colReal = new Float32Array(M);
  const colImag = new Float32Array(M);
  
  for (let i = 0; i < N; i++) {
    for (let j = 0; j < M; j++) {
      colReal[j] = real[i + j * N];
      colImag[j] = imag[i + j * N];
    }
    fft(colReal, colImag);
    for (let j = 0; j < M; j++) {
      real[i + j * N] = colReal[j];
      imag[i + j * N] = colImag[j];
    }
  }
  
  // Return magnitude
  const magnitude = new Float32Array(N * M);
  for (let k = 0; k < N * M; k++) {
    magnitude[k] = Math.sqrt(real[k] * real[k] + imag[k] * imag[k]);
  }
  
  return magnitude;
}
```

---

## B.14 Usage Examples

### Basic CSS Evolution

```javascript
// Create field
const field = new ScalarField(200, 200);
INITIAL_CONDITIONS.bistable(field, { ε: 0.3, seed: 42 });

// Evolve for 100 steps
for (let t = 0; t < 100; t++) {
  cssStep(field, 0.5, { η: 0.8, λ: 0.1, μ: 0.5, ν: 0 });
}
```

### Learning Toward Target

```javascript
const field = new ScalarField(100, 100);
INITIAL_CONDITIONS.noise(field, { A: 0.3 });

const target = new ScalarField(100, 100);
INITIAL_CONDITIONS.disk(target, { r: 30 });

for (let t = 0; t < 50; t++) {
  cssStep(field, 0.5, { η: 0.5, μ: 0.3 });
  learningStep(field, target, 0.1, 0.5);
  
  const loss = l1Loss(field, target);
  console.log(`t=${t}: L=${loss.toFixed(2)}`);
}
```

### Spectral Learning (Checkerboard)

```javascript
const field = new ScalarField(64, 64);  // Power of 2 for FFT
INITIAL_CONDITIONS.bistable(field, { ε: 0.4 });

const target = new ScalarField(64, 64);
INITIAL_CONDITIONS.checkerboard(target, { size: 16 });

for (let t = 0; t < 100; t++) {
  swiftHohenbergStep(field, 0.5, 0.2, 1.0);
  learningStep(field, target, 0.15, 0.5);
  
  const loss = combinedLoss(field, target, 0.6, 0.01);
  console.log(`t=${t}: L=${loss.total.toFixed(2)}`);
}
```

---

*← [Appendix A: Notation & Units](Appendix_A_Notation.md) | [Back to Contents](README.md) →*
