CHAPTER 4
Basic Shapes
Once a coordinate system is established in the <svg>  tag, you are ready to begin draw‐
ing. This chapter describes the basic shapes you can use to create the major elements of
most drawings: lines, rectangles, polygons, circles, and ellipses.
Lines
SVG lets you draw a straight line with the <line>  element. Just specify the x- and y-
coordinates of the line’s endpoints. Coordinates may be specified without units, in which
case they are considered to be user coordinates, or with units such as em, in, etc. (as
described in Chapter 3 , in “The Viewport” on page 27 ).
<line x1=" start-x" y1="start-y"
  x2="end-x" y2="end-y" />
The SVG in Example 4-1  draws several lines; the reference grid in Figure 4-1  is not part
of the SVG you see in the example.
Example 4-1. Basic lines
http://oreillymedia.github.io/svg-essentials-examples/ch04/basic-lines.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <!-- horizontal line -->
  <line x1="40" y1="20" x2="80" y2="20" style="stroke: black;" />
  <!-- vertical line -->
  <line x1="0.7cm" y1="1cm" x2="0.7cm" y2="2.0cm"
    style="stroke: black;" />
   <!-- diagonal line -->
   <line x1="30" y1="30" x2="85" y2="85" style="stroke: black;" />
</svg>
39

Figure 4-1. Basic lines
Stroke Characteristics
Lines are considered to be strokes of a pen that draws on the canvas. The size, color, and
style of the pen stroke are part of the line’s presentation.  Thus, these characteristics will
go into the style  attribute.
stroke-width
As mentioned in Chapter 3 , the canvas grid lines are infinitely thin. Where, then, does
a line or stroke fall in relation to the grid line? The answer is that the grid line falls in
the center of a stroke. Example 4-2  draws some lines where the stroke width has been
set to 10 user coordinates to make the effect obvious. The result, in Figure 4-2 , has the
grid lines drawn in so you can see the effect clearly.
Example 4-2. Demonstration of stroke-width
http://oreillymedia.github.io/svg-essentials-examples/ch04/stroke-width.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <!-- horizontal line -->
  <line x1="30" y1="10" x2="80" y2="10"
    style="stroke-width: 10; stroke: black;" />
  <!-- vertical line -->
  <line x1="10" y1="30" x2="10" y2="80"
    style="stroke-width: 10; stroke: black;" />
  <!-- diagonal line -->
  <line x1="25" y1="25" x2="75" y2="75"
    style="stroke-width: 10; stroke: black;" />
</svg>
40 | Chapter 4: Basic Shapes

Figure 4-2. Demonstration of stroke-width
The SVG coordinate grid may be infinitely thin, but your computer
screen is made of fixed-size pixels. A diagonal line can look jagged as
the computer  translates  it to the nearest  pixel  blocks;  this is known  as
aliasing . Alternatively,  the computer  can use anti-aliasing  to soften
the edges, blurring the line across all pixels it partially overlaps.
Most  SVG  viewers  use anti-aliasing  by default,  and this can some‐
times make a 1-pixel black line look like a 2-pixel gray line, because
it is centered  on the space  between  two pixels.  Y ou can control  the
use of anti-aliasing  with  the CSS shape-rendering  style  property.
Setting this property to crispEdges  (on an element or the SVG as a
whole)  will turn  off anti-aliasing,  resulting  in clear  (if sometimes
jagged)  lines.  A value  of geometricPrecision  will emphasize  smooth
(if sometimes blurry) edges.
Stroke Color
Y ou can specify the stroke color in a variety of ways:
•One of the basic color keyword names: aqua , black , blue , fuchsia , gray , green ,
lime , maroon , navy , olive , purple , red, silver , teal , white , and yellow . Y ou may
also use the color keywords from section 4.2 of the SVG specification .
•A six-digit hexadecimal specifier in the form #rrggbb , where rr is the red compo‐
nent, gg is the green component, and bb is the blue component in the
range 00–ff.
•A three-digit hexadecimal specifier in the form #rgb, where r is the red component,
g is the green component, and b is the blue component in the range 0–f. This is a
shorthand form of the previous method of specifying color. To produce the six-
digit equivalent, each digit of the short form is duplicated; thus #d6e  is the same
as #dd66ee .
•An rgb specifier in the form rgb(red-value , green-value , blue-value ), where
each value is an integer in the range 0–255 or a percentage in the range 0%
to 100% .
Stroke Characteristics | 41

