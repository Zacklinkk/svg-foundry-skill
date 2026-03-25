CHAPTER 3
Coordinates
The world of SVG is an infinite canvas. In this chapter, we’ll find out how to tell a viewer
program which part of this canvas you’re interested in, what its dimensions are, and
how to locate points within that area.
The Viewport
The area of the canvas your document intends to use is called the viewport . Y ou establish
the size of this viewport with the width  and height  attributes on the <svg>  element.
Each attribute’s value can be simply a number, which is presumed to be in pixels; this is
said to be specified in user coordinates. Y ou may also specify width  and height  as a
number followed by a unit identifier, which can be one of the following:
em
The font size of the default font, usually equivalent to the height of a line of text
ex
The height of the letter x
px
Pixels (in CSS2-supporting graphics, there are 96 pixels per inch)
pt
Points (1/72 of an inch)
pc
Picas (1/6 of an inch)
cm
Centimeters
27

1.In this book, coordinates are specified as a pair of numbers in parentheses, with the x-coordinate first. Thus,
(10,30) represents an x-coordinate of 10 and a y-coordinate of 30.
2.To save space, we are leaving out the <?xml …?>  and <!DOCTYPE …>  lines. These are set in stone, so you can
take them for granite.mm
Millimeters
in
Inches
Possible SVG viewport declarations include the following:
<svg width="200" height="150">  
<svg width="200px" height="150px">
Both of these specify an area 200 pixels wide and 150 pixels tall.
<svg width="2cm" height="3cm">
This specifies an area 2 centimeters wide and 3 centimeters high.
<svg width="2cm" height="36pt">
It is possible, though unusual, to mix units; this element specifies an area 2 centi‐
meters wide and 36 points high.
An <svg>  element may also specify its width  and height  as a percentage. When the
element is nested within another <svg>  element, the percentage is measured in terms
of the enclosing element. If the <svg>  element is the root element, the percentage is in
terms of the window size. Y ou will see nested <svg>  elements in “Nested Systems of
Coordinates” on page 36 .
Using Default User Coordinates
The viewer sets up a coordinate system where the horizontal, or x-coordinate, increases
as you go to the right, and the vertical, or y-coordinate, increases as you move vertically
downward.  The upper-left corner of the viewport is defined to have an x- and y-
coordinate of 0.1 This point, written as (0,0), is also called the origin.  The coordinate
system is a pure geometric system; points have neither width nor height, and the grid
lines are considered infinitely thin. Y ou can read more about this subject in Chapter 4 .
Example 3-1  establishes a viewport 200 pixels wide and 200 pixels high, and then draws
a rectangle whose upper-left corner is at coordinate (10,10) with a width of 50 pixels
and a height of 30 pixels.2 Figure 3-1  shows the result, with rulers and a grid to show
the coordinate system.
28 | Chapter 3: Coordinates

Example 3-1. Using default coordinates
http://oreillymedia.github.io/svg-essentials-examples/ch03/default_coordinates.html
<svg width="200" height="200">
    <rect x="10" y="10" width="50" height="30"
        style="stroke: black; fill: none;" />
</svg>
Figure 3-1. Rectangle using default coordinates
Even if you don’t specify units in the viewport, you may still use them in some SVG
shape elements, as in Example 3-2 . Figure 3-2  shows the result,  with rulers and a grid
to show the coordinate system.
Example 3-2. Explicit use of units
http://oreillymedia.github.io/svg-essentials-examples/ch03/explicit_units.html
<svg width="200" height="200">
    <rect x="10mm" y="10mm" width="15mm" height="10mm"
        style="stroke:black; fill:none;" />
</svg>
Figure 3-2. Rectangle using explicit units
Using Default User Coordinates | 29

Specifying units in the <svg>  element does not affect coordinates given without units
in other elements. Example 3-3  shows a viewport set up in millimeters, but the rectangle
is still drawn at pixel (user) coordinates, as you see in Figure 3-3 .
Example 3-3. Units on the svg element
http://oreillymedia.github.io/svg-essentials-examples/ch03/units_on_svg.html
<svg width="70mm" height="70mm">
    <rect x="10" y="10" width="50" height="30"
       style="fill: none; stroke: black;" />
