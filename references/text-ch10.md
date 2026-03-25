Chapter 7.
 Anchoring in International Waters
Think you’ve got a handle on SVG text alignment now? Not *too*
 complicated, is it? (Is it?) Maybe you’re wondering, though, why the text-anchor
 keywords are not simply left
 , center
 , and right
 . It’s a good question, and the start of a whole other level of complication.

The examples in the book so far have used English text, laid out left to right the way English text normally is. However, other languages arrange text right to left or top to bottom. Even within English and other Western languages, you may want to use vertical text to fit within the layout of a chart or diagram, or for artistic effect.

SVG includes a number of features to support alternative text modes, and this chapter and the next explain how they are supposed to work. However, some of the features were not well designed, and most are not well implemented in web browsers at the time of writing. These chapters therefore contain many warnings about browser incompatibilities. Where practical, they also offer suggestions for workarounds.

If you are developing designs that use non-Western languages, you will want to test your code thoroughly in any software you need to support.

You can also get involved to make things better in the future: provide feedback on web standards in development, and file bug reports and feature requests with browsers. Many developers involved in SVG and CSS have limited experience with non-European languages. Even if all you can do is provide practical use cases of how text layout *should*
 work, it will make a difference.
Starting from the Other End
The start
 and end
 values for text-anchor
 were intended to support mixed-language content without any bias toward left-to-right scripts. It was well-intended, but had the opposite effect: it makes changing the language of SVG designs *more*
 difficult, not less.

In CSS layout, text is contained by the layout box, and alignment moves text within it. Using start
 and end
 values therefore allows the text to align according to the norm for the text language without throwing off the overall layout. In theory, anyway. 
 
 Unfortunately, these values are still rarely used for text-align
 on the Web because of a lack of support from Internet Explorer.

In SVG layout, text is aligned relative to a point, not a box. Look through all the examples in previous chapters, and imagine what would happen if the text started from the same anchor point, and then progressed from right to left (or top to bottom) instead of left to right. The text would either overlap graphics or run off the edge of the SVG.

If you’re switching between left-to-right and right-to-left languages, you’ll often want to use the CSS direction
 property to specify which direction should be used for layout: ltr
 or rtl
 . This will not reverse the characters; the browser will still arrange sequences of characters according to their natural direction as defined in the Unicode specification. However, those sequences of characters will be embedded within an SVG text layout in the direction you specify.
Tip
The default value of the direction property is ltr
 . It is inherited, and can be used as a presentation attribute in SVG.

Whenever possible, design your layout using the natural direction of the text language you will be using. Many characters, such as punctuation, don’t have an implicit direction. They will therefore follow the imposed direction
 value instead of the native direction of the rest of the text. The text within each layout chunk will be broken into sequences of characters with the same direction, and then those sequences arranged according to the direction
 property. With direction: ltr
 , starting punctuation will always be positioned on the left and ending punctuation will be set on the right of the text, even if the remaining text consists entirely of characters ordered from right to left. Punctuation in the middle of the text may match surrounding characters, or may break up the layout completely, depending on how the character is tagged in Unicode.
Warning
When mixing content with different directions, Internet Explorer does not implement the Unicode bidirectional algorithm correctly for SVG. Text sequences can end up in seemingly random positions, offset in the wrong direction from the rest.

If you are translating the text within an SVG diagram, you will need to reverse all the text-anchor
 values for languages with the reverse text direction in order to keep the text in the same region of the graphic.

To demonstrate the interaction between text-anchor
 , direction
 , and the natural direction of text, Example 7-1
 uses a script to generate various combinations. It creates SVGs embedded in an HTML table, with English, Arabic, Greek, and Hebrew text aligned start, middle, or end, and either left-to-right or right-to-left direction imposed. Figure 7-1
 displays the resulting web page. Note that the order of letters remains the same regardless of the layout direction, but the position of the exclamation mark moves so it is always at the “end” of the text, based on the direction
 property. The comma in the English text, in contrast, is surrounded by left-to-right characters, and matches it accordingly.

Example 7-1.
 Using text-anchor and direction on text with different natural directions
<!DOCTYPE html>

