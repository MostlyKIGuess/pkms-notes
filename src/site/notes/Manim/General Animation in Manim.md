
- General Idea of animation in Manim is that you mobjects inside a self and then we play it, in the simplest terms.
## An animation.
Animations have a fixed time span.

PARAMETERS:

- **mobject** – The mobject to be animated. This is not required for all types of animations.
- **run_time** – The duration of the animation in seconds.

## Example (Essential):

^458fb2

```python
from manim import *


class AnimationExample(Scene):

def construct(self):

circle = Circle()

square = Square()

triangle = Triangle()

self.add(circle)

self.wait(2)

self.remove(circle)

self.play(ShowCreationThenFadeOut(square),runtime = 3)

#or

self.play(Create(triangle))

self.play(Rotating(triangle))

self.play(FadeOut(triangle))
```
Output:
![[AnimationExample.mp4]]
- Explanation Line by Line:
	- First line as I mentioned we create a class that we work on, so to play this we would have done in the terminal:
	```sh
	manim Example.py AnimationExample -pqh
	```
 
	- So we define the construct method on self as the parameter.
	- Then we use from the [[Mobjects in Manim]] circle, Square and Triangle and grab them inside the variables we have defined.
	- Now the important part of animation..
	- Imagine Self as a canvas so when you just do **add** it adds it into the canvas, we will learn about it's placement later on in this but as of now just understand that when we do self.add(stuff), we are adding that stuff into the canvas, as you can see from the video , that doesn't really have any animation but when we have done that on the square  we have used ShowCreationThenFadeOut which will give you a warning but that's okay , it's just a good practice to use the other option that I showed in triangle that to create it first and then fade it out, I will list other useful function here as well, which you can use for animation. 
	-  Self.wait({seconds}), accepts seconds as the input and you can specify the runtime which also takes values in seconds as I have done for square creation. 






- Before we dwell more into examples, be sure to learn and understand the [[Building Blocks of Manim]]. 
## Tracing Objects:
- So in manim we can trace objects using [[Common Functions in Manim#^9c76cd]] ,[[Common Functions in Manim#^561871]] and [[Common Functions in Manim#^1baea5]]
- Here move_to basically means the object will move to that place where we have pointed, and circ.get_start means the dot in the canvas where the circles started from.


Example:
```python
from manim import *

  

class Tracing(Scene):

def construct(self):

circ = Circle(color=RED).shift(4*LEFT)

dot = Dot(color=RED).move_to(circ.get_start())

rolling_circle = VGroup(circ, dot)

trace = TracedPath(circ.get_start)

rolling_circle.add_updater(lambda m: m.rotate(-0.4))

self.add(trace, rolling_circle)

# self.wait()

self.play(rolling_circle.animate.shift(8*RIGHT),rate_func = linear, run_time = 4)

# self.wait()

rolling_circle.clear_updaters()

trace.clear_updaters()

trace2 = TracedPath(circ.get_start, dissipating_time=0.4, stroke_width=5, stroke_color=BLUE)

self.add(trace2, rolling_circle)

rolling_circle.add_updater(lambda m: m.rotate(+0.2))

self.play(rolling_circle.animate.shift(8*LEFT), run_time=4,rate_func = linear)
```
![[Tracing.mp4]]
- Here we have used the previously discussed function and different type of tracing by modifying parameters.
 ^ed7255
