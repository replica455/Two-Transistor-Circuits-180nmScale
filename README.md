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

