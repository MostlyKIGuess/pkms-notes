---
{"dg-publish":true,"permalink":"/manim/coordinate-system/plotsss/","noteIcon":""}
---


Hahaha I missed it at first, but not anymore..

## Implicit Functions

```python
plot_implicit_curve(func, min_depth=5, max_quads=1500, **kwargs)
```
PARAMETERS:

- **func** ( Callable [[float,float], float]) – The function to graph, in the form of f(x, y) = 0.
    
- **min_depth** (_int_) – The minimum depth of the function to calculate.
    
- **max_quads** (_int_) – The maximum number of quads to use.
    
- **kwargs** (_Any_) – Additional parameters to pass into `ImplicitFunction`.

```python
from manim import *

class ImplicitExample(Scene):
    def construct(self):
        ax = Axes()
        a = ax.plot_implicit_curve(
            lambda x, y: y * (x - y) ** 2 - 4 * x - 8, color=BLUE
        )
        self.add(ax, a)
```
![Pasted image 20240112002259.png](/img/user/Manim/Coordinate-system/Pasted%20image%2020240112002259.png)

## Parametric Curves
```python
plot_parametric_curve(function, use_vectorized=False, **kwargs)
```
Example(myfav render):
```python
from manim import *

import numpy as np

class Butterfly(Scene):

def construct(self):

ax = Axes(

x_length=15,

)

t_values = np.linspace(0, 12 * PI, 1000)

  

def butterfly_func(t):

x = np.sin(t) * (np.exp(np.cos(t)) - 2 * np.cos(4 * t) - np.sin(t / 12)**5)

y = np.cos(t) * (np.exp(np.cos(t)) - 2 * np.cos(4 * t) - np.sin(t / 12)**5)

return np.array([x, y, 0])

  

butterfly_curve = ax.plot_parametric_curve(butterfly_func,t_range=[0,12*PI],color = BLUE)

  

self.play(Create(butterfly_curve), run_time=10)

self.wait()
```
<iframe width="560" height="315" src="https://www.youtube.com/embed/WhcKkGoZTFA?si=0vEKDpUFfodlJzjW" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> 


## Polar Graphs
```python
plot_polar_graph(r_func, theta_range=None, **kwargs)
```
- r_func will be in terms of theta, theta_range is pretty self explanatory.
```python
from manim import *

class Polars(Scene):

def construct(self):

polar = PolarPlane()

self.play(Create(polar))

#write cannabis curve in numpy

cannabis = lambda t:(1 + 0.9 * np.cos(8 * t))*(1 + 0.1 * np.cos(24 * t))*(0.9 + 0.1*np.cos(200*t))*(1+np.sin(t))

graph = polar.plot_polar_graph(cannabis, [0, 2*PI],color=RED)

self.play(Create(graph),run_time = 10)
```
<iframe width="560" height="315" src="https://www.youtube.com/embed/PqX_PBT96zM?si=cbketqx22RL8Qy24" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> 

## 3D Graphs
- For 3D Graphs we use something called as plot_surface and ThreeDScene to set up camera and stuff.
```python
plot_surface(function, u_range=None, v_range=None, colorscale=None, colorscale_axis=2, **kwargs)
```

```python
from manim import *

class ThreeD(ThreeDScene):

def construct(self):

self.set_camera_orientation(phi=75 * DEGREES, theta=-60 * DEGREES)

threed = ThreeDAxes()

self.play(Create(threed))

def bump(u,v):

x = u;

y = v;

z = np.sin(u)*np.cos(v);

return z;

threedplot = threed.plot_surface(

bump,

u_range=[-PI,PI],

v_range=[-PI,PI],

color=BLUE,

fill_opacity=0.5,

checkerboard_colors=[TEAL, TEAL_B],

)
self.play(Create(threedplot),run_time = 10)

```
<iframe width="560" height="315" src="https://www.youtube.com/embed/CNW8vJtXpak?si=fD_6tRcpfQemDSv9" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> 
```python
def Coolfuncig(u,v):

x = u;

y = v;

s = np.sqrt(u**2+v**2 + 0.000000001);

z = abs(1/s);

return z;

plot = threed.plot_surface(

Coolfuncig,

u_range=[-3,3],

v_range=[-3,3],

color=BLUE,

fill_opacity=0.9,

checkerboard_colors=[TEAL, TEAL_B],

)

self.play(Create(plot),run_time = 3)
```
<iframe width="560" height="315" src="https://www.youtube.com/embed/Rzmpuo7lspk?si=7fAM5v-6xhEZzZPs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> 


