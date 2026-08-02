# Technical Summary

## System

The studied platform is a single-wing quad-tiltrotor UAV. It has four rotor
thrust commands and two independent rotor-tilt commands, so the vehicle is
over-actuated and its control effectiveness depends on airspeed, attitude,
thrust level and tilt geometry.

The simulation model combines:

- six-degree-of-freedom rigid-body dynamics;
- rotor thrust and tilt geometry;
- fixed-wing aerodynamic lift, drag and moments;
- first-order actuator dynamics with limits and saturations;
- a ground-anchored segmented cable represented as tension-only spring-damper
  links.

The cable is modelled as a lumped-mass chain, so slack/taut transitions emerge
from the simulation instead of being imposed as a fixed external force.

## Control Architecture

The controller is a cascaded INDI architecture:

1. The outer loop converts position and velocity tracking errors into virtual
   acceleration commands.
2. The local acceleration-effectiveness model maps acceleration increments into
   attitude and body-thrust increments.
3. The inner loop tracks attitude and angular-rate response using incremental
   control.
4. A constrained WLS allocation distributes the virtual demand across the four
   rotor thrusts and two tilt angles.

![Cascaded INDI/WLS architecture](assets/figures/control-architecture.png)

The architecture is treated as a local incremental-effectiveness problem:
outer- and inner-loop virtual demands are converted into actuator increments and
then allocated under constraints.

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

For the validation campaign, quantitative results and figures, see
[Results](results-summary.md).

The main technical conclusion is the robustness margin: tether parameter
uncertainty and slack/taut transitions can move the closed loop from bounded
tracking to loss of altitude.
