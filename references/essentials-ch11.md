CHAPTER 11
Filters
The preceding chapters have given you a basis for creating graphics that convey infor‐
mation with great precision and detail. If you’re going on a spring picnic, you want a
precise map. When you look in the newspaper for the graphics that describe the weather
forecast, you want “just the facts. ”
If you’re asked later to describe the day of the picnic, nobody wants a crisp recitation of
meteorological statistics. Similarly, nobody wants to see a graphic of a spring flower
composed of pure vectors; Figure 11-1  fails totally to convey any warmth or charm.
Figure 11-1. Flower composed of plain vectors
Graphics are often designed to evoke feelings or moods as much as they are meant to
convey information. Artists who work with bitmap graphics—and therefore work with
the appearance  of an object instead of its geometrical definition—have many tools at
their disposal to add such effects. They can produce blurred shadows, selectively thicken
or thin lines, add textures to part of the drawing, or make an object appear to be em‐
bossed or beveled.
How Filters Work
Although SVG is not a bitmap description language,  it still lets you use some of these
same tools. When an SVG viewer program processes a graphic object, it will render the
155

object to some bitmapped output device; at some point, the program will convert the
object’s description into the appropriate set of pixels that appear on the output device.
Now let’s say that you use the SVG <filter>  element to specify a set of operations (also
called primitives ) that display an object with a blurred shadow offset slightly to the side,
and attach that filter to an object:
<filter id="drop-shadow" >
  <!-- filter operations go here -->
</filter>
<g id="spring-flower"
  style="filter: url(#drop-shadow);" />
  <!-- drawing of flower goes here -->
</g>
Because the flower uses a filter in its presentation style, SVG will not render the flower
directly to the final graphic. Instead, SVG will render the flower’s pixels into a temporary
bitmap.  The operations specified by the filter will be applied to that temporary area and
their result will be rendered into the final graphic.
The size of the temporary bitmap will by default depend on the resolution and size of
the display screen on which the image will be rendered. This means that some filter
effects can have different appearances at different sizes, even if all the SVG code is the
same. The specifications define attributes to control the effective resolution of the filter
effects , but these are not consistently implemented in SVG viewers and are not discussed
here.
Creating a Drop Shadow
Example 5-8  created a drop shadow by offsetting a gray ellipse underneath a colored
ellipse. It worked, but it wasn’t elegant. Let’s investigate a way to create a better-looking
drop shadow with a filter.
Establishing the Filter’s Bounds
The <filter>  element has attributes that describe the clipping region for a filter.  Y ou
specify an x, y, width , and height  in terms of the percentage of the filtered object’s
bounding box. (That is the default.) Any portion of the resulting output that’s outside
the bounds will not be displayed. If you are intending to apply a filter to many objects,
you may want to omit these attributes altogether and take the default values of x equal
to –10%, y equal to –10%, width  equal to 120%, and height  equal to 120%. This gives
extra space for filters—such as the drop shadow that we’re constructing—that produce
output larger than their input.
156 | Chapter 11: Filters

These attributes are in terms of the filtered object’s bounding box; specifically,
filterUnits  has a value of objectBoundingBox  by default.  If you wish to specify
boundaries in user units, then set the attribute’s value to userSpaceOnUse .
Y ou can also specify the units used in the filter primitives with the primitiveUnits
attribute. Its default value is userSpaceOnUse , but if you set it to objectBoundingBox ,
then you can express units as a percent of the graphic’s size.
Using <feGaussianBlur> for a Drop Shadow
Between the beginning and ending <filter>  tags are the filter primitives  that perform
the operations you desire. Each primitive has one or more inputs, and exactly one out‐
put. An input can be the original graphic, specified as SourceGraphic , the alpha (opa‐
queness) channel of the graphic, specified as SourceAlpha , or the output of a previous
filtering primitive. The alpha source is useful when you’re interested in only the shape
of the graphic, regardless of color; it avoids the interactions of alpha and color, as de‐
scribed in Chapter 10 , in “Masking” on page 149 .
Example 11-1  is a first attempt to produce a drop shadow on the flower, using the
<feGaussianBlur>  filter  primitive. We specify SourceAlpha  as its input (the in at‐
tribute), and the amount of blur with the stdDeviation  attribute. The larger this num‐
ber, the greater the blur. If you give two numbers separated by whitespace as the value
for stdDeviation , the first number is taken as the blur in the x-direction and the second
as the blur in the y-direction.
Example 11-1. First attempt to produce a drop shadow
<defs>
  <filter id="drop-shadow" >
      <feGaussianBlur  in="SourceAlpha"  stdDeviation= "2"/>
  </filter>
</defs>
<g id="flower"  filter="url(#drop-shadow)" >
    <!-- drawing here -->
</g>
Figure 11-2  shows the result, which is probably not what you thought it would be.
Don’t be surprised; remember, the filter returns the output, which is a blurred alpha
channel, instead  of the original source graphic. Y ou could get the effect you want by
putting the flower within the <defs>  section of the document and changing the SVG to
read as follows:
<use xlink:href= "#flower"  filter="url(#drop-shadow)"
    transform= "translate(4, 4)" />
<use xlink:href= "#flower" />
Creating a Drop Shadow | 157

Figure 11-2. Result of first attempt at a drop shadow
However, that would require SVG to execute all the elements that make up the flower
twice. Instead, the solution is to add more filter primitives, so that all the work can be
handled during rendering.
Storing, Chaining, and Merging Filter Results
Example 11-2  is the updated filter.
Example 11-2. Improved drop shadow filter
<filter id="drop-shadow" >
  <feGaussianBlur  in="SourceAlpha"  stdDeviation= "2" result="blur" >  
  <feOffset  in="blur"  dx="4" dy="4" result="offsetBlur" >  
  <feMerge>     
        <feMergeNode  in="offsetBlur" >
        <feMergeNode  in="SourceGraphic" />
  </feMerge>
</filter>
The result  attribute specifies that the result of this primitive can be referenced
later by the name blur . This isn’t like an XML id; the name you give is a local
name  that’s  valid  only  for the duration  of the primitives  contained  in the current
<filter> .
The <feOffset>  primitive takes its input, in this case the blur  result from the
Gaussian  blur,  offsets  it by the specified  dx and dy values,  and stores  the resulting
bitmap under the name offsetBlur .
The <feMerge>  primitive  encloses  a list of <feMergeNode>  elements,  each  of
which  specifies  an input.  The inputs  are stacked  one on top of another  in the
order  that they  appear.  In this case,  you want  the offsetBlur  below  the original
SourceGraphic .
Y ou can now refer to this improved drop shadow filter sequence when drawing the
flower, producing a surprisingly pleasant image in Figure 11-3 .
158 | Chapter 11: Filters

<g id="flower"  filter="url(#drop-shadow)" >
    <!-- drawing here -->
