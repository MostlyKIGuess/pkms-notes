---
{"dg-publish":true,"permalink":"/manim/matrix/matrix-in-manim/"}
---

- As discussed earlier they kind of work like numpy arrays, so here's an example to kick start
```python
from manim import *

class MatrixExamples(Scene):
    def construct(self):
        m0 = Matrix([[2, "\pi"]])
        m1 = Matrix([[2, 0, 4]],
            v_buff=1.3,
            h_buff=0.8,
            bracket_h_buff=SMALL_BUFF,
            bracket_v_buff=SMALL_BUFF,
            left_bracket="\{",
            right_bracket="\}")
        m1.add(SurroundingRectangle(m1.get_columns()[1]))
        m2 = Matrix([[2, 1]],
            element_alignment_corner=UL,
            left_bracket="(",
            right_bracket=")")
        m3 = Matrix([[2, 1]],
            left_bracket="\\langle",
            right_bracket="\\rangle")
        m4 = Matrix([[2, 1]],
        ).set_column_colors(RED, GREEN)
        m5 = Matrix([[2, 1]],
        ).set_row_colors(RED, GREEN)
        g = Group(
            m0,m1,m2,m3,m4,m5
        ).arrange_in_grid(buff=2)
        self.add(g)
```


![Pasted_image_20240115025634.png| Matrix Image](/img/user/Manim/Matrix/Pasted_image_20240115025634.png)




- You can apply Matrix Transformations as we discussed [[Manim/Transformation in Manim#^a80ada\|../Transformation in Manim#^a80ada]] , even in 3D for example:
```python
from manim import *

  

class ExampleArrow3D(ThreeDScene):

def construct(self):

axes = ThreeDAxes()

arrow = Arrow3D(

start=np.array([0, 0, 0]),

end=np.array([2, 2, 2]),

resolution=8,

color=BLUE,

)

self.set_camera_orientation(phi=75 * DEGREES, theta=30 * DEGREES)

self.play(Create(axes))

self.play(Create(arrow))

label = Text("2i+2j+2k",font_size=20).next_to(arrow.get_end())

self.add_fixed_in_frame_mobjects(label)

self.play(FadeIn(label))

self.remove(label)

icap = Arrow3D(

start=np.array([0, 0, 0]),

end=np.array([1, 0, 0]),

resolution=8,

color=GREEN,

)

jcap = Arrow3D(

start=np.array([0, 0, 0]),

end=np.array([0, 1, 0]),

resolution=8,

color=TEAL,

)

kcap = Arrow3D(

start=np.array([0, 0, 0]),

end=np.array([0, 0, 1]),

resolution=8,

color=RED,

)

matrix = [[-1, 2, 0]]

group = VGroup(arrow, icap, jcap, kcap)

self.play(ApplyMatrix(matrix, group),run_time=5)

label = Text("8i+12j-2k",font_size=20).next_to(arrow.get_end())

self.add_fixed_in_frame_mobjects(label)

self.play(FadeIn(label))
```
<iframe width="560" height="315" src="https://www.youtube.com/embed/CSiB4kypz4o?si=tJwekxsQreqETT8N" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> 
