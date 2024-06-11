---
{"dg-publish":true,"permalink":"/manim/coordinate-system/graphs/reimann-integrals-and-slopes/"}
---



```python
get_riemann_rectangles(graph, x_range=None, dx=0.1, input_sample_type='left', stroke_width=1, stroke_color=ManimColor('#000000'), fill_opacity=1, color=(ManimColor('#58C4DD'), ManimColor('#83C167')), show_signed_area=True, bounded_graph=None, blend=False, width_scale_factor=1.001)
```
These are the parameters, now let's try doing a Reimann integral graph on a let's say $x^{2}-5$ . It will return a VGroup of mobjects that we can animate yayy ;)

```python
get_secant_slope_group(x, graph, dx=None, dx_line_color=ManimColor('#FFFF00'), dy_line_color=None, dx_label=None, dy_label=None, include_secant_line=True, secant_line_color=ManimColor('#83C167'), secant_line_length=10)
```

- This is not included in the example but,this function returns the function which is a derivative of the graph which we did using .plot:
```python
plot_derivative_graph(graph, color=ManimColor('#83C167'), **kwargs)
```


```python
from manim import *


class Reimann(Scene):

def construct(self):

grid = Axes(y_range=[-2, 10])

func = grid.plot(lambda x : -1*(x**2 - 5),color=YELLOW)

grid.add_coordinates()

reimannlol = grid.get_riemann_rectangles(

func,

x_range = [-2,2],

dx=0.1,

input_sample_type="right",

stroke_width=0.2,

fill_opacity=0.8,

color=(TEAL, BLUE_B, DARK_BLUE),

);

self.play(Create(grid))

self.play(Create(func),run_time = 4)

self.play(Create(reimannlol),run_time = 5)

self.remove(reimannlol)

self.wait()

#slopes timmeee

slopeatzero = grid.get_secant_slope_group(

x=0,

graph=func,

dx=0.1,

secant_line_length=3,

secant_line_color=BLUE,

);

self.play(Create(slopeatzero),run_time = 2)

slopeatone = grid.get_secant_slope_group(

x=1,

graph=func,

dx=0.1,

dx_label="dx = 0.1",

dy_label="dy = 0.9",

secant_line_length=3,

secant_line_color=BLUE,

);

self.play(Transform(slopeatzero,slopeatone),run_time = 2 )
```
<iframe width="560" height="315" src="https://www.youtube.com/embed/w1fr4mYz6Y8?si=9R_QeogYDavapmJb" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> 
