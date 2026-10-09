# APDL Workflow Notes

## Preprocessing
- Enter `/PREP7`.
- Select `PLANE182`.
- Define Young's modulus and Poisson's ratio.
- Define 16 keypoints and connect them with lines.
- Create the primary area and three additional areas from line selections.

## Mesh generation
The script obtains the length of line 13 and uses parameter `n` to establish line sizing. It calculates division counts and bias settings, applies `LESIZE`, and maps areas 2, 3, and 4. The loop starts at `n=30` and increments `n` by 2 after each solution pass.

## Boundary conditions and loads
- `SYMM` constraints on lines 1 and 2.
- `UY` constraints on lines 3 and 4.
- Pressure `-1000` on lines 5 and 11.

Confirm these line numbers still identify the intended physical boundaries if the geometry is edited.

## Solution and postprocessing
- Enter `/SOLU` and execute `SOLVE`.
- Enter `/POST1`.
- Plot X-direction normal stress using `PLNSOL,S,X`.
- Sort nodal X-direction stress values using `NSORT,S,X`.
- Retrieve the maximum sorted value with `*GET`.
- Calculate the relative change compared with the prior iteration.

## Suggested improvements
- Replace hard-coded values with named parameters.
- Add checks for failed solutions and missing results.
- Review whether an absolute relative-change criterion is appropriate.
- Add a maximum-iteration limit.
- Record mesh size, element/node counts, and stress at each iteration.
- Export a convergence table.
- Verify mapped-mesh suitability and `AMAP` keypoint ordering.
