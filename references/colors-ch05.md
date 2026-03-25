Chapter 2. The Painter’s Model
If I asked you to draw a yellow circle with a blue outline, would it look the same as if I asked you to draw a blue circle and fill it in with yellow?

If I asked you to draw a red pentagon and a green square centered on the same spot on a page, would most of the image be red or green?

There are no hard and fast rules when you’re drawing things by hand.  If someone gives you ambiguous instructions, you can always ask for clarification.  But when you’re giving instructions to a computer, it only has one way to follow them.  So you need to make sure you’re saying exactly what you mean.

Even if you use SVG a lot (and we’re going to assume you use it at least a little), you probably haven’t given much thought to how the computer converts your SVG code into colored patterns on the screen.  If you’re going to really make the most of those colored patterns, however, you need to know how your instructions will be interpretted.

This chapter discusses the basics of the SVG *rendering model*, the process by which the computer generates a drawing from SVG markup and styles.  It reviews the basic fill and stroke properties that define how you want shapes or text to be painted.  The entire rest of the book can really summed up as different ways you can specify fill or stroke values.

The SVG rendering model is known as a *painter’s model*.  Like layers of paint on a wall, content on top obscures content below.  The SVG specifications define which content gets put on top of which other content.  However, this chapter also introduces two properties, z-index and paint-order, which allow you to change up the rendering rules.  These properties are newly introduced in SVG 2, and are only just starting to be supported in web browsers.  We therefore also show how you can achieve the same effect with SVG 1.1 code.
Fill ‘er Up with the fill Property
The basic elements and attributes in you SVG code define precise geometric shapes.  For example, a one-inch square, positioned with its top left corner at the coordinate system origin, looks like this:

<rect width="1in" height="1in" />

A circle ten centimeters in diameter, centered on the middle of the coordinate system, is created with code like the following:

<circle cx="50%" cy="50%" r="5cm" />

If you used either of those elements in an SVG, without any style information, it would be displayed as a solid black region exactly matching the dimensions you specify.  This is the default fill value: solid black.

The fill property tells the SVG-rendering software what to do with that geometric shape.  For every pixel on the screen—or ink spot on the paper—the software determines if that point is inside or outside of the shape.  If it is inside, the software turns to the fill value to find out what to do next.

In the simple case (like the default black), the fill value is a color and all the points inside the shape get replaced by that color.  In other cases, the fill value is a direction to look up more complicated painting code.  Where to look it up is indicated by a URL referencing the id of an SVG element representing the instructions (a *paint server*, which we’ll talk more about starting in Chapter 5)
Tip
If you *don’t* want the software to fill in the shape, all you have to do is say so.  The fill property also takes a value of none.

The fill is by default painted solid and opaque (unless there are different instructions in the paint server).  The fill-opacity property can adjust this.  It takes a decimal number as a value: values between 0 and 1 cause the shape’s paint to be blended with the colors of the background.  A value of 1 (the default) is opaque, while a value of 0 has much the same effect as fill: none.  We’ll discuss opacity in detail in Chapter 4.

When it is not clear which sections of the shape are inside versus outside, the fill-rule property gives the computer exact instruction.  It affects <path> elements with donut holes inside them, as well as paths, polygons, and polylines with criss-crossing edges.

The fill-rule property has two options:

- 
evenodd switches between inside and outside every time you cross an edge.

- 
nonzero (the default) gets “more inside” when you cross an edge that is drawn in the same direction as the last one, and only gets back to outside again when you have cancelled them all out by crossing edges in the opposite direction.

Example 2-1 draws a criss-crossed <polygon>, first with the default nonzero fill rule and then with an evenodd fill rule; Figure 2-1 shows the result.  The shapes have a thin dark stroke around the edges so you can see them even when the shape is filled on both sides of the edge.
Example 2-1. Modifying the fill region with the fill-rule property
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     viewBox="0 0 400 200" width="4.3in" height="2.15in">
    <title>Fill-rule comparison</title>
    <rect fill="lightSkyBlue" height="100%" width="100%" />       

    <polygon id="p"
             fill="blueViolet" stroke="navy"
             points="20,180 20,20 180,20 180,180 60,60 140,60" /> 
    <use xlink:href="#p" x="50%" fill-rule="evenodd" />           
