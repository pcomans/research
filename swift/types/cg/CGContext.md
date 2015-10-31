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
3. you can begin a new subpath in the cur- rent path that you are constructing by calling CGContextMoveToPoint.
4. CGContextAddLineToPoint adds a straight-line segment from the current point to a new point in user space


**CGContextAddCurveToPoint** adds a cubic Bézier curve segment to a Quartz path. This function takes the following parameters:
```
context, the graphics context to add the path to.
I cp1x,cp1y, the x and y coordinates for the first control point.
I cp2x,cp2y, the x and y coordinates for the second control point. I p1x,p1y, the x and y coordinates for the endpoint of the curve.
```

**CGContextAddQuadCurveToPoint** adds a quadratic Bézier curve seg- ment to a Quartz path. This function takes the following parameters:
```
context, the graphics context to add the path to.
I cpx,cpy, the x and y coordinates for the control point.
I p1x,p1y, the x and y coordinates for the endpoint of the curve.
```

**Path Construction Convenience Functions**
```
CGContextAddRect //adds a closed rectangular subpath to the current path
CGContextAddLines //To add a number of connected line segments at once, you can use the function . You supply an array of points and Quartz constructs a new subpath using these points. The first point in the array is the initial point on the subpath. The first line segment is constructed from the initial point to the second point in the array. Each subsequent line segment is constructed from the trailing endpoint of the previous line segment to the next point in the array. The final result is a series of connected line segments. When CGContextAddLines returns, the current point is the last point in the array of points passed to the function. The resulting subpath is open; you must call CGContextClosePath if you want to close it.
CGContextAddArc takes the following parameters:
	context //the graphics context to add the path to.
	centerXcenterY// the x and y coordinates for the center of the circle thatdefines the arc.
	radius// the radius of the circle that defines the arc.
	startAngle// the starting angle for the arc.
	endAngle// the ending angle for the arc.
	clockwise// the direction along the circle to create the arc. If true, the direction of the arc is clockwise; otherwise it is counterclockwise.
```