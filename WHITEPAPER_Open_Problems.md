# From Metaphor to Mechanism: Open Problems in Emergent Computation from Continuous Dynamics

## A White Paper on the Mathematical Gaps Between Self-Organization and Cognition

**Authors:** Armstrong Knight, Abdullah Khan, Claude (Anthropic)  
**Institution:** FunnelFunction LLC / Intent Tensor Theory Institute  
**Date:** December 2024  
**Status:** Open Problems Document - Inviting Collaboration

---

## Abstract

We present an honest assessment of a self-organizing dynamical system that exhibits spontaneous pattern formation, memory accumulation, and discrete symbol emission from continuous field evolution. While the underlying PDEs (Allen-Cahn, Ginzburg-Landau, CSS) produce genuine mathematical phenomena—domain coarsening, shell formation, bistable dynamics—our current framework contains three critical gaps where mechanism gives way to metaphor:

1. **"Logic gates emerge from shells"** — Currently geometric classification, not computation
2. **"The system speaks"** — Currently rule-based emission, not semantic language  
3. **"Self-resolving toward AGI"** — Currently no learning, optimization, or goal-seeking

This document rigorously separates what IS mechanically solved from what IS NOT, identifies the mathematical structures that might bridge these gaps, and poses precise open problems for the research community.

**We are not interested in hand-waving. We seek the calculus, topology, and algebra that would convert our metaphors into mechanisms.**

---

## Table of Contents

