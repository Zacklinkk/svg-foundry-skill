Chapter 1. Using SVG in Web Pages
From *SVG Essentials*

John Donne said that no man is an island, and likewise SVG does not exist in isolation.  Of course, you can view SVG images on their own, as an independent file in your web browser or SVG viewer.  Many of the examples in this book work that way.  But in other cases, you will want your graphic to be integrated in a larger document, which contains paragraphs of text, forms, or other content that cannot easily be displayed using SVG alone. This chapter describes various ways of integrating SVG within HTML and other document types.

Figure 1-1 shows the cat drawing from another chapter of *SVG Essentials*, inserted into an HTML page in four different ways.  The results look almost identical, but each method has benefits and limitations.
SVG as an Image
SVG is an image format, and as such it can be included in HTML pages in the same ways as other image types.  There are two approaches: you can include the image within the HTML markup in an <img> element (recommended when the image is a fundamental part of the page’s content); or you can insert the image as a CSS style property of another element (recommended when the image is primarily decorative).

Regardless of which method you use, including SVG as an *image* imposes certain limitations.  The image will be rendered (“drawn” in the sense that the SVG code is converted to a raster image for display) separately from the main web page, and there is no way to communicate between the two.  Styles defined on the main web page will have no effect on the SVG.  You may need to define a default font size within your SVG code if your graphic includes text or defines lengths relative to the font size.  Furthermore, scripts running on the main web page will not be able to discover or modify any of the SVG’s document structure.

