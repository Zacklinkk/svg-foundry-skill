Chapter 2.
 SVG Text Basics
For the simplest use cases, SVG text is straightforward. A short label can be added to a diagram with a single markup element and a pair of attributes.

SVG text, like SVG shapes, are positioned within a two-dimensional coordinate system. The coordinate system can be controlled by viewBox
 attributes and transform
 properties, but by default it starts in the top-left corner of the graphic. All points in the image are defined by their position relative to that origin point, along the horizontal *x*
 -axis and vertical *y*
 -axis.

The rest of the book will assume you are familiar with the SVG coordinate system. If any of that sounded confusing, you might have some background reading to do.
Letters on a Page
SVG text is, conveniently enough, drawn using the <text>
 element. Attributes on the <text>
 element define the position at which to start writing. The child text content of the element provides the words and letters to be written. It looks something like this:

<text

 x=

"10px"

 y=

"80%"

>

SVG Text</text>


Put that code in a 400×80 SVG file, on top of a background rectangle, as in Example 2-1
 , and the result looks like Figure 2-1
 .

Example 2-1.
 Defining text within an SVG
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xml:lang=

"en"

     width=

"4in"

 height=

"0.8in"

 viewBox=

"0 0 400 80"

 >

    <title>

Basic SVG Text</title>

    <rect

 width=

"100%"

 height=

"100%"

 fill=

"lightYellow"

 />

    <text

 x=

"10px"

 y=

"80%"

>

SVG Text</text>

</svg>

Which…isn’t particularly exciting. But it confirms that yes, we can display text within an SVG file.

The text is positioned on the page using the x
 and y
 attributes on each <text>
 element. As with the positioning attributes used for SVG’s basic shapes (circles, ellipses, lines, and rectangles), there are various ways to specify the position:

- A number without units is interpreted as that number of user units in the current coordinate system.

- Lengths *with*
 units are scaled according to the viewBox
 or transformations in effect. 
 CSS px
 units are always interchangeable with SVG user units.

- Percentages are relative to the width or height specified in the viewBox
 of the nearest <svg>
 or <symbol>
 element (or the actual width and height, if no viewBox
 was given).

On a <text>
 element, both x
 and y
 default to 0 if not specified.
Tip
The default y="0"
 value can position text out of sight, above the top edge of the SVG, if the default top-left origin is used.

Any text content within an SVG that *isn’t*
 inside a <text>
 element will not be displayed. 
 This is in contrast to HTML, where text is printed to the screen by default.
Tip
This behavior difference can be used within inline SVG (in HTML pages) to print a warning to the page for users of older browsers that don’t support SVG. Text inside the <svg>
 but not inside a <text>
 will only be visible on these out-of-date browsers.

For short text labels, the x
 and y
 attributes may be the only positioning information you need. These values create an anchor point for the text. The browser aligns the first letter at this point and then types out the rest in a single row. Each letter is positioned next to the previous one, according to the normal spacing rules for the font and styles used.

Depending on how you are controlling the position of your graphic, x
 and y
 may not even be required. Coordinate system transformations also affect the position (and direction and scale) of text. The transformations can be applied to a parent <g>
 (grouping) element, or to the <text>
 element itself, as follows:

<text

 transform=

"translate(10,64)"

>

SVG Text</text>


The x
 and y
 attributes default to 0, but the (0,0) origin point of the coordinate system has now been shifted 10 units left and 64 units down. Because SVG user units are interchangeable with px
 units, and the original SVG was 80 units tall, and 80% of 80 is 64, this positions the text in the exact same place as Figure 2-1
 .

Of course, you can combine transform
 with x
 and y
 , too. Just as with other SVG shapes, the positioning attributes are calculated in the transformed coordinate system. So any of these snippets would also align the text in the same position:

<text

 x=

"10px"

 transform=

"translate(0,64)"

>

SVG Text</text>

<text

 y=

"80%"

 transform=

"translate(10,0)"

>

SVG Text</text>

<text

 x=

