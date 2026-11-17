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
  *![Rotational Speed Diagram](fbd_a11_motor_speed.JPG)*

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
  *![Motor Torque FBD](fbd_a11_motor_torque.JPG)*

- *c. Symbolic Solution:*
  $$T_{screw} = \frac{F_{screw} \cdot d_m}{2} \left( \frac{L + \pi \mu d_m \sec\alpha}{\pi d_m - \mu L \sec\alpha} \right)$$
  $$T_{motor} = \frac{T_{screw}}{\eta_{mech}}$$
  $$P_{max} = T_{motor} \cdot \omega_{max} = T_{motor} \cdot \left( \frac{2\pi \cdot RPM_{max}}{60} \right)$$

- *d. Numerical Solution:*
  $$T_{screw} = \frac{1000 \cdot 0.01143}{2} \left[ \frac{0.00254 + \pi(0.15)(0.01143)(1.033)}{\pi(0.01143) - (0.15)(0.00254)(1.033)} \right] = 1.305\text{ N}\cdot\text{m}$$
  $$T_{motor} = \frac{1.305\text{ N}\cdot\text{m}}{0.90} = 1.45\text{ N}\cdot\text{m} \quad (12.83\text{ in-lbf})$$
  $$P_{max} = 1.45\text{ N}\cdot\text{m} \cdot \left( \frac{2\pi \cdot 19.69}{60} \right) = 2.99\text{ W}$$

## Task 1.3 — Motor Selection & Speed-Torque Verification

