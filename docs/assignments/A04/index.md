# A4: Motor Mount

---

## Process Overview & Documentation
* **Material Selected**: PLA ($\sigma_y \approx 50\text{ MPa}$, $E \approx 3.5\text{ GPa}$)
* **Design Load ($P$)**: 300 N applied at motor shaft free end
* **Safety Factor ($n$)**: 3
* **Max Deflection Limit ($\delta_{\text{max}}$)**: 0.30 mm ($3.0 \times 10^{-4}\text{ m}$)
* **Total Project Time**: 7.5 Hours

---

## Feature 1

### Knowns & Unknowns
* **Known Parameters**:
  * Applied Shaft Load ($P$): 300 N
  * Safety Factor ($n$): 3
  * Maximum Allowable Deflection ($\delta_{\text{max}}$): 0.30 mm
  * Material (PLA): Yield Strength $\sigma_y = 50\text{ MPa}$, Elastic Modulus $E = 3.5\text{ GPa}$
  * Allowable Design Stress ($\sigma_{\text{allow}}$):
    $\sigma_{\text{allow}} = \sigma_y / n = 50\text{ MPa} / 3 \approx 16.67\text{ MPa}$
  * Feature Length ($L_1$): 30 mm (0.03 m)
  * Feature Width ($b_1$): 30 mm (0.03 m)
* **Unknowns**:
  * Cross-sectional thickness ($h_1$) required to satisfy yield strength and deflection criteria.

### Free Body Diagram (FBD)
![Feature 1 FBD](./fbd_feature1.png)
*Figure 1: Free body diagram of Feature 1 modeled as a cantilever beam with fixed reaction moment $M_{\text{max}} = P \times L_1$ and vertical reaction force $R_y = P = 300\text{ N}$.*

### Symbolic Modeling
1. **Section Moment of Inertia**:
   $I_1 = (b_1 \times h_1^3) / 12$

2. **Bending Stress Equation**:
   $M_{\text{max}} = P \times L_1$
   $\sigma_{\text{max}} = (M_{\text{max}} \times (h_1 / 2)) / I_1 = (6 \times P \times L_1) / (b_1 \times h_1^2)$
   $\sigma_{\text{max}} \le \sigma_{\text{allow}} \implies (b_1 \times h_1^2) \ge (18 \times P \times L_1) / \sigma_y$

3. **Deflection Equation**:
   $\delta_{\text{max}} = (P \times L_1^3) / (3 \times E \times I_1) = (4 \times P \times L_1^3) / (E \times b_1 \times h_1^3)$
   $\delta_{\text{max}} \le 0.30\text{ mm} \implies (b_1 \times h_1^3) \ge (4 \times P \times L_1^3) / (E \times \delta_{\text{allow}})$

### Numerical Solution
* **Yield Stress Constraint**:
  $b_1 \times h_1^2 \ge (18 \times 300 \times 0.03) / (50 \times 10^6) = 3.24 \times 10^{-6}\text{ m}^3$
  $h_1 \ge \sqrt{(3.24 \times 10^{-6}) / 0.03} = 0.0104\text{ m} = \mathbf{10.4\text{ mm}}$

* **Deflection Constraint**:
  $b_1 \times h_1^3 \ge (4 \times 300 \times 0.03^3) / ((3.5 \times 10^9) \times (3.0 \times 10^{-4})) = 3.086 \times 10^{-8}\text{ m}^4$
  $h_1 \ge \sqrt[3]{(3.086 \times 10^{-8}) / 0.03} = 0.0101\text{ m} = \mathbf{10.1\text{ mm}}$

* **Governing Dimension**: Yield stress governs (10.4 mm > 10.1 mm). 
* **Final Chosen Dimension**: **$h_1 = 11\text{ mm}$**

---

## Feature 2

### Knowns & Unknowns
* **Known Parameters**:
  * Transferred Shaft Load ($P$): 300 N
  * Material (PLA): $E = 3.5\text{ GPa}$, $\sigma_y = 50\text{ MPa}$, $n = 3$, $\sigma_{\text{allow}} = 16.67\text{ MPa}$
  * Allowable Deflection ($\delta_{\text{max}}$): 0.30 mm
  * Wall Attachment Interface: M3 bolts into Rigid Wall A (3.4 mm clearance holes)
  * Feature Length ($L_2$): 40 mm (0.04 m)
  * Feature Width ($b_2$): 40 mm (0.04 m)
* **Unknowns**:
  * Wall plate thickness ($h_2$) to satisfy yield strength and deflection limits.

### Free Body Diagram (FBD)
![Feature 2 FBD](./fbd_feature2.png)
*Figure 2: Free body diagram of Feature 2 attaching to Rigid Wall A showing internal shear force $V(x)$ and bending moment $M(x)$ distribution.*

### Symbolic Modeling
1. **Section Moment of Inertia**:
   $I_2 = (b_2 \times h_2^3) / 12$

2. **Bending Stress Equation**:
   $\sigma_{\text{max}} = (6 \times P \times L_2) / (b_2 \times h_2^2) \le \sigma_{\text{allow}} \implies (b_2 \times h_2^2) \ge (18 \times P \times L_2) / \sigma_y$

