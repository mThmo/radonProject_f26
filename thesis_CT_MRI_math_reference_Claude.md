# Undergrad Math Thesis: CT, MRI, Radon Transform & Fourier Slice Theorem
### A Reference Document — Theory-First Approach

---

## The Core Idea

You are proposing a thesis in **Inverse Problems and Integral Transforms**, using medical imaging (CT and MRI) as the concrete application. The central mathematical question is:

> *Given only the measurements of an unknown function (e.g., the interior of a human body), can we reconstruct it? Under what conditions? And how stable is that reconstruction?*

This is a deeply legitimate and respected area of applied mathematics, sitting at the intersection of:
- **Functional Analysis** (Hilbert spaces, operator theory)
- **Harmonic Analysis** (Fourier analysis, integral transforms)
- **Inverse Problem Theory** (ill-posedness, regularization)

A thesis that prioritizes rigorous mathematical theory over optimized implementation is exactly what many math departments prefer. The "weight" of the work comes from proof-writing and conceptual depth, not code.

---

## Why This Topic Works as a Math Thesis

### 1. It is Classically Well-Motivated
The Radon transform was introduced by **Johann Radon in 1917**, decades before CT scanners existed. The mathematics came first; the application came later. This is ideal for a pure/applied math thesis — you are studying the theory *because* it is beautiful, and the physics just happens to be a perfect laboratory.

### 2. It Bridges Abstract and Concrete
You will prove results that directly explain observable phenomena:
- *Why do CT reconstructions become blurry when you add noise?* → Ill-posedness of the inverse Radon transform.
- *Why does MRI have a concept called "k-space"?* → MRI directly samples the Fourier transform; k-space IS the Fourier domain.
- *Why is Filtered Backprojection the standard CT algorithm?* → It falls out naturally from the change of variables in the Fourier Slice Theorem.

### 3. The Mathematical Depth is Genuine
This is not a shallow applied topic. To do it rigorously, you will need:
- Measure theory and L² spaces
- Fourier analysis on ℝⁿ (Plancherel, Parseval)
- Distribution theory (Schwartz spaces, tempered distributions)
- Sobolev spaces (for regularity and stability analysis)
- Operator theory (boundedness, adjoint operators, compact operators)
- Spectral theory (for understanding the ramp filter and ill-posedness)

---

## Suggested Thesis Structure

### Chapter 0: Motivation and Physical Setup (Brief)
Keep this short. Just enough physics to justify the math.
- CT: An X-ray beam passes through tissue; detectors measure **line integrals** of the attenuation function. The data collected across all angles is a **sinogram**.
- MRI: RF pulses perturb hydrogen proton spins; the received signal encodes **Fourier coefficients** of the proton density function. Measurements are collected in **k-space**.
- Your thesis is about the math *after* this data exists — how do we invert it?

---

### Chapter 1: Functional Analytic Framework

