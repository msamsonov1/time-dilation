# High-G Optical Metrology & Dynamic Strain De-Aliasing Engine (HG-ODE)

[![Live Web Application](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-06b6d4?style=for-the-badge&logo=github)](https://msamsonov1.github.io/time-dilation/)
[![NASA SBIR](https://img.shields.io/badge/NASA%20SBIR-Phase%20I%20($225k)-blue?style=for-the-badge)](https://msamsonov1.github.io/time-dilation/)
[![Lead Organization](https://img.shields.io/badge/Proprietary%20Technology-RF%20WaveCode-emerald?style=for-the-badge)](https://msamsonov1.github.io/time-dilation/)
[![Tech Stack](https://img.shields.io/badge/Stack-HTML5%20%7C%20TailwindCSS%20%7C%20Chart.js%20%7C%20MathJax%20v3-06b6d4?style=for-the-badge)](#technical-architecture)

> **Live Deployment:** [https://msamsonov1.github.io/time-dilation/](https://msamsonov1.github.io/time-dilation/)

Developed by **RF WaveCode**, HG-ODE is an interactive, single-file multiphysics simulation and digital twin engine built for a **NASA SBIR Phase I Proposal** under solicitation topics **TX08.01 (Advanced Sensors & Clocks)** and **TX13.01 (Deep Space PNT & Optical Communications)**. 

The software models, isolates, and de-aliases severe dynamic mechanical vibrations and centrifugal strains ($>100,000\text{ G}$) from sub-femtosecond relativistic optical frequency comb signals in spaceborne atomic timing architectures.

---

## 🚀 Executive Summary

During launch phases, planetary re-entry maneuvers, and spin-stabilized flight stages, spaceborne optical cavities undergo mechanical micro-strains ($\sim 10^{-7}$) that alias directly into resonant optical beat-notes. This mechanical noise floor overwhelms subtle relativistic kinematic time dilation signatures ($\sim 10^{-13}$) by six orders of magnitude.

**RF WaveCode's HG-ODE (High-G Optical Metrology & Dynamic Strain De-Aliasing Engine)** solves this problem via an Extended Kalman Filter (EKF) digital twin that couples the **Pockels photoelastic tensor** with **real-time magnetic bearing runout telemetry**, decoupling mechanical displacement from optical phase to preserve picosecond-grade time transfer and synchronization.

```
       +-----------------------------------------------------------+
       |   Stationary Isolated Optical Breadboard Metrology Train  |
       |  (Fiber Comb, Heterodyne Photodetector, Reference Clock)  |
       +-----------------------------+-----------------------------+
                                     |
                       Optical Interrogation Path
                                     |
                                     v
       +-----------------------------------------------------------+
       |   High-Speed Vacuum Centrifuge Chamber (< 10⁻⁶ Torr)      |
       |   Active Mag-Lev Suspended Rotor (0 – 120,000 RPM)        |
       |                                                           |
       |   [Channel 1: Hub Prism (R₁)] ---> [Channel 2: Rim (R₂)]   |
       |        Low G / Near-Reference            High-G Dilated   |
       +-----------------------------+-----------------------------+
                                     |
                          Coupled Dual-Channel Data
                                     |
                                     v
       +-----------------------------------------------------------+
       |          RF WaveCode EKF Inversion & De-Aliasing          |
       |   De-mixes Micro-Strain Runout (10⁻⁷) from Phase (10⁻¹³)  |
       +-----------------------------------------------------------+
```

---

## 🔬 Core Physics & Mathematical Framework

### 1. Coupled High-G Phase Observable
The instantaneous fractional beat-note shift observed across the radial channels is formulated as:

$$\frac{\Delta f_{\text{total}}(t)}{f_0} = \underbrace{\frac{\omega^2 (R_2^2 - R_1^2)}{2c^2}}_{\text{Kinematic Time Dilation}} + \underbrace{\xi_{\text{PE}} \cdot \varepsilon_{rr}(t)}_{\text{Centrifugal Photoelastic Strain}} + \underbrace{\frac{\delta R_{\text{runout}}(t)}{R_0}}_{\text{Dynamic Bearing Runout}}$$

### 2. Photoelastic Tensor & Finite Elasticity
Centrifugal radial strain deforms the optical cavity and modulates its refractive index ellipsoid via the Pockels coefficients:

$$\Delta \left( \frac{1}{n^2} \right)_i = \sum_{j=1}^6 p_{ij}\varepsilon_j(r, \omega)$$

$$\varepsilon_{rr}(r) = \frac{\rho \omega^2}{8E} \left[ (3+\nu) R_2^2 - (1+3\nu) r^2 \right]$$

### 3. State-Space Observer & Extended Kalman Filter (EKF)
The RF WaveCode de-aliasing kernel operates continuously to estimate the true relativistic phase state vector $\hat{\mathbf{x}}$:

$$\hat{\mathbf{x}}_{k|k} = \hat{\mathbf{x}}_{k|k-1} + \mathbf{K}_k \left[ \mathbf{y}_k - \mathbf{h}(\hat{\mathbf{x}}_{k|k-1}, \mathbf{u}_k) \right]$$

$$\mathbf{h}(\mathbf{x}) = \begin{bmatrix} f_0 \left( 1 - \frac{\omega^2 (R_2^2 - R_1^2)}{2c^2} \right) + \mathbf{C}_{\text{strain}} \cdot \boldsymbol{\varepsilon}(\omega) \\ \mathbf{B}_{\text{probe}}(\delta x, \delta y) \end{bmatrix}$$

### 4. Acoustic Stress & Structural Yield Boundaries
To prevent rotor burst dynamics and account for Born rigidity limitations ($c_s \ll c$):

$$\sigma_{\text{hoop}} = \rho (\omega R_2)^2 \le \frac{\sigma_{\text{yield}}}{\text{SF}}$$

---

## 🎛️ Application Features

The interactive web dashboard is organized into four core functional modules:

### 1. Live Simulation & Telemetry HUD (`#viewSim`)
* **Real-Time Canvas Simulation:** Renders the 5-axis magnetic levitation stator, high-vacuum enclosure, optical injection paths, and spinning rotor assembly.
* **Dual-Channel Relativistic Clocks:** Displays proper time accumulation ($\tau_1$ vs. $\tau_2$) down to **femtosecond/picosecond resolution**.
* **Inter-Frame Optical Transmission Event:** When Channel 2 reaches $3.000\text{ s}$ proper time, an optical packet transmits from the rim to the hub, triggering an analytical telemetry modal detailing the accumulated time differential ($\Delta \tau$).
* **Closed-Loop Controls & Fault Injection:** Sliders for angular velocity ($0\text{–}120,000\text{ RPM}$), radial distances ($R_1, R_2$), substrate materials (Titanium, Carbon Fiber, CVD Diamond), and simulated bearing runout fault injection.
* **Phase-Space Scatter Chart (Chart.js):** Live plotting of $(v_2/c)^2$ vs. $\Delta f/f_0$ showing the theoretical kinematic curve against structural yield hazard boundaries.
* **Benchtop Subsystem Power Budget:** Displays live wattage consumption across spindle drive, mag-lev coils, turbomolecular pump, diaphragm foreline pump, optical comb, and GPU de-aliasing nodes ($885\text{ W}$ cruise, $2.45\text{ kW}$ peak).

### 2. Core Physics & Mathematical Model (`#viewPhysics`)
* 4 detailed technical cards with LaTeX formatting via **MathJax v3**.
* Covers relativistic metric transformations, Pockels photoelastic coupling, EKF formulation, and structural stress boundaries.

### 3. NASA SBIR Phase I Proposal (`#viewProposal`)
* **Solicitation:** NASA STMD TX08.01 / TX13.01 ($225,000 / 6 Months).
* **Prime Lead:** 100% small business allocation to **RF WaveCode** executing algorithmic architecture, coupled multiphysics modeling, and high-G sensor simulation.
* **Phased Milestone Roadmap:** 4 milestones spanning Months 1–6 transitioning from coupled FEM modeling to hardware testbed blueprinting.

### 4. Commercial & Nationwide ARR Breakdown (`#viewARR`)
* **$218.4M Mature Commercial ARR** model across 5 civilian aerospace and industrial verticals:
  1. Commercial Aviation GPS-Denied INUs: **$61.2M**
  2. Deep Space & LEO Optical Inter-Satellite Links: **$51.8M**
  3. High-G Launch Vehicle Stage Avionics: **$47.3M**
  4. Quantum Sensing & Optical Lattice Clocks: **$35.0M**
  5. Ultracentrifuge OEM Structural Health Monitoring: **$23.1M**
* Enterprise valuation model ($1.75\text{B}–$2.62\text{B}) based on an 8x–12x deep-tech software multiple.

---

## 🛠️ Technical Architecture

This application strictly implements a **zero-dependency, single-file HTML5/ES6 architecture**:

* **Rendering Engine:** Vanilla HTML5 Canvas API with High-DPI screen scaling.
* **Styling:** [Tailwind CSS CDN](https://tailwindcss.com/) with high-contrast aerospace dark theme (`#05070d`).
* **Telemetry Charting:** [Chart.js CDN](https://www.chartjs.org/) with real-time scatter and line dataset streams.
* **Mathematical Typography:** [MathJax v3](https://www.mathjax.org/) with client-side dynamic typesetting on tab transitions.
* **Vector Icons:** [Lucide Icons](https://lucide.dev/).

---

## 📥 Local Development & Quickstart

Because the entire application is self-contained in a single file, running it locally requires no build steps or package installations:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/msamsonov1/time-dilation.git
   cd time-dilation
   ```

2. **Run with any static local web server:**
   ```bash
   # Using Python 3:
   python3 -m http.server 8080

   # Using Node.js (npx serve):
   npx serve .

   # Or open index.html directly in any modern browser:
   open index.html
   ```

3. **Navigate to:** `http://localhost:8080`

---

## 🏢 Organization & Contacts

* **Prime Contractor:** **RF WaveCode** (Algorithms, Multiphysics, GPU Kernels, Embedded Flight Software)
* **Program Topic:** NASA SBIR Phase I TX08.01 / TX13.01
* **Inquiries & Licensing:** [support@rfwavecode.com](mailto:support@rfwavecode.com)

---

CONFIDENTIAL & PROPRIETARY 2026
ALL RIGHTS RESERVED — RFWAVECODE


Commercial Licensing, Technical Partnerships & STTR Inquiries:
Contact: Mikhail Samsonov / Parker O'Shell
Email: support@rfwavecode.com
Website: www.rfwavecode.com
Office: 1980 N Atlantic Ave., Ste 213, Cocoa Beach, FL 32931