</g>
Figure 11-3. Result of improved drop shadow
When  you first start  working  with  filters,  we strongly  recommend
that you do things  in stages,  testing  filters  one at a time.  I created  large
numbers  of stunningly  ugly  results  during  botched  attempts  to dis‐
cover how a filter really works. Y ou probably will too. We’ll just keep
it as our little secret.
Similarly,  when  you first learn  about  filters,  you will be tempted  to
apply as many of them as possible to a drawing, just to see what will
happen.  Y our  purpose  is experimentation,  so go ahead.  Once  you
finish experimenting and begin production work, the purpose of the
filter changes. Filters should support and enhance your message, not
overwhelm  it. Judicious  use of one or two filters  is a buoy;  a flotilla
of filters almost always sinks the message.
Creating a Glowing Shadow
The drop shadow works well on the flower, but looks totally unimpressive when ap‐
plied to text, as you can see in Figure 11-4 .
Figure 11-4. Drop shadow applied to text
The text would look better with a glowing turquoise area surrounding it,  and you can
create this effect with the <feColorMatrix>  primitive to change black to a different
color.
Creating a Glowing Shadow | 159

The <feColorMatrix> Element
The <feColorMatrix>  element allows you to change color values in a very generalized
way. The sequence of primitives used to create a glowing turquoise shadow is shown in
Example 11-3 .
Example 11-3. Glow filter
<filter id="glow">
  <feColorMatrix  type="matrix"
    values=
      "0 0 0 0   0
       0 0 0 0.9 0
       0 0 0 0.9 0
       0 0 0 1   0" />
  <feGaussianBlur  stdDeviation= "2.5"
    result="coloredBlur" />
  <feMerge>
    <feMergeNode  in="coloredBlur" />
    <feMergeNode  in="SourceGraphic" />
  </feMerge>
</filter>
The <feColorMatrix>  is a very versatile primitive, allowing you to modify any of the
color or alpha values of a pixel. When the type  attribute equals matrix , you must set
the value  to a series of 20 numbers describing the transformation. The 20 numbers are
best understood when written in four rows of five columns each. Each row represents
an algebra equation defining how to calculate the output R, G, B, or A value (in order
by row). The numbers in the row are multiplied by the input pixel’s value for R, G, B,
and A and the constant 1 (in order by column), and then added together to get the
output value. To set up a transformation that paints all opaque areas the same color, you
can ignore the input colors and the constant, and just set the values in the alpha col‐
umn. The model for such a matrix looks like this:
values=
  "0 0 0 red 0
   0 0 0 green 0
   0 0 0 blue 0
   0 0 0 1 0"
where the red, green, and blue values are decimal numbers that usually range from 0 to
1. In Example 11-3 , red is set to to 0, and the green and blue values are set to 0.9, which
will produce a bright cyan color.
Y ou’ll note that the example does not have an in attribute for the input to this primitive;
the default is to use the SourceGraphic . There is also no result  attribute on this prim‐
itive. This means that the color matrix operation’s output is available only as the implicit
input to the next filter primitive. If you use this shortcut, then the next filter primitive
must not have an in attribute.
160 | Chapter 11: Filters

In the example, the result of <feColorMatrix>  is a cyan-colored source. The rest of the
filter uses a Gaussian blur to spread it out; the resulting cyan-colored blur is stored for
future reference as coloredBlur . Finally, <feMerge>  produces the output glow under‐
neath the object in question.
With these two filters, you can create the new, improved Figure 11-5  with SVG like this:
<g id="flower"  style="filter: url(#drop-shadow);" >
  <!-- draw the flower -->
</g>
<text x="120" y="50"
  style="filter: url(#glow); fill: #003333; font-size:18;" ">
Spring <tspan x="120" y="70">Flower</tspan>
</text>
Figure 11-5. Drop shadow and glowing text
More About the <feColorMatrix> Element
The preceding example used the most general kind of color matrix, where you get to
specify any values you wish. There are three other values for the type  attribute. Each of
these “built-in” color matrices accomplishes a particular visual task and has its own way
of specifying values :
hueRotate
The values  is a single number that tells how many degrees the color values should
be rotated. The mathematics used to accomplish this are very similar to those used
in the rotate  transformation as described in “The rotate Transformation”  on page
78 in Chapter 6 . The relation between rotation and resulting color is not at all
obvious, as shown in Figure 11-6 ; you may want to experiment with the online
version:
http://oreillymedia.github.io/svg-essentials-examples/ch11/hue_rotate.html
Creating a Glowing Shadow | 161

Figure 11-6. Result of hueRotate on fully saturated colors
saturate
The values  attribute specifies a single number in the range 0 to 1. The smaller the
number, the more “washed out” the colors will be, as you see in Figure 11-7 . A value
of 0 converts the graphic to black and white. The filter can be used only to de-
saturate (wash out) an image; you cannot increase saturation (to make a normal
image technicolor) with this method.
Test out other saturation values with the online example:
http://oreillymedia.github.io/svg-essentials-examples/ch11/saturate.html
Figure 11-7. Result of saturate on primary and secondary colors
luminanceToAlpha
This filter creates an alpha channel based upon a color’s luminance.  The luminance
is the inherent “brightness” of a color, as described in “Masking”  on page 149 in
Chapter 10 . In Figure 11-8 , the luminance of the colored squares is used as an alpha
channel for solid black squares. The filter discards the color of the original squares,
with the result being solid black with varying opacity levels. The lighter a color, the
less the transparency it confers upon the filtered object. The values  attribute is
ignored for this type .
Figure 11-8. Result of luminanceToAlpha
162 | Chapter 11: Filters

The <feImage> Filter
Up to this point, you have seen only the original  graphic or its alpha channel as input
to a filter. SVG’s <feImage>  element lets you use any JPG, PNG, or SVG file—or an SVG
element with an id attribute—as input to a filter. Example 11-4  imports a picture of the
sky with a cloud in it to use as a background for the picture of the flower.
Example 11-4. Using the feImage element
<defs>
<filter id="sky-shadow"  filterUnits= "objectBoundingBox" >
    <feImage  xlink:href= "sky.jpg"  result="sky"
         x="0" y="0" width="100%" height="100%"
         preserveAspectRatio= "none"/>
    <feGaussianBlur  in="SourceAlpha"  stdDeviation= "2" result="blur"/>
    <feOffset  in="blur" dx="4" dy="4" result="offsetBlur" />
    <feMerge>
        <feMergeNode  in="sky"/>
        <feMergeNode  in="offsetBlur" />
        <feMergeNode  in="SourceGraphic" />
    </feMerge>
</filter>
</defs>
<g id="flower"  style="filter: url(#sky-shadow)" >
    <!-- flower graphic goes here -->
</g>
<!-- show original image -->
<image xlink:href= "sky.jpg"  x="170" y="10"
    width="122" height="104"/>
Figure 11-9  shows the result, with the original picture of the sky shown at right at its
true size. The image is stretched to fit the filter region defined on the <filter>  element
by default (there are no dimensions on the filter, so the default filter region is 10%
padding around the object bounding box). Y ou can set explicit height, width, and x/y
offsets on the <feImage>  element. By default, these measurements are in
userSpaceOnUse  units; however, any percentage values are calculated relative to the
filter region. Y ou can use the primitiveUnits  attribute on the <filter>  element to
switch to objectBoundingBox  units, but this will affect all the elements in the filter.
The <feImage> Filter | 163

