# A6 – Bracket Drawing (Parametric CAD & Engineering Drawings)

## Objective
The objective of this assignment is to translate the strength and stiffness analysis from the previous assignment into a fully parametric 3D CAD model and generate a detailed, multiview engineering drawing. The bracket model incorporates dynamic parameters for all features (A–E) and reflects the revised T-beam base geometry (b_f = 1.250 in, t_f = 0.200 in, h_s = 0.800 in, t_s = 0.200 in) with three designated sliding fit tolerance gaps.

---

## Material Properties & Design Limits
* **Material:** Aluminum 6061-T6
* **Yield Strength (S_y):** 40,000 psi (40 ksi)
* **Modulus of Elasticity (E):** 10.0 x 10^6 psi (10.0 Mpsi)
* **Safety Factor (SF):** 4.0
* **Applied Load (F):** 600 lbf
* **Symmetric Side Load (P_D = P_E):** 300 lbf
* **Allowable Stress (Sigma_allow):**
  Sigma_allow = S_y / SF = 40,000 psi / 4 = 10,000 psi
* **Maximum Allowable Deflection (Delta_max):** 0.005 in

---

## Process Overview

### 1. Parametric CAD Modeling (Fusion 360)
Every feature dimension was defined dynamically using expressions in the **Parameters Table** (`Modify > Change Parameters`). Linking sketch dimensions directly to parameters ensures the CAD geometry updates automatically if design criteria change.

#### Global & Derived User Parameters
| Parameter Name | Expression | Unit | Evaluated Value | Description |
| :--- | :--- | :--- | :--- | :--- |
| `F` | `600` | `lbf` | 600 lbf | Total applied strap load |
| `S_y` | `40000` | `psi` | 40,000 psi | Yield strength (6061-T6) |
| `SF` | `4.0` | `ul` | 4.0 | Safety factor |
| `Sigma_allow` | `S_y / SF` | `psi` | 10,000 psi | Allowable design stress |
| `E` | `10000000` | `psi` | 10,000,000 psi | Young's Modulus |
| `Delta_max` | `0.005` | `in` | 0.005 in | Deflection limit |
| `L_A` | `3.0` | `in` | 3.000 in | Feature A span length |
| `M_max_A` | `(F * L_A) / 8` | `in*lbf` | 225 in-lbf | Feature A max bending moment |
| `r_A` | `(((4 * M_max_A) / (PI * Sigma_allow)) / 1 in^3)^(1/3) * 1 in` | `in` | 0.306 in | Radius derived from stress |
| `D_A` | `2 * r_A` | `in` | 0.612 in | Support cylinder outer diameter |
| `L_B` | `2.0` | `in` | 2.000 in | Extension link length |
| `w_B` | `D_A` | `in` | 0.612 in | Link width (matched flush to D_A) |
| `t_B` | `0.250` | `in` | 0.250 in | Link thickness |
| `L_C` | `4.0` | `in` | 4.000 in | Cross-beam span width |
| `b_C` | `0.500` | `in` | 0.500 in | Cross-beam thickness |
| `M_max_C` | `(F * L_C) / 4` | `in*lbf` | 600 in-lbf | Feature C max bending moment |
| `Z_req_C` | `M_max_C / Sigma_allow` | `in^3` | 0.060 in^3 | Required section modulus |
| `h_C` | `((6 * Z_req_C) / b_C)^(1/2)` | `in` | 0.849 in | Cross-beam height |
| `P_D` | `F / 2` | `lbf` | 300 lbf | Symmetric side load |
| `L_D` | `2.50` | `in` | 2.500 in | Vertical support column height |
| `I_D_min` | `(P_D * (L_D^3)) / (3 * E * Delta_max)` | `in^4` | 0.03125 in^4 | Required moment of inertia |
| `t_D` | `0.350` | `in` | 0.350 in | Side wall thickness |
| `b_f` | `1.250` | `in` | 1.250 in | T-beam flange width |
| `t_f` | `0.200` | `in` | 0.200 in | T-beam flange thickness |
| `h_s` | `0.800` | `in` | 0.800 in | T-beam stem height |
| `t_s` | `0.200` | `in` | 0.200 in | T-beam stem thickness |

