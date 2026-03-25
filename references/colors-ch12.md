Chapter 9. Radial Gradients
Linear gradients, as we have seen, are defined by the coordinates of a line (the gradient vector).  The color stops are positions along this line.  Each color stretches out to infinity along parallel lines.  All of which can be used to create many effects.  But it isn’t the only gradient option in SVG.

A *radial* gradient is one in which color changes radiate outwards from a central point.  These gradients are generally used to represent the shading on spheres, lights, disks, and similar rounded structures.

In its simplest form, a radial gradient is defined by a circle, with the colors changing from the circle’s center to its edge.  This is the default behavior for SVG’s <radialGradient> element.  As usual, the default behavior is not the only option, and you can create a number of effects with SVG radial gradients—including effects you cannot yet create with CSS gradients.

This chapter looks at all the possibilities, skimming through the areas where radial gradients are similar to linear gradients and focusing on the differences.  The final section looks at the big picture, showing how you can combine many different gradients to create a complex scene.
Radial Gradient Basics
Radial gradients are similar to linear gradients in structure, at least in the markup.  As with <linearGradient>, a <radialGradient> is a container for <stop> elements, each of which have an offset attribute with values between 0 and 1 (or 0% and 100%).  The value at each stop is still specified with stop-color and stop-opacity styles or presentation attributes.

The difference is in how those stop offsets are mapped to the two-dimensional space being filled.

For radial gradients, the offset is a proportion of the distance between the starting point and the ending circle.  By default (when a <radialGradient> does not have any positioning attributes), the ending circle is the largest circle that will fill the object bounding box, and the starting point is its center.

Example 9-1 adapts four of the basic red-blue gradients from Chapter 6; the stops are the same, but they are now inside <radialGradient> elements.
Tip
If the radial gradients from Example 9-1 were in the same file as their linear counterparts, we could have used xlink:href to copy the stops, even though they are different gradient types.

The gradients in Example 9-1 are used to fill circles, with the result shown in Figure 9-1.
Example 9-1. Creating radial gradients to fill circles
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="1in">
    <title>Radial Gradients</title>
    <radialGradient id="red-blue">
        <stop stop-color="red" offset="0"/>
        <stop stop-color="lightSkyBlue" offset="1"/>
    </radialGradient>
    <radialGradient id="red-blue-2">
        <stop stop-color="red" offset="0"/>
        <stop stop-color="lightSkyBlue" offset="0.3"/>
        <stop stop-color="red" offset="1"/>
    </radialGradient>
    <radialGradient id="red-blue-3">
        <stop stop-color="red" offset="0.3"/>
        <stop stop-color="lightSkyBlue" offset="0.7"/>
    </radialGradient>
    <radialGradient id="red-blue-4">
        <stop stop-color="red" offset="0.3"/>
        <stop stop-color="lightSkyBlue" offset="0.3"/>
        <stop stop-color="lightSkyBlue" offset="0.7"/>
        <stop stop-color="red" offset="0.7"/>
    </radialGradient>
    <circle r="0.5in" cy="50%" cx="12.5%" fill="url(#red-blue)" />
    <circle r="0.5in" cy="50%" cx="37.5%" fill="url(#red-blue-2)" />
    <circle r="0.5in" cy="50%" cx="62.5%" fill="url(#red-blue-3)" />
    <circle r="0.5in" cy="50%" cx="87.5%" fill="url(#red-blue-4)" />
</svg>

Instead of the color stops being drawn as parallel lines, they are drawn as concentric circles.  When the color transitions are sharp, as in the last sample, this creates a bulls-eye effect.
Filling the Box
When filling a circle with a radial gradient, the ending circle of the gradient—the largest circle that will fill the object bounding box—is the circle itself.  The gradient therefore fits the shape perfectly.  In cases where it doesn’t, the extra space is filled according to the spreadMethod property, which means that, by default, they are padded with the final stop color.

Figure 9-2 shows the gradients from Example 9-1 used to fill 1-inch-square rectangles.

Radial gradients also neatly fill <ellipse> elements, as demonstrated in Figure 9-3; each stop now represents a concentric ellipse.  The exact same elliptical pattern is seen when the gradients fill rectangles that are the same size as the ellipse’s bounding box, as shown in Figure 9-4.