<html>

<head>

    <meta

 charset=

"utf-8"

/>

    <title>

Multilingual SVG Text Direction and Layout</title>

    <style>

        table

 {

            width

:

 100%

;

            table-layout

:

 fixed

;

            border-collapse

:

 collapse

;

        }

        th

:nth-child

(

n-2

)

 {

            width

:

 3em

;

        }

        td

,

 th

 {

            border

:

 solid

 royalBlue

;

            padding

:

 0

;

            background

:

 #DEF

;

        }

        svg

 {

            height

:

 5em

;

            width

:

 100%

;

        }

        line

 {

            stroke

:

 gray

;

        }

        text

 {

            fill

:

 currentColor

;

        }

        .start

  {

 color

:

 darkGreen

;

 }

        .middle

 {

 color

:

 navy

;

 }

        .end

    {

 color

:

 darkRed

;

 }

    </style>

</head>

<body>

<script>

(

function

(){

    var

 svgNS

 =

 "http://www.w3.org/2000/svg"

;

    var

 doc

 =

 document

;

    var

 strings

 =

 [

 "Hello, World!"

,

                    "مرحبا أيها العالم!"

,

                    "Γειά Σου Κόσμε!"

,

                    "שלום העולם!"

 ],

        directions

 =

 [

 "ltr"

,

 "rtl"

 ],

        anchors

 =

 [

 "start"

,

 "middle"

,

 "end"

 ];

    var

 table

 =

 doc

.

createElement

(

"table"

),

        tbody

 =

 doc

.

createElement

(

"tbody"

);

    table

.

insertBefore

(

tbody

,

 null

);

    var

 row

,

 rowh

,

 rowsub

,

 cell

,

 svg

,

 line

,

 text

;

    for

 (

var

 i

=

0

,

 a

=

anchors

.

length

;

 i

<

a

;

 i

++

){

        for

 (

var

 j

=

0

,

 d

=

directions

.

length

;

 j

<

d

;

 j

++

)

 {

            row

 =

 doc

.

createElement

(

"tr"

);

            rowh

 =

 doc

.

createElement

(

"th"

);

            rowh

.

textContent

 =

 anchors

[

i

];

            row

.

setAttribute

(

"class"

,

 anchors

[

i

]);

            row

.

insertBefore

(

rowh

,

 null

);

            rowsub

 =

 doc

.

createElement

(

"th"

);

            rowsub

.

textContent

 =

 directions

[

j

];

            row

.

insertBefore

(

rowsub

,

 null

);

            cell

 =

 doc

.

createElement

(

"td"

);

            row

.

insertBefore

(

cell

,

 null

);

            svg

 =

 doc

.

createElementNS

(

svgNS

,

 "svg"

);

            svg

.

setAttribute

(

"direction"

,

 directions

[

j

]);

            svg

.

setAttribute

(

"text-anchor"

,

 anchors

[

i

]);

            cell

.

insertBefore

(

svg

,

 null

);

            line

 =

 doc

.

createElementNS

(

svgNS

,

 "line"

);

            line

.

setAttribute

(

"x1"

,

 "50%"

);

            line

.

setAttribute

(

"x2"

,

 "50%"

);

            line

.

setAttribute

(

"y2"

,

 "100%"

);

            svg

.

insertBefore

(

line

,

 null

);

            for

 (

var

 k

=

0

,

 s

=

strings

.

length

;

 k

<

s

;

 k

++

)

 {

                text

 =

 doc

.

createElementNS

(

svgNS

,

 "text"

);

                text

.

textContent

 =

 strings

[

k

];

                text

.

setAttribute

(

"x"

,

 "50%"

);

                text

.

setAttribute

(

"y"

,

 (

k

+

1.5

)

 +

 "em"

);

                svg

.

insertBefore

(

text

,

 null

);

            }

            tbody

.

insertBefore

(

row

,

 null

);

        }

    }

    doc

.

body

.

insertBefore

(

table

,

 null

);

})();

</script>

</body>

</html>

For mixed direction text, character-by-character positioning attributes are assigned based on the logical reading order, not the displayed
 order. Absolutely positioned characters still form new text chunks.
