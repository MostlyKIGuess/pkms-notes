---
{"dg-publish":true,"permalink":"/college-courses/sem2/aec/a5/aec-a5/"}
---


# 1 $I_D$ vs $V_{DS}$
![Pasted image 20240326202327](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240326202327.png)


![Pasted image 20240326204257](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240326204257.png)


This is the $I_{D} \ \ vs \ \ V_{DC}$  plot that we obtain when we sweep both the $V_{GS} \ \ \text{and} \ \ V_{DS}$ the different color lines represent different $V_{GS}$ values with higher $V_{GS}$ being at the top as 
$$I_{D} = \frac{1}{2}\cdot\mu C_{ox}\frac{W}{L}\cdot (V_{Gs}-V_{Th})(V_{DS}-\frac{V_{DS}^{2}}{2})  $$
hence due to the domination term in the quadratic being negative giving us the double derivative to be negative we see this graph to have that downward opening. Assuring it is $I_{D} \ \ vs \ \ V_{DS}$ .

We observe from the plot that different curves are obtained while sweeping ${V_{DS}}$ for various values of ${V_{GS}}$.

- When ${V_{GS}}$ is very small (less than a threshold voltage ${V_{T}}$), the MOSFET is inactive, resulting in zero current flow—a state known as the cut-off region.
- As ${V_{GS}}$ increases beyond ${V_{T}}$, the MOSFET becomes active. In the linear/triode mode, as ${V_{DS}}$ increases (where ${V_{DS} < V_{GS} - V_{T}}$), the MOSFET behaves like a resistor. Current increases with $latex{V_{DS}}$ until it reaches ${V_{GS} - V_{T}}$.
- With further increase in ${V_{DS}}$ (${V_{DS} > V_{GS} - V_{T}}$), the MOSFET enters saturation mode. In this mode, the current remains constant, as evidenced by the plots.

In saturation mode, the current value obtained is determined by:
$$I_{D} = \frac{1}{2}\cdot\mu C_{ox}\frac{W}{L}\cdot (V_{Gs}-V_{Th})(V_{DS}-\frac{V_{DS}^{2}}{2})  $$


# 2 $I_D$ vs $V_{GS}$


## a) $V_{DS}=50mV$

#### Plot:
![Pasted image 20240327011703](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240327011703.png)

- To determine $V_T$ from the $ID$ versus $V_{GS}$ plot, first, we create a graph where we plot the current ($ID$) against the voltage ($V_{GS}$): $ID$ versus $V_{GS}$ using $ID$ and $V_{GS}$.
- Next, we observe the steepness of the line on the graph.
- Then, we locate the point on the graph where the line is steepest and note the corresponding voltage value ($V_{GS}$).
- Finally, we draw a tangent line to the curve at that steep point. The intersection of this tangent line with the voltage axis ($V_{GS}$ axis) gives us $V_T$


![Pasted image 20240327110821](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240327110821.png)


This shows that it gives max value around 717 mV, so we can extrapolate in the original graph from there as that will have the highest slope , as this is the derivative plot.


![Pasted image 20240327110844](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240327110844.png)


![Pasted image 20240327111846](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240327111846.png)

$$V_{Th}=551mV \ \ \mu C_{ox} = 0.329 \frac{uA}{V^2}$$


## b) $V_{DS}=1.8V$



![Pasted image 20240327115915](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240327115915.png)


- To determine $V_T$ from the $ID$ versus $V_{GS}$ plot, first, we create a graph where we plot the current ($ID$) against the voltage ($V_{GS}$): $I_D$ versus $V_{GS}$ using $I_D$ and $V_{GS}$.
- Next, we observe the steepness of the line on the graph.
- Then, we locate the point on the graph where the line is steepest and note the corresponding voltage value ($V_{GS}$).
- Finally, we draw a tangent line to the curve at that steep point. The intersection of this tangent line with the voltage axis ($V_{GS}$ axis) gives us $V_T$


Here the graph we will be using will be sqrt as in the $I_D$ equation when the value of $V_{DS}$ is increased we assume(by our knowledge of the circuit) that it's in saturation so equation changes to: 

$$I_{D}= \frac{1}{2}\cdot\mu C_{ox} \frac{W}{L} (V_{GS}-V_{Th})^2$$

- So taking it's sqrt we get it will be directly proportional to $V_{Th}$ gives us the appropriate results.

![Pasted image 20240327122238](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240327122238.png)


$$V_{Th}= 455$$


$$
\Delta V_{\text{th}} = \gamma \left( \frac{L_{\text{DD}}}{L_{\text{DE}}} \right) \frac{V_{\text{DS}}}{V_{\text{DD}}}
$$