Figure 11-9. Result of feImage
In addition  to importing  a complete  image  file, you can use a URL
fragment  in the xlink:href  attribute  to import  part of an SVG
graphic—from a file or from elsewhere in the same SVG—into your
filter. Unfortunately, image fragments in <feImage>  are not yet sup‐
ported  in Mozilla  Firefox.  Y ou can monitor  Bugzilla  bug 455986  to
see if that changes.
The <feComponentTransfer> Filter
The problem with the background is that it is too dark. Using saturate  isn’t the answer;
it changes the intensity of the color, but maintains its brightness (luminance). To lighten
the image, you need to increase the value of each color channel. Y ou could do this with
a custom color matrix, but <feComponentTransfer>  provides an easier and more flex‐
ible way to manipulate each channel (component) separately. It also lets you adjust each
color channel differently, so you can make the blue sky both lighter and less intense by
increasing the level of green and red more than the blue level.
Y ou adjust the levels of red, green, blue, and alpha by placing a <feFuncR> , <feFuncG> ,
<feFuncB> , and <feFuncA>  element inside <feComponentTransfer> . Each of these sub‐
elements may independently specify a type  attribute telling how that particular channel
is to be modified.
To simulate the effect of a brightness control, you specify the linear  function, which
places the current color value C into the formula slope  * C +  intercept . The intercept
provides a “base value” for the result; the slope  is a simple scaling factor. Example 11-5
uses a filter that adds a brightened sky to the flower with the drop shadow. Note that
the red and green channels are adjusted differently than the blue channel. This dra‐
matically brightens the sky in Figure 11-10 .
164 | Chapter 11: Filters

Example 11-5. Changing brightness with feComponentTransfer
http://oreillymedia.github.io/svg-essentials-examples/ch11/linear_transfer.html
<filter id="brightness-shadow"  filterUnits= "objectBoundingBox" >
  <feImage  xlink:href= "sky.jpg"  result="sky"/>
  <feComponentTransfer  in="sky" result="sky">
    <feFuncB  type="linear"  slope="3" intercept= "0"/>
    <feFuncR  type="linear"  slope="1.5" intercept= "0.2"/>
    <feFuncG  type="linear"  slope="1.5" intercept= "0.2"/>
  </feComponentTransfer>
  <feGaussianBlur  in="SourceAlpha"  stdDeviation= "2" result="blur"/>
  <feOffset  in="blur" dx="4" dy="4" result="offsetBlur" />
  <feMerge>
    <feMergeNode  in="sky"/>
    <feMergeNode  in="offsetBlur" />
    <feMergeNode  in="SourceGraphic" />
  </feMerge>
</filter>
Figure 11-10. Result of linear component transfer
A simple linear adjustment will add and multiply the same amount to every color value
within a channel. This is not the case with the gamma  function, which places the current
color value C into the formula amplitude  * Cexponent + offset . The offset  provides a
“base value” for the result; the amplitude  is a simple scaling factor, and exponent  makes
the result a curved line rather than a straight line. Because the color value is always
between 0 and 1, the larger your exponent, the smaller  the modified value will be.
Figure 11-11  shows the curves generated with exponent values of 0.6 (the solid black
line), 0.3 (the dashed line), and 1.66667 (the gray line).
Looking at the dashed line, you can see that a low original color value such as 0.1 will
be boosted to 0.5, a 400% increase. An original value of 0.5, on the other hand, will
increase only 60% to 0.8. The effect is to brighten the image as a whole and to increase
the contrast in dark areas while reducing contrast in light areas. For exponents greater
than 1 (the gray line), the modified values are smaller than the original, darkening the
image while increasing the contrast in bright areas. Note that the solid gray and black
lines are symmetrical around the diagonal: the gamma value of 1.6667 is the inverse of
The <feComponentTransfer> Filter | 165

a gamma of 0.6. In any case, the exponent has no effect when the original value is either
0 or 1.
Figure 11-11. Gamma curve functions
When you specify a gamma filter, you set the amplitude , exponent , and offset  at‐
tributes to correspond to the values in the preceding formula. Example 11-6  uses gamma
correction to adjust the sky. In this particular case, the differences between Figures 11-10
and 11-12  are minor, but there are some images that can be improved much more by
one method than by the other.
Example 11-6. Gamma adjustment with feComponentTransfer
http://oreillymedia.github.io/svg-essentials-examples/ch11/gamma_transfer.html
<feImage  xlink:href= "sky.jpg"  result="sky"/>
<feComponentTransfer  in="sky" result="sky">
  <feFuncB  type="gamma"
    amplitude= "1" exponent= "0.2" offset="0"/>
  <feFuncR  type="gamma"
    amplitude= "1" exponent= "0.707" offset="0"/>
  <feFuncG  type="gamma"
    amplitude= "1" exponent= "0.707" offset="0"/>
</feComponentTransfer>
Figure 11-12. Result of using gamma correction
166 | Chapter 11: Filters

The astute  reader  (that’s  you)  may  have  observed  that both  linear  and
gamma  functions  can produce  color  values  greater  than  1.0. The SVG
specification  says this is not an error;  after  each  filter  primitive,  the
SVG processor will clamp the values to a valid range. Thus, any val‐
ue greater than 1.0 is reduced to 1.0, and any value less than 0 is set
to 0.
<feComponentTransfer>  has other options for the type  attribute. Note that you may
mix and match any of these; you can gamma-correct the red values while brightening
the green values with a linear function:
identity
A “do-nothing” function. This lets you explicitly state that a color channel should
remain unaffected. (This is the default if you don’t provide an <feFuncX> element
for a particular channel.)
table
Lets you divide the color values into a series of equal intervals, each of which will
be proportionately scaled. Consider the following remapping, which doubles the
value of the lowest quarter of the color range, squeezes the next quarter into a range
of one-tenth, keeps the third quarter in exact proportion, and then squeezes the last
quarter of the values into the remaining 15% of the color range:
Original value range Modified value range
0.00–0.25 0.00–0.50
0.25–0.50 0.50–0.60
0.50–0.75 0.60–0.85
0.75–1.00 0.85–1.00
Y ou would specify this mapping for the green channel by listing the endpoints of the
remapped range in the tableValues  attribute:
<feFuncG  type="table"
  tableValues = "0.0, 0.5, 0.6, 0.85, 1.0" />
If you are dividing the input spectrum into n different sections, you must provide n+1
items in tableValues , separated by whitespace or commas:
discrete
Lets you divide the color values into a series of equal intervals, each of which will
be mapped to a single discrete color value. Consider the following remapping, which
maps the value of the lowest quarter of the color range to 0.125, sets the next quarter
to 0.375, the third quarter to 0.625, and remaining quarter to 0.875 (i.e., each quarter
of the range is mapped to its center point):
The <feComponentTransfer> Filter | 167