Again, the results in Figure 9-3 and Figure 9-4 use the exact same gradients that were defined in Example 9-1; there are no attributes on the gradient elements that indicate an elliptical pattern should be used.  Instead, the elliptical pattern is a direct result of the stretched coordinate system created by object bounding box units.  The stop patterns are still mathematically “circles”, in that each point with that stop’s color is the same distance from the center point—but only when you measure it in the stretched coordinates.
Tip
The only way to create a radial SVG gradient that always follows a perfect circular shape, regardless of the bounding box dimensions, is to use gradientUnits="userSpaceOnUse".  You will then need to use other properties to size and position the gradient to match the shape it will fill.

When filling squares or rectangles, the 100% stop offset is positioned around the (possibly stretched) circle that just touches each side.  In the figures, however, the solid-color padding camouflages this boundary.  The spreadMethod attribute can again be used to change the padding effect, and again the other options are repeat and reflect.
Warning
As mentioned in Chapter 8, Webkit browsers do not support repeating or reflecting gradients at the time of writing.  Older versions of other browsers (pre-2014 Firefox and pre-2013 Chrome) will also pad the gradient irrespective of the spreadMethod.

When you repeat or reflect a radial gradient, you do not end up with a dotted pattern of repeating circles.  Instead, the color pattern along each ray—radiating outwards from the central point—is extended, repeating or reflecting, until it reaches the edge of the shape.  This dramatically changes the look of the corners when a radial gradient is used to fill a rectangle.  The different options are used in Example 9-2; the effects are shown in Figure 9-5.
Example 9-2. Changing the corner effect using spreadMethod
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="1in">
    <title>Repeating Radial Gradients</title>
    <radialGradient id="red-blue" spreadMethod="pad">
        <stop stop-color="red" offset="0"/>
        <stop stop-color="lightSkyBlue" offset="1"/>
    </radialGradient>
    <radialGradient id="repeat" spreadMethod="repeat"
                    xlink:href="#red-blue" />
    <radialGradient id="reflect" spreadMethod="reflect"
                    xlink:href="#red-blue" />

    <rect height="1in" width="32%" x="0" fill="url(#red-blue)" />
    <rect height="1in" width="32%" x="34%" fill="url(#repeat)" />
    <rect height="1in" width="32%" x="68%" fill="url(#reflect)" />
</svg>

With a repeating gradient, the color switches instantaneously along the edge of the circle/ellipse, to restart at the first color stop.

With a reflected gradient, the edge is shown as a peak before the color shifts back.  If there is enough space between the edge of the gradient circle and the edge of the shape, this can create a nice ripple effect, like the waves from a stone dropped in a puddle.  To see this effect, we will first need to create a radial gradient where the gradient circle doesn’t fill up the entire bounding box.
Scaling the Circle
You know by now that the default position and size of a linear gradient can be changed with attributes on the gradient element.  The same is true for radial gradients.  Just as the positioning attributes for <linearGradient> looked much like the attributes for a <line>, so the positioning attributes for a <radialGradient> look much like those of a <circle>.

The attributes cx, cy, and r on a <radialGradient> define the size and position of the 100% offset circle.  They are all equal to 50% by default, which creates a circle centered in the coordinate system and filling its full width and height.

Example 9-3 modifies these attributes, using a reflect spread method and userSpaceOnUse gradient units, to create a radiating ripple pattern, as shown in Figure 9-6.
Example 9-3. Controlling the size and position of a radial gradient
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="2in">
    <title>Repeating Radial Ripples</title>
    <radialGradient id="raindrop" spreadMethod="reflect"
                    gradientUnits="userSpaceOnUse"
                    cx="30%" cy="70%" r="15px">
        <stop stop-color="lightSteelBlue" offset="0.4"/>
        <stop stop-color="darkSlateGray" offset="0.8"/>
        <stop stop-color="darkSlateBlue" offset="1"/>
    </radialGradient>
    <rect height="100%" width="100%" fill="url(#raindrop)" />
</svg>

The radius of the base circle is 15px, so for every 15px distance the gradient cycles through the list of stops—in one direction or the other.  The dark rings in the figure are spaced 30px apart, the distance required to cycle through the reflected gradient in both directions.
Adjusting the Focus
There are two other positioning attributes for a radial gradient: fx and fy.  These are the coordinates of the *focal point* of the gradient.  While the gradient circle describes the position of the 100% offset stop, the focal point describes the position of the 0% stop.

