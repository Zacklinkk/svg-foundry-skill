CHAPTER 12
Animating SVG
Up to this point, all the images you have seen are static images; once constructed, they
never change. In this chapter, we will examine two of three methods of making graphic
images move. The first method, SMIL-based animation, should be used for movement
that is a fundamental part of what the graphic represents, and for which the motion can
be defined ahead of time. CSS animations should be used for stylistic effects and simple
feedback (like highlighting an element on focus/hover). Scripting should be used for
more complex interaction, and it is covered in Chapter 13 .
In Chapter 11 , we suggested that filters should be used as a means to enhance a graphic’s
message, not as an end in themselves. This suggestion is even more crucial with ani‐
mation. Drunk with the power of animation, you will be tempted to turn your every
graphic into an all-dancing, all-singing, Broadway spectacular. As long as your goal is
experimentation, this is fine. If your goal is to convey a message, however, nothing is
worse than gratuitous use or overuse of animation. Let me state this clearly: nobody
except the company CEO is interested in repeated viewing of a spinning, flashing, color-
changing, strobe-lit version of the company logo.
In this chapter, the message is the animation, so most of the examples will be remarkably
free of any content while, of course, avoiding gratuitous and overwrought animation as
much as possible.
Internet  Explorer  browsers  (through  version  11, the latest  at the time
of writing) do not support either the SMIL-based animation or CSS
animation applied to SVG elements. There are JavaScript-based sol‐
utions  such  as SMILscript  and FakeSMILe  that can convert  SMIL-
based animation into scripted animation for Internet Explorer.
191

Animation Basics
The animation features of SVG are based on the World Wide Web Consortium’s
Synchronized Multimedia Integration Language Level 3 (SMIL3) specification . In this
system, you specify the starting and ending values of the attribute, color, motion, or
transformation you wish to animate; the time at which the animation should begin; and
the duration of the animation. Example 12-1  gives the basic code.
Example 12-1. The incredible shrinking rectangle
http://oreillymedia.github.io/svg-essentials-examples/ch12/simple_animation.html
<rect x="10" y="10" width="200" height="20" stroke="black" fill="none">
  <animate
    attributeName= "width"
    attributeType= "XML"
    from="200" to="20"
    begin="0s" dur="5s"
    fill="freeze"  />
</rect>
The first thing to notice is that the <rect>  element is no longer an empty element; the
animation is contained within the element.
The <animate>  element specifies the following:
•The attributeName  whose value should change over time; in this case, width .
•The attributeType . The width  attribute is an XML attribute.  The other common
value of attributeType  is CSS, indicating that the property you want to change is
a CSS property. If you omit this specification, the default value of auto  is used; it
searches through CSS properties first and then XML attributes.
•The starting ( from ) and ending ( to) values for the attribute. In this example, the
starting value is 200, and the ending value is 20. The from  value is optional; if you
leave it out, the starting value is whatever was specified in the parent element.There
is also a by attribute, which you may use instead of to; it is an offset added to the
starting from  value; the result is the ending value.
•The beginning and duration times for the animation. In this example, time is meas‐
ured in seconds, specified by the s after the number. Other ways to define time are
described in the next section, “How Time Is Measured” on page 194 .
•What to do when the animation finishes. In this example, after the 5-second dura‐
tion, the attribute will “freeze” at the to value. This is the SMIL fill  attribute, which
tells the animation engine how to fill up the remaining time. Don’t confuse it with
SVG’s fill  attribute, which tells SVG how to paint an object. If you remove this
192 | Chapter 12: Animating SVG