</svg>
Figure 3-3. Viewport with units; rectangle without units
Specifying User Coordinates for a Viewport
In the examples so far, numbers without units have been considered to be pixels.  Some‐
times this is not what you want. For example, you might want to set up a system where
each user coordinate represents 1/16th of a centimeter. (We’re using this coordinate
system to prove a point, not to show a paragon of good design.) In this system, a square
that is 40 units by 40 units will display as 2.5 centimeters on a side.
To accomplish this effect, you set the viewBox  attribute on the <svg>  element.  The value
of this attribute consists of four numbers that represent the minimum x-coordinate,
minimum y-coordinate, width, and height of the user coordinate system you want to
superimpose on the viewport.
So, to set up the 16-units-per-centimeter coordinate system for a 4-centimeter by 5-
centimeter drawing, you’d use this starting tag:
<svg width="4cm" height="5cm" viewBox= "0 0 64 80" >
Example 3-4  gives the SVG for a picture of a house, displayed using the new coordinate
system. Figure 3-4  shows the result.  The grid and darker numbers show the new user
coordinate system; the lighter numbers are positioned at 1-centimeter intervals.
30 | Chapter 3: Coordinates
www.allitebooks.com


Example 3-4. Using a viewBox
http://oreillymedia.github.io/svg-essentials-examples/ch03/using_viewbox.html
<svg width="4cm" height="5cm" viewBox= "0 0 64 80" >
  <rect x="10" y="35" width="40" height="40"
    style="stroke: black; fill: none;" />
  <!-- roof -->
  <polyline  points="10 35, 30 7.68, 50 35"
    style="stroke:black; fill: none;" />
  <!-- door -->
  <polyline  points="30 75, 30 55, 40 55, 40 75"
    style="stroke:black; fill: none;" />
</svg>
Figure 3-4. New user coordinates
The numbers you specify for the value of the viewBox  attribute may be separated by
commas or whitespace. If either the width or height is 0, none of your graphic will
display. It is an error to specify a negative value for the viewBox  width or height.
Specifying User Coordinates for a Viewport | 31

If you were  reading  the code  in Example  3-4 carefully,  you would  have
noted that we used a decimal value to get the peak of the house’s roof
positioned  just right.  Nearly  all numbers  in SVG  are floating-point
decimal  numbers.  SVG  viewers  are required  to support  at least  32-
bit precision  numbers  and are encouraged  to use higher  precision
numbers  for some  calculations.  In fact,  you can even  use scientific
notation  to work  in a coordinate  system  with  very  large  or small
numbers,  so that the point  30,7.68  could  have  been  written  like
3.0E+1,7.68e0 . But for readability and brevity, we wouldn’t recom‐
mend  it—reserve  the scientific  notation  for when  it is really  necessary.
Preserving Aspect Ratio
In the previous example, the aspect ratio, or ratio of width to height, of the viewport
and the viewBox  were identical (4/5 = 64/80). What happens, though, if the aspect ratio
of the viewport and the viewBox  are not the same, as in this example, where viewBox
has an aspect ratio of 1:1 (the width and height are equal), but the viewport has an aspect
ratio of 1:3 (the height is three times as big as the width)?
<svg width="45px" height="135px" viewBox= "0 0 90 90" >
There are three things SVG can do in this situation:
•Scale the graphic uniformly according to the smaller dimension so the graphic will
fit entirely into the viewport. In the example, the picture would become half its
original width and height. Y ou’ll see examples of this in “Using the meet Specifi‐
er” on page 34 .
•Scale the graphic uniformly according to the larger dimension and cut off the parts
that lie outside the viewport. In the example, the picture would become one and a
half times its original width and height. Y ou’ll see examples of this in “Using the
slice Specifier” on page 35 .
•Stretch and squash the drawing so it fits precisely into the new viewport. (That is,
don’t preserve the aspect ratio at all.) See the details in “Using the none Specifier”
on page 36 .
In the first case, because the image will be smaller than the viewport in one dimension,
you must specify where to position it. In the example, the picture will be scaled uniformly
to a width and height of 45 pixels. The width of the reduced graphic fits the width of
the viewport perfectly, but you must now decide whether the image meets (is aligned
with) the top, middle, or bottom of the 135-pixel viewport height.
In the second case, because the image will be larger than the viewport in one dimension,
you must specify which area is to be sliced away. In the example, the picture will be
scaled uniformly to a width and height of 135 pixels. Now the height of the graphic fits
32 | Chapter 3: Coordinates