</svg>

The opening <svg> element establishes the coordinate system and sets the default size of the printed figure.  A <rect> element adds a solid-color backdrop.  For this simple SVG code, styles are set with presentation attributes.

The basic polygon has fill and stroke styles, but the fill-rule property will inherit the default nonzero value.

A duplicated copy of the same polygon is offset horizontally by half the width of the SVG.  The copied polygon will inherit the fill-rule="evenodd" value set on the <use> element.

No matter how many times the edges or sub-paths cross over each other, each point is either inside or outside the shape.  Areas are not painted twice just because they are inside two different sub-paths.  That may not seem like a relevant distinction when the fill is a solid color, but it becomes important when fill is partially transparent.

Every shape in SVG, as well as text, can be filled—and will be filled by default.  This includes open-ended <path> elements and <polyline> elements, which define a shape where the end of the line does not connect with the beginning.  The fill region of these shapes is created by connecting the final point back to the beginning in a straight line.  If it ends up crossing other edges as it does so, the fill-rule calculations apply.
Tip
Open segments within a <path> are closed by connecting them back to the initial point on that sub-path:  the last point created by a move-to command.

Even a straight <line> element is technically filled in by default: however, because the return line that connects the end point to the beginning exactly overlaps the original line, the resulting shape does not enclose any area.  No points are inside the shape, and so no points are affected by the fill value.  You need to *stroke* the line if you want to see it.
Stroke It with the stroke Property
In computer graphics, stroking a shape means drawing a line along its edge.  Different programs have different interpretations of what that can mean.

In SVG (currently, anyway), stroking is implemented by generating a secondary shape extending outwards and inwards from the edges of the main shape.  That stroke region is then painted using the same approach as for filling the main shape: the software scans across, and determines whether a point is inside or outside the stroke.  If the point is inside, the software uses the painting instructions from the stroke property to assign a color.
Tip
Each section of the stroke-shape is only painted once, regardless of whether the strokes from different edges of the shape overlap or cross each other.

The default for stroke is none, meaning don’t paint a stroke region at all.  Just like for fill, the other options are a color value or a reference to a paint server element.

Just as with fill, there is a stroke-opacity property to modify the stroke paint.  Just as with fill-opacity, we’ll discuss stroke-opacity in more detail in Chapter 4.

There are many other stroke-related properties.  We’re not going to talk about them much in this book, but they control the geometry of the stroke region.  As a quick reference, they are as follows:

stroke-width

The thickness of the stroke, as a length, number of user units, or percentage of the weighted width and height of the coordinate system.  In SVG 1.1, the stroke region is always centered on the edge of the shape, so half the stroke width extends outside it.

stroke-linecap

The approach to use for stroking around open ends of a path or line; the default butt trims the stroke tight and perpendicular to the endpoint.  The other options, round and square, extend the stroke by half the stroke width, in the specified shape.

stroke-linejoin

The approach to use for stroking around corners in the shape; the default miter extends the strokes in straight lines until they meet in a point.  The other options are round (use a circular arc to connect the two strokes) and bevel (connect the two strokes with an additional straight line).

stroke-miterlimit

The maximum distance to extend a mitered line join beyond the official edge of the shape, as a multiple of the stroke width (default 4 times the width).  If the stroke edges don’t meet in a point within that distance, a bevelled line join is used instead.

stroke-dasharray

A pattern of distances (dashes and gaps) for stroking and not stroking the shape.  The default, none, creates a continuous stroke over the entire shape.  The ends of each dash are affected by the stroke-linecap setting.

stroke-dashoffset

The distance into the dasharray pattern at which to position the beginning of the path. Default is 0.