"50px"

 y=

"-20%"

      transform=

"translate(-40,80)"

>

SVG Text</text>



 The transform
 attribute used in SVG 1 and 1.1 only accepts user-unit values for translations, so it is not as flexible as x
 and y
 ; we needed
 to convert the units and percentages to user unit measurements. 
 
 In contrast, the extended CSS transform
 property introduced
 by the CSS Transforms Module accepts lengths or percentages, or even calc()
 functions combining the two, but it does *not*
 allow lengths or angles to be written as numbers without units.
Warning
For optimal browser support, the SVG 1.1 syntax is recommended; CSS transforms on SVG elements are not supported in Internet Explorer, although they are planned for Microsoft Edge. Inconsistencies in other browsers are still being ironed out. Under the new CSS Transforms specification, the SVG 1.1 syntax (without units) will continue to be supported when used as an attribute.

This side of SVG text — attributes that set x
 and y
 positions, and transformations — is very similar to SVG shapes. Other text options used for SVG text are very similar to CSS-formatted HTML content.
Big Words, Little Words
The text in Example 2-1
 used the browser’s default text styles. Nothing in the code specifies what font to use, how large, or in what colors. In effect, it just says “write this text here.”

The default font-family
 is entirely up to the browser or user defaults; it is usually a fairly innocuous, common font, but will differ from one program to another. The default font-size
 should be medium
 ; on most browsers, this is equivalent to 16px. Unfortunately, this cannot be relied upon for many uses of SVG on the Web.
Warning
In all WebKit and Blink browsers prior to mid-2015, if you do not set a font size, it will default to 1px high when the SVG is embedded as an image (<img>
 element or CSS background image) inside HTML.

There is another reason to always set a font size for text within an SVG image. The text is being drawn in a custom coordinate system and will scale with the image. The ratio of the font size to the image size is kept constant, but the actual final font size on screen can vary.
Note
In contrast, with HTML, you should always use the default root font size provided by the browser for body text. Headings and fine print can be made proportionally larger or smaller rather than given explicit values. This allows your design to adapt to user’s settings, to make it accessible to those who cannot read small text.

The size of text in SVG is controlled by the familiar CSS font-size
 property. However, given SVG’s flexible coordinate systems, the results of setting font-size
 may not always be what you expect.

There are two ways to set the font-size
 property — and most other style properties — in SVG. You can either use CSS rules with the same syntax as for HTML, or you can define the font-size
 attribute. 
 The attribute is known as a *presentation attribute*
 and it participates in the CSS cascade as a rule with zero specifity. Except that it’s actually less than zero: it can be overridden by the zero-specifity wildcard (*
 ) selector!

Values set as a presentation attribute still inherit as normal, and still replace values inherited from parent elements. So in Example 2-2
 , the em-based width
 and height
 attributes on the <svg>
 will be calculated relative to the style rule font-size
 , but the actual text will be drawn at the size specified in the presentation attribute on the <text>
 element. The net result is shown in Figure 2-2
 .

Example 2-2.
 Defining font-size with style rules and presentation attributes
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xml:lang=

"en"

     width=

"24em"

 height=

"4.8em"

 viewBox=

"0 0 400 80"

 >

    <title>

Font-size and SVG Text</title>

    <style 

type=

"text/css"

>

        svg

 {

 font-size

:

 12pt

;

 }

    </style>

    <rect

 width=

"100%"

 height=

"100%"

 fill=

"lightYellow"

 />

    <text

 x=

"10px"

 y=

"80%"

 font-size=

"64px"

>

SVG Text</text>

</svg>

There are a number of ways you can specify font-size
 within CSS, all of which can also be used in SVG:

- As an absolute keyword, such as small
 , medium
 , or x-large
 . Much like t-shirts, your mileage may vary as to the precise values of these: browsers are allowed to decide exactly what these words mean. They should be the same (relative to the absolute measurements) in SVG as HTML.

- As a relative keyword, larger
 or smaller
 . Again, the amount of change is browser-dependent.

