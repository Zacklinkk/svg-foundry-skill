CHAPTER 6
Transforming the Coordinate System
Up to this point, all graphics have been displayed as is—drawn exactly where and how
they are defined in their attributes. There will be times when you have a graphic you
would like to rotate, scale, or move to a new location. To accomplish these tasks, you
add the transform  attribute to the appropriate SVG elements. This chapter examines
the details of these transformations.
The translate Transformation
In Chapter 5 , you saw that you can use x and y attributes with the <use>  element to
place a group of graphic objects at a specific place. Look at the SVG in Example 6-1 ,
which defines a square and draws it at the upper-left corner of the grid, then redraws it
with the upper-left corner at coordinates (50,50). The dotted lines in Figure 6-1  aren’t
part of the SVG, but serve to show the part of the canvas we’re interested in.
Example 6-1. Moving a graphic with use
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
    <g id="square" >
        <rect x="0" y="0" width="20" height="20"
            style="fill: black; stroke-width: 2;" />
    </g>
    <use xlink:href= "#square"  x="50" y="50"/>
</svg>
As it turns out, the x and y values are really shorthand for one form of the more general
and more powerful transform  attribute. Specifically, the x and y values are equivalent
to an attribute like transform="translate( x-value , y-value)", where translate  is a
fancy technical term for move . The x-value  and y-value  are measured in the current user
coordinate system. Let’s use transform  to get the same effect of making a second square
with its upper-left corner at (50,50). Example 6-2  lists the SVG.
69

Figure 6-1. Result of moving with use
Example 6-2. Moving the coordinate system with translation
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
    <g id="square" >
        <rect x="0" y="0" width="20" height="20"
            style="fill: none; stroke:black; stroke-width: 2;" />
    </g>
    <use xlink:href= "#square"  transform="translate(50,50)" />
</svg>
The resulting display will look exactly like that in Figure 6-1 . Y ou might think this was
accomplished by moving the square to a different place on the grid, as shown concep‐
tually in Figure 6-2 , but you would be wrong.
Figure 6-2. How moving appears to work (but really doesn’t)
What is really going on behind the scenes is an entirely different story. Rather than
moving the square, the translate  specification picks up the entire grid  and moves it to
a new location on the canvas. As far as the square is concerned, it’s still being drawn
with its upper-left corner at (0,0), as depicted in Figure 6-3 .
70 | Chapter 6: Transforming the Coordinate System

Figure 6-3. How moving with translate really works
The online example allows you to experiment with different coordinates:
http://oreillymedia.github.io/svg-essentials-examples/ch06/translate.html
A transformation never  changes a graphic object’s grid coordinates;
rather, it changes the position of the grid on the canvas.
At first glance, using translate  seems as ridiculous and inefficient as moving your
couch further away from the outside wall of the house by moving the entire living room,
walls and all, to a new position. Indeed, if translation were the only transformation
available, moving the entire coordinate system would be wasteful. However, you will
soon see other transformations and combinations of a sequence of transformations that
are more mathematically and conceptually convenient if they apply to the entire coor‐
dinate system.
The scale Transformation
It is possible to make an object appear larger or smaller than the size at which it was
defined by scaling the coordinate system. Such a transformation is specified as follows:
transform="scale( value)"
Multiplies all x- and y-coordinates by the given value .
transform="scale( x-value , y-value)"
Multiplies all x-coordinates by the given x-value  and all y-coordinates by the given
y-value .
Example 6-3  is an example of the first kind of scaling transformation, which uniformly
doubles the scale of both axes. Once again, the dotted lines in Figure 6-4  aren’t in the
SVG; they simply show the area of the canvas we’re interested in.  Note that the square’s
upper-left corner is at (10,10).
The scale Transformation | 71

Example 6-3. Uniformly scaling a graphic
http://oreillymedia.github.io/svg-essentials-examples/ch06/scale.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
    <g id="square" >
        <rect x="10" y="10" width="20" height="20"
            style="fill: none; stroke: black;" />
    </g>
    <use xlink:href= "#square"  transform="scale(2)" />
</svg>
Figure 6-4. Result of using scale transformation
Y ou might be thinking, “Wait a minute—I can understand why the square got larger.
But I didn’t ask for a translate , so why is the square in a different place?” Everything
becomes clear when you look at Figure 6-5  to see what has actually occurred. The grid
hasn’t moved; the (0,0) point of the coordinate system is still in the same place, but each
user coordinate is now twice as large as it used to be. Y ou can see from the grid lines
that the upper-left corner of the rectangle is still at (10,10) on the new, larger grid,
because objects never  move. This also explains why the outline of the larger square is
thicker. The stroke-width  is still one user unit, but that unit has now become twice as
large, so the stroke thickens.
Figure 6-5. How the scale transformation works
72 | Chapter 6: Transforming the Coordinate System