The gradients we’ve seen have used the default focal point, which coincides with the circle’s center; the default value of fx is the value of cx and the default for fy is the value of cy.  Change the default, and the gradient will be asymetrical.

What does it mean to have a focal point other than the center of the circle?  It means that the length of the gradient cycle will be different in every direction.  Every ray radiating out from that point will pass through all the stop colors by the time it reaches the edge of the circle.
Warning
The focal point should be *inside* the gradient circle; if it isn’t, under SVG 1.1 it will be shifted to exactly meet the nearest edge of the circle.  SVG 2 proposes a different behavior, based on what is currently used in HTML canvas gradient functions.  In other words, don’t expect a consistent appearance cross-browser if the focal point is outside the circle!

What does it *look* like to have a focal point other than the center of the circle?  The effect is to compress the gradient in one direction while expanding it in the other.   It’s probably easiest just to show you.

Example 9-4 constructs a grid of gradients, shifting the focal point to left and right of center using fx, and shifting the centre of the gradient circle up and down using cy.  The resulting combinations are shown in Figure 9-7.

The example uses a spreadMethod of reflect to show how the repeat distance in any given direction matches the original distance between the focal point and the centers edge.  It also just looks really cool!
Example 9-4. Controlling the size and position of a radial gradient
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="3in">
    <title>Focus Point vs Center Point</title>
    <defs>
        <radialGradient id="center" spreadMethod="reflect"
                        r="30%">                           
            <stop stop-color="darkRed" offset="0"/>
            <stop stop-color="lightYellow" offset="1"/>
        </radialGradient>
        <radialGradient id="left" xlink:href="#center"
                        fx="25%"/>                         
        <radialGradient id="right" xlink:href="#center"
                        fx="75%"/>

        <radialGradient id="left-up" xlink:href="#left"
                        cy="25%"/>                         
        <radialGradient id="left-down" xlink:href="#left"
                        cy="75%"/>
        <radialGradient id="center-up" xlink:href="#center"
                        cy="25%"/>
        <radialGradient id="center-down" xlink:href="#center"
                        cy="75%"/>
        <radialGradient id="right-up" xlink:href="#right"
                        cy="25%"/>
        <radialGradient id="right-down" xlink:href="#right"
                        cy="75%"/>

        <rect id="r" width="30%" height="30%" />           
    </defs>

    <use xlink:href="#r" x="0%"  y="0%"  fill="url(#left-up)"/>
    <use xlink:href="#r" x="35%" y="0%"  fill="url(#center-up)"/>
    <use xlink:href="#r" x="70%" y="0%"  fill="url(#right-up)"/>
    <use xlink:href="#r" x="0%"  y="35%" fill="url(#left)"/>
    <use xlink:href="#r" x="35%" y="35%" fill="url(#center)"/>
    <use xlink:href="#r" x="70%" y="35%" fill="url(#right)"/>
    <use xlink:href="#r" x="0%"  y="70%" fill="url(#left-down)"/>
    <use xlink:href="#r" x="35%" y="70%" fill="url(#center-down)"/>
    <use xlink:href="#r" x="70%" y="70%" fill="url(#right-down)"/>
</svg>

The gradient that will be used for the center block uses the default, centered, circle and focal point.  The radius is set at 30% of the object bounding box, to leave room for the repeat effects.

The other gradients for the center row are created by adjusting the fx to left or right of center, after duplicating the color stops and radius from the first gradient using xlink:href.

The remaining gradients duplicate the color stops, radius, and focal point from one of the previous gradients, and then adjust the cy value.

Because there will be nine rectangles with the same width and height, a pre-defined shape is created and then duplicated with <use> elements.

As you can see, changing the center point of the gradient circle has the effect of translating the entire gradient pattern.  Changing the focal point, in contrast, significantly changes the overall pattern of the gradient, particularly in the repeats.
Tip
If you changed the center point *without* changing the focal point, that would change the look of the gradient considerably, of course.  In Example 9-4 the fy vertical position of the gradient isn’t set, so it automatically adjusts to match the change in cy.
Transforming Radial Gradients
The gradientTransform attribute applies to <radialGradient>, and functions in the same way that it does for linear gradients—it changes the underlying coordinate system of the gradient, translating, rotating, scaling, or skewing not only the center point but the entire gradient.

Once again, you have to watch out for the effects of the stretched object bounding box coordinate system.  No matter how you rotate an objectBoundingBox radial gradient, it will always be stretched out along the longer axis of the shape.

