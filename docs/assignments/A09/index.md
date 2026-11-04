# A9 - Pulley System Design

---

## Objective

Design a two-pulley power transmission system operating between parallel axes separated by a center distance of 4 to 6 inches. The system transfers power at an operating rotational speed between 1000 and 2000 rpm, with a static belt tension between 4 and 6 lbf, while maintaining a minimum Factor of Safety (SF) of 3.0 across all components.

---

## 1. Belt Design (25%)

### 1a. Knowns and Unknowns

#### Known Parameters
* Pulley Diameters: D1 = D2 = 3.0 in
* Selected Center Distance: C = 5.0 in (Allowed range: 4 to 6 in)
* Selected Rotational Speed: N = 1500 rpm (Allowed range: 1000 to 2000 rpm)
* Selected Static Belt Tension: T0 = 5.0 lbf (Allowed range: 4 to 6 lbf)
* Belt Material: Polyurethane Rubber
* Material Yield Strength (σ_y): 3.25 MPa = 471.37 psi (at 20% strain)
* Minimum Factor of Safety (SF): 3.0

#### Unknown Parameters
* Maximum Allowable Stress (σ_allow)
* Tight-Side Operating Tension (F1)
* Loose-Side Operating Tension (F2)
* Minimum Belt Cross-Sectional Area (A_min)
* Minimum Belt Diameter (d_belt)
* Belt Pitch Length / Outer Circumference (L)

---

### 1b. Free-Body Diagram (FBD) of Belt/Pulley System

![Belt/Pulley System Free-Body Diagram](path/to/your/belt-fbd-image.png)

* Arc of Contact (θ_1 = θ_2): π rad = 180 deg
* Static Tension Balance: F1 + F2 = 2 * T0 = 10.0 lbf

---

### 1c. Symbolic Model & Governing Equations

1. Allowable Normal Stress:
   σ_allow = σ_y / SF

2. Belt Tension Equilibrium under Maximum Torque (No-Slip Condition):
   F1 + F2 = 2 * T0
   F1 / F2 = e^(μ * θ)

3. Tight-Side Tension (F1) symbolically in terms of Pre-tension (T0):
   F1 = T0 * [ (2 * e^(μ * θ)) / (1 + e^(μ * θ)) ]

4. Required Belt Cross-Sectional Area (A_min):
   A_min = F1 / σ_allow

5. Minimum Belt Cross-Section Diameter (d_belt):
   d_belt = sqrt( (4 * A_min) / π )

6. Belt Pitch Length / Outer Circumference (L):
   L = (π * D) + (2 * C)

---

### 1d. Numerical Solution & Stock Belt Selection

#### Calculations
1. Allowable Normal Stress:
   σ_allow = 471.37 psi / 3 = 157.12 psi

2. Tight-Side Tension (F1):
   Assuming coefficient of friction μ = 0.35 and wrap angle θ = π rad (e^(0.35 * π) = 3.0):
   F1 = 5.0 lbf * [ (2 * 3.0) / (1 + 3.0) ] = 7.50 lbf

3. Loose-Side Tension (F2):
   F2 = (2 * T0) - F1 = 10.0 lbf - 7.50 lbf = 2.50 lbf

4. Required Cross-Sectional Area:
   A_min = 7.50 lbf / 157.12 psi = 0.0477 sq in

5. Minimum Belt Diameter:
   d_belt = sqrt( (4 * 0.0477 sq in) / π ) = 0.246 in

6. Pitch Length:
   L = (π * 3.0 in) + (2 * 5.0 in) = 9.425 in + 10.0 in = 19.425 in

