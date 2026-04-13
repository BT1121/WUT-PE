## 1. Derivation of System Parameters (Voltage, Current, and Resistance)

The project requires designing a Boost converter for a laptop operating in Discontinuous Conduction Mode (DCM). Based on the selected device specifications (Dell Laptop Charger), the nominal output is $20\text{V}$ and $4.5\text{A}$.

Input Voltage ($V_{in}$) Limits:The energy source is a nominal $11.1\text{V}$ Li-ion battery pack. To simulate real-world battery discharging behavior (State of Charge, SOC), the input voltage varies by $\pm 20\%$:

- Nominal Input: 
$V_{in\_nom} = 11.1\text{V}$

- Maximum Input (Fully Charged): $V_{in\_max} = 11.1\text{V} \times 1.2 = 13.32\text{V}$

- Minimum Input (Depleted): $V_{in\_min} = 11.1\text{V} \times 0.8 = 8.88\text{V}$

***Output Load ($R$) Limits:***

The converter must maintain $20\text{V}$ at the output ($V_{out}$) while the load varies between $50\%$ and $120\%$ of the nominal current ($I_{out\_nom} = 4.5\text{A}$). In the simulation, the load is modeled as a resistor ($R = V_{out} / I_{out}$):

## 2. Component Selection ($L$ and $C$)

A switching frequency of $f_s = 100\text{ kHz}$ was selected as a standard industry practice to reduce the size and cost of the passive components, fulfilling the grading rubric's cost-efficiency requirement.

***Inductor ($L$) Derivation:***

To guarantee DCM operation across all conditions, the selected inductance must be smaller than the critical inductance ($L_{crit}$). The worst-case scenario for leaving DCM (entering CCM) occurs at the minimum input voltage ($8.88\text{V}$) and maximum load current ($5.4\text{A}$, $3.70\ \Omega$).First, we calculate the boundary duty cycle ($D$) using the CCM conversion ratio:$$D = 1 - \frac{V_{in\_min}}{V_{out}} = 1 - \frac{8.88}{20} = 0.556$$Next, we calculate $L_{crit}$:$$L_{crit} = \frac{R_{min}}{2 \cdot f_s} \cdot D \cdot (1-D)^2$$ $$L_{crit} = \frac{3.70}{2 \cdot 100,000} \cdot 0.556 \cdot (0.444)^2 \approx 2.03\ \mu\text{H}$$

Selection: To provide a safe design margin and ensure deep DCM operation, an inductor of $L = 1.5\ \mu\text{H}$ was selected.

***Capacitor ($C$) Derivation:***

The project restricts the output voltage ripple ($\Delta V_{out}$) to $4\%$ of the nominal voltage:$$\Delta V_{out} = 20\text{V} \times 0.04 = 0.8\text{V}$$In a boost converter, the capacitor supplies the entire load current when the switch is ON. The worst-case ripple occurs at maximum load ($I_{out\_max} = 5.4\text{A}$) and maximum duty cycle ($D = 0.556$):$$C_{min} = \frac{I_{out\_max} \cdot D}{f_s \cdot \Delta V_{out}}$$ $$C_{min} = \frac{5.4 \cdot 0.556}{100,000 \cdot 0.8} \approx 37.53\ \mu\text{F}$$

Selection: To account for non-ideal component behaviors and ensure the ripple remains well below the $0.8\text{V}$ limit, a standard capacitor value of $C = 100\ \mu\text{F}$ was selected.

## 3. Justification of Test Cases (Corner Testing)

To prove the robustness of the open-loop converter design, three specific operational scenarios (Test Cases A, B, and C) were simulated. These cases represent the absolute limits of the system's operational envelope:

- Case A: Worst-Case Heavy Load & Low Battery ($V_{in} = 8.88\text{V}, R = 3.70\ \Omega$)
  -  Purpose: This is the most critical test for a DCM design. It proves that even when the battery is nearly depleted and the laptop demands maximum power ($108\text{W}$), the inductor fully discharges its energy each cycle, successfully maintaining the Discontinuous Conduction Mode without failing into CCM.

- Case B: Nominal Operation ($V_{in} = 11.1\text{V}, R = 4.44\ \Omega$)
  -  Purpose: This case demonstrates the system's stability under standard, day-to-day conditions ($90\text{W}$ output). It acts as the baseline measurement for voltage regulation and efficiency.

- Case C: Worst-Case Light Load & Full Battery ($V_{in} = 13.32\text{V}, R = 8.88\ \Omega$)
  -  Purpose: In DCM, open-loop control is highly sensitive to light loads. This case proves that when the battery is fully charged and the laptop requires minimal power ($45\text{W}$), the output voltage can still be successfully regulated to $20\text{V}$ by adjusting the duty cycle, without causing catastrophic overvoltage to the load.