line, the default value ( remove ) will return the width  attribute to its original value
of 200 after the 5-second animation has finished.
Figures 12-1  and 12-2  show the beginning and ending stages of the animation. They
can’t do justice to the actual effect, so we strongly recommend you try it out within your
browser.
Figure 12-1. Beginning of animation
Figure 12-2. Ending of animation
Example 12-2  is a bit more ambitious. It starts with a 20 by 20 green square that will
grow to 250 by 200 over the space of 8 seconds. For the first 3 seconds, the opacity of
the green will increase, and then decrease for the next 3 seconds. Note that fill-opacity
is referred to with attributeType="CSS"  because it was set in a style .
Example 12-2. Multiple animations on a single object
http://oreillymedia.github.io/svg-essentials-examples/ch12/multiple_animation.html
<rect x="10" y="10" width="20" height="20"
  style="stroke: black; fill: green; style: fill-opacity: 0.25;" >
  <animate  attributeName= "width" attributeType= "XML"
    from="20" to="200" begin="0s" dur="8s" fill="freeze" />
  <animate  attributeName= "height"  attributeType= "XML"
    from="20" to="150" begin="0s" dur="8s" fill="freeze" />
  <animate  attributeName= "fill-opacity"  attributeType= "CSS"
    from="0.25" to="1" begin="0s" dur="3s" fill="freeze" />
  <animate  attributeName= "fill-opacity"  attributeType= "CSS"
    from="1" to="0.25" begin="3s" dur="3s" fill="freeze" />
</rect>
The last simple example, Example 12-3 , animates a square and a circle. The square will
expand from 20 by 20 to 120 by 120 over the space of 8 seconds. Two seconds after the
beginning of the animation, the circle’s radius will start expanding from 20 to 50 over
the space of four seconds. Figure 12-3  shows a combined screenshot of the animation
at four times: 0 seconds, when the animation begins; 2 seconds, when the circle starts
to grow; 6 seconds, when the circle finishes growing; and 8 seconds, when the animation
is finished.
Animation Basics | 193

Example 12-3. Simple animation of multiple objects
http://oreillymedia.github.io/svg-essentials-examples/ch12/multiple_animation2.html
<rect x="10" y="10" width="20" height="20"
  style="stroke: black; fill: #cfc;" >
  <animate  attributeName= "width" attributeType= "XML"
     begin="0s" dur="8s" from="20" to="120" fill="freeze" />
  <animate  attributeName= "height"  attributeType= "XML"
     begin="0s" dur="8s" from="20" to="120" fill="freeze" />
</rect>
<circle cx="70" cy="70" r="20"
  style="fill: #ccf; stroke: black;" >
  <animate  attributeName= "r" attributeType= "XML"
    begin="2s" dur="4s" from="20" to="50" fill="freeze" />
</circle>
Figure 12-3. Stages of multiobject animation
How Time Is Measured
SVG’s animation clock starts ticking when the SVG has finished loading, and it stops
ticking when the user leaves the page. Y ou may specify a beginning or duration for a
particular animation segment as a numeric value in one of these ways:
•A full clock value in hours, minutes, and seconds ( 1:20:23 ).
•A partial clock value in minutes and seconds ( 02:15 ).
•A time value followed by an abbreviation that is one of h (hours), min (minutes), s
(seconds), or ms (milliseconds), for example, dur="3.5s"  begin="1min" . Y ou may
not put any whitespace between the value and the unit.
If no unit is specified, the default is seconds.
Synchronizing Animation
Instead of defining each animation’s start time as the document  loading time, you can
tie an animation’s beginning time to the beginning or end of another animation.
194 | Chapter 12: Animating SVG

Example 12-4  animates two circles; the second one will start expanding as soon as the
first one has stopped shrinking. Figure 12-4  shows the important stages of the
animation.
Example 12-4. Synchronization of animations
<circle cx="60" cy="60" r="30" style="fill: #f9f; stroke: gray;" >
  <animate  id="c1" attributeName= "r" attributeType= "XML"
    begin="0s" dur="4s" from="30" to="10" fill="freeze" />
</circle>
<circle cx="120" cy="60" r="10" style="fill: #9f9; stroke: gray;" >
  <animate  attributeName= "r" attributeType= "XML"
    begin="c1.end"  dur="4s" from="10" to="30" fill="freeze" />
</circle>
Figure 12-4. Stages of synchronized animations
It is also possible to add an offset to a synchronization. To make an animation start 2
seconds after another animation, you would use a construction of the form
begin="otherAnim.end+2s" . (Y ou may add whitespace around the plus sign.) In
Example 12-5 , the second circle begins to grow 1 1/4 seconds after the first circle begins
shrinking.
Example 12-5. Synchronization of animations with offsets
http://oreillymedia.github.io/svg-essentials-examples/ch12/sync_with_offset.html
<circle cx="60" cy="60" r="30" style="fill: #f9f; stroke: gray;" >
  <animate  id="c1" attributeName= "r" attributeType= "XML"
    begin="0s" dur="4s" from="30" to="10" fill="freeze" />
