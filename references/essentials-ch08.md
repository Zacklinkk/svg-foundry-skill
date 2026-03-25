CHAPTER 7
Paths
All of the basic shapes described in Chapter 4  are really shorthand forms for the more
general <path>  element.  Y ou are well advised to use these shortcuts; they help make
your SVG more readable and more structured. The <path>  element is more general; it
draws the outline of any arbitrary shape by specifying a series of connected lines, arcs,
and curves. This outline can be filled and drawn with a stroke, just as the basic shapes
are. Additionally, these paths (as well as the shorthand basic shapes) may be used to
define the outline of a clipping area or a transparency mask, as you will see in Chapter 10 .
All of the data describing an outline is in the <path>  element’s d attribute (the d stands
for data ). The path data consists of one-letter commands, such as M for moveto  or L for
lineto , followed by the coordinate information for that particular command.
moveto, lineto, and closepath
Every path must begin with a moveto  command.
The command letter is a capital M followed  by an x- and y-coordinate, separated by
commas or whitespace. This command sets the current location of the “pen” that’s
drawing the outline.
This is followed by one or more lineto  commands, denoted by a capital L, also followed
by x- and y- coordinates, and separated by commas or whitespace. Example 7-1  has
three paths. The first draws a single line, the second draws a right angle, and the third
draws two 30-degree angles. When you “pick up” the pen with another moveto , you are
starting a new subpath. Notice that you can use either a comma or whitespace to separate
the x- and y-coordinates, as shown in all three paths. The result is Figure 7-1 .
85

Example 7-1. Using moveto and lineto
http://oreillymedia.github.io/svg-essentials-examples/ch07/moveto-lineto.html
<svg width="150px" height="150px" viewBox= "0 0 150 150"
  xmlns="http://www.w3.org/2000/svg" >
<g style="stroke: black; fill: none;" >
  <!-- single line -->
  <path d="M 10 10 L 100 10" />
  <!-- a right angle -->
  <path d="M 10, 20  L 100, 20  L  100,50" />
  <!-- two 30-degree angles -->
  <path d="M 40 60 L 10, 60 L 40 42.68
    M 60, 60 L 90 60 L 60, 42.68" />
</g>
</svg>
Figure 7-1. Result of using moveto and lineto
Examining the last path more closely, with commas replaced by whitespace:
Value Action
M 40 60 Move pen to (40,60)
L 10 60 Draw a line to (10,60)
L 40 42.68 Draw a line to (40,42.68)
M 60 60 Start a new subpath; move pen to (60,60)—no line is drawn
L 90 60 Draw a line to (90,60)
L 60 42.68 Draw a line to (60,42.68)
Y ou may have noticed the path data doesn’t look very much like the
typical  values  for XML  attributes.  Because  the entire  path  data  is
contained in one attribute rather than an individual element for each
point or line segment, a path takes up less memory when read into a
Document Object Model structure by an XML parser. Additionally, a
path’s  compact  notation  allows  a complex  graphic  to be transmitted
without requiring a great deal of bandwidth.
If you want to use a <path>  to draw a rectangle, you can draw all four lines, or you can
draw the first three lines and then use the closepath  command, denoted by a capital Z,
to draw a straight line back to the beginning point of the current subpath. Example 7-2
86 | Chapter 7: Paths

is the SVG for Figure 7-2 , which shows a rectangle drawn the hard way, a rectangle
drawn with closepath , and a path that draws two triangles by opening and closing two
subpaths.
Example 7-2. Using closepath
<g style="stroke: black; fill: none;" >
  <!-- rectangle; all four lines -->
  <path d="M 10, 10 L 40, 10 L 40, 30 L 10, 30 L 10, 10" />
  <!-- rectangle with closepath -->
  <path d="M 60 10 L 90 10 L 90 30 L 60 30 Z" />
  <!-- two 30-degree triangles -->
  <path d="M 40 60 L 10 60 L 40 42.68 Z
     M 60 60 L 90 60 L 60 42.68 Z" />
