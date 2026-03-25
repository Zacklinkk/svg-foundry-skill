CHAPTER 8
Patterns and Gradients
To this point, you have used only solid colors to fill and outline graphic objects. Y ou are
not restricted to using solid colors; you may also use a pattern or a gradient to fill or
outline a graphic. That’s what we’ll examine in this chapter.
Patterns
To use a pattern, you define a graphic object that is replicated horizontally and vertically
to fill another object (or stroke). This graphic object is called a tile, because the act of
filling an object with a pattern is very much like covering an area of a floor with tiles.
In this section, we will use the quadratic curve drawn by the SVG in Example 8-1  as our
tile. It’s outlined in gray to show its area (20 by 20 user units) clearly.
Example 8-1. Path for a pattern tile
<path d="M 0 0 Q 5 20 10 10 T 20 20"
    style="stroke: black; fill: none;" />
<path d="M 0 0 h20 v20 h-20 z"
    style="stroke: gray; fill: none;" />
Figure 8-1  is zoomed in so you can see it in detail.
Figure 8-1. Zoomed-in view of pattern tile
107

patternUnits
To create a pattern tile, you must enclose the <path>  elements that describe your tile in
a <pattern>  element, and then make several decisions. The first decision is how you
wish to space the tiles, and this is reflected in the patternUnits  attribute. Do you want
the tiles spaced to fill a certain percentage of each object they’re applied to, or do you
want them spaced at equal intervals, no matter what the size of the object they’re filling?
If you want the tile dimensions on an object-by-object basis, you specify the pattern’s
upper-left x and y coordinates, and its width  and height  as percentages or decimals in
the range 0 to 1, and set the patternUnits  attribute to objectBoundingBox . An object’s
bounding box  is the smallest rectangle that completely encloses a particular graphic
object. Example 8-2  creates a sample tile that will be replicated five times horizontally
and five times vertically in any object that it fills.
Example 8-2. Tiles spaced with patternUnits set to objectBoundingBox
<defs>
<pattern  id="tile" x="0" y="0" width="20%" height="20%"
    patternUnits= "objectBoundingBox" >
<path d="M 0 0 Q 5 20 10 10 T 20 20"
    style="stroke: black; fill: none;" />
<path d="M 0 0 h 20 v 20 h -20 z"
    style="stroke: gray; fill: none;" />
</pattern>
</defs>
<rect x="20" y="20" width="100" height="100"
    style="fill: url(#tile); stroke: black;" />
<rect x="135" y="20" width="70" height="80"
    style="fill: url(#tile); stroke: black;" />
<rect x="220" y="20" width="150" height="130"
    style="fill: url(#tile); stroke: black;" />
In Figure 8-2 , the leftmost rectangle, which is 100 user units wide and tall, provides an
exact fit for five tiles that are each 20 user units wide and tall. In the middle rectangle,
the width and height aren’t great enough to show any one pattern tile completely, so
they are truncated. In the rightmost rectangle, extra space is added, because the rec‐
tangle’s width and height exceeds five times the space required for a single tile. In all
cases, because x and y are both set, the upper-left corner of the tile coincides with the
upper-left corner of the rectangle.
108 | Chapter 8: Patterns and Gradients

Figure 8-2. Tiles spaced by objectBoundingBox
If you’re used to most graphics programs, this behavior comes as somewhat of a shock.
Typical graphic editing programs put tiles directly next to one another to fill the area,
no matter what its size. There is never extra padding between tiles, and tiles are cut off
only by the edge of the object they’re filling. If this is the behavior that you want, you
must set the patternUnits  attribute to userSpaceOnUse , and specify the x and y coor‐
dinates, and the width  and height  of the tile in user units. Example 8-3  uses the same
sample tile, set to its exact width and height of 20 user units.
Example 8-3. Changing patternUnits to userSpaceOnUse
http://oreillymedia.github.io/svg-essentials-examples/ch08/patternunits.html
<defs>
<pattern  id="tile" x="0" y="0" width="20" height="20"
    patternUnits= "userSpaceOnUse" >
