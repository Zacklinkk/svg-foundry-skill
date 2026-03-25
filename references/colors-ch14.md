Chapter 11. Picture-Perfect Patterns
The word *pattern* evokes the repeating designs we explored in Chapter 10.  However, the SVG <pattern> element is more flexible than this.  By defining a pattern tile that fits the object bounding box, you can create a pattern that fills the entire shape without repeating.

Why would you want to do that?  One reason is to be able to layer together multiple paint servers.  Another is to fill a shape with an image, similar to a CSS background image.

As we have already mentioned briefly, SVG 2 will allow you to layer multiple paint servers in a single fill declaration, and use an image as a fill value directly, making these types of pattern redundant.  Until those features are available, however, these techniques are essential to create many effects.

This chapter also examines the use of the viewBox and preserveAspectRatio attributes on the <pattern> element.  These attributes are particularly important with full-image fills, but they can also be used with repeated patterns.
The Layered Look
In Example 9-8 at the end of Chapter 9, we created the effect of a spotlight on a stage by drawing three separate rectangles, each filled with a different gradient.  This approach is fine for a single shape, but isn’t very useful if you want to use the same layering effect on multiple shapes.  That’s where patterns come in.  A pattern can contain all the layers, turning them into a single paint server for use by other shapes.

Example 11-1 uses three gradients combined in a <pattern> to make a circle and an ellipse appear as unevenly-lit colored globes.  The same pattern is also used to fill a text heading, as shown in Figure 11-1.
Example 11-1. Layering gradients using a non-repeating pattern
<svg xmlns="http://www.w3.org/2000/svg"
     width="100%" height="100%" viewBox="0 0 430 650">
    <title>Multiple gradients</title>
    <defs>
        <linearGradient id="red-blue" y2="1">              
            <stop stop-color="red" offset="0"/>
            <stop stop-color="blue" offset="1"/>
        </linearGradient>
        <linearGradient id="yellow-violet" y1="1" y2="0">  
            <stop stop-color="yellow"
                  stop-opacity="0.9" offset="0.1"/>
            <stop stop-color="darkMagenta"
                  stop-opacity="0" offset="0.5"/>          
            <stop stop-color="violet"
                  stop-opacity="0.9" offset="0.9"/>
        </linearGradient>
        <radialGradient id="flare" fx="0.2" fy="0.2"
                        stop-color="white" >               
            <stop stop-color="inherit"
                  stop-opacity="0" offset="0.75"/>
            <stop stop-color="inherit"
                  stop-opacity="0.05" offset="0.85"/>
            <stop stop-color="inherit"
                  stop-opacity="0.2" offset="1"/>
        </radialGradient>
        <pattern id="gradient-pattern" width="1" height="1"
                 patternContentUnits="objectBoundingBox" >   
            <rect width="1" height="1" fill="url(#red-blue)"/>
            <rect width="1" height="1" fill="url(#yellow-violet)"/>
            <rect width="1" height="1" fill="url(#flare)"/>  
        </pattern>
    </defs>
    <g fill="url(#gradient-pattern)">                        
        <circle cx="215" cy="200" r="200" />
        <ellipse cx="110" cy="515" rx="110" ry="135" />
        <text x="430" y="550" text-anchor="end"
              font-size="100px" font-family="serif"
              stroke="indigo">Layers</text>
    </g>
</svg>

The red-blue gradient stretches from the top left to bottom right (remember, x2 is 100% by default) of the bounding box.  It uses solid colors and will be at the bottom of the layers.

The yellow-violet gradient stretches from the bottom left to the top right of the bounding box.

This gradient is partially transparent, transitioning to complete transparency (stop-opacity="0") at the mid point, to allow the other layer to show through.  The transparent stop still needs a stop-color, otherwise it will default to black and the colors on either side will be given a dark gray tint.

The white radial gradient is mostly transparent, only transitioning to 20% opacity at the very edges.  To make it easy to edit or animate, the single color is specified on the gradient element and then explicitly inherited by each stop.  Unfortunately, stop-color does not inherit by default.

The <pattern> uses objectBoundingBox units for both the pattern tile (by default) and the content (explicitly).  The tile width and height set it to fill the entire box without repeating.

The gradient layers are drawn by filling <rect> elements, which are also scaled to fill the entire bounding box, 1 unit each for width and height.

The layered pattern is assigned to fill the shapes and text by setting the fill presentation attribute on a group.  However, each element inherits the fill property independently, and uses its own bounding box to define the gradient layers.

The pattern—and component gradients—in Figure 11-1 stretches to fit each shape (or text element) according to the object bounding box dimensions.  For abstract gradients, that isn’t a problem.  For other pattern contents, the distortion is unacceptable.

