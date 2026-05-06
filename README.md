TransRDM: Urban Morphology & Mobility Dashboard

An interactive systems-dynamics dashboard for simulating the relationships between urban form (Local Climate Zones), climate change, and mobility transitions.

🚀 Live Demo

[Link to your GitHub Pages URL will go here]

🛠 Features

Full Parameter Control: Manually define urban morphology coefficients ($a_1, a_2, b_1$).

Dynamic Transition: Simulates a 30-year shift from a "Start" urban state to an "End" urban state.

Climate Integration: Adjustable baseline temperatures and global warming rates.

Mobility Modeling: Calculations for VMT (Vehicle Miles Traveled) vs. AMT (Active Mobility Traveled) based on thermal comfort.

Real-time Visualization: Powered by Chart.js for instant feedback on algebraic re-calculations.

📊 Mathematical Logic

The dashboard uses the following core logic:

Surface Temperature: Calculated using a quadratic relationship for Mean Radiant Temperature (MRT) based on LCZ constants.

EV Adoption: Modeled using a Sigmoid growth function controlled by the $k_{ev}$ parameter.

Exposure: A product of Active Mobility duration, Temperature, and Density Sensitivity ($\beta$).

📝 How to Use

Define Morphology: Enter the coefficients for your starting urban form and target urban form.

Adjust Environment: Set the baseline climate and the projected annual warming rate.

Simulate Tech: Adjust the EV adoption speed to see the impact on net emissions.

Observe Results: View the interaction between rising heat and the shift from car-dependency to active transit.

Built for urban researchers and planners to explore socio-spatial climate scenarios.