A scaling  transformation  never  changes  a graphic  object’s  grid
coordinates  or its stroke  width;  rather,  it changes  the size of the
coordinate system (grid) with respect to the canvas.
It is possible to specify a different scale factor for the x-axis and y-axis of the coordinate
system by using the second form of the scale  transformation. Example 6-4  draws the
square with the x-axis scaled by a factor of three and the y-axis scaled by a factor of one
and a half. As you can see in Figure 6-6 , the one-unit stroke width is also nonuniformly
scaled.
Example 6-4. Nonuniform scaling of a graphic
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <g id="square" >
    <rect x="10" y="10" width="20" height="20"
      style="fill: none; stroke: black;" />
  </g>
  <use xlink:href= "#square"  transform="scale(3, 1.5)" />
</svg>
Figure 6-6. Result of using nonuniform scale transformation
To this point, the examples have applied the transform  attribute to only the <use>
element. Y ou can apply a transformation to a series of elements by grouping them and
transforming the group:
<g id="group1"  transform= "translate(3, 5)" >
  <line x1="10" y1="10" x1="30" y2="30"/>
  <circle cx="20" cy="20" r="10"/>
</g>
Y ou may also apply a transformation to a single object or basic shape. For example, here
is a rectangle whose coordinate system is scaled by a factor of three:
<rect x="15" y="20" width="10" height="5"
  transform= "scale(3)"
  style="fill: none; stroke: black;" />
It’s fairly clear that the width and height of the scaled rectangle should be three times as
large as the unscaled rectangle. However, you may wonder if the x- and y-coordinates
are evaluated before or after the rectangle is scaled. The answer is that SVG applies
transformations to the coordinate system before it evaluates any of the shape’s
The scale Transformation | 73

coordinates. Example 6-5  is the SVG for the scaled rectangle, shown in Figure 6-7  with
grid lines that are drawn in the unscaled coordinate system.
Example 6-5. Transforming a single graphic
<!-- grid guide lines in non-scaled coordinate system -->
<line x1="0" y1="0" x2="100" y2="0" style="stroke: black;" />
<line x1="0" y1="0" x2="0" y2="100" style="stroke: black;" />
<line x1="45" y1="0" x2="45" y2="100" style="stroke: gray;" />
<line x1="0" y1="60" x2="100" y2="60" style="stroke: gray;" />
<!-- rectangle to be transformed -->
<rect x="15" y="20" width="10" height="5"
    transform= "scale(3)"
    style="fill: none; stroke: black;" />
Figure 6-7. Result of transforming a single graphic
The effect  of applying  a transformation  to a shape  is the same  as if the
shape were enclosed in a transformed group. In the preceding exam‐
ple, the scaled rectangle is equivalent to this SVG:
<g transform= "scale(3)" >
   <rect x="15" y="20" width="10" height="5"
     style="fill: none; stroke: black;" />
</g>
Sequences of Transformations
It is possible to do more than one transformation on a graphic object. Y ou just put the
transformations, optionally separated by whitespace or a comma, in the value of the
transform  attribute. Here is a rectangle that undergoes two transformations, a trans‐
lation followed by a scaling. (The axes are drawn to show that the rectangle has, indeed,
moved.)
<!-- draw axes -->
<line x1="0" y1="0" x2="0" y2="100" style="stroke: gray;" />
<line x1="0" y1="0" x2="100" y2="0" style="stroke: gray;" />
<rect x="10" y="10" height="15" width="20"
  transform= "translate(30, 20) scale(2)"
  style="fill: gray;" />
74 | Chapter 6: Transforming the Coordinate System

This is the equivalent of the following sequence of nested groups, and both will produce
what you see in Figure 6-8 :
<g transform= "translate(30, 20)" >
  <g transform= "scale(2)" >
  <rect x="10" y="10" height="15" width="20"
    style="fill: gray;" />
  </g>