</g>
Examining the last path more closely:
Value Action
M 40 60 Move pen to (40,60)
L 10 60 Draw a line to (10,60)
L 40 42.68 Draw a line to (40,42.68)
Z Close path by drawing a straight line to (40,60), where this subpath began
M 60 60 Start a new subpath; move pen to (60,60)—no line is drawn
L 90 60 Draw a line to (90,60)
L 60 42.68 Draw a line to (60,42.68)
Z Close path by drawing a straight line to (60,60), where this subpath began
Figure 7-2. Result of using closepath
There is another difference between drawing the rectangle with all four lines and using
a closepath  command. When you close the path, the start and end lines are joined to‐
gether to form a continuous shape for stroking styles. The difference is noticeable if you
are using wide strokes or stroke-linecap  and stroke-linejoin  effects. Example 7-3
uses a larger stroke width, and Figure 7-3  shows the result, magnified to make the
difference more clearly visible.
moveto, lineto, and closepath | 87

Example 7-3. Individual lines versus closepath
<g style="stroke: gray; stroke-width: 8; fill: none;" >
    <!-- rectangle; all four lines -->
    <path d="M 10 10 L 40 10 L 40 30 L 10 30 L 10 10" />
    <!-- rectangle with closepath -->
    <path d="M 60 10 L 90 10 L 90 30 L 60 30 Z" />
</g>
Figure 7-3. Result of individual strokes versus closepath
Relative moveto and lineto
The preceding commands are all represented by uppercase letters, and the coordinates
are presumed to be absolute  coordinates.  If you use a lowercase command letter, the
coordinates are interpreted as being relative  to the current pen position. Thus, the fol‐
lowing two paths are equivalent:
<path d="M 10 10 L 20 10 L 20 30  M 40 40 L 55 35"
    style="stroke: black;" />
<path d="M 10 10 l 10  0 l  0 20  m 20 10 l 15 -5"
    style="stroke: black;" />
If you start a path with a lowercase m (moveto ), its coordinates will be interpreted as an
absolute position, as there’s no previous pen position from which to calculate a relative
position. All the other commands in this chapter also have the same upper- and
lower case distinction. An uppercase command’s coordinates are absolute, and a low‐
ercase command’s coordinates are relative. The closepath  command, which has no co‐
ordinates, has the same effect in both upper- and lowercase.
Path Shortcuts
If content is king and design is queen, then bandwidth efficiency is the royal courtier
who keeps the palace running smoothly. Because any nontrivial drawing will have paths
with many tens of coordinate pairs, the <path>  element has shortcuts that allow you to
represent a path in as few bytes as possible.
The Horizontal lineto and Vertical lineto Commands
Horizontal and vertical lines are common enough to warrant shortcut commands. A
path may specify a horizontal line with an H command followed by an absolute
88 | Chapter 7: Paths

1.Y ou can also put multiple single coordinates after a horizontal lineto  or vertical lineto , although you’ll only
notice an effect if you’re using line markers, which we haven’t discussed yet. H 25 35 45  is the same as
H 45 , and v 11 13 15  is the same as v 39 .x-coordinate , or an h command followed by a relative x-coordinate. Similarly, a vertical
line is specified with a V command followed by an absolute y-coordinate, or a v com‐
mand followed by a relative y-coordinate.
The following table compares the short and long way to draw horizontal and vertical
lines:
Shortcut Equivalent to Effect
H 20 L 20 current_y Draws a line to absolute location (20, current_y )
h 20 l 20 0 Draws a line to ( current_x  + 20,current_y )
V 20 L current_x  20 Draws a line to absolute location ( current_x ,20)
v 20 l 0 20 Draws a line to location ( current_x , current_y  + 20)
Thus, the following path draws a rectangle 15 units in width and 25 units in height, with
the upper-left corner at coordinates (12,24).
<path d="M 12 24 h 15 v 25 h -15 z" />
Notational Shortcuts for a Path
Paths can also be made shorter by applying the following two rules:
•Y ou may place multiple sets of coordinates after an L or l, just as you do in the
<polyline>  element. The following six paths all draw the same diamond shown in
Figure 7-4 ; the first three are in absolute coordinates and the last three in relative
coordinates. The third and sixth paths have an interesting twist—if you place mul‐
tiple pairs of coordinates after a moveto , all the pairs after the first are presumed to
be preceded by a lineto : 1
<g style="fill:none; stroke: black" >
  <path d="M 30 30 L 55 5 L 80 30 L 55 55 Z" />
  <path d="M 30 30 L 55 5 80 30 55 55 Z" />
  <path d="M 30 30 55 5 80 30 55 55 Z" />
  <path d="m 30 30 l 25 -25 l 25 25 l -25 25 z" />
  <path d="m 30 30 l 25 -25 25 25 -25 25 z" />
  <path d="m 30 30 25 -25 25 25 -25 25 z" />
</g>
Path Shortcuts | 89

