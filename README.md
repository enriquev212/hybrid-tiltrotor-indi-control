# INDI Control for Hybrid Tilt-Rotor UAVs

Compact public entry point for a Research Project on cascaded Incremental Nonlinear Dynamic Inversion (INDI) control for hybrid tilt-rotor UAVs.

This repository is intended for quick external review: it includes the full RP report, the final defense deck, a short technical summary, and a small set of representative visuals. It intentionally omits the full simulator source, detailed controller implementation, tuning files, and raw logs.

![Real simulation view of the tilt-rotor model](docs/assets/hero/simulation-view.gif)

## Project Scope

Hybrid tilt-rotor UAVs combine rotor-borne hover capability with fixed-wing lift in forward flight. This makes them attractive for longer-endurance missions, but difficult to control: the vehicle changes behavior across hover, transition, and cruise, while actuator limits and aerodynamic effects strongly influence the control allocation problem.

The project studied:

- 6 DoF rigid-body dynamics of a fixed-wing quad-tiltrotor;
- cascaded INDI control for trajectory tracking;
- weighted least-squares actuator allocation;
- aerodynamic lift and actuator-limit effects;
- wind and tether/cable disturbances;
- benchmark comparisons and cooperative payload-transport concepts.

## Contribution

The public material focuses on the parts that can be shown without releasing the full collaborative implementation:

- cascaded INDI control structure;
- mapping from position/velocity tracking errors to virtual acceleration commands;
- local control-effectiveness modelling;
- weighted least-squares allocation under actuator bounds and preferences;
- interpretation of tracking, control-effort, and stability results;
- selected figures prepared from the Research Project material.

## Documentation

| Document | Purpose |
|---|---|
| [Full report](docs/report/indi-research-project-report.pdf) | Complete Research Project report |
| [Defense deck](docs/slides/indi-hybrid-uav-defense.pptx) | Final PowerPoint defense presentation |
| [Contribution summary](docs/contribution-summary.md) | Role and contribution scope |
| [Technical summary](docs/technical-summary.md) | Architecture-level technical explanation |

## Selected Figures

<table>
  <tr>
    <td width="50%">
      <img src="docs/assets/figures/control-architecture.png" alt="INDI control architecture">
      <br><strong>Cascaded INDI architecture</strong>
    </td>
    <td width="50%">
      <img src="docs/assets/figures/baseline-cruise.png" alt="Cruise baseline tracking">
      <br><strong>Cruise tracking response</strong>
    </td>
  </tr>
</table>

Only the most representative visuals are shown on this page. The full report and defense deck are included above; implementation files remain excluded.

## Omitted Material

This public overview does not include:

- full Python simulator source;
- detailed controller implementation files;
- configuration and tuning files;
- raw simulation logs or datasets;
- third-party reference papers;
- collaborator files that are not necessary for public context.

The goal is to show the engineering content of the project without publishing material that should remain internal to the collaboration.