Original value range Modified value
0.00–0.25 0.125
0.25–0.50 0.375
0.50–0.75 0.625
0.75–1.00 0.875
Y ou would specify this mapping for the green channel by listing the discrete values,
separated by commas or whitespace, in the tableValues  attribute:
<feFuncG  type="discrete"
  tableValues = "0.125 0.375 0.625 0.875" />
Dividing the input channel into n sections requires n entries in the tableValues  at‐
tribute. Exception : If you want to remap all the input values to a single output value, you
must place that entry into tableValues  twice. Thus, to set any input value of the blue
channel to 0.5, you would say:
<feFuncB  type="discrete"  tableValues= "0.5 0.5" />
If you want  to invert  the range  of color  values  for a channel  (i.e.,
change  increasing  values  from  a minimum  to maximum  into de‐
creasing values from the maximum to the minimum), use this:
<feFuncX type="table"
  tableValues=" maximum minimum"/>
Figure 11-13  shows the result of using discrete and table transfers as well as inversion
via a table transfer.
Figure 11-13. Result of using table and discrete transfers
168 | Chapter 11: Filters

Defining the Color Space
Ordinarily, the values for red, green, or blue run in a straight line from 0 to 1, with 0
being  none of the color and 1 being 100% of the color. This is called a linear color
space . However, when SVG calculates the color values between gradient stops (as de‐
scribed in Chapter 8 , in “Gradients”  on page 113), SVG uses a special way of representing
color such that the values do not follow a straight line from 0 to 1. This representation
is called the  standard RGB or sRGB  color space , and its use can make gradients much
more natural-looking. Figure 11-14  shows a comparison. The first gradient goes from
black to green, the second from red to green to blue, and the third from black to white.
By default, filter arithmetic calculates any interpolated (“in-between”) values in the lin‐
ear RGB space, so if you apply a filter to an object that has been filled with a gradient,
you will get results that aren’t at all what you expect. In order to get the correct result,
you must tell the filter to do its calculations in sRGB space by adding a
color-interpolation-filters="sRGB"  attribute to your <filter>  element. As
an alternative, you may choose to leave the filter alone and apply
color-interpolation="linearRGB"  to the <gradient>  element, so that it uses the same
color space as the default for filters.
Figure 11-14. Comparison of linearRGB and sRGB
The <feComposite> Filter
So far we have combined the results of filters by using <feMerge>  to layer the inter‐
mediate results one over another. The much more general <feComposite>  element  takes
two inputs, specified with the in and in2 attributes, and an operator  that tells how the
two are to be combined. In the following explanation, presume you’ve specified
result="A"  and result="B"  for previous filter primitive outputs:
<feComposite operator=" over" in="A" in2="B"/>
Produces the result of layering A over B, exactly as <feMergeNode>  does. In fact,
<feMergeNode>  is really just a convenient shortcut for an <feComposite>  element
that specifies an over  operation. ( <feMergeNode>  also allows you to layer more than
two graphics at a time.)
The <feComposite> Filter | 169

1.“Compositing Digital Images, ” T. Porter, T. Duff, SIGGRAPH ’84 Conference Proceedings, Association for
Computing Machinery, Volume 18, Number 3, July 1984.<feComposite operator=" in" in="A" in2="B"/>
The result is the parts of A that overlap the opaque areas of B. It is similar to a masking
effect, but the mask is based on only the alpha channel of B, not its color luminance.
Don’t confuse the name of this attribute value with the in attribute.
<feComposite operator=" out" in="A" in2="B"/>
The result is the parts of A that are outside the opaque areas of B (with a reverse-
masking effect for partially transparent areas).
<feComposite operator=" atop" in="A" in2="B"/>
The result is the part of A that is inside B, as well as the part of B outside A. To quote
the article in which these operators were first defined: “… paper  atop table  includes
paper  where it is on top of table , and table  otherwise; area beyond the edge of the
table is out of the picture. ”1
<feComposite operator=" xor" in="A" in2="B"/>
The result is the part of A that is outside B together with the part of B that is
outside A.
<feComposite in="A" in2="B" operator=" arithmetic "…/>
The ultimate in flexibility. Y ou provide four coefficients: k1, k2, k3, and k4. The
result for each channel of each pixel is calculated as follows:
k1 * A * B + k2 * A + k3 * B + k4
where A and B are the values for that channel and pixel from the input graphics.
The arithmetic operator is useful for doing a “dissolve” effect. If you
want  to have  a resulting  image  that is a% of image  A and b% of image
B, set k1 and k4 to 0, k2 to a/100, and k3 to b/100. For example, to
make a blend with 30% of A and 70% of B, you’d use this:
<feComposite  in="A" in2="B" result="combined"
  k1="0" k2="0.30" k3="0.70" k4="0"/>
Figure 11-15  shows the combinations we’ve described, with red text as the in image and
a blurred offset shadow as the in2 image; the arithmetic blend is 50% of the text and
50% of its shadow.
170 | Chapter 11: Filters

Figure 11-15. Result of using feComposite operators
Example 11-7  uses the in and out operators to do “cut-outs. ” The drop shadow has been
eliminated from this example to produce a more visually pleasing result in Figure 11-16 .
Example 11-7. Use of feComposite in and out
<defs>
<filter id="sky-in"  filterUnits= "objectBoundingBox" >
  <feImage  xlink:href= "sky.jpg"  result="sky"
    x="0" y="0" width="100%" height="100%"
    preserveAspectRatio= "none"/>
  <feComposite  in="sky" in2="SourceGraphic"
    operator= "in"/>
</filter>
<filter id="sky-out"  filterUnits= "objectBoundingBox" >
  <feImage  xlink:href= "sky.jpg"  result="sky"
    x="0" y="0" width="100%" height="100%"
    preserveAspectRatio= "none"/>
  <feComposite  in="sky" in2="SourceGraphic"
    operator= "out"/>
</filter>
<g id="flower" >
  <!-- flower graphic goes here -->
</g>
</defs>
<use xlink:href= "#flower"  transform= "translate(10,10)"
  style="filter: url(#sky-in);" />
<use xlink:href= "#flower"  transform= "translate(170,10)"
  style="filter: url(#sky-out);" />
Figure 11-16. Result of feComposite in and out
The <feComposite> Filter | 171

