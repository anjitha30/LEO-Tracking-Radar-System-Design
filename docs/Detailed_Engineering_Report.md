# Detailed Engineering Report: Preliminary Conceptual Design of a Ground-Based LEO Tracking Radar

## Executive Summary
This report documents the engineering approach used to develop a preliminary conceptual design for a ground-based monostatic radar capable of detecting and tracking small LEO objects between 500 km and 800 km altitude[cite: 2]. The design target is a metallic spherical object of approximately 10 cm diameter[cite: 2]. The assessment requirements include detection, acquisition, continuous tracking, range estimation, radial velocity estimation, angular position estimation, and orbit-determination support[cite: 2].

The problem was approached as a system-engineering exercise rather than as an attempt to produce a production-ready radar[cite: 2]. The work was decomposed into requirements, frequency selection, waveform design, link budget, antenna, transmitter, receiver, signal processing, tracking, orbit determination, and sensitivity analysis[cite: 2]. The resulting preliminary architecture uses **3 GHz operation**, a **15 MHz LFM waveform**, a **16 m parabolic reflector**, and **15 kW peak RF power**[cite: 2].

At the 800 km design point, the preliminary link budget gives approximately **15.2 dB ideal processed SNR**[cite: 2]. After allocating 5 dB for implementation and processing losses, approximately **10.2 dB effective margin** remains[cite: 2]. The 15 MHz waveform provides **10 m theoretical range resolution** and approximately **0.094 m/s theoretical velocity resolution** for the assumed coherent processing interval[cite: 2].

---

## 1. Problem Definition and Requirements
The assignment asks for a ground-based radar capable of detecting and tracking LEO objects from 500 km to 800 km altitude, including a target with an approximate 10 cm diameter metallic-sphere RCS[cite: 2]. The system must support detection, acquisition, continuous tracking, range, radial velocity, angular position, and orbit-determination support[cite: 2].

The principal numerical requirements are:
* **Detection Margin:** $\ge 10 \text{ dB}$[cite: 1, 2]
* **Range Resolution:** $\le 10 \text{ m}$[cite: 1, 2]
* **Velocity Accuracy:** $< 1 \text{ m/s}$[cite: 1, 2]

The 800 km case is treated as the primary design point because it is the most demanding range[cite: 1, 2].

---

## 2. Engineering Approach
The design process followed a top-down decomposition:
$$\text{Requirements} \rightarrow \text{RF Frequency} \rightarrow \text{Waveform} \rightarrow \text{Radar Equation / Link Budget} \rightarrow \text{Antenna} \rightarrow \text{Transmitter} \rightarrow \text{Receiver} \rightarrow \text{Digital Processing} \rightarrow \text{Detection} \rightarrow \text{Doppler} \rightarrow \text{Tracking} \rightarrow \text{Orbit Determination} \rightarrow \text{Sensitivity Analysis}$$[cite: 2]

This decomposition helped prevent individual component choices from being made independently of system performance[cite: 2]. Each major parameter was linked back to a requirement or to a system-level trade-off[cite: 2].

---

## 3. Frequency Selection
A preliminary operating frequency of **3 GHz** was selected ($\lambda = 0.1 \text{ m}$)[cite: 1, 2]. The choice represents a compromise between antenna aperture, angular resolution, Doppler sensitivity, RF technology practicality, and atmospheric propagation[cite: 2]. A detailed site-specific propagation analysis would be required in a subsequent design phase[cite: 2].

The 0.1 m wavelength gives a direct Doppler relationship of:
$$f_D = \frac{2 v_r}{\lambda} = 20 v_r \quad (\text{Hz for } v_r \text{ in m/s})$$[cite: 1, 2]

This provides useful sensitivity to radial velocity[cite: 2].

---

## 4. Waveform Design
An LFM chirp was selected because it allows a relatively long, high-energy pulse to be transmitted while achieving fine range resolution after matched filtering[cite: 2]. 

* **Bandwidth ($B$):** $15 \text{ MHz}$[cite: 1, 2]
* **Pulse Duration ($\tau$):** $100 \ \mu\text{s}$[cite: 1, 2]
* **Time-Bandwidth Product ($BT$):** $1500$[cite: 1, 2]
* **Theoretical Range Resolution ($\Delta R$):** 
  $$\Delta R = \frac{c}{2B} = \frac{3 \times 10^8}{2(15 \times 10^6)} = 10 \text{ m}$$[cite: 1, 2]
* **Ideal Pulse-Compression Gain:** 
  $$10 \log_{10}(1500) \approx 31.8 \text{ dB}$$[cite: 2]

