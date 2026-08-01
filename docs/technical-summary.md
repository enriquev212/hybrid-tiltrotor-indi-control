# Technical Summary

## Problem

Hybrid tilt-rotor UAVs must operate across hover, transition, and cruise. Their control problem is difficult because thrust direction, aerodynamic lift, actuator limits, and vehicle attitude all interact. A controller must track a trajectory while allocating commands across rotor speed, rotor tilt, and attitude-related degrees of freedom.

## Approach

The project studied a cascaded Incremental Nonlinear Dynamic Inversion (INDI) architecture.

At a high level:

1. Position and velocity errors are converted into a desired virtual acceleration.
2. Acceleration feedback is used to form an incremental correction.
3. A local control-effectiveness model relates command increments to acceleration response.
4. A weighted least-squares allocator selects feasible actuator-command increments while respecting limits and preferences.
5. The resulting commands are passed to the lower-level attitude and actuator loop.

## Why INDI

INDI is useful for hybrid vehicles because it works incrementally around the current operating point. This is valuable when vehicle dynamics vary across flight regimes and a single fixed linear model is not sufficient.

## Evaluated Aspects

The included public figures summarize:

- cascaded INDI control architecture;
- hover, transition, and cruise regimes;
- benchmark tracking behavior;
- cruise trajectory tracking;
- cruise control effort.

## Redaction Note

Detailed source code, tuning values, internal implementation files, raw logs, and complete project deliverables are intentionally omitted from this public overview.
