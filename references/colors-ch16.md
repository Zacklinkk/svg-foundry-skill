Chapter 13. Painting Lines
The gradient and pattern examples so far have used the fill property to apply that paint to a shape.  However, we mentioned early on in the book that paint servers can also be used for the stroke property.

Applying paint servers to strokes introduces new complexities, which is why we have separated the topic into its own chapter.  We cover the difficulties here, but also give some examples of unique effects you can create with painted strokes.  And we include teasers about some new features proposed for SVG 2 that will make this easier.
Beyond the Edges
Strokes, as we’ve briefly mentioned, do not have to use solid colors.  The stroke property, like the fill property, can use a url() function to reference a paint server—a gradient or pattern—by its id value.  You can also include a fallback color in case there is a problem with that paint server, and—as you’ll soon see—those fallback colors do get used.

It is when using paint servers that you are most strongly reminded that the stroke is really a secondary shape, built upon the element which defines it, and not a line drawn with a pen or brush.  Patterns and gradients currently cannot be painted *along* the path; similar to when painting the fill of a shape, the stroke is cut out of wallpaper-like paint server content.

There are two main areas where designers tend to get frustrated when using paint servers for strokes:

- 
The objectBoundingBox units used by paint servers *do not* include the stroke region.

- 
All paint servers create a rectangle region of paint, that is unaffected by the shape or direction of the stroke.

These complications aren’t always impenatrable obstacles.  For many shapes, a gradient or patterned stroke works just fine.  Example 13-1 uses the basic horizontal red-blue gradient from Chapter 6 to paint a thick-stroked rectangle, as displayed in Figure 13-1.
Example 13-1. Using gradients as stroke paint on a rectangle
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="1.5in" >
    <title>Gradient on a Stroke</title>
    <linearGradient id="red-blue" >
        <stop stop-color="red" offset="0"/>
        <stop stop-color="lightSkyBlue" offset="1"/>
    </linearGradient>
    <rect width="80%" height="50%" x="10%" y="25%"
          stroke-width="20%" stroke="url(#red-blue)"
          fill="none" />
</svg>

The gradient vector may be sized based on the rectangle’s geometric dimensions, rather than the stroke’s dimensions, but with a smooth gradient such as this it is hard to tell.  If you change the spreadMethod to repeat, it becomes much more obvious, as shown in Figure 13-2.  Half of the stroke on each side is beyond the gradient vector, in the repeat region.

The sharp repeat edges in Figure 13-2 are not something you’re likely to create by accident, so you might think that objectBoundingBox units are not a big obstacle after all.  They aren’t, until you start stroking lines instead of shapes.
The Empty Box
Example 13-2 uses the same red-blue linear gradient to stroke a series of straight lines in different orientations.  Because a straight line cannot be filled, using a thick gradient stroke is often a desired effect.  However, as Figure 13-3 shows, the result isn’t often what is desired.
Example 13-2. Using gradients as stroke paint on strait lines
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="3in" >
    <title>Gradient on Strait Lines</title>
    <linearGradient id="red-blue" >
        <stop stop-color="red" offset="0"/>
        <stop stop-color="lightSkyBlue" offset="1"/>
    </linearGradient>
    <g fill="none" stroke-width="0.5in"
       stroke="url(#red-blue) purple" >               
        <line x1="10%" x2="90%" y1="10%" y2="10%" />  
        <line x1="90%" x2="90%" y1="25%" y2="75%" />
        <line x1="90%" x2="10%" y1="90%" y2="90%" />
        <line x1="10%" x2="10%" y1="75%" y2="25%" />
        <line x1="30%" x2="70%" y1="25%" y2="75%" />  
        <line x1="70%" x2="30%" y1="25%" y2="75%" />
    </g>
</svg>

Style properties are applied as presentation attributes on a group containing the individual lines.  The stroke value includes both the reference to the gradient and a fallback solid color.