This choice demonstrates the interaction between waveform duration, bandwidth, transmitted energy, and range resolution rather than treating range resolution as an isolated specification[cite: 2].

---

## 5. Initial Link Budget
For a 10 cm diameter metallic sphere, a nominal RCS of $\sigma = \pi(0.05)^2 = 0.00785 \text{ m}^2$ is used[cite: 1, 2]. This is an idealized assessment assumption; the RCS of a real object may vary significantly with geometry, attitude, and frequency[cite: 2].

The monostatic radar equation is used to estimate the received echo:
$$P_r = \frac{P_t G_t G_r \lambda^2 \sigma}{(4\pi)^3 R^4}$$[cite: 2]

With $P_t = 15 \text{ kW}$, $G_t = G_r \approx 52.2 \text{ dBi}$, $\lambda = 0.1 \text{ m}$, $\sigma = 0.00785 \text{ m}^2$, and $R = 800 \text{ km}$, the estimated received power is approximately **$-135.8 \text{ dBm}$**[cite: 1, 2].

For a 15 MHz bandwidth and 3 dB receiver noise figure, the receiver noise estimate is approximately **$-99.2 \text{ dBm}$**[cite: 1, 2]. The resulting raw SNR is approximately **$-36.6 \text{ dB}$**[cite: 1, 2]. This shows that detection cannot depend on instantaneous SNR and motivates pulse compression and coherent processing[cite: 2].

---

## 6. Antenna Design
A **16 m parabolic reflector** is proposed[cite: 1, 2]. With an assumed 65% aperture efficiency, antenna gain is calculated as:
$$G = \eta \left( \frac{\pi D}{\lambda} \right)^2 \approx 52.2 \text{ dBi}$$[cite: 1, 2]

The approximate half-power beamwidth (HPBW) is:
$$\text{HPBW} \approx \frac{70 \lambda}{D} \approx 0.44^\circ$$[cite: 1, 2]

A two-axis azimuth/elevation mount is proposed[cite: 1, 2]. For angular tracking, a monopulse architecture is preferred because it produces instantaneous angular-error information relative to boresight[cite: 1, 2]. A preliminary $\le 0.05^\circ$ measurement target is retained as a design target requiring detailed antenna/feed and calibration verification[cite: 1, 2].

---

## 7. Transmitter Design
The preliminary peak RF output is **15 kW**[cite: 1, 2]. A modular solid-state architecture is preferred, using multiple PA modules with power combining[cite: 1, 2]. A 60% PA efficiency is used as a preliminary assumption for system power and thermal estimates[cite: 1, 2].

Engineering challenges include power combining, thermal management, impedance matching, monitoring, protection, redundancy, and integration with the T/R network[cite: 1, 2]. Individual RF amplifier, driver, and protection components can potentially be selected from COTS technology[cite: 2].

---

## 8. Receiver Design
The proposed receiver chain is:
$$\text{Antenna} \rightarrow \text{T/R Protection} \rightarrow \text{RF Filtering} \rightarrow \text{LNA} \rightarrow \text{Mixer / Downconversion} \rightarrow \text{IF Filtering / Amplification} \rightarrow \text{ADC} \rightarrow \text{FPGA / DSP}$$[cite: 1, 2]

A 3 dB receiver noise figure is assumed for the conceptual link budget[cite: 1, 2]. The receiver must recover from high-power transmission, preserve coherent phase information, and provide sufficient dynamic range for weak echoes[cite: 2].

---

## 9. Signal Processing
The digital processing chain is:
$$\text{Digital Downconversion} \rightarrow \text{Matched Filter / Pulse Compression} \rightarrow \text{Range Bins} \rightarrow \text{Coherent Integration} \rightarrow \text{Doppler FFT} \rightarrow \text{Range-Doppler Map} \rightarrow \text{CFAR} \rightarrow \text{Detection}$$[cite: 1, 2]

With 100 coherent pulses, ideal coherent integration gain is **20 dB**[cite: 1, 2]. Combined with approximately **31.8 dB pulse-compression gain**, the ideal processed SNR is approximately **15.2 dB**[cite: 1, 2]. Allocating 5 dB for implementation and processing losses leaves approximately **10.2 dB effective preliminary detection margin**[cite: 1, 2].

---

## 10. Doppler, PRF, and Velocity Resolution
For $R_{\text{max}} = 800 \text{ km}$, the simple unambiguous-range constraint gives:
$$\text{PRF}_{\text{max}} \approx 187.5 \text{ Hz}$$[cite: 1, 2]