3. **Deflection Equation**:
   $\delta_{\text{max}} = (4 \times P \times L_2^3) / (E \times b_2 \times h_2^3) \le \delta_{\text{allow}} \implies (b_2 \times h_2^3) \ge (4 \times P \times L_2^3) / (E \times \delta_{\text{allow}})$

### Numerical Solution
* **Yield Stress Constraint**:
  $b_2 \times h_2^2 \ge (18 \times 300 \times 0.04) / (50 \times 10^6) = 4.32 \times 10^{-6}\text{ m}^3 \implies h_2 \ge \mathbf{10.4\text{ mm}}$

* **Deflection Constraint**:
  $b_2 \times h_2^3 \ge (4 \times 300 \times 0.04^3) / ((3.5 \times 10^9) \times (3.0 \times 10^{-4})) = 7.314 \times 10^{-8}\text{ m}^4 \implies h_2 \ge \mathbf{12.2\text{ mm}}$

* **Governing Dimension**: Deflection governs (12.2 mm > 10.4 mm).
* **Final Chosen Dimension**: **$h_2 = 13\text{ mm}$**

---

## Sketch

Below is the isometric hand sketch showing the fully dimensioned motor mount assembly incorporating the four M3 motor plate holes, central motor shaft clearance hole, four M3 wall attachment clearance holes, and a deflection-minimizing corner fillet/gusset.

![Isometric Concept Sketch](./isometric_hand_sketch.png)
*Figure 3: Isometric hand sketch with explicit dimension callouts ($b_1 = 30\text{ mm}$, $h_1 = 11\text{ mm}$, $b_2 = 40\text{ mm}$, $h_2 = 13\text{ mm}$, $L_2 = 40\text{ mm}$).*

---

## CAD Model (Parametric)

The 3D model was constructed parametrically in Fusion 360 using driving global parameters to ensure automatic geometry updates.

### Parametric Table
| Parameter Name | Expression | Description |
| :--- | :--- | :--- |
| `b1` | `30 mm` | Feature 1 Width |
| `h1` | `11 mm` | Feature 1 Thickness |
| `L1` | `30 mm` | Feature 1 Length |
| `b2` | `40 mm` | Feature 2 Width |
| `h2` | `13 mm` | Feature 2 Thickness |
| `L2` | `40 mm` | Feature 2 Length |
| `hole_wall` | `3.4 mm` | Wall M3 Bolt Clearance |
| `hole_motor` | `3.4 mm` | Motor Plate M3 Bolt Clearance |
| `hole_shaft` | `6.0 mm` | Center Shaft Clearance |
| `bc_diameter`| `22.0 mm`| Motor Mounting Bolt Circle |
| `fillet_rad` | `3.0 mm` | Stiffening Fillet Radius |

### CAD Renders & Features
To minimize cantilever deflection under the 300 N shaft load, a 3 mm fillet and a central structural rib were modeled at the internal 90° junction.

![Fusion 360 Model Render](./cad_render.png)
*Figure 4: Parametric CAD model rendered in Fusion 360 showing clearance holes and deflection-minimizing stiffening rib.*

### CAD File Download
* [Download Raw CAD File (.f3d)](./motor_mount.f3d)
* [Download STEP Model (.step)](./motor_mount.step)

---

## MEGR 2157 Multiview Drawing

The Third-Angle Projection engineering drawing sheet includes Front, Top, Right Side, and Shaded Isometric views, fully dimensioned per ASME standards with proper centerlines, center marks, and hole callouts.

![MEGR 2157 Multiview Drawing Sheet](./megr2157_multiview_drawing.png)
*Figure 5: ASME Third-Angle Projection multiview drawing generated in Fusion 360.*

* [Download Drawing Sheet PDF](./megr2157_multiview_drawing.pdf)

---

## Lessons Learned

### Key Takeaways
1. **Deflection vs. Stress Limits**: While yield stress governed the required thickness for Feature 1 (10.4 mm vs 10.1 mm), beam deflection governed Feature 2 (12.2 mm vs 10.4 mm) due to length scaling cubed ($\delta \propto L^3$).
2. **Parametric Modeling Discipline**: Defining a construction circle centered at the origin for the 22 mm bolt circle diameter allowed seamless parametric updates without breaking hole dependencies.
3. **Deflection Control**: Adding a structural rib drastically increases joint stiffness without adding excessive weight or material volume.

### Time Log
* **Analysis & Symbolic Math**: 2.5 Hours
* **FBDs & Hand Sketching**: 1.0 Hour
* **Fusion 360 Parametric CAD**: 2.0 Hours
* **MEGR 2157 Multiview Drawing**: 1.0 Hour
* **Portfolio Documentation**: 1.0 Hour
* **Total Time**: **7.5 Hours**

---

## Appendix

### Reference Research Links
* [Servocity Planetary Gear Motor Mount Options](https://www.servocity.com)
* [McMaster-Carr Bracket Structural Fastener Design Guide](https://www.mcmaster.com)
* [Beam Deflection Tables – Machinery's Handbook (p. 256 - 274)](https://instructure.charlotte.edu/courses/272052/assignments/2902671)