Figure 7-4. Result of drawing a diamond with a path
•Any unnecessary whitespace may be eliminated. Y ou don’t need a blank after a
command letter because all commands are one letter only. Y ou don’t need a blank
between a number and a command because the command letter can’t be part of the
number. Y ou don’t need a blank between a positive and a negative number because
the leading minus sign of the negative number can’t be a part of the positive number.
This lets you reduce the third and sixth paths in the preceding listing even further:
<path d="M30 30 55 5 80 30 55 55Z" />
<path d="m30 30 25-25 25 25-25 25z" />
Another example of the whitespace elimination rule in action is shown by the ex‐
ample that drew a rectangle 15 units in width and 25 units in height, with the upper-
left corner at coordinates (12,24):
<path d="M 12 24 h 15 v 25 h -15 z" /> <!-- original -->
<path d="M12 24h15v25h-15z" /> <!-- shorter -->
Elliptical Arc
Lines are simple; two points on a path uniquely determine the line segment between
them. Because an infinite number of curves can be drawn between two points, you must
give additional information to draw a curved path between them.  The simplest of the
curves we will examine is the elliptical arc—that is, drawing a section of an ellipse that
connects two points.
Although arcs are visually the simplest curves, specifying a unique arc requires the most
information. The first pieces of information you need to specify are the x- and y-radii
of the ellipse on which the points lie. This narrows it down to two possible ellipses, as
you can see in section (a) of Figure 7-5 . The two points divide the two ellipses into four
arcs. Two of them, (b) and (c), are arcs that measure less than 180 degrees. The other
two, (d) and (e), are greater than 180 degrees. If you look at (b) and (c), you will notice
they are differentiated by their direction; (b) is drawn in the direction of increasing
negative (counterclockwise) angle, and (c) in the direction of increasing positive (clock‐
wise) angle. The same relationship holds true between (d) and (e).
But wait—that still doesn’t uniquely specify the potential arcs! There’s no law that says
the ellipse has to have its x-radius parallel to the x-axis. Part (f) of Figure 7-5  shows the
two points with their candidate ellipses rotated 30 degrees with respect to the x-axis.
90 | Chapter 7: Paths

Figure 7-5. Variations of the elliptical arc command
(Figure 7-5  is adapted from the one found in section 8.3.8 of the World Wide Web
Consortium’s SVG specification.)
Thus, an arc command begins with the A abbreviation for absolute coordinates or a for
relative coordinates, and is followed by seven parameters:
•The x- and y-radius of the ellipse on which the points lie.
•The x-axis-rotation  of the ellipse.
•The large-arc-flag , which is 0 if the arc’s measure is less than 180 degrees, or 1
if the arc’s measure is greater than or equal to 180 degrees.
•The sweep-flag , which is 0 if the arc is to be drawn in the negative angle direction,
or 1 if the arc is to be drawn in the positive angle direction.
•The ending x- and y- coordinates of the ending point. (The starting point is deter‐
mined by the last point drawn or the last moveto  command.)
Here are the paths used to draw the elliptical arcs in sections (b) through (e) of Figure 7-5 :
<path d="M 125,75 A100,50 0 0,0 225,125" /> <!-- b -->
<path d="M 125,75 A100,50 0 0,1 225,125" /> <!-- c -->
<path d="M 125,75 A100,50 0 1,0 225,125" /> <!-- d -->
<path d="M 125,75 A100,50 0 1,1 225,125" /> <!-- e -->
Online, you can experiment with all the arc parameters to see what they do:
http://oreillymedia.github.io/svg-essentials-examples/ch07/arc.html
As a further example, let’s enhance the background we started in Example 5-8  to com‐
plete the yin-yang symbol that is part of the South Korean flag. Example 7-4  keeps the
Elliptical Arc | 91

full ellipses as <ellipse>  elements, but creates the semicircles it needs with paths. The
result is shown in Figure 7-6 .
Example 7-4. Using elliptical arc
<svg width="400px" height="300px" viewBox= "0 0 400 300"
  xmlns="http://www.w3.org/2000/svg" >
  <!-- gray drop shadow -->
  <ellipse  cx="154" cy="154" rx="150" ry="120" style="fill: #999999;" />
  <!-- light blue ellipse -->
  <ellipse  cx="152" cy="152" rx="150" ry="120" style="fill: #cceeff;" />
  <!-- large light red semicircle fills upper half,
      followed by small light red semicircle that dips into
      lower-left half of symbol -->
  <path d="M 302 152 A 150 120, 0, 1, 0, 2 152
      A 75 60, 0, 1, 0, 152 152"  style="fill: #ffcccc;" />
  <!-- light blue semicircle rises into upper-right half of symbol -->
  <path d="M 152 152 A 75 60, 0, 1, 1, 302 152"  style="fill: #cceeff;" />
