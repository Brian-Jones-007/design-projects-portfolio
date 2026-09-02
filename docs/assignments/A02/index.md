# A2 - Truss Stress Analysis

## Objective

The objective of this assignment was to design, analyze, size, and 3D model an asymmetrical trapezoidal plate truss system under specified static loading conditions. The design requires evaluating static equilibrium, member axial forces, structural member sizing, double shear pin connections, and validating total theoretical mass against native 3D CAD mass properties.

---

## Analyze

### 1. Geometry & Load Identification
To establish the baseline layout of the truss, the spatial coordinates of all joint nodes and external loading vectors were established relative to origin Node C $(0,0)\text{ m}$. Using design parameters $a = 0.4\text{ m}$ and $b = 0.3\text{ m}$, the system geometry was mapped with a pin support at Node A and a roller support at Node B.
* **Node C (Origin):** $(0.00, 0.00)\text{ m}$
* **Node B (Roller Support):** $(-0.40, 0.30)\text{ m}$
* **Node D (Applied Load Point):** $(0.40, 0.00)\text{ m}$
* **Node A (Pin Support):** $(0.80, 0.30)\text{ m}$
* **Applied Forces ($P$):** $+25\text{ kN}$ vertical force at Node C and $-25\text{ kN}$ vertical force at Node D

![Global Free Body Diagram](./images/fbd_global.png)  
*Figure 1: Global Free Body Diagram showing node coordinates, support reactions, and external loads.*

### 2. Static Equilibrium & Support Reactions
Global static equilibrium equations were solved to determine the external reaction forces at supports Node A and Node B. Taking the sum of moments about pin support Node A ($\Sigma M_A = 0$) determined the vertical reaction at roller Node B, followed by vertical ($\Sigma F_y = 0$) and horizontal ($\Sigma F_x = 0$) summations for Node A.
$$\Sigma M_A = 0 \implies B_y = -8.333\text{ kN} \quad \text{(acting downward)}$$
$$\Sigma F_y = 0 \implies A_y = +8.333\text{ kN} \quad \text{(acting upward)}$$
$$\Sigma F_x = 0 \implies A_x = 0.000\text{ kN}$$

![Reaction Force Calculations](./images/calc_reactions.png)  
*Figure 2: Handwritten static equilibrium equations for external support reactions.*

### 3. Internal Member Force Analysis (Method of Joints)
Internal axial forces for all five structural members were systematically calculated using the Method of Joints, starting at Joint C and resolving vector equilibrium through Joints B, D, and A.
* **Member BC:** $F_{BC} = -13.89\text{ kN}$ (Compression)
* **Member CD:** $F_{CD} = +11.11\text{ kN}$ (Tension)
* **Member BA:** $F_{BA} = -22.22\text{ kN}$ (Compression)
* **Member BD:** $F_{BD} = 0.00\text{ kN}$ (Zero-force member)
* **Member AD:** $F_{AD} = +41.67\text{ kN}$ (Tension) — **Governing Peak Tensile Force**

![Method of Joints Analysis](./images/calc_moj.png)  
*Figure 3: Method of joints force balance and vector breakdown.*

### 4. Structural Sizing Calculations
Structural member cross-sectional sizing was governed by peak tension member AD using A500 Structural Steel ($\sigma_y = 350\text{ MPa}$) with a required factor of safety $\text{FS}_{\text{truss}} = 3.5$.
$$\sigma_{\text{allow}} = \frac{\sigma_y}{\text{FS}_{\text{truss}}} = \frac{350\text{ MPa}}{3.5} = 100\text{ MPa}$$

$$A_{\text{truss}(\text{min})} = \frac{F_{AD}}{\sigma_{\text{allow}}} = \frac{41.67\text{ kN}}{100\text{ MPa}} = 462.96\text{ mm}^2$$

![Structural Sizing Computations](./images/calc_sizing.png)  
*Figure 4: Allowable stress, factor of safety, and minimum cross-sectional area calculations.*

### 5. Joint & Pin Sizing Analysis
Support pin connections were evaluated under double shear conditions using hardened steel pins ($\tau_y = 1172\text{ MPa}$) with a safety factor $\text{FS}_{\text{pin}} = 4.0$ based on the maximum shear force $V_{\text{max}} = 8.333\text{ kN}$.
$$\tau_{\text{allow}} = \frac{\tau_y}{\text{FS}_{\text{pin}}} = \frac{1172\text{ MPa}}{4.0} = 293.00\text{ MPa}$$

$$A_{\text{pin}(\text{min})} = \frac{V_{\text{max}}}{2 \cdot \tau_{\text{allow}}} = \frac{8.333\text{ kN}}{2 \cdot 293.00\text{ MPa}} = 14.22\text{ mm}^2$$

$$d_{\text{pin}(\text{min})} = \sqrt{\frac{4 \cdot 14.22\text{ mm}^2}{\pi}} = 4.26\text{ mm}$$

Rounding up to the nearest standard commercial component size yields a **$d_{\text{pin}} = 5.00\text{ mm}$** diameter pin.

![Pin Sizing Math](./images/calc_pins.png)  
*Figure 5: Double shear pin sizing and mass property math.*

---

## Decide

A single-plate, waterjet/laser-cut truss topology was selected over a traditional multi-part tubular space frame assembly. Selecting a flat plate design allows the entire 5-member structure to be manufactured from a single piece of stock material, eliminating assembly alignment errors, reducing manufacturing complexity, and streamlining CAD construction into a single-sketch extrusion. 

To maintain the required cross-sectional area of $A_{\text{truss}} = 462.96\text{ mm}^2$ across all members while preserving structural rigidity against out-of-plane bending:
* **Selected Plate Thickness ($t$):** $12.00\text{ mm}$
* **Derived Member Width ($w$):** $38.58\text{ mm}$ ($38.58\text{ mm} \times 12.00\text{ mm} = 462.96\text{ mm}^2$)
* **Node Joint Bosses:** $50.00\text{ mm}$ diameter circular pads centered at all four node coordinates to reinforce material surrounding the $5.00\text{ mm}$ reamed pin holes.

---

## Communicate

### CAD Modeling & Mass Property Verification
The 3D model was constructed in PTC Creo Parametric by sketching the continuous node framework, offsetting member boundaries symmetrically to $w = 38.58\text{ mm}$, adding $50\text{ mm}$ node boss pads with concentric $5.0\text{ mm}$ pin holes, and extruding to a thickness of $t = 12.0\text{ mm}$. Standard A500 Structural Steel material properties ($\rho = 7850\text{ kg/m}^3$) were assigned to the solid body.

Native Creo Mass Properties analysis confirmed a total part volume of $1.599 \times 10^6\text{ mm}^3$ and a total frame mass of **$12.55\text{ kg}$**, perfectly matching the theoretical hand-calculated frame mass. Adding four $5.0\text{ mm}$ steel pins ($0.015\text{ kg}$) yields a total system assembly mass of **$12.57\text{ kg}$** ($123.30\text{ N}$).

![Creo 3D Model Render](./images/creo_render.png)  
*Figure 6: High-resolution CAD model of the single-plate extruded truss frame.*

![Creo Mass Properties Verification](./images/creo_mass_properties.png)  
*Figure 7: Creo Mass Properties window confirming part volume and mass verification.*

> [!NOTE]
> **CAD File Downloads:**
> * [Download Truss Frame Creo Part File (`truss_frame.prt`)](./cad/truss_frame.prt)
> * [Download Universal STEP File (`truss_frame.stp`)](./cad/truss_frame.stp)