- As an absolute length with units (e.g., pt
 , mm
 , in
 ).

- As a length in font-relative units (e.g., em
 , ex
 ).

- As a percentage.

Length units introduced by the Values and Units Module Level 3 (ch
 , rem
 , and viewport units vw
 , vh
 , vmin
 , and vmax
 ) can be used for font-size
 in SVG in all browsers that support them in general. 
 
 The relative keywords, percentages, and all font-relative units are relative to the *inherited*
 font size.
Warning
If you use the rem
 (root-em) unit in SVG graphics that will be used in HTML as images, be sure to explicitly set font-size
 on the root <svg>
 element to avoid the bug with root font-size
 in WebKit and Blink browsers.

For the font-size
 presentation attribute *only*
 , you may specify the font-size
 as a length in user units (e.g., as a number without units). For setting font size in a CSS rule, this will fail because the syntax is not valid for non-SVG content.
Tip
In CSS rules *or*
 presentation attributes, you can always use px
 as a synonym for user units.

As with all other lengths in SVG, the font-size
 you specify will be scaled according to the current coordinate system. An in
 will always be 96px and therefore 96 user units. A pt
 (1
 /72
 of an inch) will always be 11
 /3
 px, and therefore 11
 /3
 user units. Nontheless, these may be quite different from the print typographer’s pt
 (point) unit or inches on a ruler.
Warning
Although all modern web browsers treat px
 as an absolute measurement, consistent with CSS 2.1, other SVG tools may scale px
 to the screen resolution or apply their own default resolution. For example, Inkscape used 90px per inch for many years, while Adobe Illustrator used 72px per inch.

For consistent results, set font-size
 in px
 if your SVG uses a viewBox
 to create a scaled user coordinate system — or if the SVG is sized in px
 directly. If your SVG is sized in absolute units (e.g., in
 or cm
 ) and does not have a viewBox
 , use absolute units for the font size (e.g., pt
 or mm
 ).

The coordinate system for a child element may be different from the coordinate system used by the parent — because of transformations or a new viewBox
 scale — so the size of letters on the screen may change even if the official font-size
 does not. Example 2-3
 demonstrates this by drawing 6pt text in various coordinate systems, as shown in Figure 2-3
 .

Example 2-3.
 Using font-size in HTML and SVG
<!DOCTYPE html>



<html

 

lang=

"en"

>



<head

>

    

<meta

 

charset=

"utf-8"

 

/>

    

<title

>

font-size and SVG Coordinate Systems

</title>

    

<style

>

        

body

 

{

            

background-color

:

 

lightYellow

;

            

font

:

 

6pt

 

sans-serif

;

                     





        

}

        

svg

 

{

            

display

:

 

block

;

            

border

:

 

solid

 

royalBlue

;

            

background-color

:

 

lightCyan

;

            

margin

:

 

2em

 

auto

;

            

width

:

 

95%

;

            

max-width

:

 

90vh

;

            

height

:

 

auto

;

            

min-height

:

 

10em

;

            

max-height

:

 

90vh

;

                         





        

}

    

</style>



</head>



<body

>

    

<p

>

This is 6pt HTML text.
       It is very tiny and difficult to read.

</p>

    

<svg

 

viewBox=

"0 0 100 100"

>

                       





        

<text

 

x=

"5"

 

y=

"1em"

>

This is 6pt SVG text.

</text>

        

<text

 

transform=

"translate(10,90) rotate(-30) scale(2,3)"

              

>

This is also 6pt text.

</text>

          





    

</svg>



</body>



</html>

The font
 style rule on the HTML <body>
 element is the only point in the document that the font size is set.

The remaining style rules ensure that the inline <svg>
 scales nicely to fit within the browser window — even if the browser does not support automatically scaling the SVG height to match the width and viewBox
 aspect ratio.

The viewBox
 creates a square aspect ratio with 100 units in both the horizontal and vertical directions. This defines the basic scale used for the first <text>
 element.

