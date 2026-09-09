# A3 – Parametric and FEA

## Objective
The objective of this assignment is to design a structural bar under direct tension by using two analytical methods: **Parametric Modeling** (axial deflection elongation) and **Finite Element Analysis (FEA)**. The primary goals include relating force, material properties, and geometry to meet a strict deflection limit (delta_max = 0.009 in), verifying yield safety factor constraints (Sy = 40 ksi), and performing a design reflection including stress concentration (Kt) effects.

* **CAD Part Download Link:** [Download Parametric Bar (.F3D / .STEP)](docs/assignments/A03/Tension_Study.f3d)

---

## Parametric Design & Calculations

### 1. Selected Parameters & Cross-Section

![Hand Sketch of Parametric Bar](sketch.jpg)

* **Applied Direct Tension Load (F):** 400 lbf
* **Modulus of Elasticity (E):** 10.0 * 10^6 psi / 68.8 GPa (Aluminum)
* **Yield Strength (Sy):** 40 ksi (40,000 psi) / 275 MPa
* **Maximum Deflection Limit (delta):** 0.009 in
* **Cross-Sectional Width (w):** 0.75 in
* **Cross-Sectional Height (h):** 0.50 in
* **Cross-Sectional Area (A):** A = w * h = 0.75 in * 0.50 in = 0.375 in^2

### 2. Axial Elongation Hand-Calculation

![Scanned Hand Calculations](hand_calcs.jpg)

Using the direct tension elongation formula from Machinery's Handbook:

delta = (F * L) / (A * E)

Rearranging to solve for bar length (L):

L = (delta * A * E) / F

L = (0.009 in * 0.375 in^2 * 10,000,000 psi) / 400 lbf = 84.375 in

* **Theoretical Bar Length (L):** 84.375 in (7.03125 ft)
* **Nominal Tensile Stress (sigma_nom):** sigma = F / A = 400 lbf / 0.375 in^2 = 1,066.67 psi (~1.07 ksi)

---

## Analyze (CAD & FEA Simulation)

### CAD Parametric Setup
The bar profile was sketched in Fusion 360 using driving parameters in Modify > Change Parameters:
* F = 400 lbf
* E = 10000000 psi
* Delta = 0.009 in
* Width = 0.75 in
* Height = 0.50 in
* Area = Width * Height (0.375 in^2)
* Length = (Delta * Area * E) / F -> Evaluated dynamically to 84.375 in.

![CAD Parameters and Annotated Dimensions](docs/assignments/A03/Parameters_Table.jpg)

---

### FEA Simulation Setup & Results
A Static Stress study was configured in the Fusion Simulation workspace:
* **Boundary Condition:** Fixed support applied to back face (X, Y, Z constrained).
* **Load:** 400 lbf direct axial tensile force applied to front face.
* **Material:** Aluminum (E = 10.0 * 10^6 psi, Sy = 40 ksi).

#### 1. Deflection Map
* **Max FEA Deflection (delta_FEA):** 0.00900 in at the free loaded face.

![Deflection Map](docs/assignments/A03/Displacement.jpg)

#### 2. von Mises Stress Map & Safety Factor
* **Max von Mises Stress (sigma_max):** 1,067 psi (1.07 ksi) uniform along the main body.
* **Yield Strength (Sy):** 40,000 psi (40 ksi)
* **Yield Check:** sigma_max < Sy (1.07 ksi << 40 ksi) -> PASSED
* **Safety Factor (SF):** SF = Sy / sigma_max = 40,000 psi / 1,067 psi = 37.5

![von Mises Stress Map](docs/assignments/A03/Stress.jpg)

---

## Decide (Design Reflection & Comparisons)

### 1. Deflection Comparison & Percent Difference
* **Hand-Calculated Deflection (delta_hand):** 0.00900 in
* **FEA Simulation Deflection (delta_FEA):** 0.00900 in

Percent Difference = (|delta_hand - delta_FEA| / delta_hand) * 100%
Percent Difference = (|0.00900 - 0.00900| / 0.00900) * 100% = 0.00%

* **Why Results Agree:** The hand calculation [delta = (F * L) / (A * E)] and linear FEA solver assume identical uniaxial tensile loading on a uniform prismatic cross-section without bending or boundary friction effects.
* **Trust Verification:** Both models are equally trustworthy for this uniform geometry. If complex stress raisers or asymmetrical constraints were present, FEA would be trusted more.

### 2. Stress Concentration Analysis (Kt)

![Hand Calculations Work](hand_calcs2.jpg)

Estimating peak stress if a central pin hole (d = 0.25 in) is added to the bar:
* **Width Ratio (d / w):** 0.25 in / 0.75 in = 0.333
* **Stress Concentration Factor (Kt):** ~2.30 (from Peterson's Stress Concentration Factors)
* **Net Cross-Sectional Area (A_net):** (0.75 in - 0.25 in) * 0.50 in = 0.250 in^2
* **Nominal Net Stress (sigma_net):** sigma_net = 400 lbf / 0.250 in^2 = 1,600 psi = 1.60 ksi
* **Peak Stress (sigma_peak):** sigma_peak = Kt * sigma_net = 2.30 * 1,600 psi = 3,680 psi = 3.68 ksi

**Safety Evaluation:** sigma_peak = 3.68 ksi << Sy = 40.0 ksi. The bar comfortably passes with a localized Safety Factor of SF_hole = 10.87.

---

## Communicate (Lessons Learned & MEGR 2157)

### Process Documentation & Lessons Learned
* **Actual Time Spent:** 4 Hours (Setup: 45 min, CAD/Calcs: 45 min, FEA: 40 min, Reflections: 45 min, Portfolio: 55 min).
* **Mistakes Made & Resolved:** 
  1. Unit Conversions: Fusion 360 defaulted material parameters to metric units (MPa, kg/mm^3). Resolved by updating Document Settings to Inches and verifying 68.8 GPa ~= 9.98 * 10^6 psi.
  2. Equation References: Fixed equation syntax in Fusion parameters to link variables dynamically to feature dimensions.
* **Lessons Learned:** Linear FEA matches 1D mechanics equations perfectly on basic prismatic geometry, but local geometric discontinuities (Kt) dictate true structural safety.

---

### MEGR 2157: Parameter Iterations

| Iteration Case | Variable Modified | Base Value | New Value | Predicted L Change | Evaluated L |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Baseline** | None | — | — | — | **84.375 in** |
| **Case A** | Load (F) | 400 lbf | 500 lbf | Decrease | **67.500 in** |
| **Case B** | Height (h) | 0.50 in | 0.75 in | Increase | **126.563 in** |
| **Case C** | Width (w) | 0.75 in | 1.00 in | Increase | **112.500 in** |

* **Analytical Finding:** Increasing applied load (F) reduces required length to maintain the 0.009 in deflection limit. Increasing cross-sectional area (w or h) increases stiffness (A * E), allowing a significantly longer bar under the same displacement constraint.
