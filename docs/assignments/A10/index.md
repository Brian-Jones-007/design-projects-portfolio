# A10 – Lead Screw Translating System

**Team Section Contributions:**
* **Calculations / Free Body Diagrams:** Brian Jones
* **Parametrically Model (CAD):** Antonio Goodson
* **FEA Analysis:** Jarek Hernandez Acaba

---

## Objective
Design and perform structural analysis for a mechanically driven lead-screw crosshead system for a tensile testing machine. The system must support a total tensile load of $2\text{ kN}$ ($2000\text{ N}$) evenly divided between two lead screws ($1000\text{ N}$ per screw), maintain a linear crosshead speed range of $1\text{ mm/min}$ to $50\text{ mm/min}$, meet a structural Design Safety Factor ($N_{sf}$) of $4$, and limit maximum crosshead deflection to $\le 0.1\text{ mm}$.

---

## Section 1: Lead Screw Calculations & Selection - Brian Jones

### Task 1.1 — Minimum Lead Screw Diameter ($d$)
* **a. Knowns and Unknowns:**
  * **Knowns:**
    * Total tensile load ($F_{total}$): $2000\text{ N}$
    * Number of lead screws ($n$): $2$
    * Load per lead screw ($F_{screw}$): $1000\text{ N}$
    * Safety Factor ($N_{sf}$): $4$
    * Material Yield Strength ($S_y$, AISI 1018 Steel): $300\text{ MPa}$ ($300 \times 10^6\text{ Pa}$)
  * **Unknowns:**
    * Allowable tensile stress ($\sigma_{allow}$)
    * Minimum tensile stress area ($A_{min}$)
    * Minimum minor diameter ($d_{min}$)

* **b. Free Body Diagram (FBD):**
  ![Lead Screw Axial Tension FBD](fbd_leadscrew_axial.JPG)

* **c. Symbolic Solution:**

$$\sigma_{allow} = \frac{S_y}{N_{sf}}$$

$$\sigma = \frac{F_{screw}}{A} \le \sigma_{allow} \implies A_{min} = \frac{F_{screw}}{\sigma_{allow}} = \frac{N_{sf} \cdot F_{screw}}{S_y}$$

$$A_{min} = \frac{\pi d_{min}^2}{4} \implies d_{min} = \sqrt{\frac{4 \cdot N_{sf} \cdot F_{screw}}{\pi S_y}}$$

* **d. Numerical Solution:**

$$\sigma_{allow} = \frac{300 \times 10^6\text{ Pa}}{4} = 75 \times 10^6\text{ Pa} = 75\text{ MPa}$$

$$d_{min} = \sqrt{\frac{4 \times (4) \times 1000\text{ N}}{\pi \times (300 \times 10^6\text{ N/m}^2)}} \approx 0.00412\text{ m} = 4.12\text{ mm} \quad (0.162\text{ in})$$

---