</circle>
<circle cx="120" cy="60" r="10" style="fill: #9f9; stroke: gray;" >
  <animate  attributeName= "r" attributeType= "XML"
    begin="c1.begin+1.25s"  dur="4s" from="10" to="30" fill="freeze" />
</circle>
Now that you know about synchronizing animations, we can introduce the end at‐
tribute,  which sets an end time for an animation. This is not a substitute for the dur
attribute! The following animation will start 6 seconds after the page loads. It will last
for 12 seconds or until an animation named otherAnim  ends, whichever comes first:
Synchronizing Animation | 195

<animate attributeName="width" attributeType="XML"
  begin="6s" dur="12s" end="otherAnim.end"
  from="10" to="100" fill="freeze"/>
Y ou can, of course, set the value of end to a specific time; this is useful for halting an
animation partway through so you can see if everything is in the right place. This is how
we were able to create Figure 12-3 . The following animation starts at 5 seconds and
should last for 10 seconds, but is halted 9 seconds after the document loads (4 seconds
after the animation starts). The animation is stopped 40% of the way through, so the
width will freeze at a value of 140 (40% of the distance from 100 to 200):
<animate attributeName="width" attributeType="XML"
    begin="5s" dur="10s" end="9s"
    from="100" to="200" fill="freeze"/>
Repeated Action
The animations so far occur exactly once; fill  is set to freeze  to keep the final stage
of the animation. If you want to have the object return to its pre-animation state, omit
the attribute. (This is equivalent to setting fill  to the default value of remove .)
Two other attributes allow you to repeat an animation. The first of them,
repeatCount , is set to an integer value telling how many times you want a particular
animation to repeat. The second, repeatDur , is set to a time telling how long the repe‐
tition should last. If you want an animation to repeat until the user leaves the page, set
either repeatCount  or repeatDur  to the value indefinite . Y ou will usually use only
one of the two, not both. If you do specify both repeatCount  and repeatDur , the one
that specifies the end time that occurs first will be used.
The animation in Example 12-6  shows two circles. The upper circle moves from left to
right in two repetitions of 5 seconds each. The second circle moves from right to left
for a total of 8 seconds.
Example 12-6. Example of repeated animation
http://oreillymedia.github.io/svg-essentials-examples/ch12/repeated_action.html
<circle cx="60" cy="60" r="30" style="fill: none; stroke: red;" >
  <animate  attributeName= "cx" attributeType= "XML"
    begin="0s" dur="5s" repeatCount= "2"
    from="60" to="260" fill="freeze" />
</circle>
<circle cx="260" cy="90" r="30" style="fill: #ccf; stroke: black;" >
  <animate  attributeName= "cx" attributeType= "XML"
    begin="0s" dur="5s" repeatDur= "8s"
    from="260" to="60" fill="freeze" />
</circle>
196 | Chapter 12: Animating SVG

1.The color change is affected by the color-interpolation  property, as described in “Defining the Color
Space” on page 169 . The default interpolation, sRGB , usually produces pleasant results.Just as it is possible to synchronize an animation with the beginning or ending of another
animation, you can tie the start of one animation to the start of a specific repetition of
another animation. Y ou give the first animation an id, and then set the begin  of the
second animation to id.repeat( count), where count  is a number beginning at 0 for
the first repetition. Example 12-7  shows an upper circle moving from left to right three
times, requiring 5 seconds for each repetition. The lower square will go right to left only
once, and will not begin until halfway through the second repetition.
Example 12-7. Synchronizing an animation with a repetition
http://oreillymedia.github.io/svg-essentials-examples/ch12/sync_repetition.html
<circle cx="60" cy="60" r="15"
  style="fill: none; stroke: red;" >
  <animate  id="circleAnim"  attributeName= "cx" attributeType= "XML"
    begin="0s" dur="5s" repeatCount= "3"
    from="60" to="260" fill="freeze" />
