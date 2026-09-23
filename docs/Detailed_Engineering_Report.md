# Detailed Engineering Report: Preliminary Conceptual Design of a Ground-Based LEO Tracking Radar

## Executive Summary
This report documents the engineering approach used to develop a preliminary conceptual design for a ground-based monostatic radar capable of detecting and tracking small LEO objects between 500 km and 800 km altitude. The design target is a metallic spherical object of approximately 10 cm diameter. The assessment requirements include detection, acquisition, continuous tracking, range estimation, radial velocity estimation, angular position estimation, and orbit-determination support.

The problem was approached as a system-engineering exercise rather than as an attempt to produce a production-ready radar. The work was decomposed into requirements, frequency selection, waveform design, link budget, antenna, transmitter, receiver, signal processing, tracking, orbit determination, and sensitivity analysis. The resulting preliminary architecture uses **3 GHz operation**, a **15 MHz LFM waveform**, a **16 m parabolic reflector**, and **15 kW peak RF power**.

At the 800 km design point, the preliminary link budget gives approximately **15.2 dB ideal processed SNR**. After allocating 5 dB for implementation and processing losses, approximately **10.2 dB effective margin** remains. The 15 MHz waveform provides **10 m theoretical range resolution** and approximately **0.094 m/s theoretical velocity resolution** for the assumed coherent processing interval.

---

## 1. Problem Definition and Requirements
The assignment asks for a ground-based radar capable of detecting and tracking LEO objects from 500 km to 800 km altitude, including a target with an approximate 10 cm diameter metallic-sphere RCS. The system must support detection, acquisition, continuous tracking, range, radial velocity, angular position, and orbit-determination support.

The principal numerical requirements are:
* **Detection Margin:** $\ge 10 \text{ dB}$
* **Range Resolution:** $\le 10 \text{ m}$
* **Velocity Accuracy:** $< 1 \text{ m/s}$

The 800 km case is treated as the primary design point because it is the most demanding range.

---

## 2. Engineering Approach
The design process followed a top-down decomposition:
$$\text{Requirements} \rightarrow \text{RF Frequency} \rightarrow \text{Waveform} \rightarrow \text{Radar Equation / Link Budget} \rightarrow \text{Antenna} \rightarrow \text{Transmitter} \rightarrow \text{Receiver} \rightarrow \text{Digital Processing} \rightarrow \text{Detection} \rightarrow \text{Doppler} \rightarrow \text{Tracking} \rightarrow \text{Orbit Determination} \rightarrow \text{Sensitivity Analysis}$$

This decomposition helped prevent individual component choices from being made independently of system performance. Each major parameter was linked back to a requirement or to a system-level trade-off.

---

## 3. Frequency Selection
A preliminary operating frequency of **3 GHz** was selected ($\lambda = 0.1 \text{ m}$). The choice represents a compromise between antenna aperture, angular resolution, Doppler sensitivity, RF technology practicality, and atmospheric propagation. A detailed site-specific propagation analysis would be required in a subsequent design phase.

The 0.1 m wavelength gives a direct Doppler relationship of:
$$f_D = \frac{2 v_r}{\lambda} = 20 v_r \quad (\text{Hz for } v_r \text{ in m/s})$$

This provides useful sensitivity to radial velocity.

---

## 4. Waveform Design
An LFM chirp was selected because it allows a relatively long, high-energy pulse to be transmitted while achieving fine range resolution after matched filtering.

* **Bandwidth ($B$):** $15 \text{ MHz}$
* **Pulse Duration ($\tau$):** $100 \ \mu\text{s}$
* **Time-Bandwidth Product ($BT$):** $1500$
* **Theoretical Range Resolution ($\Delta R$):** 
  $$\Delta R = \frac{c}{2B} = \frac{3 \times 10^8}{2(15 \times 10^6)} = 10 \text{ m}$$