One option to avoid distortion would be to use userSpaceOnUse units for the pattern contents.  However, the pattern will no longer scale to fit the shape.  Furthermore, the positioning of the coordinates are relative to the overall space for the graphic, not the object itself. If you move the object so that its coordinates change, the pattern will remain fixed relative to the background.

You could address the positioning problem by always defining your shapes relative to a fixed point in the coordinate system, and then moving them into place with transformations or the x and y attributes on a <use> element (which have the same effect as a translation).  To address the scaling problem, however, you would have to use <symbol> elements or nested SVGs to create a separate coordinate system for each shape, in which it fills the entire width or height.  All of which sounds like a lot of extra work and complication.

Luckily, there is one more option for scaling the contents of a <pattern>: define a viewBox and use preserveAspectRatio to ensure that it is not distorted.
Preserved Patterns
You cannot work with SVG—particularly on the web—without running into the viewBox attribute.  On your root <svg> element, it establishes the basic coordinate system and aspect ratio, allowing your graphics to scale to fit any region you set for them.  It can also be used on nested <svg> elements and for re-used symbols to create local scaling effects.

The viewBox is specified with four numbers: the first two specify the (*x*,*y*) coordinate of the minimum point you want to include in the graphic, while the third and fourth number specify the number of units to include in the width and height, respectively.  Usually, either the first two numbers are zero (so the origin is in the top left corner and all coordinates in the graphic are positive) or they are negative values designed to position the origin in the center of the graphic.

The preserveAspectRatio attribute is easier to overlook, simply because the default value is often all you need.  It sets one of three scaling modes:

- 
meet to scale down you entire viewBox region to just fit in (or *meet* the edges of) the available space without distortion

- 
slice to scale up the viewBox to cover the available space (*slicing* off the extra)

- 
none to stretch or squish the viewBox as required to exactly fill the available space in both directions.

For the meet and slice options, you must also specify an alignment value of the form xMinYMax that controls which point in the viewBox region is aligned with the equivalent point in the drawing rectangle.

On a <pattern> element, a viewBox attribute overrides the patternContentUnits setting and creates your own coordinate system for the pattern.  The coordinate system created by the viewBox will scale to fit the pattern tile, respecting any preserveAspectRatio attribute value.
Warning
Firefox versions prior to 40 (released mid-2015) apply the viewBox and any preserveAspectRatio values *after* converting to object bounding box units (regardless of the patternContentUnits value).  This results in a distorted pattern for non-square bounding boxes, even if the pattern’s aspect ratio perfectly matches the shape.

Because a meet option for preserving aspect ratio scales the content *down* to fit the entire width and height in the available space, this can leave blank space around the pattern contents.  This includes the default xMidYMid meet value that applies when a viewBox is specified but not preserveAspectRatio.

One option is to intentionally draw a backdrop element, within your pattern, that is much larger than the viewBox.  This backdrop will be clipped to the pattern tile, while the viewBox is scaled to fit.  This is used in Example 11-2 to to create a padded circular gradient that maintains its circular shape, regardless of the aspect ratio of the shape it fills.  The gradient’s shape is maintained by drawing it within a square shape (a circle could also be used).  That square fills the entire viewBox, but the viewBox will not always fill the entire pattern tile.

Figure 11-2 shows the resulting gradient pattern, filling an oblong rectangle.
Example 11-2. Creating an always-circular gradient using a fixed-aspect-ratio pattern
<svg xmlns="http://www.w3.org/2000/svg"
     width="100%" height="100%">                          
    <title>Always-Circular Gradient</title>
    <defs>
        <radialGradient id="radial">                       
            <stop stop-color="lightYellow" offset="0"/>
            <stop stop-color="yellow" offset="0.2"/>
            <stop stop-color="gold" offset="0.8"/>
            <stop stop-color="orangeRed" offset="1"/>
        </radialGradient>
        <pattern id="circular-gradient" width="1" height="1"
                 viewBox="0 0 1 1">                        
            <rect width="5" height="5" x="-2" y="-2"
                  fill="orangeRed"/>                       
            <rect width="1" height="1"
                  fill="url(#radial)"/>                    
        </pattern>
    </defs>
    <rect height="100%" width="100%" fill="url(#circular-gradient)"/>
</svg>

The SVG (and the rectangle it contains) scale to 100% of the available space, so you can test different aspect ratios by re-scaling the browser window.

The radial gradient uses default values for positioning and sizing, so it will be centered in the shape in which it is drawn and scale to fit the bounding box.

The pattern also fills the entire bounding box (width and height of 1, with the default patternUnits setting).  The viewBox defines the content units, creating an explicit 1:1 aspect ratio, regardless of bounding box dimensions.

A backdrop rectangle is drawn five times the size of the square created by the viewBox, centered around it.  The backdrop is filled with the same color as the last (padded) gradient stop.

The gradient itself is drawn within a square that just fits to the width and height defined by the viewBox.

