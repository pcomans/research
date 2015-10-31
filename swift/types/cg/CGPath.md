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

**Draw a rectangle:**
```objc
- (void)drawRect:(CGRect)rect{ 
   /* Create the path first. Just the path handle. */ 
   CGMutablePathRef path = CGPathCreateMutable();
   /* Here are our rectangle boundaries */ 
   CGRect rectangle = CGRectMake(10.0f, 10.0f,200.0f, 300.0f);
   /* Add the rectangle to the path */ 
   CGPathAddRect(path,NULL, rectangle);
   /* Get the handle to the current context */ 
   CGContextRef currentContext = UIGraphicsGetCurrentContext();
   /* Add the path to the context */ 
   CGContextAddPath(currentContext,path);
   /* Set the fill color to cornflower blue */ 
   [[UIColor colorWithRed:0.20f green:0.60f blue:0.80f alpha:1.0f] setFill];
   /* Set the stroke color to brown */ 
   [[UIColor brownColor] setStroke];
   /* Set the line width (for the stroke) to 5 */ 
   CGContextSetLineWidth(currentContext,5.0f);
   /* Stroke and fill the path on the context */ 
   CGContextDrawPath(currentContext, kCGPathFillStroke);
   /* Dispose of the path */ 
   CGPathRelease(path);
}
```

**Draw multiple rectangles at once**

```objc
- (void)drawRect:(CGRect)rect{ // Drawing code
   /* Create the path first. Just the path handle. */ CGMutablePathRef path = CGPathCreateMutable();
   /* Here are our first rectangle boundaries */ CGRect rectangle1 = CGRectMake(10.0f,
   10.0f, 200.0f, 300.0f);
   /* And the second rectangle */ CGRect rectangle2 = CGRectMake(40.0f,
   100.0f, 90.0f, 300.0f);
   /* Put both rectangles into an array */ CGRect rectangles[2] = {
   rectangle1, rectangle2 };
   /* Add the rectangles to the path */ CGPathAddRects(path,
   NULL,
   (const CGRect *)&rectangles, 2);
   /* Get the handle to the current context */ CGContextRef currentContext = UIGraphicsGetCurrentContext();
   /* Add the path to the context */ CGContextAddPath(currentContext,
   path);
   /* Set the fill color to cornflower blue */ [[UIColor colorWithRed:0.20f
   green:0.60f blue:0.80f
   alpha:1.0f] setFill];
   /* Set the stroke color to black */ [[UIColor blackColor] setStroke];
   /* Set the line width (for the stroke) to 5 */ CGContextSetLineWidth(currentContext,
   5.0f);
   /* Stroke and fill the path on the context */ CGContextDrawPath(currentContext,
   kCGPathFillStroke);
   /* Dispose of the path */ CGPathRelease(path);
}
```

**Adding Shadows to Shapes**
```objc
- (void) drawRectAtTopOfScreen{
   /* Get the handle to the current context */ CGContextRef currentContext =
   UIGraphicsGetCurrentContext();
   CGContextSetShadowWithColor(currentContext, CGSizeMake(10.0f, 10.0f),
   20.0f,
   [[UIColor grayColor] CGColor]);
   /* Create the path first. Just the path handle. */ CGMutablePathRef path = CGPathCreateMutable();
   /* Here are our rectangle boundaries */ CGRect firstRect = CGRectMake(55.0f, 60.0f,
   150.0f, 150.0f);
   /* Add the rectangle to the path */ CGPathAddRect(path,
   NULL, firstRect);
   /* Add the path to the context */ CGContextAddPath(currentContext,
   path);
   /* Set the fill color to cornflower blue */ [[UIColor colorWithRed:0.20f
   green:0.60f blue:0.80f
   alpha:1.0f] setFill];
   /* Fill the path on the context */ CGContextDrawPath(currentContext,
   kCGPathFill);
   /* Dispose of the path */ CGPathRelease(path);
}
```


**Adding shaddow to only one rectangle:**
```objc
- (void) drawRectAtTopOfScreen{
   /* Get the handle to the current context */ CGContextRef currentContext =
   UIGraphicsGetCurrentContext(); CGContextSaveGState(currentContext); CGContextSetShadowWithColor(currentContext,
   CGSizeMake(10.0f, 10.0f), 20.0f,
   [[UIColor grayColor] CGColor]);
   /* Create the path first. Just the path handle. */ CGMutablePathRef path = CGPathCreateMutable();
   /* Here are our rectangle boundaries */ CGRect firstRect = CGRectMake(55.0f, 60.0f,
   150.0f, 150.0f);
   /* Add the rectangle to the path */ CGPathAddRect(path,
   NULL, firstRect);
   /* Add the path to the context */ CGContextAddPath(currentContext,
   path);
   /* Set the fill color to cornflower blue */ [[UIColor colorWithRed:0.20f
   green:0.60f blue:0.80f
   alpha:1.0f] setFill];
   /* Fill the path on the context */ CGContextDrawPath(currentContext,
   kCGPathFill);
   /* Dispose of the path */ CGPathRelease(path);
   /* Restore the context to how it was when we started */
   CGContextRestoreGState(currentContext); 
}
```



```objc

```



```objc

```



```objc

```