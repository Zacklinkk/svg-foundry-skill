Chapter 1. Things You Should Already Know
The rest of this book is written with the assumption that you already know something about SVG, web design in general, and maybe even a little JavaScript programming.

However, there are always little quirks of a language that some people think are straightforward and other, equally talented, developers have never heard of.  So this chapter gives a quick review of topics that you might want to brush up on—if you don’t already know them.

SVG is Drawing with Code

An SVG is an image file.  It is perfectly possible to only use it as an image file, the same way you would use other image formats such as PNG or JPEG.  You can create and edit an SVG in a visual editor.  You can embed it in web pages as an image.

But SVG is more than an image.  It is a structured document containing markup elements, text, and style instructions.  While other image formats tell the computer which color to draw at which point on the screen, SVG tells the computer how to rebuild the graphic from its component parts.  That has two main consequences:

- 
The final appearance of an SVG depends on how well the computer displaying it follows the SVG instructions.  Cross-browser compatibility is often a concern.

- 
It is easy to edit an SVG, to add, remove, or modify parts of it without changing the rest.  You can do this in your editor, but you can also do it dynamically in your web browser to create animated or interactive graphics.

SVG is Always Open Source

Not only is an SVG a set of coded instructions for a computer, it is also a human-readable text file.  You can edit your SVG in a text editor.  Even better, you can edit SVG in a code editor with syntax highlighting and autocomplete!

The examples in this book all focus on the basic SVG code.  You can, of course, use a visual editor to draw shapes, select colors, and otherwise fuss with the appearance of your graphic.  But for full control, you will need to take a look at the actual code that editor creates.

SVG is XML (and sometimes HTML)

The SVG code you view in your text editor looks an awful lot like HTML code—full of angle brackets and attributes—but a stand-alone SVG file is parsed as an XML document.  This means that your SVG can be parsed and manipulated by tools meant for XML in general.  It also means that your web browser won’t display anything if you forget to include the XML namespaces or mixed up an important detail of XML syntax.

Nonetheless, when you insert SVG code directly in HTML 5 markup, it is processed by the HTML parser.  The HTML parser forgives errors (like missing closing tags or unquoted attributes) that would halt the XML parser.  But it also ignores any custom namespaces, downcases any unrecognized attribute or tag names, and otherwise changes things up in ways you might not expect.

SVG is Squishable

The syntax for SVG was designed to make it easy to read and understand, not to make it compact.  This can make certain SVG files seem rather verbose and redundant.  However, it also makes SVG very suitable for gzip compression, which should always be used when serving SVG on the web.  It will usually reduce file sizes by more than half, sometimes much more.  If storing a gzipped SVG on a regular file server, it is typical to use the *.svgz* extension.

SVG is also bloatable, which makes it squishable in another way.  SVG editors can add their own elements and attributes to an SVG file by giving them unique XML namespaces.  A class of SVG optimizing tools has developed that will strip out code that does not affect the final result.  Just be careful about the settings you use—optimizers can remove attributes you might want later if you’re manipulating the code yourself!

Pictures are a Collection of Shapes

So what does all that code represent?  Shapes, of course! (And text and embedded images, but we’ll get to that in a moment…)  SVG has only a few different shape elements: <rect>, <circle>, <ellipse>, <line>, <polyline>, <polygon>, and <path>.  However, those last three can be extensively customized to represent any shape you can imagine, to a certain degree of precision.  The <path> in particular contains its own coded language for describing the curves and lines that create that shape.

Images Can Have Images Inside Them

An SVG is an image, but it is also a document, and that document can contain other images, using the <image> element.  The embedded images could be other SVG files, or they could be raster images such as PNG or JPEG.  However, for security and performance reasons, some uses of SVG prevent those external images (and other external resources such as stylesheets or fonts) from being downloaded.

Text is Art

The one other building block in SVG is text.  But text isn’t an alternative to graphics—the letters that make up that text are treated like another type of vector shape.  Importantly for this book, text can be painted using the exact same style properties as vector shapes.