An important thing to note is that the shapes within the pattern are specified in the scaled user units, not in percentages.  Although the viewBox creates both a new coordinate system origin and a new scale, it does not create a new viewport for the purposes of defining percentage lengths.  Within a pattern, percentage lengths are scaled proportional to the change in scale of individual units, regardless of any viewBox values.
Tip
As mentioned for object bounding-box units, this is not generally useful or predictable, and percentages should be avoided except for userSpaceOnUse patterns.

The use of a viewBox instead of object bounding-box units also removes the co-dependency between the pattern tile scale and the pattern content scale.  A viewBox on a pattern is always scaled to fit each individual pattern tile, not the bounding box as a whole.  Figure 11-3 shows how the pattern appears after scaling down each pattern tile to 10% of the box width and 25% of its height, using the following attributes:

<pattern id="circular-gradient" width="0.1" height="0.25"
                 viewBox="0 0 1 1">

The width and height used for the <rect> elements in the pattern contents have not changed at all.

The oversized-backdrop approach used in Example 11-2 is not ideal.  If the aspect ratio of the pattern tile is more extreme than 5:1, the backdrop will not be big enough to fill it.  If having the entire tile covered by your pattern is more important than having your entire pattern content visible, you can use a slice value for preserveAspectRatio.  This ensures that the pattern contents always completely cover the tile.
Background Images, SVG-style
When a slice approach to preserving aspect ratio is combined with a single-tile bounding box pattern, the net effect is something very similar to a CSS background images set to background-size: cover.  A single graphic fills the entire shape.

Example 11-3 uses a pattern to contain a fanciful SVG graphic of sunny sky graphic, such as might be used for a backdrop in a slide or poster.  The slice effect is demonstrated by using the pattern to fill a series of rectangles of different aspect ratios, with the results shown in Figure 11-4.
Example 11-3. Re-using an SVG graphic as a non-repeating image fill
<svg xmlns="http://www.w3.org/2000/svg"
     width="100%" height="100%" viewBox="0 0 400 600">
    <title>Sliced Image Pattern</title>
    <defs>
        <linearGradient id="sky" x2="0" y2="1">            
            <stop stop-color="lightSkyBlue" offset="0"/>
            <stop stop-color="deepSkyBlue" offset="1"/>
        </linearGradient>
        <radialGradient id="sunlight" cx="0" cy="0" >
            <stop stop-color="yellow"
                  stop-opacity="0.9" offset="0.2"/>
            <stop stop-color="lightYellow"
                  stop-opacity="0" offset="1"/>
        </radialGradient>
        <radialGradient id="cloud" fx="0.5" fy="0.15" r="0.6">
            <stop stop-color="oldLace" offset="0.75"/>
            <stop stop-color="lightGray" offset="0.9"/>
            <stop stop-color="darkGray" offset="1"/>
        </radialGradient>
        <pattern id="sky-pattern" width="1" height="1"
                 viewBox="0 0 100 50"
                 preserveAspectRatio="xMinYMin slice" >    
            <rect width="100" height="50" fill="url(#sky)"/>
            <g fill="url(#cloud)">                         
                <g>
                    <circle cx="10" cy="42" r="5" />
                    <circle cx="6" cy="42" r="3" />
                    <circle cx="16" cy="43" r="3" />
                    <circle cx="14" cy="41" r="4" />
                </g>
                <g>
                    <circle cx="20" cy="22" r="7" />
                    <circle cx="50" cy="22" r="10" />
                    <circle cx="40" cy="18" r="7" />
                    <circle cx="45" cy="25" r="9" />
                    <circle cx="30" cy="25" r="12" />
                </g>
                <g>
                    <circle cx="72" cy="39" r="5" />
                    <circle cx="77" cy="40" r="3" />
                    <circle cx="83" cy="41" r="4" />
                    <circle cx="80" cy="36" r="5" />
                    <circle cx="76" cy="35" r="3" />
                    <circle cx="86" cy="39" r="3" />
                </g>
            </g>
            <rect width="50" height="50" fill="url(#sunlight)"/>
        </pattern>
    </defs>
    <g fill="url(#sky-pattern)">                           
        <rect width="400" height="150" />
        <text x="200" y="260" textLength="400"
              text-anchor="middle" font-family="sans-serif"
              font-size="124px" font-weight="bold"
              stroke-width="2" stroke="deepSkyBlue"
              >Clouds</text>
        <rect y="280" width="250" height="250" />
        <rect x="270" y="280"
              width="130" height="250" />
        <rect x="0" y="540"
              width="400" height="60" />
    </g>
</svg>

The code includes a number of gradient definitions which will be used by the graphics within the pattern.

