# A9 - Pulley System Design

---

## Objective

Design a two-pulley power transmission system operating between parallel axes separated by a center distance of 4 to 6 inches. The system transfers power at an operating rotational speed between 1000 and 2000 rpm, with a static belt tension between 4 and 6 lbf, while maintaining a minimum Factor of Safety ($SF$) of 3.0 across all components.

---

## 1. Belt Design

### 1a. Knowns and Unknowns

#### Known Parameters
* Pulley Diameters: $D_1 = D_2 = 3.0\text{ in}$
* Selected Center Distance: $C = 5.0\text{ in}$ (Allowed range: 4 to 6 in)
* Selected Rotational Speed: $N = 1500\text{ rpm}$ (Allowed range: 1000 to 2000 rpm)
* Selected Static Belt Tension: $T_0 = 5.0\text{ lbf}$ (Allowed range: 4 to 6 lbf)
* Belt Material: Polyurethane Rubber
* Material Yield Strength ($\sigma_y$): $3.25\text{ MPa} = 471.37\text{ psi}$ (at 20% strain)
* Minimum Factor of Safety ($SF$): $3.0$

#### Unknown Parameters
* Maximum Allowable Stress ($\sigma_{\text{allow}}$)
* Tight-Side Operating Tension ($F_1$)
* Loose-Side Operating Tension ($F_2$)
* Minimum Belt Cross-Sectional Area ($A_{\text{min}}$)
* Minimum Belt Diameter ($d_{\text{belt}}$)
* Belt Pitch Length / Outer Circumference ($L$)

---

### 1b. Free-Body Diagram (FBD) of Belt/Pulley System

<p align="center">
  <img src=IMG_5196.JPG alt="Belt/Pulley System Free-Body Diagram" width="80%"/>
</p>

* Arc of Contact ($\theta_1 = \theta_2$): $\pi\text{ rad} = 180^\circ$
* Static Tension Balance: $F_1 + F_2 = 2 T_0 = 10.0\text{ lbf}$

---

### 1c. Symbolic Model & Governing Equations

1. **Allowable Normal Stress:**

   $$\sigma_{\text{allow}} = \frac{\sigma_y}{SF}$$

2. **Belt Tension Equilibrium under Maximum Torque (No-Slip Condition):**

   $$F_1 + F_2 = 2 T_0$$

   $$\frac{F_1}{F_2} = e^{\mu \theta}$$

3. **Tight-Side Tension ($F_1$) symbolically in terms of Pre-tension ($T_0$):**

   $$F_1 = T_0 \left( \frac{2 e^{\mu \theta}}{1 + e^{\mu \theta}} \right)$$

4. **Required Belt Cross-Sectional Area ($A_{\text{min}}$):**

   $$A_{\text{min}} = \frac{F_1}{\sigma_{\text{allow}}}$$

5. **Minimum Belt Cross-Section Diameter ($d_{\text{belt}}$):**

   $$d_{\text{belt}} = \sqrt{\frac{4 A_{\text{min}}}{\pi}}$$

6. **Belt Pitch Length / Outer Circumference ($L$):**

   $$L = \pi D + 2C$$

---

### 1d. Numerical Solution & Stock Belt Selection

#### Calculations

1. **Allowable Normal Stress:**

   $$\sigma_{\text{allow}} = \frac{471.37\text{ psi}}{3} = 157.12\text{ psi}$$

2. **Tight-Side Tension ($F_1$):**
   Assuming coefficient of friction $\mu = 0.35$ and wrap angle $\theta = \pi\text{ rad}$ ($e^{0.35 \pi} = 3.0$):

   $$F_1 = 5.0\text{ lbf} \times \left( \frac{2 \times 3.0}{1 + 3.0} \right) = 7.50\text{ lbf}$$

3. **Loose-Side Tension ($F_2$):**

   $$F_2 = 2 T_0 - F_1 = 10.0\text{ lbf} - 7.50\text{ lbf} = 2.50\text{ lbf}$$

4. **Required Cross-Sectional Area:**

   $$A_{\text{min}} = \frac{7.50\text{ lbf}}{157.12\text{ psi}} = 0.0477\text{ in}^2$$

5. **Minimum Belt Diameter:**

   $$d_{\text{belt}} = \sqrt{\frac{4 \times 0.0477}{\pi}} = 0.246\text{ in}$$

6. **Pitch Length:**

   $$L = \pi (3.0\text{ in}) + 2(5.0\text{ in}) = 9.425\text{ in} + 10.0\text{ in} = 19.425\text{ in}$$