You can, however, create an ellipse stretched along a different axis by using a skew transformation.  Example 9-5 uses the same skew transformation on three versions of a gradient which differ according to their focal point.
Example 9-5. Skewing radial gradients
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="1in">
    <title>Skewed Radial Gradients</title>
    <defs>
        <radialGradient id="center" spreadMethod="repeat" r="40%"
            gradientTransform="translate(-0.3,0) skewX(30)">   
            <stop stop-color="indigo" offset="0"/>
            <stop stop-color="lightPink" offset="0.5"/>
            <stop stop-color="lightSkyBlue" offset="0.7"/>
            <stop stop-color="indigo" offset="1"/>             
        </radialGradient>
        <radialGradient id="left" xlink:href="#center"
                        fx="25%"/>
        <radialGradient id="right" xlink:href="#center"
                        fx="75%"/>                             

        <rect id="r" width="30%" height="100%" />
    </defs>

    <use xlink:href="#r" x="0%"  y="0%" fill="url(#left)"/>
    <use xlink:href="#r" x="35%" y="0%" fill="url(#center)"/>
    <use xlink:href="#r" x="70%" y="0%" fill="url(#right)"/>
</svg>

The skewX(30) transformation will shift points on the gradient to the right to an increasing amount as the *y*-position increases.

The gradient uses the repeat spread method, so the order of stops will not alternate.  However, the first and last stop have the same color, avoiding a sharp transition.

As in Example 9-4, the alternative gradients are created by shifting the focal point left and right.

The gradientTransform used in Example 9-5 also includes a horizontal translation to reposition the center of the gradient, which will also be affected by the skew.
Tip
Transformations on a <radialGradient> are calculated relative to the origin (top-left corner) of the coordinate system, not the center of the gradient.

Skews and off-center focal points allow radial gradients to go beyond perfect geometric  symmetry and provide a sense of three-dimensionality.

To create nuanced representations of real objects, you need to factor in the variation in color created by light and shadow.  SVG includes filter functions expressly intended to simulate lighting effects, but these add extra processing time.  If you can use gradients to achieve the same effect, it will be much more efficient—particularly if you are going to animate the image later.

Figure 9-9 compares the two effects. One shape uses an asymetrical radial gradient to simulate a yellow light shining on a red ball, the other uses lighting filters to achieve a similar effect.  The gradient does not create quite the same three-dimensional curve as the filter, but it is close.  The complete code is presented in Example 9-6.
Example 9-6. Creating lighting effects with gradients or filters
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="2.1in" >
    <title>Simulated Lighting vs Lighting Filters</title>

    <radialGradient id="faux-lighting"
                    cx="45%" cy="45%" r="60%"
                    fx="30%" fy="30%" >                    
        <stop offset="0.1" stop-color="lightYellow" />
        <stop offset="0.9" stop-color="darkRed" />         
    </radialGradient>
    <circle cx="25%" cy="50%" r="1in"
            fill="url(#faux-lighting)"/>                   

    <filter id="yellow-glow" primitiveUnits="objectBoundingBox">
        <feGaussianBlur in="SourceAlpha" stdDeviation="0.3" />
        <feComposite in2="SourceAlpha" operator="in" />    
        <feSpecularLighting surfaceScale="0.1"
                     specularConstant="1"
                     specularExponent="20"
                     lighting-color="lightYellow"
                     result="highlight">
            <fePointLight x="0.35" y="0.35" z="0.7"/>      
        </feSpecularLighting>
        <feComposite in="SourceGraphic" in2="highlight"
                     operator="arithmetic"
                     k1="0" k2="1" k3="0.9" k4="0" />
        <feComposite in2="SourceAlpha" operator="in" />    
    </filter>
    <circle cx="75%" cy="50%" r="1in"
            fill="darkRed" filter="url(#yellow-glow)" />   
</svg>

The <radialGradient> defines a circle that is slightly larger than the bounding box, and off-center towards the upper left.  The focal point is shifted even further to the upper left.  This side of the shape will therefore be more strongly influenced by the initial color stops, while the bottom right will be dominated by the final color stop.

The stops are offset slightly from the start and end of the gradient rays, creating a small circle of solid light yellow around the focal point and revealing the final red at the far edge of the gradient circle.

The effect is applied simply as the fill property of the first circle.