</circle>
<rect x="230" y="80" width="30" height="30"
  style="fill: #ccf; stroke: black;" >
  <animate  attributeName= "x" attributeType= "XML"
    begin="circleAnim.repeat(1)+2.5s"  dur="5s"
    from="230" to="30" fill="freeze" />
</rect>
Animating Complex Attributes
Animation is not limited to simple numbers and lengths.  Y ou can animate nearly any
attribute or style where you can calculate a smooth transition between two values.
To animate a color, simply make the from  and to attributes valid color values, as de‐
scribed in Chapter 4 , in “Stroke Color” on page 41. The color is treated as a vector of
three numbers for the calculations; the R, G, and B values will each transition from their
value in one color to the other.1 Example 12-8  animates the fill and stroke colors of a
circle, changing the fill from light yellow to red, and the gray outline to blue. Both
animations start 2 seconds after the page loads; this gives you time to see the original
colors.
Example 12-8. Example of animating color
http://oreillymedia.github.io/svg-essentials-examples/ch12/animate_color.html
<circle cx="60" cy="60" r="30"
        style="fill: #ff9; stroke: gray; stroke-width: 10;" >
Animating Complex Attributes | 197

  <animate  attributeName= "fill"
    begin="2s" dur="4s" from="#ff9" to="red" fill="freeze" />
  <animate  attributeName= "stroke"
    begin="2s" dur="4s" from="gray" to="blue" fill="freeze" />
</circle>
Y ou can also animate attributes that are lists of numbers, so long as the number  of
numbers in the list does not change; each value in the list is transitioned separately. That
means you can animate path data or a polygon’s points, so long as you maintain the
number of points and the types of path segments; Example 12-9  shows animations of
both a <polygon>  and a <path> .
Example 12-9. Example of animating path and polygon
http://oreillymedia.github.io/svg-essentials-examples/ch12/animate_path_poly.html
<polygon  points="30 30 70 30 90 70 10 70"
  style="fill:#fcc; stroke:black" >
  <animate  id="animation"
    attributeName= "points"
    attributeType= "XML"
    to="50 30 70 50 50 90 30 50"
    begin="0s" dur="5s" fill="freeze"  />
</polygon>
<path d="M15 50 Q 40 15, 50 50, 65 32, 100 40"
  style="fill:none; stroke: black"  transform= "translate(0,50)" >
  <animate  attributeName= "d"
    attributeType= "XML"
    to="M50 15 Q 15 40, 50 50, 32 65, 40 100"
    begin="0s" dur="5s" fill="freeze" />
</path>
Specifying Multiple Values
All the animation elements presented so far give a starting ( from  or default) value and
an ending ( to) value, and let the computer calculate how to get from one to the other.
It is possible to give specific intermediary values for an animation, allowing a single
<animate>  element to define complex sequences of changes.  Instead of animating the
color in Example 12-8  from light yellow to red, you can give a semicolon-separated list
of values  that the animation will use over the duration. Example 12-10  shows a circle
that animates color using values of light yellow, light blue, pink, and light green.
Example 12-10. Animating color by specific values
http://oreillymedia.github.io/svg-essentials-examples/ch12/animating_values.html
<circle cx="50" cy="50" r="30"
  style="fill: #ff9; stroke:black;" >
  <animate  attributeName= "fill"
198 | Chapter 12: Animating SVG

    begin="2s" dur="4s" values="#ff9;#99f;#f99;#9f9"
    fill="freeze" />
</circle>
The values  attribute can also be used to make repeating animations alternate back  and
forth between two values, using the format values=" start; end; start;".
Timing of Multistage Animations
When an animation has multiple values, the duration of the animation (the dur at‐
tribute) is the time it takes to cycle through all the values. By default, the duration of
the animation is divided into equal time periods for each transition. Example 12-10  used
four color values, so there are three color transitions; the total duration is 4 seconds, so
each transition lasts 4/3 of a second.
The keyTimes  attribute allows you to divide the duration in other ways. The format of
keyTimes  is also a semicolon-separated list, and it must have the same number of entries
as values . The first entry is always 0 and the last is always 1; the intermediary times are
expressed as decimal numbers between 0 and 1, representing the proportion of the
animation duration that should pass by the time the corresponding value is reached.
More options for controlling timing are created with the calcMode  attribute. There are
four possible values for calcMode :
paced
The SVG viewer will calculate the distance between subsequent values and divide
up the duration so that the rate of change is constant (any keyTimes  attribute will
be ignored). Paced animation mode works with colors and simple numbers or
lengths, but is not possible for lists of points or path data.
linear
The default for <animate>  elements; each transition will proceed at a steady pace,
but the time alotted to each transition is equal (if keyTimes  aren’t specified) or is
determined by keyTimes .
discrete
The animation will jump from one value to the next without transitioning. If you
animate a property that doesn’t support transitions (like font-family ), discrete
mode will be used automatically.
spline
The animation will accelerate and decelerate according to the values of the
keySplines  attribute; you can read more about it in the SVG specifications .
Timing of Multistage Animations | 199