The second <text>
 element has a transform
 attribute that positions the text, rotates it, and then applies a non-uniform scale.

The font size in Figure 2-3
 is always 8 units high in the current SVG coordinate system (8=6×11
 /3
 ). When the coordinate system is unevenly scaled, the letters are stretched to match.
Styling Text
There are many more styling options for text beyond setting its size. 
 The following CSS 2 properties can be used to style SVG text:

- 
font-family
 , font-size
 , font-size-adjust
 , font-stretch
 , font-style
 , font-variant
 , and font-weight
 for selecting and scaling the font data

- 
text-decoration
 for adding underlines, overlines, and strike-throughs

- 
text-transform
 for converting to uppercase, lowercase, or capitalized words

- 
direction
 and unicode-bidi
 to control multidirectional language

- 
letter-spacing
 and word-spacing
 to adjust text spacing
Warning
At the time of writing, Firefox does not support letter-spacing
 and word-spacing
 for SVG text.

The font-size-adjust
 property is currently *only*
 supported in Firefox among major web browsers.

All of the properties just listed can be specified using either presentation attributes or style rules. 
 Newer CSS 3 properties (introduced after the SVG 1.1 standard) may affect SVG text, but are less likely to be supported as attributes.

SVG also introduced a number of advanced text layout properties, which have since been adopted into or adapted by CSS 3 modules; these include the following: alignment-baseline
 , baseline-shift
 , dominant-baseline
 , glyph-orientation-horizontal
 , glyph-orientation-vertical
 , kerning
 , and writing-mode
 . Most of these will be discussed in Chapters 7
 and 8
 . As detailed in those chapters, these properties are not well implemented in browsers; some will be deprecated in favor of CSS 3 alternatives.

One standard CSS text styling property that does *not*
 affect SVG text is color
 . In CSS-styled HTML, color
 sets the color of the text letters. However, in SVG text, the color is controlled by the same fill
 property you use to set the color of SVG shapes.
Tip
If this seems like an unnecessary complication, be assured that it is because you have much more flexible options for coloring SVG text, as we’ll discuss in Chapter 3
 .

Combining multiple effects, you could replace the basic text example from Example 2-1
 with the following element and presentation attributes:

<text

 x=

"10"

 y=

"80%"

      font-family=

"Verdana, Geneva, sans-serif"

      font-size=

"64px"

      font-weight=

"bold"

      text-decoration=

"underline"

      fill=

"darkBlue"

      >

SVG Text</text>



 Alternatively, you could use the original <text>
 element and add a <style>
 element with the following rules:

text

 {

    font-family

:

 Verdana

,

 Geneva

,

 sans-serif

;

    font-size

:

 64px

;

    font-weight

:

 bold

;

    text-decoration

:

 underline

;

    fill

:

 darkBlue

;

}
Warning
For optimal support in SVG tools other than web browsers, specify the stylesheet language by including a type="text/css"
 attribute on the <style>
 element.

Either version, presentation attributes or style rules, would create the text in Figure 2-4
 .

When using CSS (but not presentation attributes), you can also use the font
 property as a shorthand to set size and family together, along with other common options such as italic or bold. The shorthand for the same styles would look as follows:

text

 {

    font

:

 bold

 64px

 Verdana

,

 Geneva

,

 sans-serif

;

    text-decoration

:

 underline

;

    fill

:

 darkBlue

;

}


The optional bold/italic values are specified first; both revert to normal
 if left out. The font size and at least one font-family value are required, and must be specified in that order.
Tip
Chapter 10
 will discuss fonts and the font-family
 property in more detail, including the generic family keywords (such as sans-serif
 ), system fonts (such as Verdana and Geneva), and web fonts.

So far, the text in the SVG examples has not looked that much different from text in HTML. However, the SVG text is also an SVG graphic, and can have graphical effects applied to it, such as transformations, filters, masks, and clipping paths. For example, many of the figures in Chapter 1
 used a filter to create a puffed-up three-dimensional effect.

