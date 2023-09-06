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
 2
   problems to solve -
	• Different in Vds due to channel length modulation
	• If Vout changing, Iout should not change
Resolve issues with higher Rout
Rout = ro2
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/8ed40def-d9ea-40ef-852b-9e578815c87a)



 

*****************
### 2. Cascode current mirror
Adding a cascode device will increase Rout = gm2*ro2*ro4 solving the 2 problems, but Vout CM is very high -> Vout,min = Vgs+Vov = 2Vov + Vth
 = 2*0.2 + 0.5 = 0.9 V not ideal to low power design
Also, Vds is not well defined!

From Sansen - 
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/34d2de6c-10f9-4e9b-bb5a-e43c3341b4a7)

![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/7d20c848-6218-4326-aea1-5517a2b80843)


*****************
### 3. Low voltage cascode current mirror
Vout CM reduced -> Vout,min = Vov + Vov = 2Vov
 = 2*0.2 = 0.4 V, which is very low ! 
Rout = gm2*r02*r01. Vds got reduced and therefore ro2 reduced -> Rout reduced but still very high due to cascode structure.
Also, Vds is still not well defined.
We now need extra biasing circuit, to bias Vb. 
Vb range - 
Vb,min = Vgs3 + Vov1 
Vb,max = Vgs1 + Vth 

We have 2 approach - 
Vb bias using a Current mirror with resistor - 
We will need the resistor to create a current, but the resistor value is high which will cost lots of area
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/5a27c853-3d0b-4ec9-a42d-5f53d4251a17)

Vb bias from Iref branch - 
More efficient approach is to take the current from Iref branch and copy it to bias Vb
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/65cc4319-9f38-4c99-8916-ff0f850a67f8)






*****************
### 4. Low voltage regulated cascoded current mirror
With this gain boosting technique, Rout = A*(gm2*ro2*ro4), which is really high !
Also, we now have a well-defined Vds voltage so matching is better. 
Opamp design - 
Since Vin=Vs3=200mV & Vout=Vg4=800mV we need low voltage CM input and high voltage CM output -> 2 stage Op-Amp with PMOS input and CS output
PM > 60 deg
A_OL > 60 dB
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/97495754-1601-4719-ac6f-999d7dc998b2)



Results summary - 
We introduce Iout vs. Vout (Voltage as load) graph to compare between the different current mirrors. 
From the graph we can see the following - 
	• Iout_CM (Red) - has worst Iout due to high channel length modulation
Different between Iout_Cascode and Iout_low_Vx (low voltage) is 366mV ! Which proves the low voltage CM provides stable current starting from 433mV instead of 800mV !
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/36994b0f-4fb0-4c80-a1fe-5cc7be15384e)

We also compare between different Rout vs. frequency - 
	• Rout_low_Vx_reg (Regulated cascode) has the higher Rout
Rout_cascode has high Rout as well, but it is an high power design
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/4c6864ab-607e-4d71-8174-fa14577accef)

Finally, we compare Spec vs. corners - 
Regulated cascode has lowest STD for Iout and highest Rout over corners
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/d4394877-85ca-47ee-bfa0-ea778d7f4745)