The filter starts by generating a blurred version of the alpha channel of the shape, then clipping it to the original shape edges.  This creates a layer whose alpha value decreases as it gets closer to the shape’s edges.

The lighting filter uses the alpha channel from the previous step as a *bump map* to define the three-dimensional shape of an object.  It then calculates the amount of light that would be added to the shape, using the various attributes to define the reflectiveness of the shape and using the <fePointLight> element to define the position and pattern of incident light. The lighting-color property specifies the same light yellow color for the light.

The final steps combine the lighting effect with the underlying colored graphic, again clipping it so the light is only visible on the parts of the shape that were originally opaque.

The second circle is filled in solid with darkRed, and then has the lighting glow applied via the filter property.

One strong justification for using gradients instead of lighting filters, currently, is consistency.  The exact implementations of the lighting filters vary between browsers considerably.  Figure 9-9 showed the result in Firefox; Figure 9-10 is the same code rendered by Chrome, with a much stronger washed-out effect of the light.  Internet Explorer generates an image in-between the two.

The performance benefit to using gradients instead of filters is two-fold.  First, the lighting-filter is a many-step computation while gradient generation is a single function. Second, because filters are calculated on the rendered, rasterized image, they need to be re-calculated any time the underlying shape changes or moves.
Grand Gradients
SVG gradients are a deceptively simple topic.  There may only be two types of gradients, linear and radial, but there are numerous possible variations to create different effect.  Using all these variations, gradients can add considerable nuance and detail to simple vector shapes.

Gradients are a major part of the SVG developer’s toolkit, and they can be used to create remarkably complex and sophisticated images.  In most cases, these images do not consist of a single, geometrically exact gradient, but rather overlapping multiple partially-transparent gradients.

A good exercise is to consider something like a theatrical stage, complete with red velvet side and top curtains and a stage with a spotlight. As a graphic, it’s handy for highlighting a given object—a picture, slide, video or block of text—and because it is SVG, the curtains can even be directly manipulated to open and close.

When curtain fabric folds in and out, it is caught by the available light to show areas of brightness and shadow which can be represented well with a shaded red gradient.  One of us (Amelia) had previously created a poor approximation of this scene using simple rectangles filled with repeating gradients.  The version displayed in Figure 9-12 was created by Kurt, carefully shading the curtains by positioning the stops in the gradients to match the curves in the paths.

The image is made up of three distinct layers.

At the back, covering the full width, is the stage, which is simply a rectangle the width of the graphic starting about 350 units from the top, with multiple gradient overlays on it to create the appearance of a spotlight on a brown wooden floor.  The lip of the stage is actually part of one of the gradients, and illustrates how you can use such gradients to “draw” hard boundaries.  The light, in turn, is mostly transparent, with the opacity increasing towards the center and the focal point offset to create the effect of a spotlight hitting the stage at an angle. This is a soft spotlight, where the edges are not clearly delineated but instead appear to spill over into the surrounding darkness.

The next layer consists of the left and right side curtains, which use the same core path shape and a reasonably complex red/maroon pattern that corresponds to highlights and shadows in the folds themselves.  The right curtain is the same as the left curtain, but flipped and translated to align with the right side of the screen.  Each side curtain has a shadow curtain behind it, created from the same path but displaced slightly, and with an opacity of 50%.

Finally, the top curtain overlays the other curtains, and like the side curtains features a very elaborate linear gradient with forty stops, determined primarily by trial and error.  It too has an offset shadow.