Example 2-4
 provides the code for that filter and uses the filter
 property to apply it to our basic text example. Figure 2-5
 shows the effect in action.

Example 2-4.
 Applying graphical effects to SVG text
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

xml:lang=

"en"

     

width=

"4in"

 

height=

"0.8in"

 

viewBox=

"0 0 400 80"

 

>

    

<title

>

Filter Effects on SVG Text

</title>

    

<style 

type=

"text/css"

>

        

text

 

{

            

font

:

 

bold

 

64px

 

Verdana

,

 

Geneva

,

 

sans-serif

;

            

text-decoration

:

 

underline

;

            

fill

:

 

darkBlue

;

            

stroke

:

 

indigo

;

            

filter

:

 

url(#shine)

;

                                





        

}

    

</style>

    

<defs

>

        

<filter

 

id=

"shine"

>

            

<feGaussianBlur

 

in=

"SourceGraphic"

 

stdDeviation=

"2"

                            

result=

"blur"

/>

                     





            

<feColorMatrix

 

values=

"1.5  0   0  0.5 0
                                    0  1.5  0  0.5 0
                                    0   0  1.5 0.5 0
                                    0   0   0   1  -0.5"

 

/>

     





            

<feOffset

 

dx=

"-2.5"

 

dy=

"-1.5"

 

/>

                    





            

<feComponentTransfer

 

result=

"highlight"

>

                

<feFuncA

 

type=

"linear"

 

amplitude=

"2"

 

/>

         





            

</feComponentTransfer>

            

<feComposite

 

in=

"blur"

 

in2=

"highlight"

                         

operator=

"arithmetic"

                         

k1=

"0"

 

k2=

"1"

 

k3=

"1"

 

k4=

"0"

 

/>

            

<feComposite

 

in2=

"SourceGraphic"

 

operator=

"atop"

 

/>

 





        

</filter>

    

</defs>

    

<rect

 

width=

"100%"

 

height=

"100%"

 

fill=

"lightYellow"

 

/>

    

<text

 

x=

"10"

 

y=

"80%"

>

SVG Text

</text>



</svg>

The text styles, including fill
 , stroke
 , and filter
 , are applied using a CSS rule.

The <filter>
 element describes the sequence of processing steps that will convert the basic vector shape or text into its final appearance. Each step is a filter effect (fe
 ), starting with feGaussianBlur
 to create a blurred copy of the source graphic.

An feColorMatrix
 modifies the image created by the previous step; the matrix values lighten all the colors but decrease the alpha channel so that partially transparent regions will disappear completely.

An feOffset
 element shifts the lightened, blurred layer up and to the left.

An feComponentTransfer
 element with feFuncA
 modifies the alpha channel again, doubling the opacity. This resets some of the changes made previously, so that some areas return to full opacity, but does not affect areas that had their alpha drop to zero. The result of this step is given the name highlight
 so that it can be referenced in the next step.

The feComposite
 elements each take two input layers and combine them with the specified operator. First, the blurred and highlight layers are added together. Then the combined effect is layered over top the source graphic and clipped to only include the parts that overlap opaque areas of the original.

The <text>
 element itself in Example 2-4
 uses the exact same markup as in the basic code in Example 2-1
 ; all the effects are applied using style properties. Importantly, the text remains accessible as machine-readable character data to search engines, screen readers, and your browser’s own text search (“Find”) or copy-and-paste functions.
Labeling a Graphic
Because this is a book specifically about SVG text, most of the examples will only contain text, and not graphics. There are practical applications for text-only SVG, as we will see. Some graphical effects cannot be created with CSS-styled HTML text. In some cases, new CSS specifications have extended the effect to HTML, but browser support is much better with SVG, as with filtered text like that in Figure 2-5
 .

Nonetheless, the most common use of text in SVG is to annotate an SVG graphic with machine-readable, easily editable text. This section works through the process of adding text to an image. We’ll start with the graphic from Figure 2-6
 , a minimalist drawing of a lily native to the Rocky Mountain regions of North America.

The complete code for the drawing is 150 lines when neatly formatted, so we’re not going to print it out here. 
 For the purpose of adding text to the graphic, the most important aspect of the SVG is the coordinate system it uses. The viewBox
 is "0 0 400 500"
 , so the origin is top-left and the entire graphic is 400 units wide and 500 units tall. The base of the stem is positioned at (100,450).

We start by adding a title and subtitle to the image, aligned with the base of the drawing:

<text

 role=

"heading"

 aria-level=

"1"

      x=

"170"

 y=

"450"

>

Wood Lily</text>

<text

 xml:lang=

"la"

 role=

"heading"

 aria-level=

"2"

      x=

"190"

 y=

"480"

>

Lilium montanum</text>


The text is then styled by a series of CSS rules:

text

 {

    font-family

:

 Georgia

,

 serif

;

    font-size

:

 24px

;

}

[

role

=

"heading"

]

 {

    fill

:

 darkSlateGray

;

}

[

role

=

"heading"

][

aria-level

=

"1"

]

 {

    font-size

:

 48px

;

}

:lang

(

la

)

{

    font-style

:

 italic

;

}


The drawing with styled titles is shown in Figure 2-7
 .

The heading text is selected using an attribute selector, based on the value of the role
 attribute. The top-level and secondary headings are distinguished by the aria-level
 attribute. Although you could have used classes to apply the styles, the ARIA attribute adds meaning for accessible technologies that interpret your SVG content, such as screen readers.

Unlike HTML, SVG does not have any *semantic*
 (meaningful) elements that define the purpose of different text sections. There is no <h1>

 or <figcaption>
 that could convey the purpose of the text or give it a hierarchy. The role
 and aria-level
 attributes provide this structure.
Note
The Accessible Rich Internet Applications (ARIA) attributes were developed by the W3C’s Web Accessibility Initiative (WAI) to make it easier for accessible technologies to navigate multipart web pages and interact with JavaScript-controlled websites. SVG 2 explicitly adopts them for use in SVG, but most web browsers already support them.

Unfortunately, there are only a limited number of WAI-ARIA roles available to describe basic text features, such as paragraphs and emphasized words, that were already well defined in HTML. More nuanced roles may be available in future versions of ARIA.

Similarly, a semantic selector is used to apply the italic font style to the botanical name. The markup uses the xml:lang="la"
 attribute to identify the text as Latin, in contrast to the main language of the document, which is set to English using an xml:lang="en"
 attribute on the root <svg>
 element. The :lang(la)
 CSS pseudoclass selector then applies the style rules. Unlike an attribute selector, the :lang()
 selector not only applies to the element that has the language defined on it, but also to any child elements that inherit that language setting. Plus, it avoids the hassle of dealing with XML namespaces in CSS!

Text in an SVG need not be limited to a simple title. In data visualizations and diagrams, you may need multiple labels integrated carefully with the graphical content. Figure 2-8
 shows one possible set of labels for the lily drawing, outlining the parts of the plant and flower structure. The labels have been made interactive, so the user can hover/focus each one and the label and connector line will both be highlighted.

The labels are structured using indentation to represent the structure of the parts being described. One way to represent this layout in SVG markup would be to re-create the nesting structure with group (<g>
 ) elements and transformations:

<g

 class=

"labels"

 transform=

"translate(225,0)"

>

    <text

 y=

"1em"

>

flower</text>

    <g

 transform=

"translate(25,0)"

>

        <text

 y=

"2em"

>

pistil</text>

        <g

 transform=

"translate(25,0)"

>

            <text

 y=

"3em"

>

stigma</text>

            <text

 y=

"4em"

>

style</text>

            <text

 y=

"5em"

>

ovary</text>

        </g>

        <text

 y=

"6.5em"

>

stamen</text>

        <g

 transform=

"translate(25,0)"

>

            <text

 y=

"7.5em"

>

filament</text>

            <text

 y=

"8.5em"

>

anther</text>

        </g>

        <text

 y=

"10em"

>

sepal/petal</text>

    </g>

    <text

 y=

"11.5em"

>

leaf</text>

    <text

 y=

"13em"

>

stem</text>

</g>


The code is not ideal. The position of each label is hard coded into the attribute; if you wanted to add or remove a label, or change the spacing in between them, you would need to edit the file in multiple locations. In Chapter 4
 , we’ll show how you can position multiple parts of a text block relative to each other; however, it will never be as flexible as HTML text layout.

So why use SVG for structured text content? Primarily because it can be positioned precisely relative to SVG *graphics*
 . The guidelines connecting the labels to the drawing are also positioned using *x-*
 and *y-*
 coordinates. For the entire graphic to make sense, these positions need to remain coordinated with the text positions, at any scale.
Tip
In order to use em
 units to position the ends of the lines, the <line>
 elements must have the same font-size
 property as the <text>
 elements. The easiest way to do this is to set the font size on the parent <svg>
 .

A number of additional features are then integrated in the code to improve the accessibility of the text:

- Each text label is given an id
 value, so that it can be referenced in the aria-labelledby
 attribute of the corresponding graphic object.

- The text elements and groups have been given role
 attributes of list
 and list-item
 so that the nested structure is clearly communicated to screen readers; this requires extra <g>
 elements to combine
 the text label and its nested list in a single list-item
 element.

- The nested lists also have aria-labelledby
 attributes linking back to their headings, so they are clearly identified if the user jumps between content.

- In most browsers, the list items are made keyboard accessible by adding
 focusable="true"
 and tabindex="0"
 attributes using a script.

Example 2-5
 provides the complete code for the text and guidelines.

Example 2-5.
 Positioning structured, interactive labels on a diagram
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xmlns:xlink=

"http://www.w3.org/1999/xlink"

     width=

"400"

 height=

"500"

 viewBox=

"0 0 400 500"

     xml:lang=

"en"

 aria-labelledby=

"title"

>

  <title>

Wood Lily</title>

  <style 

type=

"text/css"

>

      /* Styles for main graphic omitted */

      svg

 {

          font-size

:

 24px

;

      }

      text

 {

          font-family

:

 Georgia

,

 serif

;

          fill

:

 #123

;

      }

      [

role

=

"heading"

]

 {

          fill

:

 darkSlateGray

;

      }

      [

role

=

"heading"

][

aria-level

=

"1"

]

 {

          font-size

:

 48px

;

      }

      :lang

(

la

)

{

          font-style

:

 italic

;

      }

      line

 {

          stroke

:

 #123

;

          stroke-width

:

 2px

;

          stroke-linecap

:

 round

;

          stroke

-

opacity

:

 0

.

3

;

          stroke-dasharray

:

 1

 3

;

          transition

:

 stroke

-

opacity

 0.5s

 0.1s

,

                      stroke-dasharray

 0.1s

;

      }

      text

:hover

 +

 line

,

 text

:focus

 +

 line

 {

          stroke

-

opacity

:

 0

.

8

;

          stroke-dasharray

:

 7

 0

;

          transition

:

 stroke

-

opacity

 0.5s

,

                      stroke-dasharray

 0.4s

 0.2s

;

      }

      :focus

 {

          outline

:

 none

;

      }

      .labels

 text

:hover

,

      text

:focus

,

      g

:focus

 >

 text

:first-of-type

 {

          text-decoration

:

 underline

;

      }

  </style>

  <!-- Main graphics markup omitted -->

  <g

 id=

"title"

>

      <text

 role=

"heading"

 aria-level=

"1"

            x=

"170"

 y=

"450"

>

Wood Lily</text>

      <text

 xml:lang=

"la"

 role=

"heading"

 aria-level=

"2"

            x=

"190"

 y=

"480"

>

Lilium montanum</text>

  </g>

  <g

 class=

"labels"

 transform=

"translate(225,0)"

 role=

"list"

     aria-label=

"parts of the plant"

>

      <g

 role=

"listitem"

>

          <text

 id=

"flower"

 y=

"1em"

>

flower</text>

          <g

 transform=

"translate(25,0)"

             role=

"list"

 aria-labelledby=

"flower"

>

              <g

 role=

"listitem"

>

                  <text

 id=

"pistil"

 y=

"2em"

>

pistil</text>

                  <g

 transform=

"translate(25,0)"

                     role=

"list"

 aria-labelledby=

"pistil"

>

                      <text

 id=

"stigma"

 y=

"3em"

                            role=

"listitem"

>

stigma</text>

                      <line

 x1=

"-5"

 y1=

"2.75em"

 x2=

"-130"

 y2=

"75"

/>

                      <text

 id=

"style"

 y=

"4em"

                            role=

"listitem"

>

style</text>

                      <line

 x1=

"-5"

 y1=

"3.75em"

 x2=

"-150"

 y2=

"100"

/>

                      <text

 id=

"ovary"

 y=

"5em"

                            role=

"listitem"

>

ovary</text>

                      <line

 x1=

"-5"

 y1=

"4.75em"

 x2=

"-155"

 y2=

"115"

/>

                  </g>

              </g>

              <g

 role=

"listitem"

>

                  <text

 id=

"stamen-label"

 y=

"6.5em"

>

stamen</text>

                  <g

 transform=

"translate(25,0)"

                     role=

"list"

 aria-labelledby=

"stamen"

>

                      <text

 id=

"filament"

 y=

"7.5em"

                            role=

"listitem"

>

filament</text>

                      <line

 x1=

"-5"

 y1=

"7.25em"

 x2=

"-120"

 y2=

"145"

/>

                      <text

 id=

"anther"

 y=

"8.5em"

                            role=

"listitem"

>

anther</text>

                      <line

 x1=

"-5"

 y1=

"8.25em"

 x2=

"-112"

 y2=

"161"

/>

                  </g>

              </g>

              <text

 id=

"sepal"

 y=

"10em"

                    role=

"listitem"

>

sepal/petal</text>

              <line

 x1=

"-5"

 y1=

"9.75em"

 x2=

"-115"

 y2=

"175"

/>

          </g>

      </g>

      <text

 id=

"leaf"

 y=

"11.5em"

 role=

"listitem"

>

leaf</text>

      <line

 x1=

"-5"

 y1=

"11.25em"

 x2=

"-110"

 y2=

"270"

/>

      <text

 id=

"stem"

 y=

"13em"

 role=

"listitem"

>

stem</text>

      <line

 x1=

"-5"

 y1=

"12.75em"

 x2=

"-135"

 y2=

"305"

/>

  </g>

  <script>

<!

[

CDATA

[

(

function

(){

    var

 items

 =

 document

.

querySelectorAll

(

"[role='listitem']"

);

    for

 (

var

 i

=

0

,

 n

=

items

.

length

;

 i

<

n

;

 i

++

)

 {

        items

[

i

].

setAttribute

(

"tabindex"

,

 0

);

        items

[

i

].

setAttribute

(

"focusable"

,

 true

);

    }

})();

]]

>

</script>

</svg>
Warning
The focusable
 attribute, introduced in SVG Tiny 1.2, is only supported in Internet Explorer. The tabindex
 attribute, adopted for SVG 2 from HTML, is currently only supported for SVG in Blink/WebKit browsers.

To ensure broader keyboard accessibility in SVG, you could wrap each <text>
 element in an <a>
 link; these are keyboard focusable in most browsers by default.

CSS pseudoclass selectors trigger the changes in styles when text are moused-over or focused; CSS transitions animate the effect in browsers that support them for SVG content (i.e., except in IE). However, these interactive features will only apply if the SVG file is viewed directly or if it is embedded in a web page using an <object>
 element. If the SVG is embedded as an image, it will not receive any user events (also, the script will not execute). Alternatively, the SVG code could be copied into an HTML file directly as inline markup.