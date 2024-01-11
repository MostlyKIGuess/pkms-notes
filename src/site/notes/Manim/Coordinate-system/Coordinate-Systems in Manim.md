---
{"dg-publish":true,"permalink":"/manim/coordinate-system/coordinate-systems-in-manim/","noteIcon":""}
---


- I think I have missed it but we use .plot to [[Manim/Coordinate-system/Plotsss\|Plotsss]]  and get graphs. So whenever I say graph I mean the return value of .plot, it returns parametric function.

- We have 2 ways to create coordinate systems, one of them is NumberPlane and the other one is Axes, I will show what both looks like here:
- One thing I would mention is when you assign coordinates to shift like I am doing here, we use c2p, which literally means cords to points, it is in reference "to" the mobject and not to the coordinate system, so I have shown example in this one how it works. Here are more point generations [[Manim/Coordinate-system/Points Generation\|Points Generation]].
### c2p  || NumberPlane

```python
from manim import *

class SpeedExample(Scene):

def construct(self):

dot = Dot(color=YELLOW)

cordinatesystems = NumberPlane(

x_length=10,

y_length=10,

x_range=[-10, 10, 1],

y_range=[-10, 10, 1],

axis_config={"color": WHITE},

);

NumberPlane.add_coordinates(cordinatesystems)

self.add(cordinatesystems)

self.play(Create(dot))

self.play(dot.animate.shift(cordinatesystems.c2p(2, 3)))

self.play(dot.animate.shift(cordinatesystems.c2p(2, 0)))

self.play(dot.animate.shift(cordinatesystems.c2p(-6, -3)),run_time = 3)

# self.wait(1)

self.remove(dot, cordinatesystems)

self.wait(1)

Grid = Axes(

x_length=10,

y_length=10,

x_range=[-10, 10, 1],

y_range=[-10, 10, 1],

axis_config={"color": WHITE},

);

dot = Dot(color=YELLOW)

self.add(Grid)

self.play(Create(dot))

self.play(dot.animate.shift(Grid.c2p(2, 3)))

self.play(dot.animate.shift(Grid.c2p(-2, -3)))

self.wait(1)
```
<iframe width="560" height="315" src="https://www.youtube.com/embed/XAbJ5BOHffI?si=LZmQSTla1cMXah0f" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


### get_graph_label || get_T_label || get_area
- How get_area works is:
	-  it takes the function as the first parameter, x_range as the second,color and opacity are not necessary,but you can add them as well.
  - get_T_label:
	  - Creates a labelled triangle marker with a vertical line from the x-axis to a curve at a given x-value. It takes the function as graph = $f(x)$ and where do you want the label to be , here I have used Pi/2, you can use anything almost.
- get_graph_label:
	-   Creates a label at the graph where you have specified the x, for example in this code we are generating the label in the graph "sinfunc" where x = pi/2, and you add the label as Text or MathTex, whichever suits your needs the best.
	
	```python
	from manim import *
	
	
	class Area(Scene):
	
	def construct(self):
	
	grid = Axes(
	
	x_length=12,
	
	y_length=6,
	
	x_range=[-6, 6, 1],
	
	y_range=[-3, 3, 1],
	
	axis_config={"color": WHITE},
	
	);
	
	grid.add_coordinates()
	
	# grid.shift(2*DOWN)
	
	sinfunc = grid.plot(lambda x:2*np.sin(x),color = YELLOW)
	
	area = grid.get_area(sinfunc,
	
	x_range=(PI / 2,4),
	
	color=(BLUE),
	
	opacity=0.6,
	
	)
	
	horizontal_line = grid.get_horizontal_line(grid.input_to_graph_point(PI / 2, sinfunc), color=YELLOW)
	
	graphlabel = grid.get_graph_label(sinfunc,label= MathTex(r"\frac{\pi}{2}"), x_val=PI/2,color=BLUE)
	
	tlabel = grid.get_T_label(x_val=PI/2,graph=sinfunc,line_color=BLUE)
	
	self.add(grid)
	
	self.play(Create(sinfunc))
	
	self.play(Create(area),run_time = 4)
	
	self.remove(area)
	
	self.play(Create(horizontal_line),Create(graphlabel),Create(tlabel),run_time =5)
	
	self.wait()
	```
<iframe width="560" height="315" src="https://www.youtube.com/embed/0dUo1YX08rQ?si=c_Hm8UXWyXAnH9zk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> 