The pattern itself has a 2:1 (100×50) aspect ratio defined in its viewBox.  No patternUnits or patternContentUnits attribute are required: the patternUnits are the default, and the width and height fill the entire bounding box.  Again, content units are not required, since they are replaced by the viewBox.  Finally, the preserveAspectRatio sets the slice scale and the alignment.

The pattern contents consists of a rectangle filled with a blue gradient, a collection of gradient-filled circles to create the clouds, and finally another rectangle filled wit the gold-to-transparent radial gradient for the sun.

The <rect> elements and text that actually draw the graphic inherit the fill setting from a group.  As usual, the bounding box for each fill layer is calculated per element, not for the group.

The pattern uses an xMinYMin alignment option, in addition to the slice scaling mode, so that the top left corner of the graphic is always included within the bounding box—although not necessarily the shape itself, as demonstrated when the fill is used for the text.

Although you cannot yet use a separate image file *instead* of an SVG paint server, you can use an image file *within* a paint server, using the SVG <image> element.

Example 11-4 creates a pattern out of a photographic image ( a composite image of the Earth from space by NASA).  The image is scaled up to fill the pattern tile, with the extra sliced off.  Figure 11-5 shows the result, while Figure 11-6 shows the source image for comparison.
Example 11-4. Filling text with a photographic image inside a pattern
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="400" height="250" viewBox="0 0 800 500">
    <title>Earth in Space</title>
    <style type="text/css">
        .earth {
            font-family: sans-serif;
            font-weight: bold;
            font-size: 148pt;

            text-anchor: middle;
            fill: url('#photoFill');
            stroke: #205334;
            stroke-width: 3;
            text-decoration: overline underline;
            text-shadow: white 0 0 8px ;                   
        }
        .background {
            fill: url(#background);
        }
    </style>
    <defs>
        <pattern id="photoFill" width="1" height="1"
                 viewBox="0 0 1 1"
                 preserveAspectRatio="xMidYMid slice">     
            <image x="-0.1" y="-0.1" width="1.2" height="1.2"
                   xlink:href="globe_west_1024.jpg" />     
        </pattern>
        <linearGradient id="background"
                        gradientTransform="rotate(90)">    
            <stop stop-color="black" offset="0"/>
            <stop stop-color="black" offset="0.5"/>
            <stop stop-color="navy" offset="0.80"/>
            <stop stop-color="blue" offset="0.9"/>
            <stop stop-color="lightBlue" offset="0.95"/>
            <stop stop-color="green" offset="0.95"/>
            <stop stop-color="brown" offset="1.2"/>
        </linearGradient>
    </defs>

    <rect width="100%" height="100%" class="background"/>
    <text class="earth" x="400" y="60%">EARTH</text>       

    <metadata>                                             
Image Source: http://visibleearth.nasa.gov/view.php?id=57723
    </metadata>
</svg>

Both the font-styles and the SVG styles are set using a CSS class.  A text-shadow adds a slight glow around the letters in browsers which support this property, while paired underline and overline decorations add a border effect.

The height and width attributes create a single pattern tile that fits the bounding box.  The viewBox creates a square aspect ratio which will expand to cover the tile, with excess sliced off according to preserveAspectRatio.

The image itself is drawn slightly larger than the viewBox, so that the black space around the globe will not be included.

A <linearGradient> provides a horizon-like backdrop effect.

The text is positioned in the middle of the SVG viewBox, with the size, font, and text-anchor set by the CSS class.

The <metadata> element holds extra information about the file that is neither part of the image nor its alternative text.

The example demonstrates how text-decoration marks (underlines, overlines, and strike-throughs) are treated as an intrinisic part of the text content, and are both filled and stroked with the same styles as the letters.
Warning
Although most browsers support text-shadow for text laid out with CSS, Internet Explorer and Firefox do not for SVG text, nor do many other tools based on the SVG 1.1 specifications.  Even web browsers that do support it can be buggy, particularly the text is scaled.  It must be specified using CSS, not as a presentation attribute.

In other words, use text-shadow for non-essential decoration only.

The image used in Example 11-4 is square, exactly matching the aspect ratio created by the height and width attributes on the <image> element.  If it did not match, by default it would scale down to fit, ruining the slice effect on the pattern.  For a different image, you would need to change the image elements dimensions and also the aspect ratio in the viewBox so that the image would still fill it completely.
Tip
If you do not know the aspect ratio of the photograph you’re going to use, you can specify a slice value for preserveAspectRatio on the <image> element itself.  The two slice settings might slice off more than really necessary, but that’s better than having empty space show through!

You could describe the effect from Example 11-4 as *clipping* the image to text.  You can, in fact, create a very similar effect (minus the strokes and shadow), by including the text inside a <clipPath> element and referencing it in the clip-path property of an <image> element.  However, in that case the <image> would be the element on the screen, and the text would be a graphical effect.  By using an image fill to a <text> element, the text remains selectable, accessible content.