</svg>
Figure 7-6. Result of using elliptical arc
Y ou cannot  draw  a full ellipse  with  a single  path  command;  if the
starting  and ending  points  of the arc are the same,  there  are infin‐
ite ways  to position  the ellipse.  SVG  viewers  will skip such  an arc
command.  If you specify  an ellipse  that is too small  to reach  be‐
tween the starting and ending points, the SVG viewer will scale up
the ellipse until it is just big enough.
For exact  details  on how  out-of-range  parameters  are handled,  see
the specification’s arc implementation notes .
92 | Chapter 7: Paths

Converting from Other Arc Formats
Y ou may be wondering why you can’t specify an arc  by defining a center point for the
ellipse, its x- and y-radius, the starting angle, and the extent of the angle’s arc, as some
other vector graphics systems do. This is a straightforward method of specification, and
is excellent for drawing arcs as single objects. This, paradoxically, is exactly why SVG
instead chooses such a seemingly eccentric method to specify arcs. In SVG, an arc is not
presumed to be living in lonely splendor; it is intended to be part of a connected path
of lines and curves. (For example, a rounded rectangle is precisely that—a series of lines
and elliptical arcs.) Thus, it makes sense to specify an arc by its endpoints.
Sometimes, though, you do want an isolated semicircle (or, more accurately, semi-
ellipse). Presume you have an ellipse specified as follows:
<ellipse cx=" cx" cy="cy" rx="rx" ry="ry"/>
Here are the paths to draw the four possible semi-ellipses (items in parentheses are
intended as algebraic expressions to be calculated):
<!-- northern hemisphere -->
<path d="M (cx - rx)  cy
  A rx ry 0 1 1 (cx + rx)  cy"/>
<!-- southern hemipshere -->
<path d="M (cx - rx)  cy
  A rx ry 0 1 0 (cx + rx)  cy"/>
<!-- eastern hemisphere -->
<path d="M cx (cy - ry)
  A rx ry 0 1 1 cx (cy + ry) "/>
<!-- western hemisphere -->
<path d="M cx (cy - ry)
  A rx ry 0 1 0 cx (cy + ry) "/>
Sometimes you may want to draw an arbitrary arc that has been specified in center-and-
angles  notation and wish to convert it to SVG’s endpoint-and-sweep  format. In other
cases, you may want to convert an arc from the SVG format to a center-and-angles
format. The mathematics for this second case is rather complex, and is detailed in the
SVG specification. Y ou can see a JavaScript version of these conversions in Appendix F .
Bézier Curves
Arcs can be characterized as clean and functional, but one would rarely use the word
graceful  to describe them. If you want graceful, you need to use curves that are produced
by graphing quadratic and cubic equations. Mathematicians have known about these
curves for literally hundreds of years, but drawing them was always a computationally
demanding task. This changed when Pierre Bézier, an engineer who worked for French
car manufacturer Rénault, and Paul de Casteljau, a physicist and mathematician who
Converting from Other Arc Formats | 93

worked for Citroën, developed and promoted a computationally convenient way to
generate these curves.
If you have used graphics programs like Adobe Illustrator, you draw these Bézier curves
by specifying two points and then moving a “handle” as shown in the following diagram.
The end of this handle is called the control point , because it controls the shape of the
curve. As you move the handle, the curve changes in a way that, to the uninitiated, is
completely mystifying. Mike Woodburn, a graphic designer at Key Point Software, sug‐
gests Figure 7-7  as a way to visualize how the control point and the curve interact:
imagine the line is made of flexible metal. Inside the control point is a magnet; the closer
a point is to the control point, the more strongly it is attracted.
Figure 7-7. How graphics programs draw Bézier curves
Another way to visualize the role of the control point is based on the de Casteljau method
of constructing the curves. We will use this approach in the following sections. See
further details on the underlying mathematics , presented in a remarkably lucid fashion.
Quadratic Bézier Curves
The simplest of the Bézier curves is the quadratic curve.  Y ou specify a beginning point,
an ending point, and a control point. Imagine two tent poles placed at the endpoints of
the line. These tent poles meet at the control point. Stretched between the centers of the
tent poles is a rubber band. The place where the curve bends is tied to the exact center
of that rubber band. This situation is shown in Figure 7-8 .
Figure 7-8. Visualizing a quadratic Bézier curve
The lines between the start and endpoints and the control point are tangent to the start
and end of the curve. The curve starts by following the line to the control point, but
then bends over to reach the midpoint heading in the same direction as the “tent pole”
line. The curve ends by sliding up alongside the line from the control point to the
endpoint. Programs like Adobe Illustrator show you only one of the “tent poles. ” The
94 | Chapter 7: Paths

