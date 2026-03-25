Chapter 3.
 Colorful Language
The shape and size of SVG letters may be controlled by the standard CSS font selection properties, but the final appearance is uniquely SVG. As far as painting the text, letters are just another vector shape in SVG, defined by the Bézier curves within the vector font files. Results may be strange if you use a bitmap font, but those are few and far between on modern computers.

Like all SVG shapes, the paint of text is affected by two properties: fill
 for the interior, and stroke
 for the outline.
Fill and Stroke
The normal shape of a letter in an OpenType-compatible vector font is created by filling in the outlines from the font file. Therefore, for “normal” looking text in SVG, you set the fill
 property to a solid color. For more adventurous text, you can use gradient or patterned fills, or no fill at all, just a stroked outline.

Each glyph is painted individually, in logical reading order. If the text has a thick stroke, the overlap of each letter on previous ones may be visible. 
 For cursive scripts, strokes are often unattractive because they highlight the edges of each glyph, counteracting the impression of a continuous stroke from one letter to the next.
Tip
As with every other shape in SVG, the default appearance for text is solid black fill and no stroke.

Both fill
 and stroke
 have the same allowed values:

- The keyword none

- 
 A color value, using any CSS color format supported by the software — keywords, hex colors like #080844
 or #fab
 , or functions like rgb(20, 100, 128)
 or hsla(0,80%,75%,0.7)

- A url()
 reference to the id
 of an SVG *paint server*
 (gradient or pattern element).

You can include a fallback color in addition to a paint server reference. It will be used if there is an error generating the gradient or pattern.
Note
The SVG specifications also allow you to define colors based on the International Color Commission’s ICC color profiles. For example, you could use Pantone named colors. These are mostly of use in print graphics, and are not supported in web browsers.

SVG’s painting options can be used to dress up text within an HTML document. A short heading or title on a web page can be replaced by an inline SVG that draws the text. Example 3-1
 uses this approach to create headings with the most basic of SVG-only effects: outlined letters. The final web page is displayed in Figure 3-1
 .

Example 3-1.
 Using SVG text to add graphical effects to HTML headings
<!DOCTYPE html>



<html

>



<head

>

    

<meta

 

charset=

"utf-8"

 

/>

    

<title

>

SVG Text Headings

</title>

    

<style

>

        

body

 

{

            

background-color

:

 

#DEF

;

            

font-family

:

 

serif

;

        

}

        

h1

 

svg

,

 

h2

 

svg

 

{

            

width

:

 

80%

;

            

height

:

 

1em

;

                   





            

display

:

 

inline

-

block

;

            

vertical-align

:

 

baseline

;

      





            

overflow

:

 

visible

;

            

padding-bottom

:

 

0.4em

;

            

margin-bottom

:

 

-0.4em

;

         





        

}

        

h1

,

 

h2

 

{

            

font-family

:

 

sans-serif

;

            

stroke

:

 

darkRed

;

               





        

}

        

h1

 

{

            

fill

:

 

coral

;

        

}

        

h2

 

{

            

fill

:

 

lightCoral

;

        

}

    

</style>



</head>



<body

>

    

<h1

>

<svg

 

role=

"presentation"

>

                          





        

<text

 

y=

"1em"

>

Level 1 Heading

</text>

               





    

</svg>

</h1>

    

<p

>

An introductory paragraph full of lots of interesting text,
    which goes on for a few lines to take up space.  Plenty of
    space.  Very interesting text.  So interesting, you can't wait
    to see:
    

</p>

    

<h2

>

<svg

 

role=

"presentation"

>

        

<text

 

y=

"1em"

>

A subheading

</text>

</svg>

</h2>

    

<p

>

Such excitement!  Subheadings after headings.  This must be
    a really interesting web page to require such exciting headings.
    We really ought to have another one, to finish things off.
    

</p>

    

<h2

>

<svg

 

role=

"presentation"

>

        

<text

 

