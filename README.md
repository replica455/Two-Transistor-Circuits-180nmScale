# Design and Analysis of Various 2 MOSFET circuits in 180nm technology scale

Aim of project to discuss various 2 transistor circuit applications like basic Inverter, Current mirror, amplifier, 2:1 MUX and many more.
- The tools used - ```Electric Vlsi Tool``` ``` LTSpice```
- Technology file - ```BSIM3 models for AMI Semiconductor's C5 process```

# Introduction

Let us first discuss a simple mos technology and Try to establish the characterestic graph from schematic and then from layout 

- ![NMOS_1](https://user-images.githubusercontent.com/55652905/178313979-1f7bfc69-3794-4922-a449-4e75e895c508.JPG)

We have attempted to make a simple 4 port NMOS circuit in Electric VLSI CAD Tool. In this particular cell we have to select a 4  terminal NMOS from the component list and. export the names of terminal as gate(g), drain(d), source(s) and obviously to avoid the body effect and lowering of threshold we make the body terminal to ground(gnd). Most importantly we have made the parameters as follows
- ```WIDTH = 10```
- ```Length = 2```

Now as we are using 180nm Technology so it is quite expected the width is 10 * 180 = 1.8um and length is 2 * 180 = 0.36um 
