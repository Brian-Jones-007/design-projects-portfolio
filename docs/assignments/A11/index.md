# A11 – Motor Selection

*Team Section Contributions:*
- *Calculations / Free Body Diagrams:* Brian Jones
- *Parametric Model (CAD):* Antonio Goodson
- *FEA Analysis:* Jarek Hernandez Acaba

## Objective

Develop the motorized drive and structural integration for a 2 kN dual-leadscrew bench-top tensile tester building directly upon the A10 translating crosshead system. Specifically, this analysis covers motor selection, motor mount bracket design, shaft-to-leadscrew coupler sizing, heavy 40 Series T-slot frame twisting analysis, and floating bearing tolerances while maintaining a minimum structural Design Safety Factor ($N_{sf}$) of 4 across a linear crosshead speed range of 1 mm/min to 50 mm/min.

---

## Section 1: Motor Torque & Speed Selection - Brian Jones

## Task 1.1 — Motor Rotational Speed Range ($\omega$ / RPM)

- *a. Knowns and Unknowns:*
  - *Knowns:*
    - Linear crosshead speed range ($v$): $1\text{ mm/min}$ to $50\text{ mm/min}$
    - Selected lead screw from A10: $1/2\text{"-10}$ Single-Start Acme Lead Screw ([McMaster-Carr 98935A823](http://www.mcmaster.com/98935A823))
    - Screw lead ($L$): $2.54\text{ mm/rev}$ ($0.100\text{ in/rev}$)
  - *Unknowns:*
    - Minimum motor speed ($RPM_{min}$)
    - Maximum motor speed ($RPM_{max}$)

- *b. Free Body Diagram (FBD):*
  *(Insert image: `![Rotational Speed Diagram](fbd_a11_motor_speed.JPG)`)*

- *c. Symbolic Solution:*
  $$RPM_{min} = \frac{v_{min}}{L}, \quad RPM_{max} = \frac{v_{max}}{L}$$

- *d. Numerical Solution:*
  $$RPM_{min} = \frac{1\text{ mm/min}}{2.54\text{ mm/rev}} = 0.394\text{ RPM}$$
  $$RPM_{max} = \frac{50\text{ mm/min}}{2.54\text{ mm/rev}} = 19.69\text{ RPM}$$

## Task 1.2 — Required Driving & Motor Output Torque ($T_{motor}$)

- *a. Knowns and Unknowns:*
  - *Knowns:*
    - Total tensile load ($F_{total}$): $2000\text{ N}$ ($1000\text{ N}$ per lead screw, $F_{screw}$)
    - Lead screw major diameter ($d$): $12.7\text{ mm}$ ($0.500\text{ in}$)
    - Lead screw mean diameter ($d_m$): $11.43\text{ mm}$ ($0.01143\text{ m}$)
    - Lead ($L$): $2.54\text{ mm}$ ($0.00254\text{ m}$)
    - Thread angle ($\alpha$, Acme $29^\circ$): $14.5^\circ \implies \sec\alpha = 1.033$
    - Coefficient of friction ($\mu$, AISI 1018 Steel on 932 Bearing Bronze): $0.15$
    - Required screw driving torque from A10 ($T_{screw}$): $1.305\text{ N}\cdot\text{m}$ ($11.55\text{ in-lbf}$)
    - Thrust collar / coupling efficiency ($\eta_{mech}$): $0.90$ (accounting for thrust bearing & coupler losses)
  - *Unknowns:*
    - Required continuous motor output torque per screw ($T_{motor}$)
    - Mechanical power required at max speed ($P_{max}$)

- *b. Free Body Diagram (FBD):*
  *(Insert image: `![Motor Torque FBD](fbd_a11_motor_torque.JPG)`)*

- *c. Symbolic Solution:*
  $$T_{screw} = \frac{F_{screw} \cdot d_m}{2} \left( \frac{L + \pi \mu d_m \sec\alpha}{\pi d_m - \mu L \sec\alpha} \right)$$
  $$T_{motor} = \frac{T_{screw}}{\eta_{mech}}$$
  $$P_{max} = T_{motor} \cdot \omega_{max} = T_{motor} \cdot \left( \frac{2\pi \cdot RPM_{max}}{60} \right)$$

- *d. Numerical Solution:*
  $$T_{screw} = \frac{1000 \cdot 0.01143}{2} \left[ \frac{0.00254 + \pi(0.15)(0.01143)(1.033)}{\pi(0.01143) - (0.15)(0.00254)(1.033)} \right] = 1.305\text{ N}\cdot\text{m}$$
  $$T_{motor} = \frac{1.305\text{ N}\cdot\text{m}}{0.90} = 1.45\text{ N}\cdot\text{m} \quad (12.83\text{ in-lbf})$$
  $$P_{max} = 1.45\text{ N}\cdot\text{m} \cdot \left( \frac{2\pi \cdot 19.69}{60} \right) = 2.99\text{ W}$$

## Task 1.3 — Motor Selection & Speed-Torque Verification

- *Selected Component:* High-Torque Planetary Geared Stepper / DC Gearmotor (24V DC, 100:1 Planetary Gearbox, rated continuous torque $\ge 2.0\text{ N}\cdot\text{m}$, speed range $0\text{–}30\text{ RPM}$, $8\text{ mm}$ keyed output shaft, NEMA 23 / 4-bolt flange).
- *Verification:* At the maximum crosshead speed of $50\text{ mm/min}$ ($19.69\text{ RPM}$), the required torque of $1.45\text{ N}\cdot\text{m}$ sits safely inside the continuous duty envelope of the selected gearmotor.
- *(Insert annotated Speed-Torque Curve plot here: `![Speed Torque Curve](speed_torque_curve.png)`)*

---

## Section 2: Motor Mount Bracket Bearing Stress Analysis - Brian Jones

## Task 2.1 — Minimum Bracket Plate Thickness ($t_{min}$)

- *a. Knowns and Unknowns:*
  - *Knowns:*
    - Motor reaction torque ($T_{motor}$): $1.45\text{ N}\cdot\text{m}$
    - Motor flange bolt pattern: $n_m = 4\text{ bolts}$ on a bolt circle diameter $D_{bc} = 66.68\text{ mm}$ (NEMA 23 standard, radius $r = 33.34\text{ mm} = 0.03334\text{ m}$)
    - Mounting bolt diameter ($d_b$, M5 / #10-32): $5.0\text{ mm}$ ($0.005\text{ m}$)
    - Frame mounting pattern: $4\text{ bolts}$ (M6, $d_{b,frame} = 6.0\text{ mm}$) on a wider $80\text{ mm} \times 80\text{ mm}$ pattern ($r_{frame} = 56.57\text{ mm}$) — motor bolt circle is the governing critical case due to smaller radius and smaller bolt diameter
    - Bracket Material (Aluminum 6061-T6, matching A10 crosshead): $S_y = 276\text{ MPa}$ ($276 \times 10^6\text{ Pa}$)
    - Safety Factor ($N_{sf}$): $4$
  - *Unknowns:*
    - Allowable bearing stress ($\sigma_{b,allow}$)
    - Tangential shear force per bolt ($F_b$)
    - Minimum required bracket thickness ($t_{min}$)

- *b. Free Body Diagram (FBD):*
  *(Insert image: `![Motor Mount Bearing Stress FBD](fbd_a11_mount_bearing.JPG)`)*

- *c. Symbolic Solution:*
  $$\sigma_{b,allow} = \frac{S_y}{N_{sf}}$$
  $$F_b = \frac{T_{motor}}{n_m \cdot r}$$
  $$\sigma_b = \frac{F_b}{d_b \cdot t} \le \sigma_{b,allow} \implies t_{min} = \frac{F_b \cdot N_{sf}}{d_b \cdot S_y}$$

- *d. Numerical Solution:*
  $$\sigma_{b,allow} = \frac{276 \times 10^6\text{ Pa}}{4} = 69\text{ MPa}$$
  $$F_b = \frac{1.45\text{ N}\cdot\text{m}}{4 \cdot 0.03334\text{ m}} = 10.87\text{ N}$$
  $$t_{min} = \frac{10.87\text{ N} \cdot 4}{(0.005\text{ m})(276 \times 10^6\text{ Pa})} = 3.15 \times 10^{-5}\text{ m} = 0.0315\text{ mm}$$
  *(Note: If the $1000\text{ N}$ axial thrust load is carried directly through the bracket flange rather than a separate frame thrust bearing, $F_{bolt,axial} = 1000\text{ N} / 4 = 250\text{ N}$).*
- *Selected Thickness:* To prevent bending deflection, maintain concentric alignment with the lead screw, and accommodate counterbored fasteners, a standard **$6.35\text{ mm}$ ($0.25\text{ in}$) Aluminum 6061-T6** plate is selected ($N_{sf} \gg 4$).

---

## Section 3: Shaft-to-Leadscrew Coupler Torsion Analysis - Brian Jones

## Task 3.1 — Coupler Inner Diameter, Outer Diameter & Wall Thickness

- *a. Knowns and Unknowns:*
  - *Knowns:*
    - Transmitted torque ($T_{motor}$): $1.45\text{ N}\cdot\text{m}$
    - Motor output shaft diameter ($d_{motor}$): $8.0\text{ mm}$ ($0.008\text{ m}$)
    - Turned lead screw end-journal diameter ($d_{journal}$): $8.0\text{ mm}$ ($0.008\text{ m}$, turned down from $9.75\text{ mm}$ minor diameter of the $1/2\text{"-10}$ Acme screw)
    - Coupler Material (Aluminum 6061-T6): $S_y = 276\text{ MPa}$
    - Shear Yield Strength ($S_{sy}$, Distortion Energy Theory): $0.577 S_y = 159.25\text{ MPa}$
    - Safety Factor ($N_{sf}$): $4$
  - *Unknowns:*
    - Allowable torsional shear stress ($\tau_{allow}$)
    - Coupler inner diameter ($d_i$)
    - Minimum outer diameter ($d_{o,min}$) and minimum wall thickness ($t_{min}$)

- *b. Free Body Diagram (FBD):*
  *(Insert image: `![Coupler Torsional Stress FBD](fbd_a11_coupler_torsion.JPG)`)*

- *c. Symbolic Solution:*
  $$\tau_{allow} = \frac{S_{sy}}{N_{sf}} = \frac{0.577 S_y}{N_{sf}}$$
  $$J = \frac{\pi}{32}(d_o^4 - d_i^4)$$
  $$\tau_{max} = \frac{T_{motor} \cdot (d_o / 2)}{J} = \frac{16 T_{motor} d_o}{\pi (d_o^4 - d_i^4)} \le \tau_{allow}$$
  $$t_{coupler} = \frac{d_o - d_i}{2}$$

- *d. Numerical Solution:*
  $$\tau_{allow} = \frac{159.25\text{ MPa}}{4} = 39.81\text{ MPa} \quad (39.81 \times 10^6\text{ Pa})$$
  Setting $d_i = 8.0\text{ mm} = 0.008\text{ m}$ and solving numerically for $d_o$: 
  $$\frac{16 (1.45\text{ N}\cdot\text{m}) d_o}{\pi (d_o^4 - 0.008^4)} = 39.81 \times 10^6\text{ Pa} \implies d_{o,min} = 8.69\text{ mm}$$
  $$t_{min} = \frac{8.69\text{ mm} - 8.00\text{ mm}}{2} = 0.346\text{ mm}$$
- *Selected Component:* A standard rigid/beam shaft coupling with **$8\text{ mm}$ bore ($d_i = 8.0\text{ mm}$)** and **$20\text{ mm}$ outer diameter ($d_o = 20.0\text{ mm}$, wall thickness $t = 6.0\text{ mm}$)** is selected to accommodate set-screw/clamping hardware and prevent torsional windup.
  $$\tau_{actual} = \frac{16 (1.45)(0.020)}{\pi (0.020^4 - 0.008^4)} = 0.947\text{ MPa} \ll 39.81\text{ MPa} \quad (N_{sf} = 168)$$

---

## Section 4: Frame Twisting Analysis & Corner Bracket Selection - Brian Jones

## Task 4.1 — Top Frame Twist Equilibrium & Bracket Sizing

- *a. Knowns and Unknowns:*
  - *Knowns:*
    - Dual axial lead screw loads ($F_{screw}$): $1000\text{ N}$ upward per screw ($2000\text{ N}$ total)
    - Dual motor reaction torques ($T_{motor}$): $1.45\text{ N}\cdot\text{m}$ each (conservative worst-case: both motors rotate in the same direction, so torques sum to $T_{twist,total} = 2 T_{motor} = 2.90\text{ N}\cdot\text{m}$)
    - Top horizontal frame span ($L_{frame}$, matching A10 crosshead/frame width): $300\text{ mm}$ ($0.300\text{ m}$)
    - Lead screw spacing ($L_b$, from A10): $200\text{ mm}$ ($0.200\text{ m}$, located $50\text{ mm}$ inboard from each vertical column)
    - Frame profile: Heavy 40 Series T-Slot Aluminum Extrusion ($40\text{ mm} \times 40\text{ mm}$, 6105-T5 Aluminum, $S_y = 241\text{ MPa}$, width $w = 0.040\text{ m}$)
    - Safety Factor ($N_{sf}$): $4$
  - *Unknowns:*
    - Vertical reaction forces at each corner joint ($R_z$)
    - Stabilizing twisting moment per end ($M_{twist}$) and transverse couple forces ($R_y$)
    - Required bracket load rating

- *b. Free Body Diagram (FBD):*
  *(Insert image: `![Top Frame Twisting FBD](fbd_a11_frame_twist.JPG)`)*

- *c. Symbolic Solution:*
  $$\sum F_z = 0 \implies R_{z,left} + R_{z,right} - 2 F_{screw} = 0 \implies R_z = F_{screw}$$
  $$\sum M_x = 0 \implies M_{twist,left} + M_{twist,right} - 2 T_{motor} = 0 \implies M_{twist} = T_{motor}$$
  Representing the stabilizing twist moment as a force couple across the $40\text{ mm}$ extrusion width ($w$):
  $$R_y = \frac{M_{twist}}{w} = \frac{T_{motor}}{w}$$

- *d. Numerical Solution:*
  $$R_z = 1000\text{ N} \quad (\text{design load with } N_{sf}=4: 4000\text{ N})$$
  $$M_{twist} = 1.45\text{ N}\cdot\text{m} \quad (\text{design moment with } N_{sf}=4: 5.80\text{ N}\cdot\text{m})$$
  $$R_y = \frac{1.45\text{ N}\cdot\text{m}}{0.040\text{ m}} = 36.25\text{ N} \quad (\text{design transverse force with } N_{sf}=4: 145\text{ N})$$
- *Bracket Selection:* Using the [Connectors for T-Slot Framing](https://www.google.com/url?q=https://www.grainger.com/category/material-handling/structural-framing-systems/t-slot-framing-systems/connectors-for-t-slot-framing?categoryIndex%3D3&sa=D&source=editors&ust=1794660370632009&usg=AOvVaw1jPTIGUgKcF462TtNc4MwI) catalog, **40 Series Heavy-Duty Cast Aluminum Corner Gussets (4-hole, M8 T-nuts)** are selected for all top-to-vertical joints. Standard 40 Series gussets rate for $> 4500\text{ N}$ direct shear and $> 85\text{ N}\cdot\text{m}$ moment capacity, satisfying $N_{sf} > 4$.

---

## Section 5: Analytical Euler Column Buckling Check - Brian Jones

## Task 5.1 — Vertical Standoff Buckling Verification

- *a. Knowns and Unknowns:*
  - *Knowns:*
    - Column length ($H$): $500\text{ mm}$ ($0.500\text{ m}$, accommodating $18\text{ in} = 457.2\text{ mm}$ minimum lead screw length)
    - Axial compressive load per column ($P$): $1000\text{ N}$
    - Heavy 40 Series ($40\text{ mm} \times 40\text{ mm}$) Moment of Inertia ($I$): $10.2 \times 10^{-8}\text{ m}^4$ ($10.2\text{ cm}^4$)
    - Modulus of Elasticity ($E$, 6105-T5 Aluminum): $68.9\text{ GPa}$ ($68.9 \times 10^9\text{ Pa}$)
    - End-condition factor ($K$, pinned-pinned conservative): $1.0$
  - *Unknowns:*
    - Critical Euler buckling load ($P_{cr}$)
    - Buckling Safety Factor ($N_{sf,buckling}$)

- *b. Free Body Diagram (FBD):*
  *(Insert image: `![Vertical Column Buckling FBD](fbd_a11_column_buckling.JPG)`)*

- *c. Symbolic Solution:*
  $$P_{cr} = \frac{\pi^2 E I}{(K H)^2}$$
  $$N_{sf,buckling} = \frac{P_{cr}}{P}$$

- *d. Numerical Solution:*
  $$P_{cr} = \frac{\pi^2 (68.9 \times 10^9\text{ Pa})(10.2 \times 10^{-8}\text{ m}^4)}{(1.0 \cdot 0.500\text{ m})^2} = 277,450\text{ N} \quad (277.5\text{ kN})$$
  $$N_{sf,buckling} = \frac{277,450\text{ N}}{1000\text{ N}} = 277.5 \gg 4$$

---

## Section 6: Floating Bearing Mount Fit & Tolerances - Brian Jones

## Task 6.1 — RC3 Running Fit & Machinery's Handbook Clearance Holes

- *a. Knowns and Unknowns:*
  - *Knowns:*
    - Nominal lead screw end-journal diameter ($d_{nominal}$): $8.000\text{ mm}$ ($0.3150\text{ in}$, turned end journal of the $1/2\text{"-10}$ Acme screw)
    - Required fit class: **RC3 (Close Running Fit)** per *Machinery's Handbook* ($0.24\text{–}0.40\text{ in}$ / $6\text{–}10\text{ mm}$ basic size range)
    - Frame mounting fasteners: M6 socket head cap screws
  - *Unknowns:*
    - Bearing bore upper and lower limits
    - Lead screw shaft journal upper and lower limits
    - Resulting diametral clearance range and M6 clearance hole dimensions

- *b. Free Body Diagram (FBD):*
  *(Insert image: `![Floating Bearing Fit Diagram](fbd_a11_floating_bearing.JPG)`)*

- *c. Symbolic & Tolerance Limits Solution:*
  - **Bearing Bore (Hole, H7):** $+0.000\text{ mm} / +0.015\text{ mm} \implies \mathbf{8.000\text{ mm}\text{ to }8.015\text{ mm}}$
  - **Lead Screw Journal (Shaft, f6):** $-0.013\text{ mm} / -0.022\text{ mm} \implies \mathbf{7.978\text{ mm}\text{ to }7.987\text{ mm}}$
  - **Minimum Clearance:** $8.000\text{ mm} - 7.987\text{ mm} = \mathbf{0.013\text{ mm}}$
  - **Maximum Clearance:** $8.015\text{ mm} - 7.978\text{ mm} = \mathbf{0.037\text{ mm}}$
  - **M6 Mounting Bolt Clearance Holes (per Machinery's Handbook):** **$6.4\text{ mm}$** (close fit) or **$6.6\text{ mm}$** (normal fit) thru-hole, with a $10.5\text{ mm}$ counterbore if socket head cap screws are recessed.

---

## Section 7: Parametrically Model (CAD) - Antonio Goodson

## Section 8: Top Frame FEA Verification - Jarek Hernandez Acaba

## Section 9: Vertical Standoff FEA Verification - Jarek Hernandez Acaba

## Bill of Materials & Engineering Cost

## Lessons Learned

## CAD FILES - Antonio Goodson & Jarek Hernandez
