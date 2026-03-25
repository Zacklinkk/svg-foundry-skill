Chapter 12. Textured Text
We’ve now shown a few examples of using patterns to fill text, and we highlighted some other text styling options at the end of Chapter 11.  We also used stroked text to demonstrate the paint-order property back in Chapter 2.

Throughout, we’ve emphasized that filling and stroking text in SVG is similar to filling shapes.

For the most part, that’s true.  However, there are a few differences with respect to paint servers, which stem from the way object bounding boxes are calculated for text.  This chapter looks at paint servers and text more closely.
Bounding Text
Every element in SVG that is drawn to the screen has a bounding box.  Although they aren’t used in painting, so do groups and other container elements such as <use> and <svg>; these container bounding boxes *are* used for clipping, masking, and filters.  The bounding box is always a rectangle aligned with the axes of the coordinate system for that element (including any transformations).

For shapes, the box is always the *tightest* rectangle that can control the basic geometry of the shape.  For containers, it is the tightest rectangle that can contain all the child components.  For text, it is the tightest rectangle that can contain the *em boxes* for each character.

The *em box* in typography is the region in which each character is drawn.  It is always the full em height for the font, and the full width determined by the normal spacing of the letters.  In many cases, the actual letter only takes up a small part of this rectangle.  For other characters, the letter might extend outside of the rectangle, with swooshes that extend under or over other characters in the row of text.  As a result, the region of text that you’re filling may not match the object bounding box used to scale your paint server.

Example 12-1 defines a simple striped pattern, scaled to the object bounding box, and uses it to fill four different text elements.  Each element takes up a different amount of space within the drawing.  Figure 12-1 shows how they are painted.
Example 12-1. Filling text with an object bounding box pattern
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="400px" height="300px" viewBox="0 0 400 300">
    <title>Text Bounding Box</title>
    <style type="text/css">
        text {
            font-size: 50px;
            font-family: sans-serif;
            font-weight: bold;
        }
    </style>
    <pattern id="stripes" width="100%" height="20%"
             patternContentUnits="objectBoundingBox"> 
        <rect width="1" height="0.1" fill="indigo" />
        <rect width="1" height="0.1" y="0.1"
              fill="royalblue" />                     
     </pattern>
    <defs>
        <path id="p" d="M250,150 L350,200 250,270" />
    </defs>

    <g fill="url(#stripes)">                               
        <text x="50%" y="1em" dx="-10"
              text-anchor="end">Mixing</text>
        <text x="50%" y="1em" dx="10"
              text-anchor="start">BLOCK</text>             
        <text x="10%" y="3em"
              >Three <tspan x="10%" dy="1.2em"
              >line </tspan><tspan x="10%" dy="1.2em"
              >text</tspan></text>                         
        <text><textPath xlink:href="#p"
              >TextPath</textPath></text>                  
    </g>
</svg>

Each pattern tile extends the width of the bounding box and one-fifth (20%) of its height.

The pattern contents are also scaled to the bounding box.  Each stripe is one-tenth (0.1) of the bounding box height.

A group applies the pattern fill to all of the <text> elements.  As usual, each element will be filled based on its own bounding box.

The first two elements are aligned horizontally.  One has mixed-case letters, the other is all block capitals.

The next element uses positioned <tspan> segments to break it across three lines.

The final text element is arranged along a <textPath> that takes up approximately the same height on the page as the three-line text.

The first thing to notice in Figure 12-1 is that the stripes are different sizes in the multi-line and <textPath> text than in the single-line text elements.  These elements have larger bounding boxes, which create larger pattern tiles.

If you look specifically at the top row (the two single-line elements), you’ll notice that you can’t count a full five pairs of stripes from top to bottom of the letters.  You’ll also notice that the stripes are the same size for both words, despite the fact that the block capitals do not descend below the baseline.  This is a consequence of calculating the bounding box based on the full em-height of each character, even if the characters used do not fill that space.