### Task 1.2 — Rotational Speed Range ($\omega$ / $RPM$)
* **a. Knowns and Unknowns:**
  * **Knowns:**
    * Linear crosshead speed range ($v$): $1\text{ mm/min}$ to $50\text{ mm/min}$
    * Selected screw lead ($L$): $2.54\text{ mm/rev}$ ($0.100\text{ in/rev}$, standard 1/2"-10 Acme screw)
  * **Unknowns:**
    * Minimum rotational speed ($RPM_{min}$)
    * Maximum rotational speed ($RPM_{max}$)

* **b. Free Body Diagram (FBD):**
  ![Rotational Speed to Velocity Diagram](fbd_rotational_speed.JPG)

* **c. Symbolic Solution:**

$$v = RPM \cdot L \implies RPM = \frac{v}{L}$$

$$RPM_{min} = \frac{v_{min}}{L}, \quad RPM_{max} = \frac{v_{max}}{L}$$

* **d. Numerical Solution:**

$$RPM_{min} = \frac{1\text{ mm/min}}{2.54\text{ mm/rev}} \approx 0.394\text{ RPM}$$

$$RPM_{max} = \frac{50\text{ mm/min}}{2.54\text{ mm/rev}} \approx 19.69\text{ RPM}$$

---

### Task 1.3 — Maximum Torque ($T_{max}$)
* **a. Knowns and Unknowns:**
  * **Knowns:**
    * Axial load per screw ($F_{screw}$): $1000\text{ N}$
    * Major diameter ($d$): $12.7\text{ mm}$ ($0.500\text{ in}$)
    * Mean diameter ($d_m$): $11.43\text{ mm}$ ($0.01143\text{ m}$)
    * Lead ($L$): $2.54\text{ mm}$ ($0.00254\text{ m}$)
    * Thread angle ($\alpha$, Acme $29^\circ$): $14.5^\circ \implies \sec\alpha = 1.033$
    * Coefficient of friction ($\mu$, steel on bronze): $0.15$
  * **Unknowns:**
    * Required driving torque per screw ($T_{screw}$)

* **b. Free Body Diagram (FBD):**
  ![Lead Screw Thread Friction and Torque FBD](fbd_leadscrew_torque.JPG)

* **c. Symbolic Solution:**

$$T_{screw} = \frac{F_{screw} \cdot d_m}{2} \left[ \frac{L + \pi \mu d_m \sec\alpha}{\pi d_m - \mu L \sec\alpha} \right]$$

* **d. Numerical Solution:**

$$T_{screw} = \frac{1000 \cdot 0.01143}{2} \left[ \frac{0.00254 + \pi (0.15)(0.01143)(1.033)}{\pi (0.01143) - (0.15)(0.00254)(1.033)} \right] \approx 1.305\text{ N}\cdot\text{m} \quad (11.55\text{ in-lbf})$$

---

### Task 1.4 — Lead Screw Selection
* **Selected Component:** Standard 1/2"-10 Single-Start Acme Lead Screw (18-inch length minimum).
* **Specifications:** Major Diameter $= 0.500\text{ in}$, Lead $= 0.100\text{ in/rev}$, Pitch $= 0.100\text{ in}$.

---

## Section 2: Nut & Flange Thread Bearing Analysis

* **a. Knowns and Unknowns:**
  * **Knowns:**
    * Axial load per nut ($F_{screw}$): $1000\text{ N}$
    * Major thread diameter ($d$): $12.7\text{ mm}$
    * Minor thread diameter ($d_r$): $9.75\text{ mm}$
    * Pitch ($p$): $2.54\text{ mm}$
    * Engaged threads ($n_t$): $6$
    * Bronze Yield Strength ($S_y$): $120\text{ MPa}$
    * Allowable bearing stress ($\sigma_{b,allow}$): $\frac{120\text{ MPa}}{4} = 30\text{ MPa}$
  * **Unknowns:**
    * Total bearing area ($A_b$)
    * Actual thread bearing stress ($\sigma_b$)

* **b. Free Body Diagram (FBD):**
  ![Nut Thread Bearing Stress FBD](fbd_nut_bearing.JPG)

* **c. Symbolic Solution:**

$$A_b = \frac{\pi}{4} (d^2 - d_r^2) \cdot n_t$$

$$\sigma_b = \frac{F_{screw}}{A_b} = \frac{4 F_{screw}}{\pi n_t (d^2 - d_r^2)} \le \sigma_{b,allow}$$

* **d. Numerical Solution:**

$$A_b = \frac{\pi}{4} \left( (0.0127)^2 - (0.00975)^2 \right) \cdot 6 \approx 3.12 \times 10^{-4}\text{ m}^2$$

$$\sigma_b = \frac{1000\text{ N}}{3.12 \times 10^{-4}\text{ m}^2} \approx 3.20\text{ MPa}$$

* **Conclusion:** Since $\sigma_b = 3.20\text{ MPa} \le 30\text{ MPa}$, thread bearing strength is verified with $N_{sf} > 4$.

---

## Section 3: Crosshead Structural Design & Analysis

### Task 3.2 — Beam Stress & Strength Analysis
* **a. Knowns and Unknowns:**
  * **Knowns:**
    * Total force ($F_{total}$): $2000\text{ N}$
    * Beam span ($L_b$): $200\text{ mm} = 0.20\text{ m}$
    * Material (Aluminum 6061-T6): $S_y = 276\text{ MPa}$, $E = 68.9\text{ GPa}$
    * Allowable stress ($\sigma_{allow}$): $\frac{276}{4} = 69\text{ MPa}$
    * Crosshead width ($b$): $40\text{ mm} = 0.040\text{ m}$
  * **Unknowns:**
    * Maximum bending moment ($M_{max}$)
    * Minimum required height for strength ($h_{min, strength}$)

* **b. Free Body Diagram (FBD):**
  ![Crosshead Beam Bending FBD and Shear Moment Diagram](fbd_crosshead_beam_.JPG)

* **c. Symbolic Solution:**

$$M_{max} = \frac{F_{total} \cdot L_b}{4}$$

$$\sigma_{max} = \frac{M_{max} \cdot c}{I} = \frac{6 M_{max}}{b h^2} \le \sigma_{allow} \implies h_{min, strength} = \sqrt{\frac{6 M_{max}}{b \cdot \sigma_{allow}}}$$

* **d. Numerical Solution:**

$$M_{max} = \frac{2000\text{ N} \cdot 0.20\text{ m}}{4} = 100\text{ N}\cdot\text{m}$$

$$h_{min, strength} = \sqrt{\frac{6 \cdot 100}{0.040 \cdot (69 \times 10^6)}} \approx 0.0147\text{ m} = 14.7\text{ mm}$$

---

### Task 3.3 — Beam Deflection Analysis
* **a. Knowns and Unknowns:**
  * **Knowns:**
    * Total force ($F_{total}$): $2000\text{ N}$, Span ($L_b$): $0.20\text{ m}$
    * Modulus of Elasticity ($E$): $68.9\text{ GPa}$
    * Maximum allowable deflection ($\delta_{max}$): $0.1\text{ mm} = 1.0 \times 10^{-4}\text{ m}$
    * Width ($b$): $40\text{ mm} = 0.040\text{ m}$
  * **Unknowns:**
    * Required moment of inertia ($I_{req}$)
    * Minimum required height for stiffness ($h_{min, stiffness}$)

* **b. Free Body Diagram (FBD):**
  ![Crosshead Beam Deflection FBD](fbd_crosshead_deflection.JPG)

* **c. Symbolic Solution:**

$$\delta_{max} = \frac{F_{total} \cdot L_b^3}{48 E I} \le 0.1\text{ mm} \implies I_{req} = \frac{F_{total} \cdot L_b^3}{48 E \cdot \delta_{max}}$$

$$I = \frac{b h^3}{12} \implies h_{min, stiffness} = \sqrt[3]{\frac{12 \cdot I_{req}}{b}}$$

* **d. Numerical Solution:**

$$I_{req} = \frac{2000 \cdot (0.20)^3}{48 \cdot (68.9 \times 10^9) \cdot (1.0 \times 10^{-4})} \approx 4.838 \times 10^{-8}\text{ m}^4$$

$$h_{min, stiffness} = \sqrt[3]{\frac{12 \cdot (4.838 \times 10^{-8})}{0.040}} \approx 0.0244\text{ m} = 24.4\text{ mm}$$

* **Final Selected Dimensions:** $h = 25\text{ mm}$ ($1.0\text{ in}$), $b = 40\text{ mm}$ ($1.57\text{ in}$). Deflection under $2\text{ kN}$ is $\delta = 0.093\text{ mm} \le 0.1\text{ mm}$.

---

### Task 3.4 — Multiview Paper Sketch of Crosshead - Antonio Goodson


---

## Section 4: Parametrically Model (CAD) - Antonio Goodson


---

## Section 5: FEA Analysis - Jarek Hernandez Acaba