the viewport perfectly, but you must decide whether to slice off the right side, left side,
or both edges of the picture to fit within the 45-pixel viewport width.
Specifying Alignment for preserveAspectRatio
The preserveAspectRatio  attribute lets you specify the alignment of the scaled image
with respect to the viewport, and whether you want it to meet the edges or be sliced off.
The model for this attribute is
preserveAspectRatio=" alignment  [meet | slice]"
where alignment  specifies the axis and location and is one of the combinations shown
in Table 3-1 . This alignment specifier is formed by concatenating an x-alignment and
a y-alignment min, mid (middle), or max value. The default value for
preserveAspectRatio  is xMidYMid meet .
The y-alignment  begins  with  a capital  letter,  because  the x- and y-
alignments are concatenated into a single word.
Table 3-1. Values for alignment portion of preserveAspectRatio
Y AlignmentX Alignment
xMin
Align  minimum  x value
of viewBox  with left
edge of viewportxMid
Align midpoint x value
of viewBox  with
horizontal center of
viewportxMax
Align maximum x
value of viewBox
with right edge of
viewport
yMin
Align minimum y value of viewBox
with top edge of viewportxMinYMin xMidYMin xMaxYMin
yMid
Align midpoint y value of viewBox
with vertical center of viewportxMinYMid xMidYMid xMaxYMid
yMax
Align maximum y value of viewBox
with bottom edge of viewportxMinYMax xMidYMax xMaxYMax
Thus, if you want to have the picture with a viewBox="0 0 90 90"  fit entirely within a
viewport that is 45 pixels wide and 135 pixels high, aligned at the top of the viewport,
you would write the following:
<svg width="45px" height="135px" viewBox= "0 0 90 90"
  preserveAspectRatio= "xMinYMin meet" >
Preserving Aspect Ratio | 33

In this case, because the width fits precisely, the x-alignment is irrel‐
evant;  you could  equally  well use xMidYMin  or xMaxYMin . However,
you normally  use preserveAspectRatio  when  you don’t  know  the
aspect  ratio  of the viewport.  For example,  you might  want  the im‐
age to scale to fit the application window, or you might let the CSS of
a parent document set the height and width. In those situations, you
need  to consider  how  you want  your  image  to display  when  the view‐
port is too wide as well as when it is too tall.
If you don’t specify a preserveAspectRatio , the default value is xMidYMid meet , which
will scale down the graphic to fit the available space, and center it both horizontally and
vertically.
This is all fairly abstract; the following sections give some concrete examples that show
you how the combinations of alignment and meet  and slice  interact with one another.
Using the meet Specifier
The starting <svg>  tags in Example 3-5  all use the meet  specifier.
Example 3-5. Use of meet specifier
<!-- tall viewports -->
<svg preserveAspectRatio= "xMinYMin meet"  viewBox= "0 0 90 90"
    width="45" height="135">
<svg preserveAspectRatio= "xMidYMid meet"  viewBox= "0 0 90 90"
    width="45" height="135">
<svg preserveAspectRatio= "xMaxYMax meet"  viewBox= "0 0 90 90"
    width="45" height="135">
<!-- wide viewports -->
<svg preserveAspectRatio= "xMinYMin meet"  viewBox= "0 0 90 90"
    width="135" height="45">
<svg preserveAspectRatio= "xMidYMid meet"  viewBox= "0 0 90 90"
    width="135" height="45">
<svg preserveAspectRatio= "xMaxYMax meet"  viewBox= "0 0 90 90"
    width="135" height="45">
Figure 3-5  shows where the reduced image fits into the enclosing viewBox .
34 | Chapter 3: Coordinates

Figure 3-5. meet—viewBox fits in viewport
Using the slice Specifier
Figure 3-6  shows the use of the slice  specifier to eliminate parts of the picture that do
not fit in the viewport. They were created with the <svg>  tags in Example 3-6 .
Example 3-6. Use of slice specifier
<!-- tall viewports -->
<svg preserveAspectRatio= "xMinYMin slice"  viewBox= "0 0 90 90"
    width="45" height="135">
<svg preserveAspectRatio= "xMidYMid slice"  viewBox= "0 0 90 90"
    width="45" height="135">
<svg preserveAspectRatio= "xMaxYMax slice"  viewBox= "0 0 90 90"
    width="45" height="135">
<!-- wide viewports -->
<svg preserveAspectRatio= "xMinYMin slice"  viewBox= "0 0 90 90"
    width="135" height="45">
<svg preserveAspectRatio= "xMidYMid slice"  viewBox= "0 0 90 90"
    width="135" height="45">
<svg preserveAspectRatio= "xMaxYMax slice"  viewBox= "0 0 90 90"
    width="135" height="45">