Tip
The Unicode bidirectionality rules do not reorder characters between different absolutely positioned text chunks.

Rarely, you want to force characters to be arranged in a specific order regardless of the script’s normal direction. This is achieved using the CSS unicode-bidi
 property, set to a value of bidi-override
 . This tells the browser to ignore the bidirectional text algorithm defined by Unicode, and lay out the characters in the exact order and direction specified.

In HTML, text direction is controlled by the dir
 attribute and the <bdo>
 (bidirectional override) element. To maintain correct language layout in the absence of CSS styling, authors are advised to use these features instead of the CSS direction
 and unicode-bidi
 properties.
Warning
When SVG is inline in HTML, Firefox, WebKit, and Blink browsers use the HTML document’s direction as the default SVG direction, but Internet Explorer does not. For consistent results, explicitly set direction
 and unicode-bidi
 on your SVG elements.

As mentioned in Chapter 5
 , the direction of the text does not affect the direction of a dx
 offset. Positive or negative dx
 values shift the text position toward the positive or negative *x*
 -axis, respectively. For right-to-left text, a positive dx
 will reduce spacing and a negative offset will increase it.
Head to Toe Layout
SVG was also designed to support vertically oriented text. Although the primary purpose was to support languages in which text is naturally written top to bottom, it can also be used for graphical effect, such as to create vertical labels on a chart.

The SVG vertical writing properties currently have limited support. Better support is expected for a new CSS Writing Modes Module. However, the syntax is different — simplifying many of the most confusing
 aspects of the SVG specification, but adding compatibility headaches.
Warning
Firefox never implemented SVG vertical writing. Developers have begun implementing the new CSS vertical writing mode specification, but at the time of writing the user must specifically enable support.

Other browsers’ implementations are inconsistent and buggy, especially when mixing right-to-left text direction with vertical writing mode. Blink has the most complete SVG vertical writing implementation, but in versions prior to Chromium 44, the layout is thrown off when the user zooms the browser window.

If using vertical writing in SVG, test your work thoroughly in all browsers and devices you need to support.

For most languages and scripts where vertical writing is common, the character glyphs are positioned upright, one under the other. For other scripts, the entire string of glyphs is rotated sideways. When Latin letters (or Greek or Cyrillic, etc.) are included in upright vertical text, they may likewise be oriented upright or they may be rotated. Cursive scripts such as Arabic would nearly always be rotated sideways when embedded in vertical text, because positioning letters upright one under the other would interrupt the normal word forms of the letters.

A set of related style properties define which of these modes should be used. The writing-mode
 style property defines whether vertical or horizontal text should be used. The SVG 1.1 specifications define many possible values, but they all add up to two different modes:

- 
lr
 , lr-tb
 , rl
 , or rl-tb
 create horizontal text. The distinction between the rl
 values (right-to-left) and lr
 values (left-to-right) does not have an effect; use the direction
 property for that.

- 
tb
 or tb-rl
 create vertical text. The rl
 part of the long form indicates that lines are normally stacked from the right to the left across the page; because SVG 1.1 does not have any automatic line wrapping, this is not usually relevant.
Tip
These redundant values will be simplified by CSS writing modes, as described later in the chapter. For best support, you will need to use old and new values as fallbacks.

In SVG 1.1, the orientation of the individual characters within vertical text is controlled by the glyph-orientation-vertical
 property. The default value (auto
 ) prints characters upright if they come from a script where that is normal, or rotates them otherwise. The upright characters include some special punctuation characters designed for vertical scripts, or variants on Latin characters or numbers that are particularly encoded for display as full-width characters.

To change the auto
 setting, you can set glyph-orientation-vertical
 to an orientation angle that should apply to all characters. The only option that produces logical text is 0
 , which forces all the characters to remain upright while stacked top to bottom.
Tip
The glyph-orientation-vertical
 can also be set to 90
 , 180
 , or 270
 to rotate characters. However,
 the results are not particularly readable, and there are no direct equivalents in the new CSS text-orientation
 property in CSS Writing Modes.

