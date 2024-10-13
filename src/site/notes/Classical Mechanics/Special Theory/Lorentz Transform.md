---
{"dg-publish":true,"permalink":"/classical-mechanics/special-theory/lorentz-transform/"}
---



### Lorentz Transformation Equations

Good Resource: https://www.feynmanlectures.caltech.edu/II_26.html

For a frame moving with velocity $v$ along the $x$-axis, the Lorentz transformation equations are:

- **Time Transformation**:
  $$
  t' = \gamma \left( t - \frac{vx}{c^2} \right)
  $$
- **Space Transformation**:
  $$
  x' = \gamma (x - vt)
  $$

Where $\gamma$ is the Lorentz factor(always greater than 1 btw):
$$
\gamma = \frac{1}{\sqrt{1 - \frac{v^2}{c^2}}}
$$

---

### Intuition Behind Lorentz Transformation

The Lorentz transformation ensures that the speed of light is constant ($c$) in all inertial reference frames. It modifies the time and space coordinates so that observers in different reference frames (moving relative to each other) will still measure the same speed for light.

- For example, consider a light beam emitted from the origin at $t = 0$. In the stationary frame, the light moves according to $x = ct$. After applying the Lorentz transformation, the same light beam in a moving frame satisfies $x' = ct'$. This shows that **light travels at speed $c$ in all frames**, regardless of the observer's velocity.
- For me this understanding comes from a matrix transformation where speed of light is a eigenvector and we are shifting the space time according to that vector.

---

### Space-Time Diagram for Lorentz Transformation

The following diagram shows the relationship between time ($t$) and space ($x$) in both the stationary frame $(t, x)$ and the moving frame $(t', x')$.

```tikz
\begin{document}
\begin{tikzpicture}[scale=3.0]

% Draw axes for the stationary frame
\draw[->] (-1, 0) -- (5, 0) node[right] {$x$}; % x-axis
\draw[->] (0, -1) -- (0, 5) node[above] {$t$}; % t-axis

% Worldline of object moving at speed of light (red)
\draw[thick, red] (0, 0) -- (4, 4) node[above right] {Object 3 ($v = c$)};
\draw[thick, red, dashed] (0, 0) -- (2.5, 4) node[above right] {$x' = ct'$};

% Worldline of object 1 (blue, slower than light)
\draw[thick, blue] (0, 0) -- (4, 3) node[above right] {Object 1 ($v_1 < c$)};
\draw[thick, blue, dashed] (0, 0) -- (2, 4) node[above right] {$x'_1 = v_1 t'$};

% Worldline of object 2 (green, slower than light)
\draw[thick, green] (0, 0) -- (4, 2) node[above right] {Object 2 ($v_2 < c$)};
\draw[thick, green, dashed] (0, 0) -- (1.5, 4) node[above right] {$x'_2 = v_2 t'$};

% Labels for axes
\draw (4, 0) node[below] {Space ($x$)};
\draw (0, 4) node[left] {Time ($t$)};


\end{tikzpicture}
\end{document}

```

