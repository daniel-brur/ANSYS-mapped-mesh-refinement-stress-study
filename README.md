# ANSYS APDL Optimization Mapped-Mesh Stress Study

An ANSYS Mechanical APDL script that builds a 2D structural model, defines a linear-elastic material, creates mapped finite-element meshes, applies boundary conditions and pressure loads, solves the model, and evaluates maximum X-direction normal stress while refining the mesh.

## Overview

The script demonstrates a parametric APDL workflow:

1. Defines `PLANE182` as the element type.
2. Defines isotropic linear-elastic material properties.
3. Creates a structure using 16 keypoints and element lines.
4. Builds four areas from line selections.
5. Refines line sizing in a loop.
6. Generates mapped meshes for selected areas.
7. Applies symmetry/displacement constraints.
8. Applies pressure loads to selected lines.
9. Starts iterative process by solving the structural model.
10. Plots and retrieves maximum X-direction stress.
11. Compares the current maximum stress with the previous iteration.

## Repository contents

```text
.
├── README.md
├── LICENSE
├── .gitignore
├── GITHUB_DESCRIPTION.txt
├── GITHUB_TOPICS.txt
├── src/
│   └── mapped_mesh_stress_study.inp
├── examples/
│   └── model_geometry_notes.md
└── docs/
    └── APDL_WORKFLOW.md
```

## Requirements

- ANSYS Mechanical APDL supporting the commands and element type used.
- A license that permits the intended analysis.
- Basic familiarity with APDL and finite-element modeling.

## How to run

1. Download or clone this repository.
2. Open ANSYS Mechanical APDL.
3. Set a working directory where result files can be written.
4. Load `src/mapped_mesh_stress_study.inp` through the APDL input-file workflow.
5. Review units, material properties, loads, boundary conditions, and mesh settings.
6. Run the input file and inspect the solver output and convergence behavior.

## Model parameters

| Parameter | Value | Meaning |
|---|---:|---|
| Element type | `PLANE182` | 2D structural solid element |
| Young's modulus | `2.9e7` | Must use a consistent unit system |
| Poisson's ratio | `0.3` | Isotropic linear-elastic material |
| Initial mesh parameter, `n` | `30` | Starting value for line divisions |
| Mesh increment | `+2` | Increase between iterations |
| Pressure | `-1000` | Applied to lines 5 and 11; units depend on the model |
| Convergence threshold | `0.0005` | Relative stress-change criterion |

**Important:** APDL does not automatically enforce a unit system. Geometry, elastic modulus, and pressure must use consistent units.

## Convergence logic

The script retrieves the maximum `S,X` value and compares it with the previous iteration:

```text
porcentaje = (current_max_stress - previous_max_stress) / current_max_stress
```

When `porcentaje` is below the specified threshold, the loop sets `i=0`, then increments `n` by 2 after each pass.

Review this criterion before treating the result as proof of mesh convergence. Consider whether an absolute value is appropriate, whether the denominator can be zero, and whether local stress singularities affect the result.

## Validation notes

This repository packages and documents the supplied APDL code; it does not certify the model or its results.

Before using the script for engineering decisions, validate:

- Geometry and area topology.
- Whether `AMAP` and the keypoint order suit each area.
- Whether all intended areas are meshed.
- Pressure direction and sign.
- Boundary-condition suitability.
- The model unit system.
- Mesh-refinement logic and stopping condition.
- Stress component and location used for convergence.
- Solver output and warnings for the installed ANSYS version.

The source initializes `i=1` and sets `i=0` when the convergence condition is met. Verify that the loop exits as expected in your ANSYS version and inspect the output for syntax or modeling warnings.

## License

Released under the MIT License. See `LICENSE`.

## Disclaimer

This repository is an APDL scripting and finite-element workflow example. It is provided "as is" without warranty and is not a substitute for engineering judgment, verification, validation, or applicable design codes.
