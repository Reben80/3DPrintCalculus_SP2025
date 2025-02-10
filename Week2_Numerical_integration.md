# Assinment 2: 3D Visualization of Left Riemann and Trapezoidal Sums

This notebook demonstrates how to visualize numerical integration using the Left Riemann Sum and Trapezoidal Rule in three dimensions.

## Left Riemann Sum

The Left Riemann Sum is a numerical approximation method for definite integrals. It estimates the area under a curve by summing the areas of rectangles whose heights are determined by the function value at the left endpoint of each subinterval.

The Left Riemann Sum formula is:

$$  
S_L = \sum_{i=0}^{n-1} f(x_i) \Delta x
$$

where:
- $\Delta x = \frac{b-a}{n}$ is the width of each subinterval.
- $x_i = a + i \Delta x$ is the left endpoint of each subinterval.
- $f(x_i)$ is the function value at the left endpoint.

### Function Definition

```mathematica
LeftRiemannSum3DPositive[f_, {a_?NumericQ, b_?NumericQ}, n_Integer?Positive, thickness_?Positive, plotFunction_ : True] := 
  Module[{dx, cuboids, funcPlot, combinedPlot},
   If[b <= a, Return[$Failed, Module]];
   dx = (b - a)/n;
   cuboids = 
    Table[{Opacity[0.5], Blue, 
      Cuboid[{a + i*dx, 0, 0}, {a + (i + 1)*dx, thickness, 
        f[a + i*dx]}]}, {i, 0, n - 1}];
   funcPlot = 
    Plot3D[f[x], {x, a, b}, {y, 0, thickness}, 
     PlotStyle -> Opacity[0.3], Mesh -> None, 
     AxesLabel -> {"x", "y (thickness)", "z (f(x))"}];
   combinedPlot = 
    If[TrueQ[plotFunction], 
     Show[funcPlot, Graphics3D[cuboids], PlotRange -> All, 
      BoxRatios -> {1, 0.1, 1}], 
     Show[Graphics3D[cuboids], PlotRange -> All, 
      BoxRatios -> {1, 0.1, 1}]];
   combinedPlot];
```

### Example Usage

```mathematica
myPlot = LeftRiemannSum3DPositive[Sin, {0, 2*Pi}, 30, 0.1, False];
Show[myPlot]
Export["sinPlot1.stl", myPlot];
```

---

## Trapezoidal Rule

The Trapezoidal Rule is another numerical method for approximating definite integrals. Instead of using rectangles, it approximates the area under a curve using trapezoids.

The Trapezoidal Rule formula is:

$$
S_T = \frac{\Delta x}{2} \sum_{i=0}^{n-1} \left[f(x_i) + f(x_{i+1})\right]
$$

where:
- $\Delta x = \frac{b-a}{n}$ is the width of each subinterval.
- $x_i = a + i \Delta x$ are the points of partition.

### Function Definition

```mathematica
TrapezoidalSum3D[f_, {a_?NumericQ, b_?NumericQ}, n_Integer?Positive, thickness_?Positive, plotFunction_ : True] := 
  Module[{dx, prisms, funcPlot, combinedPlot, i, x0, x1, A, B, C, D, E, F, G, H},
   If[b <= a, Return[$Failed, Module]];
   dx = (b - a)/n;
   prisms = Table[(x0 = a + i*dx;
      x1 = a + (i + 1)*dx;
      A = {x0, 0, 0}; B = {x1, 0, 0}; C = {x1, 0, f[x1]}; D = {x0, 0, f[x0]};
      E = {x0, thickness, 0}; F = {x1, thickness, 0}; G = {x1, thickness, f[x1]}; H = {x0, thickness, f[x0]};
      {Opacity[0.5], Green,
       Polygon[{A, B, C, D}],
       Polygon[{E, F, G, H}],
       Polygon[{A, B, F, E}],
       Polygon[{B, C, G, F}],
       Polygon[{C, D, H, G}],
       Polygon[{D, A, E, H}]}), {i, 0, n - 1}];
   funcPlot = 
    Plot3D[f[x], {x, a, b}, {y, 0, thickness}, 
     PlotStyle -> Opacity[0.3], Mesh -> None, 
     AxesLabel -> {"x", "y (thickness)", "z (f(x))"}];
   combinedPlot = 
    If[TrueQ[plotFunction], 
     Show[funcPlot, Graphics3D[prisms], PlotRange -> All, 
      BoxRatios -> {1, 0.1, 1}], 
     Show[Graphics3D[prisms], PlotRange -> All, 
      BoxRatios -> {1, 0.1, 1}]];
   combinedPlot];
```

### Example Usage

```mathematica
myTrapezoidalPlot = TrapezoidalSum3D[Sin, {0, Pi}, 10, 0.1, False];
Show[myTrapezoidalPlot]
Export["sinTrapezoidalPlot.stl", myTrapezoidalPlot];
```