#### 1.1 Function Spaces
Define the spaces in which your objects live:
- **Schwartz space** 𝒮(ℝⁿ): infinitely differentiable, rapidly decaying. The natural domain for the Fourier transform.
- **L²(ℝⁿ)**: square-integrable functions. The natural setting for energy-finite signals (motivated by Plancherel's theorem).
- **Sobolev spaces** Hˢ(ℝⁿ): L² functions whose (weak) derivatives up to order *s* are also L². These capture smoothness and will arise when analyzing stability.
- Briefly discuss why L² sometimes fails (e.g., functions with sharp edges lie in L² but not in smoother spaces; this motivates BV spaces or distributions for images with discontinuities).

#### 1.2 The Fourier Transform
- Define on 𝒮(ℝⁿ) and extend to L²(ℝⁿ) via density.
- Key theorems: **Plancherel** (‖f̂‖ = ‖f‖), **Parseval**, convolution theorem, Fourier inversion.
- Emphasize the geometric content: the Fourier transform decomposes a function into oscillatory components; its magnitude spectrum reveals frequency content and its phase encodes spatial structure.

#### 1.3 Hilbert Space Theory (as needed)
- Inner products, orthonormal bases, projection theorem.
- Bounded linear operators, adjoints.
- Compact operators and their spectral properties (relevant when discussing why inversion is ill-posed).

---

### Chapter 2: The Radon Transform

#### 2.1 Definition
For a function f: ℝ² → ℝ (compactly supported or in a suitable function space), the **Radon transform** is:

```
(Rf)(s, θ) = ∫_{-∞}^{∞} f(s·θ̂ + t·θ̂⊥) dt
```

More concisely, it integrates f over the line at signed distance *s* from the origin in direction θ:

```
(Rf)(s, θ) = ∫_{ℓ(s,θ)} f(x) dℓ
```

The output Rf is a function on the **cylinder** ℝ × S¹, often visualized as a **sinogram** (a point in the image traces a sinusoidal curve in sinogram space).

#### 2.2 The Radon Transform as an Operator
Analyze R as a **continuous linear operator**:
```
R : L²(ℝ²) → L²(ℝ × S¹)
```
- Show boundedness.
- Compute the **adjoint R***, known as the **backprojection operator**:
```
(R*g)(x) = ∫_{S¹} g(x·θ, θ) dθ
```
R* "smears" each projection back over the image, averaging all lines through each point. This has a beautiful geometric interpretation.
- Note: R*R is **not** the identity. This is a first hint that inversion is non-trivial.

#### 2.3 Basic Properties
- **Linearity**: Obvious.
- **Symmetry**: Rf(s, θ) = Rf(−s, θ + π), so sinograms are naturally 2π-periodic in a redundant way.
- **Consistency conditions** (Helgason-Ludwig): Not all functions on ℝ × S¹ are valid sinograms. The range of R is a proper subspace, characterized by polynomial moment conditions.
- Behavior under **rotations and translations** in the image domain.

---

### Chapter 3: The Fourier Slice Theorem

This is the centerpiece of the thesis.

#### 3.1 Statement
Let f ∈ 𝒮(ℝ²), and let F₂f denote its 2D Fourier transform and F₁(Rf)(·, θ) denote the 1D Fourier transform of the projection at angle θ. Then:

```
F₁{(Rf)(·, θ)}(ρ) = (F₂f)(ρ cos θ, ρ sin θ)
```

**In words**: The 1D Fourier transform of a projection is a radial slice through the 2D Fourier transform of the original function.

#### 3.2 Proof Sketch
The proof is elegant and elementary given your setup:
1. Write out the 1D Fourier transform of the projection integral.
2. Use Fubini's theorem to exchange the order of integration.
3. Recognize the result as the 2D Fourier transform evaluated along a ray.

#### 3.3 Geometric Interpretation
The theorem says that if you take projections of f at every angle θ and compute their 1D Fourier transforms, you are filling in the 2D Fourier plane **radially** — one spoke per projection angle. Reconstruction from projections is equivalent to **polar-to-Cartesian interpolation in Fourier space**.

#### 3.4 Consequences
- **Exact reconstruction formula** (in the continuous limit): collect projections at all angles, take their 1D Fourier transforms, fill the 2D Fourier plane, apply 2D inverse Fourier transform.
- **Why dense sampling at low frequencies but sparse at high**: The radial sampling density is uniform in angle, so the center of the Fourier plane is over-sampled relative to the periphery.
- This non-uniform density is the origin of the **ramp filter** in Filtered Backprojection.

---

### Chapter 4: Inversion — Filtered Backprojection

#### 4.1 Derivation of Filtered Backprojection (FBP)
Start from the 2D inverse Fourier transform and change to polar coordinates:

```
f(x) = ∫∫ F₂f(ξ) e^{2πi x·ξ} dξ₁ dξ₂

     = ∫₀^π ∫_{-∞}^{∞} (F₁Rf)(ρ, θ) |ρ| e^{2πi ρ (x·θ̂)} dρ  dθ
```

The Jacobian of the polar coordinate change introduces the factor **|ρ|** — the ramp filter. The algorithm becomes:
1. For each angle θ, take the 1D Fourier transform of the projection.
2. **Multiply by |ρ|** (the ramp filter).
3. Take the inverse 1D Fourier transform (this is the "filtering" step).
4. **Backproject**: integrate the filtered projections over all angles (this is R*).

So **FBP = R* ∘ (ramp filter)**.

#### 4.2 The Ramp Filter and Its Problems
The ramp filter |ρ| amplifies high frequencies. This is problematic because:
- Real data always contains noise (measurement errors, quantum effects in CT).
- Noise lives predominantly at high frequencies.
- The ramp filter blows up high-frequency noise, making the reconstruction noisy and sometimes unusable.

This is the **instability** or **ill-posedness** of the inverse Radon transform. A small perturbation in the data can cause a large change in the reconstruction.

#### 4.3 Regularization via Window Functions
To suppress noise, the ramp filter is multiplied by a **window function** W(ρ) that tapers to zero at high frequencies:
- **Hamming window**: W(ρ) = 0.54 + 0.46 cos(πρ/ρ_max)
- **Hann window**: W(ρ) = 0.5(1 + cos(πρ/ρ_max))
- **Shepp-Logan filter**: W(ρ) = sinc(ρ/2ρ_max)

The trade-off: more aggressive windowing reduces noise but blurs the image (loss of high-frequency detail). This is a fundamental tension in inverse problem regularization.

---

### Chapter 5: Ill-Posedness and Regularization Theory

#### 5.1 Hadamard's Conditions
A problem is **well-posed** (in the sense of Hadamard) if:
1. A solution exists.
2. The solution is unique.
3. The solution depends continuously on the data.

The inverse Radon transform satisfies (1) and (2) under reasonable conditions, but **fails (3)**. Small errors in the sinogram (data) can produce arbitrarily large errors in the reconstruction — this is ill-posedness.

#### 5.2 Tikhonov Regularization
The standard approach: instead of solving Rf = g exactly, minimize:

```
‖Rf - g‖² + α‖Lf‖²
```

where α > 0 is the **regularization parameter** and L is a differential operator (often L = I or a Laplacian). The first term enforces data fidelity; the second enforces smoothness. The solution:

```
f_α = (R*R + αL*L)⁻¹ R* g
```

The choice of α is critical:
- **α too small**: the solution overfits noise (essentially unregularized).
- **α too large**: the solution is over-smoothed, losing true features.
- Standard selection methods: **L-curve criterion**, **discrepancy principle** (Morozov), **generalized cross-validation**.

#### 5.3 Spectral Analysis of the Ill-Posedness
Using the singular value decomposition (SVD) of R (or the related **singular value expansion** for infinite-dimensional operators), one can show that the singular values of R decay to zero. The decay rate governs how ill-posed the problem is:
- Faster decay = more severely ill-posed.
- The ramp filter is precisely the spectral inverse: it inverts the singular values, which explains its amplification of high frequencies (since small singular values become large after inversion).

This gives a beautiful connection between **spectral theory** and the practical problem of noise amplification.

---

### Chapter 6: MRI and the Connection to Radon

#### 6.1 MRI is Direct Fourier Sampling
In MRI, the measured signal at each time point is:

```
S(k) = ∫∫ ρ(x) e^{-2πi k·x} dx dy
```

where ρ(x) is the proton density (the "image" we want) and k is a point in **k-space** (the Fourier domain). The MRI scanner directly samples k-space along trajectories determined by the gradient waveforms.

Reconstruction for **Cartesian k-space sampling** is simply an inverse 2D FFT.

#### 6.2 Radial MRI and the Radon Connection
For **radial k-space sampling** (spokes through the origin at various angles), the Fourier Slice Theorem directly applies:
- Each radial spoke in k-space = 1D Fourier transform of a projection = one row of the sinogram.
- Radial MRI reconstruction is therefore *identical* to CT reconstruction from projections.
- The same FBP algorithm applies, with the same ramp filter and the same ill-posedness issues.

This deep connection — that radial MRI and X-ray CT are mathematically the same inverse problem — is one of the most satisfying theoretical results you can present.

#### 6.3 Non-Cartesian Reconstruction Challenges
For non-Cartesian trajectories (spiral, radial, etc.):
- Simple FFT doesn't apply directly (FFT requires uniform Cartesian samples).
- Must use **gridding**: interpolate non-Cartesian samples onto a Cartesian grid (using a convolution kernel), then apply FFT.
- Or use **Non-Uniform FFT (NUFFT)** algorithms.
- The density compensation function (DCF) plays the role of the ramp filter — it corrects for the non-uniform sampling density in k-space.

*Note: For a theory-focused thesis, you can treat this section at a higher level, focusing on the mathematical framework rather than implementation details.*

---

### Chapter 7: Demonstrations and Visualizations

Since your thesis is theory-first, the computational component should be simple and illustrative — tools to build intuition, not to optimize performance.

#### Suggested Demonstrations (Python/MATLAB)

**The Shepp-Logan Phantom**: a standard test image for CT algorithms; looks like a cross-section of a head.

1. **Forward Problem**: Apply the Radon transform to get the sinogram. Visualize the sinogram and note how each point in the image traces a sinusoidal curve.

2. **Naive Backprojection**: Apply R* to the sinogram without filtering. Observe the characteristic "star artifact" blur — this illustrates why the ramp filter is necessary.

3. **Filtered Backprojection**: Apply the ramp filter before backprojection. Observe the sharp reconstruction. Compare different window functions (Hann, Hamming) to see the noise/sharpness trade-off.

4. **Ill-Posedness Demo**: Add Gaussian noise to the sinogram before reconstruction. Show quantitatively (RMSE or SNR) and visually how the reconstruction degrades with noise, and how windowing mitigates this.

5. **Fourier Slice Theorem Visualization**: Take the 2D FFT of the phantom. Take projections at several angles and compute their 1D FFTs. Overlay these on the 2D FFT magnitude and show they lie exactly on radial slices.

#### Useful Python Tools
```python
from skimage.transform import radon, iradon  # FBP built-in
from skimage.data import shepp_logan_phantom
import numpy as np
import matplotlib.pyplot as plt
```

---

## How to Pitch This to Your Advisor

Use precise language to signal mathematical seriousness. Say something like:

> "I want to write my thesis on the functional analysis of inverse problems in medical imaging. Specifically, I plan to:
> 1. Develop the L² operator theory of the Radon transform and its adjoint.
> 2. Prove the Fourier Slice Theorem rigorously and derive Filtered Backprojection from it.
> 3. Analyze the ill-posedness of the inverse Radon transform and discuss regularization strategies, particularly Tikhonov regularization and spectral windowing.
> 4. Connect this framework to MRI k-space sampling, particularly for radial trajectories.
> 5. Illustrate the theory with basic Python/MATLAB visualizations using the Shepp-Logan phantom."

This framing emphasizes rigor, theory, and proofs — exactly what a math advisor wants to hear.

---

## Things to Avoid

- **Too much physics**: You don't need to explain how X-ray tubes work, how MRI magnets produce fields, or how gradient coils function. A paragraph of motivation is enough. Your thesis is about what happens *after* the measurements are taken.

- **Over-engineering the code**: Resist the urge to implement GPU-accelerated FBP or iterative reconstruction algorithms. Your visualizations are pedagogical tools, not research contributions.

- **Skimping on proofs**: Because you have limited computational work, the rigor of your proofs and the depth of your theoretical explanations carry the thesis. Every major result should be proved (or cited and explained thoroughly).

- **Ignoring the functional analytic subtleties**: For example, the Radon transform as an operator between L² spaces requires care — you need to define the right weighted L² spaces on the cylinder to make R bounded. Don't gloss over these details.

---

## Possible Extensions (If You Want More Depth)

- **Distribution theory**: Define the Radon transform on tempered distributions 𝒮'(ℝⁿ). This is the "correct" generalization and handles functions that aren't in L².
- **3D Radon and X-ray transforms**: The 2D case (line integrals) used in CT is actually the **X-ray transform**. The true 3D Radon transform integrates over planes. Discussing the distinction is a nice theoretical addition.
- **Compressed sensing / sparsity**: Modern MRI uses far fewer k-space samples than Nyquist sampling would require, relying on sparsity priors (L¹ regularization). This connects to the active research area of compressed sensing.
- **Iterative reconstruction**: Algebraic Reconstruction Technique (ART) and its modern variants. These are formulated as optimization problems and connect naturally to your regularization theory chapter.
- **Sobolev space analysis**: Precisely quantify the smoothing order of R and R* using Sobolev norms, giving a rigorous characterization of the ill-posedness in terms of function space regularity.

---

## Key References to Start With

- **Natterer, F.** — *The Mathematics of Computerized Tomography* (2001). The canonical mathematical reference. Rigorous, comprehensive, and exactly at the right level.
- **Helgason, S.** — *The Radon Transform* (1999). More abstract, group-theoretic treatment. Good for the purist.
- **Kak & Slaney** — *Principles of Computerized Tomographic Imaging* (1988, free online). More engineering-flavored but covers the math well; good for building intuition.
- **Engl, Hanke & Neubauer** — *Regularization of Inverse Problems* (1996). The standard reference for Tikhonov regularization and ill-posedness theory.
- **Folland, G.** — *Real Analysis* (1999). For the functional analysis background (L² spaces, Fourier analysis).

---

*This document consolidates a planning conversation about a math undergrad thesis topic. It is intended as a reference for the thesis structure, key mathematical content, and framing strategy.*
