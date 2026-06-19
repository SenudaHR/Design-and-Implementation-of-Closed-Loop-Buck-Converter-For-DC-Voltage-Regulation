# Closed-Loop Buck Converter for DC Voltage Regulation

## Overview

This project presents the design, simulation, and implementation of a **closed-loop Buck (step-down) DC-DC converter** capable of regulating an output voltage between **3 V and 12 V** from a **15 V DC input supply**. The converter employs analog feedback control with PWM modulation to maintain output voltage regulation under varying line and load conditions.

Developed as part of the **EN2111 Electronic Circuit Design** module at the **Department of Electronic & Telecommunication Engineering, University of Moratuwa**.

---

## Features

* Adjustable output voltage: **3 V – 12 V**
* Input voltage: **15 V DC**
* Rated load current: **1 A**
* Closed-loop voltage regulation
* PWM-based control architecture
* Type-II compensated feedback loop
* Analog implementation using operational amplifiers
* High-side MOSFET gate driving using IR2110
* Software validation through simulation

---

## System Architecture

The converter consists of the following major subsystems:

1. **Power Stage**

   * IRF3205 N-channel MOSFET
   * 1N5819 Schottky freewheeling diode
   * LC output filter

2. **Feedback Network**

   * Output voltage sensing divider
   * Adjustable output voltage setting

3. **Reference Voltage Generator**

   * LM431 precision 2.5 V reference

4. **Error Amplifier & Compensation**

   * Type-II compensator
   * NE5532 operational amplifier

5. **PWM Modulator**

   * Triangle waveform generator
   * Comparator-based PWM generation

6. **Gate Driver**

   * IR2110 high-side MOSFET driver

---

## Design Specifications

| Parameter              | Value                  |
| ---------------------- | ---------------------- |
| Input Voltage          | 15 V                   |
| Output Voltage         | 3 – 12 V               |
| Maximum Output Current | 1 A                    |
| Switching Frequency    | 18 kHz                 |
| Reference Voltage      | 2.5 V                  |
| Control Method         | Analog Closed-Loop PWM |

---

## Key Design Equations

### Duty Cycle

```math
D = \frac{V_{out}}{V_{in}}
```

### Inductor Selection

```math
L = \frac{V_{out}(1-D)}{f_s \Delta I_L}
```

### Output Capacitor Selection

```math
C = \frac{\Delta I_L}{8f_s\Delta V_{out}}
```

---

## Selected Components

| Component          | Part Number |
| ------------------ | ----------- |
| MOSFET             | IRF3205     |
| Gate Driver        | IR2110      |
| Op-Amp             | NE5532      |
| Voltage Reference  | LM431       |
| Freewheeling Diode | 1N5819      |
| Output Inductor    | 10 mH       |
| Output Capacitor   | 470 μF      |

---

## Simulation Results

### Triangle Wave Generator

| Parameter            | Theoretical | Simulated |
| -------------------- | ----------- | --------- |
| Frequency            | 20 kHz      | 17.8 kHz  |
| Peak-to-Peak Voltage | 12 V        | 10.5 V    |
| Center Voltage       | 7.5 V       | 7.52 V    |

### Buck Converter

| Output Voltage | Current Ripple |
| -------------- | -------------- |
| 3 V            | 19.0 mA        |
| 12 V           | 27.3 mA        |

The simulation results closely follow theoretical predictions while highlighting practical non-idealities such as ESR, diode voltage drops, and component parasitics.

---

## Control Loop Design

The converter employs:

* Average Small-Signal Modeling
* Type-II Compensation
* Target crossover frequency of **1 kHz**
* Phase margin of approximately **60°**

This ensures:

* Stable operation
* Good transient response
* Zero steady-state voltage error

---

## Challenges Encountered

### Switching Frequency Limitation

The analog PWM generator could not reliably operate at higher switching frequencies due to:

* Op-amp bandwidth limitations
* Component tolerances
* PCB parasitics

As a result, the final implementation was limited to approximately **20 kHz**.

### Quiescent Power Consumption

The analog control circuitry continuously consumes power due to:

* Triangle wave generator
* Error amplifier
* Comparator stages

This reduces efficiency under light-load conditions.

---

## Future Improvements

Future versions can replace the analog control loop with a digital controller implemented on a microcontroller.

Potential benefits:

* Higher switching frequencies
* Improved efficiency
* Adaptive control algorithms
* Digital compensation
* Telemetry and monitoring features

Example digital PI control implementation:

```c
error = Vref - Vout;
duty = duty_prev + a0*error + a1*error_prev;

if(duty > DMAX)
    duty = DMAX;
else if(duty < DMIN)
    duty = DMIN;

updatePWM(duty);
```



---

## Team Members

* Deshan W. U.
* Dhananjaya K. T. G. T. N.
* Dissanayaka D. M. D. P.
* Ratnayake R. M. S. H.

Department of Electronic & Telecommunication Engineering
University of Moratuwa, Sri Lanka

---

## License

This project is released under the MIT License.