The first four lines outline the drawing region: straight across the top, down the right side, back horizontally across the bottom, then vertically up the left.

The final two lines are diagonal, one going down left to right and the other angled down right to left.

Why are so many of the lines stroked in solid purple?  Because pure horizontal or vertical lines have no bounding box region.  And no bounding box region means no bounding box gradient.  If we had not specified a fallback color, the strokes would not have been visible at all.

Instinctively, this seems rather extreme for gradients.  After all, even if the horizontal lines do not have any height, they still have width.  Shouldn’t that be enough for a horizontal linear gradient?  And the vertical lines, couldn’t they at least be padded red on one side and blue on the other?

The difficulty comes from the way paint servers are implemented, as rectangular sheets of paint that are transformed to fit the bounding box.  Transformations that result in any dimension collapsing to zero cause division-by-zero errors in the browser’s internal math.  To avoid them, the SVG specifications treat zero-width or zero-height bounding boxes as an error for bounding box paint servers.  The browser responds to the error the same way it would if it couldn’t find the paint server at all—by using the fallback color.

In contrast, the diagonal lines are painted with the gradient—even though the lines still have no fill region—because the bounding box is the tightest rectangle that can contain the object *aligned with the coordinate system axes*.  For an angled line, that is the rectangle of which it is the diagonal.  Although the two diagonal lines are drawn in opposite directions, they have the same bounding box, and so the gradient is continuous between the two.

When most people think of a gradient on a line, they are usually thinking of something more like Figure 13-4.  Each gradient goes from one end of the line to the other, in the direction that the line is drawn.

As you probably guessed, Figure 13-4 can be created with SVG.  However, it requires considerably more markup than Example 13-2.  You cannot use gradients with object bounding box units, so you need to use a userSpaceOnUse gradient.  Then you need to match the gradient vector to the position, direction, and length of the line.

There are two ways to do this.  Example 13-3 shows the code used for Figure 13-4.  It creates a separate <linearGradient> element for each <line>, and sets all the gradient vector positioning attributes to match the line’s positioning attributes.
Example 13-3. Matching linear gradients with strait lines
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="3in" >
    <title>Gradients Made to Measure for Strait Lines</title>
    <linearGradient id="red-blue"
                    gradientUnits="userSpaceOnUse">   
        <stop stop-color="red" offset="0"/>
        <stop stop-color="lightSkyBlue" offset="1"/>
    </linearGradient>
    <g fill="none" stroke-width="0.5in" >             
        <linearGradient xlink:href="#red-blue" id="g1"
              x1="10%" x2="90%" y1="10%" y2="10%" />  
        <line x1="10%" x2="90%" y1="10%" y2="10%"
              stroke="url(#g1) purple"/>              
        <linearGradient xlink:href="#red-blue" id="g2"
              x1="90%" x2="90%" y1="25%" y2="75%" />
        <line x1="90%" x2="90%" y1="25%" y2="75%"
              stroke="url(#g2) purple"/>
        <linearGradient xlink:href="#red-blue" id="g3"
              x1="90%" x2="10%" y1="90%" y2="90%" />
        <line x1="90%" x2="10%" y1="90%" y2="90%"
              stroke="url(#g3) purple"/>
        <linearGradient xlink:href="#red-blue" id="g4"
              x1="10%" x2="10%" y1="75%" y2="25%" />
        <line x1="10%" x2="10%" y1="75%" y2="25%"
              stroke="url(#g4) purple"/>
        <linearGradient xlink:href="#red-blue" id="g5"
              x1="30%" x2="70%" y1="25%" y2="75%" />
        <line x1="30%" x2="70%" y1="25%" y2="75%"
              stroke="url(#g5) purple"/>
        <linearGradient xlink:href="#red-blue" id="g6"
              x1="70%" x2="30%" y1="25%" y2="75%" />
        <line x1="70%" x2="30%" y1="25%" y2="75%"
              stroke="url(#g6) purple"/>
    </g>