<path d="M 0 0 Q 5 20 10 10 T 20 20"
    style="stroke: black; fill: none;" />
<path d="M 0 0 h 20 v 20 h -20 z"
    style="stroke: gray; fill: none;" />
</pattern>
</defs>
<rect x="20" y="20" width="100" height="100"
    style="fill: url(#tile); stroke: black;" />
<rect x="135" y="20" width="70" height="80"
    style="fill: url(#tile); stroke: black;" />
<rect x="220" y="20" width="150" height="130"
    style="fill: url(#tile); stroke: black;" />
In Figure 8-3 , the tiles have constant size in all three rectangles. Their alignment is,
however, dependent upon the underlying coordinate system. The middle rectangle, for
example, has an x-coordinate that is not a multiple of 20, so the rectangle’s upper-left
corner doesn’t coincide with a tile’s upper-left corner. (The top edges do align, because
the upper y-coordinate of all three rectangles was carefully chosen to be a multiple
of 20.)
Patterns | 109

Figure 8-3. Tiles spaced by userSpaceOnUse
If you do not specify a value for patternUnits , the default is set to
objectBoundingBox .
patternContentUnits
Y ou must next decide what units are to be used to express the pattern data itself. By
default, the patternContentUnits  attribute is set to userSpaceOnUse . If you set the
attributes to objectBoundingBox , the path data points are expressed in terms of the
object being filled. Example 8-4  shows the SVG that produces Figure 8-4 .
If you use userSpaceOnUse  for your  patternContentUnits , you
should  draw  any objects  to be filled  with  the upper-left  corner  of their
bounding boxes at the origin (0,0).
If you use objectBoundingBox , you will have  to reduce  the
stroke-width  in the pattern data. The width will also be scaled pro‐
portionally  to the bounding  box,  not measured  in user  units,  so a
stroke-width  of 1 would  cover  the entire  tile. In the example,  the
stroke  is set to 0.01,  or 1% of the average  of the object  bounding  box’s
height and width.
Example 8-4. patternContentUnits set to objectBoundingBox
<defs>
<pattern  id="tile"
    patternUnits= "objectBoundingBox"
    patternContentUnits= "objectBoundingBox"
     x="0" y="0" width=".2" height=".2">
    <path d="M 0 0 Q .05 .20 .10 .10 T .20 .20"
        style="stroke: black; fill: none; stroke-width: 0.01;" />
    <path d="M 0 0 h 0.2 v 0.2 h-0.2z"
        style="stroke: black; fill: none; stroke-width: 0.01;" />
</pattern>
</defs>
110 | Chapter 8: Patterns and Gradients

<g transform= "translate(20,20)" >
<rect x="0" y="0" width="100" height="100"
    style="fill: url(#tile); stroke: black;" />
</g>
<g transform= "translate(135,20)" >
<rect x="0" y="0" width="70" height="80"
    style="fill: url(#tile); stroke: black;" />
</g>
<g transform= "translate(220,20)" >
<rect x="0" y="0" width="150" height="130"
    style="fill: url(#tile); stroke: black;" />
</g>
Figure 8-4. patternContentUnits set to objectBoundingBox
If you want to reduce an existing graphic object for use as a tile, it’s easier to use the
viewBox  attribute to scale it. Specifying viewBox  will override any
patternContentUnits  information. Another possible option is to use the
preserveAspectRatio  attribute, as described in “Preserving Aspect Ratio” on page 32 .
Example 8-5  uses a scaled-down version of the cubic poly-Bézier curve from Figure 7-13
as a tile. The stroke-width  is set to 5; otherwise, when scaled down, the pattern you
see in Figure 8-5  would not be visible.
Example 8-5. Using viewBox to scale a pattern
<defs>
<pattern  id="tile"
    patternUnits= "userSpaceOnUse"
    x="0" y="0" width="20" height="20"
    viewBox= "0 0 150 150" >
    <path d="M30 100 C 50 50, 70 20, 100 100,
                      110, 130, 45, 150, 65, 100"
        style="stroke: black; stroke-width: 5; fill: none;" />
