**Important**
Changes to the current transformation matrix (CTM) do not affect the coordinates of path segments already added to the current path.

**Path Construction Primitives:**  
All paths in Quartz can be constructed using one or more of the following five basic path construction primitive functions, regardless of the complexity of the path.
```
CGContextMoveToPoint begins a new subpath in the current path.
CGContextAddLineToPoint adds a straight line segment to the current path.
CGContextAddCurveToPoint adds a cubic Bézier curve segment to the current path.
CGContextAddQuadCurveToPoint adds a quadratic Bézier curve segment to the current path.
CGContextClosePath ends the current path.
```

1. The first step when constructing a new path, whether constructing it with the path primitive functions or convenience functions, is to discard any existing path with the function CGContextBeginPath.
2. the next step is to call CGContextMoveToPoint to establish the initial point of the first subpath; this also establishes a current point. You then add segments to the path using the other path construction primitives