With 100 pulses, the coherent processing interval (CPI) is approximately **0.533 s**[cite: 1, 2]. The theoretical velocity resolution is therefore:
$$\Delta v \approx \frac{\lambda}{2 T_{\text{CPI}}} \approx 0.094 \text{ m/s}$$[cite: 1, 2]

This comfortably meets the $< 1 \text{ m/s}$ requirement[cite: 1, 2]. However, the low PRF produces Doppler ambiguity for LEO velocities[cite: 1, 2]. The proposed operational solution is staggered/multiple-PRF operation and ambiguity-resolution processing[cite: 1, 2].

---

## 11. Detection and Tracking
Detection is followed by track initiation, range/Doppler/angle measurement, filtering, and prediction[cite: 1, 2]. A closed-loop Az/El servo uses predicted target position and angle error to keep the antenna pointed at the object[cite: 1, 2].

$$\text{Detection} \rightarrow \text{Acquisition} \rightarrow \text{Measurement} \rightarrow \text{Tracking Filter} \rightarrow \text{Prediction} \rightarrow \text{Servo Control} \rightarrow \text{Antenna} \rightarrow \text{Next Measurement}$$[cite: 1, 2]

---

## 12. Orbit Determination Support
The radar provides time-tagged measurement vectors:
$$z_k = [R, \dot{R}, \text{Az}, \text{El}]^T$$[cite: 1, 2]

Repeated observations are transformed into an Earth-referenced coordinate system and used to estimate the six-state vector $[x, y, z, v_x, v_y, v_z]^T$[cite: 1, 2]. An **Extended Kalman Filter (EKF)** is proposed as a preliminary state-estimation architecture[cite: 1, 2].

---

## 13. Sensitivity Analysis and Design Trades

| Parameter Variation | Approx. Detection Margin |
| :--- | :--- |
| **Baseline (800 km, 10 cm, 16m, 15kW)** | **10.2 dB**[cite: 1, 2] |
| Range = 700 km | ~12.5 dB[cite: 1, 2] |
| Range = 500 km | ~18.4 dB[cite: 1, 2] |
| RCS = 0.5× nominal (0.0039 m²) | ~7.2 dB[cite: 1, 2] |
| RCS = 2.0× nominal (0.0157 m²) | ~13.2 dB[cite: 1, 2] |
| Tx Power = 10 kW | ~8.4 dB[cite: 1, 2] |
| Tx Power = 20 kW | ~11.5 dB[cite: 1, 2] |
| Antenna = 14 m | ~8.1 dB[cite: 1, 2] |
| Antenna = 18 m | ~12.2 dB[cite: 1, 2] |

The sensitivity study shows that received power varies as $R^{-4}$, while for a monostatic system using the same antenna aperture, power scales as $D^4$[cite: 1, 2]. Range and antenna aperture are therefore the strongest performance drivers[cite: 1, 2].

---

## 14. Custom vs. COTS Engineering

| Subsystem | Approach |
| :--- | :--- |
| RF Synthesizer / Exciter | COTS + custom control[cite: 2] |
| ADC / FPGA Hardware | COTS hardware + custom firmware[cite: 2] |
| LNA / Mixer / IF Chain | COTS candidates[cite: 2] |
| PA Modules | COTS candidates[cite: 2] |
| 15 kW PA Assembly & Combining | Custom integration[cite: 2] |
| Power Combiner / T-R Network | Custom high-power integration[cite: 2] |
| 16 m Reflector & Monopulse Feed | Custom[cite: 2] |
| Az/El Mount & Servo Control | Custom integration[cite: 2] |
| DSP (Pulse Compression / CFAR) | Custom DSP/software[cite: 2] |
| Tracking & Orbit Estimation | Custom software[cite: 2] |

---

## 15. Limitations and Further Work
1. Detailed electromagnetic modeling and antenna/feed pattern characterization[cite: 2].
2. Site-specific atmospheric propagation and weather-loss analysis[cite: 2].
3. Optimization of staggered/multiple PRFs for range-Doppler ambiguity resolution[cite: 2].
4. High-power PA thermal management and T/R switch protection design[cite: 2].
5. Orbit determination validation using realistic satellite trajectories[cite: 2].

---

## 16. Final Conclusion
The proposed **3 GHz, 15 kW, 16 m monostatic radar** provides a traceable preliminary architecture for detection and tracking of a 10 cm-class LEO object at ranges up to 800 km[cite: 1, 2]. Under the stated assumptions, the design achieves **10 m range resolution**, **~0.094 m/s velocity resolution**, and **~10.2 dB effective detection margin**[cite: 1, 2].