Example 2-2 mixes and matches various stroke line join and line cap options on copies of the same polygon, to generate the effects shown in Figure 2-2.
Example 2-2. Controlling the geometry of the stroke region
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     viewBox="0 0 400 400" width="4.3in" height="4.3in">
    <title>Stroke join and cap styles compared</title>
    <style type="text/css">
        .backdrop {                                        
            fill: peachPuff;
        }
        .shapes {
            fill: none;
            stroke: indigo;
            stroke-width: 8px;
        }
        .join-round {
            stroke-linejoin: round;
        }
        .join-bevel {
            stroke-linejoin: bevel;
        }
        .join-miter {
            stroke-linejoin: miter;
            stroke-miterlimit: 10;
        }
        .cap-round {
            stroke-linecap: round;
        }
        .cap-butt {
            stroke-linecap: butt;
        }
        .cap-square {
            stroke-linecap: square;
        }
        .wider {
            stroke-width: 14px;
        }
    </style>
    <defs>
        <polyline id="p2"
            points="20,20 20,80 100,80 40,20 100,20" />    
    </defs>
    <rect class="backdrop" height="100%" width="100%" />

    <g class="shapes">
        <g class="cap-round">
            <title>Round line caps</title>
            <g id="row">                                   
                <use xlink:href="#p2" x="0"   class="join-round">
                    <title>Round line joins</title>
                </use>
                <use xlink:href="#p2" x="35%" class="join-bevel">
                    <title>Bevelled line joins</title>
                </use>
                <use xlink:href="#p2" x="70%" class="join-miter">
                    <title>Mitered line joins</title>
                </use>
            </g>
        </g>
        <g class="cap-butt">                               
            <title>Butt (cropped) line caps</title>
            <use xlink:href="#row" y="25%" />
        </g>
        <g class="cap-square">
            <title>Square line caps</title>
            <use xlink:href="#row" y="50%" />
        </g>
        <g class="cap-square wider">                       
            <title>Square line caps with a wider stroke</title>
            <use xlink:href="#row" y="75%" />
        </g>
    </g>
</svg>

For this more complex example, a <style> block of CSS rules is used to set fill and stroke properties on the elements according to their classes.

The basic shape, a <polyline>, is pre-defined in a <defs> section.

Three copies of the polyline are arranged to form each row in the comparison grid.  Each one will have a different stroke-linejoin style.

The row as a whole is then duplicated and shifted vertically, assigning a new stroke-linecap style each time.

The final row also inherits a different value of stroke-width.

Unlike fill, the stroke region *is* affected by open-ended paths.  Stroked <polyline> elements look different from a <polygon> with the same points.  A <path> element (or sub-path thereof) that is closed with a Z command has a line join at the final point, while open sub-paths start and end with line caps.
Stroking the Fill and Filling the Stroke
When a shape has both fill and stroke paint, some pixels are included in both the fill area and the stroke region, and therefore have two different colors specified.  As with all of SVG, the painter’s model applies: if both colors are opaque, the color of the layer on top replaces the color of the layer below.

But which layer is “on top”?

By default, the stroke is painted on top of the fill.  This means that you can always see the full stroke width.  It also means that if the stroke is partially-transparent, it will appear as a two-toned stroke.  The fill paint color will be visible under the inner half of the stroke region but not under the outer half.
Tip
Stroke markers—symbols that display on the corners of custom shapes—are painted after the fill and stroke, in order from start to end of the path.

In SVG 1.1, the only way to draw a stroke underneath the fill is to separate it into two shapes: one with stroke only, and then the same shape duplicated in the same position (with a <use> element), filled but not stroked:

<g stroke="blue" fill="red">
    <g fill="none">
        <path id="shape" d="..." />
    </g>
    <use xlink:href="#shape" stroke="none" />
</g>

