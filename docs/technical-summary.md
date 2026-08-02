# Technical Summary

## System

The studied platform is a single-wing quad-tiltrotor UAV. It has four rotor thrust commands and two independent rotor-tilt commands, so the vehicle is over-actuated and its control effectiveness depends on airspeed, attitude, thrust level and tilt geometry.

The simulation model combines:

- six-degree-of-freedom rigid-body dynamics;
- rotor thrust and tilt geometry;
- fixed-wing aerodynamic lift, drag and moments;
- first-order actuator dynamics with limits and saturations;
- a ground-anchored segmented cable represented as tension-only spring-damper links.

## Control Architecture

The controller is a cascaded INDI architecture:

1. The outer loop converts position and velocity tracking errors into virtual acceleration commands.
2. The local acceleration-effectiveness model maps acceleration increments into attitude and body-thrust increments.
3. The inner loop tracks attitude and angular-rate response using incremental control.
4. A constrained WLS allocation distributes the virtual demand across the four rotor thrusts and two tilt angles.

![Cascaded INDI/WLS architecture](assets/figures/control-architecture.png)

INDI is useful here because it corrects commands from the measured or estimated acceleration response instead of requiring exact inversion of the full nonlinear model. This is important for a hybrid tilt-rotor whose dynamics change across the envelope and for tethered flight, where the cable behaves as a structured external disturbance.

## Key Equations

In the report notation, the outer and inner loops use local incremental
effectiveness models:

```math
\Delta\ddot{\boldsymbol{\xi}} \approx G_o\,\Delta u_o,
\qquad
\Delta\nu_i \approx B_i\,\Delta\delta .
```

The constrained weighted least-squares allocator solves the active virtual
control demand while respecting command limits:

```math
\begin{aligned}
\Delta u^\star
&=
\arg\min_{\Delta u}\;
\gamma^2 \left\|W_\nu\left(G\,\Delta u-\Delta\nu\right)\right\|_2^2
+
\left\|W_u\left(\Delta u-\Delta u_p\right)\right\|_2^2 \\
&\text{subject to}\quad
\Delta u_{\min} \le \Delta u \le \Delta u_{\max}.
\end{aligned}
```

The ground-anchored cable is modelled as a unilateral axial spring-damper.
Main symbols: $T_i$ is segment tension, $\ell_i$ current length, $\ell_0$ rest
length, $\dot{\ell}_i$ axial extension rate, $e_i$ axial unit vector, $v_i$ node
velocity, and $k,d$ segment stiffness and damping.

```math
\dot{\ell}_i = (v_{i+1}-v_i)\cdot e_i
```

The tension law is:

```math
T_i =
\begin{cases}
\max\!\left(k(\ell_i-\ell_0)+d\dot{\ell}_i,\,0\right), & \ell_i>\ell_0,\\
0, & \ell_i \le \ell_0.
\end{cases}
```

## Evaluation

The report evaluates the controller through:

- a cable-free baseline and free-flight envelope study;
- a nominal ground-anchored tethered orbit;
- one-at-a-time ±15% cable and vehicle parameter sensitivity;
- actuator effort analysis comparing free and tethered cruise;
- an auxiliary real-flight quadrotor PID-vs-INDI benchmark.

The main technical conclusion is that cascaded INDI with constrained allocation is a promising basis for the over-actuated tilt-rotor, but tether parameter uncertainty and slack/taut transitions define a thin robustness margin that must be addressed before moving toward cooperative payload transport.