The <feBlend> Filter
But wait, there’s more! Y es, filters provide yet another way to combine images. The
<feBlend>  element requires two inputs, specified with the in and in2 attributes, and a
mode  that tells how the inputs are to be blended. The possible values are: normal ,
multiply , screen , lighten , and darken . Given opaque inputs
<feBlend in="A" in2="B" mode=" m"/>, the following describes the color of the re‐
sulting pixel for each mode:
normal
B only; this is the same as the over  operator in <feComposite> .
multiply
Multiplies (as the name suggests) A’s value and B’s value for each color channel.
Because color values are in the range 0–1, multiplying them makes them smaller .
This darkens colors, with the strongest effect for dark colors or very different intense
colors, and no effect when one of the colors is white. The result is similar to creating
photographic slides for both images and then stacking them together in the same
slide projector—only the light that passes through both is visible.
screen
Adds the color values together for each channel, and then subtracts their product.
Bright or light colors tend to dominate over dark colors, but colors of similar
brightness get combined. The result is similar to having two different slide projec‐
tors, one for each image, shining on the same screen—bright light from one pro‐
jector overpowers shadows from the other.
darken
Takes the minimum of A and B in each channel. This is the darker color, hence the
name.
lighten
Takes the maximum of A and B in each channel. This is the lighter color, hence the
name.
Note that the appropriate calculation is done independently for each of the red, green,
and blue values. So, if you were to darken a pure red square with RGB values of
(100%, 0%, 0%) and a gray square with RGB values of (50%, 50%, 50%), the resulting
color would be (50%, 0%, 0%). If the inputs are not opaque, then all the modes except
for screen  factor in the transparencies when making the calculations.
Finally, once the color value is calculated, the opacity of the result is determined by the
formula 1 – (1 –  opacity of  A) * (1 –  opacity  of B). Using this formula, two opaque
items will still be opaque; two items that are 50% opaque will combine to one that is
75% opaque.
172 | Chapter 11: Filters

