<div align="center">

<img src="RIL_Logo.png" alt="Resilient Infrastructure Lab" width="90" />

# TransRDM Dashboard

**Transport Robust Decision Making**

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-8c1f3f?style=flat-square&logo=github)](https://porto-longboards.github.io/unnamed-horse/)
[![Resilient Infrastructure Lab](https://img.shields.io/badge/RIL-Arizona%20State%20University-FFC627?style=flat-square)](https://isearch.asu.edu/)
![No dependencies](https://img.shields.io/badge/dependencies-none-brightgreen?style=flat-square)
![Single file](https://img.shields.io/badge/deployment-single%20HTML-blue?style=flat-square)

An interactive browser-based simulation dashboard for exploring urban mobility, emissions, and heat exposure trajectories from **2026 to 2050** under varying built-environment, climate, and technology scenarios.

[**→ Open the dashboard**](https://porto-longboards.github.io/unnamed-horse/)

</div>

---

## Overview

TransRDM runs an algebraic simulation engine entirely in the browser — no server, no backend, no build step. Users adjust parameters through sliders and dropdowns and see all 8 output charts update in real time.

The simulation models a population of agents navigating an urban environment that evolves over 24 years. Built-environment morphology is represented using **Local Climate Zone (LCZ)** classifications, which drive the urban heat island effect via a calibrated quadratic MRT equation. Vehicle and active-mode travel are determined by density and temperature elasticities, capped by a maximum travel budget. Emissions and heat exposure are then aggregated across the population.

---

## Features

| Feature | Description |
|---|---|
| 🏙️ **LCZ morphology transition** | Select start and end Local Climate Zones; coefficients interpolate linearly over 24 years |
| 📊 **8 real-time charts** | Grouped into Mobility, Emissions & Heat, and Context — all side by side |
| 🔒 **Scenario lock** | Snapshot any run as a dashed overlay to compare against a new parameter set |
| 🎯 **Synchronized tooltips** | Hovering any chart moves the crosshair across all charts simultaneously |
| 📏 **Locked y-axes** | Fixed axis ranges ensure visual comparability across scenarios |
| 🌵 **RIL dark theme** | ASU maroon accent, desert color palette |

---

## File Structure

```
unnamed-horse/
├── index.html       # Full self-contained dashboard (HTML + CSS + JS)
├── RIL_Logo.png     # Resilient Infrastructure Lab logo
└── README.md        # This file
```

---

## How to Use

### Parameters

All parameters live in the left sidebar, organized into five sections.

<details>
<summary><strong>🏙️ Urban Morphology (LCZ)</strong></summary>

Select a Local Climate Zone for the **start (2026)** and **end (2050)** of the simulation. Coefficients `a₁`, `a₂`, `b₁` are linearly interpolated between the two over time, representing gradual urban morphology change (e.g. densification or greening).

| LCZ | Type | Character |
|-----|------|-----------|
| LCZ 1 | Compact high-rise | Dense tall buildings, heavy paving, minimal vegetation |
| LCZ 2 | Compact midrise | Dense mid-height buildings, few trees |
| LCZ 3 | Compact low-rise | Dense low buildings, very little vegetation |
| LCZ 4 | Open high-rise | Tall buildings, open arrangement, some trees |
| LCZ 5 | Open midrise | Mid-height buildings, mixed surfaces and vegetation |
| LCZ 6 | Open low-rise | Low buildings, open layout, significant vegetation cover |

</details>

<details>
<summary><strong>🌡️ Climate</strong></summary>

| Parameter | Unit | Description |
|-----------|------|-------------|
| Baseline temp | K | Reference outdoor temperature at t = 0 |
| Warming rate | °C/yr | Linear climate warming added on top of baseline |

</details>

<details>
<summary><strong>👥 Population & Density</strong></summary>

| Parameter | Unit | Description |
|-----------|------|-------------|
| Initial population | agents | Number of agents at t = 0 (range: 1–100) |
| Population growth rate | — | Exponential growth rate |
| Initial density | pop/sq mi | Built-environment density at t = 0 |
| Density change rate | pop/sq mi/yr | Linear density change over the simulation |

</details>

<details>
<summary><strong>🚗 Transport</strong></summary>

| Parameter | Unit | Description |
|-----------|------|-------------|
| Initial VMT | mi/agent/day | Vehicle miles travelled per agent at t = 0 |
| Initial AMT | mi/agent/day | Active mode travel per agent at t = 0 |
| Conversion factor (cf) | — | VMT/AMT conversion; sets the total travel budget ceiling |
| λ₁ | — | VMT elasticity with respect to built-environment density |
| λ₂ | — | VMT elasticity with respect to temperature |

</details>

<details>
<summary><strong>⚡ Technology & Emissions</strong></summary>

| Parameter | Unit | Description |
|-----------|------|-------------|
| EV adoption rate | — | Logistic growth rate of EV fleet share |
| Initial EV share | fraction | Share of fleet that is electric at t = 0 |
| Max EV share | fraction | Saturation ceiling for EV adoption |
| ICE emissions | kgCO₂/mi | Emission factor for internal combustion vehicles |
| EV grid emissions | kgCO₂/mi | Emission factor for grid-powered EVs |

</details>

---

### Outputs

Charts are grouped into three rows, each with three panels side by side.

| Group | Output | Unit | Description |
|-------|--------|------|-------------|
| **Mobility** | VMT | mi/agent/day | Vehicle miles travelled per agent |
| | AMT | mi/agent/day | Active mode travel per agent |
| **Emissions & Heat** | E | kgCO₂/day | Total emissions across all agents |
| | H | mi·K/day | Heat exposure: active travel weighted by thermal load |
| | MRT | K | Mean radiant temperature of the urban environment |
| **Context** | EV share | fraction | Share of vehicle fleet that is electric |
| | Population | agents | Total agent count |
| | Urban density | pop/sq mi | Population density |

The **stat bar** at the top of the chart area shows final-year (2050) values for VMT, AMT, E, and H.

---

### Scenario Comparison

1. Configure a baseline scenario using the sidebar controls
2. Click **⬡ Lock scenario** in the header — a dashed, lighter-colored line appears on all charts representing this baseline
3. Adjust any parameters — the solid line updates live while the dashed line stays fixed
4. Compare the two scenarios across all charts on shared, locked y-axes
5. Click **✕ Clear lock** to remove the baseline

---

### Y-Axis Ranges

All y-axes are locked to fixed ranges to ensure visual comparability across scenarios:

| Chart | Min | Max |
|-------|-----|-----|
| VMT | 0 | 30 mi/agent/day |
| AMT | 0 | 20 mi/agent/day |
| E | 0 | 10 kgCO₂/day |
| H | 0 | 5,000 mi·K/day |
| MRT | 305 | 345 K |
| EV share | 0 | 1 |
| Population | 0 | 100 agents |
| Urban density | 0 | 25,000 pop/sq mi |

> To adjust a range, edit the `Y_RANGES` object near the top of the `<script>` section in `index.html`.

---

## Model Equations

<details>
<summary><strong>MRT — Mean Radiant Temperature</strong></summary>

LCZ coefficients `a₁(t)`, `a₂(t)`, `b₁(t)` are linearly interpolated between the start and end LCZ over the simulation period.

```
x(t)    = T_base + cc·t − 273.15
ΔMRT(t) = a₁(t)·x² + a₂(t)·x + b₁(t) − x
ΔT(t)   = ΔMRT(t) + cc·t
MRT(t)  = T_base + ΔT(t)
```

</details>

<details>
<summary><strong>EV Adoption — Logistic growth</strong></summary>

```
elec(t) = elecmax / (1 + (elecmax / elec₀ − 1) · exp(−k_ev · t))
```

</details>

<details>
<summary><strong>Travel Demand</strong></summary>

```
TMT_max = VMT₀ + AMT₀ · cf
a₃      = VMT₀ / (d₀^λ₁ · ΔT₀^λ₂)

VMT(t)  = min( a₃ · dens(t)^λ₁ · ΔT(t)^λ₂ , TMT_max )
AMT(t)  = max( 0, (TMT_max − VMT(t)) / cf )
```

</details>

<details>
<summary><strong>Emissions & Heat Exposure</strong></summary>

```
E(t) = pop(t) · [ (1 − elec(t)) · VMT(t) · EF_ice + elec(t) · VMT(t) · EF_grid ]
H(t) = pop(t) · AMT(t) · MRT(t)
```

</details>

---

## Local Development

No build tools needed. Clone and open:

```bash
git clone https://github.com/porto-longboards/unnamed-horse.git
cd unnamed-horse

# Option A — open directly in browser
open index.html

# Option B — serve locally (avoids any file:// quirks)
python -m http.server 8000
# then visit http://localhost:8000
```

Deployed automatically to GitHub Pages on every push to `main`.

---

## Credits

Developed by the [**Resilient Infrastructure Lab**](https://isearch.asu.edu/) at **Arizona State University**.