</g>
Figure 6-8. Result of translate followed by scale
Figure 6-9  shows what is happening at each stage of the transformation.
Figure 6-9. How translate followed by scale works
The order in which you do a sequence of transformations affects the
result.  In general,  transformation  A followed  by transformation  B will
not give the same  result  as transformation  B followed  by transforma‐
tion A.
Example 6-6  draws the same rectangle as in the previous example, in gray. Then it draws
the rectangle again in black, but does the scale  before the translate . As you can see
from the result in Figure 6-10 , the rectangles end up in very different places on the
canvas.
Example 6-6. Sequence of transformations—scale followed by translate
<!-- draw axes -->
<line x1="0" y1="0" x2="0" y2="100" style="stroke: gray;" />
<line x1="0" y1="0" x2="100" y2="0" style="stroke: gray;" />
Sequences of Transformations | 75

<rect x="10" y="10" width="20" height="15"
    transform= "translate(30, 20) scale(2)"  style="fill: gray;" />
<rect x="10" y="10" width="20" height="15"
    transform="scale(2) translate(30, 20)"
    style="fill: black;"/>
Figure 6-10. Result of scale followed by translate
The reason the black rectangle ends up farther away from the origin is that the scaling
is applied first, so the translate of 20 units in the x-direction and 10 units in the y-
direction is done with units that are now twice as large, as shown in Figure 6-11 .
Figure 6-11. How scale followed by translate works
In the online example, you can experiment with any sequence of transformations, and
compare the transformed rectangle against the original:
http://oreillymedia.github.io/svg-essentials-examples/ch06/sequence.html
Technique: Converting from Cartesian Coordinates
If you are transferring data from other systems to SVG, you may have to deal with vector
drawings that use Cartesian coordinates (the ones you learned about in high school
algebra) to represent data. In this system, the (0,0) point is at the lower left of the canvas,
and y-coordinates increase as you move upward. Figure 6-12  shows the coordinates of
a trapezoid drawn with Cartesian coordinates.
76 | Chapter 6: Transforming the Coordinate System

Figure 6-12. Trapezoid drawn with Cartesian coordinates
The y-axis is “upside-down” relative to the SVG default, so the coordinates need to be
recalculated. Rather than do it by hand, you can use a sequence of transformations to
have SVG do all the work for you. First, translate the picture into SVG, with the coor‐
dinates exactly as shown in Example 6-7 . (The example also includes the axes as a guide.)
To nobody’s surprise, the picture will come out upside-down. Note that the image in
Figure 6-13  is not left-to-right reversed, because the x-axis points in the same direction
in both Cartesian coordinates and the default SVG coordinate system.
Example 6-7. Direct use of Cartesian coordinates
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <!-- axes -->
  <line x1="0" y1="0" x2="100" y2="0" style="stroke: black;" />
  <line x1="0" y1="0" x2="0" y2="100" style="stroke: black;" />
  <!-- trapezoid -->
  <polygon  points="40 40, 100 40, 70 70, 40 70"
      style="fill: gray; stroke: black;" />
</svg>
Figure 6-13. Result of using original Cartesian coordinates
To flip the image back right-side-up, you can take advantage of the fact that scaling a
shape by a negative value reverses the order of coordinates. However, because the entire
grid ends up flipped to the other side of the 0 coordinate, you also need to translate the
shape back onto the visible part of the canvas. The conversion follows these steps:
1.Find the maximum y-coordinate in the original drawing. In this case, it turns out
to be 100, the endpoint of the y-axis in the original.
2.Enclose the entire drawing in a <g> element.
Technique: Converting from Cartesian Coordinates | 77

3.Enter a translate that moves the coordinate system downward by  the maximum y
value: transform="translate(0, max-y)".
4.The next transform will be to scale the y-axis by a factor of –1, flipping it upside-
down: transform="translate(0, max-y) scale(1, -1)" .
Y ou don’t  want  to change  the x-axis  values,  but you still need  to specify
an x-value  for both  the translate  and scale  functions.  The do-
nothing value for the translate is 0, but the do-nothing value for the
scale  transform  is 1, because  coordinates  are multiplied  by the scale
factor.  A scale(0)  transform  would  collapse  your  shape  to a single
point  (because  every  coordinate,  multiplied  by zero,  would
become zero).
Example 6-8  incorporates this transformation, producing a right-side-up trapezoid in
Figure 6-14 .
Example 6-8. Transformed Cartesian coordinates
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <g transform= "translate(0,100) scale(1,-1)" >
    <!-- axes -->
    <line x1="0" y1="0" x2="100" y2="0" style="stroke: black;" />
    <line x1="0" y1="0" x2="0" y2="100" style="stroke: black;" />
    <!-- trapezoid -->
    <polygon  points="40 40, 100 40, 70 70, 40 70"
        style="fill: gray; stroke: black;" />
  </g>
