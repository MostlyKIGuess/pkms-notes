---
{"dg-publish":true,"permalink":"/vlsi/measuring-time-delay-and-stuff-in-ng-spice/"}
---



https://ngspice.sourceforge.io/docs/ngspice-manual.pdf
pg- 318-325

![Pasted image 20241017230948.png](/img/user/VLSI/Pasted%20image%2020241017230948.png)

![Pasted image 20241017231007.png](/img/user/VLSI/Pasted%20image%2020241017231007.png)

![Pasted image 20241017231026.png](/img/user/VLSI/Pasted%20image%2020241017231026.png)





```spice
.measure tran t_vin_cross WHEN V(a)={Vin_threshold} CROSS=1
.measure tran t_vout_cross WHEN V(c)={Vout_threshold} CROSS=1
.measure tran prop_delay PARAM='t_vout_cross - t_vin_cross'

```




```spice

.include TSMC_180nm.txt
.param SUPPLY=1.8
.param VGG=1.5
.param LAMBDA=0.18u
.param width_N={50*LAMBDA}
.param width_P = {2.5*width_N}
.global gnd vdd

Vin a 0 pulse 0 1.8 0 100ps 100ps 9.9ns 20ns
VDD DD  gnd 1.8V 
**SIN(VO VA FREQ TD THETA PHASE) (page-87 NGSPICE manual V32 2020)

M1p      b       a       DD     DD  CMOSP   W={width_P}   L={LAMBDA} AS={2.5*width_P*LAMBDA} PS={5*LAMBDA+2*width_P} AD={2.5*width_P*LAMBDA} PD={5*LAMBDA+2*width_P}

M1n      b       a       gnd     gnd  CMOSN   W={width_N}   L={LAMBDA} AS={2.5*width_N*LAMBDA} PS={5*LAMBDA+2*width_N} AD={2.5*width_N*LAMBDA} PD={5*LAMBDA+2*width_N}

M2p      c       b       DD     DD  CMOSP   W={width_P}   L={LAMBDA} AS={2.5*width_P*LAMBDA} PS={5*LAMBDA+2*width_P} AD={2.5*width_P*LAMBDA} PD={5*LAMBDA+2*width_P}

M2n      c       b       gnd     gnd  CMOSN   W={width_N}   L={LAMBDA} AS={2.5*width_N*LAMBDA} PS={5*LAMBDA+2*width_N} AD={2.5*width_N*LAMBDA} PD={5*LAMBDA+2*width_N}

* C1 c gnd 100f
C1 c gnd 500f


* .dc VDS 0 1.8 0.1
.tran 1ps 40ns 

.param Vin_threshold = 1.8
.param Vout_threshold = 1.8

.measure tran t_vin_cross WHEN V(a)={Vin_threshold} CROSS=1
.measure tran t_vout_cross WHEN V(c)={Vout_threshold} CROSS=1
.measure tran prop_delay PARAM='t_vout_cross - t_vin_cross'

.control
set hcopypscolor = 1 *White background for saving plots
set color0=white ** color0 is used to set the background of the plot (manual sec:17.7)
set color1=black ** color1 is used to set the grid color of the plot (manual sec:17.7)

run

let Vout = V(c)
let Vin = V(a)
* let Vb = V(b)
plot  Vin Vout



.endc
```