The larger elements are likewise slightly smaller than their bounding boxes, although the unused parts make up a smaller proportion of the larger bounding box.
Warning
For <textPath> and other text elements with rotated characters, many browsers (Blink, Webkit and Internet Explorer at the time of writing) rotate the pattern along with the letter.  For Example 12-1, this results in patchy, discontinuous stripes.  Figure 12-1 is based on the rendering in Firefox (v37).

Example 12-1 is an example of a pattern in which the pattern contents, in object bounding-box units, directly depend on the pattern tile size.

A viewBox can be used to define a complete coordinate system for the contents, independent of the tile size.  A none setting for aspect ratio control then stretches that square to fill the full tile, regardless of any changes to the tile size.  This still creates a distorted coordinate system, but for rectangular stripes that isn’t a problem.
Warning
As mentioned in Chapter 11, Firefox up to version 40 does not render patterns with a viewBox correctly.

Example 12-2 shows how this would work for the stripe pattern.  To demonstrate the independence between pattern content and pattern tile, two variations of the pattern are created—one with thick stripes and one with thin stripes—by duplicating the pattern content with an xlink:href reference.
Example 12-2. Creating scalable bounding-box patterns with viewBox, without preserving aspect ratio
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="400px" height="70px" viewBox="0 0 400 70">
    <title>Adjustable Stripes</title>
    <style type="text/css">
        text {
            font-size: 50px;
            font-family: sans-serif;
            font-weight: bold;
        }
    </style>
    <pattern id="thick-stripes" width="100%" height="40%"
             viewBox="0 0 2 2" preserveAspectRatio="none">
        <rect width="2" height="1" fill="indigo" />
        <rect width="2" height="1" y="1"
              fill="royalblue" />
    </pattern>
    <pattern id="thin-stripes" height="12.5%"
             xlink:href="#thick-stripes" />

    <text x="50%" y="1em" dx="-10" fill="url(#thick-stripes)"
          text-anchor="end">Stripy</text>
    <text x="50%" y="1em" dx="10" fill="url(#thin-stripes)"
          text-anchor="start">Stripy</text>
</svg>

Each rectangle is half the height of the viewBox and its full width.  The number of horizontal stripe that fit within the bounding box height is therefore controlled only by the single height attribute on the pattern, and can easily be over-ridden in a second <pattern> element referencing the first.
Switching Styles Mid-Stream
Although each <text> element in Example 12-1 creates its own object bounding box for painting, the individual <tspan> elements in the multi-line text do not.  This is always true despite the fact that individual <tspan> elements can have different fill settings from the rest of the text.
Tip
If the fill for a <tspan> uses an object bounding box scale, it is based on the scale of the entire <text> element, not the individual span.

