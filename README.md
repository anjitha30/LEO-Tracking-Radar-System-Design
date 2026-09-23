# LEO-Tracking-Radar-System-Design
# Preliminary Conceptual Design of a 3 GHz Ground-Based LEO Tracking Radar

![Domain](https://img.shields.io/badge/Domain-RF%20%26%20Radar%20Systems-blue)
![Platform](https://img.shields.io/badge/Application-LEO%20Space%20Debris%20Tracking-orange)
![Status](https://img.shields.io/badge/Status-Completed%20System%20Assessment-green)

## Executive Summary
This repository contains the system architecture, mathematical derivations, link budget model, and trade-off analysis for a preliminary ground-based monostatic radar capable of detecting and continuously tracking Low Earth Orbit (LEO) objects (such as space debris) at altitudes between **500 km and 800 km**. 

The primary design target evaluated is a metallic spherical object with a **10 cm diameter** (nominal RCS $\sigma \approx 0.00785 \text{ m}^2$).

---

## Key Performance Requirements vs. Design Results

| Parameter | Requirement | Designed / Achieved Metric | Status |
| :--- | :--- | :--- | :--- |
| **Operating Altitude** | $500 \text{--} 800 \text{ km}$ | $800 \text{ km}$ (Worst-case design point) | Verified |
| **Target Size / RCS** | $10 \text{ cm}$ sphere | $\sigma = 0.00785 \text{ m}^2$ | Verified |
| **Effective Detection Margin**| $\ge 10 \text{ dB}$ | **$10.2 \text{ dB}$** (at $800 \text{ km}$) | **Met** |
| **Range Resolution ($\Delta R$)**| $\le 10 \text{ m}$ | **$10 \text{ m}$** ($15 \text{ MHz}$ LFM Bandwidth) | **Met** |
| **Velocity Resolution ($\Delta v$)**| $< 1 \text{ m/s}$ | **$\approx 0.094 \text{ m/s}$** ($100\text{-pulse CPI}$) | **Met** |

---

## Proposed System Architecture
[ Waveform Generator (LFM Chirp) ]
│
▼
[ RF Exciter / Driver ]
│
▼
[ 15 kW Solid-State PA ]
│
▼
[ High-Power T/R Switch ] ◄─────► [ 16 m Parabolic Antenna (G = 52.2 dBi) ]
│                                      │
▼                                      ▼
[ Low-Noise Amplifier ]                     [ LEO Target ]
│
▼
[ Quadrature Downconverter ]
│
▼
[ ADC / FPGA Processing ]
│
▼
[ Matched Filter / Pulse Compression ]
│
▼
[ Coherent Integration (100 Pulses) ] ──► [ Doppler FFT ] ──► [ CFAR Detection ]
│
▼
[ Extended Kalman Filter]

---

## Detailed System Parameters

### 1. RF & Waveform Design
* **Operating Frequency ($f_0$):** $3 \text{ GHz}$ ($S\text{-band}$, $\lambda = 0.1 \text{ m}$)
* **Waveform Type:** Linear Frequency Modulation (LFM Chirp)
* **Chirp Bandwidth ($B$):** $15 \text{ MHz}$
* **Pulse Width ($\tau$):** $100 \ \mu\text{s}$
* **Time-Bandwidth Product ($BT$):** $1500$ ($\approx 31.8 \text{ dB}$ processing gain)
* **Unambiguous PRF:** $187.5 \text{ Hz}$ (Supports staggered PRF for Doppler ambiguity resolution)

### 2. Antenna & High-Power Transmitter
* **Antenna Aperture:** $16 \text{ m}$ Parabolic Reflector ($\eta = 65\%$)
* **Antenna Gain ($G$):** $\approx 52.2 \text{ dBi}$
* **Half-Power Beamwidth (HPBW):** $\approx 0.44^\circ$
* **Tracking Architecture:** Two-axis Az/El mount with Monopulse angle estimation ($\le 0.05^\circ$ target accuracy)
* **Peak Output Power ($P_t$):** $15 \text{ kW}$ (Modular GaN Solid-State Power Amplifier topology)

---

## Link Budget & Detection Analysis ($800 \text{ km}$ Design Point)

$$\text{Received Echo Power } (P_r) = \frac{P_t \cdot G_t \cdot G_r \cdot \lambda^2 \cdot \sigma}{(4\pi)^3 \cdot R^4 \cdot L}$$

+-------------------------------------------------------------+
| Parameter                        | Value                    |
+-------------------------------------------------------------+
| Design Range (R)                 | 800 km                   |
| Target RCS (σ)                   | 0.00785 m²               |
| Received Echo Power (Pr)         | -135.8 dBm               |
| Receiver Noise Floor (kTB + 3dB NF)| -99.2 dBm              |
+-------------------------------------------------------------+
| Raw SNR                          | -36.6 dB                 |
| (+) Pulse Compression Gain       | +31.8 dB                 |
| (+) 100-Pulse Coherent Integration| +20.0 dB                 |
+-------------------------------------------------------------+
| Ideal Processed SNR              | +15.2 dB                 |
| (-) Implementation Losses        | -5.0 dB                  |
+-------------------------------------------------------------+
| Effective Preliminary Margin     | +10.2 dB                 |
+-------------------------------------------------------------+


---

## Sensitivity Analysis

The system performance was evaluated across range variations, target size uncertainties, and power scaling:

* **Range Dependency ($R = 500 \text{ km}$ vs. $800 \text{ km}$):** Margin increases from $10.2 \text{ dB}$ to **$18.4 \text{ dB}$** due to the steep $R^{-4}$ path loss reduction.
* **Aperture Scaling ($14\text{ m}$ vs. $18\text{ m}$):** Received power scales as $\approx D^4$ for monostatic dual-aperture configurations, yielding $8.1 \text{ dB}$ ($14\text{ m}$) to $12.2 \text{ dB}$ ($18\text{ m}$) margins.
* **RCS Uncertainty ($0.5\times$ to $2.0\times$):** Yields an operational margin swing between $7.2 \text{ dB}$ and $13.2 \text{ dB}$.

---

## Repository Structure
├── docs/
│   ├── LEO_Radar_Detailed_Engineering_Report.pdf   # Complete 17-section system report
│   └── LEO_Radar_4_Page_Assessment.pdf            # Executive assessment overview
└── README.md                                      # System summary and link budget breakdown

---

## Author & Contact

**Anjitha Chandran**  
*Electrical & Electronics Engineer | Space Systems & RF Researcher*  
* **LinkedIn:** [https://www.linkedin.com/in/anjitha-chandran/](#)  