</pattern>
</defs>
<rect x="20" y="20" width="100" height="100"
    style="fill: url(#tile); stroke: black;" />
Patterns | 111

Figure 8-5. Pattern scaled with viewBox
Nested Patterns
Again, this may have occurred to you: “If an object can be filled with a pattern, can that
pattern be filled with a pattern as well?” The answer is yes. As opposed to nested markers,
which are rarely necessary, there are some effects you can’t easily achieve without nested
patterns. Example 8-6  creates a rectangle filled with circles, all filled with horizontal
stripes. This produces the unusual, but valid, striped polka-dot effect shown in
Figure 8-6 .
Example 8-6. Nested patterns
<defs>
  <pattern  id="stripe"
    patternUnits= "userSpaceOnUse"
    x="0" y="0" width="6" height="6">
    <path d="M 0 0 6 0"
      style="stroke: black; fill: none;" />
  </pattern>
  <pattern  id="polkadot"
    patternUnits= "userSpaceOnUse"
    x="0" y="0" width="36" height="36">
    <circle cx="12" cy="12" r="12"
      style="fill: url(#stripe);  stroke: black;" />
  </pattern>
</defs>
<rect x="36" y="36" width="100" height="100"
    style="fill: url(#polkadot); stroke: black;" />
Figure 8-6. Patterns within patterns
112 | Chapter 8: Patterns and Gradients

Gradients
Rather than filling an object with a solid color, you can fill it with a gradient , a smooth
color transition from one shade to another. Gradients can be linear , where the color
transition occurs along a straight line, or radial , where the transition occurs as you
radiate outward from a center point.
The linearGradient Element
A linear gradient is a transition through a series of colors along a straight line.  Y ou
specify the colors you want at specific locations, called gradient stops . The stops are part
of the structure of the gradient; the colors are part of the presentation. Example 8-7
shows the SVG for a gradient that fills a rectangle with a smooth transition from gold
to cyan. The result is in Figure 8-7 .
Example 8-7. Simple two-color gradient
http://oreillymedia.github.io/svg-essentials-examples/ch08/linear_gradient.html
<defs>
  <linearGradient  id="two_hues" >
    <stop offset="0%" style="stop-color: #ffcc00;" />
    <stop offset="100%" style="stop-color: #0099cc;" />
  </linearGradient>
</defs>
<rect x="20" y="20" width="200" height="100"
  style="fill: url(#two_hues);  stroke: black;" />
Figure 8-7. Simple two-color gradient
The <stop> element
Let’s examine the <stop>  element more closely.  It has two required attributes: offset
and stop-color . The offset  tells the point along the line at which the color should be
equal to the stop-color . The offset  is expressed as a percentage from 0 to 100% or as
a decimal value from 0 to 1.0. While you don’t need to place stops at 0% and 100%, you
usually will. The stop-color  is specified here in a style , but you may also specify it as
an attribute. Example 8-8  is a slightly more complex linear gradient, with stops for gold
at 0%, reddish-purple at 33.3%, and light green at 100%. The result is shown in
Figure 8-8 .
Gradients | 113

Example 8-8. Three-color gradient
http://oreillymedia.github.io/svg-essentials-examples/ch08/three_stop_gradient.html
<defs>
  <linearGradient  id="three_stops" >
    <stop offset="0%" style="stop-color: #ffcc00;" />
    <stop offset="33.3%" style="stop-color: #cc6699" />
    <stop offset="100%" style="stop-color: #66cc99;" />
  </linearGradient>
</defs>
<rect x="20" y="20" width="200" height="100"
    style="fill: url(#three_stops); stroke: black;" />
