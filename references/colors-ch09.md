Chapter 6. Simple Gradients
Vector graphics are often thought of as “line drawings”, with sharp edges and even expanses of color.  That clean, minimalist style can often be just what your design needs.  But a complete graphics language should also have the option to create soft edges and color transitions.  Gradients are the simplest way to create these effects in SVG.

Gradients consist of smooth transitions from one color or opacity state to another.  SVG currently supports two types of gradients: linear gradients, where each color in the transition is stretched out along parallel lines, and radial gradients, where each color makes a circular shape.

This chapter introduces the basic structure of the SVG gradient elements, and the different color transitions they can create.  The SVG syntax will be contrasted with the CSS syntax to create similar effects.  For now, the examples will use the default orientation and scale of each gradient, and will use them to fill simple rectangles.  The following chapters will explore the full flexibility of SVG gradients.
Gradiated Gradients
The single-color gradients from Example 5-1 in Chapter 5 demonstrated the bare minimum markup for a gradient.  However, they didn’t use any of the features that create a gradiated result.  For that, we’ll need at least two color stops so that a color transition can be displayed.

The basic structure of a two-color linear gradient is as follows:

<linearGradient id="red-blue">
    <stop stop-color="red" offset="0"/>
    <stop stop-color="lightSkyBlue" offset="1"/>
</linearGradient>

The two <stop> elements define the colors that should be blended together to create the gradient; the result is displayed in Figure 6-1, used to fill a rectangle.  Each <stop> has a new attribute, offset, that positions that color along the distance of the gradient.  The offset value is expressed either as a number between 0.0 and 1.0 or as a percentage; here, the very beginning of the gradient is pure red and the very end is light blue.

When you have more than two color stops, the spacing between the offsets controls the rate of change in the color, as shown in Figure 6-2, which uses the following asymetrical gradient:

<linearGradient id="red-blue-2">
    <stop stop-color="red" offset="0"/>
    <stop stop-color="lightSkyBlue" offset="0.3"/>
    <stop stop-color="red" offset="1"/>
</linearGradient>

If the first offset is greater than zero, or the last offset is less than 1, there will be blocks of solid color on either side of the gradiated section, as in the following gradient, displayed in Figure 6-3:

<linearGradient id="red-blue-3">
    <stop stop-color="red" offset="0.3"/>
    <stop stop-color="lightSkyBlue" offset="0.7"/>
</linearGradient>

This is effectively what happens when you only have one <stop>, as in the solid-colored gradients (Example 5-1): the offset on both sides is filled in with the same solid color.
Tip
Offsets outside the range of 0 to 1 (or 0% to 100%) will be clamped to that interval.

The order of XML elements matters, and <stop> elements must be given in order from start to end of the gradient.  If you specify an offset that is less than the offset for a previous stop, it will be adjusted to exactly match the previous maximum offset.

If consecutive stops have the same offset, you can create a sharp edge or stripe, as in the following code used to draw Figure 6-4.

<linearGradient id="red-blue-4">
    <stop stop-color="red" offset="0.3"/>
    <stop stop-color="lightSkyBlue" offset="0.3"/>
    <stop stop-color="lightSkyBlue" offset="0.7"/>
    <stop stop-color="red" offset="0.7"/>
</linearGradient>
Transparency Gradients
The previous examples are all fully opaque color gradients, but we mentioned that gradients can control opacity as well.  The opacity of each gradient stop can be set using the stop-opacity property.  As with all the opacity properties, the value is a number between 0 and 1.  As with stop-color, the value is not inherited by default.

Using stop-opacity, you can create a gradient with a single color transitioning from opaque to transparent, or you can transition both color and opacity, as in the following gradient:

<linearGradient id="red-blue-5">
  <stop stop-color="red"          stop-opacity="0" offset="0"/>
  <stop stop-color="lightSkyBlue" stop-opacity="1" offset="1"/>
</linearGradient>

This semi-transparent gradient is displayed in Figure 6-5 as a filled rectangle super-imposed over top of the text “Gradients…”.  On the far left, the color is pure red, but it is completely transparent.  As the gradient progresses to the right it both changes color and becomes more opaque, obscuring the text more and more.

The only required attribute here is offset; stop-color automatically defaults to black and stop-opacity to 1 (100% opaque, or no transparency). We’ve been using this to skip the opacity value in most of the colored gradients, but you could also skip the color, as in this gradient which transitions from transparent black to opaque black:

<linearGradient id="brightness">
    <stop offset="0" stop-opacity="0"/>
    <stop offset="1" />
</linearGradient>

This gradient is displayed in Figure 6-6, again using text drawn behind the gradient-filled rectangle to emphasize the opacity change.
Controlling the Color Transition
More complex gradients can be created with more stops. For instance, the following will create a rainbow gradient, cycling through fully saturated colors from magenta to blue to cyan to green, yellow, red, and back to magenta again:

<linearGradient id="rainbow">
    <stop stop-color="#F0F" offset="0"/>
    <stop stop-color="#00F" offset="0.1667"/>
    <stop stop-color="#0FF" offset="0.3333"/>
    <stop stop-color="#0F0" offset="0.5"/>
    <stop stop-color="#FF0" offset="0.6667"/>
    <stop stop-color="#F00" offset="0.8333"/>
    <stop stop-color="#F0F" offset="1"/>
</linearGradient>

The resulting rainbow is shown in Figure 6-7.

The color stops were specifically chosen so that at least one color channel would remain maximized at all times, maintaining the fully saturated effect.  The following snippet defines the exact same gradient, using HSL notation for the colors and percentages for the offsets:

<linearGradient id="rainbow">
    <stop stop-color="hsl(300, 100%, 50%)" offset="0%"/>
    <stop stop-color="hsl(240, 100%, 50%)" offset="16.67%"/>
    <stop stop-color="hsl(180, 100%, 50%)" offset="33.33%"/>
    <stop stop-color="hsl(120, 100%, 50%)" offset="50%"/>
    <stop stop-color="hsl( 60, 100%, 50%)" offset="66.67%"/>
    <stop stop-color="hsl(  0, 100%, 50%)" offset="83.33%"/>
    <stop stop-color="hsl(-60, 100%, 50%)" offset="100%"/>
</linearGradient>
Note
The color magenta—also known as fuschia—is defined using both a hue of 300° (the start of the gradient) and a hue of -60° (the end of the gradient).  Just as these are the same angle on a circle, they are the same hue on the color wheel.

Although the colors can be specified using HSL values, the colors are blended in straight lines through RGB color space, not by circling around the HSL color wheel.  As mentioned in “Mixing and Matching”, the intermediary colors are affected by the sRGB model’s weighting of color brightness.  Once adjusted to the sRGB model, each channel (red, green, and blue) is then increased or decreased independently.

If we had not specified both primary and secondary colors, the mid-point between magenta (#F0F) and cyan (#0FF) would not normally be fully saturated blue (#00F).  Instead it would be a much lighter blue, with both the red and green channels partially lit.  If you were to blend pure red (#F00) with saturated blue (#00F) the result would be a dark purple—with both color channels halfway between fully lit and off—not a fully-saturated magenta.

Figure 6-8 compares the deep, muddy blends generated from the primary colors with the much lighter, spring-toned gradient from the secondary colors.  The complete code for the figure is presented in Example 6-1.
Example 6-1. Comparing two multi-colored gradients
<svg xmlns="http://www.w3.org/2000.svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="2in">
    <title>Dark and Light Rainbow Gradients</title>
    <linearGradient id="primary">                          
        <stop stop-color="#F00" offset="0"/>
        <stop stop-color="#00F" offset="0.3333"/>
        <stop stop-color="#0F0" offset="0.6667"/>
        <stop stop-color="#F00" offset="1"/>               
    </linearGradient>
    <linearGradient id="secondary">
        <stop stop-color="#F0F" offset="0"/>
        <stop stop-color="#0FF" offset="0.3333"/>
        <stop stop-color="#FF0" offset="0.6667"/>
        <stop stop-color="#F0F" offset="1"/>
    </linearGradient>
    <rect width="85.714%" height="1in" x="0" y="0"
          fill="url(#primary)" />
    <rect width="85.714%" height="1in" x="14.286%" y="1in"
          fill="url(#secondary)" />                        
</svg>

Although the <linearGradient> elements *could* be contained within a <defs> block, it is not necessary—a gradient is never drawn directly regardless.

Each of these gradients uses three colors, but four stops, in order to cycle back to the starting color at the end.

The two rectangles are each six-sevenths of the figure wide (85.714%), offset by one-seventh (14.286%) of the width of the figure (one-sixth of the rectangle width), in order to align the parts of the gradient with approximately the same hue.  Note that the hues do not perfectly align because of the adjustments from the sRGB model; for example, halfway between magenta and cyan the color is still slightly purplish.