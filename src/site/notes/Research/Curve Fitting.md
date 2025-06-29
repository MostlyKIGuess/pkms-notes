---
{"dg-publish":true,"permalink":"/research/curve-fitting/"}
---


- Use Gauss-Newton
- Or Use Google Ceres
- Or Curve Fitting with g2o

Some literature:

Graph optimization is a way to express the optimization problem as a graph. A graph consists of a number of vertices and edges connecting these vertices. A vertex is used to represent an optimization variable, and an edge is used to represent an error term. Therefore, for any of the above-mentioned nonlinear least-squares problems, we can construct a corresponding graph. We can simply call it a graph or use the probability graph definition, call it a Bayesian graph or a factor graph. Sometimes it is also called a hypergraph because an edge can be connected to more than two variables, e.g., where an error term is related to more than two variables.