* **Ideal Pulse-Compression Gain:** 
  $$10 \log_{10}(1500) \approx 31.8 \text{ dB}$$

This choice demonstrates the interaction between waveform duration, bandwidth, transmitted energy, and range resolution rather than treating range resolution as an isolated specification.

---

## 5. Initial Link Budget
For a 10 cm diameter metallic sphere, a nominal RCS of $\sigma = \pi(0.05)^2 = 0.00785 \text{ m}^2$ is used. This is an idealized assessment assumption; the RCS of a real object may vary significantly with geometry, attitude, and frequency.

The monostatic radar equation is used to estimate the received echo:
$$P_r = \frac{P_t G_t G_r \lambda^2 \sigma}{(4\pi)^3 R^4}$$

With $P_t = 15 \text{ kW}$, $G_t = G_r \approx 52.2 \text{ dBi}$, $\lambda = 0.1 \text{ m}$, $\sigma = 0.00785 \text{ m}^2$, and $R = 800 \text{ km}$, the estimated received power is approximately **$-135.8 \text{ dBm}$**.

For a 15 MHz bandwidth and 3 dB receiver noise figure, the receiver noise estimate is approximately **$-99.2 \text{ dBm}$**. The resulting raw SNR is approximately **$-36.6 \text{ dB}$**. This shows that detection cannot depend on instantaneous SNR and motivates pulse compression and coherent processing.

---

## 6. Antenna Design
A **16 m parabolic reflector** is proposed. With an assumed 65% aperture efficiency, antenna gain is calculated as:
$$G = \eta \left( \frac{\pi D}{\lambda} \right)^2 \approx 52.2 \text{ dBi}$$

The approximate half-power beamwidth (HPBW) is:
$$\text{HPBW} \approx \frac{70 \lambda}{D} \approx 0.44^\circ$$

A two-axis azimuth/elevation mount is proposed. For angular tracking, a monopulse architecture is preferred because it produces instantaneous angular-error information relative to boresight. A preliminary $\le 0.05^\circ$ measurement target is retained as a design target requiring detailed antenna/feed and calibration verification.

---

## 7. Transmitter Design
The preliminary peak RF output is **15 kW**. A modular solid-state architecture is preferred, using multiple PA modules with power combining. A 60% PA efficiency is used as a preliminary assumption for system power and thermal estimates.

Engineering challenges include power combining, thermal management, impedance matching, monitoring, protection, redundancy, and integration with the T/R network. Individual RF amplifier, driver, and protection components can potentially be selected from COTS technology.

---

## 8. Receiver Design
The proposed receiver chain is:
$$\text{Antenna} \rightarrow \text{T/R Protection} \rightarrow \text{RF Filtering} \rightarrow \text{LNA} \rightarrow \text{Mixer / Downconversion} \rightarrow \text{IF Filtering / Amplification} \rightarrow \text{ADC} \rightarrow \text{FPGA / DSP}$$

A 3 dB receiver noise figure is assumed for the conceptual link budget. The receiver must recover from high-power transmission, preserve coherent phase information, and provide sufficient dynamic range for weak echoes.

---

## 9. Signal Processing
The digital processing chain is:
$$\text{Digital Downconversion} \rightarrow \text{Matched Filter / Pulse Compression} \rightarrow \text{Range Bins} \rightarrow \text{Coherent Integration} \rightarrow \text{Doppler FFT} \rightarrow \text{Range-Doppler Map} \rightarrow \text{CFAR} \rightarrow \text{Detection}$$

With 100 coherent pulses, ideal coherent integration gain is **20 dB**. Combined with approximately **31.8 dB pulse-compression gain**, the ideal processed SNR is approximately **15.2 dB**. Allocating 5 dB for implementation and processing losses leaves approximately **10.2 dB effective preliminary detection margin**.

---

## 10. Doppler, PRF, and Velocity Resolution
For $R_{\text{max}} = 800 \text{ km}$, the simple unambiguous-range constraint gives:
$$\text{PRF}_{\text{max}} \approx 187.5 \text{ Hz}$$