The online example for this section allows you to experiment with the different
preserveAspectRatio  options to slice, shrink, and shift the cat around any
sized SVG:
http://oreillymedia.github.io/svg-essentials-examples/ch03/meet_slice_specifier.html
Preserving Aspect Ratio | 35

Figure 3-6. slice—graphic fills viewport
Using the none Specifier
Finally, there is the third option for  scaling a graphic when the viewBox  and viewport
don’t have the same aspect ratio. If you specify preserveAspectRatio="none" , then the
graphic will be scaled nonuniformly so its user coordinates fit the viewport. Figure 3-7
shows such a “fun-house mirror” effect produced with the <svg>  tags in Example 3-7 .
Example 3-7. Aspect ratio not preserved
<!-- tall viewport -->
<svg preserveAspectRatio= "none" viewBox= "0 0 90 90"
   width="45" height="135">
<!-- wide viewport -->
<svg preserveAspectRatio= "none" viewBox= "0 0 90 90"
  width="135" height="45">
Figure 3-7. Aspect ratio not preserved
Nested Systems of Coordinates
Y ou can establish a new viewport and system of coordinates at any time by putting
another <svg>  element into your document. The effect is to create a “mini-canvas” upon
which you can draw. We used this technique to create illustrations such as Figure 3-5 .
Rather than drawing the rectangles, then rescaling and positioning the cat inside each
one (the brute-force approach), we took these steps:
36 | Chapter 3: Coordinates

•Draw the blue rectangles on the main canvas
•For each rectangle, define a new <svg>  element with the appropriate
preserveAspectRatio  attribute
•Draw the cat into that new canvas (with <use> ), and let SVG do the heavy lifting
Here’s a simplified example that shows a circle on the main canvas,  then inside a new
canvas outlined by a blue rectangle that’s also on the main canvas. Figure 3-8  is the
desired result.
Figure 3-8. Nested viewports
First, generate the SVG for the main coordinate system and the circle (note that the user
coordinates coincide exactly with the viewport in this document):
<svg width="200px" height="200px" viewBox= "0 0 200 200" >
    <circle cx="25" cy="25" r="25" style="stroke: black; fill: none;" />
</svg>
The result is in Figure 3-9 .
Figure 3-9. Circle in main viewport
Now, draw the boundary of the box showing where you want the new viewport to be:
<svg width="200px" height="200px" viewBox= "0 0 200 200" >
    <circle cx="25" cy="25" r="25" style="stroke: black; fill: none;" />
    <rect x="100" y="5" width="30" height="80"
       style="stroke: blue; fill: none;" />
</svg>
This produces Figure 3-10 .
Nested Systems of Coordinates | 37

Figure 3-10. Circle and boundary box in main viewport
Now, add another <svg>  element for the new viewport. In addition to specifying the
viewBox , width , height , and preserveAspectRatio  specification, you may also specify
the x and y attributes—in terms of the enclosing <svg>  element—where the new view‐
port is to be established (if you don’t give values for x and y, they are presumed to
be 0):
<svg width="200px" height="200px" viewBox= "0 0 200 200" >
    <circle cx="25" cy="25" r="25" style="stroke: black; fill: none;" />
    <rect x="100" y="5" width="30" height="80"
        style="stroke: blue; fill: none;" />
    <svg x="100px" y="5px" width="30px" height="80px"
      viewBox= "0 0 50 50"  preserveAspectRatio= "xMaxYMax meet" >
    </svg>
</svg>
Setting up the new coordinates with this nested <svg>  element doesn’t change the visual
display, but it does permit you to add the circle in that new system, producing the result
shown in Figure 3-8 :
<svg width="200px" height="200px" viewBox= "0 0 200 200" >
  <circle cx="25" cy="25" r="25" style="stroke: black; fill: none;" />
  <rect x="100" y="5" width="30" height="80" style="stroke: blue;
    fill: none;" />
  <svg x="100px" y="5px" width="30px" height="80px" viewBox= "0 0 50 50"
    preserveAspectRatio= "xMaxYMax meet" >
    <circle cx="25" cy="25" r="25" style="stroke: black;
      fill: none;" />
  </svg>
</svg>
If you try to use a meet  or slice  value  for the preserveAspectRatio
attribute  on an <svg>  nested  inside  another  <svg>  with
preserveAspectRatio="none" , the results  may  surprise  you.  The
aspect ratio of the nested element’s viewport will be evaluated in the
squished  or stretched  coordinates  of the parent  SVG,  possibly  re‐
sulting  in an image  that is both  squished  and cropped  or shrunk
to fit.
38 | Chapter 3: Coordinates