#### Component Selection
* Selected Product: High-Flex Polyurethane Round Belt
* Selected Belt Diameter: $1/4\text{ in}$ ($0.250\text{ in}$) [Satisfies $d_{\text{belt}} \ge 0.246\text{ in}$]
* Selected Pitch Length: $19.5\text{ in}$ Outer Length
* Catalog Reference: [McMaster-Carr Round Belts](https://www.mcmaster.com/products/round-belts/round-belts-8/)

---

## 2. Shaft Diameter Design 

### 2a. Knowns and Unknowns
* Shaft Material: 1060 Hardened Steel
* Tensile Yield Strength ($\sigma_y$): $\sim 90,000\text{ psi}$
* Shear Yield Strength ($\tau_y$): $0.577 \times \sigma_y = 51,930\text{ psi}$
* Safety Factor ($SF$): $3.0$
* Transmitted Torque ($T$): $T = (F_1 - F_2) \left(\frac{D}{2}\right)$
* Unknown: Minimum Shaft Diameter ($d_{\text{shaft}}$)

### 2b. FBD of Shafts

<p align="center">
  <img src=IMG_5196.JPG alt="Shaft Free-Body Diagram" width="80%"/>
</p>

* Shear force and bending/torsional moment loads applied at the pulley mounting position balancing reaction forces at bearing supports.

---

### 2c. Symbolic Equations

1. **Allowable Shear Stress:**

   $$\tau_{\text{allow}} = \frac{\tau_y}{SF}$$

2. **Torsional Stress:**

   $$\tau = \frac{16 T}{\pi d_{\text{shaft}}^3}$$

3. **Minimum Shaft Diameter:**

   $$d_{\text{shaft}} = \sqrt[3]{\frac{16 T}{\pi \tau_{\text{allow}}}}$$

---

### 2d. Numerical Solution

1. **Transmitted Torque:**

   $$T = (7.50\text{ lbf} - 2.50\text{ lbf}) \times 1.5\text{ in} = 7.50\text{ in-lbf}$$

2. **Allowable Shear Stress:**

   $$\tau_{\text{allow}} = \frac{51,930\text{ psi}}{3} = 17,310\text{ psi}$$

3. **Minimum Shaft Diameter:**

   $$d_{\text{shaft}} = \sqrt[3]{\frac{16 \times 7.50\text{ in-lbf}}{\pi \times 17,310\text{ psi}}} = 0.130\text{ in}$$

4. **Selected Standard Shaft Diameter:** $3/8\text{ in}$ ($0.375\text{ in}$)

---

## 3. Journal Bearing Design 

### 3a. Knowns and Unknowns
* Radial Load ($W$): $W = F_1 + F_2 = 10.0\text{ lbf}$
* Shaft Speed ($N$): $1500\text{ rpm}$
* Shaft Diameter ($d_{\text{shaft}}$): $0.375\text{ in}$
* Bearing Length ($L_b$): $0.50\text{ in}$
* Unknowns: Projected Bearing Area ($A_b$), Bearing Pressure ($P$), Linear Velocity ($V$), $PV$ Rating, Selected Material

### 3b. FBD of Journal Bearing

* Downward radial load $W$ balanced by uniform contact bearing pressure distributed over projected area ($d_{\text{shaft}} \times L_b$).

---

### 3c. Symbolic Model

1. **Projected Area:**

   $$A_b = d_{\text{shaft}} \cdot L_b$$

2. **Bearing Pressure:**

   $$P = \frac{W}{d_{\text{shaft}} \cdot L_b}$$

3. **Linear Surface Speed:**

   $$V = \frac{\pi \cdot d_{\text{shaft}} \cdot N}{12} \quad [\text{ft/min}]$$

4. **PV Rating:**

   $$PV = P \times V$$

---

### 3d. Numerical Solution

1. **Bearing Pressure:**

   $$P = \frac{10.0\text{ lbf}}{0.375\text{ in} \times 0.50\text{ in}} = 53.33\text{ psi}$$

2. **Linear Velocity:**

   $$V = \frac{\pi \times 0.375\text{ in} \times 1500\text{ rpm}}{12} = 147.26\text{ ft/min}$$

3. **PV Rating:**

   $$PV = 53.33\text{ psi} \times 147.26\text{ ft/min} = 7,853\text{ psi}\cdot\text{ft/min}$$

---

### 3e. Material Selection
* Referenced Source: Machinery's Handbook (Table 4, pg 2440)
* Selected Material: Sintered Bronze (SAE 841)
* Material Limits: Max $P = 2,000\text{ psi}$, Max $V = 1,200\text{ ft/min}$, Max $PV = 50,000\text{ psi}\cdot\text{ft/min}$
* Evaluation: Operating $PV$ of $7,853\text{ psi}\cdot\text{ft/min}$ is well within allowable bounds, satisfying the $SF \ge 3.0$ requirement.

---

## 4. Frame Support Design 

<p align="center">
  <img src=Belt_Sweep.jpg alt="Frame CAD Diagram" width="80%"/>
</p>

* Frame Concept: A-Frame structural assembly constructed from 6061-T6 aluminum maintaining fixed 5.0 in center-to-center shaft alignment.
* Stress Analysis: Bending stress on support uprights under maximum radial load ($10.0\text{ lbf}$) yields max stress under 120 psi, far below the 35,000 psi yield strength of 6061-T6 aluminum.

---

## 5. Engineering Lessons Learned 

1. Quantifying how initial pre-tension ($T_0$) directly drives radial bearing loads demonstrated the critical trade-off between belt torque capacity and journal bearing frictional heating ($PV$ rating).
2. Sizing components based on standard stock catalog dimensions (McMaster-Carr) highlighted the necessity of rounding calculated minimums (e.g., $d_{\text{belt}} = 0.246\text{ in}$ up to stock $0.250\text{ in}$) early to maintain manufacturing feasibility.
3. Evaluating non-slip friction dynamics underscored how wrap angle and friction coefficient govern maximum load capacity before slippage occurs.

---

## Process Documentation & Process Log

* Start Time: 4:00 PM
* Completion Time: 11:15 PM
* Actual Total Execution Time: 7.25 Hours
* Errors Corrected: Initial calculation of shaft torque omitted pulley radius; recalculated with $R = 1.5\text{ in}$ to achieve correct torsional stress values.