•The currentColor  keyword, which uses the computed CSS color  property for the
element. The color  property—which doesn’t have a direct effect in SVG—is used
in HTML to set text color, and is inherited by child elements. Using currentColor
in an inline SVG icon (see “Inline SVG in XHTML or HTML5”  on page 22) allows
the icon to take on the color of the surrounding text.
Example 4-3  uses all of these methods (with the exception of currentColor ), giving the
colorful results of Figure 4-3 .
Example 4-3. Demonstration of stroke color
http://oreillymedia.github.io/svg-essentials-examples/ch04/stroke-color.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <!-- red -->
  <line x1="10" y1="10" x2="50" y2="10"
    style="stroke: red; stroke-width: 5;" />
  <!-- light green -->
  <line x1="10" y1="20" x2="50" y2="20"
    style="stroke: #9f9; stroke-width: 5;" />
  <!-- light blue -->
  <line x1="10" y1="30" x2="50" y2="30"
    style="stroke: #9999ff; stroke-width: 5;" />
  <!-- medium orange -->
  <line x1="10" y1="40" x2="50" y2="40"
    style="stroke: rgb(255, 128, 64); stroke-width: 5;" />
  <!-- deep purple -->
  <line x1="10" y1="50" x2="50" y2="50"
    style="stroke: rgb(60%, 20%, 60%); stroke-width: 5;" />
</svg>
Figure 4-3. Demonstration of stroke color
There are yet more ways to specify color.  They are taken from the CSS3 Color specifi‐
cation . Although widely supported in web browsers, they are not part of the SVG 1.1
specification, and may not be supported by other SVG implementations; as of this writ‐
ing, for example, neither Apache Batik nor Inkscape supports them. There are three
new color functions and one new keyword:
42 | Chapter 4: Basic Shapes

•rgba()  specifier  in the form rgb(red-value , green-value , blue-value ,
alpha-value ), where the color values are in the same format as for the rgb()
function, and the alpha value is a decimal in the range 0–1
•hsl()  specifier  in the form hsl(hue, saturation , lightness ), where hue is an
integer angle from 0 to 360, and saturation  and lightness  are integers in the range
0–255 or percentages in the range 0% to 100%
•hsla()  specifier, with the hue, saturation, and lightness values the same as for
hsl, and the alpha value the same as for rgba
•transparent  (fully transparent); this is the same as rgba(0, 0, 0, 0)
If you do not specify  a stroke  color,  you won’t  see any lines;  the de‐
fault value for the stroke  property is none .
stroke-opacity
Up to this point, all the lines in the example have been solid, obscuring anything beneath
them.  Y ou control the opacity (which is the opposite of transparency) of a line by giving
the stroke-opacity  a value from 0.0 to 1.0, where 0 is completely transparent and 1
is completely opaque. A value less than 0 will be changed to 0; a value greater than 1
will be changed to 1. Example 4-4  varies the opacity from 0.2 to 1 in steps of 0.2, with
the result in Figure 4-4 . The red line in the figure lets you see the transparency effect
more clearly.
Example 4-4. Demonstration of stroke-opacity
http://oreillymedia.github.io/svg-essentials-examples/ch04/stroke-opacity.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <line x1="30" y1="0" x2="30" y2="60"
        style="stroke:red; stroke-width: 5;" />
  <line x1="10" y1="10" x2="50" y2="10"
    style="stroke-opacity: 0.2; stroke: black; stroke-width: 5;" />
  <line x1="10" y1="20" x2="50" y2="20"
    style="stroke-opacity: 0.4; stroke: black; stroke-width: 5;" />
  <line x1="10" y1="30" x2="50" y2="30"
    style="stroke-opacity: 0.6; stroke: black; stroke-width: 5;" />
  <line x1="10" y1="40" x2="50" y2="40"
    style="stroke-opacity: 0.8; stroke: black; stroke-width: 5;" />
  <line x1="10" y1="50" x2="50" y2="50"
    style="stroke-opacity: 1.0; stroke: black; stroke-width: 5;" />