If you’re looking to create an off-kilter layout with upside-down text, you have more flexibility using rotate
 .

There is also a glyph-orientation-horizontal
 property; it does not include an auto
 value, and the default is an angle of 0 (upright characters).
Warning
The glyph orientation settings are only supported in WebKit/Blink browsers. Although Internet Explorer supports the tb
 writing mode, the glyph orientation is always auto
 .

Example 7-2
 demonstrates the use of writing-mode
 and glyph-orientation-vertical
 for mixed Chinese-and-English text and for all-English text. Figure 7-2
 shows the result in a browser that supports both features (Chrome). The text is shown with both start
 and end
 text anchors, and the anchor point for each text element is marked by a circle.

Example 7-2.
 Using vertical writing mode in SVG
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

xmlns:xlink=

"http://www.w3.org/1999/xlink"

     

width=

"4.3in"

 

height=

"4.5in"

>

    

<title

>

Mixed Scripts and Writing Modes

</title>

    

<style 

type=

"text/css"

>

        

svg

 

{

            

font

:

 

20px

 

sans-serif

;

            

stroke-width

:

 

2px

;

        

}

        

text

 

{

            

fill

:

 

currentColor

;

        

}

    

</style>

    

<defs

>

        

<circle

 

id=

"anchor"

 

r=

"3px"

 

fill=

"currentColor"

                

stroke=

"currentColor"

 

fill-opacity=

"0.5"

/>

      





    

</defs>

    

<rect

 

fill=

"#CEE"

 

width=

"100%"

 

height=

"100%"

 

/>

    

<g

 

color=

"darkGreen"

 

text-anchor=

"start"

>

        

<g

 

id=

"sample"

 

fill=

"currentColor"

 

>

            

<use

 

xlink:href=

"#anchor"

 

x=

"1em"

 

y=

"2em"

 

/>

            

<text

 

x=

"1em"

 

y=

"2em"

 

>

Hi, SVG World!

</text>

        





            

<use

 

xlink:href=

"#anchor"

 

x=

"2.5em"

 

y=

"3em"

 

/>

            

<text

 

x=

"2.5em"

 

y=

"3em"

 

>

您好SVG世界！

</text>

       





        

</g>

        

<use

 

xlink:href=

"#sample"

 

transform=

"translate(0,50)"

             

writing-mode=

"tb"

 

/>

                               





        

<use

 

xlink:href=

"#sample"

 

transform=

"translate(100,50)"

             

writing-mode=

"tb"

 

glyph-orientation-vertical=

"0"

/>

 





    

</g>

    

<g

 

color=

"darkRed"

 

text-anchor=

"end"

       

transform=

"translate(200,0)"

 

>

                           





        

<use

 

xlink:href=

"#sample"

 

transform=

"translate(140,0)"

 

/>

        

<use

 

xlink:href=

"#sample"

 

transform=

"translate(0,350)"

             

writing-mode=

"tb"

 

/>

        

<use

 

xlink:href=

"#sample"

 

transform=

"translate(100,350)"

             

writing-mode=

"tb"

 

glyph-orientation-vertical=

"0"

/>

    

</g>



</svg>

The anchor
 circle is defined to be centered around the origin, so it will end up centered on whichever (*x*
 ,*y*
 ) point is defined in the <use>
 element. It uses the currentColor
 keyword to set both fill and stroke to the same color, but with different opacity values.

The phrase “Hi, SVG World!” is used as a sample of Latin text.

For comparison with a language with a natural vertical writing mode, the phrase has been translated into traditional Chinese characters; the acronym “SVG” remains written in Latin characters. The exclamation mark at the end is a special Unicode full-width punctuation character which will not normally rotate when typeset vertically. The two text elements have different anchors in both the *x*
 - and *y*
 -directions, so they will not overlap regardless of whether they are set horizontally or vertically.

The group containing both text elements and their anchor-markers is duplicated, but with writing-mode
 set to top to bottom (tb
 ). The glyph orientation will use the default auto
 setting.

The text and anchors are duplicated again, with top-to-bottom mode but also an explicit upright glyph orientation for all characters.

