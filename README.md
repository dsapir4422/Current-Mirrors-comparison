# Current-Mirrors-comparison
This project will introduce a current mirror at different complexity levels. we will compare between a basic current mirror, cascode, low voltage and a regulated low voltage with gain boosting as opamp !

---------------------------------
For this project we will use CMOS general pdk 90nm (gpdk90) technology by Cadence.

Our goal is to copy $10uA$ -> $5uA$ current with the below spec - 
* $I_{ref} = 10uA$
* $I_{out} = 5uA$
* $V_{AA} = 2V$
* $V_{out}$ minimum equal $1.4V$
* $I_{out}$ accuracy +/-2%
* $R_{out} > 100MOhm$

Spec will hold cross corners - TT, SS, FF and temperatures = [-40, 25, 125]


*****************
### 1. Basic current mirror
 A basic current mirror consist of a diode-connected transistor (M1) connected to a 2nd transistor by its $V_{GS}$ voltage. Described by the following basic equation -
 
 ![basic_cm](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/67c109e3-aeac-4e78-9b63-4bd1ff0e7566)


 This basic circuit has 2 problems to solve:
 - Different in $V_{DS}$ due to channel length modulation
 - If Vout changing, Iout should not change

Simulated circuit DC point - 

<img src="https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/8ed40def-d9ea-40ef-852b-9e578815c87a" alt="Image Alt Text" width="500" height="500" />

In this basic circuit - $R_{out} = r_{o2}$, which is the right NMOS output resistance.

*****************
### 2. Cascode current mirror

From Sansen book  - 

<img src="https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/34d2de6c-10f9-4e9b-bb5a-e43c3341b4a7" alt="Image Alt Text" width="250" height="250" align="center" />

Adding a cascode device will increase output resistance by -  $R{out}$ = $g_{m2}$ * $r_{o2}$ * $r_{o4}$ which solves the 2 problems stated before, but Vout CM is very high -> $V_{out,min} = V_{gs}+V_{ov} = 2V_{ov} + V{th}$
 = 2*0.2 + 0.5 = 0.9 V not ideal to low power design
Also, $V_{DS}$ is not well defined!

Simulated circuit DC point - 

<img src="https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/7d20c848-6218-4326-aea1-5517a2b80843" alt="Image Alt Text" align="center" />


*****************
### 3. Low voltage cascode current mirror
Vout CM reduced -> $V{out,min}$ = $V_{ov}$ + $V_{ov}$ = $2V_{ov}$
 = 2*0.2 = 0.4 V, which is very low ! 
 
$R{out}$ = $g_{m2}$ * $r_{o2}$ * $r_{o4}$. Vds got reduced and therefore $r_{o2}$ reduced -> Rout reduced but still very high due to cascode structure.
Also, Vds is still not well defined.
We now need extra biasing circuit, to bias Vb. 

**Vb range -**

$V{b,min}$ = $V_{GS3}$ + $V_{ov1}$ 

$V{b,max}$ = $V_{GS1}$ + $V_{th}$ 

We have 2 approach - 

Vb bias using a Current mirror with resistor - 
We will need the resistor to create a current, but the resistor value is high which will cost lots of area
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/5a27c853-3d0b-4ec9-a42d-5f53d4251a17)


Vb bias from Iref branch - 
More efficient approach is to take the current from Iref branch and copy it to bias Vb
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/65cc4319-9f38-4c99-8916-ff0f850a67f8)

*****************
### 4. Low voltage regulated cascoded current mirror
We will add an opamp in negative feedback to boost Rout.
With this gain boosting technique, $R_{out}$ = $A*(g_{m2}*r_{o2}*r_{o4})$, which is really high !
Also, we now have a well-defined $V_{DS}$ voltage so matching is better. 

**Opamp design**

Since Vin=Vs3=200mV & Vout=Vg4=800mV we need a low voltage CM input and high voltage CM output -> 2 stage Op-Amp with PMOS input and CS output
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/8fdbb756-a87c-4aa4-bc1e-c487cb932547)
Spec is - 

PM > 60 deg
A_OL > 60 dB

we will add a capacitor Cc between the 2 stages the increase phase margin - 
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/76d21374-2503-47a6-9aff-1f1301fddf48)


Simulated DC point - 
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/97495754-1601-4719-ac6f-999d7dc998b2)



**Results summary**

We introduce Iout vs. Vout (Voltage as load) graph to compare between the different current mirrors. 

From the graph we can see the following:
* 	Iout_CM (Red) - has worst Iout due to high channel length modulation
 
Different between Iout_Cascode and Iout_low_Vx (low voltage) is 366mV ! Which proves the low voltage CM provides stable current starting from 433mV instead of 800mV !
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/36994b0f-4fb0-4c80-a1fe-5cc7be15384e)

We also compare between different Rout vs. frequency:
*	Rout_low_Vx_reg (Regulated cascode) has the higher Rout
 
Rout_cascode has high Rout as well, but it is a high power design
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/4c6864ab-607e-4d71-8174-fa14577accef)

Finally, we compare Spec vs. corners - 

We can see that Regulated cascode has lowest STD for Iout and highest Rout over corners
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/d4394877-85ca-47ee-bfa0-ea778d7f4745)

Iout of regulated cascode cross corners (process and temperatures) - 
![image](https://github.com/dsapir4422/Current-Mirrors-comparison/assets/87266625/cc9560b0-4732-441e-8642-cefbb7e49ef6)





