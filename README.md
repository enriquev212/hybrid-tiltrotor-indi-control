# INDI Control for Hybrid Tilt-Rotor UAVs

Public repository for an MSc research project on cascaded Incremental Nonlinear
Dynamic Inversion (INDI) control for a hybrid tilt-rotor UAV in free and
ground-anchored tethered flight.

The project studies how a single-wing quad-tiltrotor can coordinate rotor
thrust, rotor tilt, aerodynamic lift and attitude while rejecting the structured
disturbance introduced by a segmented tether. The control architecture combines
outer-loop guidance, inner-loop stabilization and weighted least-squares (WLS)
allocation over rotor thrusts and tilt angles.

This public version gathers the main academic deliverables and selected
technical results. The full simulator source code, tuning files and raw logs are
kept out of scope.

![Real simulation view of the tilt-rotor model](docs/assets/hero/simulation-view.gif)

**Headline results:** stable circular tracking up to ~14 m/s | 0.087 m nominal
tethered RMSE | ±15% cable/vehicle sensitivity study | 70% lower simulated RMSE
than PID in the quadrotor benchmark.

## Project Materials

| Artifact | Why open it |
| --- | --- |
| [Full report PDF](docs/report/indi-research-project-report.pdf) | Complete research project report, including modelling, controller design, simulation setup, results and bibliography. |
| [Final presentation PPTX](docs/slides/indi-hybrid-uav-defense.pptx) | Final defense deck with the visual narrative, architecture figures and main result plots. |

## Core Idea

INDI is used as an incremental control strategy: instead of relying on exact
cancellation of the full nonlinear plant, the controller corrects commands from
measured or estimated acceleration response and local control effectiveness.
That makes it well suited to a hybrid tilt-rotor whose dynamics change across
hover, transition and forward flight.

The tethered case adds the central difficulty. The cable can go slack or taut,
applies forces and moments through an offset hook point, and introduces periodic
tension disturbances. The project evaluates whether cascaded INDI with
constrained WLS allocation can keep the vehicle on a circular tethered orbit and
how sensitive that behaviour is to cable and vehicle parameters.

## Key Findings

- In free flight, the controller tracks circular references within a bounded
  envelope, limited by airspeed near 14 m/s and lateral acceleration near
  12 m/s².
- In nominal tethered flight, the UAV tracks the circular orbit accurately while
  the cable behaves as a structured periodic disturbance.
- The cable is not just an added load: it increases thrust demand in hover, but
  can reduce mean thrust in cruise by contributing inward force toward the
  anchor.
- A ±15% cable sensitivity study shows thin robustness margins. Cable stiffness,
  cable length, cable linear density and vehicle mass are stability-critical;
  damping and diameter are weaker in the tested range.
- The quadrotor benchmark supports the INDI implementation path: in simulation
  INDI reduces PID tracking RMSE substantially, while real-flight logs show
  comparable tracking with slightly lower command activity.

## Documentation

- [Research overview](docs/research-overview.md) - context, problem statement,
  modelling approach and contribution narrative.
- [Technical summary](docs/technical-summary.md) - compact description of the
  vehicle model, cascaded INDI loops and WLS allocation.
- [Results summary](docs/results-summary.md) - quantitative findings from the
  free-flight, tethered-flight and benchmark studies.
- [Contribution summary](docs/contribution-summary.md) - public contribution
  areas and repository scope.
- [References](docs/references.md) - bibliography used in the report.
- [Report notes](docs/report/README.md) - chapter map for the complete report.
- [Slides notes](docs/slides/README.md) - presentation flow for the final
  defense deck.

## Representative Figures

### Cascaded INDI/WLS Architecture

<img src="docs/assets/figures/control-architecture.png" alt="Cascaded INDI control architecture">

### Nominal Tethered Orbit

<img src="docs/assets/figures/nominal-tethered-flight.png" alt="Nominal tethered flight tracking error and cable tension">

### Cable Sensitivity and Stability

<img src="docs/assets/figures/cable-sensitivity-stability.png" alt="Closed-loop stability map under cable and vehicle parameter perturbations">

## Repository Layout

```text
.
|-- README.md
|-- NOTICE.md
|-- CITATION.cff
|-- docs/
|   |-- research-overview.md
|   |-- technical-summary.md
|   |-- results-summary.md
|   |-- contribution-summary.md
|   |-- references.md
|   |-- report/
|   |-- slides/
|   `-- assets/
```

## Public Scope

This repository focuses on public technical documentation and selected academic
deliverables. It does not include:

- full simulator source code;
- controller implementation files;
- tuning or configuration files;
- raw simulation logs or datasets;
- third-party reference PDFs.

## Citation

Citation metadata is available in [`CITATION.cff`](CITATION.cff). The repository
can be cited as the public material associated with the research project report:
**Incremental Nonlinear Dynamic Inversion (INDI) Control for Hybrid UAVs**.

## Project Team

Developed by Enrique Valverde Sacristán and José Ricardo Furiati Aguilar as an
MSc Aerospace Engineering research project at ISAE-SUPAERO and ENAC.

Supervisors: Prof. Murat Bronz, Dr. Mauro Villanueva-Aguado and Prof. Yves
Brière.

Maintainer/contact: Enrique Valverde Sacristán
([enriquev212](https://github.com/enriquev212),
[enriquevalverdesacristan@gmail.com](mailto:enriquevalverdesacristan@gmail.com)).

## Notice

No open-source license is currently included. Unless the project owners add a
license or grant permission separately, reuse and redistribution rights should
not be assumed. See [NOTICE.md](NOTICE.md).