</svg>

The basic gradient containing the color stops is used as a template.  It has userSpaceOnUse set for gradientUnits, which will be inherited by the gradients that reference it.

There’s no use setting the stroke value on the group; each element will need a separate stroke property referencing its custom-made gradient.

The <linearGradient> elements each have an xlink:href attribute referencing the template gradient, a unique id, and x1, x2, y1, and y2 attributes that directly match those on the corresponding <line>.

The lines each have the correct stroke gradient set as a presentation attribute.  The fallback color is included just in case, but won’t be used unless we made a typo mistake.

The main downside of the code in Example 13-3 is that it is rather repetitive.  The duplicated attributes between the <line> and <linearGradient> elements mean that you have to update everything twice if you want to re-position anything.  However, this structure is quite effective if you’re using a data-based script to generate the lines in the first place.  You could create a function that sets these attributes on an element based on the data, and then run it twice, once for the line and once for the gradient.
Tip
If your <line> elements make use of default values for some positioning attributes, remember that <linearGradient> has a different default for x2 (100% instead of 0).

Another limitation of the approach used in Example 13-3 is that it significantly increases the number of elements in your DOM.  If you have a very large number of lines and gradients in a dynamic graphic, this could slow down your web page.

An alternative approach to aligning gradients and lines is to use a single gradient, and draw all your lines along it.  Then use transformations to size and position the lines where you want them.  Example 13-4 uses this approach.  Because SVG 1.1 transformations must be specified in user units, not percentages, the drawing has been re-defined with a viewBox and the numbers have been adjusted.  As a result, the final SVG shown in Figure 13-5 is not an exact match for Figure 13-4, but it is close.
Example 13-4. Transforming lines drawn to match a linear gradient
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="3in" viewBox="0 0 100 70">            
    <title>Userspace Gradient on Transformed Strait Lines</title>
    <linearGradient id="red-blue" gradientUnits="userSpaceOnUse">
        <stop stop-color="red" offset="0"/>
        <stop stop-color="lightSkyBlue" offset="1"/>
    </linearGradient>
    <g fill="none" stroke-width="12"
       stroke="url(#red-blue) purple" >                      
        <line x2="100%"
              transform="translate(10,7) scale(0.8,1)" />    
        <line x2="100%"
              transform="translate(90,17.5)
                         rotate(90) scale(0.35,1)" />        
        <line x2="100%"
              transform="translate(90,63) scale(-0.8,1)" />  
        <line x2="100%"
              transform="translate(10,52.5)
                         rotate(-90) scale(0.35,1)" />
        <line x2="100%"
              transform="translate(32.5,17.5)
                         rotate(45) scale(0.495,1)" />
        <line x2="100%"
              transform="translate(67.5,17.5)
                         rotate(135) scale(0.495,1)" />
    </g>
</svg>

The viewBox uses a 100-unit width, so that the horizontal percentages can be directly converted to user unit values.  However, vertical percentages will need to be scaled to the 70-unit height.

There is only one gradient, which uses userSpaceOnUse units.  The gradient vector uses the default arrangement: horizontally from 0 to 100%.

A single stroke value is applied to the group containing all the lines.  Again, the fallback color is only just-in-case; the user-space gradient should always provide valid results.

Each <line> has the exact same positioning attributes: a single x2="100%" to match the default attributes on the <linearGradient> the actual size and position of the line is controlled by the transform attribute.

The transformations are arranged in a particular order that ensures they do not interact in unexpected ways: first, the origin (and start of the line) is translated into place, then it is rotated into the correct orientation, and finally it is scaled along its length (which is the *x*-axis in its transformed coordinate system).

Instead of a 180° rotation, the right-to-left gradient is created with a negative *x*-scaling factor.