y=

"1em"

>

Another sub

</text>

</svg>

</h2>

    

<p

>

There.  Now this feels like a proper mock-up.  You can really
    get the feeling of a full page outline, can't you?
    

</p>



</body>



</html>

The <svg>
 elements within each heading are set to take up the full available width and one line of text’s height (1em). Exactly how high that is will depend on the inherited font-size
 values. In this case, those will be the browser default font sizes for <h1>
 and <h2>
 .

The SVG will create an inline block that sits on the normal text baseline for the heading text. The text will then be adjusted to sit along the bottom of the SVG.

Because this means that the descenders of letters will extend outside the <svg>
 , it has overflow
 set to visible
 . 
 
 Because WebKit and older Blink browsers won’t draw inline SVG content outside of the padding region, some extra padding is added and then canceled out with a negative margin.

The styles for the headings are set directly on the <h1>
 and <h2>
 elements; they will inherit into the SVG.

The markup for the headings is very simple: the heading text is inside a <text>
 element, inside an <svg>
 , within the HTML heading markup. 
 
 The ARIA presentation
 role tells assistive technologies to ignore the SVG markup, because it is only there for stylistic effect. The text content of the SVG will still be recognized, with its role defined by the HTML heading elements.

One positioning attribute is required: the y
 value that shifts the text from sitting on the top edge of the SVG to sitting on the bottom edge.

One important limitation of using SVG for HTML headings is that the layout will not automatically adjust for narrow screens. HTML text in a CSS block element would wrap to a new line if it reaches the edge of the content area. SVG text, in contrast, will keep typing in a straight line off the edge of the screen.

As we’ll see in future chapters, multiline SVG text is possible, but browsers do not yet support automatically wrapping text. Even if they did, that would not automatically cause the parent <svg>
 element to expand to make room.

Because the text content of the inline SVGs is contained *inside*
 the HTML heading markup, there is no need for extra ARIA attributes to explain how the SVG text formatting relates to the document outline. In fact, in a browser that does not support inline SVG, the SVG elements would be ignored and the text would be treated as normal HTML heading text.

However, those HTML headings would not be colored, because the color was set using the SVG-specific properties fill
 and stroke
 . 
 
 If you wanted the heading text to show up in red or orange on Internet Explorer 8, you would need to also set the color
 property.

Manually coordinating two different properties like this leaves room for error. If you want to match a color in the SVG with the color of the HTML text that it is embedded in, you can instead use the currentColor
 keyword.
Coordinating Colors
The currentColor
 keyword always evaluates to the value of the color
 property in effect for the element. It was originally introduced in CSS to represent the default behavior of borders, which match text color if not otherwise set.

For SVG, the most important use of currentColor
 is to coordinate inline icons with surrounding text. Example 3-2
 uses currentColor
 to create icons that provide extra information about the destination of a hyperlink. The icons change color with the rest of the hyperlink. Figure 3-2
 shows the resulting HTML content as it appears with default browser link colors in Firefox.

Example 3-2.
 Using currentColor to create SVG content that matches HTML
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

Color-Matching Icons

</title>

    

<style

>

        

body

 

{

            

background-color

:

 

#DEF

;

            

font-family

:

 

sans-serif

;

            

font-size

:

 

x-large

;

        

}

        

svg

.icon

 

{

            

display

:

 

inline

-

block

;

            

height

:

 

1em

;

            

width

:

 

1em

;

            

vertical-align

:

 

text-bottom

;

         





        

}

        

svg

.defs

 

{

            

display

:

 

block

;

            

position

:

 

absolute

;

            

left

:

 

-10

;

            

height

:

 

0

;

 

width

:

 

0

;

                 





        

}

    

</style>



</head>



<body

>

    

<svg

 

class=

"defs"

 

aria-hidden=

"true"

         

focusable=

"false"

 

width=

"0"

 

height=

"0"

>

        

<symbol

 

id=