Finally, all the options are repeated again, but with text-anchor
 set to end
 . The translations have been adjusted to position the anchor points at the far edge of the graphic, so that the text does not overlap or run off the edge.

Once again, all the text direction options do not change the direction of x
 , y
 , dx
 , or dy
 offsets. Even if top-to-bottom Latin text *looks*
 the same as text that has been rotated with a transformation, the coordinate
 system has not changed. Absolute y
 positions create text chunks for alignment purposes in vertical text, not absolute x
 positions.

Until vertical writing modes are well supported, SVG does have one well-supported option for setting sideways text: rotate the entire text element using transform
 . To set text upright, top to bottom, the most reliable approach is to give x
 and dy
 values for each character. To create mixed orientation text, you can rotate the text element as a whole with transformations, then back-rotate individual characters with the rotate
 attribute, and finally, correct the spacing with dx
 .

Using this approach, we can create Figure 7-3
 , which looks almost the same as Figure 7-2
 , but works in all the major web browsers; the screenshot is from Firefox 40. The rather ugly markup required to create the layout is given in Example 7-3
 .

Example 7-3.
 Simulating vertical writing mode using well-supported SVG features
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

xmlns:xlink=

"http://www.w3.org/1999/xlink"

     

width=

"4.3in"

 

height=

"4.5in"

>

    

<title

>

Faking Vertical Writing Modes

</title>

    

<style 

type=

"text/css"

>

        

svg

 

{

            

font

:

 

20px

 

sans-serif

;

            

stroke-width

:

 

2px

;

        

}

        

text

 

{

            

fill

:

 

currentColor

;

        

}

    

</style>

    

<defs

>

        

<circle

 

id=

"anchor"

 

r=

"3px"

 

fill=

"currentColor"

                

stroke=

"currentColor"

 

fill-opacity=

"0.5"

/>

    

</defs>

    

<rect

 

fill=

"#CEE"

 

width=

"100%"

 

height=

"100%"

 

/>

    

<g

 

color=

"darkGreen"

 

text-anchor=

"start"

>

        

<g

 

id=

"sample"

 

fill=

"currentColor"

 

>

            

<use

 

xlink:href=

"#anchor"

 

x=

"1em"

 

y=

"2em"

 

/>

            

<text

 

x=

"1em"

 

y=

"2em"

 

>

Hi, SVG World!

</text>

     





            

<use

 

xlink:href=

"#anchor"

 

x=

"2.5em"

 

y=

"3em"

 

/>

            

<text

 

x=

"2.5em"

 

y=

"3em"

 

>

您好SVG世界！

</text>

        

</g>

        

<g

 

id=

"rotated"

 

transform=

"translate(0,50)"

 

>

            

<use

 

xlink:href=

"#anchor"

 

x=

"1em"

 

y=

"2em"

 

/>

            

<text

 

x=

"2em"

 

y=

"-1em"

 

transform=

"rotate(90)"

                  

dy=

"1ex"

                  

>

Hi, SVG World!

</text>

                      





            

<use

 

xlink:href=

"#anchor"

 

x=

"2.5em"

 

y=

"3em"

 

/>

            

<text

 

x=

"3em"

 

y=

"-2.5em"

 

transform=

"rotate(90)"

                  

rotate=

"-90 -90 0 0 0 -90 -90 -90"

                  

dx=

"1em 0 -0.7em 0 0 1em"

 

dy=

"1ex"

                  

>

您好SVG世界！

</text>

                         





        

</g>

        

<g

 

id=

"upright"

 

transform=

"translate(100,50)"

 

>

            

<use

 

xlink:href=

"#anchor"

 

x=

"1em"

 

y=

"2em"

 

/>

            

<text

  

x=

"1em 1em 1em 1em 1em 1em 1em
                      1em 1em 1em 1em 1em 1em 1em"

                  

dy=

"1em 1em 1em 1em 1em 1em 1em
                      1em 1em 1em 1em 1em 1em 1em"

                  

text-anchor=

"middle"

                  

y=

"2em"

 

>

Hi, SVG World!

</text>

              





            

<use

 

xlink:href=

"#anchor"

 

x=

"2.5em"

 