</svg>
Stroke Characteristics | 43

Figure 4-4. Demonstration of stroke-opacity
stroke-dasharray Attribute
If you need dotted or dashed lines, use the stroke-dasharray  attribute, whose value
consists of a list of numbers, separated by commas or whitespace, specifying dash length
and gaps. The list should have an even number of entries, but if you give an odd number
of entries, SVG will repeat the list so the total number of entries is even. (See the last
instance in Example 4-5 .)
Example 4-5. Demonstration of stroke-dasharray
http://oreillymedia.github.io/svg-essentials-examples/ch04/stroke-dasharray.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
   <!-- 9-pixel dash, 5-pixel gap -->
   <line x1="10" y1="10" x2="100" y2="10"
       style="stroke-dasharray: 9, 5;
       stroke: black; stroke-width: 2;" />
   <!-- 5-pixel dash, 3-pixel gap, 9-pixel dash, 2-pixel gap -->
   <line x1="10" y1="20" x2="100" y2="20"
       style="stroke-dasharray: 5, 3, 9, 2;
       stroke: black; stroke-width: 2;" />
   <!-- Odd number of entries is duplicated; this is equivalent to:
        9-pixel dash, 3-pixel gap,  5-pixel dash,
        9-pixel gap,  3-pixel dash, 5-pixel gap -->
   <line x1="10" y1="30" x2="100" y2="30"
       style="stroke-dasharray: 9 3 5;
       stroke: black; stroke-width: 2;" />
</svg>
Figure 4-5  shows the results, zoomed in for clarity.
Figure 4-5. Demonstration of stroke-dasharray
44 | Chapter 4: Basic Shapes

1.Technically, the x value is the smaller of the x-coordinate values, and the y is the smaller of the y-coordinate
values of the rectangle’s sides in the current user coordinate system. Because you are not yet using transfor‐
mations, which are covered in Chapter 6 , this is the moral equivalent of the upper-left corner.Rectangles
The rectangle is the simplest of the basic shapes.  Y ou specify the x- and y-coordinates
of the upper-left corner of the rectangle,1 its width , and its height . The interior of the
rectangle is filled with the fill  color you specify.  If you do not specify a fill color, the
interior of the shape is filled with black. The fill color may be specified in any of the ways
described in “Stroke Color” on page 41, or it may take the value none  to leave the interior
unfilled and thus transparent. Y ou may also specify a fill-opacity  in the same format
as you did for stroke-opacity  in “stroke-opacity”  on page 43. Both fill  and
fill-opacity  are presentation properties, and they belong in the style  attribute.
After the interior is filled (if necessary), the outline of the rectangle is drawn with strokes,
whose characteristics you may specify as you did for lines. If you do not specify a stroke,
the value none  is presumed, and no outline is drawn. Example 4-6  draws several varia‐
tions of the <rect>  element. Figure 4-6  shows the result, with a grid for reference.
Example 4-6. Demonstration of the rectangle element
http://oreillymedia.github.io/svg-essentials-examples/ch04/rectangle.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <!-- black interior, no outline -->
  <rect x="10" y="10" width="30" height="50"/>
  <!-- no interior, black outline -->
  <rect x="50" y="10" width="20" height="40"
    style="fill: none; stroke: black;" />
  <!-- blue interior, thick semi-transparent red outline -->
  <rect x="10" y="70" width="25" height="30"
    style="fill: #0000ff;
      stroke: red; stroke-width: 7; stroke-opacity: 0.5;" />
  <!-- semi-transparent yellow interior, dashed green outline -->
  <rect x="50" y="70" width="35" height="20"
    style="fill: yellow; fill-opacity: 0.5;
      stroke: green; stroke-width: 2; stroke-dasharray: 5 2" />