The <set> Element
All of these animations have modified values over time. Sometimes, particularly for
non-numeric attributes or properties that can’t transition, you simply want to change
the value at a chosen point in the animation sequence.
For example, you might want an initially invisible text item to become visible at a certain
time; there’s no real need for both a from  and to. Thus, SVG has the convenient short‐
hand of the <set>  element, which needs only a to attribute and the proper timing
information. Example 12-11  shrinks a circle down to 0, then reveals text 1/2 second
after the circle is gone.
Example 12-11. Example of set element
http://oreillymedia.github.io/svg-essentials-examples/ch12/animation_set.html
<circle cx="60" cy="60" r="30" style="fill: #ff9; stroke: gray;" >
  <animate  id="c1" attributeName= "r" attributeType= "XML"
    begin="0s" dur="4s" from="30" to="0" fill="freeze" />
</circle>
<text text-anchor= "middle"  x="60" y="60" style="visibility: hidden;" >
  <set attributeName= "visibility"  attributeType= "CSS"
    to="visible"  begin="4.5s" dur="1s" fill="freeze" />
  All gone!
</text>
The <animateTransform> Element
The <animate>  element doesn’t work with rotate, translate, scale, or skew transforma‐
tions because they’re all “wrapped up” inside the transform  attribute. This is where the
<animateTransform>  element comes to the rescue. Y ou set its attributeName  to
transform . The type  attribute’s value then specifies the transformation whose values
should change (one of translate , scale , rotate , skewX , or skewY ). The from  and to
values are specified as appropriate for the transform you’re animating. As of this writing,
most implementations currently support only <animateTransform>  on the XML
transform  attribute rather than the CSS3 transformations.
Example 12-12  stretches a rectangle from normal scale to a scale of four times in the
horizontal direction and two times in the vertical direction. Note that the rectangle is
centered around the origin so it doesn’t move as it scales; it is inside a <g> so it can be
translated to a more convenient location. Figure 12-5  shows the beginning and end of
the animation.
200 | Chapter 12: Animating SVG

Example 12-12. Example of animateTransform
http://oreillymedia.github.io/svg-essentials-examples/ch12/animate_transform.html
<g transform= "translate(100,60)" >
  <rect x="-10" y="-10" width="20" height="20"
    style="fill: #ff9; stroke: black;" >
    <animateTransform  attributeType= "XML"
      attributeName= "transform"  type="scale"
      from="1" to="4 2"
      begin="0s" dur="4s" fill="freeze" />
  </rect>
</g>
Figure 12-5. animateTransform—before and after
If you intend to animate more than one transformation, you must use the additive
attribute. The default value of additive  is replace , which replaces the specified trans‐
formation in the object being animated. This won’t work in a series of transformations,
because the second animation would override the first one. By setting additive  to
sum, SVG will accumulate the transformations. Example 12-13  stretches and rotates the
rectangle. The before and after pictures are in Figure 12-6 .
Example 12-13. Example of multiple animateTransform elements
http://oreillymedia.github.io/svg-essentials-examples/ch12/additive_transform.html
<rect x="-10" y="-10" width="20" height="20"
  style="fill: #ff9; stroke: black;" >
  <animateTransform  attributeName= "transform"  attributeType= "XML"
    type="scale" from="1" to="4 2"
    additive= "sum" begin="0s" dur="4s" fill="freeze" />
  <animateTransform  attributeName= "transform"  attributeType= "XML"
    type="rotate"  from="0" to="45"
    additive= "sum" begin="0s" dur="4s" fill="freeze" />
