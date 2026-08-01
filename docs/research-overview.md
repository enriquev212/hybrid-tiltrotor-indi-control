# Research Overview

## Context

Hybrid UAVs aim to combine vertical take-off and landing with efficient forward flight. This makes tilt-rotor and quadplane configurations attractive for inspection, mapping, surveillance, logistics and other missions where hover precision and cruise endurance are both useful.

The long-term motivation of this RP is cooperative aerial transport: multiple hybrid UAVs carrying a suspended payload through cables. The report studies a prerequisite building block for that goal: a single hybrid tilt-rotor UAV connected to a ground anchor by a tether.

## Problem

The control problem is difficult for two reasons.

First, the vehicle changes character across the flight envelope. In hover it behaves closer to a multirotor; in cruise the wing contributes significantly to lift; during transition the rotor tilt, thrust level and aerodynamic state all change the effectiveness of the actuators.

Second, the tether is not a simple constant load. It can go slack or taut, applies forces and moments through an offset hook point, and introduces oscillatory cable tension. These effects can change tracking error, actuator demand and closed-loop stability.

## Modelling Approach

The report builds a unified nonlinear plant model with:

- rigid-body translation and rotation;
- rotor thrust and tilt-angle mapping;
- fixed-wing aerodynamic forces and moments;
- actuator dynamics and physical limits;
- segmented ground-anchored cable dynamics.

The cable is modelled as a lumped-mass chain with tension-only spring-damper segments. This allows slack/taut transitions to emerge from the simulation rather than being imposed as a fixed external force.

## Control Approach

The controller uses cascaded Incremental Nonlinear Dynamic Inversion:

- the outer loop generates acceleration-level guidance commands from position and velocity errors;
- the inner loop stabilizes attitude and angular-rate dynamics;
- WLS allocation distributes the requested increments across rotor thrust and rotor tilt commands while respecting limits.

INDI is well matched to this problem because it relies on measured or estimated acceleration response and local control effectiveness, instead of requiring perfect cancellation of the complete nonlinear model.

## Evaluation Path

The report follows the same story as the defense deck:

1. Define the hybrid UAV and tether model.
2. Explain the INDI principle and cascaded implementation.
3. Validate the controller first without the tether.
4. Add the ground-anchored tether and evaluate nominal tracking.
5. Perturb cable and vehicle parameters by +/- 15% to study robustness.
6. Compare actuator demand with and without the tether.
7. Add a quadrotor PID-vs-INDI benchmark as a hardware-oriented implementation check.

## Main Outcome

The nominal tethered vehicle tracks the circular orbit accurately, showing that INDI can reject the periodic cable disturbance in the tested case. However, the sensitivity analysis shows that the stable regime is narrow: cable stiffness, cable length, cable linear density and vehicle mass can push the system from bounded tracking into instability.

The project therefore establishes a useful modelling and control basis, but also identifies the next technical challenge: robustness to tether uncertainty before extending the framework to multi-UAV suspended-payload transport.