Figure 8-8. Three-stop gradient
Y ou can also use a stop-opacity  attribute when specifying a stop color,  with 1 being
totally opaque and 0 being totally transparent. Example 8-9  creates a gradient that fades
out dramatically up to the halfway point, then fades slightly toward the end. The result
is shown in Figure 8-9 .
Example 8-9. Three-opacity gradient
http://oreillymedia.github.io/svg-essentials-examples/ch08/stop_opacity.html
<defs>
  <linearGradient  id="three_opacity_stops" >
    <stop offset="0%" style="stop-color: #906; stop-opacity: 1.0" />
    <stop offset="50%" style="stop-color: #906; stop-opacity: 0.3" />
    <stop offset="100%" style="stop-color: #906; stop-opacity: 0.10" />
  </linearGradient>
</defs>
<rect x="20" y="20" width="200" height="100"
    style="fill: url(#three_opacity_stops); stroke: black;" />
114 | Chapter 8: Patterns and Gradients

Figure 8-9. Gradient using stop-opacity
Establishing a transition line for a linear gradient
The default behavior of a linear gradient is to transition along a horizontal line from
the left side of an object to its right side. If you want the transition of colors to occur
across a vertical line or a line at an angle, you must specify the line’s starting point with
the x1 and y1 attributes and its ending points with the x2 and y2 attributes. By default,
these are also expressed as percentages from 0% to 100% or decimals from 0 to 1.
Example 8-10  uses the same color stops in a horizontal, vertical, and diagonal gradi‐
ent. Rather than duplicate the stops into each <linearGradient>  element, the example
uses the xlink:href  attribute to refer to the original left-to-right gradient. The stops
will be inherited, but the x- and y-coordinates will be overridden by each individual
gradient. The arrows in Figure 8-10  do not appear in the SVG of Example 8-10 .
Example 8-10. Defining vectors for a linear gradient
http://oreillymedia.github.io/svg-essentials-examples/ch08/transition_line.html
<defs>
<linearGradient  id="three_stops" >
  <stop offset="0%" style="stop-color: #ffcc00;" />
  <stop offset="33.3%" style="stop-color: #cc6699" />
  <stop offset="100%" style="stop-color: #66cc99;" />
</linearGradient>
<linearGradient  id="right_to_left"
  xlink:href= "#three_stops"
  x1="100%" y1="0%" x2="0%" y2="0%"/>
<linearGradient  id="down"
  xlink:href= "#three_stops"
  x1="0%" y1="0%" x2="0%" y2="100%"/>
<linearGradient  id="up"
  xlink:href= "#three_stops"
  x1="0%" y1="100%" x2="0%" y2="0%"/>
<linearGradient  id="diagonal"
  xlink:href= "#three_stops"
  x1="0%" y1="0%" x2="100%" y2="100%"/>
</defs>
<rect x="40" y="20" width="200" height="40"
Gradients | 115

  style="fill: url(#three_stops); stroke: black;" />
<rect x="40" y="70" width="200" height="40"
  style="fill: url(#right_to_left); stroke: black;" />
<rect x="250" y="20" width="40" height="200"
  style="fill: url(#down); stroke: black;" />
<rect x="300" y="20" width="40" height="200"
  style="fill: url(#up); stroke: black;" />
<rect x="40" y="120" width="200" height="100"
  style="fill: url(#diagonal); stroke: black;" />
Figure 8-10. Defining vectors for a linear gradient
If you wish  to establish  the transition  line using  user  space  coordi‐
nates  instead  of percentages,  set the gradientUnits  to
userSpaceOnUse  instead  of the default  value,  which  is
objectBoundingBox .
The spreadMethod attribute
The transition line does not have to go from one corner of an object to another.  What
happens if you say that the transition line goes from (20%,30%) to (40%,80%)? What
happens to the part of the object outside that line? Y ou can set the spreadMethod  at‐
tribute to one of these values:
pad
The beginning and ending stop colors will be extended to the edges of the object.
repeat
The gradient will be repeated start-to-end until it reaches the edges of the object
being filled.
116 | Chapter 8: Patterns and Gradients

