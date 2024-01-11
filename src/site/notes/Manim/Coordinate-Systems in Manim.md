- We have 2 ways to create coordinate systems, one of them is NumberPlane and the other one is Axes, I will show what both looks like here:
- One thing I would mention is when you assign coordinates to shift like I am doing here, we use c2p, which literally means cords to points, it is in reference "to" the mobject and not to the coordinate system, so I have shown example in this one how it works.
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
![[SpeedExample.mp4]]