</svg>
Rectangles | 45

Figure 4-6. Demonstration of the rect element
The strokes  that form  the outline  “straddle”  the abstract  grid lines,  so
the strokes  will be half inside  the shape  and half outside  the shape.
Figure  4-7, a close-up  of the semi-transparent  red outline  drawn  in
Example 4-6 , shows this clearly.
Figure 4-7. Close-up of transparent border
If you do not specify a starting x or y value, it is presumed to be 0. If you specify a width
or height  of 0, then the rectangle is not displayed. It is an error to provide negative
values for either width  or height .
Rounded Rectangles
If you wish to have rectangles with rounded corners, specify the x- and y-radius of the
corner curvature. The maximum number you may specify for rx (the x-radius) is one-
half the width of the rectangle; the maximum value of ry (the y-radius) is one-half the
height of the rectangle. If you specify only one of rx or ry, they are presumed to be
equal. Example 4-7  shows various combinations of rx and ry.
Example 4-7. Demonstration of rounded rectangles
http://oreillymedia.github.io/svg-essentials-examples/ch04/rounded-rectangles.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <!-- rx and ry equal, increasing -->
  <rect x="10" y="10" width="20" height="40" rx="2" ry="2"
46 | Chapter 4: Basic Shapes

    style="stroke: black; fill: none;" />
  <rect x="40" y="10" width="20" height="40" rx="5"
    style="stroke: black; fill: none;" />
  <rect x="70" y="10" width="20" height="40" ry="10"
    style="stroke: black; fill: none;" />
  <!-- rx and ry unequal -->
  <rect x="10" y="60" width="20" height="40" rx="10" ry="5"
    style="stroke: black; fill: none;" />
  <rect x="40" y="60" width="20" height="40" rx="5" ry="10"
    style="stroke: black; fill: none;" />
</svg>
Figure 4-8  shows the result, with a grid in the background for reference.
Figure 4-8. Demonstration of rounded rectangles
If you’re  familiar  with  the CSS border-radius  property,  you might
know  the trick  of turning  a rectangle  into a circle  or ellipse  by set‐
ting the corner radius to 50% of the height and width. Although you
can specify  an SVG  rectangle’s  corner  radius  with  percent  values,  they
will be interpreted as a percent of the viewport width ( rx) or height
(ry)—the same as if you used a percentage for setting the rectangle’s
width  or height—not  as a percentage  of the rectangle  itself.  Good
thing SVG has an easier way to create circles and ellipses…
Circles and Ellipses
To draw a circle, use the <circle>  element and specify the center x-coordinate, center
y-coordinate, and radius with the cx, cy, and r attributes.  As with a rectangle, the default
is to fill the circle with black and draw no outline unless you specify some other com‐
bination of fill  and stroke .
Circles and Ellipses | 47

An ellipse needs both an x-radius and a y-radius in addition to a center x- and y-
coordinate. The attributes for these radii are named rx and ry.
In both circles and ellipses, if the cx or cy is omitted, it is presumed to be 0. If the radius
is 0, no shape will be displayed; it is an error to provide a negative radius. Example 4-8
draws some circles and ellipses. They are shown in Figure 4-9 .
Example 4-8. Demonstration of circles and ellipses
http://oreillymedia.github.io/svg-essentials-examples/ch04/circles-ellipses.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <circle cx="30" cy="30" r="20" style="stroke: black; fill: none;" />
  <circle cx="80" cy="30" r="20"
    style="stroke-width: 5; stroke: black; fill: none;" />
  <ellipse  cx="30" cy="80" rx="10" ry="20"
    style="stroke: black; fill: none;" />
  <ellipse  cx="80" cy="80" rx="20" ry="10"
    style="stroke: black; fill: none;" />