reflect
The gradient will be reflected end-to-start, start-to-end until it reaches the edges of
the object being filled.
Figure 8-11  shows the leftmost square’s gradient padded, the middle square’s gradient
repeated, and the right square’s gradient reflected. The original transition line has been
added to the SVG for each square in Example 8-11  to make the effect easier to detect.
Example 8-11. Effects of spreadMethod values on a linear gradient
http://oreillymedia.github.io/svg-essentials-examples/ch08/spread_method.html
<defs>
<linearGradient  id="partial"
  x1="20%" y1="30%" x2="40%" y2="80%">
  <stop offset="0%" style="stop-color: #ffcc00;" />
  <stop offset="33.3%" style="stop-color: #cc6699" />
  <stop offset="100%" style="stop-color: #66cc99;" />
</linearGradient>
<linearGradient  id="padded"
  xlink:href= "#partial"
  spreadMethod= "pad"/>
<linearGradient  id="repeated"
  xlink:href= "#partial"
  spreadMethod= "repeat" />
<linearGradient  id="reflected"
  xlink:href= "#partial"
  spreadMethod= "reflect" />
<line id="show-line"  x1="20" y1="30" x2="40" y2="80"
  style="stroke: white;" />
</defs>
<rect x="20" y="20" width="100" height="100"
  style="fill: url(#padded); stroke: black;" />
<use xlink:href= "#show-line"  transform= "translate (20,20)" />
<rect x="130" y="20" width="100" height="100"
  style="fill: url(#repeated); stroke: black;" />
<use xlink:href= "#show-line"  transform= "translate (130,20)" />
<rect x="240" y="20" width="100" height="100"
  style="fill: url(#reflected); stroke: black;" />
<use xlink:href= "#show-line"  transform= "translate (240,20)" />
Gradients | 117

1.If the bounding box of the object being filled is not square, the transition path will become elliptical to match
the aspect ratio of the bounding box.
Figure 8-11. spreadMethod values pad, repeat, and reflect for a linear gradient
The radialGradient Element
The other type of gradient you can use is the radial gradient, where each color stop
represents a circular path, radiating outward from a focus point.1 It’s set up in much the
same way as a linear gradient. Example 8-12  sets three stops: orange, green, and purple.
The result is shown in Figure 8-12 .
Example 8-12. Radial gradient with three stops
http://oreillymedia.github.io/svg-essentials-examples/ch08/three_stop_radial.html
<defs>
  <radialGradient  id="three_stops" >
    <stop offset="0%" style="stop-color: #f96;" />
    <stop offset="50%" style="stop-color: #9c9;" />
    <stop offset="100%" style="stop-color: #906;" />
  </radialGradient>
</defs>
<rect x="20" y="20" width="100" height="100"
  style="fill: url(#three_stops); stroke: black;" />
Figure 8-12. Radial gradient with three stops
Establishing transition limits for a radial gradient
Instead of using a line to determine where the 0% and 100% stop points should be, a
radial gradient’s limits are determined by a circle; the center is the 0% stop point, and
the outer circumference defines the 100% stop point. Y ou define the outer circle with
the cx (center x), cy (center y), and r (radius) attributes. All of these are in terms of
118 | Chapter 8: Patterns and Gradients

percentages of the object’s bounding box. The default value for all these attributes is
50%. Example 8-13  draws a square with a radial gradient with the zero point centered
at the upper left of the square and the outer edge at the lower right. The result is shown
in Figure 8-13 .
Example 8-13. Setting limits for a radial gradient
http://oreillymedia.github.io/svg-essentials-examples/ch08/radial_limits.html
<defs>
  <radialGradient  id="center_origin"
    cx="0%" cy="0%" r="141% ">
      <stop offset="0%" style="stop-color: #f96;" />
      <stop offset="50%" style="stop-color: #9c9;" />
      <stop offset="100%" style="stop-color: #906;" />
  </radialGradient>
</defs>
<rect x="20" y="20" width="100" height="100"
  style="fill: url(#center_origin); stroke: black;" />
