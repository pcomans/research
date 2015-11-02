Todos: 
- [ ] Skim the second Quartz book to see if you missed something in the first
- [ ] Go through apples Quartz book and online website to add stuff that are new etc. 
- [ ] Add more imagery
- [ ] Read Saving and Restoring a Coordinate System page  01

**Goals of this research paper:**     
This research paper should result in a comprehensive Graphics Util lib, so that you can work efficiently with quartz

**Things this research paper covers:**  
- Drawing paths
- Stroking and filling paths
- Transformations of Paths
- Applying Shadows to paths
- Applying stroke and fill Gradients 
- An overview of all the convenience methods quartz offer. (**So that you know what id doesn't offer**)
- Working with text

**Things this research paper does not cover:**    
- Quartz ColorSpace
- Images (although basic imagery techniques should be covered in the future)
- BlendModes
- PDF support
- Using the quartz pattern function
- Working with Colorspaces
- CGDataProider and CGDataConsumer


# Misc
**CGContextClosePath** prior to calling CGContextStrokePath to stroke the path, it is closed by calling  
**Important** Changes to the current transformation matrix (CTM) do not affect the coordinates of path segments already added to the current path.


# Context Path Construction Primitives:   
All paths in Quartz can be constructed using one or more of the following five basic path construction primitive functions, regardless of the complexity of the path.
- **CGContextMoveToPoint** begins a new subpath in the current path.
- **CGContextAddLineToPoint** adds a straight line segment to the current path.
- **CGContextAddCurveToPoint** adds a cubic Bézier curve segment to the current path.
- **CGContextAddQuadCurveToPoint** adds a quadratic Bézier curve segment to the current path.
- **CGContextClosePath** ends the current path.

**Creating paths within the context**:
1. The first step when constructing a new path, whether constructing it with the path primitive functions or convenience functions, is to discard any existing path with the function **CGContextBeginPath**.
2. the next step is to call **CGContextMoveToPoint** to establish the initial point of the first subpath; this also establishes a current point. You then add segments to the path using the other path construction primitives
3. you can begin a new subpath in the cur- rent path that you are constructing by calling **CGContextMoveToPoint**.
4. **CGContextAddLineToPoint** adds a straight-line segment from the current point to a new point in user space


**CGContextAddCurveToPoint** Adds a cubic Bézier curve segment to a Quartz path. This function takes the following parameters:
1. **context**, the graphics context to add the path to.
2. **cp1x**,**cp1y**, the x and y coordinates for the first control point.
3. **cp2x**,**cp2y**, the x and y coordinates for the second control point. I p1x,p1y, the x and y coordinates for the endpoint of the curve.

**CGContextAddQuadCurveToPoint** adds a quadratic Bézier curve seg- ment to a Quartz path. This function takes the following parameters:
1. **context**, the graphics context to add the path to.
2. **cpx**,**cpy**, the x and y coordinates for the control point.
3. **p1x**,**p1y**, the x and y coordinates for the endpoint of the curve.

# Context Path Construction Convenience Functions  
- **CGContextAddRect** Adds a closed rectangular subpath to the current path
- **CGContextAddLines** To add a number of connected line segments at once, you can use the function . You supply an array of points and Quartz constructs a new subpath using these points. The first point in the array is the initial point on the subpath. The first line segment is constructed from the initial point to the second point in the array. Each subsequent line segment is constructed from the trailing endpoint of the previous line segment to the next point in the array. The final result is a series of connected line segments. When CGContextAddLines returns, the current point is the last point in the array of points passed to the function. The resulting subpath is open; you must call CGContextClosePath if you want to close it.
- **CGContextAddArc** All angles in Quartz are specified in radians. The zero angle is along the positive x axis in Quartz coordinates and positive angles increase counterclockwise. The convenience function CGContextAddArc adds an arc segment to the current path. The starting point of the arc is defined by the values of centerX, centerY, radius, and startAngle. The ending point of the arc is defined by centerX, cen- terY, radius, and endAngle. The direction of the arc depends on the value of the clockwise parameter passed to the function. After this function returns, the cur- rent point of the current path is the ending point of the arc segment. The result- ing subpath is open; you must call CGContextClosePath if you want to close it.If a current point exists in the path prior to calling CGContextAddArc, Quartz first adds to the path a line segment from the current point to the starting point of the arc, then adds the arc segment. If there is no current point defined in the cur- rent path, this function adds only the arc segment to the path. takes the following parameters:

**CGContextAddArc** takes these arguments:  
1. **context** the graphics context to add the path to.
2. **centerX** **centerY** the x and y coordinates for the center of the circle thatdefines the arc.
3. **radius** the radius of the circle that defines the arc.
4. **startAngle** the starting angle for the arc.
5. **endAngle** the ending angle for the arc.

**CGContextAddArcToPoint**  
Provides another way to add an arc seg- ment to a path. The arc segment created by CGContextAddArcToPoint is defined by a radius and two tangent lines. Figure 6.8 shows the geometry involved; the arc segment added by CGContextAddArcToPoint is shown as a solid line. The function CGContextAddArcToPoint takes five parameters—the x and y coordi- nates of a point p1, the x and y coordinates of a second point p2, and the radius of the arc r. The starting point for constructing the segment added to the path is the point p0, the current point prior to calling CGContextAddArcToPoint. The line from the point p0 to the point p1 and the line from the point p1 to p2 are the two lines tangent to the arc. These lines plus the arc radius r define the arc. The starting point for the arc segment is the point p1t; at that point the arc is tangent to the line from p0 to p1. The ending point of the arc is the point p2t— the point where the arc is tangent to the line from p1 to p2. If the point p0 is not the same point as the tangent point p1t, the function CGContextAddArcToPoint first adds a line segment to the current path to connect the initial current point to the starting point of the arc segment, then adds the arc segment to the path.  
<img width="320" alt="example" src="https://dl.dropboxusercontent.com/u/2559476/Screen%20Shot%202015-10-31%20at%2016.07.50.png">  
<img width="320" alt="CGContextAddArcToPoint example" src="https://dl.dropboxusercontent.com/u/2559476/Screen%20Shot%202015-10-31%20at%2015.54.35.png">

# Stroking paths
**CGContextStrokePath** & **CGContextDrawPath** with the painting mode kCGPathStroke This usage of these two functions is identical, so use the one that suits your taste. Stroking a path can be combined with filling of that path by using the function CGContextDraw- Path with the painting modes kCGPathFillStroke or kCGPathEOFillStroke. These painting modes first fill and then stroke the path.

**CGContextStrokeRect** & **CGContextStrokeRectWithWidth**  
Stroked rectangles don’t require any path construction when you use the conve- nience functions CGContextStrokeRect and CGContextStrokeRectWithWidth

**CGContextSetLineWidth**.  
The default line width is 1 unit. Because the line width is specified in Quartz user space units, it is affected by changes to the CTM

# Mitter:
**kCGLineJoinMiter** 
For this type of join, Quartz extends the outer edges of the stroke until they meet at an angle, similar to the corner on a picture frame.

**kCGLineJoinRound**   
Quartz draws a circular arc around the point where the segments meet. The diameter of this circle is the line width, resulting in a rounded corner.

**kCGLineJoinBevel**   
Quartz finishes the segments with butt line caps, leaving a notch at the corner. (See the next section “Line Caps” for more discussion of butt line caps.) The resulting notch beyond the segments is filled in with a triangle, resulting in a beveled corner.

**Note:**   
When the join style is a miter join, Quartz replaces a miter join with a bevel join if the angle between two connected path segments is too small; this avoids sharp spikes. The miter limit determines the angle when a miter join is replaced by a bevel join; it only applies when the Quartz line join is kCGLineJoinMiter. The miter limit is based on the miter length. The miter length is the distance between the point where the inner edges of the stroke intersect and the point where the outer edges of the stroke intersect. Figure 6.13 illustrates the miter length. If the ratio of the miter length and the line width exceeds the miter limit, Quartz replaces a miter join with a bevel join.
The ratio between the miter length and the line width is expressed in terms of the angle θ between the two segments:
 miterlimit =  miterlength /linewidth = 1 / (sin*(θ/2))

**Note:**   
The default Quartz miter limit is 10. This value causes miter joins to be replaced by beveled joins when the angle between the segments is less than about 11 degrees. A larger miter limit allows the miter length to be larger, that is, allows larger spikes. Figure 6.14 shows the results of the miter limit. In the lower por- tion of the figure, the miter limit is the default value of 10. As the angle between the two connected path segments gets smaller, the miter join is replaced by a beveled join. The upper portion of the figure shows a large miter limit with a small angle between the two connected path segments; the sharp spike is the

## Caps
**kCGLineCapButt**   
is the default. Quartz stops the stroke at the endpoint of the open path segment and squares off the end. There is no projec- tion beyond the end of the path.

**kCGLineCapSquare**   
The square line cap () is sometimes called a projected line cap. Quartz projects the stroke beyond the endpoint by one-half the line width and squares off the end of the stroke.

**kCGLineCapRound**   
When the line cap is a round line cap (), Quartz paints a semi- circular arc at each endpoint. The diameter of the arc is the line width.


## Line Dash
CGContextSetLineDash


## Filling Paths
**The nonzero winding number rule**   
takes into account the direction of the path segments that make up the path. To determine whether a point is in the interior of a path, Quartz conceptually constructs a ray from that point out to infinity. It counts the number of times the ray intersects the path where the path travels from right to left and subtracts that count from the number of times the ray intersects the path where the path goes left to right.The resulting number is the winding number
Note: fill that path using the nonzero winding number rule by using the function CGContextFillPath or the function CGContext- DrawPath with the painting mode kCGPathFill
Note: Stroking a path can be combined with filling by using the function CGContext- DrawPath with the painting mode kCGPathFillStroke to use the nonzero winding number fill rule

**The second fill rule available in Quartz is the even-odd fill rule.**   
For this rule, the direction of the path segments is unimportant. To determine whether a point is in the interior of a path using the even-odd fill rule, Quartz conceptually con- structs a ray from that point out to infinity. It counts the number of times the ray intersects the path. If the number of intersections is odd, the point is inside the path. If the number of intersections is even, the point is outside the path. The even-odd fill rule is rarely used. Its use is typically confined to emulating another graphics model such as QuickDraw
**Note:** To fill the current path using the even-odd fill rule, you can use the function CGContextEOFillPath or the function CGContextDrawPath with the painting mode kCGPathEOFill.
**Note:** kCGPathEOFillStroke to use the even-odd fill rule. These painting modes first fill, then stroke the path.

**Closing a fill:**    
Regardless of the fill rule applied, to fill an open subpath, Quartz must implicitly close it prior to filling. Operations that fill a path implicitly close all open sub- paths prior to filling. Operations that both fill and stroke only implicitly close the path for the filling portion of the operation; the stroke portion of the operation is applied without closing any open subpaths that exist. If you want the path to be closed prior to stroking, you must explicitly close it with CGContextClosePath.

<img width="300" alt="fill & stroke" src="https://dl.dropboxusercontent.com/u/2559476/Screen%20Shot%202015-11-01%20at%2007.35.49.png">

**Note:** Convenience fill methods for rects: Quartz has convenience functions for filling rectangles. The function CGContext- FillRect allows you to fill a specified rectangle without performing any explicit path construction. The function CGContextFillRects allows you to fill a specified array of rectangles without any explicit path construction. These functions ignore any current path in the context and, after they return, the current path in the context is empty. Quartz uses the nonzero winding number fill rule when filling rectangles with these convenience functions.

**Clearing context:**    
CGContextClearRect, which should not be confused with an erasing operation. Information about the function CGContextClearRect is in “Erasing and Clearing a Context” (page 351 of the programing with quartz book).

## Clipping
Every Quartz context has a current clipping area—the area into which Quartz can draw. Any drawing that lies outside this clipping area does not appear; only those portions inside the clipping area are painted. Drawing is clipped to the clipping area in the context.
Quartz provides two ways to perform clipping. The first is based on paths. In Tiger and later versions, Quartz also allows clipping to image masks.

Quartz offers two convenience functions that clip to a rectangular area. The function CGContextClipToRect intersects a rectangle with the existing clipping area. Quartz intersects the supplied rectangle with the existing clipping area to create the new clipping area on the context. The function CGContextClipToRectsconstructs a clipping area that consists of the interior of a sequence of rectangles. Quartz intersects the interior of these rectangles with the existing clipping area to create a new clipping area. The function CGContextClipToRects uses the non- zero winding number rule to determine the interior of the rectangles. Both these functions ignore any existing path in the context; after they return, the current path in the context is empty.

## CGPath 
**CGPathRef** cannot be modified or added to but a  
**CGMutablePathRef** can be modified.  
**CGContextAddPath** add a CGPathRef or CGMutablePathRef to the current path in the context.  
**CGPathAddArc** takes:  
1. x,y coordinates for the center of the circle that defines the arc path seg- ment.  
2. Radius of the circle that defines the arc path segment.  
3. arc angle for the arc path segment.  
4. ending arc angle for the arc path segment.  
5. isClockwise Boolean value. If true, the arc segment is added clockwise; otherwise it is added counterclockwise.  
6. CGMutablePathRef of the CGPath object to add the arc path segment to.  
7. CGAffineTranform ( If this pointer is NULL, Quartz applies no transformation to the arc path coordinates prior to adding them to the CGPath.)  

**CGPath Construction Primitives**   
- CGPathMoveToPoint 

**Note:** These parameters closely parallel the parameters to the function CGContextAdd- Arc, which adds an arc path segment to the current path in the context. Instead of passing the context as the first parameter, you pass the mutable CGPath

```swift
// Create a new transform consisting of a 45-degree rotation
let theTransform:CGAffineTransform  = CGAffineTransformMakeRotation(M_PI/4);//45deg
// Apply a scaling transformation to the transform just created. 
theTransform = CGAffineTransformScale(theTransform, 1, 2);
//moves entire context
CGContextTranslateCTM(context, 100., 100.);
// Release the path when done with it. 
CGPathRelease(path);
```

## Transforming
There are 2 ways to trasnform vector:  
**A.** is to transform the path object it self or  
**B.** Transform the entire context like this:  
1. Save the Quartz coordinate system with CGContextSaveGState.
2. Apply the affine transform to the context using CGContextConcatCTM.
3. Add the untransformed path with CGContextAddPath.
4. Restore the Quartz coordinate system with CGContextRestoreGState.

- **CGContextGetPathBoundingBox**  Returns a CGRect that corresponds to the bounding box of the current path  
- **CGRectNull** If the current path in the context is empty, CGContextGetPathBoundingBox returns the special rectangle CGRectNull
- **CGRectEqualToRect** The Quartz utility function **CGRectEqualToRect** allows comparison of two CGRect structures for equality, making it straightforward to determine if the CGRect returned by CGContextGetPathBoundingBox is CGRectNull.
- **CGContextIsPathEmpty** Returns a Boolean value indicating whether the current path in the context is empty. An empty path contains no current point or path segments.
- **CGContextGetPathCurrentPoint** Returns a CGPoint that corresponds to the current point of the path in the context. The coordinate values of the point are expressed in the user space coordinate system in effect at the time you call CGContextGetPathCurrentPoint. 
- **CGPointZero** If the current path in the context is empty, CGContextGetPathCurrentPoint returns the value CGPointZero.
- **CGContextGetClipBoundingBox** Returns a CGRect that corresponds to the bounding box of the clipping area. The coordinate values of the returned rectangle are expressed in the user space coordinate system in effect at the time you call CGContextGetClipBoundingBox.
- **CGContextReplacePathWithStrokedPath**  Replaces the current path with a stroked version of that path, using the parame- ters of the context to determine the new path. The same context parameters that would be used to stroke a path are consulted when creating the stroked version of the path. The new path, if filled, produces the same result as would have been obtained from stroking the original path. One use of this function is to create a clipping path that corresponds to a stroked version of a path. You’d follow a call to CGContextReplacePathWithStrokedPath with a call to CGContextClip.
1. context, the CGContextRef whose path you want to examine.  
2. point, a CGPoint you want to test with the current path.  
3. mode, the CGPathDrawingMode of the operation you want to hit test. 
- **CGContextPathContainsPoint** allows you to perform simple hit detection with Quartz paths. This function takes the fol- lowing parameters:  
- **CGContextPathContainsPoint** returns true if point is inside the cur- rent path in context. If point is not inside the current path, the function returns false  
- **CGPathContainsPoint** can be used to determine whether a CGPoint is inside a CGPath object.

## Color
- **CGColorCreateCopyWithAlpha**  To make a new CGColor object with the same color space and component color values
- **CGContextSetFillColor** &  **CGContextSetStrokeColorWithColor**  Each of these functions simultaneously sets the corresponding color space and color, using the CGColor object passed to it.

Changing colors after the fact:  
**CGContextSetFillColor**   
**CGContextSetStrokeColor**  
**CGContextSetFillColorSpace** or **CGContextSet**: StrokeColorSpace if the current fill or stroke color space is not already the color space you require.


## Graphics State Parameters  
*Param* - *Default value* - *Description*
- **CTM** - Context dependent - The current transformation matrix. Determines the mapping of coordinates from user space to device space.
- **Clipping area** - Context dependent - The clipping area is the only portion of the drawing canvas into which Quartz renders.
- **Fill color** - DeviceGray opaque black- The color space and the component values, plus an alpha value, used by Quartz when performing fill operations and the painting of image masks. 
- **Stroke color** - DeviceGray opaque black - The color space and the component values, plus an alpha value, used by Quartz when performing stroking operations.
- **Line width** - 1.0 - The thickness, in user space units, that Quartz uses when stroking paths.
- **Line cap** - kCGLineCapButt - The style Quartz uses to paint open endpoints on a subpath when stroking.
- **Line join** - kCGLineJoinMiter - The style Quartz uses to paint the join of connected path segments when stroking.
- **Miter limit** - 10.0 - Determines the angle between connected path segments where Quartz replaces a miter join with a bevel join when str
- **Line dash** - A solid line - The dash pattern Quartz uses when stroking paths.
- **Alpha** - 1.0 - A global alpha value that applies an additional alpha value to all drawing in the context.
- **Rendering intent** - kCGRenderingIntentDefault - The rendering intent applied when painting paths, text, or image masks.
- **Interpolation quality** - kCGInterpolationDefault  - The interpolation quality to apply when rendering sampled images.
- **Should anti-alias** - true, for those contexts that support anti-aliasing -  A Boolean value that determines whether Quartz anti-aliases when rendering.
- **Shadow** - No shadow - Determines the shadow applied when rendering. Panther and later versions only.
- **Pattern phase** - CGSizeZero - The offset to the starting point of a pattern.
- **Should smooth fonts** - true, for those contexts that support font smoothing - A Boolean value that determines whether Quartz should smooth fonts when drawing text. Jaguar and later versions only.
- **Text drawing mode** - kCGTextFill - The painting mode Quartz uses when drawing text.
- **Font** - None - The font Quartz uses when drawing text.
- **Font size** - The point size Quartz uses when drawing text.
- **Character** spacing - An additional spacing that Quartz adds after each text character when drawing text.
- **Blend mode** - Determines how Quartz composites source drawing to the destination. Available in Tiger and later versions only.- 

**Note:** If you obtain a context from a framework or another source, you should not assume the context parameters are set to the values you need. You need to set the graphics state parameters to those appropriate for your drawing task.

**CGContextSaveGState**  
makes a copy of the current graphics state in the context and pushes that copy onto the top of the graphics state stack. The exe- cution of CGContextSaveGState does not modify any of the values in the current graphics state.
**CGContextRestoreGState**  
pops off (or removes) the topmost graphics state on the graphics state stack and it becomes the current graphics state in the context.

**Note:** You save and restore these GStates when you need to change them.  
**Note:** you can save the GState to a variable (code needed) or just roll with it and resore many times until you reach the first GState in the stack. (This process needs a little practice to understand) (remeber the save and restore pushes and pops items on an array)  
**Note:** I think you are better served doing transforming on the path before you add it to the context  
**Note:** I think its better to clear the CGContext and just recreate everything again than messing with saving and restoring GStates. It seems very un-intitive. Unless performance is an issue not using them that is.  

**IMPORTANT:** Saving and restoring the graphics state is a relatively cheap operation. You should use CGContextSaveGState and CGContextRestoreGState to manage the graphics state as needed rather than working hard to avoid their use.  
**IMPORTANT:** Every call you make to CGContextSaveGState must be balanced with a subsequent call to CGContextRestoreGState. Failing to do so produces 

**Functions that modify the GState:**  
- CGContextSetLineWidth  
- CGContextScaleCTM
- CGContextTranslateCTM
- CGContextRotateCTM
- CGContextClip
- CGContextTranslateCTM
- CGContextSetFillColorWithColor
- CGContextFillPath
- CGContextScaleCTM
- CGContextSetLineDash
- CGContextStrokePath

Context clipping example: (code example does exist)  
<img width="370" alt="context clipping example" src="https://dl.dropboxusercontent.com/u/2559476/Screen%20Shot%202015-11-01%20at%2010.55.53.png">

## Text

- **GContextSetFont** sets only the font
- **CGContextSelectFont** sets both the font and the font size
- **CGContextSetFontSize** sets the font size parameter in the graphics state.
- **CGContextSetTextPosition** As a convenience, you can set the text position with the function 
- **CGContextGetTextPosition** you can retrieve the current text position with .
- **CGContextSetTextMatrix** You explicitly set the text matrix in its entirety (including the text position) by using . 
- **CGContextGetTextMatrix** You can obtain the current text matrix in the context  
- **CGContextSetTextDrawingMode** You set the text drawing mode by calling the function
- **CGTextDrawingMode.kCGTextFill**: draws text characters as you expect—it fills the outlines of the paths that make up the glyphs, using the current fill color. (Note that the fill algorithms for painting text characters are significantly different than those for line art graphics. To achieve good results, Quartz uses different render- ing techniques for painting text characters than it does for painting line art.
- **CGTextDrawingMode.kCGTextStroke**: mode strokes the outlines of the characters, using the current line width in the graphics state. The glyphs are painted with the current stroke color. The other parameters of the graphics state that affect stroking, such as the line dash, line cap, and line join, also affect the stroking of text.
- **CGTextDrawingMode.kCGTextFillStroke**: mode both fills and strokes the text. The effect is that of first filling, then stroking the text. Note that if you want to draw shadowed text  that is both stroked and filled, you may need to use transparency layers to achieve the effect you most likely will want. See “Drawing with Transparency Layers (Pan- ther)” (page 522) for a discussion of transparency layers and shadows.
- **CGTextDrawingMode.kCGTextInvisible**: mode performs no painting of the characters, but it updates the text matrix as if it had painted them. This is potentially useful for measuring text before drawing it or positioning pieces of text as if other text were drawn first.
- **CGTextDrawingMode.kCGTextClip** : mode intersects the current clipping area with the area described by the interior of the paths making up the text characters. Note that this is an atomic operation; that is, when making a single Quartz drawing call, the interiors of the glyphs being drawn are treated as a single object when clip- ping. Subsequent calls to text drawing functions with the text drawing mode kCGTextClip most likely will produce an empty clipping area unless the addi- tional text drawing is placed so that it intersects the previous clipping area.
- **CGTextDrawingMode.kCGTextFillClip** mode first paints the text as if the drawing mode is kCGTextFill; then it intersects the out- lines with the current clipping area. The adjustment of the text position is done only after the text is painted and the clip is applied.
- **CGTextDrawingMode.kCGTextStrokeClip** mode first paints the text as if the drawing mode is kCG- TextStroke; then it intersects the outlines with the current clipping area. The adjustment of the text position is done only after the text is painted and the clip is applied.
- **CGTextDrawingMode.kCGTextFillStrokeClip** mode first paints the text as if the drawing mode is kCGTextFillStroke; then it intersects the outlines with the current clipping area. The adjustment of the text position is done only after the text is painted and the clip is applied.

<img width="450" alt="example of text clipping" src="https://www.dropbox.com/s/gxnyhzj9jkxbvmb/Screen%20Shot%202015-11-02%20at%2007.37.08.png?raw=1">

**Important:** The text matrix and, therefore, the text position are not part of the Quartz graphics state and are not affected by CGContextSaveGState and CGContextRestoreGState. Because the text matrix is not part of the graphics state, you cannot use CGContextSaveGState and CGContextRestore- GState to save and restore it. If you do need to save and restore the text matrix, you call the function CGContextGetTextMatrix to obtain the current text matrix and later restore to that matrix by calling CGContextSetTextMatrix.

**Text & Flipped views:**  
One typical use is to draw text upright in a coordinate system that has its y axis oriented with positive y values going down the drawing canvas (a “flipped” coordinate system). Using a text matrix that is scaled by -1 in the y coordinate allows you to flip the text you draw without modifying the CTM prior to text drawing. 

**Note:** To achieve the effect of clipping to all the text characters in a sequence of multiple runs, each containing a different font and/or size, the best approach is to (1) clip to a single run at a time, (2) draw the graphics you want clipped to the multiple run of text, (3) restore to a saved graphics state without the clip, and (4) repeat the process for the next run of characters.
**Note:** Quartz has no notions of synthetic bold, italic, or outline styles as available in some graphics systems such as QuickDraw. Many fonts themselves have the intrinsic styles corresponding to the bold, italic, and bold-italic styles and you should use the intrinsic font faces to obtain these text styles.

**CGContextSelectFont** Before you can use Quartz functions to draw text using characters, you need to call the function CGContextSelectFont to set the font face, font size, and text encoding. The font selected becomes the new font in the graphics state and the size becomes the new font size. This function takes four parameters::  
- **context**, the graphics context in which to set the font.
- **name**, the PostScript name of the font.
- **size**, the size of the font in text space units.
- **textEncoding**, a value of type CGTextEncoding. The possible values are kCG- EncodingMacRoman or kCGEncodingFontSpecific.


**CGContextShowText** draw the glyphs that correspond to the charac- ter codes in the string either at the current text position 
**CGContextShowTextAtPoint** or at the point passed to CGContextShowTextAtPoint. The character advances that these routines use are the natural advances of the glyphs (scaled by the font size and text matrix), plus any extra character spacing for each glyph. The extra character spacing is a text space value added to each glyph width.

AroutinethatdrawsRomantextusingCGContextShowTextandCGContextShowTextAtPoint:

```objc
void drawQuartzRomanText(CGContextRef context) {
  int i;
  static const char *text = "Quartz"; 
  size_t textlen = strlen(text); 
  float fontSize = 60;
  float opaqueBlack[] = { 0., 0., 0., 1. }; 
  float opaqueRed[] = { 0.663, 0., 0.031, 1. };
  // Set the fill color space. This sets the
  // fill painting color to opaque black. 
  CGContextSetFillColorSpace(context, getTheRGBColorSpace());
  // Set the text matrix this code requires. 
  CGContextSetTextMatrix(context, CGAffineTransformIdentity);
  // Choose the font with the PostScript name "Times-Roman", at 
  // fontSize points, with the MacRoman encoding. 
  CGContextSelectFont(context, "Times-Roman", fontSize,kCGEncodingMacRoman);
  // The default text drawing mode is fill. Draw the text at (70, 400). 
  CGContextShowTextAtPoint(context, 70, 400, text, textlen);
  // Set the fill color to red. 
  CGContextSetFillColor(context, opaqueRed);
  // Draw the next piece of text where the previous one left off.
  CGContextShowText(context, text, textlen);
  for(i = 0 ; i < 3 ; i++){
    // Get the current text pen position.
    CGPoint p = CGContextGetTextPosition(context); // Translate to the current text pen position. CGContextTranslateCTM(context, p.x, p.y);
    // Rotate clockwise by 90 degrees for the next
    // piece of text.
    CGContextRotateCTM(context, DEGREES_TO_RADIANS(-90)); // Draw the next piece of text at the origin in black. CGContextSetFillColor(context, opaqueBlack); CGContextShowTextAtPoint(context, 0, 0, text, textlen); // Draw the next piece of text where the previous piece // left off and paint it with red. CGContextSetFillColor(context, opaqueRed); CGContextShowText(context, text, textlen);
  } 
}
```