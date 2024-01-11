---
{"dg-publish":true,"permalink":"/manim/transformation-in-manim/"}
---

- As we saw in the Building blocks there are many simple function, now we dwell a little deeper into this transformation which will make things more interesting.


## ApplyMatrix
- For this it takes it's first parameter as the matrix which we will make using numpy it's just an array at certain basic level. Here's an example on how to use it, when I use Numberplane and ThreeDAxes, I will talk about those later in this docs as well.
```python
from manim import *

import numpy as np

class ApplyMatrixExample(Scene):

def construct(self):

#to do this you need to just know this one basic stuff of numpy on how it handles matrices

#here's how it does it:

matrix2x2 = np.array([[1, 2], [2, 1]])

matrix3x3 = np.array([[1, 2, 3], [0, 1, 1], [3, 2, 1]])

Grid = NumberPlane();

self.play(Create(Grid))

ThreeDGrid = ThreeDAxes()

self.play(ApplyMatrix(matrix2x2, Grid))

self.remove(Grid)

self.play(Create(ThreeDGrid))

self.play(ApplyMatrix(matrix3x3, ThreeDGrid))

self.wait(1)
```
![[ApplyMatrixExample.mp4]]