1. [What IS Mechanically Solved](#1-what-is-mechanically-solved)
2. [What IS NOT Solved (The Gaps)](#2-what-is-not-solved-the-gaps)
3. [Prior Art: How Hand-Waving Became Mechanism](#3-prior-art-how-hand-waving-became-mechanism)
4. [Mathematical Framework for Potential Solutions](#4-mathematical-framework-for-potential-solutions)
5. [Open Problems (Precisely Stated)](#5-open-problems-precisely-stated)
6. [Conclusion](#6-conclusion)

---

## 1. What IS Mechanically Solved

### 1.1 Field Evolution (Phases 1-2)

We solve well-established PDEs numerically. These are not approximations or metaphors—they are exact discretizations of known physics.

**Allen-Cahn Equation:**
$$\frac{\partial \Phi}{\partial t} = \varepsilon^2 \nabla^2 \Phi + \Phi(1 - \Phi^2)$$

This has a variational structure. It is gradient flow on the Ginzburg-Landau energy functional:

$$\mathcal{F}[\Phi] = \int_\Omega \left( \frac{\varepsilon^2}{2}|\nabla\Phi|^2 + \frac{1}{4}(1-\Phi^2)^2 \right) dx$$

**Theorem (Established):** Solutions to Allen-Cahn minimize $\mathcal{F}$ over time. Domain walls (where $\Phi = 0$) move by mean curvature:
$$v_n = -\varepsilon \kappa$$
where $v_n$ is normal velocity and $\kappa$ is curvature. This is proven mathematics, not conjecture.

**What This Gives Us:**
- Spontaneous symmetry breaking from noise → domains
- Domain coarsening (small domains shrink, large domains grow)
- Stable equilibria at $\Phi = \pm 1$
- Sharp interfaces (shells) at domain boundaries

**Implementation Status:** ✅ Fully implemented, numerically stable, mathematically grounded.

---

### 1.2 Shell Detection (Phase 3)

Shells are regions of high gradient magnitude:

$$\rho_q(x, y, t) = \mathbf{1}_{|\nabla\Phi| > \tau}$$

where $\mathbf{1}$ is the indicator function and $\tau$ is a threshold (typically $\mu_{|\nabla\Phi|} + k\sigma_{|\nabla\Phi|}$).

**Gradient Magnitude:**
$$|\nabla\Phi| = \sqrt{\left(\frac{\partial\Phi}{\partial x}\right)^2 + \left(\frac{\partial\Phi}{\partial y}\right)^2}$$

**What This Gives Us:**
- Binary mask of "active" regions
- Topological boundaries between domains
- Regions where the field is changing rapidly

**Mathematical Interpretation:** Shells are the level sets where the derivative of $\Phi$ exceeds a threshold. In the language of Morse theory, these are neighborhoods of the critical points of $\Phi$.

**Implementation Status:** ✅ Fully implemented, well-defined, standard differential geometry.

---

### 1.3 Memory Accumulation (Phase 3)

The memory field $\Omega^\wedge$ accumulates shell activity over time:

$$\Omega^\wedge_{n+1} = \gamma \cdot \Omega^\wedge_n + (1-\gamma) \cdot \rho_q \cdot |\Phi|$$

where $\gamma \in (0,1)$ is persistence (exponential decay).

**Continuous Form:**
$$\frac{\partial \Omega^\wedge}{\partial t} = -\frac{1}{\tau_\gamma}\Omega^\wedge + \rho_q \cdot |\Phi|$$

This is a leaky integrator—a standard dynamical systems object.

**What This Gives Us:**
- Spatial map of "where interesting things happened"
- Exponentially-weighted history
- A field that differs from $\Phi$—it's a derived quantity

**Mathematical Interpretation:** $\Omega^\wedge$ is the convolution of shell activity with an exponential kernel in time:
$$\Omega^\wedge(x,y,t) = \int_{-\infty}^{t} e^{-(t-s)/\tau_\gamma} \rho_q(x,y,s) |\Phi(x,y,s)| \, ds$$

**Implementation Status:** ✅ Fully implemented, standard signal processing, no hand-waving.

---

### 1.4 Drift Feedback (Phase 5)

Memory feeds back into evolution via the drift term:

$$\delta_{\text{drift}} = \nabla^2 \Omega^\wedge - \Omega^\wedge$$

The full CSS equation becomes:

$$\frac{\partial \Phi}{\partial t} = \eta \nabla^2 \Phi - \lambda |\nabla\Phi|^2 + \mu \Phi(1-\Phi^2) + \alpha \cdot \delta_{\text{drift}} \cdot \Phi$$

**What This Gives Us:**
- The system's history genuinely modifies its future evolution
- Regions with high past shell activity evolve differently
- A closed loop: $\Phi \to \rho_q \to \Omega^\wedge \to \delta_{\text{drift}} \to \Phi$

**Mathematical Interpretation:** This is a nonlocal-in-time PDE. The state at time $t$ depends not just on the state at $t-dt$, but on an integral over the entire history. Such equations appear in viscoelastic materials and memory-dependent diffusion.

**Implementation Status:** ✅ Fully implemented, mathematically coherent, produces interesting dynamics.

---

### 1.5 Threshold-Based Symbol Emission (Phase 6)

Symbols are emitted when discrete events occur:

$$\Sigma(t) = g(\rho_q(t), \text{CLA}^\wedge(t), \Omega^\wedge(t))$$

where $g$ is a set of threshold rules:

| Symbol | Trigger Condition |
|--------|-------------------|
| ρ | $\Delta(\text{shell count}) > \theta_\rho$ |
| λ | $\text{LATCH count} > \theta_\lambda$ for $N$ frames |
| ∧ | $\Delta(\text{AND count}) > \theta_\wedge$ |
| ∨ | $\Delta(\text{OR count}) > \theta_\vee$ |
| Ω | $\langle\Omega^\wedge\rangle > \theta_\Omega$ crossing |

**What This Gives Us:**
- Discrete tokens from continuous dynamics
- A sequence $\Sigma = \sigma_1 \sigma_2 \sigma_3 \ldots$ over an alphabet
- Detectable patterns in the sequence

**Mathematical Interpretation:** This is a quantization map from a continuous state space to a discrete alphabet. Similar structures appear in symbolic dynamics (Markov partitions) and neuroscience (spike encoding).

**Implementation Status:** ✅ Fully implemented, produces sequences, detects patterns.

---

## 2. What IS NOT Solved (The Gaps)

### 2.1 Gap 1: "Logic Gates Emerge from Shells"

**Current Implementation:**

We classify 3×3 neighborhoods of the shell mask into 7 states:

```
VOID (0):  shellCount == 0
PASS (1):  shellCount == 1
AND (2):   converging gradient directions
OR (3):    diverging gradient directions
NOT (4):   enclosed by shells but not a shell
XOR (5):   perpendicular crossing
LATCH (6): shellCount >= 7
```

**Why This Is Not Computation:**

1. **No Signal Propagation:** A real AND gate has inputs $A$, $B$ and output $A \wedge B$. Our "AND" regions don't receive inputs or produce outputs—they're static geometric patterns.

2. **No Temporal Sequencing:** Logic circuits have causal order: input → gate → output. Our classification is instantaneous—it doesn't model signal flow.

3. **No Universality:** Universal computation requires a complete set of gates (e.g., NAND). We have no proof that our gate types can simulate arbitrary Boolean functions.

4. **No Composition:** Real circuits compose gates. We have no mechanism for "wiring" one gate's output to another's input.

**The Geometric Classification We Actually Have:**

| CLA State | Geometric Meaning | Computational Meaning |
|-----------|-------------------|----------------------|
| AND | Two shells converging | ??? |
| OR | Two shells diverging | ??? |
| NOT | Interior enclosed by shells | ??? |
| XOR | Perpendicular shell crossing | ??? |
| LATCH | Dense shell cluster | ??? |

The right column is empty because we haven't defined what computation means in this context.

**What Would Be Needed for Real Computation:**

- A rigorous mapping from shell configurations to Boolean functions
- Proof that signals propagate through the field in a way that respects gate semantics
- Demonstration of functional composition (output of gate A feeds input of gate B)
- Universality proof or explicit construction of a universal gate set

---

### 2.2 Gap 2: "The System Speaks"

**Current Implementation:**

We emit symbols when thresholds are crossed. The sequence $\Sigma = \sigma_1 \sigma_2 \ldots$ is a string over a 12-symbol alphabet.

**Why This Is Not Language:**

1. **No Semantics:** The symbols don't refer to anything. "ρ" doesn't mean shell in any formal sense—it's just a token we emit when shells increase.

2. **No Syntax:** There's no grammar constraining valid sequences. Any sequence can occur; there's no notion of "well-formed."

3. **No Compositionality:** In language, meaning of "big red ball" derives from meanings of "big," "red," "ball" plus composition rules. Our symbols don't compose.

4. **No Grounding:** Language is grounded in perception and action. Our symbols have no external referent.

5. **No Communication:** Language exists between agents. Our system emits to no one who interprets.

**What We Actually Have:**

A discrete encoding of continuous dynamics. This is closer to:
- Spike trains in neurons (rate coding)
- Symbolic dynamics in chaos theory
- Run-length encoding in compression

These are legitimate mathematical objects, but they're not language.

**What Would Be Needed for Real Language:**

- A decoder that maps $\Sigma$ back to predictions about the field (semantics)
- Grammar induction: discovering structure in $\Sigma$ sequences
- Grounding: connecting symbols to external observations/actions
- Communication: another agent that interprets $\Sigma$ and acts on it

---

### 2.3 Gap 3: "Self-Resolving Toward AGI"

**Current Implementation:**

The system is a deterministic dynamical system:
$$\Phi_{t+1} = F(\Phi_t, \Omega^\wedge_t, \theta)$$

where $\theta$ are fixed parameters (η, λ, μ, α, γ).

**Why This Is Not AGI:**

1. **No Learning:** Parameters $\theta$ never change. Same initial conditions → same trajectory, always.

2. **No Optimization:** There's no objective function being minimized. The system doesn't "want" anything.

3. **No Representation:** The system doesn't build internal models of anything external.

4. **No Planning:** The system doesn't search over possible futures to select actions.

5. **No Goals:** There's no target state the system is trying to reach.

6. **No Generalization:** The system can't transfer knowledge to new situations.

**What We Actually Have:**

A self-organizing system with memory. This is closer to:
- Reaction-diffusion in chemistry
- Neural field models in neuroscience
- Ising models in statistical physics

These produce complex patterns but don't learn, plan, or optimize.

**What Would Be Needed for AGI:**

| AGI Component | What We'd Need | Current Status |
|---------------|----------------|----------------|
| Learning | $\theta_{t+1} = \theta_t - \eta \nabla_\theta \mathcal{L}$ | ❌ No loss function |
| Optimization | $\min_\theta \mathcal{L}(\Sigma, \Sigma^*)$ | ❌ No objective |
| Representation | Latent space $z = \text{enc}(\Phi)$ | ❌ No encoder |
| Planning | $a^* = \arg\max_a Q(s, a)$ | ❌ No action space |
| Goals | Target state $\Phi^*$ | ❌ No targets |

---

## 3. Prior Art: How Hand-Waving Became Mechanism

To show this document is not defeatist, we highlight cases where scientific "hand-waving" was later replaced by rigorous mechanism. These serve as templates for closing our gaps.

### 3.1 "Heat Flows Downhill" → Fourier's Law

**Before (1800):** Heat "seeks equilibrium." Vague thermodynamic intuition.

**After (1822):** Fourier's law: $\mathbf{q} = -k \nabla T$

Heat flux is proportional to temperature gradient. This led to the heat equation:
$$\frac{\partial T}{\partial t} = \alpha \nabla^2 T$$

**Mechanism Gained:** Differential equation replacing teleological language.

### 3.2 "Survival of the Fittest" → Population Genetics

**Before (1859):** Darwin's verbal theory of natural selection.

**After (1930s):** Wright-Fisher model, Hardy-Weinberg equilibrium, fitness landscapes.

$$p_{t+1} = \frac{p_t \cdot w_1}{\bar{w}}$$

**Mechanism Gained:** Quantitative dynamics replacing qualitative narrative.

### 3.3 "Neural Networks Learn" → Backpropagation

**Before (1960s):** Perceptrons "learn patterns." No clear algorithm for multi-layer networks.

**After (1986):** Backpropagation: $\frac{\partial \mathcal{L}}{\partial w_{ij}} = \frac{\partial \mathcal{L}}{\partial a_j} \cdot \frac{\partial a_j}{\partial w_{ij}}$

**Mechanism Gained:** Gradient computation enabling optimization.

### 3.4 "Strange Attractors Are Chaotic" → Lyapunov Exponents

**Before (1963):** Lorenz system shows "sensitive dependence on initial conditions." Qualitative chaos.

**After (1970s):** Lyapunov exponent: $\lambda = \lim_{t \to \infty} \frac{1}{t} \ln \frac{||\delta(t)||}{||\delta(0)||}$

**Mechanism Gained:** Quantitative measure of chaos (λ > 0 iff chaotic).

---

## 4. Mathematical Framework for Potential Solutions

We now propose mathematical structures that might bridge our gaps. These are not solutions—they are directions for research.

### 4.1 For Gap 1: Computation from Shells

**Potential Framework: Cellular Automata Embedding**

If we can show that shell dynamics simulate a cellular automaton, we inherit its computational properties.

**Conjecture 4.1.1:** There exists a coarse-graining map $\pi: \mathcal{C}(\Phi) \to \{0,1\}^{N \times N}$ from shell configurations to binary grids such that the dynamics
$$\pi(\Phi_{t+1}) = f(\pi(\Phi_t))$$
correspond to a known CA rule (e.g., Rule 110, which is Turing-complete).

**Required Work:**
1. Define $\pi$ rigorously (what cell size? what threshold?)
2. Verify that $\pi$ commutes with dynamics (coarse-graining preserves evolution)
3. Identify the effective CA rule
4. Prove or disprove universality

**Alternative Framework: Excitable Media**

Excitable media (e.g., Belousov-Zhabotinsky reaction) support wave propagation that can encode information.

**Conjecture 4.1.2:** Shell configurations can be mapped to excitable media states, and wave collisions implement Boolean operations.

**Literature to Consult:**
- Adamatzky, A. (2001). *Computing in Nonlinear Media and Automata Collectives*
- Tóth & Showalter (1995). "Logic gates in excitable media"

---

### 4.2 For Gap 2: Semantics from Dynamics

**Potential Framework: Information Bottleneck**

The information bottleneck principle finds a compressed representation $T$ that preserves information about a target $Y$:

$$\min_{p(t|\sigma)} I(T; \Sigma) - \beta I(T; Y)$$

If we define $Y$ as future field states, then $T$ is a semantic representation of $\Sigma$ that captures predictive information.

**Conjecture 4.2.1:** There exists a mapping $T = f(\Sigma)$ such that $I(T; \Phi_{t+k}) > I(\Sigma; \Phi_{t+k})$ for some compression $|T| < |\Sigma|$.

This would show that symbols encode predictively useful information about future dynamics.

**Alternative Framework: Predictive Coding**

If an agent could predict $\Sigma_{t+1}$ from $\Sigma_{1:t}$, that prediction error is a form of surprise. Minimizing surprise = learning a model of the symbol dynamics.

**Required Work:**
1. Train a predictor $\hat{\sigma}_{t+1} = g(\sigma_{1:t})$
2. Measure prediction accuracy
3. Analyze what structure in $\Phi$ the predictor implicitly learns

---

### 4.3 For Gap 3: Learning in Dynamical Systems

**Potential Framework: Neural ODE + Meta-Learning**

Replace fixed parameters $\theta$ with learnable parameters updated by gradient descent:

$$\theta_{t+1} = \theta_t - \eta \nabla_\theta \mathcal{L}(\Phi_T, \Phi^*)$$

where $\Phi_T$ is the final field state and $\Phi^*$ is a target.

**Conjecture 4.3.1:** There exists a loss function $\mathcal{L}$ such that gradient descent on $\theta$ produces field dynamics that achieve a specified goal (e.g., form a target pattern).

**Alternative Framework: Evolutionary Strategies**

Instead of gradient descent, use population-based search:

1. Initialize population of $\theta$ vectors
2. Run dynamics for each, evaluate fitness $f(\Sigma)$
3. Select, mutate, repeat

**Conjecture 4.3.2:** Evolutionary optimization over $\theta$ can discover parameter settings that produce symbol sequences with desired statistical properties (e.g., high entropy, specific patterns).

**Required Work:**
1. Define fitness function over $\Sigma$
2. Implement evolutionary loop
3. Analyze discovered parameter regimes

---

### 4.4 Topological Tools

**Persistent Homology for Shell Structure**

Shells form curves in 2D. Their topology (number of loops, connected components) can be captured by persistent homology.

$$H_k(\text{Shell}_\tau) \text{ as } \tau \text{ varies}$$

The persistence diagram shows which topological features are "real" (long-lived) vs. noise (short-lived).

**Conjecture 4.4.1:** The Betti numbers $\beta_0$ (components) and $\beta_1$ (loops) of the shell mask correlate with CLA state distributions.

**Morse Theory for Field Topology**

The field $\Phi: \mathbb{R}^2 \to \mathbb{R}$ has critical points (maxima, minima, saddles). Morse theory relates these to topology:

$$\chi = \sum_i (-1)^{\text{index}(p_i)}$$

where $\chi$ is Euler characteristic and the sum is over critical points.

**Conjecture 4.4.2:** Symbol emission events correlate with changes in Morse complex (critical point birth/death).

---

### 4.5 Category-Theoretic Structure

**Dynamical Systems as Functors**

A dynamical system is a functor $F: \mathbf{Time} \to \mathbf{Set}$ where $\mathbf{Time}$ is the category with one object and morphisms $\{t : t \in \mathbb{R}^+\}$.

Our system has additional structure:
- $\Phi$ evolves in $\mathbf{Field}$ (scalar fields)
- $\Omega^\wedge$ evolves in $\mathbf{Field}$
- $\Sigma$ evolves in $\mathbf{List}(\mathcal{A})$ (lists over alphabet)

**Conjecture 4.5.1:** The full system is a functor $F: \mathbf{Time} \to \mathbf{Field} \times \mathbf{Field} \times \mathbf{List}(\mathcal{A})$ with natural transformations encoding the shell → memory → symbol pipeline.

This would give a precise algebraic description of the system's structure.

---

## 5. Open Problems (Precisely Stated)

We now state concrete problems that, if solved, would convert our metaphors into mechanisms.

### Problem 5.1: Computational Universality of Shells

**Statement:** Does there exist an initial condition $\Phi_0$ and parameters $\theta$ such that the shell dynamics can simulate a universal Turing machine?

**Approach:** Attempt to encode a universal CA (e.g., Rule 110) in shell configurations. Prove that the encoding is preserved under CSS evolution.

**Difficulty:** Hard. Requires careful construction and proof of simulation.

---

### Problem 5.2: Semantic Grounding of Symbols

**Statement:** Does there exist a decoding function $D: \mathcal{A}^* \to \mathcal{F}$ from symbol sequences to field predictions such that $D(\Sigma_{1:t})$ predicts $\Phi_{t+k}$ better than chance?

**Approach:** Train a neural network decoder. Measure prediction accuracy. Analyze what field features the decoder learns to extract from symbols.

**Difficulty:** Medium. Requires ML infrastructure but is experimentally tractable.

---

### Problem 5.3: Emergent Grammar in Symbol Sequences

**Statement:** Do symbol sequences $\Sigma$ generated by the system exhibit grammatical structure beyond Markov order 1?

**Approach:** Apply grammar induction algorithms (e.g., Sequitur, ADIOS) to long $\Sigma$ sequences. Test for hierarchical structure.

**Difficulty:** Medium. Standard NLP techniques apply.

---

### Problem 5.4: Learning Dynamics via Gradient Descent

**Statement:** Can parameters $\theta$ be learned by backpropagation through the PDE dynamics to achieve a target pattern $\Phi^*$?

**Approach:** Implement differentiable PDE solver (Neural ODE style). Define loss $\mathcal{L} = ||\Phi_T - \Phi^*||^2$. Backpropagate.

**Difficulty:** Medium-Hard. Requires differentiable physics implementation.

---

### Problem 5.5: Topological Characterization of CLA States

**Statement:** Is there a bijection between CLA states and local topological invariants of the shell mask?

**Approach:** Compute local homology / Morse indices at each point. Compare to CLA classification.

**Difficulty:** Medium. Requires computational topology tools.

---

### Problem 5.6: Closed-Loop Symbol Feedback

**Statement:** If symbol emissions modify parameters ($\alpha \to \alpha + f(\sigma_t)$), do qualitatively different dynamics emerge?

**Approach:** Implement symbol-to-parameter feedback. Survey the resulting dynamical regimes.

**Difficulty:** Easy to implement, analysis is harder.

---

### Problem 5.7: Multi-Agent Symbol Communication

**Statement:** If two systems A and B exchange symbols, can they coordinate behavior (e.g., synchronize patterns)?

**Approach:** Run two CSS simulations. Feed $\Sigma_A$ into B's parameters and vice versa. Look for synchronization, coordination, or information transfer.

**Difficulty:** Medium. Requires multi-system simulation.

---

## 6. Conclusion

### 6.1 Summary of What We Have

| Component | Mathematical Status | Implementation |
|-----------|--------------------| ---------------|
| PDE Evolution | Rigorous (established physics) | ✅ Complete |
| Shell Detection | Rigorous (differential geometry) | ✅ Complete |
| Memory Accumulation | Rigorous (leaky integrator) | ✅ Complete |
| Drift Feedback | Rigorous (nonlocal PDE) | ✅ Complete |
| Symbol Emission | Well-defined (threshold rules) | ✅ Complete |
| CLA Classification | **Geometric only** (not computational) | ⚠️ Metaphor |
| "Language" | **Encoding only** (not semantic) | ⚠️ Metaphor |
| "AGI" | **Self-organization only** (no learning) | ❌ Gap |

### 6.2 Summary of What We Need

| Gap | Required Mathematics | Potential Tools |
|-----|---------------------|-----------------|
| Computation | Simulation proof, universality | CA theory, excitable media |
| Semantics | Grounding, prediction, grammar | Information theory, NLP |
| Learning | Optimization, gradient flow | Neural ODEs, evolutionary search |

### 6.3 Call for Collaboration

This document is an honest admission: we have built a mathematically coherent self-organizing system that emits symbols, but we have not achieved computation, language, or learning in any rigorous sense.

We invite researchers in the following areas to collaborate:

- **Dynamical Systems:** Rigorous analysis of CSS dynamics, bifurcation structure
- **Computational Topology:** Persistent homology of shell configurations
- **Theoretical Computer Science:** Universality proofs for pattern-based computation
- **Machine Learning:** Differentiable physics, learning in dynamical systems
- **Linguistics/NLP:** Grammar induction on symbol sequences
- **Category Theory:** Algebraic structure of the full pipeline

**Contact:** 
- GitHub: [FunnelFunction/0.0_Executable_GIF_Physics](https://github.com/FunnelFunction/0.0_Executable_GIF_Physics)
- Website: [funnelfunction.com](https://funnelfunction.com)
- ITT: [intent-tensor-theory.com](https://intent-tensor-theory.com)

### 6.4 Final Statement

We believe that continuous-to-discrete bridges are fundamental to cognition. The brain is a continuous dynamical system that emits discrete actions. Markets are continuous price dynamics that emit discrete trades. Language emerges from continuous neural activity.

Our system is a toy model of this bridge. It is not AGI. It is not even close to AGI. But it is a mathematically precise playground where the questions of emergence, encoding, and computation can be studied.

The gaps we have identified are not failures—they are research directions. We offer this white paper as a roadmap for anyone who wants to help close them.

---

## Appendix A: Mathematical Notation

| Symbol | Meaning |
|--------|---------|
| $\Phi(x,y,t)$ | Scalar field |
| $\nabla\Phi$ | Gradient (vector) |
| $\nabla^2\Phi$ | Laplacian (scalar) |
| $\|\nabla\Phi\|$ | Gradient magnitude |
| $\rho_q$ | Shell indicator (binary) |
| $\Omega^\wedge$ | Memory field |
| $\delta_{\text{drift}}$ | Drift feedback term |
| $\text{CLA}^\wedge$ | Collapse Logic Algebra state |
| $\Sigma$ | Symbol sequence |
| $\mathcal{A}$ | Symbol alphabet |
| $\theta$ | Parameters (η, λ, μ, α, γ) |

## Appendix B: Implementation Details

**Language:** JavaScript (browser-based)  
**Numerics:** Float32Array, central differences, Euler integration with substeps  
**Stability:** Bounded bistable form $\Phi(1-\Phi^2)$, field clamping, CFL-aware substeps  
**Output:** GIF encoding via LZW compression  
**Lines of Code:** ~3,700 (single HTML file, zero dependencies)  
**Live Demo:** [render-executable-gif-physics.onrender.com](https://render-executable-gif-physics.onrender.com)

## Appendix C: References

1. Allen, S. M., & Cahn, J. W. (1979). A microscopic theory for antiphase boundary motion.
2. Ginzburg, V. L., & Landau, L. D. (1950). On the theory of superconductivity.
3. Cahn, J. W., & Hilliard, J. E. (1958). Free energy of a nonuniform system.
4. Adamatzky, A. (2001). Computing in Nonlinear Media and Automata Collectives.
5. Tishby, N., Pereira, F. C., & Bialek, W. (1999). The information bottleneck method.
6. Chen, R. T., et al. (2018). Neural ordinary differential equations.
7. Wolfram, S. (2002). A New Kind of Science.
8. Edelsbrunner, H., & Harer, J. (2010). Computational Topology.

---

*This document is version 1.0. We will update it as progress is made on the open problems.*

*"The map is not the territory, but a good map shows where the territory ends and the unknown begins."*