- *Selected Component:* High-Torque Planetary Geared Stepper / DC Gearmotor (24V DC, 100:1 Planetary Gearbox, rated continuous torque $\ge 2.0\text{ N}\cdot\text{m}$, speed range $0\text{–}30\text{ RPM}$, $8\text{ mm}$ D-shaped output shaft, NEMA 23 / 4-bolt flange).
- *Verification:* At the maximum crosshead speed of $50\text{ mm/min}$ ($19.69\text{ RPM}$), the required torque of $1.45\text{ N}\cdot\text{m}$ sits safely inside the continuous duty envelope of the selected gearmotor.
- *(Insert annotated Speed-Torque Curve plot here: ![Speed Torque Curve](speed_torque_curve.png)*

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
  *![Motor Mount Bearing Stress FBD](fbd_a11_mount_bearing.JPG)*

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
    - Motor output shaft diameter ($d_{motor}$, D-shaped profile): $8.0\text{ mm}$ ($0.008\text{ m}$)
    - Turned lead screw end-journal diameter ($d_{journal}$): $8.0\text{ mm}$ ($0.008\text{ m}$, turned down from $9.75\text{ mm}$ minor diameter of the $1/2\text{"-10}$ Acme screw)
    - Coupler Material (Aluminum 6061-T6): $S_y = 276\text{ MPa}$
    - Shear Yield Strength ($S_{sy}$, Distortion Energy Theory): $0.577 S_y = 159.25\text{ MPa}$
    - Safety Factor ($N_{sf}$): $4$
  - *Unknowns:*
    - Allowable torsional shear stress ($\tau_{allow}$)
    - Coupler inner diameter ($d_i$)
    - Minimum outer diameter ($d_{o,min}$) and minimum wall thickness ($t_{min}$)

- *b. Free Body Diagram (FBD):*
  *![Coupler Torsional Stress FBD](fbd_a11_coupler_torsion.JPG)*

- *c. Symbolic Solution:*
  $\tau_{allow} = \frac{S_{sy}}{N_{sf}} = \frac{0.577 S_y}{N_{sf}}$
  $J = \frac{\pi}{32}(d_o^4 - d_i^4)$
  $\tau_{max} = \frac{T_{motor} \cdot (d_o / 2)}{J} = \frac{16 T_{motor} d_o}{\pi (d_o^4 - d_i^4)} \le \tau_{allow}$
  $t_{coupler} = \frac{d_o - d_i}{2}$

- *d. Numerical Solution:*
  $\tau_{allow} = \frac{159.25\text{ MPa}}{4} = 39.81\text{ MPa} \quad (39.81 \times 10^6\text{ Pa})$
  Setting $d_i = 8.0\text{ mm} = 0.008\text{ m}$ and solving numerically for $d_o$:
  $\frac{16 (1.45\text{ N}\cdot\text{m}) d_o}{\pi (d_o^4 - 0.008^4)} = 39.81 \times 10^6\text{ Pa} \implies d_{o,min} = 8.69\text{ mm}$
  $t_{min} = \frac{8.69\text{ mm} - 8.00\text{ mm}}{2} = 0.346\text{ mm}$
- *Selected Component:* A standard rigid shaft coupling with **$8\text{ mm}$ D-bore / set-screw clamping ($d_i = 8.0\text{ mm}$)** and **$20\text{ mm}$ outer diameter ($d_o = 20.0\text{ mm}$, wall thickness $t = 6.0\text{ mm}$)** is selected to engage the motor's D-shaft flat without slip or torsional windup.
  $\tau_{actual} = \frac{16 (1.45)(0.020)}{\pi (0.020^4 - 0.008^4)} = 0.947\text{ MPa} \ll 39.81\text{ MPa} \quad (N_{sf} = 168)$
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
  *![Top Frame Twisting FBD](fbd_a11_frame_twist.JPG)*

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
  *![Vertical Column Buckling FBD](fbd_a11_column_buckling.JPG)*

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
    - Frame mounting fasteners: M5 socket head cap screws
  - *Unknowns:*
    - Bearing bore upper and lower limits
    - Lead screw shaft journal upper and lower limits
    - Resulting diametral clearance range and M5 clearance hole dimensions

- *b. Free Body Diagram (FBD):*
  *![Floating Bearing Fit Diagram](fbd_a11_floating_bearing.JPG)*

- *c. Symbolic & Tolerance Limits Solution:*
  - **Bearing Bore (Hole, H7):** $+0.000\text{ mm} / +0.015\text{ mm} \implies \mathbf{8.000\text{ mm}\text{ to }8.015\text{ mm}}$
  - **Lead Screw Journal (Shaft, f6):** $-0.013\text{ mm} / -0.022\text{ mm} \implies \mathbf{7.978\text{ mm}\text{ to }7.987\text{ mm}}$
  - **Minimum Clearance:** $8.000\text{ mm} - 7.987\text{ mm} = \mathbf{0.013\text{ mm}}$
  - **Maximum Clearance:** $8.015\text{ mm} - 7.978\text{ mm} = \mathbf{0.037\text{ mm}}$
  - **M5 Mounting Bolt Clearance Holes (per Machinery's Handbook):** **$5.3\text{ mm}$** (close fit) or **$5.5\text{ mm}$** (normal fit) thru-hole, with a $9.2\text{ mm}$ counterbore if socket head cap screws are recessed.

---

## Section 7: Parametric Model (CAD) - Antonio Goodson


### Coupler Alignment

The first step of order I took was to insert the coupler and motor. The coupler that I chose was machinable on one side so I changed the inner diameter of that side to be 8 mm to fit the motor shaft. Now the coupler has one side being half an inch and the other 8 mm I was able to connect the motor to the lead screw.

<p align="center">
  <img src="Couple.png" alt="Couple" width="42%" />
  <img src="Motors.png" alt="Motors" width="41%" />
</p>


### Motor Mount

Now that I have a motor and coupler attached, I needed to create a mount for the motor so the coupler isn't being interfered with and the motor sits still. The first thing I did was create the necessary equations needed to do this. I looked at the dimensions of the coupler and motor to fit the NEMA 23 sizing.

<p align="center">
  <img src="Mount_Eq.png" alt="Mount_Eq" width="48%" />
</p>

With the equations, I then created an initial sketch for the parts connected to the motor; with this I matched the drawing in the motor listing. I also made sure to create the hole that the coupler would sit in. 

<p align="center">
  <img src="Mount_Sketch.png" alt="Mount_Sketch" width="38%" />
</p>

After creating the sketch, I went to extrude the necessary regions by the calculated thickness of .25 in, and excluded the coupler hole and M5 screw holes. After this, I made the inset where the motor would sit.

<p align="center">
  <img src="Couple_Hole.png" alt="Couple_Hole" width="44%" />
  <img src="Motor_Inset.png" alt="Motor_Inset" width="41%" />
</p>

 The next part was to extrude the stem. This was extruded by the height of the coupler with walls of the same thickness as the base.

<p align="center">
  <img src="Couple_Lenght.png" alt="Couple_Lenght" width="38%" />
</p>

Last step of the motor mounts was to create the part where it mounts to the frame for this I used the width of the frame which was 0.03 m and made it the length of 3 inches I once again used M5 screw holes and filled in the coupler hole with the exception of a hole needed for the lead screw for this hole I followed the book for a RC3 clearance hole. After doing this, I cleaned up the model.

<p align="center">
  <img src="Frame_connection.png" alt="Frame_connection" width="43%" />
  <img src="Cleanup.png" alt="Cleanup" width="37%" />
</p>

With the mount completed, I then added it to the assembly, including the M5 screws needed; the ones I chose had a length of 12 mm. While doing this, I decided to use the circular pattern to reduce work, which turned out to work perfectly.

<p align="center">
  <img src="Mount_attached.png" alt="Mount_attached" width="43%" />
</p>

### Lead Screw Bearing

With the mount completed, I sent the bearing to hold the lead screw in place at the bottom of the frame doing this. I created a list of equations so it helps this list of equations share similarities with the motor mount, as it connects to the same type of frame. I made sure to go by the book with a RC3 clearance for a diameter of 0.5 inches.

<p align="center">
  <img src="Bearing_Eq.png" alt="Bearing_Eq" width="56%" />
</p>

The steps are the same as the motor mount, where I create a sketch to map things out. The sketch is almost identical to the side of the motor mount where it connects to the frame.

<p align="center">
  <img src="Bearing_Sketch.png" alt="Bearing_Sketch" width="50%" />
</p>

I extrude the calculated thickness and a certain length to align the lead screw and then clean up the model. This is important as it prevents the lead screw from jumping out of its necessary place; it also prevents the crosshead from rotating.

<p align="center">
  <img src="Bearing_Shape.png" alt="Bearing_Shape" width="40%" />
  <img src="Cleanup2.png" alt="Cleanup2" width="43%" />
</p>

### Assembly

One of the first steps I took in the assembly was to create a hole for the lead screw, as seen in the following image. This is important as it provides a space where the lead screw can actually sit, like in the top frame where it goes through and the bottom frame where it has room to move. I then inserted the lead screw bearing and aligned it. After this, I added the necessary bolts to complete the assembly.
<p align="center">
  <img src="Clear_Hole.png" alt="Clear_Hole" width="44%" />
  <img src="Bearing_Fitted.png" alt="Bearing_Fitted" width="39%" />
</p>
<p align="center">
  <img src="Full_Assembly.png" alt="Full_Assembly" width="35%" />
</p>



## Section 8: Top Frame FEA Verification - Jarek Hernandez Acaba

To make sure that our top assembly could operate and function while holding the loads, FEAs were conducted on the bases along with the frame.

![fixed](A11FIXED.PNG)
![loads](A11LOADS.PNG)

Once the loads and fixed points were defined, I created a mesh at stress-raise locations to further study those critical areas. Once the mesh was created, I hit Study and analyzed stress, deflection, and safety factor.

### Coarse

![details](A11COARSEMESHDETAILS.PNG)
![disp](A11DISPCOARSE.PNG)
![stress](A11STRESSCOARSE.PNG)
![SF](A11SFCOARSE.PNG)
### Normal 
![details](A11MIDMESHDETAILS.PNG)
![disp](A11DISP.PNG)
![stress](A11STRESS.PNG)
![SF](A11SF.PNG)
### Fine
![details](A11FINEMESHDETAILS.PNG)
![disp](A11DISPFINE.PNG)
![stress](A11STRESSFINE.PNG)
![SF](A11SFFINE.PNG)

### Results to Report

| Mesh Level | Elements | Max Displacement (mm) | Max von Mises Stress (MPa) | Min Factor of Safety (SF) | % Change in Stress | % Change in Deflection |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Coarse** | 281,683 | 0.03675 | 9.93 | 28 | — | — |
| **Medium** | 915,361 | 0.03674 | 11.36 | 24 | +14.39% | -0.03% |
| **Fine** | 1,640,979 | 0.03673 | 10.98 | 25 | -3.35% | -0.03% |

Both motors have a 1,000N normal load applied symmetrically to their inner faces where the motor would sit on. The forces travel through the cylindrical slot where the shaft coupling would go, and the mounting flanges onto the tracks of the aluminium frame. Due to applied loads that are balanced and centered across the transverse axis of the frame, vertical flexural bending dominates the deformation profile. Torsion and lateral warping factors are so small that they are not accounted for. The model exaggerates the bending of the structure, but it's a good visual representation of what is happening inside the frame.

### Optimization Pass

Results from FEAs were well beyond the proposed limits, with FS ranging from 25 to 28 against a 250MPa yield strength. Given that the stress numbers were so low compared to the 250MPa yield strength, the structure was over-designed, which means that a good optimization pass to focus on was to reduce the mass of the mounts. This could be done by integrating the coupling into the frame. Critical locations for stresses currently concentrate at the shaft inner fillet transitions where the vertical motor mount columns meet the mounting base pads. 

## Section 9: Vertical Standoff FEA Verification - Jarek Hernandez Acaba
For this section, I had to analyze the structural assembly that included the two vertical columns, a crossbeam, lower base rails, and the mounts; the reason it was all included is that the forces are happening at the center where the mounts are located. The material for all of the pieces was an alloy with a yield strength of 2.7 * 10^8 N/m^2 (275 MPa). 

Before proceeding with the FEA, I had to define fixed boundaries and apply the loads, which consist of two loads: one being an axial compression of 1000 N each and a transverse load of 1.45 N*m  on each mount. 

### Fixed and loads

![fixed](A1FIXED.PNG)
![loads](A1TQ.PNG)


### Coarse 
![details1](A1COARSEMESHDETAILS.PNG)
![disp1](A1COARSEDISPFORCES.PNG)
![stress1](A1COARSESTRESSFORCES.PNG)
![SF1](A1COARSESFFORCES.PNG)

### MID

![details1](A1MIDMESHDETAIL.PNG)
![disp1](A1DISPMIDFORCES.PNG)
![stress1](A1MIDSTRESSFORCES.PNG)
![SF1](A1MIDSFFORCES.PNG)

### Fine

![details1](A1FINEMESHDETAILS.PNG)
![disp1](A1FINEDISPFORCES.PNG)
![stress1](A1FORCESTRESS.PNG)
![SF1](A1SFFORCES.PNG)

### Results to Report

| Mesh Level | Elements | Max Displacement (mm) | Max von Mises Stress (MPa) | Min Factor of Safety (SF) | % Change in Stress | % Change in Deflection |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Coarse** | 470,003 | 0.2774 | 16.95 | 16 | — | — |
| **Medium** | 1,147,816 | 0.2817 | 16.71 | 16 | -1.42% | +1.55% |
| **Fine** | 5,258,615 | 0.2847 | 19.97 | 14 | +19.51% | +1.07% |

Deflection converged across all of the refinements, changing by only 1.55% from coarse to medium and 1.07% from medium to fine. Stress numbers were relatively stable between coarse and medium, and the fine mesh increased to 19.97 MPa. The max stress was 19.97 MPa at the corners of the T-slot edges. The max deflection was .287mm at the upper motor mount top flange and the center span of the horizontal cross beam. The factor of safety stayed well above 4%. The two forces I previously stated, 1.45 N*m and the 1000 N loads entered through the motor mount, induce bending and torsion in the upper horizontal crossbeam. The deformed plots reveal noticeable portal frame sway. Another thing I noted was that the 1.45 N*m per-mount torque introduces a slight asymmetric twist along the top crossbeam, causing one side of the upper assembly to deflect slightly further than the opposite upright

### Optimization pass

Some of the proposed targeted changes would be to add some cross bracing to cut the .2847 mm of deflection, as it would make the structure even sturdier. Another thing would be to pocket the non-load-bearing material from the motor mount walls to reduce top-heavy mass, while thickening the base flange pads and increasing the radii to diffuse local clamping stresses. Due to SF being so high, I would also downsize the frame profile in order to make it sturdier, and while it decreases SF, it's a good trade-off to do.

When it comes to shift in critical locations, I would add some larger corner gussets and thicker motor mount flanges to relieve the sharp stress risers currently concentrated at the base's corners and mounting edges. Doing this, the peak stresses will distribute more evenly along the mid-span bending fibers of the vertical uprights. 


## Bill of Materials & Engineering Cost

<table>
  <thead>
    <tr>
      <th>Item Number</th>
      <th>Quantity</th>
      <th>Name</th>
      <th>Cost Per</th>
      <th>Cost</th>
      <th>Link</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>1/2"-10 Lead Screw</td>
      <td>$9.90</td>
      <td>$19.80</td>
      <td>www.mcmaster.com/98935A823</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2</td>
      <td>Flange Nut</td>
      <td>$125.72 each</td>
      <td>$251.44</td>
      <td>www.mcmaster.com/95120A111</td>
    </tr>
    <tr>
      <td>3</td>
      <td>1</td>
      <td>T-Slot Framing System</td>
      <td>$245.87</td>
      <td>$245.87</td>
      <td>www.grainger.com/product/80-20-T-Slot-Framing-System-Rectangle-804T48</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2</td>
      <td>Nema 23 Stepper Motor 2.4Nm 4A 57x57x82mm 8mm</td>
      <td>$18.92</td>
      <td>$37.84</td>
      <td>www.omc-stepperonline.com/nema-23-stepper-motor-2-4nm-339-79oz-in-4a-57x57x82mm-8mm-shaft-4-wires-23hs32-4004s</td>
    </tr>
    <tr>
      <td>5</td>
      <td>2</td>
      <td>Machinable-Bore Clamping Shaft Coupling</td>
      <td>$39.05</td>
      <td>$78.10</td>
      <td>www.mcmaster.com/3084K31/</td>
    </tr>
    <tr>
      <td>6</td>
      <td>1</td>
      <td>Black-Oxide Alloy Steel Socket Head Screw (100 count)</td>
      <td>$16.36</td>
      <td>$16.36</td>
      <td>www.mcmaster.com/91290A228/</td>
    </tr>
    <tr>
      <td>7</td>
      <td>6</td>
      <td>Inside Corner Bracket: Wide Inside-Corner Bracket, 15 Series, 1 1/2 in x 2 13/16 in x 1 1/2 in</td>
      <td>$9.99</td>
      <td>$59.94</td>
      <td>www.grainger.com/product/80-20-Inside-Corner-Bracket-Wide-5JRU9</td>
    </tr>
<br>
    </tr>
    <tfoot>
    <tr>
      <td colspan="2" style="text-align: center; font-weight: bold; background-color: #f9f9f9;">
        Total Part Cost: $709.35
      </td>
    </tr>
  </tfoot>
  </tbody>
</table>
  <br>

<table>
  <thead>
    <tr>
      <th>Person</th>
      <th>Hours</th>
      <th>Hourly</th>
      <th>Cost</th>
    </tr>
  </thead>
  <body>
    <tr>
      <td>Brian</td>
      <td>5</td>
      <td>$35</td>
      <td>$175</td>
    </tr>
    <tr>
      <td>Antonio</td>
      <td>6</td>
      <td>$35</td>
      <td>$210</td>
    </tr>
    <tr>
      <td>Jarek</td>
      <td>6</td>
      <td>$35</td>
      <td>$210</td>
    </tr>
    <tfoot>
    <tr>
      <td colspan="2" style="text-align: center; font-weight: bold; background-color: #f9f9f9;">
      Pay Cost: $595
      </td>
    </tr>
    <tr>
      <td colspan="2" style="text-align: center; font-weight: bold; background-color: #f9f9f9;">
        Project Cost: $1,264.39
    </tr>
  </tfoot>
  </body>
</table>


## Lessons Learned

### - Brian Jones

- *Time Spent:* 4.5 hours
- *Key Takeaways:*
  - I learned how to calculate motor speed (0.394–19.69RPM) and torque ($1.45\text{ N}\cdot\text{m}$) from crosshead velocity (1–50mm/min), screw lead (2.54mm/rev), and friction losses (=0.15, mech=0.90)
  - I learned that theoretical stress minimums (like a 0.0315mm bracket thickness at Nsf=4) are much thinner than what can be practically machined, so manufacturability governs the final part sizes
  - I learned how to resolve dual motor reaction torques ($2.90\text{ N}\cdot\text{m}$) into frame stabilizing forces (36.25N) and verify vertical standoff stability using Euler column buckling (Nsf=277.5)
  - I learned how to use *Machinery's Handbook* to select an RC3 running fit (H7/f6) and M5 clearance holes so the lower lead screw bearing floats axially without binding

### - Antonio Goodson

- *Time Spent:* 6 hours
- *Key Takeaways:*
  - Within this assignment, I feel that I was able to research more effectively and with better foresight of how things would work together.
  - Some setbacks that I encountered were finding the appropriate motor and coupler.
  - The problem was that finding a motor with a keyed shaft with a diameter of 8 mm was difficult. When I did find one, it cost over $1000, so I switched to a D-shaped shaft and found a stepper motor within the specifications, which greatly reduced the cost.
  - For the coupler, there was great trouble finding one that would connect an 8 mm to a half inch, so what I did was find a machinable coupler that connected to Half an inch and increase the machining hole in the CAD software.


### - Jarek Hernandez

- *Time Spent:* 6 hours
- *Key Takeaways:*
  - I was able to get more practice with SolidWorks' FEA modeling tools and learned how to apply multiple loads in this case, one axial compression force while also having a transverse load.
  - Quickly realized how important it is to have an ample amount of space on your computer to run FEA models.
  - While looking at supports to assemble the frame, I quickly realized how much variety there is and how important it is to pick the correct brackets to prevent failures.
  - I also learned how crucial it is to have a good graphics card the bigger the assemblies get due to increase poly count the bigger a toll it takes on your computer.
  - This is more a review from A7, but I also remembered how important theoretical verification is, by comparing your results from FEA to the original hand calculations to validate simulation results.


## CAD FILES - Antonio Goodson & Jarek Hernandez

[Motor Mount](Motor_Mount.SLDPRT)

[Bearing](Lead_Screw_Bearing.SLDPRT)

[Original_Assembly](A11_Assem.zip)

[Part_1_FEA](A11TOPSTUFF.zip)

[Part_2_FEA](A11FEA2.zip)


## 2157 Section
