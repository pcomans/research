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
CGContextAddArc //All angles in Quartz are specified in radians. The zero angle is along the positive x axis in Quartz coordinates and positive angles increase counterclockwise. The convenience function CGContextAddArc adds an arc segment to the current path. The starting point of the arc is defined by the values of centerX, centerY, radius, and startAngle. The ending point of the arc is defined by centerX, cen- terY, radius, and endAngle. The direction of the arc depends on the value of the clockwise parameter passed to the function. After this function returns, the cur- rent point of the current path is the ending point of the arc segment. The result- ing subpath is open; you must call CGContextClosePath if you want to close it.If a current point exists in the path prior to calling CGContextAddArc, Quartz first adds to the path a line segment from the current point to the starting point of the arc, then adds the arc segment. If there is no current point defined in the cur- rent path, this function adds only the arc segment to the path. takes the following parameters:
	context //the graphics context to add the path to.
	centerXcenterY// the x and y coordinates for the center of the circle thatdefines the arc.
	radius// the radius of the circle that defines the arc.
	startAngle// the starting angle for the arc.
	endAngle// the ending angle for the arc.
	clockwise// the direction along the circle to create the arc. If true, the direction of the arc is clockwise; otherwise it is counterclockwise.
CGContextAddArcToPoint provides another way to add an arc seg- ment to a path. The arc segment created by CGContextAddArcToPoint is defined by a radius and two tangent lines. Figure 6.8 shows the geometry involved; the arc segment added by CGContextAddArcToPoint is shown as a solid line. The function CGContextAddArcToPoint takes five parameters—the x and y coordi- nates of a point p1, the x and y coordinates of a second point p2, and the radius of the arc r. The starting point for constructing the segment added to the path is the point p0, the current point prior to calling CGContextAddArcToPoint. The line from the point p0 to the point p1 and the line from the point p1 to p2 are the two lines tangent to the arc. These lines plus the arc radius r define the arc. The starting point for the arc segment is the point p1t; at that point the arc is tangent to the line from p0 to p1. The ending point of the arc is the point p2t— the point where the arc is tangent to the line from p1 to p2. If the point p0 is not the same point as the tangent point p1t, the function CGContextAddArcToPoint first adds a line segment to the current path to connect the initial current point to the starting point of the arc segment, then adds the arc segment to the path.
	![CGContextAddArcToPoint example](https://dl.dropboxusercontent.com/u/2559476/Screen%20Shot%202015-10-31%20at%2015.54.35.png) 
```