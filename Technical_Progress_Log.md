# Technical Progress Log

## 1. Environment

- OS: Windows 11
- Linux environment: WSL2
- Distribution: Ubuntu 22.04 LTS
- CFD software: OpenFOAM Foundation 14
- Visualization: ParaView (Windows)
- GPU: NVIDIA GeForce RTX 3050 Laptop

## 2. Current project

### pitzDailySteady

Official OpenFOAM tutorial used to learn the structure
and execution of an OpenFOAM case.

- Geometry: backward-facing step
- Solver framework: foamRun
- Physical model: incompressible turbulent flow
- Turbulence model: k-epsilon
- Numerical method: Finite Volume Method (FVM)
- Simulation type: steady-state

## 3. Completed milestones

- [x] Install WSL2 and Ubuntu 22.04
- [x] Install OpenFOAM Foundation 14
- [x] Configure OpenFOAM environment
- [x] Execute blockMesh
- [x] Execute checkMesh
- [x] Run pitzDailySteady
- [x] Open simulation results in ParaView

## 4. Concepts consolidated

- [x] Basic OpenFOAM case structure
- [x] Role of blockMesh
- [x] Role of checkMesh
- [x] Difference between fvSchemes and fvSolution
- [x] Basic boundary conditions
- [x] Physical meaning of k and epsilon
- [x] Understanding simulation output directories

## 5. Problems and solutions

### ParaView performance in WSLg

Problem:
Very slow rendering when using ParaView through WSLg.

Decision:
Use the native Windows installation of ParaView.

## 6. Pending tasks

- [x] Understand k-epsilon turbulence model
- [ ] Identify simulation time directories
- [x] Review the results in ParaView for Windows
- [ ] Consolidate the pitzDailySteady fundamentals

## 7. Next milestone

Understand the existing simulation results and complete
the fundamental OpenFOAM exercises before starting
the laminar cylinder CFD project.

## Project 02A – Laminar Flow Around a Cylinder

### Mesh generation and validation

- Mesh generated using Gmsh 4.13.1 and imported into OpenFOAM Foundation 14 using `gmshToFoam`.
- 2D computational domain represented by a thin extruded mesh with one cell layer in the z-direction.
- `frontAndBack` patch configured as `empty`.
- Final mesh:
  - Cells: 62,634
  - Points: 62,962
  - Faces: 219,383
  - Maximum aspect ratio: 2.49
  - Maximum non-orthogonality: 41.83°
  - Average non-orthogonality: 7.06°
  - Maximum skewness: 0.649
- `checkMesh` completed successfully (`Mesh OK`).

**Mesh limitation:** The current mesh does not contain a structured boundary-layer refinement. Its adequacy for resolving near-wall gradients must be assessed through a mesh-independence study.

### Initial transient simulation

- Solver: `foamRun` with `incompressibleFluid`.
- Flow model: laminar, incompressible, transient.
- Reynolds number: 100.
- Number of cells: 62,634.
- Time step: 0.001 s.
- Simulation end time: 1.0 s.
- Number of time steps: 1,000.
- Serial execution completed successfully.

**Initial numerical observation:** Maximum Courant number reached approximately 2.18. Temporal resolution requires further investigation.

### MPI parallelisation benchmark

Hardware:
- CPU: AMD Ryzen 9 5900HX.
- Physical cores: 8.
- Logical CPUs: 16.
- MPI implementation: Open MPI 4.1.2.

Decomposition:
- Method: `scotch`.
- Number of subdomains: 4.
- Total cells: 62,634.
- Average cells per subdomain: 15,658.5.
- Maximum load imbalance: 0.278%.

Benchmark configuration:
- Simulation interval: 0–0.1 s.
- Time step: 0.001 s.
- Time steps: 100.
- Identical mesh and numerical settings for serial and parallel runs.

| Configuration | Clock time |
|---|---:|
| Serial (1 process) | 164 s |
| MPI (4 processes) | 48 s |

- Speedup: 3.42.
- Parallel efficiency: 85.5%.

**Conclusion:** Four MPI processes provide a substantial performance improvement for the current mesh. Additional benchmarks with 6 and 8 processes are planned to investigate parallel scaling.

### MPI scaling study

Additional benchmarks were performed using 6 and 8 MPI processes.

All configurations used:
- Identical mesh (62,634 cells).
- Identical physical and numerical settings.
- Simulation interval: 0–0.1 s.
- Time step: 0.001 s.
- 100 time steps.
- Scotch domain decomposition.

Performance results based on OpenFOAM `ExecutionTime`:

| MPI processes | Execution time (s) | Speedup | Parallel efficiency |
|---:|---:|---:|---:|
| 1 | 163.041 | 1.00 | 100.0% |
| 4 | 47.324 | 3.45 | 86.1% |
| 6 | 42.257 | 3.86 | 64.3% |
| 8 | 36.047 | 4.52 | 56.5% |

The 8-process configuration achieved the lowest execution time, reducing computational time by approximately 77.9% compared with the serial run.

However, parallel efficiency decreased as the number of MPI processes increased. Four processes provided the highest parallel efficiency among the tested parallel configurations.

**Conclusion:** For the current mesh and hardware, 8 MPI processes are the fastest tested configuration, while 4 processes provide better parallel efficiency. The optimal configuration may change with mesh size and computational workload.

**Benchmark limitation:** Each configuration was measured once. Repeated runs and median execution times would be required for a more rigorous performance assessment.

### Pending numerical validation

- Investigate temporal resolution and Courant number.
- Assess mesh independence.
- Verify vortex shedding and periodic flow behaviour.
- Calculate drag coefficient (Cd), lift coefficient (Cl) and Strouhal number (St).
- Evaluate pressure solver performance and possible preconditioner alternatives.