---
{"dg-publish":true,"permalink":"/manim/introduction-to-manim/"}
---

- Basics of running manim:
   - If you have installed it , then make sure you have the community version which currently is the v$0.19.0$ , and so basically what we will be doing is creating classes, and there are different type of [[Manim/Scenes in Manim\|Scenes in Manim]]  we will go there but later.., now we will just use Scene.
   - Now manim has something called self which method we will use to create animation in the videos.
   - So something like this, your code would look like:
   ```python
   from manim import *

	class MyScene(Scene):
	    def construct(self):
        # Your animation code
	        text = Text("Hello, Manim!")
	        self.play(Create(text))
	```
Output:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/1JHjW-L-vjk?si=qpVTT4qXdVQAVp7d" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

- To get the output you would do the following in your terminal with the same directory as your manim python file.
```
manim (filename.py) (ClassName) -pqh 
```
here write those without the brackets and -pqh suggests, play quality high, you can use
-pql, -pqm and -pqp, suggesting low, medium, production.
H - 1080p60
L - 480p30
P - 1440p60
M - 720p30
k - 4k60fps