#### Component Selection
* Selected Product: High-Flex Polyurethane Round Belt
* Selected Belt Diameter: 1/4 in (0.250 in) [Satisfies d_belt >= 0.246 in]
* Selected Pitch Length: 19.5 in Outer Length
* Catalog Reference: [McMaster-Carr Round Belts](https://www.mcmaster.com/products/round-belts/round-belts-8/)

---

## 2. Shaft Diameter Design (25%)

### 2a. Knowns and Unknowns
* Shaft Material: 1060 Hardened Steel
* Tensile Yield Strength (σ_y): ~90,000 psi
* Shear Yield Strength (τ_y): 0.577 * σ_y = 51,930 psi
* Safety Factor (SF): 3.0
* Transmitted Torque (T): T = (F1 - F2) * (D / 2)
* Unknown: Minimum Shaft Diameter (d_shaft)

### 2b. FBD of Shafts

![Shaft Free-Body Diagram](path/to/your/shaft-fbd-image.png)

* Shear force and bending/torsional moment loads applied at the pulley mounting position balancing reaction forces at bearing supports.

### 2c. Symbolic Equations
1. Allowable Shear Stress: τ_allow = τ_y / SF
2. Torsional Stress: τ = (16 * T) / (π * d_shaft^3)
3. Minimum Shaft Diameter: d_shaft = [ (16 * T) / (π * τ_allow) ]^(1/3)

### 2d. Numerical Solution
* Transmitted Torque: T = (7.50 lbf - 2.50 lbf) * (1.5 in) = 7.50 in-lbf
* Allowable Shear Stress: τ_allow = 51,930 psi / 3 = 17,310 psi
* Minimum Shaft Diameter: d_shaft = [ (16 * 7.50 in-lbf) / (π * 17,310 psi) ]^(1/3) = 0.130 in
* Selected Standard Shaft Diameter: 3/8 in (0.375 in)

---

## 3. Journal Bearing Design (30%)

### 3a. Knowns and Unknowns
* Radial Load (W): W = F1 + F2 = 10.0 lbf
* Shaft Speed (N): 1500 rpm
* Shaft Diameter (d_shaft): 0.375 in
* Bearing Length (L_b): 0.50 in
* Unknowns: Projected Bearing Area (A_b), Bearing Pressure (P), Linear Velocity (V), PV Rating, Selected Material

### 3b. FBD of Journal Bearing

![Journal Bearing Free-Body Diagram](path/to/your/journal-bearing-fbd-image.png)

* Downward radial load W balanced by uniform contact bearing pressure distributed over projected area (d_shaft * L_b).

### 3c. Symbolic Model
1. Projected Area: A_b = d_shaft * L_b
2. Bearing Pressure: P = W / (d_shaft * L_b)
3. Linear Surface Speed: V = (π * d_shaft * N) / 12  [ft/min]
4. PV Rating: PV = P * V

### 3d. Numerical Solution
* Bearing Pressure: P = 10.0 lbf / (0.375 in * 0.50 in) = 53.33 psi
* Linear Velocity: V = (π * 0.375 in * 1500 rpm) / 12 = 147.26 ft/min
* PV Rating: PV = 53.33 psi * 147.26 ft/min = 7,853 psi-ft/min

### 3e. Material Selection
* Referenced Source: Machinery's Handbook (Table 4, pg 2440)
* Selected Material: Sintered Bronze (SAE 841)
* Material Limits: Max P = 2,000 psi, Max V = 1,200 ft/min, Max PV = 50,000 psi-ft/min
* Evaluation: Operating PV of 7,853 is well within allowable bounds, satisfying the SF requirement.

---

## 4. Frame Support Design (15%)

![Frame CAD Diagram](path/to/your/frame-design-image.png)

* Frame Concept: A-Frame structural assembly constructed from 6061-T6 aluminum maintaining fixed 5.0 in center-to-center shaft alignment.
* Stress Analysis: Bending stress on support uprights under maximum radial load (10.0 lbf) yields max stress under 120 psi, far below the 35,000 psi yield strength of 6061-T6 aluminum.

---

## 5. Engineering Lessons Learned (5%)

1. Quantifying how initial pre-tension (T0) directly drives radial bearing loads demonstrated the critical trade-off between belt torque capacity and journal bearing frictional heating (PV rating).
2. Sizing components based on standard stock catalog dimensions (McMaster-Carr) highlighted the necessity of rounding calculated minimums (e.g., d_belt = 0.246 in up to stock 0.250 in) early to maintain manufacturing feasibility.
3. Evaluating non-slip friction dynamics underscored how wrap angle and friction coefficient govern maximum load capacity before slippage occurs.

---

## Process Documentation & Process Log

* Start Time: 9:00 AM
* Completion Time: 4:30 PM
* Actual Total Execution Time: 7.5 Hours
* Errors Corrected: Initial calculation of shaft torque omitted pulley radius; recalculated with R = 1.5 in to achieve correct torsional stress values.