</rect>
Figure 12-6. Multiple animateTransforms—before and after
The <animateTransform> Element | 201

Y ou can also use additive="sum"  to combine  the effects  of anima‐
tion elements that control numerical and color attributes. If the ani‐
mations  are specified  using  to, adding  them  together  causes  subse‐
quent animations to use the current value of the previous animation
as their  starting-point.  If the animations  use the by attribute  to de‐
fine their effects, or use both  from  and to, then the final value will be
the sum of all the individual changes.
The <animateMotion> Element
Y ou can cause an object to animate along a straight-line path by using translate  with
the <animateTransform>  element.  However, if you wanted to move the object in a more
complicated pattern, you would need an extended series of transform animations timed
to start one after another. The <animateMotion>  element makes it easier to animate an
object along an arbitrary path, whether a straight line or a series of overlapping loops.
If you want to use <animateMotion>  for straight-line motion,  you simply set the from
and to attributes, assigning them each a pair of ( x,y) coordinates. The coordinates
specify the position where the (0,0) point of the shape’s coordinate system will be moved,
similar to how translate(x,y)  works. Example 12-14  moves a grouped circle and
rectangle from (0,0) to (60,30).
Example 12-14. Animation along a linear path
http://oreillymedia.github.io/svg-essentials-examples/ch12/linear_animateMotion.html
<g>
  <rect x="0" y="0" width="30" height="30" style="fill: #ccc;" />
  <circle cx="30" cy="30" r="15" style="fill: #cfc; stroke: green;" />
  <animateMotion  from="0,0" to="60,30" dur="4s" fill="freeze" />
</g>
Multiple points can be specified with values , but the motion will still be a series of
straight lines. If you want a more complex path to follow, use the path  attribute instead;
its value is in the same format as the d attribute in the <path>  element. Example 12-15 ,
adapted from the SVG specification, animates a triangle along a cubic Bézier curve path.
Example 12-15. Animation along a complex path
http://oreillymedia.github.io/svg-essentials-examples/ch12/
complex_animate_motion.html
<!-- show the path along which the triangle will move -->
<path d="M50,125 C 100,25 150,225, 200, 125"
  style="fill: none; stroke: blue;" />
<!-- Triangle to be moved along the motion path.
   It is defined with an upright orientation with the base of
202 | Chapter 12: Animating SVG

   the triangle centered horizontally just above the origin. -->
<path d="M-10,-3 L10,-3 L0,-25z"  style="fill: yellow; stroke: red;" >
  <animateMotion
    path="M50,125 C 100,25 150,225, 200, 125"
    dur="6s" fill="freeze" />
</path>
As you can see in Figure 12-7 , the triangle stays upright throughout its entire path.
Figure 12-7. animateMotion along a complex path
If you would prefer that the object tilt so its x-axis is always parallel to the slope of the
path,  just add the rotate  attribute with a value of auto  to the <animateMotion>  element.
Example 12-16  shows the SVG, and Figure 12-8  shows screenshots taken at various
stages of the animation.
Example 12-16. Animation along a complex path with auto-rotation
http://oreillymedia.github.io/svg-essentials-examples/ch12/
animate_motion_rotate.html
<!-- show the path along which the triangle will move -->
<path d="M50,125 C 100,25 150,225, 200, 125"
  style="fill: none; stroke: blue;" />
<!-- Triangle to be moved along the motion path.
   It is defined with an upright orientation with the base of
   the triangle centered horizontally just above the origin. -->
<path d="M-10,-3 L10,-3 L0,-25z"  style="fill: yellow; stroke: red;"  >
  <animateMotion
    path="M50,125 C 100,25 150,225, 200, 125"
    rotate="auto"
    dur="6s" fill="freeze" />
</path>
Put simply, when you leave off the rotate  attribute, you get the default value of 0, and
the object acts like a hot-air balloon floating along the path. If you set rotate  to auto ,
the object acts like a car on a roller coaster, tilting up and down as the path does.
The <animateMotion> Element | 203