y=

"3em"

 

/>

            

<text

  

x=

"2.5em 2.5em 2.5em 2.5em 2.5em 2.5em 2.5em
                      2.5em"

                  

dy=

"1em 1em 1em 1em 1em 1em 1em 1em"

                  

text-anchor=

"middle"

                  

y=

"3em"

 

>

您好SVG世界！

</text>

                 





        

</g>

    

</g>

    

<g

 

color=

"darkRed"

 

text-anchor=

"end"

       

transform=

"translate(200,0)"

 

>

        

<use

 

xlink:href=

"#sample"

 

transform=

"translate(140,0)"

 

/>

        

<use

 

xlink:href=

"#rotated"

 

transform=

"translate(0,300)"

 

/>

 





        

<g

 

id=

"upright-end"

 

transform=

"translate(100,350)"

           

text-anchor=

"middle"

>

            

<use

 

xlink:href=

"#anchor"

 

x=

"1em"

 

y=

"2em"

 

/>

            

<text

  

x=

"1em 1em 1em 1em 1em 1em 1em
                      1em 1em 1em 1em 1em 1em 1em"

                  

dy=

"-13em 1em 1em 1em 1em 1em 1em
                        1em 1em 1em 1em 1em 1em 1em"

                  

y=

"2em"

 

>

Hi, SVG World!

</text>

              





            

<use

 

xlink:href=

"#anchor"

 

x=

"2.5em"

 

y=

"3em"

 

/>

            

<text

  

x=

"2.5em 2.5em 2.5em 2.5em 2.5em 2.5em 2.5em
                      2.5em"

                  

dy=

"-7em 1em 1em 1em 1em 1em 1em 1em"

                  

y=

"3em"

 

>

您好SVG世界！

</text>

        

</g>

    

</g>



</svg>

The horizontal text samples are the same as in Example 7-2
 .

The sideways Latin text is created with a rotational transformation. However, because the rotation also affects the x
 and y
 attributes on the <text>
 element, they have to be changed as well: the anchor point is (1em,2em) in the base coordinate system, but (2em,–1em) in the rotated coordinate system. Finally, a dy
 attribute shifts the text to be approximately centered under the anchor (a vertical shift in the rotated coordinate system creates a horizontal shift on the page) to match the default behavior of top-to-bottom text.

The mixed-orientation Chinese and English text is more complicated. Again, the text as a whole is rotated, and the x
 and y
 attributes adjusted accordingly. The rotate
 attribute turns the Chinese characters upright again, and then the dx
 value corrects the spacing when switching between rotated and un-rotated characters; in the rotated coordinate system, dx
 adjustments move letters up and down on the page. The dy
 attribute again shifts the entire string to center it under the anchor point.

The upright vertical text is created entirely with character-by-character positioning, each letter reset to the absolute *x*
 -coordinate and offset to a new line with the dy
 value. The letters are centered under the original anchor point with text-anchor="middle"
 . Because this is still technically horizontal text, the text-anchor
 property controls horizontal alignment.

For fully upright text, no special strategy is required for mixed Latin and Chinese characters. The only difference compared to the all-Latin text is that the x
 and y
 positions use a different anchor point, and there are fewer characters overall.

The rotated text can be reused and switched to end alignment simply by changing the inherited value of text-anchor
 . The transformations on the <use>
 element and the reused <g>
 add together, so the translation on the <use>
 has been reduced accordingly.

Switching to end alignment requires more work for the upright text. Changing text-anchor
 does not have the intended effect; these are single-character horizontal text chunks for anchoring purposes. Instead, the initial dy
 value is adjusted as required to shift the entire text string to the correct side of the anchor point.

Although the final appearance is acceptable, the code is repetitive and difficult to maintain. Any change to the text content or layout will require many coordinated changes in positioning attributes.

The code also includes adjustments for one feature we have not discussed yet. In horizontal text, the anchor point is by default aligned with the bottom of the Latin letters. In contrast, vertical text is by default centered around the anchor point (this is changed for sideways text in CSS Writing Modes). Both of these are examples of *alignment baselines*
 , which are the topic of Chapter 8
 .