Example 12-3 demonstrates this effect by using a bounding-box gradient to fill three separate <tspan> elements within a larger <text> element.  It also provides a slightly more readable example of using a pattern to fill text; instead of bold stripes, it uses a more subtle texture to recreate the effect of chalk on a blackboard.  Figure 12-3 shows the result.
Example 12-3. Filling text with an object bounding box pattern
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="10.9cm" height="5cm" >
    <title>Chalkboard Text</title>
    <style>
        text {
            font-family: cursive;
            font-weight: bold;
            font-size: 9mm;
            font-size-adjust: 0.54;              
        }
        .chalk {
            fill: url(#chalk-texture);
        }
        .chalk-marks {
            fill: white;
            fill-opacity: 0.8;
            stroke: silver;
            stroke-width: 0.2px;
        }
        .highlight {
            fill: url(#highlight-gradient);
        }
        .blackboard {
            fill: black;
        }
    </style>
    <pattern id="chalk1" class="chalk-marks"
             patternUnits="userSpaceOnUse"
             width="9" height="6">                         
        <circle r="1" cx="0" cy="3" />
        <circle r="1" cx="1" cy="5" />
        <circle r="1" cx="2" cy="1" />
        <circle r="1" cx="3" cy="2" />
        <circle r="1" cx="4" cy="5" />
        <circle r="1" cx="5" cy="4" />
        <circle r="1" cx="6" cy="0" />
        <circle r="1" cx="7" cy="6" />
        <circle r="1" cx="8" cy="3" />
    </pattern>
    <pattern id="chalk2" xlink:href="#chalk1"
             x="2" width="5" height="5"/>                  
    <pattern id="chalk3" xlink:href="#chalk1"
             y="4" width="7" height="7"/>
    <pattern id="chalk-texture" patternUnits="userSpaceOnUse"
             width="315" height="210">                     
        <rect fill="url(#chalk1)" width="315" height="210"/>
        <rect fill="url(#chalk2)" width="315" height="210"/>
        <rect fill="url(#chalk3)" width="315" height="210"/>
    </pattern>

    <linearGradient id="highlight-gradient" x2="0" y2="1">
        <stop stop-color="gold" offset="0.3" />            
        <stop stop-color="deeppink" offset="0.7" />
    </linearGradient>

    <rect class="blackboard" width="100%" height="100%" />
    <text y="5mm" class="chalk" textLength="32cm">         
        <tspan x="3mm" dy="1em"><tspan
               class="highlight">Textured</tspan>          
            Text Effects</tspan>
        <tspan x="3mm" dy="1.6em">Textured
            <tspan class="highlight">Text</tspan> Effects</tspan>
        <tspan x="3mm" dy="1.6em">Textured
            Text <tspan class="highlight">Effects</tspan></tspan>
    </text>
</svg>

You don’t have to use px or pt to size text: the SVG is scaled to use metric units, so the text is as well.  A font-size-adjust setting ensures that the apparent font size will remain approximately the same regardless of the actual font used; the 0.54 value is based on Comic Sans MS, the default cursive font on a Windows computer.

The basic chalk pattern is created from nine erratically-positioned white and gray dots within a small pattern tile.  The styling for the circles is set by the chalk-marks class on the <pattern> itself.

To make the pattern denser and more irregular, it is duplicated twice, but with different pattern tile sizes and offsets.

A composite pattern is then created by layering the three chalk mark patterns together.  The composite pattern uses userSpaceOnUse units to define a large pattern tile that is just large enough to contain an even number of repeats of each of the layered patterns.

The gradient used for the highlighted spans is a simple vertical linear gradient scaled to the object bounding box (default gradientUnits).  Approximately the first third (to offset 0.3) is solid gold—in color, if not in value—and the last third (after offset 0.7) is solid pink, with a transition in between.

The text is contained within a single <text> element.  A textLength attribute ensures that the total length of three lines of text is adjusted to slightly less than three times the width of the SVG.

Three <tspan> elements break the text into separate lines.  Within each line, a <tspan> with class highlight changes the fill of a single word.

The gradient-filled words within Figure 12-3 most obviously show how the entire <text> element is a single bounding box.  However, it is also evident in the chalk texture.  If each row of text were its own element, filled with the same pattern, the cracks and irregularities in the fill would be positioned at the exact same points.  Instead, since each of the component patterns repeats on its own scale—and none of those exactly match the height of a line of text—the result looks appropriately random.

Example 12-3 uses the browser’s default cursive font.  In general, that isn’t recommended, since they can vary so much from one browser or operating system to another.  Normally, you would give a list of specific font-family names, or use web fonts to be sure that the correct font was available.  However, sometimes those strategies fail, so it is worth paying attention to the other code attributes that encourage a consistent rendering: the textLength attribute and the font-size-adjust style.
Warning
The textLength attribute—which squeezes or stretches text to the given length—is not supported in some older SVG tools, and is buggy in Internet Explorer.

The font-size-adjust property, which tells the browser to preserve the ex-height of the font instead of the em-height (by specifying the ratio between the two), is currently only supported in Firefox.

Because of those settings, the overall layout is preserved (in a conforming web browser) even if a completely different font is used, as shown in Figure 12-4.