"external-link"

 

viewBox=

"-10 -10 20 20"

>

    





            

<circle

 

r=

"9.5"

                    

fill=

"currentColor"

 

fill-opacity=

"0.3"

 

/>

            

<g

 

fill=

"currentColor"

 

font-size=

"6"

>

                

<text

 

x=

"-8"

 

y=

"-2"

 

>

W

</text>

                

<text

 

x=

"-3"

 

y=

"3"

 

>

W

</text>

                

<text

 

x=

"2"

 

y=

"8"

 

>

W

</text>

            

</g>

        

</symbol>

        

<symbol

 

id=

"pdf"

 

viewBox=

"0 0 20 20"

>

                   





            

<rect

 

id=

"r"

 

x=

"0.5"

 

y=

"0.5"

 

width=

"14"

 

height=

"15"

                  

fill=

"white"

 

stroke=

"currentColor"

 

/>

            

<use

 

xlink:href=

"#r"

 

x=

"1.5"

 

y=

"2"

 

/>

            

<use

 

xlink:href=

"#r"

 

x=

"3"

 

y=

"4"

 

/>

            

<text

 

x=

"5"

 

y=

"13"

 

fill=

"currentColor"

                  

font-size=

"6"

>

PDF

</text>

        

</symbol>

    

</svg>

    

<p

>

An external link with an informative icon, to the
    

<a

 

href=

"http://www.w3.org/TR/SVG11/Overview.html"

>

SVG 1.1
        

<svg

 

class=

"icon"

>

                                      





            

<use

 

xlink:href=

"#external-link"

>

                

<title

>

External link

</title>

            

</use>

        

</svg>

</a>

        specifications on the Web.

</p>

    

<p

>

Or you can download the
        

<a

 

href=

"http://www.w3.org/TR/SVG11/REC-SVG11-20110816.pdf"

           

>

complete specifications as a single file
        

<svg

 

class=

"icon"

>

            

<use

 

xlink:href=

"#pdf"

>

                

<title

>

PDF file

</title>

            

</use>

        

</svg>

</a>

.

</p>



</body>



</html>

The icon
 class will be applied to individual inline <svg>
 elements, and ensures they will be 1em-square elements that align nicely with the surrounding text.

The defs
 class applies to a single inline SVG that holds all the definitions for the icon symbols. The CSS styles ensure that this element does not affect the web page layout or appearance.

The first icon will be used to mark links to external websites. It consists of the letters “WWW” overlaid on a semitransparent circle, both filled with the current text color.

The second icon will be used to mark links to PDF files. It consists of the letters “PDF” written on a set of stacked rectangles to represent a multipage document. The letters are filled with the current text color; the pages are white but have a matching colored outline.

The icons are used by inserting a small block of inline SVG code directly, contained within each <a>
 link in the HTML.

The currentColor
 keyword can be used anywhere in SVG that a color value is needed. In Example 3-2
 , it is used as both fill
 and stroke
 color within the icon symbols. The symbols inherit styles — including the color
 setting — from the <use>
 elements, which inherit them from the links. 
 Links are by default colored in web browsers, with different colors depending on whether that link has previously been visited or is actively being selected.
Warning
Because the visited status of a link can reveal personal information about a user, browsers may hide the information about a link color from any place it could be accessed by web page scripts.

In Blink browsers, this is implemented in a way that prevents the :visited
 link color from being inherited using the currentColor
 keyword: the icon is always blue, not purple. Other color changes (such as the red :active
 link state when the link is clicked) *are*
 propagated to the icon.

Because the icons express additional information, that information is also made available as accessible text. The <title>
 element is used so that the accessible text is also available to other uses as a tooltip — in case the meaning of our super-simple icons is unclear!

The <title>
 element is a child of the <use>
 element, not of the <svg>
 . Many browsers do not create tooltips for a <title>
 that is the direct child of an <svg>
 , even when that SVG is inline in HTML.