Most web browsers will not load files referenced from an SVG used as an image; this includes other image files, external scripts, and even webfont files.  Depending on the browser and the user’s security settings, scripts defined within the SVG file may not run, and URL fragments (the part of the URL after #, which indicates which part of the file you’re interested in) may be ignored.  Animation, as defined in Chapter 12 in *SVG Essentials*, *is* supported within images (in browsers that support it in SVG in general).
Including SVG in an <img> Element
The HTML <img> element defines a space into which the browser should draw an external image file.  The image file to use is specified with the src (source) attribute. Including an SVG image within an <img> element is as simple as setting the source to point to the location of your SVG file on the web server.  Of course, you should also give a description with an alt and/or a title attribute so that users who cannot see the image can still understand what it represents. For example:

<img src="cat.svg" title="Cat Image"
       alt="Stick Figure of a Cat" />
Warning
Although most web browsers now support SVG as images, some older browsers will not know how to render the file and will display a broken-file icon (or nothing at all).  For other browsers, you may need to confirm that your web server is configured to declare the correct media type header (image/svg+xml) for files ending in *.svg*.

The height and width of the image can be set using attributes or CSS properties (which take precedence).  Other CSS properties control the placement of the image within the web page.  If you do not specify dimensions for the <img> element, the intrinsic dimensions of the image file are used.  If you specify only *one* of height or width, the other dimension is scaled proportionally so that the aspect ratio (the ratio of width to height) matches the intrinsic dimensions.

For raster images, the intrinsic dimension is the image size in pixels.  For SVG, it’s more complicated. If the root <svg> element in the file has explicit height and width attributes, those are used as the intrinsic dimensions of the file.  If one of height *or* width is specified, but not both, and the <svg> has a viewBox attribute, then the viewBox will be used to calculate the aspect ratio and the image will be scaled to match the specified dimension.  Otherwise, if the <svg> has a viewBox attribute but no dimensions, then the height and width parts of the viewBox are treated as lengths in pixels. If that all sounds incomprehensible, rest assured: we’ll introduce the viewBox attribute properly in Chapter 3 in *SVG Essentials*.

If neither the <img> element nor the root <svg> element has any information about the size of the image, the browser *should* apply the default HTML size for embedded content, 150 pixels tall and 300 pixels wide, but it is best not to rely on this.
Including SVG in CSS
Various CSS style properties accept a URL to an image file as a value.  The most commonly used is the background-image property, which draws the image (or multiple layered images) behind the text content of the element being styled.

By default, a background image is drawn at its intrinsic dimensions and repeated in both the horizontal and vertical direction to fill up the dimensions of the element.  The intrinsic dimensions of an SVG file are determined in the same manner as described in “Including SVG in an <img> Element”.  If there are no intrinsic dimensions, the SVG will be scaled to 100% of the height and width of the element.  The size can be set explicitly using the background-size property, and repeat patterns and image position can be set using background-repeat and background-position:

div.background-cat {
 background-image: url("cat.svg");
 background-size: 100% 100%;
}

In addition to background images, SVG files can be used in CSS as a list-image (used to create decorative bulleted lists) or border-image (used to create fanciful borders).
Note
When using raster images for multiple small icons and logos, it is common to arrange all the images in a grid within a single image file, and then use background-size and background-position to display the correct image for each element.  That way, the web browser only has to download one image file, resulting in much faster display of the web page. The compound image file is called a CSS *sprite*, named after a mythical helpful elf that magically makes things easier.  SVG files can be designed as sprites, and browsers are getting better at rendering them efficiently, but you should probably avoid making the sprite file too big.

The SVG specifications define other ways to create multiple icons within a single image file; you then use URL fragments to indicate which icon to display.  Ideally, these would replace sprites based on the background-position property.  However, as mentioned previously, some browsers ignore URL fragments when rendering SVG as an image, so these features are not currently of much practical use in CSS.
SVG as an Application
To integrate an external SVG file into an HTML page without the limitations of treating the SVG as an image, you can use an embedded object.

The <object> element is the general-purpose way of embedding external files in HTML (version 4 and up) and XHTML documents.  It can be used to embed images, similar to <img>, or to embed separate HTML/XML documents, similar to an <iframe>.  More importantly, it can also be used to embed files of any arbitrary type, so long as the browser has an application (a browser plug-in or extension) to interpret that file type.  Using an object to embed your SVG can make your graphic available to users of older browsers that cannot display SVG directly, so long as they have an SVG plug-in.

The type attribute of the <object> element indicates the type of file you’re embedding. The attribute should be a valid Internet media type (commonly known as a MIME type). For SVG, use type="image/svg+xml".

The browser uses the file type to determine how (or if) it can display the file, without having to download it first.  The location of the file itself is specified by the data attribute.  The alt and title attributes work the same as for images.

The object element must have both a start and end tag.  Any content in between the two will be rendered only if the object data itself cannot be displayed.  This can be used to specify a fallback image or some warning text to display if the browser doesn’t have any way of displaying SVG.1 The following code displays both a text explanation and a raster image in browsers that don’t support SVG:

<object data="cat.svg" type="image/svg+xml"
       title="Cat Object" alt="Stick Figure of a Cat" >
    <!-- As a fallback, include text or a raster image file -->
    <p>No SVG support! Here's a substitute:</p>
    <img src="cat.png" title="Cat Fallback"
       alt="A raster rendering of a Stick Figure of a Cat" />
  </object>

When you include an SVG file as an embedded object (whether with <object> or <embed>), the external file is rendered in much the same way as if it was included in an <img> element: it is scaled to fit the width and height of the embedding element, and it does not inherit any styles declared in the parent document.

Unlike with images, however, the embedded SVG can include external files, and scripts can communicate between the object and the parent page, as described in Chapter 13 in *SVG Essentials*.
SVG Markup in a Mixed Document
The image and application approaches to integrating SVG in a web page are both methods to display a complete, separate, SVG file.  However, it is also possible to mix SVG code with HTML or XML markup in a single file.

Combining your markup into one file can speed up your web page load times, because the browser does not have to download a separate file for the graphic.  However, if the same graphic is used on many pages on your website, it can increase the total download size and time by repeating the SVG markup within each page.

More importantly, all the elements within a mixed document will be treated as a single document object when applying CSS styles and working with scripts.
Foreign Objects in SVG
One way of mixing content is to insert sections of HTML (or other) content within your SVG.  The SVG specifications define a way of embedding such “foreign” content within a specified region of the graphic.

The <foreignObject> element defines a rectangular area into which the web browser (or other SVG viewer) should draw the child XML content.  The browser is responsible for determining *how* to draw that content.   The child content is often XHTML (XML-compliant HTML) code, but it could be any form of XML that the SVG viewer is capable of displaying.  The type of content is defined by declaring the XML namespace on the child content using the xmlns attribute.

The rectangular drawing area is defined by the x, y, width, and height attributes of the <foreignObject> element, in a manner similar to the <use> or <image> elements, which we’ll get to in Chapter 5 in *SVG Essentials*.

The rectangle is evaluated in the local SVG coordinate system, and so is subject to coordinate system transformations (which we’ll talk about in Chapter 6 in *SVG Essentials*) or other SVG effects. The child XML document is rendered normally into a rectangular frame, and then the result is manipulated like any other SVG graphic.  An SVG foreign object containing an XHTML paragraph is shown in Figure 1-2.

The <foreignObject> element has great potential for creating mixed SVG/XHTML documents, but is currently not well supported.  Internet Explorer (at least up to version 11) does not support it at all, and there are bugs and inconsistencies in the other browsers’ implementations.

If you want to define fallback content in case the SVG viewer cannot display foreign content, you can use the <switch> element in combination with the requiredFeatures attribute, as shown in Example 1-1.  In browsers that support XHTML and foreign objects, that code creates Figure 1-2; in other browsers, it displays SVG text.

The <switch> element instructs the SVG viewer to draw only the first direct child element (and all of *its* children) for which the requiredFeatures, requiredExtensions, and systemLanguage test attributes either evaluate to true or are absent.  We’ll discuss the use of the systemLanguage attribute to switch between different texts in Chapter 9 in *SVG Essentials*.  When testing for required features, you use one of the URL strings given in the specifications; <foreignObject> support is part of the Extensibility feature.
Warning
Unfortunately, there is no consistent, cross-browser way to specify *which* type of foreign object is required.  Maybe you want to use the MathML language to display a formula for your chart, with a plain-text version as a fallback for browsers that don’t understand MathML.  The requiredExtensions attribute is supposed to indicate what type of added capability is needed, but the SVG 1.1 specifications did not clearly describe how the extensions should be identified—except to say that it should be with a URL.  Firefox uses the XML namespace URL, but other browsers do not.
Example 1-1. The <foreignObject> element, with a <switch>
<g transform="skewX(20)">
<switch>
  <!-- select one child element  -->
  <foreignObject x="1em" y="25%" width="10em" height="50%"
     requiredFeatures=
     "http://www.w3.org/TR/SVG11/feature#Extensibility">
     <body xmlns="http://www.w3.org/1999/xhtml">
        <p>This is an XHTML paragraph embedded within an SVG!
           So this text will wrap nicely around multiple lines,
           but it will still be skewed from the SVG transform.
        </p>
     </body>
  </foreignObject>
  <text x="1em" y="25%" dy="1em">
    This SVG text won't wrap, so it will get cut off...
  </text>

</switch>
</g>
Inline SVG in XHTML or HTML5
The other way to mix SVG with XHTML is to include your SVG markup in an XHTML document; it also works with non-XML-compliant HTML documents using the HTML5 syntax.  This way of including SVG in a web page is called *Inline SVG* to distinguish it from SVG embedded as an image or object, although it really should be called In*file* SVG, because there’s no requirement that your SVG code has to all appear on a single line!

Inline SVG is supported in all major desktop web browsers for versions released in 2012 and later, and most of the latest mobile browsers. For XHTML, you indicate that you’re switching to SVG by defining all your SVG elements within the SVG namespace.  The easiest way to do this is to set xmlns="http://www.w3.org/2000/svg" on the top-level <svg> element, which changes the default namespace for that element and all its children.  For an HTML5 document (a file with <!DOCTYPE html>), you can skip the namespace declaration in your markup.  The HTML parser will automatically recognize that <svg> elements and all their children—except for children of <foreignObject> elements—are within the SVG namespace.

Inserting SVG markup into an (X)HTML document is easier than the reverse: you don’t need a separate <foreignObject>-like element to define where to render the SVG.  Instead, you apply positioning styles to the <svg> element itself, making it the frame for your graphic.

By default, the SVG will be positioned with the inline display mode (meaning that it is inserted within the same line as the text before and after it), and will be sized based on the height and width attributes of the <svg> element.  With CSS, you can change the size by setting the height and width CSS properties, and change the position with the display, margin, padding, and many other CSS positioning properties.2

Example 1-2 gives the code for a very simple SVG drawing in a very simple HTML5 document.  The result is Figure 1-3.  The xmlns attribute on the <svg> element is optional for HTML5.  For an XHTML document, you would change the DOCTYPE declaration at the top of the file, and you would wrap the CSS code in the <style> element with a <![CDATA[...]]> block.

If you do not set the height and width of the SVG with either CSS or attributes, web browsers should apply the default 150-pixel-by-300-pixel size, but be warned! Many versions of browsers apply different defaults.  Unfortunately, unlike when using an SVG file in an <img> element, you cannot just set one of the height or width and have the SVG scale based on the aspect ratio defined by its viewBox attribute.3
Example 1-2. Inline SVG within an HTML file
<!DOCTYPE html>
<html>
<head>
  <title>SVG in HTML</title>
  <style>

svg {
  display:block; 
  width:500px;
  height:500px;
  margin: auto;
  border: thick double navy; 
  background-color: lightblue;
}
body {
  font-family: cursive; 
}
circle {
  fill: lavender; 
  stroke: navy;
  stroke-width: 5;
}

  </style>
</head>
<body>
  <h1>Inline SVG in HTML Demo Page</h1>
    <svg viewBox="0 0 250 250"
         xmlns="http://www.w3.org/2000/svg">
      <title>An SVG circle</title>
      <circle cx="125" cy="125" r="100"/>
      <text x="125" y="125" dy="0.5em" text-anchor="middle">
         Look Ma, Same Font!</text>
    </svg>
  <p>And here is regular HTML again...</p>
</body>
</html>

The first style rules define how the SVG should be positioned and sized
within the HTML document.

You can also style the box in which the SVG will be drawn using other CSS properties.

Styles you define for the main document will be inherited by the SVG.

You can also define styles for your SVG elements within your main stylesheet.

1 In addition to fallback content, an <object> may also contain <param> elements defining parameters for the plug-in.  However, these aren’t used for rendering SVG data.

2 CSS positioning properties apply to top-level <svg> elements, ones which are direct children of HTML elements.  An <svg> that is a child of another SVG element will be positioned based on the rules for nested SVGs, as described in Chapter 5 in *SVG Essentials*.

3 As explained in Chapter 3 in *SVG Essentials*, the preserveAspectRatio attribute will scale an SVG while maintaining its aspect ratio. For inline SVG, this will scale the graphic to fit within the box (height and width) you define for it; it doesn’t change the size of the box within the web page.