next time you’re using such a program, mentally add in the second pole and the resulting
curves will be far less mysterious.
That’s the concept; now for the practical matter of actually producing such a curve in
SVG. Y ou specify a quadratic curve in a <path>  data with the Q or q command.  The
command is followed by two sets of coordinates that specify a control point and an
endpoint. The uppercase command implies absolute coordinates; lowercase implies
relative coordinates. The curve in Figure 7-8  was drawn from (30,75) to (300,120) with
the control point at (240,30), and was specified in SVG as follows:
<path d="M30 75 Q240 30, 300 120" style="stroke: black; fill: none;"/>
The online example shows it with and without the “tent poles”:
http://oreillymedia.github.io/svg-essentials-examples/ch07/quadratic-bezier.html
Y ou may specify several sets of coordinates after a quadratic curve command. This will
generate a poly-Bézier curve. Presume you want a <path>  that draws a curve from
(30,100) to (100,100) with a control point at (80,30) and then continues with a curve to
(200,80) with a control point at (130,65). Here is the SVG for this path, with control
point coordinates in bold. The result is shown in the left half of Figure 7-9 ; the control
points and lines are shown in the right half of the figure:
<path d="M30 100 Q 80 30, 100 100, 130 65, 200 80"/>
Figure 7-9. Quadratic poly-Bézier curve
Y ou are probably wondering, “What happened to graceful ? That curve is just lumpy. ”
This is an accurate assessment. Just because curves are connected doesn’t mean they will
look good together. That’s why SVG provides the smooth quadratic curve  command,
which is denoted by the letter T (or t if you want to use relative coordinates). The
command is followed by the next endpoint of the curve; the control point is calculated
automatically, as the specification says, by “reflection of the control point on the previous
command relative to the current point. ”
Bézier Curves | 95

For the mathematically inclined, the new control point x2, y2 is cal‐
culated  from  the previous  segment’s  endpoint  x, y and the previous
control point x1, y1 with these formulas:
x2 = x + (x - x1) = 2 * x - x1
y2 = y + (y - y1) = 2 * y - y1
Here is a quadratic Bézier curve drawn from (30,100) to (100,100) with a control point
at (80,30) and then smoothly continued to (200,80). The left half of Figure 7-10  shows
the curve; the right half shows the control points. The reflected control point is shown
with a dashed line. Gracefulness has returned!
<path d="M30 100 Q 80 30, 100 100 T 200 80"/>
Figure 7-10. Smooth quadratic poly-Bézier curve
The online example allows you to experiment with the quadratic poly-Bézier curve:
http://oreillymedia.github.io/svg-essentials-examples/ch07/smooth-quadratic-
bezier.html
Cubic Bézier Curves
A single quadratic Bézier curve has exactly one peak or valley per curve segment. While
these curves are more versatile than simple arcs, you can do even better by using cubic
Bézier curves, which can have both a peak and a valley in the same segment, among
other possible shapes. In other words, a cubic curve can contain an inflection point (the
point where the curve changes from bending in one direction to bending in the other).
The difference between the quadratic and cubic curves is that the cubic curve has two
control points, one for each endpoint. The technique for generating the cubic curve is
similar to that for generating the quadratic curve. As you can see in Figure 7-11 , you
draw three lines that connect the endpoints and control points (a), and connect their
midpoints. That produces two lines (b). Y ou connect their  midpoints, and that produces
96 | Chapter 7: Paths

2.We’re dispensing with the tent analogy; it gets too unwieldy. Curves based on yurts and geodesic domes are
left as exercises for the reader.one line (c), whose midpoint determines one of the points on the final curve.2 Notice
that the start, end, and middle angles of the curve are tangent to (they “follow”) the
control lines.
Figure 7-11. Visualizing a cubic Bézier curve
To specify such a cubic curve, use the C or c command. The command is followed by
three sets of coordinates that specify the control point for the start point, the control
point for the endpoint, and the endpoint. As with all the other path commands, an
uppercase command implies absolute coordinates; lowercase implies relative coordi‐
nates. The curve in the preceding diagram was drawn from (20,80) to (200,120) with
control points at (50,20) and (150,60). The SVG for the path was as follows:
<path d="M20 80 C 50 20, 150 60, 200 120"
    style="stroke: black; fill: none;"/>