Tip
When an SVG is viewed as its own file, its title appears in the browser’s tab or title bar, so a tooltip is not required.

By grouping symbol definitions in a single <svg>
 , they can be re-used as often as necessary, without cluttering the main document markup with SVG tags. The nested <svg>
 , <use>
 , and <title>
 elements are only slightly more complicated than an <img>
 element — but the end result is considerably more flexible, a stylable graphic that is part of the main document.
Warning
Avoid using display: none
 to hide the definitions <svg>
 . Although this *should*
 work according to the specifications, in practice, browsers will not properly render reused content that has a parent with display: none
 . 
 In particular, gradients and patterns will not work in any current browser, and some browsers may not draw re-used graphics, particularly if those graphics are not part of a <symbol>
 .

The workaround is to use CSS absolute positioning properties to visually hide the content while still having it set to be “displayed.” An aria-hidden="true"
 attribute ensures that it is also hidden from screen readers. 
 A focusable="false"
 attribute ensures that it does not recieve keyboard focus in Internet Explorer, which implemented the SVG 1.2 focusable
 attribute and sets all <svg>
 elements to receive keyboard focus by default. (In other browsers and SVG 2, the tabindex
 attribute identifies focusable elements, and <svg>
 is *not*
 focusable by default.)

Some other things to note about the code in Example 3-2
 :

- 
 The SVG icons are sized using em
 units, so that they scale to match the text in size as well as color. Because 1em is the height from the very top to the very bottom of letters, the vertical-alignment
 setting ensures that the bottom of the icon lines up with the bottom of the text (the lowest descending letters), rather than sitting on the text baseline.

- 
 The SVG icons are set to inline-block
 mode instead of the default inline
 . Although this doesn’t usually make a difference (an SVG is always a block), it ensures that the underline on the <a>
 elements isn’t continued through the text within the icon. 
 
 
 
 Whether this should ever happen is debatable; Internet Explorer never propagates an HTML underline through the SVG text, but Firefox and Blink/WebKit browsers do.

To demonstrate the importance of those points, Figure 3-3
 shows the same page (in Firefox) if the definition SVG is set to zero height and width but not absolutely positioned — resulting in a blank line at the top of the page — and the icons are left with the default inline
 display mode and baseline
 vertical alignment.

Coordinating with HTML color is not the only benefit of currentColor
 . 
 The keyword also offers flexibility when customizing the styles on content reused with the <use>
 element. Your reused content can inherit a fill
 value, a stroke
 value, and a color
 value, which can be used to set fill and/or stroke of particular parts of the icon. In the future, inheritable CSS variables will add even more flexibility, but for now, color
 and currentColor
 act as an extra variable for SVG styling.

However, one limitation of currentColor
 is that it will always be just that: a color. In contrast, SVG fill
 and stroke
 can use paint servers as well.
Painted Effects
SVG paint servers are separate elements (<linearGradient>
 , <radialGradient>
 , or <pattern>
 ) that define a rectangle of graphic content. That content is then used to determine the color values for each point within the fill or outline of another shape — or text — that references the paint server in the fill
 or stroke
 property.

Paint servers can create many different graphical effects. A full discussion is outside the scope of this book,
1

 but the possibilities are worth mentioning simply because they are one of the key benefits of using SVG text.

Example 3-3
 showcases a simple effect: a gradient that fades to transparent across the width of the text. Figure 3-4
 shows the result. Although you can’t see the last (essentially transparent) letter, it is still there and can be selected if you copy and paste the text, or search for the words.

Example 3-3.
 Filling SVG text with paint server content
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xmlns:xlink=

"http://www.w3.org/1999/xlink"

     xml:lang=

"en"

     height=

"50px"

 width=

"410px"

>

    <title>

Gradient-Filled Text</title>

    <defs>

        <linearGradient

 id=

"fade"

>

            <stop

 stop-color=

"black"

 stop-opacity=

"1"

 offset=