</svg>
Figure 6-14. Transformed Cartesian coordinates
The rotate Transformation
It is also possible to rotate the coordinate system by a specified angle. In the default
coordinate system, angle measure increases as you rotate clockwise, with a horizontal
line having an angle of 0 degrees, as shown in Figure 6-15 .
78 | Chapter 6: Transforming the Coordinate System

1.All the figures in this chapter are static pictures. This one shows two squares (one rotated and one unrotated).
To show an animation of a rotating square, use <animateTransform> , which we will discuss in Chapter 12 ,
in “The <animateTransform> Element” on page 200 .
Figure 6-15. Default measurement of angles
Unless you specify otherwise, the center of rotation  (a fancy term for the pivot point ) is
presumed to be (0,0). Example 6-9  shows a square drawn in gray, then drawn again in
black after the coordinate system is rotated 45 degrees. The axes are also shown as a
guide. Figure 6-16  shows the result. If you’re surprised that the square has appeared to
move, you shouldn’t be. Remember, as shown in Figure 6-17 , the entire coordinate
system has been rotated.1
Example 6-9. Rotation around the origin
http://oreillymedia.github.io/svg-essentials-examples/ch06/rotate.html
<!-- axes -->
<polyline  points="100 0, 0 0, 0 100"  style="stroke: black; fill: none;" />
<!-- normal and rotated square -->
<rect x="70" y="30" width="20" height="20" style="fill: gray;" />
<rect x="70" y="30" width="20" height="20"
    transform= "rotate(45)"  style="fill: black;" />
Figure 6-16. Result of rotation around the origin
The rotate Transformation | 79

Figure 6-17. How rotation around the origin works
Most of the time, you will not want to rotate the entire coordinate system around the
origin; you’ll want to rotate a single object around a point other than the origin. Y ou
can do that via this series of transformations: translate( centerX , centerY)
rotate(angle) translate(- centerX , -centerY). SVG provides another version of
rotate  to make this common task easier. In this second form of the rotate  transfor‐
mation, you specify the angle and the center point around which you want to rotate:
rotate(angle, centerX, centerY)
This has the effect of temporarily establishing a new system of coordinates with the
origin at the specified center x and y points, doing the rotation, and then re-establishing
the original coordinates. Example 6-10  shows this form of rotate  to create multiple
copies of an arrow, shown in Figure 6-18 .
Example 6-10. Rotation around a center point
http://oreillymedia.github.io/svg-essentials-examples/ch06/rotate-center.html
<!-- center of rotation -->
<circle cx="50" cy="50" r="3" style="fill: black;" />
<!-- non-rotated arrow -->
<g id="arrow" style="stroke: black;" >
    <line x1="60" y1="50" x2="90" y2="50"/>
    <polygon  points="90 50, 85 45, 85 55" />
</g>
<!-- rotated around center point -->
<use xlink:href= "#arrow"  transform= "rotate(60, 50, 50)" />
<use xlink:href= "#arrow"  transform= "rotate(-90, 50, 50)" />
<use xlink:href= "#arrow"  transform= "rotate(-150, 50 50)" />
Figure 6-18. Result of rotation around a center point
80 | Chapter 6: Transforming the Coordinate System
www.allitebooks.com


2.This is also a static picture, a “square bull’s-eye. ” If you want to show an animation of an expanding square,
you’ll use <animateTransform> , which we will discuss in Chapter 12 , in “The <animateTransform> Ele‐
ment” on page 200 .Technique: Scaling Around a Center Point
While it’s possible to rotate around a point other than the origin, there is no corre‐
sponding capability to scale  around a point. Y ou can, however, make concentric sym‐
bols with a simple series of transformations. To scale an object by a given factor around
a center point, do this:
translate(- centerX*(factor-1) , -centerY*(factor-1) )
scale(factor)
Y ou may also want to divide the stroke-width  by the scaling factor so  the outline stays
the same width while the object becomes larger. Example 6-11  draws the set of concentric
rectangles shown in Figure 6-19 .2
Example 6-11. Scaling around a center point
<!-- center of scaling -->
<circle cx="50" cy="50" r="2" style="fill: black;" />
<!-- non-scaled rectangle -->
<g id="box" style="stroke: black; fill: none;" >
    <rect x="35" y="40" width="30" height="20"/>
</g>
<use xlink:href= "#box" transform= "translate(-50,-50) scale(2)"
    style="stroke-width: 0.5;" />
