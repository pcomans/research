**CGPathCreateMutable function**
Creates a new mutable path of type CGMutablePathRef and returns its handle. We should dispose of this path once we are done with it, as you will soon see.

**CGPathMoveToPoint procedure**
Moves the current pen position on the path to the point specified by a parameter of type CGPoint.

**CGPathAddLineToPoint procedure**
Draws a line segment from the current pen position to the specified position (again, specified by a value of type CGPoint).

**CGContextAddPath procedure**
Adds a given path (specified by a path handle) to a graphics context, ready for drawing.

**CGContextDrawPath procedure**
Draws a given path on the graphics context.

**CGPathRelease procedure**
Releases the memory allocated for a path handle.

**CGPathAddRect procedure**
Adds a rectangle to a path. The rectangle’s boundaries are specified by a CGRect structure.
There are three important drawing methods that you can ask the CGContextDrawPath procedure to perform:
Draws a line (stroke) to mark the boundary or edge of the path, using the currently selected stroke color.

**kCGPathFill**
Fills the area surrounded by the path with the currently selected fill color.

**kCGPathFillStroke**
Combines stroke and fill. Uses the currently selected fill color to fill the path, and the currently selected stroke color to draw the edge of the path. We’ll see an ex- ample of this method in the following section.

**Getting bounds of a path**
CGPathGetBoundingBox(self.path);


**Basic drawing (creates a cross on an iphone)**
```objc
- (void)drawRect:(CGRect)rect{
   /* Create the path */
   CGMutablePathRef path = CGPathCreateMutable();
   /* How big is our screen? We want the X to cover the whole screen */
   CGRect screenBounds = [[UIScreen mainScreen] bounds];
   /* Start from top-left */ 
   CGPathMoveToPoint(path, NULL,screenBounds.origin.x, screenBounds.origin.y);
   /* Draw a line from top-left to bottom-right of the screen */
   CGPathAddLineToPoint(path, NULL,screenBounds.size.width, screenBounds.size.height);
   /* Start another line from top-right */ 
   CGPathMoveToPoint(path,NULL, screenBounds.size.width, screenBounds.origin.y);
   /* Draw a line from top-right to bottom-left */ 
   CGPathAddLineToPoint(path,NULL, screenBounds.origin.x, screenBounds.size.height);
   /* Get the context that the path has to be drawn on */
   CGContextRef currentContext = UIGraphicsGetCurrentContext();
   /* Add the path to the context so we can draw it later */
   CGContextAddPath(currentContext, path);
   /* Set the blue color as the stroke color */ 
   [[UIColor blueColor] setStroke];
   /* Draw the path with stroke color */ 
   CGContextDrawPath(currentContext, kCGPathStroke);
   /* Finally release the path object */ 
   CGPathRelease(path);
}
```