2.There’s one exception to the distance rule for paced <animateMotion> : if your path has any moveto com‐
mands, these are counted as zero distance, meaning your object will immediately jump from the end of one
subpath to the beginning of the next.Y ou can also set rotate  to a numeric value, which will set the rotation of the object
throughout the animation. Thus, if you wanted an object rotated 45 degrees no matter
what direction the path took, you’d use rotate="45" .
Figure 12-8. animateMotion along a complex path with auto-rotation
Example 12-16  drew the path in blue so it was visible, and then duplicated the path in
the <animateMotion>  element. Y ou can avoid this duplication by  adding an <mpath>
element within the <animateMotion>  element. The <mpath>  will contain an xlink:href
attribute that references the path you want to use. This also comes in handy when you
have one path you wish to use to animate multiple objects. Here’s the preceding example,
rewritten as Example 12-17 , using <mpath> .
Example 12-17. Motion along a complex path using mpath
<path id="cubicCurve"  d="M50,125 C 100,25 150,225, 200, 125"
  style="fill: none; stroke: blue;" />
<path d="M-10,-3 L10,-3 L0,-25z"  style="fill: yellow; stroke: red;"  >
  <animateMotion  dur="6s" rotate="auto" fill="freeze" >
    <mpath xlink:href= "#cubicCurve" />
  </animateMotion>
</path>
Specifying Key Points and Times for Motion
In Example 12-16 , the triangle moved at a steady pace. This paced  animation  is the
default for <animateMotion>  (equivalent to calcMode="paced" ); the amount of time it
takes to move between subsequent points is directly proportional to the distance be‐
tween them.2
In “Timing of Multistage Animations” on page 199, we introduced the keyTimes  at‐
tribute,  which can be used to control the rate an animation transitions between different
values. Y ou can use keyTimes  for motion animation too, but if you’re using a path instead
204 | Chapter 12: Animating SVG

of a values  list to define the motion, you need to specify key points along the path using
(you guessed it) the keyPoints  attribute.
Like keyTimes , keyPoints  is a semicolon-separated list of decimal numbers. Each point
represents how far along the path the object should have moved at the corresponding
entry in the keyTimes  list. Just as keyTimes  ranges from 0 (beginning of animation) to
1 (end of animation), keyPoints  ranges from 0 (beginning of the path) to 1 (end of the
path). Example 12-18  shows the triangle moving more slowly as it goes uphill.
Although keyTimes  must be given in order from 0 to 1, keyPoints  may start or end in
the middle of the path and go in either direction. However, the keyPoints  and keyTimes
lists must have the same number of entries, and you must set calcMode="linear"  (or
"spline" , but that’s beyond the scope of this book).
Example 12-18. Variable speed motion along a path using keyPoints and keyTimes
http://oreillymedia.github.io/svg-essentials-examples/ch12/key_points.html
<path d="M-10,-3 L10,-3 L0,-25z"  style="fill: yellow; stroke: red;"  >
  <animateMotion
    path="M50,125 C 100,25 150,225, 200, 125"
    rotate="auto"
    keyPoints= "0;0.2;0.8;1"
    keyTimes= "0;0.33;0.66;1"
    calcMode= "linear"
    dur="6s" fill="freeze" />
</path>
Animating SVG with CSS
Modern browsers allow you to animate both HTML and SVG elements with CSS.  This
is a two-stage process. In the first stage, you select the element you want to animate and
set the properties of the animation as a whole. In the second stage, you say which prop‐
erties of the selected element are to change, and at what stages of the animation; these
are defined in a @keyframes  specifier.
Consider the following task: display a green star that fades to a white interior as its
border becomes thicker; the effect is that the color is being “drained” into the border.
Here is the SVG for the star:
<svg width="200" height="200" viewBox= "0 0 200 200" >
  <defs>
    <g id="starDef" >
      <path d="M 38.042 -12.361 9.405 -12.944 -0.000 -40.000
        -9.405 -12.944 -38.042 -12.361 -15.217 4.944
        -23.511 32.361 0.000 16.000 23.511 32.361 15.217 4.944 Z" />
    </g>
  </defs>
Animating SVG with CSS | 205

  <use id="star" class="starStyle"  xlink:href= "#starDef"
    transform= "translate(100, 100)"
    style="fill: #008000; stroke: #008000" />