The code for the image is presented as Example 9-8.
Example 9-8. Using complex and layered gradients to create a stage
<svg xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink="http://www.w3.org/1999/xlink"
    width="100%" height="100%" viewBox="0 0 1000 650"
    id="curtains" >                                   
    <title>Stage Curtains</title>
    <style type="text/css">
        .curtain-shadow {
            fill: black;
            fill-opacity: 0.5;                        
        }
        .side-curtain {
            fill: url(#side-curtain-gradient);
        }
        .ceiling-curtain {
            fill: url(#ceiling-curtain-gradient);
        }
    </style>
    <defs>                                            
        <path id="curtain" transform="scale(0.55)"
              d="m 0.319,0 0.252,1000 c 0,0 -2.761,40 44.129,30
46.9,-10 38.6,-20 46.9,-10 8.3,0 27.4,10 47.4,0 19,-21 63,10 63,10
0,0 11,10 44,0 33,-10 58,0 58,0 0,0 19,40 52,20 33,-30 34,-30
34,-30 L 390,-0.53 z"/>
        <path id="ceiling-curtain" transform="scale(1,0.5)"
              d="m 0,0 1000,0 0,180 c 0,0 -26,17 -49,9 -22,-9
-48,-32 -57,-18 -8,15 -31,3 -54,-5 -23,-9 -20,45 -49,23 -28,-23
-11,11 -34,0 -23,-12 -23,-32 -46,-18 -22,15 -17,-8 -37,-8 -20,0
-25,23 -54,11 -29,-11 -43,-25 -49,-3 -5,23 -5,23 -28,0 -23,-22
-46,6 -46,6 0,0 -28,12 -43,-3 -14,-14 -54,-8 -54,6 0,14 -23,20
-31,3 -9,-17 8,26 -29,8 -37,-17 -11,-34 -40,-17 -29,17 -49,-3
-63,-5 -14,-3 -14,37 -28,25 -15,-11 -9,-14 -32,-17 -23,-3 -28,37
-46,23 C 114,186 120,157 106,169 91.4,180 85.7,214 74.3,203
62.9,191 60,171 48.6,177 37.1,183 25.7,203 25.7,203 L 2.86,177
C 0,171 0,0 0,0 z"/>
        <linearGradient id="side-curtain-gradient">   
            <stop stop-color="#800000" offset="0"/>
            <stop stop-color="#c00000" offset="0.08"/>
            <stop stop-color="#e00000" offset=".15"/>
            <stop stop-color="#800000" offset="0.24"/>
            <stop stop-color="#400000" offset="0.26"/>
            <stop stop-color="#600000" offset="0.28"/>
            <stop stop-color="#800000" offset="0.30"/>
            <stop stop-color="#c00000" offset="0.33"/>
            <stop stop-color="#800000" offset="0.41"/>
            <stop stop-color="#c00000" offset="0.56"/>
            <stop stop-color="#800000" offset="0.75"/>
            <stop stop-color="#400000" offset="0.82"/>
            <stop stop-color="#800000" offset="0.89"/>
            <stop stop-color="#C00000" offset="0.94"/>
            <stop stop-color="#800000" offset="1"/>
        </linearGradient>
        <linearGradient id="ceiling-curtain-gradient">
            <stop stop-color="#400000" offset="0"/>
            <stop stop-color="#700000" offset="0.013"/>
            <stop stop-color="#800000" offset="0.013"/>
            <stop stop-color="#C00000" offset="0.03"/>
            <stop stop-color="#D00000" offset="0.04"/>
            <stop stop-color="#C00000" offset="0.05"/>
            <stop stop-color="#600000" offset="0.065"/>
            <stop stop-color="#B00000" offset="0.078"/>
            <stop stop-color="#D00000" offset="0.09"/>
            <stop stop-color="#800000" offset="0.12"/>
            <stop stop-color="#A00000" offset="0.135"/>
            <stop stop-color="#D00000" offset="0.145"/>
            <stop stop-color="#C00000" offset="0.16"/>
            <stop stop-color="#600000" offset="0.19"/>
            <stop stop-color="#D00000" offset="0.21"/>
            <stop stop-color="#800000" offset="0.24"/>
            <stop stop-color="#C00000" offset="0.28"/>
            <stop stop-color="#600000" offset="0.32"/>
            <stop stop-color="#800000" offset="0.33"/>
            <stop stop-color="#C00000" offset="0.34"/>
            <stop stop-color="#800000" offset="0.37"/>
            <stop stop-color="#D00000" offset="0.39"/>
            <stop stop-color="#800000" offset="0.42"/>
            <stop stop-color="#E00000" offset="0.46"/>
            <stop stop-color="#A00000" offset="0.51"/>
            <stop stop-color="#D00000" offset="0.55"/>
            <stop stop-color="#800000" offset="0.57"/>
            <stop stop-color="#D00000" offset="0.61"/>
            <stop stop-color="#800000" offset="0.67"/>
            <stop stop-color="#D00000" offset="0.69"/>
            <stop stop-color="#800000" offset="0.71"/>
            <stop stop-color="#C00000" offset="0.74"/>
            <stop stop-color="#D00000" offset="0.76"/>
            <stop stop-color="#800000" offset="0.78"/>
            <stop stop-color="#C00000" offset="0.80"/>
            <stop stop-color="#E00000" offset="0.83"/>
            <stop stop-color="#800000" offset="0.86"/>
            <stop stop-color="#C00000" offset="0.88"/>
            <stop stop-color="#E00000" offset="0.91"/>
            <stop stop-color="#800000" offset="0.94"/>
            <stop stop-color="#C00000" offset="1"/>
        </linearGradient>
        <linearGradient id="stageGradient" x2="0" y2="1">  
            <stop stop-color="#100800" offset="0"/>
            <stop stop-color="saddleBrown" offset="0.97"/>
            <stop stop-color="#A06020" offset="0.97"/>
            <stop stop-color="saddleBrown" offset="1"/>
        </linearGradient>
        <linearGradient id="shadowGradient" x2="0" y2="1"> 
            <stop stop-opacity="0.6" offset="0" />
            <stop stop-opacity="0" offset="0.97"/>
        </linearGradient>
        <radialGradient id="spotlightGradient" cy="0.9" fy="0.6">
            <stop stop-color="#FFFFFF" offset="0"
                  stop-opacity="0.4"/>
            <stop stop-color="#FFFFFF" offset="0.35"
                  stop-opacity="0.1"/>
            <stop stop-color="#000000" offset="1"
                  stop-opacity="0"/>                       
        </radialGradient>

        <rect id="stage" width="1000" height="330" y="260" />
    </defs>

    <rect id="background" width="100%" height="100%"
          fill="black"/>

    <g id="stage-illuminated">                             
        <use xlink:href="#stage" fill="url(#stageGradient)"/>
        <use xlink:href="#stage" fill="url(#shadowGradient)"/>
        <use xlink:href="#stage" fill="url(#spotlightGradient)"/>
    </g>

    <g id="side-curtain-left" >                            
        <use xlink:href="#curtain" class="curtain-shadow"
             x="-1" y="3"/>
        <use xlink:href="#curtain" class="side-curtain"/>
    </g>
    <g id="side-curtain-right"
       transform="translate(1000,0) scale(-1,1)">          
        <use xlink:href="#side-curtain-left"/>
    </g>
    <g id="ceiling-curtain">                               
        <use xlink:href="#ceiling-curtain" class="curtain-shadow"
             x="5" y="5" />
        <use xlink:href="#ceiling-curtain"
             class="ceiling-curtain"/>
    </g>
</svg>

An id on the <svg> element makes the entire scene available for re-use within another graphic (although you would have to import the markup into the other file to get all the styles to work correctly in web browsers).

The fill values for the curtains are set using CSS rules.  In particular, the shadows for the curtains are created simply by substituting the gradient fills with half-transparent black.

There are two complex path elements, one for the side curtains and one for the fringe across the ceiling.  Neither has any styling attributes set directly; they will be styled when they are duplicated with <use> elements.

The first two <linearGradient> objects for the curtains have been carefully designed to match the corresponding <path> elements.  Many SVG graphics editors have visual tools that can make this easier, although some fussing is inevitable.

The remaining gradients are used for the stage; the first one fills in the colour of the wood from top to bottom, including a sharp transition near the bottom to draw the edge of the stage.

The shadowGradient uses the default black stop-color, adjusting stop-opacity instead to create an extra layer of dark shading from the back to front of the stage (top to bottom of the rectangle).  Applying this as a separate layer makes it easier to later adjust the brightness separately from the color hue.

The <radialGradient> that creates the spotlight will also be layered on top of the stage, so it also makes use of the stop-opacity property to allow the other layers to show through.

After drawing a solid black backdrop, the stage is drawn using the same rectangle three times, to layer together the three different gradients.  This is, of course, a perfect example of why layered fills are an eagerly-anticipated feature of SVG 2—they would allow the stage to be drawn as a single shape filled with all three gradients stacked together.

The left curtain is drawn by layering two versions of the curtain shape together, one with the shadow styles and one with the main curtain styles.

The right curtain is drawn by duplicating and transforming the left curtain.

Finally, the ceiling curtain is drawn using the same layered approach.

Although Example 9-8 contains a lot of code compared to most of the examples in the book, it is still relatively small for an image file: 7KB as editable text, 1.4KB when compressed with gzip.  In comparison, a PNG version of the same image, large enough to fill most desktop displays, is over 80KB in size.