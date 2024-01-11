---
{"dg-publish":true,"permalink":"/manim/common-functions-in-manim/"}
---


This will contain list of some useful functions ,which I find useful haha, almost every function is useful but I just prefer these.

### ReplacementTransform
{ #3e9409}


---

- Replaces and morphs a mobject into a target mobject.
	### Parameters
	
	mobject  
	    The starting `~.Mobject`.  
	target_mobject  
	    The target `~.Mobject`.

## TracedPath
{ #9c76cd}


```python
class TracedPath(traced_point_func, stroke_width=2, stroke_color=ManimColor('#FFFFFF'), dissipating_time=None, **kwargs)
``` 
PARAMETERS:

- **traced_point_func** (_Callable_) – The function to be traced.
    
- **stroke_width** (_float_) – The width of the trace.
    
- **stroke_color** (_ParsableManimColor_ _|_ _None_) – The color of the trace.
    
- **dissipating_time** (_float_ _|_ _None_) – The time taken for the path to dissipate. Default set to `None` which disables dissipation.

## VGroup 
- Merges mobjects so that it is easier to perform animations on them:
Example:
```python
from manim import *

class VGroupExample(Scene):

def construct(self):

Circle1 = Circle()

Circle1.set_fill(RED, opacity=0.6)

Circle1.shift(2*LEFT) # so that the Circles do not overlap

Circle2 = Circle()

Circle2.set_fill(GREEN, opacity=0.6)

Circle2.shift(2*RIGHT) # so that the Circles do not overlap

Circle3 = Circle()

Circle3.set_fill(BLUE, opacity=0.6)

vgroup1 = VGroup(Circle1 , Circle2)

vgroup2 = VGroup(Circle3)

self.add(vgroup1,vgroup2)

self.wait(1)

self.play(vgroup1.animate.shift(2*UP))

self.play(vgroup2.animate.shift(2*DOWN))

vgroup1+=vgroup2

self.wait(1)

self.play(vgroup1.animate.rotate(PI/4))

vgroup1.set_fill(YELLOW, opacity=0.6)

self.wait(1)

vgroup1-=vgroup2

self.play(vgroup1.animate.rotate(PI/4))

self.play(vgroup1.animate.scale(1.5), #animate groups separately

vgroup2.animate.scale(0.5))

self.play((vgroup1+vgroup2).animate.shift(2*RIGHT)) #animate groups together without modification

self.play((vgroup1-Circle1).animate.shift(2*LEFT)) #animate group without a singular component

self.wait(1)
```
Output:
![[VGroupExample.mp4]]


## add_updater
{ #561871}


- Add an update function to the scene.
	The scene updater functions are run every frame, and they are the last type of updaters to run.
usually takes a point,  to be specific a float as a parameter and can be used like this: 
This might get confusing but just look at what pointer does.. basically it updates every frame so you will see it will follow your Tracker, we will get into this once we have covered animation basics.
```python
from manim import *

class ValueTrackerExample(Scene):
    def construct(self):
        number_line = NumberLine()
        pointer = Vector(DOWN)
        label = MathTex("x").add_updater(lambda m: m.next_to(pointer, UP))

        tracker = ValueTracker(0)
        pointer.add_updater(
            lambda m: m.next_to(
                        number_line.n2p(tracker.get_value()),
                        UP
                    )
        )
        self.add(number_line, pointer,label)
        tracker += 1.5
        self.wait(1)
        tracker -= 4
        self.wait(0.5)
        self.play(tracker.animate.set_value(5)),
        self.wait(0.5)
        self.play(tracker.animate.set_value(3))
        self.play(tracker.animate.increment_value(-2))
        self.wait(0.5)
```
![[ValueTrackerExample-1.mp4]]

{ #1baea5}


