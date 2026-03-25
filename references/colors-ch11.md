Chapter 8. And Repeat
As we manipulated the gradient vector and the gradient transform in Chapter 7, we pointed out a few times how areas beyond the end of the gradient vector are filled in with solid color.  We’ve also mentioned how this is only the *default* behavior.

This chapter examines the alternative: repeating gradients that continue to transition for as long as required to fill the shape.  Again, there are both SVG and CSS ways of achieving this, and we’ll compare the two.
How to Spread Your Gradient
The appearance of a gradient beyond the ends of the gradient vector is controlled by the spreadMethod attribute on the <linearGradient> element.  It controls how the gradient *spreads* out towards infinity.

The default value for this attribute is pad.  With it, everything before the start of the vector is padded with the first stop-color, while everything beyond the end point is padded with the last stop-color.  The examples we’ve seen so far have all used the padding behavior by default, but Example 8-1 does so explicitly; the x1 and x2 attributes limit the gradient to the middle 10% of the object bounding box, but the rest of the shape will be padded in blue and pink.  The resulting gradient is shown in Figure 8-1.
Example 8-1. Using padded gradients to fill a shape
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="1in">
    <title>Padded Gradient</title>
    <linearGradient id="divider" spreadMethod="pad"
                    x1="45%" x2="55%" >                    
        <stop stop-color="lightSteelBlue" offset="0%"/>
        <stop stop-color="darkRed" offset="50%"/>
        <stop stop-color="salmon" offset="100%"/>          
    </linearGradient>
    <rect width="100%" height="100%" fill="url(#divider)" />
</svg>

You don’t really need to specify spreadMethod, since pad is the default value; it is included here for emphasis.

Note that the stop offsets are a percentage of the gradient vector length, not of the object bounding box or user space.

The other options for spreadMethod are repeat and reflect.  Both of these settings cause the gradient to be repeated as many times as necessary to fill the shape.
Warning
Webkit (Safari browser and older Chrome) does not support the repeat and reflect values of spreadMethod.  Firefox only introduced support for repeated and reflected gradients in 2014 with version 32.  Where not supported, all gradients are rendered with padded colors.

If the repeated effect is essential to your graphic, you will need to manually repeat the gradient stops (and adjust the size of the gradient vector) as many times as required to fill your shape.
Reflections on Infinite Gradients
The reflect method reverses the order of stops on each repeat cycle.  This creates a smooth transition at the start and end points, alternating between the specified colors without any sharp discontinuities.  The colors alternate in peaks and troughs.

Taking the same code from Example 8-1 and adding the spreadMethod="reflect" attribute results in Figure 8-2.

Although the central gradient is the same as in Figure 8-1, the visual effect is quite different when that gradient region is repeated and reflected, creating an appearance of metalic tubes of alternating colors.  Note that the repetitions happen in both directions, before and after the gradient vector you specify.

The effect of a reflected gradient is strongly influence by the length of the gradient vector and the level of contrast in the colors.  Example 8-2 creates a more subtle, wave-like reflected gradient, shown in Figure 8-3.
Example 8-2. Using reflected gradients to create a smooth repeating pattern
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="1in">
    <title>Reflecting Gradient</title>
    <linearGradient id="waves" spreadMethod="reflect"
                    x2="10%" y2="10%">
        <stop stop-color="darkGreen" offset="0"/>
        <stop stop-color="mediumSeaGreen" offset="50%"/>
        <stop stop-color="skyBlue" offset="100%"/>
    </linearGradient>
    <rect width="100%" height="100%" fill="url(#waves)" />
</svg>

The gradient vector runs from the top left corner (default x1 and y1 values) to the point 10% along the diagonal.  This is the distance for a single, uni-directional cycle of the gradient.  The resulting shape therefore has five pairs (10 cycles total) of alternating green-to-blue (and back again) gradients along the diagonal.
Repeating without Reflecting
Reflected gradients create smooth effects, shifting to one color and then back to the other.  In contrast, when spreadMethod is repeat, the gradient stops repeat in the same order from beginning to end.  Unless the start and end colors are identical, this will result in a discontinuity in the gradient.

The sharp difference can be shown by once again adapting the code from Example 8-1, to now use spreadMethod="repeat".  The result is Figure 8-4.

A “sharp” difference, indeed—the edges of each gradient cycle now create sharp lines, as if you’re looking at the creases of an accordion, each side lit differently.

Repeated gradients are useful when creating striped effects.
Example 8-3 takes advantage of them to create a striped wallpaper effect, as shown in Figure 8-5.
Example 8-3. Using repeating gradients to create a striped pattern
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="1in">
    <title>Repeating Gradient</title>
    <linearGradient id="wallpaper" spreadMethod="repeat"
                    gradientUnits="userSpaceOnUse"
                    x1="5px" x2="45px">                    
        <stop stop-color="indigo" offset="50%"/>           
        <stop stop-color="deepSkyBlue" offset="50%"/>
        <stop stop-color="lightSkyBlue" offset="90%"/>     
    </linearGradient>
    <rect width="100%" height="100%" fill="url(#wallpaper)" />
</svg>

The gradient is sized using user-space units, so the width of each gradient cycle can be specified in absolute units instead of as a percentage of the referencing shape.  The gradient vector is horizontal (y1 and y2 are left as the default 0), and each cycle will be 40px wide—the difference between the x1 and x2 values.

The first stop is positioned halfway into the cycle (50% offset); the space between the start of each repeat cycle and the offset will continue to be padded with that first stop color.  In other words, the first half of each repeat will be a solid stripe in indigo.

After a sharp transition to blue at 50% offset, there is a subtle gradient to a lighter blue at 90% offset; the remainder of the repeat cycle will be filled in with the light blue.

Example 8-3 also demonstrates something we mentioned in Chapter 7: off-setting stops from the start or end of the gradient vector is not the same (in repeated or reflected gradients) as changing the length of the vector.  The color is padded in between the <stop> and the end of the vector; the repetition applies to the vector itself.