![Parametric Parameters Table](parameters_table_screenshot.png)

---

### 2. Parametric CAD Sketch Callouts
Each sketch feature in CAD uses the `fx:` dimension constraint referencing user parameters:

* **Feature A & B Sketch:** Diameter D_A = 0.612 in (`fx: D_A`), width w_B = 0.612 in (`fx: w_B`), length L_B = 2.000 in (`fx: L_B`).
* **Feature C Sketch:** Span L_C = 4.000 in (`fx: L_C`), height h_C = 0.849 in (`fx: h_C`).
* **Feature D Sketch:** Column height L_D = 2.500 in (`fx: L_D`), wall thickness t_D = 0.350 in (`fx: t_D`).
* **Feature E T-Section Sketch:** Flange b_f = 1.250 in, t_f = 0.200 in, stem h_s = 0.800 in, t_s = 0.200 in.

![Dimensioned Parametric Sketch](sketch_fx_dimensions.png)

---

### 3. Engineering Drawing Layout (ASME Standard)
A multiview drawing was generated using the **ASME (Inches)** standard on a Size B (11 x 17 in) sheet format:

1. **Third Angle Projection:** Includes Front, Top, Right Side, and Isometric views.
2. **Title Block & General Tolerance:** General title block tolerance specified as X.X +/- .02.
3. **Sliding Fit Tolerances:** Explicit tolerance callouts applied to the 3 T-beam interface gaps:
   * **Class a (Loose/Rough Fit):** +/- 0.0050 in
   * **Class b (Close Free-Running Fit):** +/- 0.0020 in
   * **Class c (Accurate Location / Min Play):** +/- 0.0008 in

![Multiview Engineering Drawing](engineering_drawing_export.png)

---

## CAD Download Link
* [Download Native Fusion 360 CAD File (.f3d)](INSERT_YOUR_PUBLIC_CLOUD_LINK_HERE)

---

## Mistakes & Design Iterations

1. **Parameter Unit Misclassification:**
   * *Issue:* `M_max_A` was initially saved with its unit set to `Text` in Fusion 360. This caused formula syntax errors (`fx:` red underline) when calculating r_A.
   * *Correction:* Since Fusion 360 locks unit types once created, the text parameter was deleted and recreated with explicit units of `in*lbf`.

2. **Exponent & Unit Stripping Syntax:**
   * *Issue:* Computing (0.75)^(1/3) inside `t_D` without explicit physical unit dimensions resulted in dimensional mismatches (0.91 in instead of 0.350 in).
   * *Correction:* Re-structured unit expressions to explicitly divide out unit terms (e.g., `/ 1 in^3`) prior to applying fractional powers (`^(1/3)`).

3. **Drawing Standard Selection:**
   * *Issue:* ANSI was not listed in the drawing setup dropdown.
   * *Correction:* Selected `ASME` (Inches), which governs US Imperial drafting practices and Y14.5 standard third-angle projections.

---

## Actual Time Spent
* **Parametric Modeling & Parameter Setup:** 2.5 hours
* **CAD Sketching & Feature Assembly:** 2.0 hours
* **Multiview ASME Drawing Layout & Tolerances:** 2.0 hours
* **Documentation & Notebook Setup:** 1.5 hours
* **Total Time:** **8.0 hours**

---

## Lessons Learned & Reflections

1. **Parameter Dependencies & Cascading Updates:**
   Setting up parameters with strict algebraic dependencies allowed the geometry to update cleanly. Linking Feature B width directly to Feature A diameter (w_B = D_A) maintained flush alignment automatically without requiring manual sketch edits.

2. **Handling Non-Convertible Parameter Types in CAD:**
   Fusion 360 prevents changing a parameter's unit type (e.g., `Text` to `in*lbf`) after creation. Deleting dependent expressions first is required before recreating locked parameters.

3. **Unit Consistency in Fractional Powers:**
   When using fractional powers like `^(1/3)` or `^(1/2)` in CAD parameter engines, units must balance algebraically within the parenthesis to prevent software unit conversion errors.