</svg>
Animation Properties
These are properties you’ll set in the CSS for the element being animated:
•animation-name  is the name of @keyframes  specifier.
•animation-duration  determines how long the animation should last; this is a
number followed by a time unit as described in “How Time Is Measured” on page
194.
•animation-timing-function  tells how intermediate values are calculated (e.g.,
should an animation ease in or out, or work in discrete steps).
•animation-iteration-count  tells how many times to repeat an animation, with
infinite  looping continuously.
•animation-direction  determines whether an animation should go in a forward
or reverse direction, and whether it should alternate between the two or not.
•animation-play-state  can be set to running  or paused .
•animation-delay  tells how long to wait to start the animation after the style is
applied.
•animation-fill-mode  tells what properties to use when the animation is not exe‐
cuting. This can be forwards  (applies properties for the time the animation ended),
backwards  (applies properties for the time the animation began), or both .
In order  to use these  properties  in a WebKit-based  browser,  you must,
as of this writing,  prefix  them  with  -webkit- ; thus,  for example,
-webkit-animation-name  or -webkit-animation-duration .
Example 12-19  shows the CSS to set up the star animation. It will be repeated four times,
with 2 seconds per iteration.
Example 12-19. CSS setup for animation
.starStyle  {
  animation-name : starAnim ;
  animation-duration : 2s;
  animation-iteration-count : 4;
  animation-direction : alternate ;
  animation-timing-function : ease;
206 | Chapter 12: Animating SVG

  animation-play-state : running;
}
Setting Animation Key Frames
Y ou set the properties to change at each stage of the animation by using the @keyframes
media type, followed by the name of the animation being controlled. Inside the
@keyframes , you list keyframe selectors, which are percentages that tell when properties
should change. For each of those selectors, list the properties and values that the ani‐
mation should take on. Example 12-20  shows the key frames for the star animation. For
WebKit-based browsers, use @-webkit-keyframes . Figure 12-9  shows three stages of
the animation.
Example 12-20. Key frame specification in CSS
http://oreillymedia.github.io/svg-essentials-examples/ch12/svg_css_anim1.html
@keyframes  starAnim  {
  0% {
    fill-opacity: 1.0;
    stroke-width : 0;
  }
  100% {
    fill-opacity: 0;
    stroke-width : 6;
  }
}
Y ou can use from  and to as synonyms for 0% and 100% .
Figure 12-9. Animation at beginning, middle, and end
Animating Movement with CSS
If you want to use pure CSS to animate movement, you can’t use the transform  at‐
tribute.  Instead, you have to use CSS styles to translate, rotate, and scale your SVG.
Animating SVG with CSS | 207

Luckily, the CSS transform  property’s value looks very much like the SVG transform
attribute, though there are differences, as noted in “CSS Transformations and SVG”  on
page 83 . If, for example, you want an SVG element to be translated to (100,50), scaled
by a factor of 1.5, and then rotated 90 degrees, the property would be as follows:
transform : translate (100px, 50px) scale(1.5) rotate(90deg);
Example 12-21  shows the key frames required for making the star move upward and
rotate, and then descend to its starting point. Because the 100% key frame doesn’t specify
a translate , the star will return to its original position (specified in the SVG). This is
why the 50% and 80% key frames, in addition to the 20% key frame, must specify the
translate  so that the star does not move vertically during that portion of the
animation.
Example 12-21. Specifying transformations in CSS
@keyframes  starAnim   {
  0% {
    fill-opacity: 1.0;
    stroke-width : 0;
  }
  20% {
    transform : translate (100px, 50px)
  }
  50% {
    transform : translate (100px, 50px) rotate(180deg)
  }
  80% {
    transform : translate (100px, 50px) rotate(360deg)
  }
  100% {
    fill-opacity: 0.0;
    stroke-width : 6;
  }
}
Create your own keyframes, and experiment with the timing properties, with the online
example:
http://oreillymedia.github.io/svg-essentials-examples/ch12/svg_css_anim2.html
At the time of this writing, browsers tend to be buggy and inconsis‐
tent when applying CSS animations and transitions to SVG graph‐
ics that are duplicated by <use>  elements.
208 | Chapter 12: Animating SVG