With 100 pulses, the coherent processing interval (CPI) is approximately **0.533 s**. The theoretical velocity resolution is therefore:
$$\Delta v \approx \frac{\lambda}{2 T_{\text{CPI}}} \approx 0.094 \text{ m/s}$$

This comfortably meets the $< 1 \text{ m/s}$ requirement. However, the low PRF produces Doppler ambiguity for LEO velocities. The proposed operational solution is staggered/multiple-PRF operation and ambiguity-resolution processing.

---

## 11. Detection and Tracking
Detection is followed by track initiation, range/Doppler/angle measurement, filtering, and prediction. A closed-loop Az/El servo uses predicted target position and angle error to keep the antenna pointed at the object.

$$\text{Detection} \rightarrow \text{Acquisition} \rightarrow \text{Measurement} \rightarrow \text{Tracking Filter} \rightarrow \text{Prediction} \rightarrow \text{Servo Control} \rightarrow \text{Antenna} \rightarrow \text{Next Measurement}$$

---

## 12. Orbit Determination Support
The radar provides time-tagged measurement vectors:
$$z_k = [R, \dot{R}, \text{Az}, \text{El}]^T$$

Repeated observations are transformed into an Earth-referenced coordinate system and used to estimate the six-state vector $[x, y, z, v_x, v_y, v_z]^T$. An **Extended Kalman Filter (EKF)** is proposed as a preliminary state-estimation architecture.

---

## 13. Sensitivity Analysis and Design Trades

| Parameter Variation | Approx. Detection Margin |
| :--- | :--- |
| **Baseline (800 km, 10 cm, 16m, 15kW)** | **10.2 dB** |
| Range = 700 km | ~12.5 dB |
| Range = 500 km | ~18.4 dB |
| RCS = 0.5× nominal (0.0039 m²) | ~7.2 dB |
| RCS = 2.0× nominal (0.0157 m²) | ~13.2 dB |
| Tx Power = 10 kW | ~8.4 dB |
| Tx Power = 20 kW | ~11.5 dB |
| Antenna = 14 m | ~8.1 dB |
| Antenna = 18 m | ~12.2 dB |

The sensitivity study shows that received power varies as $R^{-4}$, while for a monostatic system using the same antenna aperture, power scales as $D^4$. Range and antenna aperture are therefore the strongest performance drivers.

---

## 14. Custom vs. COTS Engineering

| Subsystem | Approach |
| :--- | :--- |
| RF Synthesizer / Exciter | COTS + custom control |
| ADC / FPGA Hardware | COTS hardware + custom firmware |
| LNA / Mixer / IF Chain | COTS candidates |
| PA Modules | COTS candidates |
| 15 kW PA Assembly & Combining | Custom integration |
| Power Combiner / T-R Network | Custom high-power integration |
| 16 m Reflector & Monopulse Feed | Custom |
| Az/El Mount & Servo Control | Custom integration |
| DSP (Pulse Compression / CFAR) | Custom DSP/software |
| Tracking & Orbit Estimation | Custom software |

---

## 15. Limitations and Further Work
1. Detailed electromagnetic modeling and antenna/feed pattern characterization.
2. Site-specific atmospheric propagation and weather-loss analysis.
3. Optimization of staggered/multiple PRFs for range-Doppler ambiguity resolution.
4. High-power PA thermal management and T/R switch protection design.
5. Orbit determination validation using realistic satellite trajectories.

---

## 16. Final Conclusion
The proposed **3 GHz, 15 kW, 16 m monostatic radar** provides a traceable preliminary architecture for detection and tracking of a 10 cm-class LEO object at ranges up to 800 km. Under the stated assumptions, the design achieves **10 m range resolution**, **~0.094 m/s velocity resolution**, and **~10.2 dB effective detection margin**.