</svg>
Figure 4-9. Demonstration of circle and ellipse elements
The <polygon> Element
In addition to rectangles, circles, and ellipses, you may want to draw hexagons, octagons,
stars, or arbitrary closed shapes. The <polygon>  element lets you specify a series of
points  that describe a geometric area to be filled and outlined as described earlier.  The
points  attribute consists of a series of x- and y-coordinate pairs separated by commas
or whitespace. Y ou must give an even number of entries in the series of numbers. Y ou
don’t have to return to the starting point; the shape will automatically be closed.
Example 4-9  uses the <polygon>  element to draw a parallelogram, a star, and an irregular
shape.
48 | Chapter 4: Basic Shapes

Example 4-9. Demonstration of the polygon element
http://oreillymedia.github.io/svg-essentials-examples/ch04/polygon.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
  <!-- parallelogram -->
  <polygon  points="15,10  55, 10  45, 20  5, 20"
    style="fill: red; stroke: black;" />
  <!-- star -->
  <polygon
    points="35,37.5  37.9,46.1 46.9,46.1  39.7,51.5
      42.3,60.1  35,55  27.7,60.1  30.3,51.5
      23.1,46.1  32.1,46.1"
      style="fill: #ccffcc; stroke: green;" />
  <!-- weird shape -->
  <polygon
    points="60 60,  65 72,  80 60,  90 90, 72 80, 72 85, 50 95"
    style="fill: yellow; fill-opacity: 0.5; stroke: black;
      stroke-width: 2;" />
</svg>
The results, with a grid in the background for reference, are displayed in Figure 4-10 .
Figure 4-10. Demonstration of the polygon element
Filling Polygons That Have Intersecting Lines
For the polygons shown so far, it’s been easy to fill the shape.  None of the lines forming
the polygon cross over one another, so the interior is easily distinguished from the
exterior of the shape. However, when lines cross over one another, the determination
of what is inside the polygon is not as easy. The SVG in Example 4-10  draws such a
polygon. In Figure 4-11 , is the middle section of the star considered to be inside or
outside?
Example 4-10. Unfilled polygon with intersecting lines
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
<polygon   points="48,16  16,96  96,48  0,48  80,96"
The <polygon> Element | 49

  style="stroke: black; fill: none;" />
</svg>
Figure 4-11. Unfilled polygon with intersecting lines
SVG has two different rules for determining whether a point is inside a polygon or
outside it. The fill-rule  (which is part of presentation) has a value of either nonzero
(the default) or evenodd . Depending on the rule you choose, you get a different effect.
Example 4-11  uses the rules to fill two diagrams of the star. The result is shown in
Figure 4-12 .
Example 4-11. Effect of different fill-rules
http://oreillymedia.github.io/svg-essentials-examples/ch04/polygon-fill.html
<svg width="200px" height="200px" viewBox= "0 0 200 200"
  xmlns="http://www.w3.org/2000/svg" >
<polygon  style="fill-rule: nonzero; fill: yellow; stroke: black;"
  points="48,16  16,96  96,48  0,48  80,96"  />
<polygon  style="fill-rule: evenodd;  fill: #00ff00; stroke: black;"
  points="148,16  116,96  196,48  100,48  180,96"  />
</svg>
Figure 4-12. Effect of different fill-rules
Explanation of the Fill Rules
For the sake of completeness, here is how the fill-rules  work, but don’t worry—you
don’t need to know the details in order to use them. The nonzero  rule determines
whether a point is inside or outside a polygon by drawing a line from the point in
question to infinity. It counts how many times that line crosses the polygon’s lines, adding
one if the polygon line is going right to left, and subtracting one if the polygon line is
50 | Chapter 4: Basic Shapes

going left to right. If the total comes out to zero, the point is outside the polygon. If the
total is nonzero (hence the name), the point is inside the polygon.
The evenodd  rule also draws a line from the point in question to infinity, but it simply
counts how many times that line crosses your polygon’s lines. If the total number of
crossings is odd, then the point is inside; if even, then the point is outside.
The <polyline> Element
Finally, to round out our discussion of basic shapes, we’ll return to straight lines. Some‐
times you want a series of lines that does not make a closed shape.  Y ou can use multiple
<line>  elements, but if there are many lines, it might be easier to use the <polyline>
element.  It has the same points  attributes as <polygon> , except that the shape is not
closed. Example 4-12  draws the symbol for an electrical resistor. The result is in
Figure 4-13 .
Example 4-12. The polyline element
http://oreillymedia.github.io/svg-essentials-examples/ch04/polyline.html
<svg width="100px" height="50px" viewBox= "0 0 100 50"
  xmlns="http://www.w3.org/2000/svg" >
