TransRDM Dashboard
Resilient Infrastructure Lab · Arizona State University
An interactive browser-based simulation dashboard for exploring urban mobility, emissions, and heat exposure trajectories from 2026 to 2050 under varying built-environment, climate, and technology scenarios.
Live at: porto-longboards.github.io/unnamed-horse

Overview
TransRDM (Transport Robust Decision Making) runs an algebraic simulation engine entirely in the browser — no server, no backend. Users adjust parameters through sliders and dropdowns and see all outputs update in real time.
The simulation models a single representative agent navigating an urban environment that evolves over 24 years. Built-environment morphology is represented using Local Climate Zone (LCZ) classifications, which drive the urban heat island effect via a quadratic MRT equation. Vehicle and active-mode travel are determined by density and temperature elasticities, capped by a maximum travel budget. Emissions and heat exposure are aggregated across the agent population.

File structure
index.html      — full self-contained dashboard (HTML + CSS + JS)
RIL_Logo.png    — Resilient Infrastructure Lab logo (loaded by index.html)
README.md       — this file

How to use
Parameters
All parameters are controlled from the left sidebar, grouped into five sections.
Urban Morphology (LCZ)
Select a Local Climate Zone for the start (2026) and end (2050) of the simulation. Coefficients are linearly interpolated between the two over time, representing gradual urban morphology change. Each LCZ shows a plain-English description and its fitted quadratic MRT coefficients (a₁, a₂, b₁).
LCZLabelCharacterLCZ 1Compact high-riseDense tall buildings, heavy paving, minimal vegetationLCZ 2Compact midriseDense mid-height buildings, few treesLCZ 3Compact low-riseDense low buildings, very little vegetationLCZ 4Open high-riseTall buildings, open arrangement, some treesLCZ 5Open midriseMid-height buildings, mixed surfacesLCZ 6Open low-riseLow buildings, open layout, significant vegetation
Climate

Baseline temp (K) — reference outdoor temperature at t=0
Warming rate (°C/yr) — linear climate warming applied on top of baseline

Population & Density

Initial population (agents) — number of agents at t=0 (1–100)
Population growth rate — exponential growth rate
Initial density (pop/sq mi) — built-environment density at t=0
Density change rate (pop/sq mi/yr) — linear density change over time

Transport

Initial VMT (mi/agent/day) — vehicle miles travelled per agent at t=0
Initial AMT (mi/agent/day) — active mode travel per agent at t=0
VMT/AMT conversion factor (cf) — determines the total travel budget ceiling
VMT elasticity wrt BE density (λ₁) — how density changes VMT
VMT elasticity wrt temperature (λ₂) — how heat changes VMT

Technology & Emissions

EV adoption rate — logistic growth rate of EV fleet share
Initial EV share — fraction of fleet that is electric at t=0
Max EV share — saturation ceiling for EV adoption
ICE emissions (kgCO₂/mi) — emission factor for internal combustion vehicles
EV grid emissions (kgCO₂/mi) — emission factor for grid-powered EVs


Outputs
Charts are organized into three groups, each showing three outputs side by side.
Mobility

VMT — vehicle miles travelled per agent per day
AMT — active mode travel (walking, cycling) per agent per day

Emissions & Heat

E — total emissions across all agents (kgCO₂/day)
H — heat exposure: population-weighted active travel under thermal load (mi·K/day)
MRT — mean radiant temperature of the urban environment (K)

Context

EV share — fraction of the vehicle fleet that is electric
Population — total agent count over time
Urban density — population per square mile over time

The stat bar at the top shows final-year (2050) values for VMT, AMT, E, and H.

Scenario comparison
Click ⬡ Lock scenario in the header to snapshot the current simulation. A dashed lighter line will appear on all charts representing the locked scenario. You can then adjust any parameters freely — the live (solid) line updates while the locked (dashed) line stays fixed, enabling direct visual comparison on shared, locked y-axes.
Click ✕ Clear lock to remove the locked scenario.

Synchronized tooltips
Hovering over any chart moves a crosshair across all charts simultaneously, showing values for all outputs at the same year. This makes it easy to trace how a parameter change at one point in time ripples across all outputs.

Y-axis ranges
All y-axes are locked to fixed ranges to ensure visual comparability across scenarios:
ChartMinMaxVMT030 mi/agent/dayAMT020 mi/agent/dayE010 kgCO₂/dayH05,000 mi·K/dayMRT305345 KEV share01Population0100 agentsUrban density025,000 pop/sq mi

Model equations
MRT (Mean Radiant Temperature)
x(t)       = T_base + cc·t − 273.15
ΔMRT(t)    = a₁(t)·x² + a₂(t)·x + b₁(t) − x
ΔT(t)      = ΔMRT(t) + cc·t
MRT(t)     = T_base + ΔT(t)
where a₁(t), a₂(t), b₁(t) are linearly interpolated between the start and end LCZ coefficients.
EV adoption (logistic)
elec(t) = elecmax / (1 + (elecmax/elec₀ − 1)·exp(−k_ev·t))
Travel demand
TMT_max    = VMT₀ + AMT₀·cf
a₃         = VMT₀ / (d₀^λ₁ · ΔT₀^λ₂)
VMT(t)     = min(a₃ · dens(t)^λ₁ · ΔT(t)^λ₂,  TMT_max)
AMT(t)     = max(0, (TMT_max − VMT(t)) / cf)
Emissions & heat exposure
E(t) = pop(t) · [ (1 − elec(t))·VMT(t)·EF_ice + elec(t)·VMT(t)·EF_grid ]
H(t) = pop(t) · AMT(t) · MRT(t)

Development
The dashboard is a single static HTML file with no build step, no dependencies to install, and no server required. To develop locally:
bashgit clone https://github.com/porto-longboards/unnamed-horse.git
cd unnamed-horse
# open index.html in a browser, or serve it:
python -m http.server 8000
Deployed automatically via GitHub Pages on every push to main.

Credits
Developed by the Resilient Infrastructure Lab at Arizona State University.
