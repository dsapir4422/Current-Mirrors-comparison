# Current-Mirrors-comparison
This project will introduce a current mirror at different complexity levels. we will compare between a basic current mirror, cascode, low voltage and a regulated low voltage with gain boosting as opamp !

---------------------------------
For this project we will use CMOS gpk 90nm technology.

Our goal is to copy $10uA$ current to $5uA$ current with the below spec - 
* $I_{ref} = 10uA$
* $I_{out} = 5uA$
* $V_{AA} = 2V$
* $V_{out}$ minimum equal $1.4V$
* $I_{out}$ accuracy +/-2%
* $R_{out} > 100MOhm$

Spec will hold cross corners - TT, SS, FF and temperatures = [-40, 25, 125]


*****************
### 1. Basic current mirror
 A basic current mirror consist of a diode-connected transistor (M1) connected to a 2nd transistor by its $V_{GS}$ voltage.
 It follows the following basic equation - $I_{out} = I_{ref}*\frac{(\frac{W}{L})_2}{(\frac{W}{L})_1}$ 
 - basic circuit
 - Ids vs Vout simulation
 - Rout vs freq simulation
 


 

*****************
### 2. Cascode current mirror



*****************
### 3. Low voltage cascode current mirror



*****************
### 4. Low voltage regulated cascoded current mirror
