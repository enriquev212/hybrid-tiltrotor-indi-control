# Assets

Curated public figures from the RP report, defense deck and a separately
documented post-RP elevon continuation.

## Included

- `hero/simulation-view.gif`: real simulator view of the tilt-rotor model.
- `figures/control-architecture.png`: cascaded INDI/WLS architecture.
- `figures/vehicle-frames.png`: user-provided conceptual coordinate-frame
  illustration (`Dron.png`); not an exact simulation geometry or elevon layout.
- `figures/nominal-tethered-flight.png`: nominal tethered orbit, tracking error
  and cable tension.
- `figures/cable-sensitivity-stability.png`: closed-loop stability map for
  ±15% parameter perturbations.
- `results/`: result figures used by `docs/results-summary.md`, including
  free-flight envelope, nominal tethered response, cable sensitivity,
  actuator-effort and quadrotor benchmark plots.
- `results/elevon-moment-pulse.png`: regenerated from two October 11 cable-free
  simulation reruns. Applied torque pulses, not atmospheric gusts; see the
  [extension page](../elevon-extension.md) for inherited model limitations.

The full figure set remains in the report and presentation. Raw simulation
outputs and logs are not included here.