The above snippet makes extensive use of inherited styles.  The <path> itself does not have any fill or stroke values directly set; it inherits from its surrounding.  The overall stroke and fill values are set on the containing <g>; one or the other is then cancelled out on the nested group and the <use> element.

SVG 2 introduces the paint-order property to make this effect much easier to achieve.  It’s value is a list of (whitespace-separated) keywords (fill, stroke, and markers) that indicate the order in which the various parts of the shape should be painted.  So the same effect could be created with a single element:

<path id="shape" d="..." stroke="blue" fill="red"
      paint-order="stroke fill" />

Any paint layers you don’t specify in the paint-order property will be painted later (markers, in this case), in the same order they normally would be.  This means that to swap fill and stroke, you only need to specify the stroke:

<path id="shape" d="..." stroke="blue" fill="red"
      paint-order="stroke" />

The stroke will be painted first, then fill and finally any markers.  The entire fill region will always be visible, even where it overlaps the stroke.

The default value of paint-order (equivalent to fill stroke markers) can be explicitly set with the normal keyword.
Warning
At the time of writing, paint-order is supported in the latest Firefox (since version 31) and Blink browsers (since Chromium version 35).  All other browsers use the default paint order.

The ability to control painting order is especially important with text.  Text in SVG can be stroked just like shapes can, to create an outlined effect.  However, all but the thinnest strokes tend to obscure the details of the letters.

By painting the fill region overtop of the stroke—in a contrasting color—you can reinforce the shape of the letters and restore legibility.  Example 2-3 uses paint-order and a thick stroke to create a crisp outline around heading text.  Figure 2-3 shows the result in a supporting browser.
Example 2-3. Stroking without obscuring the finer details of text
<svg xmlns="http://www.w3.org/2000/svg"
     viewBox="0 0 400 80" width="4.3in" height="0.86in">
    <title>Outlined text, using paint-order</title>
    <rect fill="navy" height="100%" width="100%" />
    <text x="50%" y="70"
          text-anchor="middle"
          font-size="80"
          font-family="sans-serif"
          fill="mediumBlue"
          stroke="gold"
          stroke-width="7"
          paint-order="stroke"
          >Outlined</text>
</svg>

If you relied solely on paint-order to achieve this effect, your text would be a blocky mess on unsupporting browsers, as shown in Figure 2-4.  Some fallback strategies are in order.

One solution is to use the CSS @supports conditional rule to only apply the outline effect if paint-order is supported.  In other cases, use a different styling that provides legible text if not the desired effect.

Example 2-4 provides a modified version of the code from Example 2-3; this version applies a much narrower stroke if we cannot control the painting order.  The result looks the same as Figure 2-3 in browsers that support paint-order (all of which currently also support the basic @supports rule).  Figure 2-5 shows how the revised code looks in other browsers.
Example 2-4. Testing support before using paint-order
<svg xmlns="http://www.w3.org/2000/svg"
     viewBox="0 0 400 80" width="4.3in" height="0.86in">
    <title>Using @supports to adjust paint-order effects</title>
    <style type="text/css">
        .outlined {
          text-anchor: middle;
          font-size: 80px;
          font-family: sans-serif;
          fill: mediumBlue;
          stroke: gold;

        /* fallback */
          stroke-width: 3;
        }

        @supports (paint-order: stroke) {
            .outlined {
              stroke-width: 7;
              paint-order: stroke;
            }
        }
    </style>
    <rect fill="navy" height="100%" width="100%" />
    <text x="50%" y="70" class="outlined"
          >Outlined</text>
</svg>
Tip
The stroke-width has been cut by more than half between Figure 2-3 and Figure 2-5. However, the stroke only appears slightly narrower, because the inside half of the stroke is now visible on top of the fill.

If changing the appearance with @supports is not acceptable to you, the only alternative is to duplicate the elements to create one for stroke and one for fill.  Depending on the way you are using your SVG, and how much control you have over its styling, you may be able to use a script to perform the conversion for you when necessary.  Because paint-order is a new style property, browsers that do not support it will not include it within the style property of each element.  You can therefore detect these browsers and generate the extra <use> elements as required.