"0"

/>

            <stop

 stop-color=

"black"

 stop-opacity=

"0"

 offset=

"1"

/>

        </linearGradient>

    </defs>

    <g

 transform=

"translate(10,40)"

       style=

"font-family: Arial;

              font-weight: bold;

              font-size: 24pt;

              fill: url('#fade');"

>

        <text>

A Whiter Shade of Pale</text>

    </g>

</svg>

Gradient-filled text is another reason you might want to use SVG text to replace HTML headings. There are no cross-browser standard methods to fill HTML text with a gradient or image, although there are various workarounds available, 
 
 
 using the non-standard ‑webkit-background-clip: text
 option (only available in WebKit/Blink browsers) or the (relatively new) CSS blending modes.

Example 3-4
 adapts Example 3-1
 to use gradient effects on the SVG heading text, for both fill and stroke; unchanged code is omitted to save space. The gradients used are more subtle than the fade-to-transparent effect; we are, after all, trying to keep the text legible. Figure 3-5
 shows the end result.

Example 3-4.
 Using SVG gradients within HTML headings
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

SVG Gradient Text Headings

</title>

    

<style

>

        

/* basic layout styles unchanged */

        

h1

,

 

h2

 

{

            

font-family

:

 

sans-serif

;

            

color

:

 

coral

;

            

fill

:

 

url(#coral-gradient)

 

currentColor

;

  





        

}

        

h1

 

{

            

stroke

:

 

url(#red-gradient)

 

darkRed

;

       





        

}

        

svg

.defs

 

{

            

display

:

 

block

;

            

position

:

 

absolute

;

            

left

:

 

-10

;

            

height

:

 

0

;

 

width

:

 

0

;

                      





        

}

    

</style>



</head>



<body

>

    

<svg

 

class=

"defs"

 

aria-hidden=

"true"

         

focusable=

"false"

 

width=

"0"

 

height=

"0"

>

        

<linearGradient

 

id=

"coral-gradient"

                        

y2=

"100%"

 

x2=

"50%"

 

>

          





            

<stop

 

offset=

"0.3"

 

stop-color=

"tomato"

 

/>

            

<stop

 

offset=

"0.4"

 

stop-color=

"lightCoral"

 

/>

            

<stop

 

offset=

"0.7"

 

stop-color=

"tomato"

 

/>

            

<stop

 

offset=

"0.9"

 

stop-color=

"crimson"

 

/>

        

</linearGradient>

        

<linearGradient

 

id=

"red-gradient"

                        

xlink:href=

"#coral-gradient"

>

 





            

<stop

 

offset=

"0.3"

 

stop-color=

"darkRed"

 

/>

            

<stop

 

offset=

"0.4"

 

stop-color=

"red"

 

/>

            

<stop

 

offset=

"0.7"

 

stop-color=

"darkRed"

 

/>

            

<stop

 

offset=

"0.9"

 

stop-color=

"#400"

 

/>

        

</linearGradient>

    

</svg>

    

<h1

>

<svg

 

role=

"presentation"

>

        

<text

 

y=

"1em"

>

Level 1 Heading

</text>

    

</svg>

</h1>

    

<!--

 remaining markup is unchanged 

-->



</body>



</html>

The headings will all have the same fill
 gradient. The color
 property and currentColor
 keyword are used to define a single fallback color for when SVG isn’t supported or when there is a problem with the gradient.

The top-level heading text will also have a gradient in the stroke, with a contrasting fallback color.

All these gradients will be defined in their own <svg>
 definitions element, which is hidden using the same styles (and aria-hidden
 attribute) as Example 3-2
 .

Attributes on the <linearGradient>
 element define the gradient position, from the default (0,0) position (top-left corner) to the midpoint of the bottom of the text area. The child <stop>
 elements define the color transition.

The second gradient uses the xlink:href
 attribute to reference the first one; the first one is used as a template, and all of its attributes are copied over. This makes it easier to coordinate the two matching gradients, as the positioning attributes are only written once.

The bounding box used to size the gradient or pattern is based on the layout boxes (*em boxes*
 ) for each character, including room on top for accents and below for descending letter. Many characters do not fill the entire em box. Some characters may even extend beyond it, particularly in decorative calligraphic fonts. The entire letter will still be filled, but the exact scale and position of the gradient or pattern may not be what you expect.
Warning
If the paint server uses userSpaceOnUse
 units, defining it in its own <svg>
 element, as in Example 3-4
 , will not work in many browsers. Firefox is the only browser (at the time of writing) that correctly applies the user space coordinate system from the filled or stroked content, instead of from the gradient’s parent SVG.

As mentioned earlier in the chapter, using SVG to create decorative HTML headings should only be used if you know you will not need the text to wrap to a new line.
Switching Styles
The examples so far have used a single set of styles for a complete <text>
 element. In order to apply different styles to *part*
 of the text, you can wrap the relevant characters in a <tspan>
 element. Like an HTML
 <span>
 , the <tspan>
 can be used to change color or font properties.

Example 3-5
 uses styled <tspan>
 elements to change the colors and font of individual words within a <text>
 element, as displayed in Figure 3-6
 .

Example 3-5.
 Using tspan elements to change the formatting of SVG text
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xml:lang=

"en"

 width=

"10cm"

 height=

"2cm"

>

    <title>

Formatting Text Spans</title>

    <style 

type=

"text/css"

>

        svg

 {

            font-family

:

 serif

;

            font-size

:

 12mm

;

            fill

:

 navy

;

        }

    </style>

    <rect

 fill=

"#CEE"

 width=

"100%"

 height=

"100%"

 />

    <text

 x=

"5mm"

 y=

"1.5cm"

 >

One,
        <tspan

 fill=

"royalBlue"

>

Two,</tspan>

        <tspan

 font-style=

"italic"

 fill=

"royalBlue"

 stroke=

"navy"

               >

Three!</tspan>

    </text>

</svg>

As with the rest of SVG, styles on a <tspan>
 may be specified as presentation attributes or as CSS rules. Example 3-5
 uses CSS rules to set default styles for the entire graphic, and then sets more specific values on the individual <tspan>
 elements with attributes — which always override inherited values, even when those values were originally set with CSS.

When restyling a <tspan>
 with paint server fill or stroke, the bounding box used to scale the paint server is based on the entire <text>
 element, not the individual <tspan>
 . For example, if we wanted to emphasize part of the gradient headings used in Example 3-4
 , we could add markup like the following:

<h1><svg

 role=

"presentation"

>

    <text

 y=

"1em"

>

Level <tspan

 class=

"digit"

>

1</tspan>

                  Heading</text>

</svg></h1>


The class would be referenced in the following style rule to fill in the digit in the same dark red gradient used for the stroke:

h1

 .digit

 {

    fill

:

 url(#red-gradient)

 darkRed

;

}


The result is shown in Figure 3-7
 . The angle and position of all three gradients — the fill on the main text, the fill on the <tspan>
 , and the stroke
 on both — align perfectly to create a continuous reflective effect.

The continuous paint is one reason for using a <tspan>
 within a larger <text>
 element. However, even when using solid-colored fill, a <tspan>
 can make your work much smoother.

If you used three individual <text>
 elements to set the individual words in Example 3-5
 , you would then have to figure out the exact horizontal position for each word. Changing the presentation of text using <tspan>
 , in contrast, does not disrupt the continuous line of text. Each character is still positioned based on the position of the previous character.

Nonetheless, in many cases you *do*
 want to disrupt or reset the position of characters within a line of text. A <tspan>
 can do that as well, as Chapter 4
 will show.

1

 A full discussion of paint servers is actually a book in itself: 
*SVG Colors, Patterns & Gradients*

 (O’Reilly), by the same authors as this book.