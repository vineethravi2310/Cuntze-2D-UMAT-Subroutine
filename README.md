# Cuntze-2D-Subroutine
<p align="center">
<img src="Images/Cuntze_Failures.png" width = 500 alt>
</p>
<p align="center">
<em>Failure Modes for Transversely-Isotropic Materials</em>
</p>

The following is a UMAT subroutine for Cuntze failure criteria in Abaqus. It applies only to unidirectional composites. The general form of the Cuntze criterion utilizes the entire 3-D state of stress and strain, but this subroutine is used for only 2D elements. The equations for the different failure modes are given below.

### Fiber Failure in Tension (FF1)
The fiber failure in tension is given by 
$$E_{ff}^{\parallel\sigma} = \frac{\epsilon_{11}^tE_{11}}{X_T}$$
It is valid only when $\sigma_{11} \geq 0$. $E_{11}$ is Youngs modulus of the unidirectional lamina in 11 direction and $\epsilon_{11}^t$ is the tensile strain of the lamina in 11 direction.

### Fiber Failure in Compression (FF2)
The fiber failure in tension is given by 
$$E_{ff}^{\parallel\tau} = -\frac{\epsilon_{11}^cE_{11}}{X_C}$$
It is valid only when $\sigma_{11} < 0$. $E_{11}$ is Youngs modulus of the unidirectional lamina in 11 direction and $\epsilon_{11}^c$ is the compressive strain of the lamina in 11 direction.

### Inter Fiber Failure (IFF1)
The inter fiber failure in tension is given by 
$$E_{ff}^{\perp\sigma} = \frac{\sigma_{22}}{Y_T} $$
It is valid only when $\sigma_{22} \geq 0$. $\sigma_{22}$ is the stress of the unidirectional lamina in 22 direction and $Y_T$ is tensile strength in the 22 direction.

### Inter Fiber Failure (IFF2)
The inter fiber failure in compression is given by 
$$E_{ff}^{\perp\tau} = -\frac{\sigma_{22}}{Y_C}$$
It is valid only when $\sigma_{22} < 0$. $\sigma_{22}$ is the stress of the unidirectional lamina in 22 direction and $Y_C$ is compressive strength in the 22 direction.

### Inter Fiber Failure (IFF3)
The inter fiber failure in shear is given by 
$$E_{ff}^{\perp\parallel} = \frac{|\tau_{12}|}{S - \mu_{\perp\parallel}\sigma_{22}}$$
$\sigma_{22}$ is the stress of the unidirectional lamina in 22 direction, $S$ is the inplane shear strength, $\tau_{12}$ is the inplane shear stress and $\mu_{\perp\parallel}$ is friction parameter.

### Resultant Failure
The resultant failure of all the modes is given by

$$E_{ff}(res) = \Bigl[ E_{ff}(FF1)^m + E_{ff}(FF2)^m + E_{ff}(IFF1)^m + E_{ff}(IFF2)^m + E_{ff}(IFF3)^m \Bigr]^\frac{1}{m}$$

where $m$ is the mode interaction exponent.
# Input to the Model
The following properties need to be entered in the following order.
  * Youngs Modulus in 11 Direction $E_{11}$
  * Youngs Modulus in 22 Direction $E_{22}$
  * Poisson's Ratio in 1-2 Plane $\nu_{12}$
  * Inplane Shear Modulus $G_{12}$
  * Longitudinal Strength in 11 Direction $X_T$
  * Compressive Strength in 11 Direction $X_C$
  * Longitudinal Strength in 22 Direction $Y_T$
  * Compressive Strength in 22 Direction $Y_C$
  * Inplane Shear Strength $S$
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

A simple tensile test is performed to check the working of the UMAT. The specimen dimensions are 100mm x 10mm. The ply layup is $[0/+45/-45/90]_s$, and each ply is 0.125mm thick. The left end of the specimen is fixed in X, Z, ROTX, ROTY, and ROTZ, and the middle of the left end is fixed in Y. A displacement of 0.15mm is applied on the right end. The material properties are
* $E_{11}$ = 135e3 MPa
* $E_{22}$ = 10e3 MPa
* $\nu_{12}$ = 0.25
* $G_{12}$ = 4.3e3 MPa
* $X_T$ = 2410 MPa
* $X_C$ = 1300 MPa
* $Y_T$ = 86 MPa
* $Y_C$ = 200 MPa
* $S$ = 152 MPa  

<p align="center">
<img src="Images/Cuntze.png" width = 1000 >
</p>
<p align="center">
<em>eLamX Results</em>
</p>

The same model has been created in eLamX; a composite calculator developed at TU Dresden. Above are the results of the eLamX calculator. From the above image, It can be seen that the failure occurs in the 90° ply. Now we shall be comparing these with the Abaqus UMAT results.