Example 2-5 provides a sample script which identifies elements by classname and performs the manipulations if required.  The result (in a browser that does not support paint-order) is shown in Figure 2-6.  Although it appears identical to Figure 2-3, the underlying DOM structure is much more complex.
Example 2-5. Simulating paint-order with Multiple Elements
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     viewBox="0 0 400 80" width="4.3in" height="0.86in">
    <title>Faking paint-order with JavaScript</title>
    <style type="text/css">
        .outlined {
          text-anchor: middle;
          font-size: 80px;
          font-family: sans-serif;
          fill: mediumBlue;
          stroke: gold;
          stroke-width: 7;
          paint-order: stroke;
        }
    </style>
    <rect fill="navy" height="100%" width="100%" />
    <text x="50%" y="70" class="outlined"
          >Outlined</text>
    <script><![CDATA[
(function(){
    var NS = {svg: "http://www.w3.org/2000/svg",
              xlink: "http://www.w3.org/1999/xlink"
             };
    var index = 10000;

    var t = document.getElementsByClassName("outlined");   
    if ( t &&
        (t[0].style["paint-order"] === undefined )){       
        Array.prototype.forEach.call(t, fakeOutline);      
    }

    function fakeOutline(el){
        el.id = el.id || "el-" + index++;                  

        var g1 = document.createElementNS(NS.svg, "g");    
        g1.setAttribute("class", el.getAttribute("class") );
        el.removeAttribute("class");
        el.parentNode.insertBefore(g1, el);

        var g2 = document.createElementNS(NS.svg, "g");    
        g2.style["fill"] = "none";
        g2.insertBefore(el, null);
        g1.insertBefore(g2, null);

        var u = document.createElementNS(NS.svg, "use");   
        u.setAttributeNS(NS.xlink, "href", "#" + el.id);
        u.style["stroke"] = "none";
        g1.insertBefore(u, null);
    }
})();
]]> </script>
</svg>

The elements to modify are identified by a specific class name, "outlined", for easy access in the script.

The style property of *any* element (here, the first element selected) can be examined to determine if it supports the paint-order property.  A strict equality test (===) is used to distinguish an empty value (no inline style was set on the element) from an undefined value (the property name is not recognized).

If the fallback is required, the fakeOutline() method is called for each element that had the class name.  The forEach() array method is used to call the function as many times as needed.  However, since the list returned by getElementsByClassName() is not a true JavaScript Array object, you cannot use t.forEach(fakeOutline).  Instead, the forEach() function is extracted from the Array prototype and is invoked using its own call() method.

The fakeOutline() function will duplicate the outlined element with a <use> element, so it will need a valid id value; if it doesn’t already have one, an arbitray value is added with a unique index.

The element is replaced by a group that is transferred all of its classes.  This of course requires that all fill and stroke styles are assigned via class, and not by tag name, or via presentation attributes.  The insertBefore() method is used to ensure that the new group will have the same position in the DOM tree as the element it is replacing.

A nested group will hold the original element, but will prevent it from inheriting the fill style.

Finally, a <use> element duplicates the element, but cancels out the stroke style so that it only inherits fill styles.  It is inserted into the main group as the last child (“before” nothing), so that it will be drawn on top of the version with no fill.

As you can tell, the script is rather convoluted for such a simple effect.  Creating a more generic fallback script—a complete polyfill for the property—is even more difficult, as you need to account for all the different ways in which a style property can be applied to an element.  Effectively, you need to recreate the work of the CSS parser to identify the style rules it discarded because they were not recognized.

In most cases, if the final appearance is essential in all browsers, it is easier to create the layered stroke and fill copies of the object within your markup.
Take a Hint with Rendering Properties
One final class of style properties helps control *how* the browser applies paint data to graphics.  After that, the rest of the book will focus on *what* you are painting in those shapes.

