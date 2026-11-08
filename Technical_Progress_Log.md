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
- [ ] Physical meaning of k and epsilon
- [ ] Understanding simulation output directories

## 5. Problems and solutions

### ParaView performance in WSLg

Problem:
Very slow rendering when using ParaView through WSLg.

Decision:
Use the native Windows installation of ParaView.

## 6. Pending tasks

- [ ] Understand k-epsilon turbulence model
- [ ] Identify simulation time directories
- [ ] Review the results in ParaView for Windows
- [ ] Consolidate the pitzDailySteady fundamentals

## 7. Next milestone

Understand the existing simulation results and complete
the fundamental OpenFOAM exercises before starting
the laminar cylinder CFD project.