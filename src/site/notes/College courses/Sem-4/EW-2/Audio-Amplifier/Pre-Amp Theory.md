---
{"dg-publish":true,"permalink":"/college-courses/sem-4/ew-2/audio-amplifier/pre-amp-theory/"}
---


# Differential Amplifier using Transistors

As the name indicates, **Differential Amplifier** is a dc-coupled amplifier that amplifies the difference between two input signals. It is the building block of analog integrated circuits and operational amplifiers (op-amp). One of the important features of the differential amplifier is that it tends to reject or nullify the part of input signals which is common to both inputs. This provides very good noise immunity in many applications. Let’s see the block diagram of a differential amplifier.

[![Differential Amplifier](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-450x122.jpg)](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-1.jpg)

### Differential Amplifier

Vi1 and Vi2 are input terminals, and Vo1 and Vo2 are output terminals with respect to ground. We can feed two input signals at the same time or one at a time. In the former case, it is called dual input; otherwise, it is single input. Similarly, there are two ways to take output as well. If the output is taken from one terminal with respect to ground, it is unbalanced output. If the output is taken between two output terminals, it is balanced output.

---

## Differential Amplifier using BJT

The simplest form of differential amplifier can be constructed using Bipolar Junction Transistors (BJTs), as shown in the circuit diagram below. It is constructed using two matching transistors in common emitter configuration whose emitters are tied together.

[![Differential Amplifier using Transistor - Circuit Diagram](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-using-Transistor-Circuit-Diagram-450x407.jpg)](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-using-Transistor-Circuit-Diagram.jpg)

---

### Configurations

Based on the methods of providing input and taking output, differential amplifiers can have four different configurations:

1. Single Input Unbalanced Output
2. Single Input Balanced Output
3. Dual Input Unbalanced Output
4. Dual Input Balanced Output

---

### Single Input Unbalanced Output

In this configuration, only one input signal is given, and the output is taken from only one of the two collectors with respect to ground, as shown below.

[![Single Input Unbalanced Output](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-using-Transistor-Single-Input-Unbalanced-Output-389x450.jpg)](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-using-Transistor-Single-Input-Unbalanced-Output.jpg)

When input signal$V_{in1}$is applied to the transistor$Q_1$, its amplified and inverted voltage is generated at the collector of$Q_1$. At the same time, its amplified and non-inverted voltage is generated at the collector of$Q_2$. 

---

### Single Input Balanced Output

In this case, only one input signal is given, but the output is taken from both collectors. 

[![Single Input Balanced Output](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-using-Transistor-Single-Input-Balanced-Output-428x450.jpg)](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-using-Transistor-Single-Input-Balanced-Output.jpg)

The output combines the effects of both transistors, providing a more amplified version with no unnecessary DC content.

---

### Dual Input Unbalanced Output

Both inputs are given in this configuration, but the output is taken from only one collector.

[![Dual Input Unbalanced Output](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-using-Transistor-Dual-Input-Unbalanced-Output-431x450.jpg)](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-using-Transistor-Dual-Input-Unbalanced-Output.jpg)

---

### Dual Input Balanced Output

This configuration consists of two identical transistors$Q_1$and$Q_2$, with their emitters coupled together and outputs taken from both collectors.

The output voltage is:

$$V_o = A_d (V_{in1} - V_{in2})$$

Where$A_d$is the differential gain. When$V_{in1} = V_{in2}$, the output will be zero, suppressing common-mode signals.

---
## DC Analysis

DC analysis determines the operating point values$I_{CQ}$and$V_{CEQ}$. The DC equivalent circuit is obtained by reducing all AC signals to zero.

[![DC Analysis](https://electrosome.com/wp-content/uploads/porto_placeholders/100x95.jpg)](https://electrosome.com/wp-content/uploads/2016/07/Differential-Amplifier-using-Transistors-DC-Analysis.jpg)

Key equations for DC analysis:

1.$I_E = \frac{V_{EE} - V_{BE}}{2R_E}$
2.$V_{CE} = V_{CC} + V_{BE} - I_C R_C$

---

## AC Analysis

AC analysis determines the voltage gain$A_d$and input resistance$R_i$. The AC equivalent circuit is obtained by reducing all DC sources to zero.

Key result for voltage gain:

$$V_o = R_C \cdot (i_{e1} - i_{e2})$$

Where$i_{e1}$and$i_{e2}$are derived from the input signals.

---