There are many interesting curves you can draw, depending upon the relationship of
the control points (see Figure 7-12 ). To make the graphic cleaner, we show only the lines
from each endpoint to its control point.
Figure 7-12. Result of cubic Bézier control point combinations
Bézier Curves | 97

Experiment with these combinations and more in the online example:
http://oreillymedia.github.io/svg-essentials-examples/ch07/cubic-bezier.html
As with quadratic curves, you can construct a cubic poly-Bézier by specifying several
sets of coordinates after a cubic curve command. The last point of the first curve be‐
comes the first point of the next curve, and so on. Here is a <path>  that draws a cubic
curve from (30,100) to (100,100) with control points at (50,50) and (70,20); it is imme‐
diately followed by a curve that doubles back to (65,100) with control points at (110,130)
and (45,150). Here is the SVG for this path, with control point coordinates in bold:
<path d="M30 100 C 50 50, 70 20, 100 100,
                  110, 130 , 45, 150, 65, 100"/>
The result is shown in the left half of Figure 7-13 ; the control points and lines are shown
in the right half of the diagram.
Figure 7-13. Cubic poly-Bézier curve
If you want to guarantee a smooth join between curves, you can use the S command (or
s if you want to use relative coordinates). In a manner analogous to that of the T com‐
mand for quadratic curves, the new curve will take the previous curve’s endpoint as its
starting point, and its first control point will be the reflection of the previous ending
control point. All you need to supply is the control point for the next endpoint on the
curve, followed by the next endpoint itself.
Here is a cubic Bézier curve drawn from (30,100) to (100,100) with control points at
(50,30) and (70,50). It continues smoothly to (200,80), using (150,40) as its ending
control point. The left half shows the curve; the right half shows the curve with the
control points. The reflected control point is shown with a dashed line in Figure 7-14 :
<path d="M30 100 C 50 30, 70 50, 100 100 S 150 40, 200 80"/>
98 | Chapter 7: Paths

Figure 7-14. Smooth cubic poly-Bézier curve
Path Reference Summary
In Table 7-1 , uppercase commands use absolute coordinates, and lowercase commands
use relative coordinates.
Table 7-1. Path commands
Command Arguments Effect
M m x y Move to given coordinates.
L l x y Draw a line to the given coordinates. You may supply multiple sets of
coordinates to draw a polyline.
H h x Draw a horizontal line to the given x-coordinate.
V v y Draw a vertical line to the given x-coordinate.
A a rx ry x-axis-rotation
large-arc sweep x yDraw an elliptical arc from the current point to ( x,y). The points are on
an ellipse with x-radius rx and y-radius ry. The ellipse is rotated
x-axis-rotation  degrees. If the arc is less than 180 degrees,
large-arc  is 0; if greater than 180 degrees, large-arc  is 1. If the
arc is to be drawn in the positive direction, sweep  is 1; otherwise it is 0.
Q q x1 y1 x y Draw  a quadratic  Bézier  curve  from  the current  point  to (x,y) using  control
point (x1,y1).
T t x y Draw  a quadratic  Bézier  curve  from  the current  point  to (x,y). The control
point will be the reflection of the previous Q command’s control point. If
there is no previous curve, the current point will be used as the control
point.
C c x1 y1 x2 y2 x y Draw a cubic Bézier curve from the current point to ( x,y) using control
point (x1,y1) as the control point for the beginning of the curve and
(x2,y2) as the control point for the endpoint of the curve.
S s x2 y2 x y Draw a cubic Bézier curve from the current point to ( x,y), using (x2,y2)
as the control point for this new endpoint. The first control point will be
the reflection of the previous C command’s ending control point. If there
is no previous curve, the current point will be used as the first control
point.
Path Reference Summary | 99

Paths and Filling
The information described in Chapter 4  in “Filling Polygons That Have Intersecting
Lines”  on page 49 is also applicable to paths, which not only can have intersecting lines,
but also can have “holes” in them. Consider the paths in Example 7-5 , both of which
draw nested squares. In the first path, both squares are drawn clockwise; in the second
path, the outer square is drawn clockwise and the inner square is drawn
counterclockwise.
Example 7-5. Using different fill-rule values on paths
<!-- both paths clockwise -->
<path d="M 0 0, 60 0, 60 60, 0 60 Z
    M 15 15, 45 15, 45 45, 15 45Z" />
