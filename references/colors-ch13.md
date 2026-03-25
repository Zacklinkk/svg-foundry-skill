Chapter 10. Tiles and Textures
While introducing paint servers in Chapter 5, we mentioned that there were a number of different sources you might wish to use to paint a shape: a single color, one or more gradients, repeating patterns, bitmap graphics, text, even other SVG files.  So far, we have described how to use a solid color or a single gradient.  All the remaining possibilites will use a single paint server element: <pattern>.

An SVG pattern defines a block of SVG graphics that will be used as a paint server for other shapes.  Any SVG content can be used, including images, text, and shapes filled with gradients.  A pattern is repeated in a rectangular (or *transformed* rectangular) tiled layout.  You can, however, make one tile the full size of the shape to create a non-repeating fill; that opens so many possibilities it will be discussed separately, in Chapter 11.

There are a number of options for sizing both the tiles and the pattern content.  This make many different designs possible—but others remain difficult.  This chapter tries to emphasize both what can and what can’t be done, and offers some work-around suggestions for the more frustrating situations.  We also compare SVG patterns with a type of repeating pattern most web designers are familiar with: repeating CSS background images.
Building a Building Block
In many ways, the <pattern> element is similar to the gradient elements introduced in previous chapters.  The attributes on the element itself define the shape and size of the pattern *tile* (repeating unit).  The child content of the <pattern> makes up the graphics that are drawn to the screen.

The attributes on <linearGradient> and <radialGradient> matched those of a <line> and <circle>.  The geometrical attributes on a <pattern> are like those of a <rect> or <image>: x, y, width, and height.  All are zero by default, and as usual a width or height of zero prevents the content from being drawn.

To understand how patterns work, it’s usually best to see one in action.  Example 10-1 presents the code for a simple striped pattern, and then uses it to fill two shapes.  Figure 10-1 shows the result.
Example 10-1. Filling shapes with a simple repeating stripes pattern
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="3in" >
    <title>Striped Pattern</title>
    <style type="text/css">
        svg {
            stroke-width: 6px;
        }
    </style>
    <pattern id="stripes" x="5%" width="10%" height="100%">
        <line x1="3" x2="3" y2="100%" stroke="maroon" />
        <line x1="9" x2="9" y2="100%" stroke="gold" />
        <line x1="15" x2="15" y2="100%" stroke="tomato" />
    </pattern>

    <rect x="13" y="10" width="4in" height="1in"
          stroke="royalblue" fill="url(#stripes)" />
    <ellipse cx="50%" cy="2in" rx="3cm" ry="2cm"
             stroke="royalblue" fill="url(#stripes)" />
</svg>

The pattern itself is declared with the following code:

<pattern id="stripes" x="5%" width="10%" height="100%">

The id attribute, as with other paint servers, is required so that the pattern can be used.  The width and height attributes define the size of each repeating tile, while x defines a horizontal offset for use when positioning the first tile.  A y attribute would give a vertical offset.

But offset from what?  Look at Figure 10-1 again, paying attention to the differences between the two shapes.  The stripes are the same width in both shapes, but are much closer together in the ellipse.
Tip
By default, the attributes that control the size of the pattern tile are calculated in objectBoundingBox units.

Each tile extends the full height (100%) of the bounding box, but only 10% of its width.  No matter how wide the shape is, there will be exactly 10 sets of stripes spaced out across it.

As you might have guessed from your experience with gradients, the default object bounding box dimensions for tiles can be changed to userSpaceOnUse.  The relevant attribute is called patternUnits, and the next example will show the effect of changing it.

The x and y offsets are also measured relative to the bounding box.  For Example 10-1, the 5% horizontal offset is half of the 10% tile width.  This positions the first complete stripe shifted over that distance from the top left corner of the bounding box.  However, the pattern still repeats in all directions: in the ellipse, you can just see the edge of another stripe on the left side, because the stripes extend farther across the tile width than the 5% offset.

The stripes themselves, therefore, are *not* scaled proportionate to the object bounding box.  By default, the contents of a pattern are measured in the userSpaceOnUse coordinate system (i.e., the coordinate system in effect for the shape being filled).  The origin of that coordinate system, however, is translated to the top left corner of each pattern tile.
Tip
This scaling of the pattern graphics is controlled by a patternContentUnits attribute on the <pattern> element.  The default is userSpaceOnUse, and the alternate option is objectBoundingBox.

