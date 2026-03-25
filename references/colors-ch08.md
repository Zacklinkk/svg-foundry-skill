Chapter 5. Serving Paint
When the fill or stroke is more complicated than a single color (transparent or otherwise), SVG uses a concept called a *paint server* to describe how the graphic is rendered.

The paint servers are defined using their own SVG elements.  Those elements—gradients and patterns—do not directly create any visible graphics.  They are only used through the fill and stroke properties of shapes and text.  However, by using XML markup to define the paint server, it can be infinitely variable: any SVG graphics can be used to generate an SVG pattern, including other patterns!

In contrast, in CSS styling, all the information about how to paint an element must be contained within the CSS style rules.  In CSS 2.1, the only way to create patterns was to reference external image files.  Since then, CSS has introduced many graphical effects, such as gradients and compound patterns, which were previously only possible with SVG.  Although the end result may look similar, the all-CSS syntax for these new properties is quite different from their SVG equivalent.  Throughout the rest of the book, the two approaches will be compared under the heading “CSS versus SVG”.

This chapter introduces the basic paint server model, and then demonstrates how it can be used in the simplest case, to serve up a single color of paint.
Paint and Wallpaper
A key feature of all SVG paint servers is that they generate a rectangular region of graphical content.  This can be limiting at times, but it allows for underlying performance optimizations.

An SVG paint server doesn’t need to know anything about the shape it is told to fill, it just slops on paint indescriminately all over the wall.  The shape itself acts as a mask or stencil that blocks off which parts of the paint actually reach the drawing, in the same way that a wall painter covers (*masks*) floorboards, ceilings, light fixtures, etc., so that only the wall gets painted.

Another way of thinking about paint servers—particularly when talking about gradients and patterns—is to picture the paint content as a large sheet of wallpaper.  The shape is cut out from that sheet, as imagined in Figure 5-1.

The computer doesn’t use paper and scissors, of course; instead, as it rasterizes (scans across) the shape, for every point that is “inside” the filled region, the computer looks up the corresponding (*x*,*y*) point from the paint server.  A paint server can therefore be any object that can assign a specific color value to each (*x*,*y*) value.

In theory, the “paint” could be anything: a single color, one or more gradients, repeating patterns, bitmap graphics, text, even other SVG files.  In practice, SVG 1.1 has two types of paint servers, gradients and repeating patterns.  However, those core elements can be used to create all of the options just mentioned, as the rest of the book will demonstrate.
Identifying Your Assets
The name “server” suggests an exteral source for multiple resources.  Theoretically, you can create a separate asset file containing all your paint servers and reference it from the fill or stroke property, but this currently has poor browser support.  More generally, the name *paint server* refers to the fact that each gradient or pattern object can serve paint (rendering instructions) to multiple SVG shapes.
Warning
At the time of writing, external paint servers are only supported in Firefox and in pre-Blink versions of Opera that use the Presto rendering engine.