With this approach, you do not create any extra elements.  However, you must completely re-define the geometry of the graphic.  If you know in advance that the gradients are an essential part of your graphic, and can plan ahead, this might not be a big obstacle.  In contrast, re-writing a graphic like this—converting code that was defined with positioning attributes to use transformations instead—is usually more hassle than it’s worth.
Using the Coordinate Space
User-space stroke gradients have more applications than as imperfect substitutes for gradients along a line.  In data visualizations, they can be used to change the appearance of a data line according to its value.  Example 13-5 demonstrates this effect.  It create a generic status monitor dashboard, with a line that marks a changing value that should not reach 100%: as the line gets closer to maximum, it turns from green to yellow to red.

The basic framework for the graphic—including the gradient—is defined in the markup, while the  data is inserted via a script.  In practice, the data would come from a web server of some sort; in the example, the data is randomly generated.  Figure 13-6 shows one possible data pattern.
Example 13-5. Using a gradient to add information to a line chart
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="3in">
    <title>Stroked Gradient as Status Indicator</title>
    <svg x="0.6in" y="0.5in" width="3.6in" height="2in"
         viewBox="0 0 180 100"
         style="overflow: visible; font: 10px sans-serif;">    
        <linearGradient id="status" y1="100%" y2="0%" x2="0%"
                        gradientUnits="userSpaceOnUse">        
            <stop stop-color="limegreen" offset="0.4"/>
            <stop stop-color="yellow"    offset="0.8"/>        
            <stop stop-color="red"       offset="0.95"/>
        </linearGradient>

        <rect stroke="black" fill="dimGray"
              width="100%" height="100%" />
        <g text-anchor="end" dominant-baseline="middle"
           transform="translate(-2,0)">                        
            <desc>Y-axis tick labels</desc>
            <text y="100">0%</text>
            <text y="80">20%</text>
            <text y="60">40%</text>
            <text y="40">60%</text>
            <text y="20">80%</text>
            <text y="0">100%</text>
        </g>
        <g text-anchor="middle">
            <text x="50%" dy="-1em" font-size="12px"
                  text-decoration="underline"
                  >Status monitor</text>
            <text x="50%" y="100%" dy="1em">Time</text>
        </g>

        <polyline id="dataline" stroke="url(#status)"
                  stroke-linejoin="bevel"
                  stroke-width="2.5" fill="none" />            
    </svg>
    <script><![CDATA[
(function(){
    var n = 19,
        dx = 10,
        maxY = 100;                                        

    var data = new Array(n),
        points = new Array(n);
    for (var i=0; i<n; i++) {
        data[i] = [i, Math.random()];
        points[i] = [i*dx, maxY * (1 - data[i][1])];       
    }

    var dataline = document.getElementById("dataline");
    dataline.setAttribute("points", points.toString() );   
})()
]]> </script>
</svg>

A nested <svg> is used to create a user-space coordinate system that exactly matches the size of the data region in the chart.  A viewBox applies a custom coordinate system, including a 100-unit height which will convert easily to percentage values.

The <linearGradient> is drawn within a user-space coordinate system, with a vector that goes from bottom to top.

The first 40% of the gradient is solid green; it then transitions through yellow until being solid red after 95%.

A solid background fills the data region, while text labels are offset *outside* of the data region (which has visible overflow).  By including the labels inside the nested SVG, they can be positioned using the viewBox coordinate system, even through they are printed outside of its width and height region.

The data line is a <polyline> element.  It’s presentation styles—including the gradient stroke—are specified in the markup, but it does not have a points attribute, so will not be drawn until the script runs.  An id attribute makes it easy to access from the script.

Inside the script, a set of initialization variables control the scale between the data and the coordinate system: n is the number of data values that will fit in the graphic, dx is the horizontal spacing (in user units) between them, maxY is the number of vertical units into which the range (0-100%) should be scaled to fit.

The data array contains the raw data (here, an index number followed by a random value between 0 and 1).  The points array contains the scaled coordinates for those data points, using the control variables to scale the data and invert the *y*-coordinates, so that 0 is at the bottom and 100% is at the top.