The stripes—the three <line> elements—are much longer than they need to be, extending to 100% of the full height of the SVG.  According to the SVG 1.1 specifications, the pattern should use the value of 100% in effect for the shape being filled.  However, as mentioned in Chapter 7, this is not consistently implemented by browsers when the shape is nested in a different SVG from the paint server.

For many pattern designs, it is necessary to over-size the graphics to be sure that the entire tile is filled, no matter the size of the shape.  Each tile has overflow: hidden so that the graphics will be clipped to the tile.
Warning
Visible overflow is intentionally left undefined for patterns, meaning that each browser can handle it however they choose.  Changing this setting is therefore not recommended.

A more practical example of using pattern tiles that scale with the bounding box, but stripes that don’t, is given in Example 10-2.  It uses a pattern to divide a rectangle into a fixed number of equal pieces, each one outlined with a thin grid line.  This is a common tool in image editing software, and the grid is used within a super-simplified web application, overlaid on top of a photograph.

To demonstrate the scaling effect of the pattern tiles, a thumbnail version of the photo is also included with its own grid; the grid squares are smaller on the thumbnail, but the grid lines stay the same thickness.  In both cases, the lines are actually drawn much larger than required, and trimmed to fit with hidden overflow.
Example 10-2. A resizeable grid pattern within a photo-editing application
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Photo Grid Overlay</title>

    <style type='text/css'>
        body {
            padding: 0.5em 0.25em;
            background-color: lightGray;
        }
        fieldset {
            display: block;
            background-color: white;
            margin-bottom: 1em;
        }
        legend {
            background-color: inherit;
            border-radius: 0.25em;
            padding: 0 0.2em;
        }
        svg {
            width: 100%;
            min-height: 300px;
            max-height: 100vh;              
            shape-rendering: crispEdges;
        }
        .grid {
            fill: none;
            pointer-events: none;           
        }
        .grid4 .grid {
            fill: url(#grid4);              
        }
        .grid9 .grid {
            fill: url(#grid9);
        }
    </style>
</head>
<body>
    <fieldset id="grid-options"
              role="radiogroup" aria-controls="graphics">
        <legend>Show Grid:</legend>                        
        <label>
            <input type="radio" name="grid" value="none" checked>
            None</label>
        <label><input type="radio" name="grid" value="grid4">
            2&times;2</label>
        <label><input type="radio" name="grid" value="grid9">
            3&times;3</label>
    </fieldset>

    <svg id="graphics" viewBox="0 0 400 400">
        <title>Photo View</title>
        <pattern id="grid4" width="50%" height="50%" >     
            <g stroke="gold" stroke-width="4px">
                <line x2="100%" />                         
                <line y2="100%" />
            </g>
        </pattern>
        <pattern id="grid9" xlink:href="#grid4"
                 width="33.33333%" height="33.33333%" />   

        <g>
            <title>Full image</title>
            <image width="400" height="300"
                   xlink:href="cat.jpeg" />
            <rect width="400" height="300" class="grid" /> 
        </g>
        <g transform="translate(140,310)">
            <title>Thumbnail</title>
            <image width="120" height="90"
                   xlink:href="cat.jpeg" />
            <rect width="120" height="90" class="grid" />
        </g>
    </svg>
    <script>
        document.getElementById("grid-options")
           .addEventListener("change", updateGrid);

        function updateGrid(e) {
            var svg = document.getElementById("graphics"),
                radio = e.target;
            if (radio.checked) {
                svg.setAttribute("class", radio.value);    
            }
        }
    </script>
</body>

The first few CSS rules control the overall layout of the web page.  The <svg> element is given minimum and maximum height values, but in the latest browsers it will auto-size according the aspect ratio set in the viewBox attribute.  The shape-rendering: crispEdges setting turns of anti-aliasing of vector graphics, which can make the grid lines appear blurred.

The rectangles with class grid will by default have no fill.  Thanks to the pointer-events setting, they will not be interactive, whether filled or not.

When particular classes are set on a parent element, the grid rectangle is set to be filled with one of two paint servers, identified with a matching id.

The initial HTML markup sets up a small form field with options for turning on the grid.

The inline SVG code defines the <pattern> elements.  The first patter uses the default units: width and height of the pattern tiles are percentages of the bounding box.

The content, however, is drawn in the user space coordinate system.  It consists of two lines (grouped together to apply common styles), each of which starts at the origin and extends in either the horizontal or vertical direction, up to the width or height of the SVG—which will usually be beyond the pattern tile edge.

The second pattern is written short-hand, using an xlink:href reference to the first pattern, but then overriding the width and height attributes.  Just as with gradients, the xlink:href attribute allows one <pattern> element to become the template for another.

The rest of the SVG code describes the visible content: <image> elements with matching <rect> elements to contain the grid.

A simple script responds to radio-button selections by changing the class on the <svg> element.  It uses the simple setAttribute method to cancel out previous class settings and apply new ones in a single step.  This might not be a good idea in a more complex application, but it works for this simple demo.

The end result is functional, but it is not ideal.  The grid lines use a 4px stroke width, but since they are centered over the edge of the pattern tiles, half of that width is always cropped off.  This means that the grid lines are visible on the top and left sides of the rectangle, but not on the bottom right.  The intersections therefore don’t quite match the geometric halves or thirds of the photograph’s width or height.

There is no simple way to fix this with SVG patterns.  When using object bounding box units for the tile *size* you cannot offset the tile by a fixed number of pixels to compensate for the imbalance, since x and y are also interpretted in bounding box units.  And because the grid lines are drawn in user-space units, there is no way to position additional lines on the opposite sides of each tile.
Stretching to Fit
There are very few pattern designs that work with the default of objectBoundingBox units for the pattern tile and userSpaceOnUse units for the content.  For patterns other than thin stripes or grids, you usually *want* the graphics to scale to match the pattern tile.  In other words, you want both patternUnits and patternContentUnits to have the same value, either objectBoundingBox or userSpaceOnUse.
Tip
Because of the mismatched defaults, you only need to set one or the other: either  patternUnits="userSpaceOnUse or patternContentUnits="objectBoundingBox".

When both the pattern tile and its contents scale relative to the object bounding box, you create a pattern that adjusts to fit the shape being filled.  There are always the same number of tiles in each box, but the entire tiled pattern scales up and down together.

When using object bounding-box units like this, keep in mind the lessons from Chapter 7 about the distorting effect of the coordinate system.  If the box is not square, the coordinate system will be non-uniform with horizontal and vertical units of different lengths.  Circles, text, and images will all be stretched and rotational angles will be uneven.  Also note that *everything* is scaled according to the new units, including stroke widths and font size.

Furthermore, for patternContentUnits, unlike for the pattern tile dimensions, you *cannot* use percentages interchangeable with decimals.  Instead, the definition of 100% from the user space coordinate system is scaled up proportional to the scaling effect on all other units.
Tip
No matter what scaling method you use, percentages in the pattern content do not refer to the pattern tile.

If the main viewport was 200 units wide (1% is 2 horizontal units) and 150 units high (1% is 1.5 vertical units), 1% will now be twice the box width and 1.5 times the box height in the scaled coordinate system.  Inside a different SVG, the ratios would be different.

Which is not particularly useful, so percentages are best avoided when using object bounding-box units for pattern contents.

What you *can* use are decimal numbers, knowing that they are always a proportion of the bounding box’s height or width.  Example 10-3 uses this approach to adapt Example 10-1.  The colored stripes now scale in width as well as spacing to fit in the different shapes, as shown in Figure 10-3.
Example 10-3. Filling shapes with a scaled repeating stripes pattern
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="3in" >
    <title>Bounding Box Striped Pattern</title>
    <style type="text/css">
        svg {
            stroke-width: 6px;
        }
    </style>
    <pattern id="stripes2" x="5%" width="10%" height="100%"
             patternContentUnits="objectBoundingBox"
             stroke-width="0.025px" >                            
        <line x1="0.0125" x2="0.0125" y2="1" stroke="maroon" />
        <line x1="0.0375" x2="0.0375" y2="1" stroke="gold" />    
        <line x1="0.0625" x2="0.0625" y2="1" stroke="tomato" />
    </pattern>

    <rect x="13" y="10" width="4in" height="1in"
          stroke="royalblue" fill="url(#stripes2)" />
    <ellipse cx="50%" cy="2in" rx="3cm" ry="2cm"
             stroke="royalblue" fill="url(#stripes2)" />
</svg>

The pattern explicitly sets patternContentUnits to match the default for patternUnits.  The pattern contents will inherit a stroke-width of 0.025px, which will be interpretted in the objectBoundingBox scale.

The coordinates of the lines have similarly been transformed into object bounding-box units.  The lines are spaced 0.025 units apart, with the first line centered 0.0125 units from the edge of the pattern tile.  The three lines plus an equal-sized gap therefore add up to 0.1 units, or 10% of the bounding box width, the exact width of each pattern tile.

When using decimal bounding box units, it is usually easiest to work solely within the mathematical user units, and not bother with units, which will be divorced from their real-world meaning.  However, you can use units, and they will be measured proportional to user unit scale, as demonstrated by the stroke-width value.
Warning
Although px units should always be interchangeable with SVG user units—in any coordinate system—Firefox (version 38) rounds up the stroke-width when it is specified as 0.025px, to 0.03 units.  It renders the stroke correctly if specified as 0.025, without units.

There is an alternative to using progressively smaller decimal numbers for coordinates.  The <pattern> element can take a viewBox attribute to define your own coordinate system for each tile.  We will explore viewBox patterns—and more uses of object bounding box patterns—in Chapter 11, which focuses on creating fill content that fills up the entire bounding box in a single tile.

For repeating tiles, however, you are more likely to use userSpaceOnUse units.
Laying Tiles
With userSpaceOnUse units for both patternUnits and (by default) patternContentUnits, you create fixed-size tiles with a fixed-size graphic in each one.  The result is something closer to the real-world concept of tiles, such as you might use to cover a floor or wall.  The tiles don’t change between larger or smaller floor areas, only the number of tiles that can fit in that room.

Example 10-4 adapts the same striped pattern from Example 10-1, but this time keeps the fixed-size lines and adapts the tiles to match.  Figure 10-4 shows the result.
Example 10-4. Filling shapes with a user-space repeating stripes pattern
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="3in" >
    <title>UserSpace Striped Pattern</title>
    <style type="text/css">
        svg {
            stroke-width: 6px;
        }
    </style>
    <pattern id="stripes3" x="12px" width="24px" height="10px"
             patternUnits="userSpaceOnUse">                     
        <line x1="3" x2="3" y2="10" stroke="maroon" />
        <line x1="9" x2="9" y2="10" stroke="gold" />            
        <line x1="15" x2="15" y2="10" stroke="tomato" />
    </pattern>

    <rect x="13" y="10" width="4in" height="1in"
          stroke="royalblue" fill="url(#stripes3)" />
    <ellipse cx="50%" cy="2in" rx="3cm" ry="2cm"
             stroke="royalblue" fill="url(#stripes3)" />
</svg>

This time, the pattern explicitly sets patternUnits to match the default for patternContentUnits.  The pattern tile attributes have all been re-defined in px units.

The lines no longer have to stretch until 100% of the SVG height; because we know the tile size, we can draw the lines exactly to match.

The stripes in the rectangle and ellipse are not only the same size; they are also perfectly aligned.  The x and y offsets for a user-space pattern tile are calculated relative to the main coordinate system’s origin, not the corner of the shape’s bounding box.  Similar to the user-space gradients we explored in Chapter 7, this creates a continuous flow of paint from one shape to another—unless the shapes themselves are transformed!

With the reliability of knowing exactly how big your pattern tiles are, you can start using graphics inside them more complicated than straight-line stripes.

Because the pattern contents will be clipped to the pattern tile, you can even create patterns that do not, at first glance, follow a rectangular repeating tile.  You can repeat shapes yourself, overlapping opposite sides of the tile, in such a way that when the tiles are placed side-by-side the shapes appear to flow from one to the next.
Tip
Drawing your pattern contents slightly larger than the pattern tile can also help with rounding errors—in some SVG rendering agents—that create single-pixel gaps or discontinuities between tiles.

Figure 10-5 shows a pattern of overlapping scales, like the scales of a tropical fish.  Although the smallest repeating unit is a staggered, overlapping circle, you can create a simple rectangular repeating pattern by drawing a square from the center of one scale to the center of the next one directly to the side and directly below, as shown in the bottom half of the figure.  Example 10-5 provides the code for the pattern, including the scaled up (if you’ll pardon the pun) version of the repeated unit.
Example 10-5. Creating a complex pattern with overlapping shapes
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="100%" height="100%" viewBox="0 0 300 400">
    <title>Fish Scale Pattern</title>
    <style type="text/css"><![CDATA[
        .scale {
            fill: url(#scale-gradient);
            stroke: black;
        }
]]> </style>
    <defs>
        <radialGradient id="scale-gradient">                     
            <stop stop-color="#004000" offset="0"/>
            <stop stop-color="green" offset="0.85"/>
            <stop stop-color="yellow" offset="1"/>
        </radialGradient>
        <pattern id="scales-pattern" width="20" height="20"
                 patternUnits="userSpaceOnUse" >                 
            <g id="scales">
                <circle class="scale" cx="0"  cy="19" r="10"/>   
                <circle class="scale" cx="20" cy="19" r="10"/>
                <circle class="scale" cx="10" cy="9"  r="10"/>   
                <circle class="scale" cx="0"  cy="-1" r="10"/>
                <circle class="scale" cx="20" cy="-1" r="10"/>   
            </g>
        </pattern>
    </defs>
    <rect width="300" height="200" fill="url(#scales-pattern)"/> 
    <g transform="translate(150,310) scale(4) translate(-10,-10)">
        <use xlink:href="#scales" />                             
        <rect width="20" height="20" fill="none"
              stroke="deepSkyBlue" stroke-width="0.5"/>          
    </g>
</svg>

A <radialGradient> is used to fill each scale in the pattern.

The pattern itself uses userSpaceOnUse values for the patternUnits, meaning that the pattern tile will be 20px square regardless of the size of the shape being filled.

The first two scales are centered *almost* at the bottom corners of the pattern tile.

The next scale, which will be drawn overtop of the previous two, is nearly centered in the tile.  However—to avoid clipping the edge of its stroke—it and all the other circles are shifted up by one unit.

The final two scales are centered just above the top corners of the tilse.  They will overlap the middle tile, and will appear to be continuous with the bottom row of scales on the tile above them—and with each other on left and right.

The rectangle in the left half of the figure displays the scales as a continous fill pattern.

At the bottom of the figure, a <use> element duplicates the group of five scales.  Thanks to the transform attribute on the <g> element, the scales are scaled up 4 times, and then offset so the tile is centered in the available space.

The blue 20×20 rectangle, drawn in the same scaled-up coordinate system, outlines the boundaries of the pattern tile.

Most patterns can be constructed in this way, by identifying the constituent rectangular tile at which the pattern repeats in exact horizontal and vertical shifts.  However, for certain geometric patterns you can take advantage of coordinate system transformations to simplify your code.
Transformed Tiles
The patternTransform attribute allows you to rotate, skew, scale, and translate a pattern.  The transformation does not only apply to the pattern’s contents, however: it applies to the entire pattern tile, and to the repeating pattern of tiles laid end to end.
Tip
As with gradientTransform, the CSS transformations module re-defines patternTransform as a presentation attribute synonym for the transform style property—although it isn’t yet supported in browsers.

Pattern transformations make it easy to create diagonal lines and other angled patterns, without having to use trigonometry to figure out the exact horizontal or vertical distance between the line repeats.  Example 10-6 uses a 45-degree rotation to create bias-cut pinstripe and grid patterns.  The example also demonstrates more ways to use one pattern as a template for another.  Figure 10-6 shows the result.
Example 10-6. Creating diagonal patterns with patternTransform
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="4.3in" viewBox="0 0 400 400">
    <title>Pinstripe Patterns</title>
    <defs fill="#444" stroke="lightSkyBlue">                    
        <pattern id="pinstripe" patternUnits="userSpaceOnUse"
                 width="30" height="30">                        
            <rect id="r" width="30" height="30"
                  stroke="none" />
            <line id="l" x1="15" y="0" x2="15" y2="30"
                  fill="none" />
        </pattern>
        <pattern id="diagonals" xlink:href="#pinstripe"
                 patternTransform="rotate(45)" />               
        <pattern id="grid" xlink:href="#pinstripe">
            <use xlink:href="#r" />                             
            <use xlink:href="#l" />
            <use xlink:href="#l" transform="rotate(90, 15, 15)"/>
        </pattern>
        <pattern id="diagonal-grid" xlink:href="#grid"
                 patternTransform="rotate(45)" />               
    </defs>

    <rect width="200" height="200"
          fill="url(#pinstripe)" />
    <rect width="200" height="200" x="200"
          fill="url(#diagonals)" />
    <rect width="200" height="200" y="200"
          fill="url(#diagonal-grid)" />
    <rect width="200" height="200" x="200" y="200"
          fill="url(#grid)" />
</svg>

The four patterns use the same color scheme, here set on the <defs> element.  Patterns and other paint servers inherit styles from their surroundings, the same as shapes do.

The core pinstripe pattern tile consists of a solid-filled square, with a vertical line stroked down its center.  The tile will be a fixed size relative to the user-space coordinate system.

The diagonals pattern duplicates the first pinstripe pattern, but rotates it by 45° to create diagonal lines.

The grid pattern also uses the pinstripe pattern element as a template for the attributes on the pattern element, but it replaces the pattern contents with a copy of the solid rectangle and two copies of the line, rotated at 90 degrees to each other.  As with gradients, if the <pattern> has *any* child content, it replaces all the content from the template.

The final pattern duplicates and rotates the grid pattern.

Example 10-6 also emphasizes how patterns and their contents inherit styles as normal based on their position in the document tree.  Patterns do *not* inherit styles from the element being filled by the pattern.
Warning
The combination of xlink:href pattern templating and pattern style inheritance suggests that you can create color variations of a pattern by changing the inherited styles, similar to how you can create color variations of an icon by setting styles on a <use> element.  Unfortunately, the drafters of the SVG specifications did not consider this possibility, and no browsers tested have implemented it this way.

However, neither do the specifications specifically say that duplicated pattern content *shouldn’t* inherit from the new pattern, so it might change in the future.  For now, ensure that all inherited styles are the same on both the original and duplicate pattern elements.

Diagonal lines such as these could also be created with object bounding-box pattern tiles, although the lessons from the section “Gradients, Transformed” still apply.  Object bounding-box units add their own transformation, which distorts rotational angles.  A 45° rotation would create lines that follow the diagonal of the bounding box, whether or not that is a 45° angle in an absolute sense.

Using transformations, you can create pattern tile layouts where the tiles are parallelograms or diamonds instead of simple rectangles.  For the most predicatable results, these patterns usually work best with userSpaceOnUse patterns.  The uneven scale of object bounding-box units might not play nice with the patternTransform values.

Figure 10-7 shows two geometric patterns that can be created through transformations of rectangular tiles.  The code to create them is presented in Example 10-7.
Example 10-7. Creating triangular and diamond patterns with patternTransform
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="6.45in" viewBox="0 0 400 600">
    <title>Transformed Patterns</title>
    <pattern id="triangles" patternUnits="userSpaceOnUse"
             width="20" height="17.32"
             patternTransform="skewX(30)">                       
        <rect width="30" height="20" fill="lightGreen" />
        <polygon points="0,0 20,0 0,17.32" fill="forestgreen" /> 
    </pattern>
    <pattern id="argyle" patternUnits="userSpaceOnUse"
             width="20" height="20"
             patternTransform="scale(2,4) rotate(45)">           
        <rect fill="mediumPurple" width="20" height="20"/>
        <rect fill="indigo" width="10" height="10"/>             
        <rect fill="navy" width="10" height="10"
              x="10" y="10"/>
        <path stroke="lavender" stroke-width="0.25" fill="none"
              d="M0,5  L20,5  M5,0  L5,20
                 M0,15 L20,15 M15,0 L15,20" />                   
    </pattern>

    <rect width="400" height="300" fill="url(#triangles)" />
    <rect width="400" height="300" y="300"
          fill="url(#argyle)" />
</svg>

The first pattern uses a 30° skew to turn the pattern tiles into parallelograms.  The height of each tile is scaled to equal the height of an equilateral triangle that has a base equal to the width of the pattern tile.

As a result, the <polygon> triangle that fills the top left corner of the untransformed tile becomes a perfect equilateral triangle after the 30° skew.

The second pattern uses a non-uniform scale and a 45° rotation to create diamond-shaped pattern tiles.

A checked pattern is created by using two rectangles, each half the width and height of the tile, in opposite corners.  The checks will be transformed along with the pattern tile, to create checked diamond shapes.

The final element of the argyle pattern is a <path> that draws four separate straight lines, bisecting each of the diamond checks.

Both of the patterns in Example 10-7 do repeat horizontally and vertically, and could have been created without transformations.  However, the pattern contents would have been more complex.  More coordinates would need to be calculated with trigonometry, and there would be greater risk that rounding errors would introduce discontinuities between adjacent tiles.