In order to use a paint server, you reference the paint server element using URL syntax, wrapped in the CSS url() functional notation.  Because of the browser support limitation, this URL is nearly always an internal reference like url(#myReference).  The hash-mark (#) indicates that what follows is a target towards a specific element; the fact that there is nothing *before* the hash-mark indicates that the browser should look for that element in the current document.  Specifically, it should look for an element with an id attribute that matches the target fragment (i.e., <pattern id="myReference">).

Thus, referencing a paint server with an ID of "customBlue" as a fill would look something like:

<rect fill="url(#customBlue)" width="100" height="100"/>

Because fill is a presentation attribute, you could also use a <style> block elsewhere in the document to set the value:

rect {
    fill: url(#customBlue);
}

The above rule would set all rectangles in the document to use that paint server, provided that the style wasn’t over-ridden by more specific CSS rules.

Relative URLs in *external* stylesheets are always relative to the CSS file location, not the location of the document using those styles.  This includes local URL fragments like #customBlue, which will never match anything if specified in an external CSS file.  In combination with the lack of support for external paint servers, this unfortunately means that you cannot effectively use external style sheets to set paint server references.
Tip
Relative URLs are also affected by the xml:base attribute or the HTML <base> element; using either can cause your paint server references to fail.

In theory (or if you only need to support Firefox), if you  had a set of colors that are predefined in a file called *brand.svg*, you could provide the relative path to that resource, then use the target fragment to point to the specific element:

<rect fill="url(brand.svg#customBlue)"
      width="100" height="100"/>

Or you could even provide the absolute URI to that same resource—assuming the external file could be securely accessed from your web domain:

<rect fill="url(//example.com/assets/brand.svg#customBlue)"
      width="100" height="100"/>

The lack of support for this option is unfortunate, because the server concept can be thought of as being just another form of asset library, a way of storing commonly used colors, gradients, patterns, masks and other resources in a single file.  For now, if you have paint servers that are used by multiple SVGs, you need to incorporate them directly in each document, either by using some pre-processing on your server or by using AJAX techniques to import them from the user’s browser.

Because numerous things might interfere with the ability to load an external resource—even separate from browser support—the SVG fill and stroke properties allow you to specify a fallback color value.  The color is given after the url() reference, separated by whitespace, like the following:

rect {
    fill: url(brandColors.svg#customBlue) mediumBlue;
}

Or, using presentation attributes and hex color syntax:

<rect fill="url(brandColors.svg#customBlue) #0000CD"
      width="100" height="100"/>

If the referenced paint server cannot be loaded, the rectangles will be painted with the specified solid blue color.
The Solid Gradient
Often times, especially when working with commercial uses of color, a designer will give that color a specific name.  The same color may show up in many graphics related to the brand: different versions of the company logo, heading text, product labels, and so on.  Rather than having to keep a list of RGB values for each color, it is much easier to define them once, give them a name, and then use that name in the content.  This also makes it much easier if you decide to change one of the colors later on in the design process!

An SVG paint server is ideally suited for this task.  It can be referenced by ID in the fill or stroke properties of multiple graphics, but the actual color value is only specified once and can be easily updated (or animated, as we’ll show in Chapter 14).

The original SVG specifications did not explicitly include a solid color paint server, but all browsers allow you to use a gradient with a single, un-changing color to this effect.  Example 5-1 demonstrates this strategy; it uses <linearGradient> elements to define four named colors that are used in the branding strategy for the fictional company ACME.  The colors are then used to draw a company logo, which is shown in Figure 5-2.
Example 5-1. Defining named colors for consistent branding using single-color gradients
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="160mm" height="80mm">                        
    <title>ACME Logo</title>
    <defs>
        <linearGradient id="AcmeRed">                    
            <stop stop-color="#FF4022" />
        </linearGradient>
        <linearGradient id="AcmeMaroon">
            <stop stop-color="#80201C" />
        </linearGradient>
        <linearGradient id="AcmeGold">
            <stop stop-color="#FFFC32" />
        </linearGradient>
        <linearGradient id="AcmeWhiteGold">
            <stop stop-color="#FFFCE0" />
        </linearGradient>
        <symbol id="AcmeLogo" viewBox="0,-40 160,80" >   
            <path d="M0,0 L40,-40 L40,-20 L160,-20
                     L160,20 L40,20 L40,40z"
                  fill="url(#AcmeRed)"/>                 
            <path d="M16,-10 L35,-29 L35,-15 L155,-15 L155,-10 z"
                  fill="url(#AcmeGold)"/>
            <path d="M13,-7 L16,-10 L155,-10 L155,-7 z"
                  fill="url(#AcmeMaroon)"/>
            <text x="40" y="15"
                  style="font-family:Arial; font-weight:bold;
                         font-size:20pt;"
                  fill="url(#AcmeWhiteGold)">ACME</text>
        </symbol>
    </defs>
    <use xlink:href="#AcmeLogo" />                       
</svg>

The SVG does not have a viewBox; scaling is controlled by the <symbol> element that contains the logo.  However, default width and height values ensure that the image has the correct intrinsic aspect ratio and a reasonable default size when embedded in other web pages.

The company has four brand colors, AcmeRed, AcmeMaroon, AcmeGold, and AcmeWhiteGold.  Each color is defined as a paint-server using a <linearGradient> with a single <stop> element.

The logo itself is defined inside a <symbol> element for easy re-use in other graphics.  The viewBox creates a coordinate system that is centered on the vertical axis.

Each shape within the symbol uses one of the pre-defined paint servers to set the fill color.

The logo is drawn within the SVG with a <use> element.  Since the <use> element does not have any positioning or sizing attributes, the re-used <symbol> will scale to fill the entire SVG area.

Examining the gradients more closely, each consists of two elements, <linearGradient> and <stop>:

<linearGradient id="AcmeRed">
    <stop stop-color="#FF4022" />
</linearGradient>

The <linearGradient> defines the paint server, and gives it the id value that will be used to reference it.  This gradient element is also a container for the <stop> element that defines the color.  For a normal gradient, there would be multiple stops defining the initial, final, and intermediary colors.  The color is specified using the stop-color presentation attribute.  There is also a stop-opacity presentation attribute, similar to fill-opacity or stroke-opacity; by default colors are fully opaque.
Warning
Although Example 5-1 works as intended in every web browser tested, it fails in Apache Batik, which is more strict on syntax.  To make it work, the <stop> elements also require an offset attribute, which we’ll discuss in “Gradiated Gradients”.

Because the colors are defined in a single location, they can be changed easily and consistently, or animated uniformly.  Because stop-color is a presentation attribute, you don’t even need to edit the XML to change the color; you can over-ride it with CSS rules.

As a result, you can use conditional CSS rules to change the color.  A stylesheet with media queries can be used to assign print colors for high quality printers, or for grayscale printing.  Because the color is used by reference in the rest of the graphic, the stylesheet does not need to identify all the elements that use each color, nor does it need to distinguish between fill and stroke values.
Tip
Although stop-color is a presentation attribute, it is not inherited by default.  It must be explicitly set on the <stop> element, either directly or by using the inherit keyword.

Example 5-2 gives a sample set of print styles.  For color printing, it redefines the colors using HSL notation which can then be mapped to the full color gamut used on the print device.  For monochrome printing, it assigns each color to a shade of gray that will create stronger contrast than if the colors were converted to gray automatically.  The grayscale version is shown in Figure 5-3.
Example 5-2. Re-defining named colors for print graphics
@media print AND (color) {
    #AcmeRed stop       { stop-color: hsl(10, 100%, 60%); }
    #AcmeMaroon stop    { stop-color: hsl( 0,  65%, 30%); }
    #AcmeGold stop      { stop-color: hsl(60, 100%, 60%); }
    #AcmeWhiteGold stop { stop-color: hsl(55, 100%, 90%); }
}
@media print AND (monochrome) {
    #AcmeRed stop       { stop-color: #555; }
    #AcmeMaroon stop    { stop-color: #222; }
    #AcmeGold stop      { stop-color: #DDD; }
    #AcmeWhiteGold stop { stop-color: #FFF; }
}
Warning
Although most browsers correctly apply CSS print styles when printing a web page, they do not always apply monochrome styles when the user chooses to print in black and white on a color printer.

Using paint servers to name non-standard colors in this way has the additional advantage that it makes your code easier to read by others.  By using meaningful ID values, the color and purpose of each element becomes apparent to any programmer who has to adapt your work in the future.