<polyline
  points="5 20, 20 20, 25 10, 35 30, 45 10,
    55 30, 65 10, 75 30, 80 20, 95 20"
  style="stroke: black; stroke-width: 3; fill: none;" />
</svg>
Figure 4-13. Example of the polyline element
It’s best to set the fill  property  to none  when  using  <polyline> ; 
otherwise,  the SVG  viewer  attempts  to fill the shape,  sometimes  with
startling results like those in Figure 4-14 .
Figure 4-14. Example of filled polyline
The <polyline> Element | 51

Line Caps and Joins
When drawing a <line>  or <polyline> , you may specify the shape of the endpoints of
the lines by setting the stroke-linecap  style property  to one of the values butt ,
round , or square . Example 4-13  uses these three values, with gray guide lines to show
the actual endpoints of the lines. Y ou can see in Figure 4-15  that round  and square
extend beyond the end coordinates; butt , the default, ends exactly at the specified end‐
point.
Example 4-13. Values of the stroke-linecap property
http://oreillymedia.github.io/svg-essentials-examples/ch04/linecap.html
<line x1="10" y1="15" x2="50" y2="15"
  style="stroke: black; stroke-linecap: butt; stroke-width: 15;" />
<line x1="10" y1="45" x2="50" y2="45"
  style="stroke: black; stroke-linecap: round; stroke-width: 15;" />
<line x1="10" y1="75" x2="50" y2="75"
  style="stroke: black; stroke-linecap: square; stroke-width: 15;" />
<!-- guide lines -->
<line x1="10" y1="0" x2="10" y2="100" style="stroke: #999;" />
<line x1="50" y1="0" x2="50" y2="100" style="stroke: #999;" />
Figure 4-15. Values of the stroke-linecap attribute
Y ou may specify the way lines connect at the corners of a shape with the
stroke-linejoin  style property, which may have the values miter  (pointed), round
(round—what did you expect?), or bevel  (flat). Example 4-14  produces the result shown
in Figure 4-16 .
Example 4-14. Values of the stroke-linejoin attribute
http://oreillymedia.github.io/svg-essentials-examples/ch04/linejoin.html
<polyline
  style="stroke-linejoin: miter; stroke: black; stroke-width: 12;
  fill: none;"
  points="30 30, 45 15, 60 30" />
<polyline
52 | Chapter 4: Basic Shapes

  style="stroke-linejoin: round; stroke: black; stroke-width: 12;
  fill: none;"
  points="90 30, 105 15, 120 30" />
<polyline
  style="stroke-linejoin: bevel; stroke-width: 12; stroke: black;
  fill: none;"
  points="150 30, 165 15, 180 30" />
Figure 4-16. Values of the stroke-linejoin attribute
If your lines meet at a sharp angle and have a mitered join, it’s possi‐
ble for the pointed  part to extend  far beyond  the lines’  thickness.  Y ou
may  set the ratio  of the miter  to the thickness  of the lines  being  joined
with the stroke-miterlimit  style property; its default value is 4.
Basic Shapes Reference Summary
The following tables summarize the basic shapes and presentation styles in SVG.
Shape Elements
Table 4-1  summarizes the basic shapes available in SVG.
Table 4-1. Shape elements
Shape Description
<line x1=" start-x" y1="start-y"
x2="end-x" y2="end-y"/>Draws a line from the starting point at coordinates ( start-x ,
start-y ) to the ending point at coordinates ( end-x , end-y ).
<rect x=" left-x" y="top-y"
width="width" height=" height"/>Draws a rectangle whose upper-left corner is at ( left-x , top-y )
with the given width  and height .
<circle cx=" center-x " cy="center-y "
r="radius"/>Draws a circle with the given radius , centered at ( center-x ,
center-y ).
<ellipse cx=" center-x " cy="center-y "
rx="x-radius " ry="y-radius "/>Draws an ellipse with the given x-radius  and y-radius  centered
at (center-x , center-y ).
<polygon points=" points-list "/> Draws an arbitrary closed polygon whose outline is described by the
points-list . The points are specified as pairs of x- and y-
coordinates. These are user coordinates only; you may not add a length
unit specifier.
Basic Shapes Reference Summary | 53