<!-- outer path clockwise; inner path counterclockwise -->
<path d="M 0 0, 60 0, 60 60, 0 60 Z
    M 15 15, 15 45, 45 45, 45 15Z" />
Figure 7-15  shows there is a difference when you use a fill-rule  of nonzero , which
takes into account the direction of the lines when determining whether a point is inside
or outside a path. Using a fill-rule  of evenodd  produces the same result for both paths;
it uses total number of lines crossed and ignores their direction.
Figure 7-15. Result of using different fill-rule values
The <marker> element
Consider the following path, which uses a line, an elliptical arc, and another line to draw
the rounded corner in Figure 7-16 :
<path d="M 10 20 100 20 A 20 30 0 0 1 120 50 L 120 110"
    style="fill: none; stroke: black;" />
100 | Chapter 7: Paths

3.Y es, markers are considered to be part of presentation rather than structure. This is one of those gray areas
where you could argue either case.
Figure 7-16. Lines and arc
Presume you want to mark the direction of the path by putting a circle at the beginning,
a solid triangle at the end, and arrowheads at the other vertices, as shown in
Figure 7-17 . To achieve this effect, you need to construct three <marker>  elements and
tell the <path>  to reference them.
Figure 7-17. Lines and arc with markers
As a first step, consider Example 7-6 , which adds the circular marker. A marker is a
“self-contained” graphic with its own private set of coordinates, so you have to specify
its markerWidth  and markerHeight  in the starting <marker>  tag. That is followed by the
SVG elements required to draw the marker, and ends with a closing </marker> . A
<marker>  element does not display by itself, but we are putting it in a <defs>  element
because that’s where reusable elements belong.
We want the circle to be at the beginning of the path, so we add  a marker-start  to the
style  in the <path> .3 The value of this property is a URL reference to the <marker>
element we’ve just created.
Example 7-6. First attempt at circular marker
<defs>
<marker id="mCircle"  markerWidth= "10" markerHeight= "10">
    <circle cx="5" cy="5" r="4" style="fill: none; stroke: black;" />
</marker>
</defs>
<path  d="M 10 20 100 20 A 20 30 0 0 1 120 50 L 120 110"
    style="marker-start: url(#mCircle);
    fill: none; stroke: black;" />
The <marker> element | 101

The result in Figure 7-18  is not quite what you planned…
Figure 7-18. Misplaced circular marker
The reason the circle appears in the wrong place is that, by default, the start marker’s
(0,0) point is aligned with the beginning coordinate of the path. Example 7-7  adds refX
and refY  attributes specifying which coordinates (in the marker’s system) are to align
with the beginning coordinate. Once these are added, the circular marker appears ex‐
actly where it is desired in Figure 7-19 .
Example 7-7. Correctly placed circular marker
<marker id="mCircle"  markerWidth= "10" markerHeight= "10"
        refX="5" refY="5">
    <circle cx="5" cy="5" r="4" style="fill: none; stroke: black;" />
</marker>
Figure 7-19. Correctly placed circular marker
Given this information, you can now write Example 7-8 , which adds the triangular
marker and references it as the marker-end  for the path. Then we can add the arrowhead
marker and reference it as the marker-mid . The marker-mid  will be attached to every
vertex except the beginning and end of the path. Notice that the refX  and refY  attributes
have been set so the wide end of the arrowhead aligns with the intermediate vertices,
while the tip of the solid triangle aligns with the ending vertex. Figure 7-20  shows the
result, which draws the first marker correctly but not the others.
Example 7-8. Attempt to use three markers
<defs>
    <marker id="mCircle"  markerWidth= "10" markerHeight= "10"
        refX="5" refY="5">
        <circle cx="5" cy="5" r="4" style="fill: none; stroke: black;" />
    </marker>
    <marker id="mArrow"  markerWidth= "4" and markerHeight= "8"
102 | Chapter 7: Paths

4.To be exact, the rotation is the average of the angle of the direction of the line going into the vertex and the
direction of the line going out of the vertex.        refX="0" refY="4">
        <path d="M 0 0 4 4 0 8"  style="fill: none; stroke: black;" />
    </marker>
    <marker id="mTriangle"  markerWidth= "5" markerHeight= "10"
        refX="5" refY="5">
        <path d="M 0 0 5 5 0 10 Z"  style="fill: black;" />
    </marker>