These final properties are considered to be “hints” from you—the author of the SVG—to the browser or other software converting your code into colored pixels.  They offer suggestions as to which features you consider most important, if the browser has to compromise performance or appearance in some way.  From most to least likely to have a consistent impact, they are:

shape-rendering

How the browser should adjust the edges of the shape to accomodate limitations in the screen resolution.  There are four options:

- 
auto, the default, which uses the browser’s standard behavior.

- 
optimizeSpeed, which indicates that fast rendering is the most important feature (probably because the graphic is being animated); the edges of the shape may not be drawn precisely.  However, the exact changes may vary by browser and in many cases will be the same as auto.

- 
crispEdges, which indicates that the browser should maximize contrast around the edges of the fill and stroke regions. This usually means the edges will be *aliased* to the nearest pixel boundary, instead of having edge-pixels partially colored (*anti-aliased*).  For vertical and horizontal lines, this can create a sharper imaged, but results for curves and diagonal lines are usually less satisfactory.

- 
geometricPrecision, which indicates that the browser should represent the exact shape as precisely as possible, and may use anti-aliasing to do so if required.

text-rendering

How the browser should adjust the shape and position of letters within text.  The four options are:

- 
auto, the default browser behavior

- 
optimizeSpeed, which is the same as auto in most browsers; for large text, this might turn off text layout adjustments (Firefox by default uses legibility adjustments for text larger than 20px).

- 
optimizeLegibility, which indicates that the browser should use all information at its disposal to adjust the rendering of individual letters and the layout of strings of text to make it easier to read.  In practice, this has been adopted as a suggestion to turn on letter kerning and non-essential ligatures specified in the font file.

- 
geometricPrecision, which indicates that the browser should treat the letters as if they were geometric shapes, and draw them exactly without adjustments from resolution-based font hinting.

In SVG 1.1, the exact impacts of text-rendering were not well defined.  The property has since been adopted by browsers to apply to non-SVG content, and the exact impact of the different options will likely be clarified in future specifications (SVG 2 or a CSS module).

color-rendering

How precise the browser should be about calculating colors, particularly when blending elements or generating gradients.  The options use the standard hint keywords:

- 
auto

- 
optimizeSpeed

- 
optimizeQuality

image-rendering

How the browser should calculate the appearance of raster images when the displayed size of that image does not exactly match the number of pixels defined in the image file.  In SVG 1.1, the options were the standard auto, optimizeSpeed, and optimizeQuality. However, in practice it became clear that there was not always agreement on how to create a “quality” scaled-up image.  The algorithms that create the best results for photographs can create a blurred effect when the image contains sharp edges.

The CSS Images and Replaced Content module, level 3, has adopted the image-rendering property and deprecated the optimizeSpeed and optimizeQuality values.  The optimizeSpeed option is replaced by a pixelated value (scale up each pixel as a square).  An additional option, crisp-edges, would apply some smoothing but maintain high-contrast edges.

The optimizeQuality option, which had recommended smooth interpolation for photos is (at the time of writing this book) covered under the default auto setting, reflecting the practice in existing browsers.  There is some discussion of having a separate smooth property, to allow alternate auto options.

buffered-rendering

A new property introduced in SVG 2, the buffered-rendering hint would let browsers know whether the appearance of a given element or group of elements is likely to change, and thefore whether performance might be improved by storing the rasterized version as a image layer in a memory buffer.  The options are

- 
auto

- 
dynamic for content that will often change

- 
static foc content that is unlikely to change
For example, in a typical video-game interface, you might have a static background with some dynamic characters that you animate with scripting.

A related set of properties are the color-interpolation instructions, which we’ll discuss in Chapter 3.  The color-interpolation setting is not (supposed to be) a hint, but rather a requirement.  However, a value of optimizeSpeed for color-rendering gives the browser permission to ignore the color-interpolation mode if it would slow down the rendering.  In practice, however, low support for the color interpolation options makes this distinction irrelevant.