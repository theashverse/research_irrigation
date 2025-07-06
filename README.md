#  Smart Soil Irrigation Simulation using Physics and IoT Concepts

This project simulates how water moves through soil using simple physics-based models. It also mimics IoT sensor noise to show how real smart irrigation systems might behave in practice.

The goal is to demonstrate how science and technology (Python + physics + sensors) can be used to improve water usage in agriculture.

---

## 📌 Features

- Simulates 1D vertical soil moisture diffusion using the finite difference method
- Models evaporation from the soil surface
- Adds noise to simulate IoT sensor readings
- Visualizes true vs. noisy sensor outputs
- Simple and beginner-friendly code in Python

---

## WORKING:

## Mathematical Model

Water movement and evaporation in soil are modeled by the partial differential equation (PDE):

$$
\frac{\partial \theta(z,t)}{\partial t} = D \cdot \frac{\partial^2 \theta(z,t)}{\partial z^2} - E
$$

Where:

- $\theta(z,t)$: water content at depth $z$, time $t$
- $D$: diffusion coefficient (water spreading speed)
- $E$: evaporation rate (water loss at the surface)

---

## Numerical Solution – Finite Difference Method (FDM)

Since computers cannot directly solve PDEs, we approximate derivatives on a grid.

### Grid Setup

- Soil depth divided into layers spaced by $\Delta z$
- Time divided into steps of size $\Delta t$

### Derivative Approximations

- **Time derivative:**

    ∂θ/∂t ≈ (θᵢⁿ⁺¹ − θᵢⁿ) / Δt

- **Second order derivative:**

    ∂²θ/∂z² ≈ (θᵢ₊₁ⁿ − 2θᵢⁿ + θᵢ₋₁ⁿ) / Δz²

- **Update equation after substituting into the PDE:**

    θᵢⁿ⁺¹ = θᵢⁿ + Δt · D · (θᵢ₊₁ⁿ − 2θᵢⁿ + θᵢ₋₁ⁿ) / Δz² − E · Δt



### Updating Moisture

Combining these, moisture at each layer is updated by:

$$
\theta_i^{n+1} = \theta_i^n + \Delta t \cdot D \cdot \frac{\theta_{i+1}^n - 2\theta_i^n + \theta_{i-1}^n}{\Delta z^2} - E \cdot \Delta t
$$

For the top layer (surface evaporation only):

$$
\theta_0^{n+1} = \theta_0^n - E \cdot \Delta t
$$

---

## Sensor Simulation with Noise

Real sensors have measurement errors, modeled here as:

$$
\theta_{\text{sensor}} = \theta_{\text{true}} + \text{random noise}
$$

Where the noise is small Gaussian (normal) noise.

---

## Smart Irrigation Using Proportional Controller

Irrigation amount is decided based on the error between desired and measured moisture:

$$
\text{Irrigation} = K_p \cdot (\theta_{\text{target}} - \theta_{\text{sensor}})
$$

- $\theta_{\text{target}}$: desired moisture level  
- $\theta_{\text{sensor}}$: current sensor reading  
- $K_p$: a constant that adjusts how strongly we respond

If soil is too dry, irrigation is applied proportionally.

---

###  Example – P-Controller Irrigation

You want the soil to stay at:

    θ_target = 0.4

The sensor shows:

    θ_sensor = 0.2

With a controller gain:

    Kp = 5

Then:

    Irrigation = Kp × (θ_target − θ_sensor)
               = 5 × (0.4 − 0.2)
               = 1.0

 You’ll add **1.0 units** of water.

If the soil is already wet (θ_sensor > 0.4), the value becomes negative — so you add **no water**.


## Code Logic Overview

1. Set up a 1D soil column divided into layers.  
2. Initialize moisture levels in all layers.  
3. At each time step:  
   - Update moisture using diffusion and evaporation equations.  
   - Simulate noisy sensor reading.  
   - Apply irrigation if soil moisture is below target.  
4. Plot moisture profiles over depth and time.

---


##  Technologies Used

- Python 3
- NumPy
- Matplotlib
- Physics-based modeling (Diffusion Equation)

---

##  Paper

This simulation is part of the research paper:

> **Smart Soil Irrigation Simulation Using Physics and IoT Ideas**  
> Ayush Gupta, KIIT  
> Published on Zenodo (2025)  
> [🔗 View Paper on Zenodo](https://doi.org/10.5281/zenodo.15820521)

---

##  Files in This Repo

| File | Description |
|------|-------------|
| `simulation.py` | Full simulation code |
| `moisture.png` | Soil moisture evolution plot |
| `sensor.png` | IoT sensor output vs. ground truth |
| `main.pdf` | Research paper (compiled from LaTeX) |

---

##  License

This project is licensed under **Creative Commons Attribution 4.0 (CC BY 4.0)**  
Feel free to use, modify, and share — just give proper credit.

---

##  Author

**Ayush Gupta**  
B.Tech CSE, KIIT  
📧 24052068@kiit.ac.in  