The points array-of-arrays is converted to a comma-separated string with its default toString() method, and used as the points attribute of the polyline.

The key to effectively using a userSpaceOnUse gradient to convey data was to precisely control the user-space coordinate system to match the data region.  Nested coordinate systems allowed the viewBox to exactly match the data region, while still leaving room outside it for the labels.
Warning
To correctly scale the gradient to the data region in Internet Explorer and Webkit/Blink browsers, the <linearGradient> *must* be a child of the nested SVG.  As mentioned a few times already, these browsers use the paint server’s parent coordinate system as the userSpaceOnUse instead of the one associated with the painted shape.

The visual effect of the user-space painted stroke is similar to having a continous rectangular gradient that is then masked by the line, to only show those regions.  However, just as with the image-filled text we discussed in Chapter 11, there is an important structural difference between a gradient clipped to a line and a line painted with a gradient.  If you have any interactive effects in your data graph, you want the line to be the object they interact with, not a mostly-invisible rectangle.

Which is not to suggest that the effect should be restricted to practical data graphics.  You could also use a textured pattern or an image as the revealed content that shines through the stroked areas.
Patterned Lines
Patterned strokes work much the same way as gradient strokes.  However, because patterns repeat in strict vertical and horizontal tiles, it can become even more obvious when the stroke orientation does not align with the paint server’s.

As with gradients, a userSpaceOnUse approach is recommended to avoid problems with objectBoundingBox units.  You *can* still use bounding-box patterns if your shape is sure to have a valid bounding box (i.e., it isn’t a horizontal or vertical strait line), but the scale of the pattern will be based on the scale of the fill region, not the scale of the stroke.

Example 13-6 uses various patterns from Chapter 10 and Chapter 11 to stroke some basic shapes—with varying degrees of effectiveness, as shown in Figure 13-7.
Example 13-6. Using a gradient to add information to a line chart
<svg xmlns="http://www.w3.org/2000/svg"
     width="4.3in" height="4.3in" viewBox="0 0 400 400">
    <title>Patterned Strokes</title>
    <defs>
        <!-- pattern definitions clipped -->                
    </defs>
    <g style="stroke-width: 40px; fill: lightSkyBlue;" >    
        <rect x="20" y="20" width="360" height="360"
                 stroke="url(#pinstripe) gray" />           
        <polygon points="200,30 370,200 200,370 30,200"
              stroke="url(#scales-pattern) green"/>         
        <circle cx="200" cy="200" r="90"
                stroke="url(#gradient-pattern) peachPuff"/> 
    </g>
</svg>

The patterns (and their constituent gradients) are copied directly from Example 10-6, Example 10-5, and Example 11-1, so they are not re-printed here.

All the shapes will have a thick, 40px-wide stroke over top of a light blue fill.

The rectangle uses the pinstripe pattern from Example 10-6.

The diamond-shaped <polygon> uses the fish scale pattern from Example 10-5.

The circle uses the layered gradients from Example 11-1.  It, and the other shapes, are given an appropriate fallback color, just in case.

The small, repeated pattern of the fish scales looks quite effective on the wide strokes.  The pinstripes don’t look bad, but it might not be the effect you wanted: the vertical stripes go across the horizontal strokes of the square, but down the length of the vertical strokes.  The layered gradients create quite an unusual effect.  The gradients were compiled using a bounding-box pattern; since that bounding box doesn’t fill up the entire stroke region, it is tiled on each side, creating sharp changes in color.

As with gradients, there are ways to work around the problems.  Some striped stroke effects can be created with dashed strokes.  When using an object bounding-box pattern for stroke, you can set the x, y, width, and height attributes to define a pattern tile that is large enough to include the stroke region (i.e., x and y are negative, while width and height are greater than 100%).  All of this will become easier in the future with multiple strokes and stroke bounding boxes.