Art is Math

The core of all vector graphics (shapes or text) is that the end result can be defined using mathematical parameters (the XML attributes) to the browser’s SVG rendering functions for each element.  The most pervasive mathematical concept in SVG is the coordinate system, used to define the position of every point in the graphic.  You can control the initial coordinate system by setting a viewBox attribute, and you can use coordinate system transformations to shift, stretch, rotate and skew the grid for certain elements.

An SVG is a Limited View of an Infinite Canvas

There are no limits on the coordinates you can give for your vector shapes (except for the practical limits of computer number precision).  The only shapes displayed, however, are those that fit within the particular range of coordinates established by the viewBox attribute.  This range of coordinates is scaled to fit the available area (the “viewport”), with accomodations for mismatched aspect ratios controlled by the preserveAspectRatio value.  You can create nested viewports with nested <svg> elements or re-used <symbol> elements; in addition to providing regions of aspect ratio control, these re-define how percentage lengths are interpretted for child content.

The distinction between viewBox and “viewport” is one of the more confusing aspects of the SVG specs.  As we’ll see when we get to Chapter 11, the <pattern> element (and also the <marker> element) can have a viewBox attribute, but does not establish a viewport for the purposes of percentage lengths.

SVG has Structure

The structure of an SVG includes the basic shapes, text, and images that are drawn to the screen, and the attributes that define their geometry.  But SVG can have more structure than that, with elements grouped into logical clusters.  Those groups can be styled and their coordinate systems transformed.  But they can also be given accessible names and descriptions to help explain exactly what the graphics represent.

SVG has Style

SVG graphics can consist solely of XML, with all style information indicated by presentation attributes.  However, these presentation styles can also be specified with CSS rules, allowing styles to be grouped by class or element type.  Using CSS also allows conditional styles to depend on media features or transient states such as :hover or :focus.

The strict separation between geometric structure (XML attributes) and presentation style (presentation attributes or CSS style rules) has always been a little arbitrary.  As SVG moves forward, expect it to collapse even more.  The SVG 2 draft specifications promote many layout attributes to become presentation attributes.  This opens these properties to all the syntactic flexibility CSS offers: classes of similar elements can be given matching sizes with a single style rule, and those sizes or layout can be modified with CSS pseudoclasses or media queries.

Behind All Good Markup is a Great DOM

The SVG markup and styles are translated into a document object model (DOM) within a web browser.  This DOM can then be manipulated using JavaScript.  All the core DOM methods defined for all XML content apply, so you can create and re-order elements, get and set attributes, and query the computed style values.

The SVG specifications define many unique properties and methods for SVG DOM elements.  These make it easier to manipulate the geometry of a graphic mathematically.  Support for SVG DOM in web browsers is not as good as one might hope, but certain methods—such as determining the length of a curved path—are indispensable for SVG designs.

SVG Can Move

In a dynamic SVG viewer (e.g., web browser) with scripting support, you can use those scripts to create animated and interactive graphics.  However, SVG also supports declarative means of interaction, whereby you define the scope of an entire interaction and the browser applies it with its own optimizations.  There are two means of doing this:

- 
using animation elements in the markup, with a syntax borrowed from the Synchronized Multimedia Integration Language (SMIL)

- 
using CSS animations and transitions of presentation styles

At the time of writing, scripted animation is supported in all web browsers, but may be blocked for certain uses of SVG.  Declarative animation (SMIL and CSS) is supported in most browsers, but not all (particularly, not Internet Explorer).

SVG Can Change

Not only can individual SVG graphics change as you use them, but the definition of SVG can change too.  The established standard (at the time this book was written) is SVG 1.1, but work is ongoing to develop a level 2 SVG specification with new features and clearer definitions of some existing features.  Furthermore, because SVG uses CSS and JavaScript, and because it is heavily integrated in HTML, it inherits changes to those languages, as well.