Figure 8-13. Setting limits for a radial gradient
In the preceding  example,  the radialGradient ’s r was set to 141%
instead of 100%. This is because the unit used to measure the radius
is the average  of the height  and width  of the object’s  bounding  box,
not the box’s  diagonal.  The ratio  of the diagonal  to the side of a square
is the square root of two, or 1.41.
The 0% stop point, also called the focal point, is by default placed at the center of the
circle that defines the 100% stop point. If you wish to have the 0% stop point at some
point other than the center of the limit circle, you must change the fx and fy attributes.
The focal point should be within the circle established for the 100% stop point. If it’s
not, the SVG viewer program will automatically move the focal point to the outer cir‐
cumference of the end circle.
In Example 8-14 , the circle is centered at the origin with a radius of 100%, but the focal
point is at (50%,50%). As you see in Figure 8-14 , this has the visual effect of moving the
“center. ”
Gradients | 119

Example 8-14. Setting focal point for a radial gradient
http://oreillymedia.github.io/svg-essentials-examples/ch08/radial_focus.html
<defs>
  <radialGradient  id="focal_set"
    cx="0%" cy="0%" fx="50%" fy="50%" r="100%">
      <stop offset="0%" style="stop-color: #f96;" />
      <stop offset="50%" style="stop-color: #9c9;" />
      <stop offset="100%" style="stop-color: #906;" />
  </radialGradient>
</defs>
<rect x="20" y="20" width="100" height="100"
  style="fill: url(#focal_set); stroke: black;" />
Figure 8-14. Setting focal point for a radial gradient
The default values for the limit-setting attributes of a <radialGradient>  are as
follows:
Attribute Default value
cx 50% (horizontal center of object bounding box)
cy 50% (vertical center of object bounding box)
r 50% (half the width/height of object bounding box)
fx Same as cx
fy Same as cy
If you wish  to establish  the circle  limits  using  user  space  coordi‐
nates  instead  of percentages,  set the gradientUnits  to
userSpaceOnUse  instead  of the default  value,  which  is
objectBoundingBox .
The spreadMethod attribute for radial gradients
In the event that the limits you’ve described don’t reach to the edges of the object,  you
can set the spreadMethod  attribute to one of the values pad, repeat , or reflect  as
described earlier in “The spreadMethod attribute” on page 116  to fill up the remaining
space as you wish. Example 8-15  has all three effects; Figure 8-15  shows the leftmost
120 | Chapter 8: Patterns and Gradients

square’s gradient padded, the middle square’s gradient repeated, and the right square’s
gradient reflected.
Example 8-15. Effects of spreadMethod values on a radial gradient
http://oreillymedia.github.io/svg-essentials-examples/ch08/radial_spread_method.html
<defs>
  <radialGradient  id="three_stops"
    cx="0%" cy="0%" r="70%">
      <stop offset="0%" style="stop-color: #f96;" />
      <stop offset="50%" style="stop-color: #9c9;" />
      <stop offset="100%" style="stop-color: #906;" />
  </radialGradient>
  <radialGradient  id="padded"  xlink:href= "#three_stops"
    spreadMethod= "pad"/>
  <radialGradient  id="repeated"  xlink:href= "#three_stops"
    spreadMethod= "repeat" />
  <radialGradient  id="reflected"  xlink:href= "#three_stops"
    spreadMethod= "reflect" />
</defs>
<rect x="20" y="20" width="100" height="100"
  style="fill: url(#padded); stroke: black;" />
<rect x="130" y="20" width="100" height="100"
  style="fill: url(#repeated); stroke: black;" />
<rect x="240" y="20" width="100" height="100"
  style="fill: url(#reflected); stroke: black;" />
Figure 8-15. spreadMethod values pad, repeat, and reflect for a radial gradient
Gradient Reference Summary
Linear and radial gradients describe a smooth transition of colors used to fill an ob‐
ject. The object in question has a bounding box, defined as the smallest rectangle that
entirely contains the object. The <linearGradient>  and <radialGradient>  elements
are both containers for a series of <stop>  elements. Each of these <stop>  elements
specifies a stop-color , an offset , and optionally a stop-opacity . For linear gradients,
the offset is a percentage of the distance along the gradient’s linear vector. For radial
gradients, it is a percentage of the distance along the gradient’s radius.
Gradients | 121