Shape Description
<polyline points=" points-list "/> Draws an arbitrary series of connected lines as described by the
points-list . The points are specified as pairs of x- and y-
coordinates. These are user coordinates only; you may not add a length
unit specifier.
When you specify a number for an attribute, it is presumed to be measured in user
coordinates. In all but the last two elements of Table 4-1 , you may also add a length unit
specifier such as mm, pt, etc. to any number. For example:
<line x1="1cm" y1="30" x2="50" y2="10pt"/>
Specifying Colors
Y ou may specify the color for filling or outlining a shape in one of the following ways:
•none , indicating that no outline is to be drawn or that the shape is not to be filled.
•A basic color name, which is one of aqua , black , blue , fuchsia , gray , green , lime ,
maroon , navy , olive , purple , red, silver , teal , white , or yellow .
•One of the extended color names from the SVG specifications .
•Six hexadecimal digits #rrggbb , each pair describing red, green, and blue values.
•Three hexadecimal digits #rgb, describing the red, green, and blue values. This is
shorthand for the previous method; digits are replicated, so #rgb is equivalent to
#rrggbb .
•rgb(r, g, b), each value ranging from 0–255 or from 0% to 100%.
•currentColor , the computed (usually inherited) color  property value for the ele‐
ment.
•One of the specifications from the CSS3 Color module  (which may not be supported
by all SVG implementations).
Stroke and Fill Characteristics
In order to see a line or the outline of a shape, you must specify the stroke characteris‐
tics, using the following attributes. A shape’s outline is drawn after its interior is filled.
All of these characteristics, summarized in Table 4-2 , are presentation properties, and
go in a style  attribute.
54 | Chapter 4: Basic Shapes

Table 4-2. Stroke characteristics
Attribute Values
stroke The stroke color, as described in “Specifying Colors” on page 54 . Default is none .
stroke-width Width of stroke; may be given as user coordinates or with a length specifier. The stroke
width is centered along the abstract grid lines. Default is 1.
stroke-opacity A number  ranging  from  0.0 to 1.0; 0.0 is entirely  transparent;  1.0 is entirely  opaque  (the
default).
stroke-dasharray A series of numbers that tell the length of dashes and gaps with which a line is to be
drawn. These numbers are in user coordinates only. The default value is none .
stroke-linecap Shape of the ends of a line; has one of the values butt  (the default), round , or
square .
stroke-linejoin The shape of the corners of a polygon or series of lines; has one of the values miter
(pointed; the default), round , or bevel  (flat).
stroke-miterlimit Maximum ratio of length of the miter point to the width of the lines being drawn; the
default value is 4.
Y ou can control the way in which the interior of a shape is to be filled by using one of
the fill attributes shown in Table 4-3 . A shape is filled before its outline is drawn.
Table 4-3. Fill characteristics
Attribute Values
fill The fill color, as described in “Specifying Colors” on page 54 . The default is black .
fill-opacity A number ranging from 0.0 to 1.0; 0.0 is entirely transparent; 1.0 (the default) is
entirely opaque.
fill-rule This  attribute  can have  the values  nonzero  (the  default)  or evenodd , which  apply
different rules for determining whether a point is inside or outside a shape. These
rules generate different effects only when a shape has intersecting lines or “holes”
in it. Details are in “Filling Polygons That Have Intersecting Lines” on page 49  earlier
in this chapter.
This is only a small sample of the style properties that can apply to SVG elements;
Table B-1 , in Appendix B , has a complete list.
Basic Shapes Reference Summary | 55