# Design and Analysis of Various 2 MOSFET circuits in 180nm technology scale

Aim of project to discuss various 2 transistor circuit applications like basic Inverter, Current mirror, amplifier, 2:1 MUX and many more.
- The tools used - ```Electric Vlsi Tool``` ``` LTSpice```
- Technology file - ```BSIM3 models for AMI Semiconductor's C5 process```

# Introduction

Let us first discuss a simple mos technology and Try to establish the characterestic graph from schematic and then from layout 

 ![NMOS_1](https://user-images.githubusercontent.com/55652905/178313979-1f7bfc69-3794-4922-a449-4e75e895c508.JPG)

We have attempted to make a simple 4 port NMOS circuit in Electric VLSI CAD Tool. In this particular cell we have to select a 4  terminal NMOS from the component list and. export the names of terminal as gate(g), drain(d), source(s) and obviously to avoid the body effect and lowering of threshold we make the body terminal to ground(gnd). Most importantly we have made the parameters as follows
- ```WIDTH = 10```
- ```Length = 2```

Now as we are using 180nm Technology so it is quite expected the width is 10 * 180 = 1.8um and length is 2 * 180 = 0.36um 
Set the Spice model by refering to the technology parameter, in my case its "NMOS"
After the schematic its time to have Design Rule Check (DRC), we can have it in Tools tab and then under it the is DRC option then check hierarchically.
In my case it made successfully with 0 error and 0 warning.

To check the simulation we need LTSpice and have to write the corrosponding spice code 
- The spice code as follows 
```
vg g 0 dc 0
vs s 0 dc 0
vd d 0 dc 0
.dc vd 0 1.8 0.1m vg 0 1.8 0.3
.include C:\Users\bikas\OneDrive\Desktop\CAD TOOLS\C5_model.txt
```
the last line defines the path to my technology file which is "C5_model.txt". Here we have we have sweept the drain voltage from 0 to 1.8V with an interval of 0.1mV and teaced the drain current (Id) woth respect to constant gate voltage.  
So after invoking the LTSpice we can have our desired result

![NMOS_2](https://user-images.githubusercontent.com/55652905/178317202-089d121c-ce6f-4dab-8883-e4d848dc1a42.JPG)

Such grah is our age old [output characterestic graph]
Now on similar fassion we can sweep the gate voltage only keeping the drain voltage to constant 1.8V and trace out the drain current,
in this case our spice code can be modified like this-
```
vg g 0 dc 0
vs s 0 dc 0
vd d 0 dc 1.8
.dc vg 0 1.8 0.01
.include C:\Users\bikas\OneDrive\Desktop\CAD TOOLS\C5_model.txt
.END

```
and the corrosponding curve obtained is like-

![NMOS_3](https://user-images.githubusercontent.com/55652905/178319941-18c75d73-6c47-469d-9708-1c07d93fb29f.JPG)

Now such a curve is our Drain current v/s Gate voltage 

After achieveing the characterestic curve my aim is to draw the schematic of the circuit and then  to have an Layout V/S Schematic 
To have the layout of the N channel MOSFET device we need to use 
- NMOS the device
- TWO N ACTIVE which forms the drain and source connection
- P WELL which forms the body to make the ground connection 
- POLYSILICON CONTACT for gate terminal

The construction looks like this 

![NMOS_4](https://user-images.githubusercontent.com/55652905/178329986-ab3b1078-bdbc-47b2-a41c-583e0b6a1c5b.JPG) ![NMOS_5](https://user-images.githubusercontent.com/55652905/178330022-0647ab2f-75fe-4b49-a649-400383b6a6de.JPG)

obviously we can make the scice model and spice code to simulate the same graph just like earlier. In the 3D view we can see the respective diffusion layers clearly.
After this it's time to have some checking done
- ERC - Electrical Rule Check allows well check i.e. the tool checks if the connection of well is made properly or not
- NCC - Network Consistency Check checks topology, width, size is according to the schematic This operation is sometimes called Layout vs. Schematic (LVS)

Both of the options available in the TOOLS tab. For successful design you will get "No well error found" and "Summary for all cells: exports match, topologies match, sizes match". 
In such manner we can also conduct the test of P channel MOSFET device. This conclude our introductory part of the project and we can enter to the actual heading of the topic. For different circuit we will also simulate various parameter matrix like various propagation delay, power dessipation and many more so lets just dive right in to our first circuit 

# Circuit 1 - The CMOS INVERTER 

So this is the digital inverter. the input voltage Vin switches one of two transistor to ON state while other remains OFF.
- The Schematic 

![INVERTER_1](https://user-images.githubusercontent.com/55652905/178407851-051116ea-dccb-48e9-bdbf-eeefe50e3c31.JPG)

along with the schematic cell of the inverter we have also created the icon view so as to reffer in other circuit with abstraction to internal circuitry and for the sake of convenience we have placed our spice code on the icon view. Asusual do perform the DRC check after forming the schematic.

- Layout

To have the layout of the of CMOS Inverter device we need to use 
1. NMOS the device
2. TWO N ACTIVE which forms the drain and source connection of NMOS
3. P WELL which forms the body to make the ground connection of NMOS
4. POLYSILICON CONTACT for gate terminal to connect Vin with gate terminal of both NMOS and PMOS
5. TWO P ACTIVE which forms the drain and source connection of PMOS
6. N WELL which forms the body to make VDD connection of PMOS

Finally export the pins and place spice model of two MOSFET after reffering the technology file.
the outcome looks like 

![INVERTER_2](https://user-images.githubusercontent.com/55652905/178411115-4e0736ee-45ab-46fb-8cc6-c1171afcdff3.JPG)![INVERTER_4](https://user-images.githubusercontent.com/55652905/178411151-c9f1d13d-33ca-4e5c-b6bc-2767d3fd7657.JPG)![INVERTER_5](https://user-images.githubusercontent.com/55652905/178411174-852468d9-8539-4dc6-858a-e6b55b7a4361.JPG)

after the construction please do make sure there is noo error while having DRC, ERC, LVS(NCC) checks. If there are error then it will get reflected on error log tab and do take adequate measures to avoid them.

- Spice code and Output

1. The transient analysis spice code

```
vdd vdd 0 dc 1.8
vin in 0 pulse (0 1.8 0 1n 1n 10n 20n)
.tran 1n 100n
.include C:\Users\bikas\OneDrive\Desktop\CAD TOOLS\C5_model.txt
.END
```
2. Transient analysis output

![INVERTER_6](https://user-images.githubusercontent.com/55652905/178411863-4126116b-4abe-4ec5-a091-c11dfdc17dc2.JPG)

3.  DC analysis spice code

```
vdd vdd 0 dc 1.8
vin in 0 pulse (0 1.8 0 1n 1n 10n 20n)
.dc vin 0 1.8 0.1m
.include C:\Users\bikas\OneDrive\Desktop\CAD TOOLS\C5_model.txt
.END
```
4. DC analysis Output

![INVERTER_7](https://user-images.githubusercontent.com/55652905/178412444-a3b548f3-2740-4dae-b3ff-ef42341a3e56.JPG)

- The parametric test

Now as we are applying the VDD=1.8V so ideally we cam assume the switching threshole to be around or near about VDD/2 i.e. 1.8/2 = 0.9V, but from our DC analysis we foend that the curve is more than 0.9V. So to acieve this ideal nature what I am thinking to resize the PMOS device keeping the NMOS device size conatant. To acieve so we need to return to the LTSpice trrminal and have a change in code.

- Modified transient analysis for Parametric Test

```
Mnmos@0 out in gnd gnd NMOS L=0.36U W=1.8U AS=5.346P AD=3.645P PS=14.94U PD=8.1U
Mpmos@0 out in vdd vdd PMOS L=0.36U W={wp} AS=7.452P AD=3.645P PS=18.54U PD=8.1U
.ENDS NOT__INVERTER

*** TOP LEVEL CELL: INVERTER_SIM{lay}
XINVERTER@0 gnd in out vdd NOT__INVERTER

* Spice Code nodes in cell cell 'INVERTER_SIM{lay}'
vdd vdd 0 dc 1.8
vin in 0 pulse (0 1.8 0 1n 1n 10n 20n)
.step param wp 1u 10u 1u
.trans 0 100n
.include C:\Users\bikas\OneDrive\Desktop\CAD TOOLS\C5_model.txt
.END
```
here we are considering the width of the PMOS as a parameter "wp" and later we are increasing the width from 1um to 10um with a step of 1um.

- Modified transient analysis output 

![INVERTER_8](https://user-images.githubusercontent.com/55652905/178413983-0b676c2c-1465-4045-8033-a974a0372b00.JPG)

- Modified DC analysis code for Parametric Test

```
Mnmos@0 out in gnd gnd NMOS L=0.36U W=1.8U AS=5.346P AD=3.645P PS=14.94U PD=8.1U
Mpmos@0 out in vdd vdd PMOS L=0.36U W={wp} AS=7.452P AD=3.645P PS=18.54U PD=8.1U
.ENDS NOT__INVERTER

*** TOP LEVEL CELL: INVERTER_SIM{lay}
XINVERTER@0 gnd in out vdd NOT__INVERTER

* Spice Code nodes in cell cell 'INVERTER_SIM{lay}'
vdd vdd 0 dc 1.8
vin in 0 pulse (0 1.8 0 1n 1n 10n 20n)
.step param wp 1u 10u 1u
.dc vin 0 1.8 0.1m
.include C:\Users\bikas\OneDrive\Desktop\CAD TOOLS\C5_model.txt
.END
```
Here we can see there is a graph (second from the left) which passes through 0.9V approx whose corrosponding width is 2um resembles almost like ideal switching threshold. so for Optimized circuit we can choose the corrosponding curve.

- Performance Matrix Test

Now its high time for us to analize the performance parameter which can involve the measurement of 
1. maximum output voltage 
2. minimum output voltage
3. Fall time 
4. Rise time
5. Falling Propagation Delay
6. Rising Propagation Delay
7. Average Propagation Delay
8. Average Output Current
9. Measurement of Charge
10. Measurement of Energy
11. Average output voltage fron 0 to 100nsec

To achieve the resuft we need to have the measurement file with ".meas" extension. From LTSpice navigate to "FILE -> Execute .MEAS Script".

The .MEAS Script -
```
* Some measurement
.param VDD =1.8
.measure Voutmax MAX V(out)
.measure Voutmin MIN V(out)

.measure tfall ; Falling time
+TRIG V(out) VAL = '0.8*1.0' FALL=1
+TRIG V(out) VAl = '0.2*1.0' FALL=1

.measure trise ; Rising time
+TRIG V(out) VAL = '0.2*1.0' RISE=1
+TRIG V(out) VAL = '0.8*1.0' RISE=1

.measure tpdf ; Falling propagation delay
+TRIG V(in) VAL = '0.8*1.0' RISE=1
+TRIG V(out) VAL = '0.2*1.0' FALL=1

.measure tpdr ; Rising propagation delay
+TRIG V(in) VAL = '0.2*1.0' FALL=1
+TRIG V(out) VAL = '0.8*1.0' RISE=1

.measure tpd PRAM '{tpdf+tpdr}/2' ; Avg. propagation delay

.measure TRAN I(VDD) AVG
.measure CHARGE INTEGRAL I(VDD) FROM= 0.1NS TO=20NS
.measure ENERGY PARAM= 'CHARGE*VDD'
```

after executing the script file in LTSpice the simulated result as follows

```
* C:\Users\bikas\OneDrive\Documents\Two_transistor_circuit_project\INVERTER\NOT.mout
Voutmax: MAX(V(out))=1.82116 FROM 0 TO 1e-007
Voutmin: MIN(V(out))=-0.0138693 FROM 0 TO 1e-007
tfall=9.91458e-008 FROM 8.54249e-010 TO 1e-007
trise=8.83469e-008 FROM 1.16531e-008 TO 1e-007
tpdf=9.91458e-008 FROM 8.54249e-010 TO 1e-007
tpdr=8.83469e-008 FROM 1.16531e-008 TO 1e-007
tpd=1e-007 FROM 0 TO 1e-007
I(VDD)=1e-007 FROM 0 TO 1e-007
CHARGE: INTEG(I(VDD))=-1.48826e-014 FROM 1e-010 TO 2e-008
ENERGY: (CHARGE*VDD)=-2.67886e-014
```
Now to get the average voltage output from 0 to 100nsec go to the transient analysis waveform. Press ctrl+Lclick on the V(out) tab of waveform, here we can find the result as

```
Interval Start : 0s
Interval End : 100ns
Average : 815.02mV
RMS : 1.2031V
```

Thus at this point we have successfully conducted the basic CMOS INVERTER citcuit.

# Circuit 2 -  

❗ Updating Soon ❗
