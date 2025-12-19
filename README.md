# Interplanetary Mission Design to the Uranian System Using Gravity Assist Optimization

## Project Motivation
Exploration of the Uranian system presents one of the most demanding challenges in interplanetary mission design due to the planet’s distance, long transfer times, and high arrival velocities. Despite its scientific importance, Uranus has only been visited once by Voyager 2, leaving major gaps in our understanding of ice giant formation and evolution.

The motivation for this project was to design a physically feasible, time-constrained interplanetary mission to Uranus using tools and methods representative of early-phase mission analysis. This project emphasizes trajectory design, gravity-assist modeling, and propulsion-mass coupling, all of which are central to real mission concept studies.

## Mission Architecture Overview
The mission architecture consists of three major phases:
<br />

1. **Earth departure**
2. **Jupiter gravity assist**
3. **Uranus arrival and orbit insertion**

A Jupiter flyby is used to reduce the launch energy required to reach Uranus while maintaining a total interplanetary time of flight under ten years.

<p align="center">
  <img src="https://i.imgur.com/lmMz01Q.png" height="50%" width="50%" alt="Earth-Jupiter-Uranus Trajectory Architecture"/>
  <br />
  <i>Figure 2-01: Earth–Jupiter–Uranus trajectory architecture adopted for this mission.</i>
</p>

## Trajectory Design Approach: Grid Search
Rather than solving for a single trajectory, the code performs a constrained grid search across launch dates, flyby geometry, and arrival dates. This approach was chosen to capture sensitivity to timing and avoid premature assumptions about optimality. The objective function is to minimize the powered maneuver required at Jupiter, which strongly correlates with overall mission feasibility.

### Earth–Jupiter Transfer Modeling
For each candidate Earth launch date and Jupiter flyby date, the program:
* Extracts heliocentric position and velocity vectors from ephemeris data.
* Solves **Lambert’s problem** for the Earth–Jupiter leg.
* Computes the Earth departure hyperbolic excess velocity ($v_{\infty}$).
* Evaluates the launch characteristic energy ($C_3$).

Launch $C_3$ is a critical constraint as it directly limits deliverable spacecraft mass and launch vehicle selection.

### Jupiter Gravity Assist Modeling
The Jupiter flyby is modeled as an unpowered gravity assist, parameterized by periapsis altitude and B-plane rotation.
<br />

* **Process:** The code computes incoming Jupiter-relative velocity, calculates the flyby turning angle from hyperbolic geometry, and rotates the velocity vector about Jupiter’s pole.
* **Key Insight:** Although the gravity assist provides a large change in heliocentric energy, it does not fully align the trajectory toward Uranus, necessitating a powered maneuver after the flyby.

### Jupiter–Uranus Transfer and Deep-Space Maneuver (DSM)
For each post-flyby velocity, the script solves Lambert’s problem for the Jupiter–Uranus leg and calculates the powered **Deep-Space Maneuver (DSM)** needed to match the solution. This DSM dominates the mission $\Delta V$ budget and is the primary optimization metric.

## Uranus Arrival and Orbit Insertion Design
Upon arrival, the spacecraft must be captured into a 180-day science orbit. Two orbit insertion strategies were evaluated:
<br />

1. **Inner Ring Insertion:** 2,000 km periapsis altitude.
2. **Outer Ring Insertion:** 27,500 km periapsis altitude.

<p align="center">
  <img src="https://i.imgur.com/KMshQku.png" height="80%" width="80%" alt="Uranus Orbit Insertion Requirements"/>
  <br />
  <i>Figure 4-02: Orbit insertion delta-V requirements for Uranus capture options.</i>
</p>

## Propulsion and Mass Budget Analysis
Using the **Tsiolkovsky rocket equation** and performance curves for a Falcon Heavy (expendable), the program computes the wet mass and propellant requirements.

<p align="center">
  <img src="https://i.imgur.com/CajlhkV.png" height="40%" width="40%" alt="Mission Performance Metrics"/>
  <br />
  <i>Figure 4-01: Key mission performance metrics, including launch energy, delta-V, and mass properties.</i>
</p>

**The Performance vs. Risk Trade-space:**
* **Inner-ring insertion** is more mass-efficient but operationally riskier.
* **Outer-ring insertion** is safer but incurs a significant mass penalty.

## Engineering Insights and Takeaways
<br />

* **Powered vs. Unpowered:** Gravity assists reduce, but rarely eliminate, propulsion requirements in complex multi-leg transfers.
* **Launch Constraints:** Launch energy ($C_3$) is the ultimate "gatekeeper" for deliverable science mass.
* **Sensitivity:** Early-phase grid searches are essential for understanding how small shifts in launch windows affect the total $\Delta V$ budget.
