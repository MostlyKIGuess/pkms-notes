---
{"dg-publish":true,"permalink":"/manim/building-blocks-of-manim/","noteIcon":""}
---


- To understand placements and alignment of the objects , I would assume you have read [[Manim/General Animation in Manim#^458fb2\|General Animation in Manim#^458fb2]] so to get how the objects are created and gets added into the working canvas, that's only when you will understand the further concepts.
### Shift
**.Shift({Amount} $*$ Direction)**  to align the objects from center.
- So this would become:
```python
 circle = Circle()
        square = Square()
        triangle = Triangle()

        circle.shift(1*LEFT)
        square.shift(1*UP)
        triangle.shift(1*RIGHT)

        self.add(circle, square, triangle)
```
![Pasted image 20240109235634.png](/img/user/Manim/Pasted%20image%2020240109235634.png)
### set_stroke || set_Fill
- set_fill accepts two parameters , color and opacity.
- set_stroke accepts two parameters, color and width.
So this would again become:
```python
  circle = Circle().shift(LEFT)
        square = Square().shift(UP)
        triangle = Triangle().shift(RIGHT)

        circle.set_stroke(color=GREEN, width=20)
        square.set_fill(YELLOW, opacity=1.0)
        triangle.set_fill(PINK, opacity=0.5)

        self.add(circle, square, triangle)
```
![Pasted image 20240109235907.png](/img/user/Manim/Pasted%20image%2020240109235907.png)
### FadeIn || FadeOut 
- FadeIn is used with self.play(FadeIn(mobject)) to fade in the object.
- Same with FadeOut, Rotate accepts two parameters , if not just keeps rotating till it's runtime.
```python
from manim import *

class SomeAnimations(Scene):
    def construct(self):
        square = Square()

        self.play(FadeIn(square))

        self.play(Rotate(square, PI/4))

        self.play(FadeOut(square))

        self.wait(1)
```
Output:
<iframe width="560" height="315" src="https://www.youtube.com/embed/a34tjLjBop8?si=mOhU7NE3zqWShDqg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Rotate:
```python

class Rotate(mobject=None, *args, use_override=True, **kwargs)

```
Animation that rotates a Mobject.

PARAMETERS:

- **mobject** ([_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")) – The mobject to be rotated.
    
- **angle** (_float_) – The rotation angle.
    
- **axis** (_np.ndarray_) – The rotation axis as a numpy vector.
    
- **about_point** (_Sequence__[__float__]_ _|_ _None_) – The rotation center.
    
- **about_edge** (_Sequence__[__float__]_ _|_ _None_) – If `about_point``is ``None`, this argument specifies the direction of the bounding box point to be taken as the rotation center.
```python
from manim import *

  

class Rotation(Scene):

def construct(self):

square = Square(color=BLUE)

self.play(Create(square))

self.play(

Rotate(

square,

angle=PI/4,

about_point = ORIGIN,

rate_func = linear,

run_time = 3,

),

)

self.remove(square)

self.wait()

circle = Circle(color=RED)

self.play(Create(circle))

self.play(circle.animate.shift(2*UP))

self.play(

Rotate(

circle,

angle=PI/4,

about_point = ORIGIN,

rate_func = linear,

run_time = 3,

),

)
```

<iframe width="560" height="315" src="https://www.youtube.com/embed/elpkihiKYSI?si=rjiqB-YtoRLlpZWY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### move_to || next_to || edge_to || grow_from_center
- move_to:
	```python
		obj.move_to(point_or_mobject, aligned_edge=UP)
	```
- next_to
	```python
	obj.next_to(mobject, direction=RIGHT, buff=0.1)
	```
- edge_to
	```python
	obj.edge_to(point_or_mobject, direction=RIGHT)
	```
- grow_from_center
	```python
	obj.grow_from_center(scale_factor=1.2)
	```
- grow_from_edge
```python
self.play(GrowFromEdge(triangle, DOWN))
```
- grow_from_point
```python
 self.play(GrowFromPoint(square, ORIGIN))
```








## Custom Example Using Building Blocks
- Here's a code that I will further explain step by step, first let's see the output and code.
```python
from manim import *


class Custom(Scene):

def construct(self):

square = Square()

square.set_fill(RED, opacity=0.6)

self.play(Create(square))

self.play(Rotate(square, PI/4))

self.play(square.animate.set_stroke(YELLOW, width=20),runtime =2)

square.set_stroke(BLUE, width=10)

self.wait(2)

self.play(square.animate.set_fill(GREEN, opacity=0.2),runtime =2)

triangle = Triangle()

self.play(square.animate.shift(3*UP).rotate(PI/4).scale(2),runtime = 3)

self.play(ReplacementTransform(square, triangle),runtime = 3)
```
![[Custom.mp4]]
- Here we used our knowledge of animate method along with building blocks to understand how to animate stuff in manim, first we are creating a square , then we learnt above that to fill it any color we would use set fill, now we remember we had already done set fill before the animation in self.play right?
- So what happens is it renders the red block directly, to explain this further I have used Rotate which as I explained a way to rotate that mobject,now we want to set a stroke but we also want to animate it then we use .animate.set_stroke rather than just doing it directly, you can see in the video what happens if we do it directly, we used self.wait() to observe that frame and grasp it fully, then we saw how fill can be animated as well.
- Now for the shift up, if we would have just shifted up it would not have rotated, but we also want to rotate it and scale it twice, so we use multiple parameters methods here and you can also not specify the runtime, but it's a good practice to do so for convenience of understanding how your animation will look like, then we use ReplacementTransform function which you can find [[Manim/Common Functions in Manim#^3e9409\|Common Functions in Manim#^3e9409]] here. To morph triangle into the square. 


- I now recommend going to [[Manim/General Animation in Manim#^ed7255\|General Animation in Manim#^ed7255]] and you will understand it a lot better. :)

