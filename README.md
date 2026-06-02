# f(R, L_m, T) Dark Matter Wormhole Simulator

**Interactive 3D simulator for traversable wormhole solutions in**
**_f_(R, L_matter, T) gravity supported by dark matter halo profiles**

> Companion tool for the MSc Mathematics project report:
> *"Dark Matter Halo Models as Sources of Traversable Wormholes in f(R, L_matter, T) Gravity"*
> — Ankit Dadhich, Department of Mathematics, Central University of Haryana (2026)

---

## Live Demo

**[Launch Simulator →](https://ankitdadhich.github.io/wormhole-simulator)**

*(Replace this link with your actual GitHub Pages URL after deployment)*

---

## What This Is

This is a fully self-contained, browser-based 3D simulator that
implements the three dark matter wormhole models derived in the
project report. It requires no installation, no server, and no
dependencies beyond a modern web browser — everything runs locally
in the page.

The simulator renders the wormhole geometry using WebGL (via
Three.js), computes all physical quantities from the paper's
equations in real time, and displays live indicators for the
traversability conditions and energy conditions as you adjust the
parameters.

---

## Physics Behind It

The simulator implements the linear model

```
f(R, L_matter, T) = R + a·L_matter + b·T
```

from the report (Eq. 14), with a constant redshift function
Ψ(r) = const. The three dark matter density profiles and their
corresponding wormhole shape functions are:

| Model | Profile | Shape Function (Eq.) |
|---|---|---|
| Model I | Einasto spike: ρ_sp = ρ_s exp[2/α (1 − (r/r_s)^α)] | Eq. (22) involving incomplete Gamma function |
| Model II | Burkert: ρ_b = ρ_s / [(1 + r/r_s)(1 + (r/r_s)²)] | Eq. (24) involving logarithm and arctan |
| Model III | Moore: ρ_m = ρ_s (r/r_s)^(−3/2) / [1 − (r/r_s)^(3/2)] | Eq. (26) involving logarithm |

The deflection angle is computed by numerical integration of:

```
α_def(r₀) = −π + 2 ∫[r₀ to ∞] e^Ψ / √[(1 − χ/r)(r²/u² − e^(2Ψ))] dr
```

using 400-step Gaussian quadrature (Eq. 35 of the report).

---

## Features

### Two Visualization Modes

**Embedding Diagram** (default)
- 3D surface of revolution representing the wormhole throat
  geometry in the equatorial plane (t = const, θ = π/2)
- Upper universe (z > 0) and lower universe (z < 0) rendered as
  two symmetric funnel-shaped sheets connected at the throat
- The amber ring marks the wormhole throat at r = r₀ where
  χ(r₀) = r₀ and dz/dr → ∞
- Drag to rotate · Scroll to zoom · Auto-rotates when idle

**Photon Trajectories**
- Six null geodesics with varying impact parameters
  u = factor × r₀ (factor from 1.10 to 3.0)
- Trajectories computed from the orbit equation derived from
  the null geodesic condition in the Morris–Thorne metric
- Sky-blue paths show how light is deflected away from the
  throat — the wormhole acts as a diverging lens, not a
  focusing one (α_def < 0 for all three models)

---

### Adjustable Parameters

**Visualization controls**
- Minimum impact parameter u/r₀ (1.02 → 3.0)
- Radial range r_max (4 → 25)
- Mesh resolution (24 → 120 grid points)

**Dark Matter Profile** — switch between Einasto, Burkert, Moore

**Einasto profile (Model I)**
- r₀ throat radius (0.5 → 5.0), default: 2.00
- ρ_s central density (0.0001 → 0.005), default: 0.001
- α Einasto index (0.3 → 4.0), default: 1.23
- r_s scale radius (0.1 → 4.0), default: 0.50

**Burkert profile (Model II)**
- r₀ throat radius (0.5 → 5.0), default: 2.40
- ρ_s central density (0.0001 → 0.02), default: 0.004
- r_s scale radius (0.5 → 8.0), default: 3.70

**Moore profile (Model III)**
- r₀ throat radius (0.5 → 5.0), default: 1.12
- ρ_s central density (10⁻⁶ → 2×10⁻⁴), default: 1.4×10⁻⁵
- r_s scale radius (2.0 → 25.0), default: 14.20

**f(R, L_m, T) Coupling Constants**
- a — L_matter coupling (−10 → +15), default: 2.00
- b — T coupling (−5 → +5), default: 1.40

---

### Live Physics Indicators (bottom bar)

All four indicators update instantly as you move any slider:

| Indicator | Condition | What it means |
|---|---|---|
| **χ′(r₀)** | Must be < 1 | Flaring-out condition: wormhole throat opens outward |
| **χ(r₀)/r₀** | Must = 1 | Throat condition: r₀ is genuinely the minimal surface |
| **α_def (rad)** | Negative = diverging | Deflection angle of light; negative means defocusing |
| **NEC: ρ + p_r at throat** | < 0 = exotic matter | Null energy condition violation at the throat |

The **Lens type** indicator shows **DIVERGING** (repulsive lensing,
as found in the report) or **FOCUSING** depending on the sign of
α_def.

The **energy conditions panel** shows a live summary of NEC, WEC,
DEC, SEC, and TEC status for the current parameter values.

---

### Default Parameter Values

The simulator loads with the exact parameter values used in the
figures of the research paper:

- **Model I (Einasto):** r₀ = 2, ρ_s = 0.001, b = 1.4, r_s = 0.5, α = 1.23
- **Model II (Burkert):** r₀ = 2.4, ρ_c = 0.004, b = −0.4, r_s = 3.7
- **Model III (Moore):** r₀ = 1.12, ρ_s = 1.4×10⁻⁵, b = 2.17, r_s = 14.2

These defaults reproduce the results shown in Figs. 1–3 and
Fig. 12 of the report.

---

## How to Use

**Online (recommended)**
Just open the link above. No installation needed.

**Offline / Local**
1. Download `index.html` (or `wormhole_simulator_v2.html`)
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. Everything works offline — the only external resources are
   Three.js (3D rendering) and Google Fonts (typography), both
   loaded from CDN. If you are offline, the simulator still works
   but uses fallback fonts.

---

## Technical Details

| Item | Detail |
|---|---|
| **Rendering** | Three.js r128 (WebGL) |
| **Shape functions** | Computed from Eqs. (22), (24), (26) of the report |
| **Incomplete Gamma** | Evaluated by series expansion (200 terms) |
| **Deflection angle** | 400-step numerical integration of Eq. (35) |
| **File size** | ~38 KB (single HTML file, no build step) |
| **Dependencies** | Three.js r128 (CDN), Google Fonts (CDN) |
| **Browser support** | Any browser with WebGL support (2015+) |

---

## Repository Structure

```
wormhole-simulator/
│
├── index.html          ← The entire simulator (single file)
└── README.md           ← This file
```

---

## Citing This Work

If you use this simulator in any academic context, please cite the
accompanying report:

> Ankit Dadhich, Sweeti Kiroriwal, and Jitendra Kumar,
> *"Dark Matter Halo Models as Sources of Traversable Wormholes
> in f(R, L_matter, T) Gravity"*,
> MSc Project Report, Department of Mathematics,
> Central University of Haryana, Mahendergarh (2026).
> Communicated for publication.

---

## Author

**Ankit Dadhich**
M.Sc. Mathematics, Central University of Haryana
📧 ankitdadhich1729@gmail.com

**Supervisors:**
Dr. Jitendra Kumar (jitendark@gmail.com)
Ms. Sweeti Kiroriwal (sweeti222020@cuh.ac.in)

Department of Mathematics, Central University of Haryana,
Jant-Pali, Mahendergarh — 123031, Haryana, India

---

## License

This simulator is released for academic and educational use.
The physics implementation is based on the authors' original
research article. Feel free to use, share, or modify it for
non-commercial academic purposes with attribution.
