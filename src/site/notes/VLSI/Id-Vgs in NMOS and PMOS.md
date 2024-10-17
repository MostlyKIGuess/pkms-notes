---
{"dg-publish":true,"permalink":"/vlsi/id-vgs-in-nmos-and-pmos/"}
---


## NMOS

```spice
.include TSMC_180nm.txt
.param SUPPLY=1.8
.param VGG=1.5
.param LAMBDA=0.09u
.param width_N={20*LAMBDA}
.global gnd vdd

VGS 	G 	gnd  0V
**SIN(VO VA FREQ TD THETA PHASE) (page-87 NGSPICE manual V32 2020)
VDS D   gnd 1.80V 

M1      D       G       gnd     gnd  CMOSN   W={width_N}   L={2*LAMBDA} AS={5*width_N*LAMBDA} PS={10*LAMBDA+2*width_N} AD={5*width_N*LAMBDA} PD={10*LAMBDA+2*width_N}

.dc VGS 0 1.8 0.01

.control
set hcopypscolor = 1 *White background for saving plots
set color0=white ** color0 is used to set the background of the plot (manual sec:17.7)
set color1=black ** color1 is used to set the grid color of the plot (manual sec:17.7)

run

let Vds = V(D)
let Ids = -i(VDS)
let Vgs = V(G)
let y = deriv((sqrt(Ids)))/ deriv(Vgs)
plot sqrt(Ids) vs Vgs
plot y 
* hardcopy id_vs_vgs_vds_50.eps  

.endc
```



## PMOS

PS: idk if this works

```spice
.include TSMC_180nm.txt
.param SUPPLY=1.8
.param VGG=1.5
.param LAMBDA=0.09u
.param width_N={20*LAMBDA}
.global gnd vdd

VGS 	G 	gnd  0V
**SIN(VO VA FREQ TD THETA PHASE) (page-87 NGSPICE manual V32 2020)
VDS D   gnd -0.05V 
VBS B  gnd -0.9V

M1      D       G       gnd     VBS  CMOSP   W={width_N}   L={2*LAMBDA} AS={5*width_N*LAMBDA} PS={10*LAMBDA+2*width_N} AD={5*width_N*LAMBDA} PD={10*LAMBDA+2*width_N}

.dc VGS 0 -1.8 -0.01

.control
set hcopypscolor = 1 *White background for saving plots
set color0=white ** color0 is used to set the background of the plot (manual sec:17.7)
set color1=black ** color1 is used to set the grid color of the plot (manual sec:17.7)

run

let Vds = -V(D)
let Ids = i(VDS)
let Vgs = -V(G)
let DIds = deriv(((Ids)))/ deriv(Vgs)
plot (Ids) vs Vgs 
plot DIds vs Vgs
* hardcopy id_vs_vgs_vds_50.eps  

.endc
```