</defs>
<path d="M 10 20 100 20 A 20 30 0 0 1 120 50 L 120 110"
    style="marker-start: url(#mCircle);
        marker-mid: url(#mArrow);
        marker-end: url(#mTriangle);
        fill: none; stroke: black;" />
Figure 7-20. Incorrectly oriented markers
To get the effect you want, you must explicitly set a marker’s orient  attribute to auto . 
This makes the marker automatically rotate to match the direction of the path.4 (Y ou
may also specify a number of degrees, in which case the marker will always be rotated
by that amount.) Here in Example 7-9  are the markers, set to produce the effect shown
in Figure 7-17 . Y ou don’t need to orient the circle; it looks the same no matter how it’s
rotated.
Example 7-9. Correctly oriented markers
<defs>
    <marker id="mCircle"  markerWidth= "10" markerHeight= "10"
        refX="5" refY="5">
        <circle cx="5" cy="5" r="4" style="fill: none; stroke: black;" />
    </marker>
    <marker id="mArrow"  markerWidth= "6" markerHeight= "10"
        refX="0" refY="4" orient="auto">
        <path d="M 0 0 4 4 0 8"  style="fill: none; stroke: black;" />
    </marker>
    <marker id="mTriangle"  markerWidth= "5" markerHeight= "10"
        refX="5" refY="5" orient="auto">
The <marker> element | 103

        <path d="M 0 0 5 5 0 10 Z"  style="fill: black;" />
    </marker>
</defs>
<path d="M 10 20 100 20 A 20 30 0 0 1 120 50 L 120 110"
    style="marker-start: url(#mCircle);
        marker-mid: url(#mArrow);
        marker-end: url(#mTriangle);
        fill: none; stroke: black;" />
Another useful attribute is the markerUnits  attribute.  If set to strokeWidth , the marker’s
coordinate system is set so one unit equals the stroke width. This makes your marker
grow in proportion to the stroke width; it’s the default behavior and it’s usually what
you want. If you set the attribute to userSpaceOnUse , the marker’s coordinates are pre‐
sumed to be the same as the coordinate system of the object that references the marker.
The marker will remain the same size irrespective of the stroke width.
Marker Miscellanea
If you want the same marker at the beginning, middle, and end of a path, you don’t need
to specify all of the marker-start , marker-mid , and marker-end  properties. Just use the
marker  property and have it reference the marker you want. Thus, if you wanted all the
vertices to have a circular marker, as shown in Figure 7-21 , you’d write the SVG in
Example 7-10 .
Example 7-10. Using a single marker for all vertices
<defs>
    <marker id="mCircle"  markerWidth= "10" markerHeight= "10"
        refX="5" refY="5">
        <circle cx="5" cy="5" r="4" style="fill: none; stroke: black;" />
    </marker>
</defs>
<path d="M 10 20 100 20 A 20 30 0 0 1 120 50 L 120 110"
    style="marker: url(#mCircle); fill: none; stroke: black;" />
Figure 7-21. Using a single marker for all vertices
It is also possible to set the viewBox  and preserveAspectRatio  attributes on a <marker>
element to gain even more control over its display.  For example, you can use a viewBox
104 | Chapter 7: Paths

to define the grid so that the (0,0) coordinate is in the center of your marker; you may
want to do this instead of using refX  and refY . viewBox  and preserveAspectRatio
work exactly as described in “Specifying User Coordinates for a Viewport”  on page 30
and in “Preserving Aspect Ratio” on page 32 .
Y ou may reference a <marker>  in a <polygon> , <polyline> , or <line>  element as well
as in a <path> .
The following thought may have occurred to you: “If a marker can have a path in it, can
that path have a marker attached to it as well?” The answer is yes, it can, but the second
marker must fit into the rectangle established by the first marker’s markerWidth  and
markerHeight . Remember that just because a thing can be done does not mean it should
be done. If you need such an effect, you are probably better off to draw the secondary
marker as a part of the primary marker rather than attempting to nest markers.
Make sure you don’t define a part of a marker to use itself as a marker. This could happen
if you had a CSS rule like this to give all your paths a star as a marker:
path {marker: url(#star) }
If the <marker>  with id star  has a <path>  in it, that path would refer to itself in an
infinite loop. To prevent this, you would add a CSS rule that says not to put any marker
on a path that is part of the star marker:
path {marker: url(#star) }
marker#star path {marker: none}
Marker Miscellanea | 105