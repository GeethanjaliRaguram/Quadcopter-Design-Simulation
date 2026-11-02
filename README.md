# Quadcopter Design and Simulation

**Geethanjali Raguram**

Software-only mechatronics project: a 5-inch FPV-style quadcopter designed and simulated using SolidWorks, Python and Wokwi. The real drone is not built yet.

Full summary: [Quadcopter_Portfolio.pdf](Quadcopter_Portfolio.pdf)

![Isometric render](Renders/Drone_Isometric.JPG)

## What is in this repo

| Folder | Contents |
|---|---|
| CAD | SolidWorks parts and the assembly (Drone Assembly.SLDASM) |
| Drawings | Arm drawing (A4) and assembly drawing (A3) with BOM, as PDF |
| Renders | Isometric, top and front renders |
| FEA | Arm static stress and displacement plots |
| PID Simulation | Python (Google Colab) roll-axis PID simulation and graphs |
| Wokwi | Arduino + MPU6050 + ESC placeholder circuit |

## Arm FEA: rib-thickness study

Static study in SolidWorks Simulation. Material: ABS (E 2.41 GPa, Poisson 0.39, density 1070 kg/m3). Fixed lug end face, 10 N per motor at the motor end.

| Metric | 3.0 mm ribs (baseline) | 2.0 mm ribs (trial) |
|---|---|---|
| Mass | 8.074 g | 7.445 g |
| Max von Mises stress | 14.72 MPa | 27.8 MPa |
| Max displacement | 1.828 mm | 2.491 mm |
| Min factor of safety | 2.04 (pass) | 1.08 (fail) |

The 2.0 mm design was rejected because it is below the FOS >= 2 criterion. The 3.0 mm baseline is kept.

## PID simulation (roll axis)

10 deg step with a 0.02 N.m wind gust at 1.5 s. Kp = 0.2.

| Controller | Overshoot | Settling | Gust peak | Final error |
|---|---|---|---|---|
| P only | 81.4 % | not settled | 12.4 deg | 4.35 deg |
| PD (Kd 0.03) | 0.0 % | 0.49 s | 5.7 deg | 5.73 deg |
| PID (Ki 0.8, Kd 0.03) | 33.1 % | 1.30 s | 4.0 deg | 0.03 deg |

PID removes the steady gust error but overshoots. Further tuning (lower Ki) was not done.

## Assumptions and limitations

- Base geometry is reference geometry from GrabCAD: [add link to the original model and author here].
- Component masses (battery, electronics) are estimates, not datasheet values. Estimated total is about 390 g (about 426 g with props and wiring).
- ABS yield strength is assumed as 30 MPa (conservative, 3D-printed).
- FEA is a single static load case. The peak stress is at the fixture edge, so it is partly artificial.
- The PID model is single-axis and simplified. Damping (0.002) and max torque (0.3 N.m) are assumed. The inertia value (1.17e-3 kg.m2) is a placeholder, not taken from the final CAD.
- In the Wokwi circuit, servos are used as ESC placeholders. Real ESCs would be powered from the battery, not the Arduino 5 V pin.
- Some assembly components (top plate, spinner, propeller, distribution board, electrical unit I) had unresolved references in my SolidWorks file and are not included in the CAD folder, the BOM or the drawings. Opening the assembly may show missing-component warnings.
- Hardware not built.

## Tools

SolidWorks Premium 2025 (Part, Assembly, Drawing, Simulation), Python in Google Colab, Wokwi.

(c) 2026 Geethanjali Raguram. All rights reserved.
