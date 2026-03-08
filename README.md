# IEEE 14-Bus Load Flow Analysis

A comprehensive load flow analysis of the IEEE 14-Bus power system conducted using PowerWorld Simulator. The study examines steady-state bus voltages, transmission line power flows, generator reactive power limits, and system losses under both base case and stressed operating conditions. Two voltage improvement techniques are applied and compared.


**Course:** SKEE 3443 — Power System Analysis  
**Institution:** Universiti Teknologi Malaysia  
**Software:** PowerWorld Simulator  
**System Base:** 100 MVA, 132 kV  



---

## System Overview

The standard IEEE 14-Bus test system was modelled with the following topology:

```
Generators:         5  (Buses 1, 2, 3, 6, 8)
Loads:              11 (distributed across PQ buses)
Transmission Lines: 20
Bus 1:              Slack Bus
Buses 2, 3, 6, 8:  PV (Generator) buses
Remaining buses:    PQ (Load) buses
```

---

## Task A — Base Case and Stressed Condition

### Base Case Results

The load flow converged successfully. Key findings:

```
Total Active Power Loss:    15.4 MW
Total Reactive Power Loss:  42.1 Mvar
```

Six buses were identified below the acceptable voltage threshold of 0.95 p.u.:

| Bus | Voltage (p.u.) |
|-----|----------------|
| 9   | 0.93409        |
| 10  | 0.93837        |
| 11  | 0.93848        |
| 12  | 0.93259        |
| 13  | 0.92951        |
| 14  | 0.91300        |

Bus 14 was identified as the most critical bus, situated electrically far from generation sources. Every PV bus generator was found to be operating at its maximum reactive power capability (Qmax), confirming the system is reactive power limited in the base case.

---

### Stressed Condition (150% Load)

Both active and reactive loads were uniformly scaled to 150% using the Scale Case tool in PowerWorld. Generator settings were kept constant.

The voltage profile deteriorated severely:

```
Bus 14 voltage:          0.6455 p.u.  (collapsed from 0.9130 p.u.)
Buses below 0.80 p.u.:  10 out of 14
```

| Bus | Voltage (p.u.) |
|-----|----------------|
| 9   | 0.69377        |
| 10  | 0.68315        |
| 11  | 0.69856        |
| 12  | 0.69505        |
| 13  | 0.68334        |
| 14  | 0.64550        |

The Slack Bus (Bus 1) was forced to supply 182.07 Mvar to prevent total system blackout, up from absorbing -22.34 Mvar in the base case. The root cause was that generators entered the stressed condition with zero reactive reserve, having already been saturated at Qmax in the base case.

---

## Task B — Voltage Improvement Techniques

Two independent methods were applied to the stressed system to achieve a minimum 5% voltage improvement at Bus 14.

---

### Method 1 — Shunt Capacitor

A 9 Mvar shunt capacitor bank was installed at Bus 14, the weakest bus in the system.

**Reasoning:** The system was reactive power limited. Installing a capacitor at the load bus provides local reactive support, reducing the reactive power flow required from distant generators and thereby reducing the voltage drop across transmission lines leading to Bus 14.

**Results:**

| Bus | Before (p.u.) | After (p.u.) |
|-----|---------------|--------------|
| 9   | 0.69377       | 0.72299      |
| 10  | 0.68315       | 0.72067      |
| 11  | 0.69856       | 0.71884      |
| 12  | 0.69505       | 0.71054      |
| 13  | 0.68334       | 0.70845      |
| 14  | 0.64550       | 0.68256      |

```
Bus 14 improvement:         5.7%  (target met)
Slack Bus reactive output:  reduced from 182.07 Mvar to 167.15 Mvar
Total Active Power Loss:    51.6 MW
Total Reactive Power Loss:  199.1 Mvar
```

---

### Method 2 — Generator Reactive Power Limit Adjustment

The maximum reactive power limit (Qmax) of Generator 8 was increased from 24.0 Mvar to 32.5 Mvar.

**Reasoning:** Generator 8 is the closest voltage control device to the critical load area. It was saturated at its limit in the stressed case. Increasing Qmax permitted additional reactive injection to support the local voltage.

**Results:**

```
Bus 14 voltage:             0.6795 p.u.  (up from 0.6455 p.u.)
Bus 14 improvement:         5.3%  (target met)
Generator 8 output:         increased from 23.99 Mvar to 32.50 Mvar
Slack Bus reactive output:  reduced from 182.07 Mvar to 161.04 Mvar
Total Active Power Loss:    51.03 MW
Total Reactive Power Loss:  197.3 Mvar
```

---

## Comparison of Methods

| Feature                   | Method 1: Shunt Capacitor         | Method 2: Generator Adjustment       |
|---------------------------|-----------------------------------|--------------------------------------|
| Parameter changed         | Added 9 Mvar capacitor at Bus 14  | Increased Gen 8 Qmax to 32.5 Mvar   |
| Bus 14 voltage            | 0.6826 p.u.                       | 0.6795 p.u.                          |
| Improvement at Bus 14     | 5.7%                              | 5.3%                                 |
| Active power loss         | 51.6 MW                           | 51.03 MW                             |
| Reactive power loss       | 199.1 Mvar                        | 197.3 Mvar                           |
| Implementation            | New hardware required             | Utilizes existing generation capacity|

Method 1 provides superior local voltage support at the endpoint. Method 2 achieves a more balanced improvement across the wider network and results in lower total system losses, making it the more efficient technical solution for the overall grid.

---

## Key Conclusions

**Proximity of reactive support matters.** Localized reactive compensation at the load center is the most effective way to correct severe voltage drops at remote buses.

**Reactive reserve is critical.** A stable system requires generators to maintain reactive reserve. When generators operate at Qmax in the base case, the system has no capacity to respond to load growth, making it highly vulnerable to voltage collapse.

**Trade-off between local voltage and system efficiency.** Shunt capacitors provide the best local voltage lift. Generator-side adjustments produce lower system losses but distribute the improvement more broadly rather than targeting the critical bus directly.

**Var-starved network sensitivity.** In a reactive power limited network, even moderate load increases can produce disproportionately large voltage drops when no reactive reserve exists.


