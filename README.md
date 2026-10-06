# CPU-GPU-Validated-Radiation-Transport-for-Black-Hole-Accretion-Flows
# Hyperion-Based Computational Astrophysics

## CPU/GPU-Validated Radiation Transport for Black-Hole Accretion Flows

This project develops a computational workflow connecting **Hyperion CPU/GPU validation** with **astrophysical accretion-flow modeling, radiation transport, synthetic high-energy spectra, and observational comparison**.

The primary goal is to establish a reliable computational pipeline that can eventually be used to investigate radiation from accreting compact objects such as black holes.

---

## Research Workflow

```text
                    HYPERION
                       │
             ┌─────────┴─────────┐
             │                   │
            CPU                 GPU
             │                   │
             └─────────┬─────────┘
                       │
                 CPU/GPU Validation
                       │
              Numerical Convergence
                       │
                       ▼
              Accretion-Flow Model
                       │
          ┌────────────┼────────────┐
          │            │            │
        Density    Temperature    Magnetic Field
          │            │            │
          └────────────┼────────────┘
                       │
                       ▼
                Radiation Transport
                       │
                       ▼
                Emergent Radiation
                       │
                       ▼
                Synthetic Spectrum
                       │
                       ▼
             X-ray / High-Energy Model
                       │
                       ▼
             Chandra / XMM-Newton
                       │
                       ▼
             Model–Observation
                  Comparison
```

---

## Scientific Motivation

Radiation transport is computationally expensive, particularly when studying complex astrophysical environments such as black-hole accretion flows.

GPU acceleration provides an opportunity to perform these calculations more efficiently. However, GPU implementations must first be validated against reliable CPU calculations.

This project therefore begins with a controlled CPU/GPU comparison and progressively develops toward an astrophysical application.

The central idea is:

> **Validate the computation first, then use the validated framework to investigate astrophysical radiation.**

---

## Main Research Questions

### 1. CPU/GPU Numerical Consistency

How closely does the CPU implementation reproduce the GPU implementation for the same physical problem?

The relative difference can be evaluated using

$$
\epsilon =
\frac{|Q_{\mathrm{CPU}}-Q_{\mathrm{GPU}}|}
{|Q_{\mathrm{GPU}}|}.
$$

---

### 2. Computational Performance

How does GPU acceleration scale as the computational resolution increases?

The GPU speedup is defined as

$$
S =
\frac{T_{\mathrm{CPU}}}
{T_{\mathrm{GPU}}}.
$$

The study will investigate how this quantity changes with problem size and resolution.

---

### 3. Accretion-Flow Physics

How do the physical properties of an accretion flow influence the emitted radiation?

The model will consider quantities such as

$$
\rho(r),\qquad
T(r),\qquad
v(r),\qquad
B(r),
$$

where:

* \(\rho\) = density
* \(T\) = temperature
* \(v\) = velocity
* \(B\) = magnetic field

---

### 4. Radiation Transport

The basic radiative-transfer equation is

$$
\frac{dI_\nu}{ds}
=
j_\nu-\alpha_\nu I_\nu,
$$

where:

* \(I_\nu\) = specific intensity
* \(j_\nu\) = emissivity
* \(\alpha_\nu\) = absorption coefficient
* \(s\) = distance along the radiation path

The objective is to calculate the emergent radiation from the modeled accretion flow.

---

### 5. Observational Connection

Can the resulting synthetic spectra reproduce important features of observed high-energy emission?

The eventual comparison will use appropriate observations from facilities such as:

* Chandra
* XMM-Newton

The final goal is to connect computational models with observable astrophysical quantities.

---

# Project Structure

```text
hyperion-astrophysics/
│
├── README.md
│
├── data/
│   ├── cpu_result.csv
│   ├── gpu_result.csv
│   ├── accretion_flow.csv
│   ├── synthetic_spectrum.csv
│   └── observed_spectrum.csv
│
├── notebooks/
│   ├── 01_cpu_gpu_validation.ipynb
│   ├── 02_accretion_flow.ipynb
│   ├── 03_radiative_transfer.ipynb
│   ├── 04_synthetic_spectrum.ipynb
│   └── 05_observation_comparison.ipynb
│
├── src/
│   ├── constants.py
│   ├── accretion.py
│   ├── radiation.py
│   ├── spectrum.py
│   └── comparison.py
│
├── results/
│   ├── figures/
│   └── tables/
│
└── requirements.txt
```

---

# Computational Workflow

## Step 1 — Hyperion CPU/GPU Validation

The first stage uses the current Hyperion CPU/GPU implementations.

The initial benchmark focuses on the `SIZE=150` comparison case.

The outputs from both implementations are compared for:

* numerical agreement
* mass fractions
* energy-related quantities
* density/temperature where available
* conservation properties
* runtime

Example Python analysis:

```python
import pandas as pd
import numpy as np

cpu = pd.read_csv("data/cpu_result.csv")
gpu = pd.read_csv("data/gpu_result.csv")

comparison = pd.DataFrame()

comparison["time"] = cpu["time"]

comparison["relative_error"] = (
    np.abs(cpu["energy"] - gpu["energy"])
    / np.abs(gpu["energy"])
)

print(comparison.describe())
```

---

# Step 2 — Resolution Study

The CPU/GPU calculation will be repeated for different computational resolutions.

Example:

```text
SIZE = 50
SIZE = 100
SIZE = 150
SIZE = 300
SIZE = 600
```

For each calculation, record:

```text
resolution
CPU runtime
GPU runtime
speedup
relative error
```

The resulting data can be analyzed using Python and plotted with Matplotlib.

---

# Step 3 — Accretion-Flow Model

A controlled accretion-flow model will be developed before coupling the full physical problem to radiation transport.

A simplified density profile can be represented as

$$
\rho(r)
=
\rho_0
\left(\frac{r}{r_0}\right)^{-p}.
$$

A temperature profile can be represented as

$$
T(r)
=
T_0
\left(\frac{r}{r_0}\right)^{-q}.
$$

The model can subsequently include:

* black-hole mass
* density
* temperature
* radial velocity
* rotational velocity
* magnetic field
* optical depth

---

# Step 4 — Radiation Transport

The accretion-flow properties are used to calculate radiation transport.

The basic equation is

$$
\frac{dI_\nu}{ds}
=
j_\nu-\alpha_\nu I_\nu.
$$

The calculation will eventually provide:

$$
I_\nu,
\qquad
F_\nu,
\qquad
\nu F_\nu.
$$

The initial Python implementation is intended for numerical testing. It will progressively be replaced by physically appropriate emissivity and absorption models.

---

# Step 5 — Synthetic Spectrum

The radiation calculation will be converted into an observable spectrum.

Frequency and photon energy are related by

$$
E=h\nu.
$$

For X-ray applications, photon energy can be expressed in keV.

The final output will contain quantities such as:

```text
energy_keV
flux
uncertainty
```

and can be visualized as an energy spectrum.

---

# Step 6 — Observational Comparison

The synthetic spectrum will eventually be compared with observational data.

The general workflow is:

```text
Physical Model
      ↓
Radiative Transfer
      ↓
Synthetic Spectrum
      ↓
Instrument Response
      ↓
Observed Spectrum
      ↓
Statistical Comparison
```

Possible comparison quantities include:

* residuals
* reduced chi-square
* likelihood
* spectral slope
* luminosity
* spectral cutoff
* flux normalization

For real X-ray analysis, the appropriate instrument response and statistical treatment must be included.

---

# Python Environment

The project uses Python for data analysis, modeling, visualization, and comparison.

Recommended packages:

```text
numpy
scipy
pandas
matplotlib
jupyter
```

Install them with:

```bash
pip install numpy scipy pandas matplotlib jupyter
```

---

# Current Status

### Completed / In Development

* Hyperion CPU/GPU workflow identified
* CPU/GPU `SIZE=150` comparison defined
* Python framework for numerical comparison
* Accretion-flow prototype
* Basic velocity, density, temperature, and magnetic-field models
* Basic radiative-transfer solver
* Synthetic-spectrum prototype

### Planned

* Run the actual Hyperion CPU/GPU benchmark
* Extract and analyze real Hyperion outputs
* Perform resolution and convergence studies
* Couple the physical accretion-flow model to Hyperion
* Implement appropriate astrophysical radiation processes
* Generate physically validated synthetic spectra
* Incorporate observational X-ray data
* Compare models with Chandra/XMM-Newton observations

---

# Important Scientific Note

The early Python accretion-flow and radiation-transport calculations are **prototype/validation models**.

They should not be interpreted as final physical predictions.

The research will progressively replace simplified assumptions with physically validated models and, where appropriate, actual Hyperion calculations.

The purpose of the prototype stage is to establish and test the computational pipeline before moving to more sophisticated astrophysical simulations.

---

# Long-Term Goal

The long-term objective is to develop a reproducible computational framework:

$$
\boxed{
\text{CPU/GPU Validation}
\rightarrow
\text{Accretion Flow}
\rightarrow
\text{Radiation Transport}
\rightarrow
\text{Synthetic Spectrum}
\rightarrow
\text{Observational Comparison}
}
$$

This framework can be used to investigate the connection between the physical conditions in accretion flows and the high-energy radiation observed from compact astrophysical objects.

---

# Author

**Krishna Prasad Adhikari**
Assistant Professor of Physics
Tribhuvan University, Nepal

---

# Disclaimer

This repository is a research and development project. Results produced during the prototype stage are intended for computational testing and methodological development. Scientific conclusions will only be drawn after appropriate numerical validation and physical verification.
