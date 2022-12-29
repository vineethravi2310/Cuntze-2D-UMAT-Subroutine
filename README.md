# Cuntze 2D UMAT Subroutine
<p align="center">
<img src="Images/Cuntze_Failures.png" width = 500 alt>
</p>
<p align="center">
<em>Failure Modes for Transversely-Isotropic Materials</em>
</p>

**Cuntze2D.for** is a UMAT subroutine for Cuntze failure criteria in Abaqus. It applies only to unidirectional composites. The general form of the Cuntze criterion utilizes the entire 3D state of stress and strain, but this UMAT considers only in-plane stresses and strains consistent with Classical Laminate Theory.

### Fiber Failure in Tension (FF1)
This mode is valid when $\sigma_1 \geq 0$
$$Eff^{\parallel\sigma} = \frac{\epsilon_{1}^tE_{\parallel}}{R^t_\parallel} $$
$\epsilon_{1}^t$ is the tensile strain of the  UD lamina in the 1 direction.

### Fiber Failure in Compression (FF2)
This mode is valid when $\sigma_1 < 0$
$$Eff^{\parallel\tau} = -\frac{\epsilon_1^cE_{\parallel}}{R^c_\parallel}$$
$\epsilon_{1}^c$ is the compressive strain of the UD lamina in the 1 direction.

### Inter-Fiber Failure (IFF1)
This mode is valid when $\sigma_2 \geq 0$
$$Eff^{\perp\sigma} = \frac{\sigma_{2}}{R^t_\perp} $$

### Inter-Fiber Failure (IFF2)
This mode is valid when $\sigma_2 < 0$
$$Eff^{\perp\tau} = -\frac{\sigma_{2}}{R^c_\perp}$$


### Inter-Fiber Failure (IFF3)
$$Eff^{\perp\parallel} = \frac{|\tau_{21}|}{R_{\perp\parallel} - \mu_{\perp\parallel}\sigma_{2}}$$

### Resultant Failure
The resultant failure of all the modes is given by

$$Eff = \Bigl[ (Eff^{\parallel\sigma})^m + (Eff^{\parallel\tau} )^m + (Eff^{\perp\sigma})^m + (Eff^{\perp\tau})^m + (Eff^{\perp\parallel})^m \Bigr]^\frac{1}{m}$$

where $m$ is the mode interaction exponent.
# Input to the Model
The following properties need to be entered in the following order. The superscripts $t$ and $c$ are for tension and compression. The subscripts $\parallel$ and $\perp$ are for the transverse, parallel to the fiber direction of a UD lamina.
  * Longitudinal Modulus $E_{\parallel}$
  * Transverse Modulus $E_{\perp}$
  * Major Poisson's Ratio in 1-2 Plane $\nu_{\perp\parallel}$ (German 2014 VDI Guideline)
  * Inplane Shear Modulus $G_{\parallel\perp}$
  * Longitudinal Tensile Strength $R^t_\parallel$
  * Longitudinal Compressive Strength $R^c_\parallel$
  * Transverse Tensile Strength $R^t_\perp$
  * Transverse Compressive Strength $R^c_\perp$
  * Inplane Shear Strength $R_{\perp\parallel}$
  * Friction Coefficient of the Material $\mu_{\perp\parallel}$
  * Mode Interaction Exponent $m$ 

# Output Visualization
 
There are six solution-dependent state variables. They are as follows
  * SDV1 : Fiber Failure in Tension (FF1)
  * SDV2 : Fiber Failure in Compression (FF2)
  * SDV3 : Inter Fiber Failure (IFF1)
  * SDV4 : Inter Fiber Failure (IFF2)
  * SDV5 : Inter Fiber Failure (IFF3)
  * SDV6 : Resultant Failure of All Modes
  
# Verification

<p align="center">
<img src="Images/BC.png" width = 1000 alt>
</p>
<p align="center">
<em>FE Model and Boundary Conditions</em>
</p>

A simple compressive test is performed to check the working of the UMAT. The specimen dimensions are 100 mm x 10 mm. The ply layup is [0°/+45°/-45°/90°/90°/-45°/+45°/0°], and each ply is 0.125 mm thick. The left end of the specimen is fixed in X, Z, ROTX, ROTY, and ROTZ, and the middle of the left end is fixed in Y. A displacement of -1.0 mm is applied on the right end. The material properties are
* $E_{\parallel}$ = 135e3 MPa
* $E_{\perp}$ = 10e3 MPa
* $\nu_{\perp\parallel}$ = 0.25
* $G_{\perp\parallel}$ = 4.3e3 MPa
* $R^t_\parallel$ = 2410 MPa
* $R^c_\parallel$ = 1300 MPa
* $R^t_\perp$ = 86 MPa
* $R^c_\perp$ = 200 MPa
* $R_{\perp\parallel}$ = 152 MPa 
* $\mu_{\perp\parallel}$ = 0.15
* $m$ = 3.1 

Below are the resultant failure indices (SDV 6) for each ply.

<p align="center">
<img src="Images/Ply1.png" width = 5000 >
</p>
<p align="center">
<em>Ply 1</em>
</p>

For ply 1, the resultant failure index (SDV 6) is 1.039. This value is more than one, which means the failure has begun.

<p align="center">
<img src="Images/Ply2.png" width = 5000 >
</p>
<p align="center">
<em>Ply 2</em>
</p>

<p align="center">
<img src="Images/Ply3.png" width = 5000 >
</p>
<p align="center">
<em>Ply 3</em>
</p>

For ply 2 and 3, the resultant failure index (SDV 6) is 0.4605.

<p align="center">
<img src="Images/Ply4.png" width = 5000 >
</p>
<p align="center">
<em>Ply 4</em>
</p>

For ply 4, the resultant failure index (SDV 6) is 0.4703. 

# Analytical Calculations




|       | $\sigma_1$  | $\sigma_2$ | $\tau_{21}$ | $\epsilon_1$ | $Eff^{\parallel\sigma}$ | $Eff^{\parallel\tau}$ | $Eff^{\perp\sigma}$ | $Eff^{\perp\tau}$ | $Eff^{\perp\parallel}$ | $Eff$ |
|    :----:   |    :----:   |    :----:   |    :----:   |    :----:   |    :----:   |    :----:   |    :----:   |    :----:   |    :----:   |    :----:   |
| Ply 1 | -1348.429 |  6.286   |    0.0    |  -0.01    |  0.0  |  1.0385  |  0.0731  |  0.0  |  0.0  | 1.0386   |
| Ply 2 |    -474.81  | -43.165  |   56.44   | -0.003437 | 0.0   |  0.3569  | 0.0   |  0.2158  |  0.3561  |  0.4605  |
| Ply 3 |    -474.81  | -43.165  |  -56.44   | -0.003437 | 0.0   |  0.3569  | 0.0   |  0.2158  |  0.3561  |  0.4605  |
| Ply 4 |    398.808  | -92.615  |    0.0    |  0.003126 | 0.1751   |  0.0  | 0.0   | 0.4631   |  0.0  | 0.4703   |


The above table shows the stresses and strains for each ply, and the failure indices are calculated analytically. The Abaqus results match perfectly with the hand calculations.

# Reference
Cuntze, R. The predictive capability of failure mode concept-based strength conditions for laminates composed of unidirectional laminae under static triaxial stress states.
Journal of Composite Materials 46.19-20, 2012, S. 2563–2594.
47