For a linear gradient, the starting point of the vector (which has the 0% stop color) is
defined by the attributes x1 and y1; the ending point (which has the 100% stop color)
by the attributes x2 and y2.
For a radial gradient, the focal point (which has the 0% stop color) is defined by the
attributes fx and fy; the circle that has the 100% stop color is defined by its center
coordinates cx and cy and its radius r.
If the gradientUnits  attribute has the value objectBoundingBox , the coordinates are
taken as a percentage of the bounding box’s dimensions (this is the default). If the value
is set to userSpaceOnuse , the coordinates are taken to be in the coordinate system used
by the object being filled.
If the vector for a linear gradient or the circle for a radial gradient does not reach to the
boundaries of the object being filled, the remaining space will be colored as determined
by the value of the spreadMethod  attribute: pad, the default, extends the start and end
colors to the boundaries; repeat  repeats the gradient start-to-end until it reaches the
boundaries; and reflect  replicates the gradient end-to-start and start-to-end until it
reaches the object boundaries.
Transforming Patterns and Gradients
Sometimes you may need to skew, stretch, or rotate a pattern or gradient. Y ou’re not
transforming the object being filled; you’re transforming the pattern or the color spec‐
trum used to fill the object. The gradientTransform  and patternTransform  attributes
let you do just that, as written in Example 8-16  and shown in Figure 8-16 .
Example 8-16. Transforming patterns and gradients
http://oreillymedia.github.io/svg-essentials-examples/ch08/pattern_gradient_trans‐
form.html
<defs>
  <pattern  id="tile" x="0" y="0" width="20%" height="20%"
      patternUnits= "objectBoundingBox" >
  <path d="M 0 0 Q 5 20 10 10 T 20 20"
      style="stroke: black; fill: none;" />
  <path d="M 0 0 h 20 v 20 h -20 z"
      style="stroke: gray; fill: none;" />
  </pattern>
  <pattern  id="skewed-tile"
    patternTransform= "skewY(15)"
    xlink:href= "#tile"/>
  <linearGradient  id="plain">
    <stop offset="0%" style="stop-color: #ffcc00;" />
    <stop offset="33.3%" style="stop-color: #cc6699" />
122 | Chapter 8: Patterns and Gradients

    <stop offset="100%" style="stop-color: #66cc99;" />
  </linearGradient>
  <linearGradient  id="skewed-gradient"
    gradientTransform= "skewX(10)"
    xlink:href= "#plain" />
</defs>
<rect x="20" y="10" width="100" height="100"
    style="fill: url(#tile); stroke: black;" />
<rect x="135" y="10" width="100" height="100"
    style="fill: url(#skewed-tile); stroke: black;" />
<rect x="20" y="120" width="200" height="50"
    style="fill: url(#plain); stroke: black;" />
<rect x="20" y="190" width="200" height="50"
    style="fill: url(#skewed-gradient); stroke: black;" />
Figure 8-16. Transformation of a pattern and gradient
One final note about gradients and patterns—although these examples have applied
them to only the filled area of a shape, you may also apply them to the stroke . This lets
you produce a multicolored or patterned outline for an object. Y ou’ll usually set the
stroke-width  to a number greater than 1 in order to make the effect more clearly visible.
The objectBoundingBox  is based  on the extent  of the shape  before
adding  the stroke.  Because  straight  vertical  and horizontal  lines  have
a zero-width  or zero-height  bounding  box,  a gradient  or pattern
using  objectBoundingBox  units  will be ignored  when  used  as the
stroke value for these lines. This means that the line won’t be drawn
at all unless  you specify  a fallback  stroke  value  like
stroke: url(#rainbow) red;  in your style.
Fallback fill and stroke options are also a good idea if your pattern
or gradient  is defined  in a separate  file, in case that file cannot  be
loaded or the SVG viewer doesn’t support external references.
Transforming Patterns and Gradients | 123