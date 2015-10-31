#### Misc

**CGContextClosePath** 
prior to calling CGContextStrokePath to stroke the path, it is closed by calling

**Important**
Changes to the current transformation matrix (CTM) do not affect the coordinates of path segments already added to the current path.


#### Path Construction Primitives:   
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

#### Path Construction Convenience Functions  
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
```
![example](https://dl.dropboxusercontent.com/u/2559476/Screen%20Shot%202015-10-31%20at%2016.07.50.png) 
![CGContextAddArcToPoint example](https://dl.dropboxusercontent.com/u/2559476/Screen%20Shot%202015-10-31%20at%2015.54.35.png) 

#### Stroking paths

**CGContextStrokePath** & **CGContextDrawPath** with the painting mode **kCGPathStroke**  
CGContextStrokePath or the function CGContextDrawPath with the painting mode kCGPathStroke. This usage of these two functions is identical, so use the one that suits your taste. Stroking a path can be combined with filling of that path by using the function CGContextDraw- Path with the painting modes kCGPathFillStroke or kCGPathEOFillStroke. These painting modes first fill and then stroke the path.

**CGContextStrokeRect** & **CGContextStrokeRectWithWidth**  
Stroked rectangles don’t require any path construction when you use the conve- nience functions CGContextStrokeRect and CGContextStrokeRectWithWidth

**CGContextSetLineWidth**.  
The default line width is 1 unit. Because the line width is specified in Quartz user space units, it is affected by changes to the CTM

#### Mitter:

**kCGLineJoinMiter** 
For this type of join, Quartz extends the outer edges of the stroke until they meet at an angle, similar to the corner on a picture frame.

**kCGLineJoinRound**  
Quartz draws a circular arc around the point where the segments meet. The diameter of this circle is the line width, resulting in a rounded corner.

**kCGLineJoinBevel**  
Quartz finishes the segments with butt line caps, leaving a notch at the corner. (See the next section “Line Caps” for more discussion of butt line caps.) The resulting notch beyond the segments is filled in with a triangle, resulting in a beveled corner.

**Note:** When the join style is a miter join, Quartz replaces a miter join with a bevel join if the angle between two connected path segments is too small; this avoids sharp spikes. The miter limit determines the angle when a miter join is replaced by a bevel join; it only applies when the Quartz line join is kCGLineJoinMiter. The miter limit is based on the miter length. The miter length is the distance between the point where the inner edges of the stroke intersect and the point where the outer edges of the stroke intersect. Figure 6.13 illustrates the miter length. If the ratio of the miter length and the line width exceeds the miter limit, Quartz replaces a miter join with a bevel join.
The ratio between the miter length and the line width is expressed in terms of the angle θ between the two segments:
 miterlimit =  miterlength /linewidth = 1 / (sin*(θ/2))

**Note:** The default Quartz miter limit is 10. This value causes miter joins to be replaced by beveled joins when the angle between the segments is less than about 11 degrees. A larger miter limit allows the miter length to be larger, that is, allows larger spikes. Figure 6.14 shows the results of the miter limit. In the lower por- tion of the figure, the miter limit is the default value of 10. As the angle between the two connected path segments gets smaller, the miter join is replaced by a beveled join. The upper portion of the figure shows a large miter limit with a small angle between the two connected path segments; the sharp spike is the

#### Caps

**kCGLineCapButt**  
is the default. Quartz stops the stroke at the endpoint of the open path segment and squares off the end. There is no projec- tion beyond the end of the path.

**kCGLineCapSquare**  
The square line cap () is sometimes called a projected line cap. Quartz projects the stroke beyond the endpoint by one-half the line width and squares off the end of the stroke.

**kCGLineCapRound**  
When the line cap is a round line cap (), Quartz paints a semi- circular arc at each endpoint. The diameter of the arc is the line width.




#### Line Dash

CGContextSetLineDash


#### Filling Paths

**The nonzero winding number rule** takes into account the direction of the path segments that make up the path. To determine whether a point is in the interior of a path, Quartz conceptually constructs a ray from that point out to infinity. It counts the number of times the ray intersects the path where the path travels from right to left and subtracts that count from the number of times the ray intersects the path where the path goes left to right.The resulting number is the winding number
Note: fill that path using the nonzero winding number rule by using the function CGContextFillPath or the function CGContext- DrawPath with the painting mode kCGPathFill
Note: Stroking a path can be combined with filling by using the function CGContext- DrawPath with the painting mode kCGPathFillStroke to use the nonzero winding number fill rule

**The second fill rule available in Quartz is the even-odd fill rule.** For this rule, the direction of the path segments is unimportant. To determine whether a point is in the interior of a path using the even-odd fill rule, Quartz conceptually con- structs a ray from that point out to infinity. It counts the number of times the ray intersects the path. If the number of intersections is odd, the point is inside the path. If the number of intersections is even, the point is outside the path. The even-odd fill rule is rarely used. Its use is typically confined to emulating another graphics model such as QuickDraw
Note: To fill the current path using the even-odd fill rule, you can use the function CGContextEOFillPath or the function CGContextDrawPath with the painting mode kCGPathEOFill.
Note: kCGPathEOFillStroke to use the even-odd fill rule. These painting modes first fill, then stroke the path.

**Closing a fill:**  
Regardless of the fill rule applied, to fill an open subpath, Quartz must implicitly close it prior to filling. Operations that fill a path implicitly close all open sub- paths prior to filling. Operations that both fill and stroke only implicitly close the path for the filling portion of the operation; the stroke portion of the operation is applied without closing any open subpaths that exist. If you want the path to be closed prior to stroking, you must explicitly close it with CGContextClosePath.

Note: Convenince fill methods for rects: Quartz has convenience functions for filling rectangles. The function CGContext- FillRect allows you to fill a specified rectangle without performing any explicit path construction. The function CGContextFillRects allows you to fill a specified array of rectangles without any explicit path construction. These functions ignore any current path in the context and, after they return, the current path in the context is empty. Quartz uses the nonzero winding number fill rule when filling rectangles with these convenience functions.