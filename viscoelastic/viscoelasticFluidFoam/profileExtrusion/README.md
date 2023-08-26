# Viscoelastic Profile Extrusion

## Authors

Author: Bruno Martins and Ricardo Costa (UMinho)

Reviser: Miguel Nóbrega (UMinho)

## Copyright

Copyright (c) 2022-2023 University of Minho

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons Licence" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

## OpenFOAM branch/version

foam-extend 4.1

## Description

The computational time required to perform a numerical simulation of profile extrusion forming, considering more realistic (viscoelastic) constitutive models, is incompatible with the industrial requirements. This microbenchmark case study mimics a typical profile extrusion problem and aims at assessing the solvers available in OpenFOAM, namely viscoelasticFluidFoam integrated into foam-extend 4.1. Due to the simplified geometry employed, the computational meshes required to get accurate results will be much smaller than the ones of the associated industrial scale case study. This should allow performing initial exploratory studies much faster. Thus, the results obtained are expected to give insights into improving the viscoelasticFluidFoam solver.    

## Custom GiesekusIBSD viscoelastic constitutive model

This benchmark requires a custom viscoelastic constitutive model, named GiesekusIBSD, where a stabilization procedure (improved both sides diffusion) is implemented, which should be compiled before running the case study, as follows:

1. Source a foam-extend 4.1 installation.
2. Change the directory to "src".
2. Execute "./Allwmake".

## Geometry

The geometry proposed in this study resembles a typical profile extrusion die. It has a circular inlet with a radius of 12.5mm, which connects to the extruder, and a rectangular outlet that allows manufacturing a profile with a rectangular cross-section of 15x2 mm. The middle of the channel comprises a convergent zone, which performs the transition between the circular inlet and the rectangular outlet. As illustrated in Figure 1, due to symmetry, just a quarter of the geometry was considered for the numerical studies.

<img src="./figures/profileExtrusion_setup.png" alt="footer" height="300">

Figure 1. Microbenchmark geometry.

## Material Properties

The material considered is representative of a thermoplastic polymer modelled with a viscoelastic multimode Giesekus constitutive model [1,2], which comprises six modes.

## Initial and Boundary Conditions

The boundary and initial conditions employed in the case study are presented in Table 1. Due to the symmetry of this simplified geometry, appropriate symmetry planes are defined.

Table 1. Microbenchmark initial and boundary conditions.

| | | | Variables | |
|--|--|--|--|--|
| | |Velocity - U [m/s] |Pressure - p [Pa]|Stress - Tau [Pa]|
| Initial Conditions | Type | uniform | uniform | uniform |
|  | Value | (0 0 0) | 0 | (0 0 0 0 0 0) |
| inlet | Type  | fixedValue | zeroGradient  | fixedValue |
|  | Value | uniform (0 0 2.03e-03) | - | uniform (0 0 0 0 0 0) |
| outlet | Type  | zeroGradient  | fixedValue | zeroGradient  |
|  | Value | - | uniform (0 0 0 ) | - |
| wall | Type  | fixedValue | zeroGradient | zeroGradient  |
|  | Value | uniform (0 0 0) | - | - |
| symmetryXZ | Type  | symmetryPlane | symmetryPlane | symmetryPlane |
|  | Value | - | - | - |
| symmetryYZ | Type  | symmetryPlane | symmetryPlane | symmetryPlane |
|  | Value | - | - | - |

## Computational Mesh

The geometry is discretized with non-orthogonal unstructured meshes generated with the cfMesh utility [3]. In this benchmark case study, two meshes are considered comprising 1 million and 20 million cells, having two different refinement regions to accommodate better the larger gradients expected at the narrower section of the extrusion die. The coarsest mesh is shown in Figure 2.

<img src="./figures/profileExtrusion_mesh.png" alt="footer" height="300">

Figure 2. Microbenchmark coarsest mesh.

## Files

The files for this case study are available in the exaFOAM WP2 repository
https://develop.openfoam.com/exafoam/wp2-validation/-/tree/master/viscoelastic/viscoelasticFluidFoam/profileExtrusion

## Case folders

For a fixed relative/absolute tolerance for residual convergence criteria, the following case folders are included:
* 1M_fixedTol - Mesh with 1 million cells.
* 20M_fixedTol - Mesh with 20 million cells.

For a fixed number of inner iterations obtained as a representative average of the simulation, the following case folders are included:
* 1M_fixedIter - Mesh with 1 million cells.
* 20M_fixedIter - Mesh with 20 million cells.

## Running the simulations

Check the README.md file inside each case folder.

## Results

The numerical distribution of pressure, velocity and stress are shown in Figure 3. The accuracy of the calculations can be assessed by the distribution of pressure, velocity and stress components along the 3D domain, the velocity and stress distributions at the outlet cross-section and the average pressure at the inlet. 

<img src="./figures/profileExtrusion_p.png" alt="footer" height="300">
<img src="./figures/profileExtrusion_UMagnitude.png" alt="footer" height="300">
<img src="./figures/profileExtrusion_tauMagnitude.png" alt="footer" height="300">

Figure 3. Microbenchmark numerical solution.

## Acknowledgment

This application has been developed as part of the exaFOAM Project https://www.exafoam.eu, which has received funding from the European High-Performance Computing Joint Undertaking (JU) under grant agreement No 956416. The JU receives support from the European Union's Horizon 2020 research and innovation programme and France, Germany, Italy, Croatia, Spain, Greece, and Portugal.

<img src="./figures/footerLogos.jpg" alt="footer" height="100">

## Dissemination Plan

The case can be made available with public access.

## References

[1] J. Azaiez, R. Guénette, A. Ait-Kadi, Entry ﬂow calculations using multi-mode models, Journal of Non-Newtonian Fluid Mechanics, 66(2-3), 271–281, 1996.

[2] H. Giesekus, A simple constitutive equation for polymer fluids based on the concept of deformation dependent tensorial mobility, Journal of Non-Newtonian Fluid Mechanics 11(1-2), 69-109, 1982.

[3] F. Juretić, cfMesh User Guide (v1.1), Zagreb, Croatia, 2015.