Figure 11-17  shows the result of blending an opaque, white-to-black gradient bar with
opaque and 50% opaque color squares that have RGB values of black ( #000 ), yellow
(#ff0 ), red (#f00 ), medium-bright green ( #0c0 ), and dark blue ( #009 ).
Figure 11-17. Result of feBlend
The <feFlood> and <feTile> Filters
The <feFlood>  and <feTile>  elements  are utility filters . Much like <feOffset> , they
allow you to carry out certain common operations within a series of filter primitives
rather than having to create extra SVG elements in your main graphic.
<feFlood>  provides a solid-colored area for use in compositing or merging. Y ou provide
the flood-color  and flood-opacity , and the filter does the rest.
<feTile>  takes its input and tiles it horizontally and vertically to fill the area specified
in the filter. The size of the tile itself is specified by the size of the input to <feTile> .
Example 11-8  uses <feComposite>  to cut out the flooded and tiled area to the shape of
a flower. The image used as a tile is shown for reference at the upper right of Figure 11-18 .
Example 11-8. Example of feFlood and feTile
<defs>
<filter id="flood-filter"  x="0" y="0" width="100%" height="100%">
  <feFlood  flood-color= "#993300"  flood-opacity= "0.8" result="tint"/>
  <feComposite  in="tint" in2="SourceGraphic"
    operator= "in"/>
</filter>
<filter id="tile-filter"  x="0" y="0" width="100%" height="100%">
  <feImage  xlink:href= "cloth.jpg"  width="32" height="32"
    result="cloth"/>
The <feFlood> and <feTile> Filters | 173

  <feTile in="cloth" result="cloth"/>
  <feComposite  in="cloth" in2="SourceGraphic"
    operator= "in"/>
</filter>
<g id="flower" >
    <!-- flower graphic goes here -->
</g>
</defs>
<use xlink:href= "#flower"  transform= "translate(0, 0)"
  style="filter: url(#flood-filter);" />
<use xlink:href= "#flower"  transform= "translate(110,0)"
  style="filter: url(#tile-filter);" />
<image xlink:href= "cloth.jpg"  x="220" y="10"
  width="32" height="32"/>
Figure 11-18. Result of feFlood and feTile elements
Lighting Effects
If you draw a bright green circle with SVG, it looks like a refugee from a traffic signal,
glowing by its own light and otherwise lying flat on the screen. If you look at a circle
cut out of green construction paper, it looks more “real” because it is lit from an outside
source and has some texture. A circle cut from green plastic not only is lit from outside;
it also has reflected highlights. Light from an outside source is called diffuse lighting ,
and the highlights that reflect off a surface are called specular lighting , from the Latin
speculum , meaning mirror .
In order to achieve these effects, you must specify the following:
•The type of lighting you want ( <feDiffuseLighting>  or <feSpecularLighting> )
•The object you want to light
•The color of light you are using
•The type of light source you want ( <fePointLight> , <feDistantLight> , or
<feSpotLight> ) and its location
174 | Chapter 11: Filters

Y ou specify the location of a light source in three dimensions; this means you will need
a z-value in addition to x- and y-values. As with two-dimensional graphics, the positive
x-axis goes from left to right, and the positive y-axis goes from top to bottom. The
positive z-axis is “coming out of the screen” and pointing at you.
Both these lighting effects use the alpha channel of the object they are illuminating as
a bump map ; higher alpha values are presumed to be “raised” above the surface of the
object.
Diffuse Lighting
The best way to show how the <feDiffuseLighting>  element  works is to jump right
into Example 11-9 , which shines a pale yellow light on a green circle, textured with the
curve pattern from Example 8-1 .
Example 11-9. Diffuse lighting with a point light source
<defs>
  <path id="curve" d="M 0 0 Q 5 20 10 10 T 20 20"      
      style="stroke: black; fill: none;"/>     
  <filter id="diff-light"  color-interpolation-filters= "sRGB"
    x="0" y="0" width="100%" height="100%">    
    <feImage  xlink:href= "#curve"  result="tile"
        width="20" height="20"/>     
    <feTile in="tile" result="tile"/>
    <feDiffuseLighting  in="tile"     
        lighting-color="#ffffcc"
        surfaceScale="1"       
        diffuseConstant="0.5"       
        result="diffuseOutput">      
        <fePointLight  x="0" y="50" z="50"/>     
    </feDiffuseLighting>      
    <feComposite  in="diffuseOutput"  in2="SourceGraphic"
        operator= "in" result="diffuseOutput" />    
    <feBlend  in="diffuseOutput"  in2="SourceGraphic"
        mode="screen" />     
  </filter>
</defs>
<circle id="green-light"  cx="50" cy="50" r="50"
    style="fill: #060; filter: url(#diff-light)" />     
Define the curve to be used as the tile.
Lighting Effects | 175

Set the color interpolation method and the boundaries for the filter.
Tile the area of the filter  with  the curve  image.  This  will become  the bump  map.
This  tiled  area is the input  to the <feDiffuseLighting>  element,  which  is
illuminated  with  a pale yellow  light,  as specified  by the lighting-color
attribute.
The surfaceScale  attribute tells the height of the surface for an alpha value of
1. (More generally, it’s the factor by which the alpha value is multiplied.)
diffuseConstant  is a multiplicative factor that is used in determining the final
RGB values of a pixel. It must have a value greater than or equal to 0; its default
value is 1. The brighter your lighting-color , the smaller this number should
be (unless you like having your picture washed out).
The result of this filter will be named diffuseOutput .
This example uses a point light source , which means a source that radiates light
in all directions.  It is positioned  at the left center  of the area we wish  to illuminate,
and it is 50 units in front of the screen. The farther you set the light away from
the object, the more evenly the object is illuminated. In this example, the light
is up close and personal to get the greatest possible effect.
The end of the <feDiffuseLighting>  element.
<feComposite> ’s in operator  clips  the filter’s  output  to the boundaries  of the
source graphic (the circle).
Finally,  <feBlend>  in screen  mode,  which  tends  to lighten  the input,  creates  the
final part of the filter.
Activate the filter on the desired object to produce Figure 11-19 .
The input  to this filter  is a four-color  graphic,  but only  the alpha
channel  is used.  However,  when  I (David)  inserted  a
<feColorMatrix type="luminanceToAlpha">  and used its output as
the input to the filter, I did not get the desired effect. Remember that
luminanceToAlpha  converts  black  areas  (zero  luminance)  to com‐
plete  transparency  (zero  alpha).  That  leaves  no difference  in alpha
levels between the black squiggle pattern and the transparent empty
background, and therefore no texture for the lighting effects.
176 | Chapter 11: Filters

Figure 11-19. Result of applying diffuse lighting filter
Specular Lighting
Specular lighting, on the other hand, gives highlights rather than illumination.
Example 11-10  shows how this works.
Example 11-10. Specular lighting with a distant light
<defs>
  <path id="curve" d="M 0 0 Q 5 20 10 10 T 20 20"
   style="stroke: black; fill: none;" />    
  <filter id="spec-light"  color-interpolation-filters= "sRGB"
    x="0" y="0" width="100%" height="100%">     
    <feImage  xlink:href= "#curve"  result="tile"
        width="20" height="20"/>     
    <feTile in="tile" result="tile"/>
    <feSpecularLighting  in="tile"     
        lighting-color="#ffffcc"
        surfaceScale="1"       
        specularConstant="1"      
        specularExponent="4"       
        result="specularOutput">      
        <feDistantLight  elevation= "25" azimuth= "0"/>     
    </feSpecularLighting>      
    <feComposite  in="specularOutput"  in2="SourceGraphic"
        operator= "in" result="specularOutput" />     
    <feComposite  in="specularOutput"  in2="SourceGraphic"
        operator= "arithmetic"  k1="0" k2="1" k3="1" k4="0"/>     
  </filter>
</defs>
<circle id="green-light"  cx="50" cy="50" r="50"
  style="fill: #060; filter: url(#spec-light)" />   
As in the previous example, define the curve.
The only difference between this and the previous example is the filter name.
As in the previous example, this section tiles the curve.
Lighting Effects | 177

Starts  the definition  of the <feSpecularLighting>  filter  and specifies  the
lighting-color  to be a pale yellow light.
The surfaceScale  attribute tells the height of the surface for an alpha value of
1. (Specifically, it’s the factor by which the alpha value is multiplied.)
specularConstant  is a multiplicative factor used in determining the final RGB
values of a pixel. It must have a value greater than or equal to 0; its default value
is 1. The brighter  your  lighting-color , the smaller  this number  should  be. The
effect of this number is also moderated by the specularExponent  attribute.
specularExponent  is another factor used in determining the final RGB values
of a pixel. This attribute must have a value from 1 to 128; the default value is 1.
The larger this number, the more “shiny” the result.
The result of this filter will be named specularOutput .
This  example  uses  a distant  light  source,  one which  is so far away  from  the image
that its light hits all parts of the image at the same angle. Instead of specifying
the position  of the light  source,  you specify  the angle  that the light  is coming
from.
The elevation  and azimuth  attributes  let you specify  the angle  in three
dimensions.  Specifically,  elevation  gives  the angle  of light  above  the plane  of
the screen:  elevation="0"  is light  shining  flat across  the image,  while
elevation="90"  is light shining straight down.
The azimuth  specifies  the angle  within  the plane;  when  elevation  is 0,
azimuth="0"  specifies  light  coming  from  the right  of the image  (more  generally,
the positive end of the x-axis); azimuth="90"  is from the bottom (positive end
of the y-axis),  azimuth="180"  is from  the left, and azimuth="270"  is from  the
top.
The end of the <feSpecularLighting>  element.  Note  that the input  to this filter
was an alpha  channel;  the output  contains  both  alpha  and color  information
(unlike <feDiffuseLighting> , which always produces an opaque result).
Use <feComposite> ’s in operator to clip the filter’s output to the boundaries of
the source graphic (the circle).
Finally,  use <feComposite>  with  the arithmetic  operator  to do a straight
addition of the lighting and the source graphic.
Activate  the filter  on the circle,  producing  the highlighting  relief  effect  in
Figure 11-20 .
178 | Chapter 11: Filters

Figure 11-20. Result of applying specular lighting filter
An excellent  tutorial  on lighting  effects  in three  dimensions  is
available.  We’re  working  in only  two dimensions,  but much  of the
information is applicable.
A third type of light source, <feSpotLight> , is specified with these attributes: x, y, and
z, the location of the spotlight (default value is 0); pointsAtX , pointsAtY , and
pointsAtZ , the place the spotlight is pointing at (default value is 0);
specularExponent , a value that controls the focus for the light source (default value is
1); and limitingConeAngle , which restricts the region where the light is projected. This
is the angle between the spotlight axis and the cone. Thus, if you want a 30-degree spread
for the entire cone, specify the angle as 15. (The default value is to allow unlimited
spread.)
Accessing the Background
In addition to the SourceGraphic  and SourceAlpha  filter inputs, a filtered object may
access the part of the image that has already been rendered onto the canvas when you
invoke a filter. These parts are called BackgroundImage  (not BackgroundGraphic ) and
BackgroundAlpha . In order to access these inputs, the filtered object must be within a
container element that has set the enable-background  attribute to the value new.
Example 11-11  performs a Gaussian blur on the background image.
Example 11-11. Accessing the background image
<defs>
  <filter id="blur-background" >   
    <feGaussianBlur  in="BackgroundImage"  stdDeviation= "2" result="blur" />
    <feComposite  in="blur" in2="SourceGraphic"  operator= "in" />
    <feOffset  dx="4" dy="4" result="offsetBlur" />
  </filter>
</defs>
<g enable-background= "new">     
  <rect x="0" y="0" width="60" height="60"
    style="fill: lightblue; stroke: blue; stroke-width:10"  />    
  <circle cx="40" cy="40" r="30"
Accessing the Background | 179

    style="fill: #fff; filter: url(#blur-background);"  />   
</g>
This is similar to the blur filter used for drop shadows, except the input is now
the BackgroundImage  rather than the SourceAlpha .
Because  <g> is a container  element,  it is a perfect  candidate  for placing  the
enable-background . All the children  of this element  will have  access  to the
background image and alpha.
The rectangle is drawn onto the canvas and into a background buffer.
The circle  does  not display  directly;  the filter  blurs  the background  image  (which
does  not include  the circle)  and composites  in the SourceGraphic . Figure  11-21
shows the result.
As of the time  of writing,  no web browser  implements
enable-background  or the BackgroundImage  and BackgroundAlpha
inputs. If you use those inputs in a filter in a browser that does not
support them, the filter will not return anything—meaning that the
filtered part of your graphic will disappear.
An alternative  is to separate  out the background  into its own  <g>
element  and use <feImage>  to import  it into your  filter.  Adapting
Example 11-11  in this way results in the following code:
<defs>
  <filter id="blur-background" >
    <feImage  xlink:href= "#background"  result="bg"/>
    <feGaussianBlur  in="bg" stdDeviation= "2" result="blur" />
    <feComposite  in="blur" in2="SourceGraphic"  operator= "in" />
    <feOffset  dx="4" dy="4" result="offsetBlur" />
  </filter>
</defs>
<g id="background" >
  <rect x="0" y="0" width="60" height="60"
    style="fill: lightblue; stroke: blue; stroke-width:10"  />
</g>
<circle cx="40" cy="40" r="30"
    style="fill: #fff; filter: url(#blur-background);"  />
The result is the same as Figure 11-21  in browsers that support the
use of SVG fragments for <feImage>  (it currently does not work in
Mozilla Firefox).
180 | Chapter 11: Filters

Figure 11-21. Result of accessing background image
The <feMorphology> Element
The <feMorphology>  element lets you “thin” or “thicken” a graphic. Y ou specify an
operator  with a value of erode  to thin or dilate  to thicken a graphic. The radius
attribute tells us how much the lines are to be thickened or thinned. It’s ordinarily applied
to alpha channels. Example 11-12  erodes and dilates a simple line drawing. As you see
in Figure 11-22 , erosion can wreak havoc on a drawing that has thin lines to begin with.
Example 11-12. Thickening and thinning with feMorphology
http://oreillymedia.github.io/svg-essentials-examples/ch11/fe_morphology.html
<defs>
  <g id="cat" stroke-width= "2">
      <!-- drawing of a cat -->
  </g>
  <filter id="erode1" >
    <feMorphology  operator= "erode" radius="1"/>
  </filter>
  <filter id="dilate2" >
    <feMorphology  operator= "dilate"  radius="2"/>
  </filter>
</defs>
<use xlink:href= "#cat"/>
<text x="75" y="170" style="text-anchor: middle;" >Normal</text>
<use xlink:href= "#cat" transform= "translate(150,0)"
    style="filter: url(#erode1);" />
<text x="225" y="170" style="text-anchor: middle;" >Erode 1</text>
<use xlink:href= "#cat" transform= "translate(300,0)"
    style="filter: url(#dilate2);" />
<text x="375" y="170" style="text-anchor: middle;" >Dilate 2 </text>
The <feMorphology> Element | 181

Figure 11-22. Result of using feMorphology
The <feConvolveMatrix> Element
The <feConvolveMatrix>  element lets you calculate a pixel’s new value in terms of the
values of its neighboring pixels. This filter lets you do effects such as blurring, sharp‐
ening, embossing, and beveling. It works by combining a pixel with its neighboring
pixels to produce a resulting pixel value. Imagine a pixel P and its eight neighboring
pixels (the usual case that is used with this filter):
A  B  C
D  P  E
F  G  H
Y ou then specify a list of nine numbers in the kernelMatrix  attribute. These numbers
tell how much to multiply each pixel by. These products will be added up. The sum
could well come out to be greater than 1 (if all the factors are positive, for example), so,
to even the intensity, the result is divided by the total of the factors. Let’s say you specify
these nine numbers (spaced out to show them as a matrix):
<feConvolveMatrix  kernelMatrix= "
   0  1  2
   3  4  5
   6  7  8" />
The new value of pixel P will then be as follows:
P' = ((0*A) + (1*B) + (2*C) +
     (3*D) + (4*P) + (5*E) +
     (6*F) + (7*G) + (8*H)) / ( 0 + 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8)
The exception is if all your matrix values sum to 0; in this case, no division is performed.
Y ou can also specify a bias  property, which shifts the output range of the filter by adding
the specified offset value to each pixel. The bias  is added after the division, but before
the result is clamped to the 0–1 allowable range.
182 | Chapter 11: Filters

2.Filters containing <feConvolveMatrix>  elements caused rendering errors when tested in Apache Batik ver‐
sion 1.7; the example works as expected when it was tested in web browsers.Example 11-13  achieves the embossing effect shown in Figure 11-23  by taking the upper-
left neighbor minus the lower-right neighbor of each pixel.2 The bias  of 0.5 is added
after the kernelMatrix  summation and division is applied. The bias causes pixels with
identical top-left and bottom-right neighbors to be displayed as gray (versus black
without a bias shift). Pixels where the top-left neighbor is brighter than the bottom-left
neighbor display in dark colors, while pixels where the bottom-left neighbor is brighter
display in bright colors. As a result, diagonal edges are highlighted as if the image were
raised up and lit from the side. The transparent background pixels are considered to
have a color of black.
The default behavior of <feConvolveMatrix>  is to apply the calculations to all the
channels, including alpha. With that setting, only the edges of each shape—the ones for
which the upper-left neighbors have a higher alpha value than the (transparent back‐
ground) lower-right neighbors—would be displayed. In order to apply calculations only
to the red, green, and blue values, we specified preserveAlpha  as true ; the default value
is false .
Example 11-13. Embossing with feConvolveMatrix
<defs>
  <filter id="emboss" >
    <feConvolveMatrix
      preserveAlpha= "true"
      kernelMatrix= "1 0 0 0 0 0 0 0 -1"
      bias="0.5"/>
  </filter>
  <g id="flower" >
    <!-- flower graphic goes here -->
  </g>
</defs>
<use xlink:href= "#flower"  style="filter: url(#emboss);" />
Figure 11-23. Result of using feConvolveMatrix
The <feConvolveMatrix> Element | 183

Although the default matrix size is three columns by three rows, you can specify any
size you want with the order  attribute. If you specify order="4" , then the matrix will
require sixteen numbers (4 by 4) in the kernelMatrix  attribute. A matrix with three
columns and two rows would be specified by order="3 2"  and would require six num‐
bers. The larger your kernel matrix, the more computation is required to produce the
result.
For a pixel in the middle of a graphic, the neighbors are easy to identify. What do you
do with the pixels on the edges of the graphic? Who are their neighbors? This decision
is made by the setting you give the edgeMode  attribute. If you set its value to be duplicate
(the default), then <feConvolveMatrix>  duplicates the edge values in the required di‐
rection to produce a neighbor. The value wrap  wraps around to the opposite side to find
a neighbor. For example, the neighbor above a pixel at the top is the pixel at the bottom,
and the neighbor to the left of a pixel at the left edge is the corresponding pixel at the
right edge. This behavior is useful if the image being modified will be used as a repeating
tile. The value of none  will provide a transparent black pixel (red, green, blue, and alpha
values of zero) for any missing neighbors.
All the possibilities from <feConvolveMatrix>  can’t possibly be described here. Ex‐
periment with the online example and see what you can come up with:
http://oreillymedia.github.io/svg-essentials-examples/ch11/convolve.html
The <feDisplacementMap> Element
This fascinating filter uses the color values of its second input to decide how far to move
the pixels in the first input. Y ou specify which color channel should be used to affect
the x-coordinate of a pixel with the xChannelSelector  attribute; the yChannelSelector
attribute specifies the color channel used to affect the y-coordinate. The legal values for
these selectors are "R", "G", "B", and "A" (for the alpha channel). Y ou must specify how
far to displace pixels; the scale  attribute gives the appropriate scaling factor. If you don’t
specify this attribute, the filter won’t do anything.
Example 11-14  creates a gradient rectangle as the second input. The displacement factor
will be set to 10, the red channel will be used as an x offset, and the green channel will
be used as a y offset. Figure 11-24  shows the result of applying this displacement to the
flower.
Example 11-14. Using a gradient as a displacement map
<defs>
  <linearGradient  id="gradient" >
    <stop offset="0" style="stop-color: #ff0000;"  />
    <stop offset="0.5" style="stop-color: #00ff00;" />
    <stop offset="1" style="stop-color: #000000;" />
  </linearGradient>
184 | Chapter 11: Filters

  <rect id="rectangle"  x="0" y="0" width="100" height="200"
    style="fill: url(#gradient);" />
  <filter id="displace" >
    <feImage  xlink:href= "#rectangle"  result="grad"/>
    <feDisplacementMap
      scale="10"
      xChannelSelector= "R"
      yChannelSelector= "G"
      in="SourceGraphic"  in2="grad"/>
  </filter>
  <g id="flower" >
    <!-- flower graphic goes here -->
  </g>
</defs>
<use xlink:href= "#flower"  style="filter: url(#displace);" />
Figure 11-24. Result of using feDisplacementMap
It’s possible to use the same graphic for both inputs. This means a graphic’s displacement
is controlled by its own coloration. This effect, as written in Example 11-15  and displayed
in Figure 11-25 , can be quite eccentric.
Example 11-15. Using a graphic as its own displacement map
<defs>
<filter id="self-displace" >
  <feDisplacementMap
    scale="10"
    xChannelSelector= "R"
    yChannelSelector= "G"
    in="SourceGraphic"  in2="SourceGraphic" />
</filter>
<g id="flower" >
  <!-- flower graphic goes here -->
</g>
</defs>
The <feDisplacementMap> Element | 185

<use xlink:href= "#flower"  style="filter: url(#self-displace);" />
Figure 11-25. Same graphic used as both inputs to feDisplacementMap
The <feTurbulence> Element
The <feTurbulence>  element lets you produce artificial textures for effects like marble,
clouds, etc. by using equations developed by Ken Perlin.This is referred to as Perlin
noise . Y ou specify these attributes:
type
One of turbulence  or fractalNoise . Fractal noise is smoother in appearance.
baseFrequency
The larger the number you give as the value for this attribute, the more quickly
colors change in the result. This number must be greater than 0 and should be less
than 1. Y ou may also give two numbers for this attribute; the first will be the fre‐
quency in the x direction, and the second will be the frequency in the y direction.
numOctaves
This is the number of noise functions that should be added together when gener‐
ating the final result. The larger this number, the more fine-grained the texture. The
default value is 1.
seed
The starting value for the random number generator this filter uses. The default
value is 0; change it to get some variety in the result.
Figure 11-26  is a screenshot of an SVG file showing various values of the first three of
these attributes.
186 | Chapter 11: Filters

Figure 11-26. Various values of feTurbulence attributes
The online example allows you to experiment with all three parameters:
http://oreillymedia.github.io/svg-essentials-examples/ch11/turbulence.html
Filter Reference Summary
The <filter>  element contains a series of filter primitives, each of which takes one or
more inputs and provides a single result for use with other filters. The result of the last
filter in the series is rendered into the final graphic. Y ou specify the dimensions of the
canvas to which the filter applies with the x, y, width , and height  attributes. Use
filterUnits  to specify the units used to define the filter region, and primitiveUnits
to specify the coordinate system for the various length values within the filter primitives.
Table 11-1  presents a filter reference summary. Each of the filter primitive elements has
an in attribute that gives the source for the primitive, and may also specify an x, y,
width , and height .
Table 11-1. Filter reference summary
Element Attributes
<feBlend> in2="second source "
mode="normal"  | "multiply"  | "screen"  | "darken"  | "lighten"
(default is normal )
<feColorMatrix> type="matrix" | "saturate" | "hueRotate" |
"luminanceToAlpha"
values=" matrix values " | "saturation value (0-1) " |
"rotate degrees "
<feComponentTransfer> Container for <feFuncR> , <feFuncG> , <feFuncB> , and <feFuncA>  elements
Filter Reference Summary | 187

Element Attributes
<feFuncX> type="identity" | "table" | "discrete" | "linear" | "gamma"
tableValues=" intervals for table, steps for discrete "
slope="linear slope "
intercept=" linear intercept "
amplitude=" gamma amplitude "
exponent=" gamma exponent "
offset=" gamma offset "
<feComposite> in2="second source "
operator="over" | "in" | "out" | "atop" | "xor" |
"arithmetic"
The following attributes are used with arithmetic  (any attributes that are not specified
have a default value of 0):
k1="factor for in1*in2 "
k2="factor for in1 "
k3="factor for in2 "
k4="additive offset "
<feConvolveMatrix> order="columns rows " (default 3 by 3)
kernel=" values"
bias="offset value "
<feDiffuseLighting> Container for a light source element
surfaceScale=" height" (default 1)
diffuseConstant=" factor" (must be non-negative; default 1)
<feDisplacementMap> scale="displacement factor " (default 0)
xChannelSelector="R" | "G" | "B" | "A"
yChannelSelector="R" | "G" | "B" | "A"
in2="second input "
<feFlood> flood-color=" color specification "
flood-opacity=" value (0-1) "
<feGaussianBlur> stdDeviation=" blur spread " (larger is blurrier; default 0)
<feImage> xlink:href=" image source "
<feMerge> Container for <feMergeNode>  elements
<feMergeNode> in="intermediate result "
<feMorphology> operator="erode" | "dilate"
radius=" x-radius y-radius "
radius=" radius"
<feOffset> dx="x offset " (default 0)
dy="y offset " (default 0)
<feSpecularLighting> Container for a light source element
surfaceScale=" height" (default 1)
specularConstant=" factor" (must be non-negative; default 1)
specularExponent=" exponent " (range 1-128; default 1)
<feTile> Tiles the in layer
188 | Chapter 11: Filters

Element Attributes
<feTurbulence> type="turbulence" | "fractalNoise"
baseFrequency=" x-frequency y-frequency "
baseFrequency=" frequency "
numOctaves=" integer"
seed="number"
<feDistantLight> azimuth=" degrees" (default 0)
elevation=" degrees" (default 0)
<fePointLight> x="coordinate " (default 0)
y="coordinate " (default 0)
z="coordinate " (default 0)
<feSpotLight> x="coordinate " (default 0)
y="coordinate " (default 0)
z="coordinate " (default 0)
pointsAtX=" coordinate " (default 0)
pointsAtY=" coordinate " (default 0)
pointsAtZ=" coordinate " (default 0)
specularConstant=" focus control " (default 1)
limitingConeAngle=" degrees"
Filter Reference Summary | 189