So on increasing the $V_{DS}$ we observe the decrease in the barrier lowering of the $V_{Th}$ .
$V_{Th}=V_{Th}-\eta V_{DS}$  

DIBL occurs as the voltage difference between drain and source increases the electric field inside the conduction channel increase and it starts pulling the electrons out and due to this number of electrons in the channel starts to increase making so that even if $V_{GS}$ is a bit smaller than then also the electrons will carry the current hence causing the $V_{Th}$ to be small.

# 3)

## a) NMOS device

- We repeat the same steps from the part 2, to get $V_{Th}$.
- Here we give out with 3 $V_{bs}$ being $-0.9V \ , \ 0V \ ,\ 0.9V.$ 



![Pasted image 20240327135559](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240327135559.png)


#### a) 900mV

![Pasted image 20240328164009](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240328164009.png)

![Pasted image 20240328164019](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240328164019.png)

### b) -900mV


![Pasted image 20240328164554](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240328164554.png)

### c) 0V

![Pasted image 20240327115915](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240327115915.png)



Using these calculations we get the following values:

![Pasted image 20240328165243](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240328165243.png)


So we observe that as we increase the $V_{bs}$, $V_{Th}$ is decreasing this is due to the fact that whenever we increase  $V_{bs}$ it creates a depletion region between source and body which behaves as a capacitor and further due to that more number of electrons are attracted to substrate and hence depletion region increases causing the barrier to be lower.

$$ 
( V_{th} = V_{th0} + \gamma(\sqrt{|2\phi_F + V_{sb}|} - \sqrt{2\phi_F}) )
$$
## b) PMOS 


![Pasted image 20240403121301](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240403121301.png)


![Pasted image 20240403122022](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240403122022.png)


From this tangent we have the following values:

| -0.9V | 0V    | 0.9V  | $V_{bs}$ |
| ----- | ----- | ----- | -------- |
| 387mV | 534mV | 723mV | $V_{th}$ |

We see that we observe the same effect just inverted in the case of PMOS because of the simple fact that now we wants holes in out depletion region so attraction of electrons would resist that change hence on increasing voltage our threshold voltage increases. And it is backed by our formula as well.

$$ 
( V_{th} = V_{th0} + \gamma(\sqrt{|2\phi_F + V_{sb}|} - \sqrt{2\phi_F}) )
$$




# 4)


Using the value of $V_{th}$ as $550mV$ in this experiment as we got it from the last question, we get the graph something like:

![Pasted image 20240404171111](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240404171111.png)

and the maximum value would be:

![Pasted image 20240404171124](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240404171124.png)

so it comes to be around 1.1.

so if we increase W/L by 4 times then we can increase gm by 4 times as gm = CoxW/L(Vgs-Vth) and because u and Cox are the property of the transistor we know we can not modify them, so we modify W/L to increase gm, but there's a limit on how much can we modify it, changing it we get:


![Pasted image 20240404171805](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240404171805.png)


something around 2.7, so seeing change of 3 which is due to the fact that changing the parameters it changes the area of the capacitor as well hence decreasing the other parameters, so we can't really increase it by just decreasing the length or increasing the width.



# 5)

## a)

![Pasted image 20240404224727](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240404224727.png)



![Pasted image 20240404225305](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240404225305.png)

The Blue region shows the saturation region as Vgs-Vth is lesser than 500mV, the red box represents the triode region and the cyan region represents deep triode region, this goes to show that we observe max value comes around 917 which makes sense as 917 - 455 is around 500mV, showing gm is usually higher as we get into saturation.

$$g_{m}=382.91405\mu\Omega ^{-1}$$ 

$$V_{Gs}=917mV$$ 

## b)

![Pasted image 20240404230234](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240404230234.png)

We will have the same distribution of regions and the max value here comes out to be 1.1250682mOhm-1 which is almost 4 times the original one but there is tradeoff.

When we adjust the size of a MOSFET , it impacts how well it amplifies signals. One thing to consider is that as we make changes, we have to balance between two factors: gain  and bandwidth.

If we make the MOSFET larger to increase its transconductance (how well it turns voltage changes into current changes), we can get more amplification, but the range of frequencies it can handle might shrink.

Another thing to keep in mind is that as we boost the gain, the range of input signal amplitudes it can handle may become limited. If the amplified signal becomes too strong, it might cause the MOSFET to operate improperly, which is not good for an amplifier. This trade-off is called the [transconductance-AC swing trade-off](https://www.researchgate.net/figure/Transconductance-versus-voltage-swing-showing-a-typical-oscillator-curve-and-a-preferred_fig2_260627221) .


# 6)

![Pasted image 20240405003749](/img/user/College courses/Sem2/AEC/a5/Pasted image 20240405003749.png)