<use xlink:href= "#box" transform= "translate(-75,-75) scale(2.5)"
    style="stroke-width: 0.4;" />
<use xlink:href= "#box" transform= "translate(-100,-100) scale(3)"
    style="stroke-width: 0.33;" />
Figure 6-19. Result of scaling around a center point
The skewX and skewY Transformations
SVG also has two other transformations: skewX  and skewY , which let you skew one of
the axes. The general form is skewX(angle) and skewY(angle). The skewX  transfor‐
mation “pushes” all x-coordinates by the specified angle, leaving y-coordinates
Technique: Scaling Around a Center Point | 81

unchanged . skewY  skews the y-coordinates, leaving x-coordinates unchanged, as shown
in Figure 6-20 , which is drawn with the code in Example 6-12 .
Example 6-12. skewX and skewY
http://oreillymedia.github.io/svg-essentials-examples/ch06/skew.html
<!-- guide lines -->  
<g style="stroke: gray; stroke-dasharray: 4 4;" >
    <line x1="0" y1="0" x2="200" y2="0"/>
    <line x1="20" y1="0" x2="20" y2="90"/>
    <line x1="120" y1="0" x2="120" y2="90"/>
</g>
<g transform= "translate(20, 0)" >     
    <g transform= "skewX(30)" >     
        <polyline  points="50 0, 0 0, 0 50"       
             style="fill: none; stroke: black; stroke-width: 2;"/>
        <text x="0" y="60">skewX</text>     
    </g>
</g>
<g transform= "translate(120, 0)" >     
    <g transform= "skewY(30)" >
        <polyline  points="50 0, 0 0, 0 50"
            style="fill: none; stroke: black; stroke-width: 2;" />
        <text x="0" y="60">skewY</text>
    </g>
</g>
These  dashed  lines  are drawn  in the default  coordinate  system,  before  any
transformation has occurred.
This will move the entire skewed “package” to the desired location.
Skew  the x-coordinates  30 degrees.  This  transformation  doesn’t  change  the
origin, which will still be at (0,0) in the new coordinate system.
To make things easier, we draw the object at the origin.
Text will be covered in more detail in Chapter 9 .
These  elements  are organized  exactly  like the preceding  ones,  except  the y-
coordinates are skewed.
Figure 6-20. Result of skewX and skewY transformations
82 | Chapter 6: Transforming the Coordinate System

Notice that skewX  leaves the horizontal lines horizontal, and skewY  leaves the vertical
lines untouched. Go figure.
Transformation Reference Summary
Table 6-1  gives a quick summary of the transformations available in SVG.
Table 6-1. SVG transformations
Transformation Description
translate( x, y) Moves the user coordinate system by the specified x and y amounts.
Note: If you don’t specify a y value, 0 is assumed.
scale(xFactor , yFactor) Multiplies all user coordinates by the specified xFactor  and
yFactor . The factors may be fractional or negative.
scale(factor) Same as scale(factor , factor).
rotate(angle) Rotates the user coordinate system by the specified angle . The center
of rotation is the origin (0,0). In the default coordinate system, angle
measure increases as you rotate clockwise, with a horizontal line having
an angle of 0 degrees.
rotate(angle , centerX , centerY) Rotates the user coordinate system by the specified angle . The center
of rotation is specified by centerX  and centerY .
skewX(angle) Skews all x-coordinates by the specified angle . Visually, this makes
vertical lines appear at an angle.
skewY(angle) Skews all y-coordinates by the specified angle . Visually, this makes
horizontal lines appear at an angle.
matrix(a b c d e f) Specifies a transformation in the form of a transformation matrix of six
values. See Appendix D .
CSS Transformations and SVG
As of this writing, CSS also has a working draft of a transformations module . Because
it is a working draft, details may change and browser support may vary. If you are already
using CSS transforms, there are some important differences from SVG:
•SVG 1.1 transforms are in user units or implicit degrees. CSS transforms use CSS
length and angle units, although the specification would also allow implicit user
units when applied to SVG elements.
•SVG 1.1 transforms are structural attributes, whereas CSS transforms can be speci‐
fied in stylesheets. Stylesheet specifications override attribute values.
•In CSS, you cannot have space between the transform type and the opening pa‐
renthesis, and you must use commas to separate numerical values.
Transformation Reference Summary | 83

•CSS transforms include a separate property to specify the origin for rotation and
scaling. In SVG, the rotation origin is part of the rotate()  function, and you cannot
specify an origin for scaling.
•CSS transforms include 3D effects.
84 | Chapter 6: Transforming the Coordinate System