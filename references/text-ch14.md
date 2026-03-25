Chapter 11.
 The Perfect Fit
If you cannot always control the font the browser uses, even with web fonts, what can you do to ensure that your overall text layout is preserved?

There are two ways you can hint to the browser how much space a given text element should take up. One comes from CSS, and adjusts the used font size. The other is SVG specific, and adjusts the spacing and width of the letters. Surprisingly, it is the CSS option that has the poorer browser support, although neither is as reliable as one would like.
Fixing Font Size
To reemphasize *why*
 text size control is important, consider Figure 11-1
 . It shows the SVG from Figure 10-2
 , and then the exact same SVG code displayed using a fallback cursive
 font (Comic Sans MS) instead of the chosen web fonts. The font itself doesn’t have the fun
 and artistic feel of the web fonts, but it is the sizing that really creates the problem. The floating letters running off the page significantly distract from the appearance of writing on paper, don’t you think?

Part of the problem is that the lowercase letters in Comic Sans take up a much larger proportion of the height; in other words, their ex-height is larger for the same em-height. The letters are correspondingly wider, despite the same font-size
 . This is a common source of inconsistency in text sizing when changing between font families.
Note
The ex-height to em-height ratio is often known as the aspect ratio or aspect value of a font, although it does not directly correspond to the width to height ratio.

The problem is so common, even with non-graphical text layouts, that CSS 2 introduced the font-size-adjust
 property. Its value is a number (usually between 0.4 and 0.6) that represents the ex-height to em-height ratio of your *preferred*
 font. If the browser is using a font with a different aspect ratio, it is supposed to adjust the used font size so that the *ex*
 -height remains constant, instead of the em-height.

Unfortunately, browser uptake was poor. 
 At the time of writing, Firefox is the only major browser that implements font-size-adjust
 . 
 It was removed from the core CSS 2.1 specification in favor of including it within CSS 3 Fonts.

One of the main problems with font-size-adjust
 is that it can be difficult to calculate the correct aspect ratio. Font files do not always explicitly state the ex-height value, and different browsers use different tools to estimate it.

For any given browser (which at this point means, for Firefox), the best way to determine the aspect ratio of a font is to create a sample page in that font. Then, incrementally change the font-size-adjust
 value until the displayed text size matches the size when font-size-adjust
 is left as the default none
 .
Tip
You can use a :hover
 or :focus
 pseudoclass selector rule to reset font-size-adjust
 to none
 , allowing you to quickly switch back and forth between the normal and adjusted size, to see if there is a change.

Adjusting the font size addresses the apparent size and legibility of individual letters, but it does not directly alter the layout impact from the width of the letters. The SVG textLength
 attribute fills this need. It specifies exactly how long the text should be, in your chosen font. The browser should adjust the displayed text to match.
Tip
textLength
 is an XML attribute, not a presentation attribute. It cannot be set using CSS.

The specifications allow textLength
 to be specified either for the <text>
 as a whole or for an individual <tspan>
 , but this is not well supported in browsers:
Warning
Blink/WebKit browsers do not adjust content within a <tspan>
 or <textPath>
 based on the textLength
 for a parent <text>
 element.

Firefox does not respond to textLength
 on a <tspan>
 or <textPath>
 , only on <text>
 .

Internet Explorer can handle constraints on any text element, but is very erratic — with sudden shifts in layout as the user interacts with the graphic — if there are lengths set on *both*
 the <text>
 and the child <tspan>
 or <textPath>
 .

In other words, for consistent results in current browsers, only use textLength
 on individual <text>
 elements without child <tspan>
 or <textPath>
 elements.

Example 11-1
 integrates both these features into the pen-pal letter SVG from Example 10-1
 . The multiline text has been broken into separate
 absolutely positioned <text>
 elements each with a textLength
 attribute. The font sizes are standardized with font-size-adjust
 . Figure 11-2
 shows the resulting layout, with or without web fonts, and with or without support for font-size-adjust
 .

Example 11-1.
 Using text sizing hints to control fallback fonts
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

 

xml:lang=

"en"

     

xmlns:xlink=

"http://www.w3.org/1999/xlink"

     

width=

"4.3in"

 

height=

"4.3in"

 

viewBox=

"0 0 400 400"

>

    

<title

>

Dear Pen Pal

</title>

    

<style 

type=

"text/css"

>



@font-face

 

{

                                               





    

font-family

:

 

"hand-writing"

;

    

font-weight

:

 

normal

;

    

font-style

:

 

italic

;

    

src

:

 

/* local("Morado Marker"), local("morado-marker"), */

         

url

(

"../Fonts/morado marker.woff"

)

 

format

(

"woff"

)

,

         

url

(

"../Fonts/morado marker.ttf"

)

 

format

(

"truetype"

)

;

    

/* Morado by Peter Wiegel
       Downloaded from: http://www.dafont.com/morado.font
    */



}



@font-face

 

{

    

font-family

:

 

hand-writing

;

    

src

:

 

local

(

"CoolStory"

)

,

 

local

(

"CoolStory Regular"

)

,

         

url

(

"../Fonts/coolstory regular.woff"

)

 

format

(

"woff"

)

,

         

url

(

"../Fonts/coolstory regular.ttf"

)

 

format

(

"truetype"

)

;

    

/* CoolStory by Peter Olexa
       Downloaded from: http://www.dafont.com/coolstory.font
    */



}



svg

 

{

    

background-color

:

 

lightSkyBlue

;

    

border

:

 

solid

 

thin

;

    

margin

:

 

10px

;

    

font-family

:

 

"hand-writing"

,

 

cursive

;

    

font-size

:

 

36px

;

    

font-size-adjust

:

 

0

.

50

;

                                







}

    

</style>

    

<defs

>

        

<rect

 

id=

"paper"

 

width=

"300"

 

height=

"350"

 

rx=

"3"

/>

    

</defs>

    

<g

 

transform=

"translate(-5,50) rotate(-20) skewY(5)"

>

        

<use

 

xlink:href=

"#paper"

 

transform=

"translate(-10,15)"

             

fill=

"black"

 

fill-opacity=

"0.3"

/>

        

<use

 

xlink:href=

"#paper"

 

fill=

"linen"

 

stroke=

"bisque"

/>

        

<g

 

transform=

"translate(0,20)"

>

            

<text

 

font-style=

"italic"

                  

x=

"30"

 

y=

"50"

 

>

Dear Pen Pal,

</text>

      





            

<text

 

x=

"10"

 

y=

"100"

 

textLength=

"245"

                   

>

Today, I received a

</text>

             





            

<text

 

x=

"10"

 

y=

"150"

 

textLength=

"260"

                   

>

new letter from my

</text>

            

<text

 

x=

"10"

 

y=

"200"

 

textLength=

"200"

                   

>

pen pal.  Hooray!

</text>

            

<text

 

font-style=

"italic"

                  

x=

"100"

 

y=

"250"

 

>

Sincerely,

</text>

            

<text

 

x=

"200"

 

y=

"300"

 

font-size=

"larger"

>

Me

</text>

        

</g>

    

</g>



</svg>

The @font-face
 rules have not changed.

The font-size-adjust
 value was determined empirically in Firefox to be the value that did *not*
 cause adjustments in the preferred web font.

The “italic” font used in the greeting and signature line no longer has a separate font-size
 presentation attribute. Instead, it will be scaled with font-size-adjust
 to match the rest of the text.

Each of the full-width text lines is given its own textLength
 value. Again, the values were determined empirically, by finding the value that did *not*
 change the text length when the preferred font was used.

Although the fallback versions in Figure 11-2
 are not as artistic as the web font versions, the overall layout is preserved. Without font-size-adjust
 , the cramped letters make it slightly difficult to read, but it is acceptable.

The biggest remaining problem with the layout occurs when the web fonts *are*
 used, but font-size-adjust
 is not. Although the two web fonts had a similar overall look, they have extremely different aspect ratios, and so look mismatched when they are set with the same font size. Even with font-size-adjust
 , the resulting size doesn’t match the size used in the original graphic (compare with Figure 11-1
 ), when the font size was adjusted manually.

It would have been better to set them as completely different fonts, with different font sizes and font-size-adjust
 values. However, we would then need to also give them textLength
 values to cancel out the effect of the larger font size on the fallback fonts when font-size-adjust
 is not supported.

Given these examples, a few guidelines can be established, based on current browser support:

- Use textLength
 whenever too-long or too-short text will significantly throw off the layout, but only if the text can be laid out as individual <text>
 elements.

- Consider using font-size-adjust
 to minimize the amount of layout squeezing or stretching that the browser has to do to match the textLength
 .

- Do not rely on font-size-adjust
 to synchronize the sizes of very different fonts.

- 
 Do not create composite font families (like the mismatched hand-writing
 family used here) unless the individual font faces have very similar dimensions at the same font-size
 setting.

Although font-size-adjust
 cannot be relied on except for small, optional adjustments, textLength
 is essential for many SVG layouts. However, manually determining the length for each span through trial and error can be a hassle. Fortunately, it is also not necessary.
Measuring SVG Text
Every SVG element has a corresponding object within the document object model (DOM) created by a web browser parsing your markup. These DOM objects can be created, deleted, or modified by scripts running in the browser.

All SVG elements can be manipulated by the core DOM methods defined for all XML and HTML documents. However, the SVG specifications also define their own DOM interfaces for each element type, with properties and methods to make it easier to geometrically manipulate graphics.

SVG text elements, in particular, have methods for determining the position and angle of individual letters and the amount of space consumed by the text. The getComputedTextLength()
 method returns the total length of an element’s text content in the text direction. 
 It includes any letter or word spacing or internal offsets from dx
 and dy
 attributes, and uses the font metrics of the element as it is currently displayed. This is the exact same computation used for the textLength

 attribute: if the two lengths match, no adjustment is applied.

As a result, to find out which values you should use in your textLength
 attributes, open your SVG in a browser that displays it just the way you want. Use the developer’s console (which can usually be accessed with the F12 key) to select all the <text>
 , <tspan>
 , and <textPath>
 elements, and then print out the computed text length for each. Copying and pasting the following code should do the trick:

var

 texts

 =

 document

.

querySelectorAll

(

"text, tspan, textPath"

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

texts

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

    console

.

log

(

texts

[

i

].

getComputedTextLength

(),

                texts

[

i

].

tagName

,

                texts

[

i

].

textContent

.

slice

(

0

,

8

));

}


Each element’s length will be printed out along with the type of tag and the first few letters (so you can figure out which one’s which). The computed values do *not*
 include any adjustments from textLength
 attributes.
Warning
When run in an HTML document, the preceding snippet will not select <textPath>
 elements in older WebKit and Blink browsers, as their selector-matching algorithm would automatically lowercase all selectors, but would not automatically lowercase SVG tag names. As mentioned in Chapter 9
 , the latest browser versions have implemented workarounds, treating SVG tag names as case insensitive in HTML.

For the original pen-pal letter (Example 10-1
 , with nested <tspan>
 elements and without any font-size-adjust
 changes), the code prints out the following values in Firefox:

235.9729461669922 "text" "Dear Pen"
737.9719848632812 "text" "       "
248.21682739257812 "tspan" "Today, I"
261.57379150390625 "tspan" "new lett"
210.3721160888672 "tspan" "pen pal."
171.97085571289062 "text" "Sincerel"
53.984378814697266 "text" "Me"
In other browsers, the exact values will be slightly different even with the same fonts and font sizes, because of differences in the text layout algorithms used. However, they should be fairly close. These values are also close to the ones used in Example 11-1
 : 248 versus 245 for the line starting “Today,” 261.5 versus 260 for the span starting “new letter.” In other words, you can round the values off without having a noticeable impact.

The getComputedTextLength()
 function is only one of many SVG-specific DOM methods available to help you calculate layouts. Although for textLength
 it is used to determine a value that is then hard coded in the SVG markup, these methods are in general most useful when using JavaScript to create dynamic SVG layouts based on changing data.
Fun with Font Adjustments
Although *intended*
 to be used to control browser and font differences in text layout, the textLength
 attribute can also be used for graphical effect. By setting textLayout
 to a value significantly different from the natural length of the text, you can force the browser to stretch or compress the text.
Warning
Internet Explorer does not correctly apply text-anchor
 values other than start
 when the length of the text is constrained with textLength
 : the text is positioned based on its default length, not its adjusted length, so the result is misaligned.

Also keep in mind that some older browsers and other tools do not support textLength
 at all.

By default, the adjustments to text length are made by changing the spacing between letters evenly throughout the text (but not at the start or end of the string). 
 
 Text length adjustments can therefore be a good substitute for letter-spacing
 and word-spacing
 in Firefox — although you won’t be able to control how much of the extra spacing is positioned between letters versus between words.

However, for graphical effect, one of the most popular choices is to stretch or compress the letters themselves. This is done by setting the lengthAdjust
 attribute to spacingAndGlyphs
 , versus the default value spacing
 .
Tip
Again, lengthAdjust
 is an XML attribute, not a style property. The spacingAndGlyphs
 value must be explicitly set on every element with a textLength
 attribute.

Example 11-2
 uses SVG to draw HTML table headings that are stretched or compressed to the same length. Figure 11-3
 shows the final web page.

Example 11-2.
 Using textLength and lengthAdjust to stretch or compress text to a fixed length
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

SVG lengthAdjust for Graphical Effect

</title>

    

<style

>

        

body

 

{

            

font

:

 

large

 

serif

;

            

padding

:

 

1em

;

            

background

:

 

lightSlateGray

;

        

}

        

table

 

{

            

table-layout

:

 

fixed

;

            

border-collapse

:

 

collapse

;

            

background

:

 

white

;

            

width

:

 

100%

;

        

}

        

th

,

 

td

 

{

            

border

:

 

solid

 

thick

 

dimgray

;

        

}

        

th

 

{

            

font

:

 

bold

 

x-large

 

sans-serif

;

            

background-color

:

 

#EEF

;

            

border-style

:

 

double

;

            

color

:

 

#222

;

        

}

        

th

 

svg

 

{

            

display

:

 

block

;

            

height

:

 

1.25em

;

            

width

:

 

100%

;

            

max-width

:

 

12em

;

            

margin

:

 

auto

;

            

padding

:

 

0.25em

 

0

;

            

overflow

:

 

visible

;

         





        

}

        

th

 

svg

 

text

 

{

            

fill

:

 

currentColor

;

            

font-size

:

 

24px

;

           





        

}

        

td

 

{

            

padding

:

 

0.2em

;

        

}

    

</style>



</head>



<body

>

    

<table

>

        

<tr

>

            

<th

>

<svg

 

viewBox=

"0 0 135 30"

>

                 





                

<text

 

dy=

"1em"

 

textLength=

"135"

                      

lengthAdjust=

"spacingAndGlyphs"

                      

>

This text is too long

</text>

        





            

</svg>

</th>

            

<td

>

Look! Each heading has the
                same length.

</td>

        

</tr>

        

<tr

>

            

<th

>

<svg

 

viewBox=

"0 0 135 30"

>

                

<text

 

dy=

"1em"

 

textLength=

"135"

                      

lengthAdjust=

"spacingAndGlyphs"

                      

>

Short

</text>

                        





            

</svg>

</th>

            

<td

>

Cool effect, don't you think?

</td>

        

</tr>

        

<tr

>

            

<th

>

<svg

 

viewBox=

"0 0 135 30"

>

                

<text

 

dy=

"1em"

 

textLength=

"135"

                      

lengthAdjust=

"spacingAndGlyphs"

                      

>

Just Right

</text>

            

</svg>

</th>

            

<td

>

They compress on smaller screens, too!

</td>

        

</tr>

    

</table>



</body>



</html>

The styles on each <svg>
 control its size to fit within the table. It fills the full width of the cell if necessary, but no more than 1.25em height (in the x-large
 font size set on the <th>
 ). The SVGs’
 scaled viewBox
 will fit into this size, centered in the available space, using the default xMidYMid meet
 value for preserveAspectRatio
 .

The text uses the color
 set in the table heading (<th>
 ) element. The 24px font-size
 value will be interpreted within the scaled SVG coordinate system.

The viewBox
 fits 30 units within the height of the SVG. At an internal font size of 24px, that’s 1.25em. Because the SVG was also set to a height of 1.25em, the result is that the text will scale to match the x-large
 value set in the table. Which happens to be equal to 24px in my browsers, but might be different in yours. The SVG coordinate system — and therefore the text — will scale down if the SVG becomes too narrow for the viewBox
 aspect ratio.

The text element is positioned vertically in the space with dy
 , but starts at x="0"
 by default. The textLength
 attribute sets it to fill the full width declared in the viewBox
 , while lengthAdjust
 tells the browser to stretch the glyphs as required to make the text fit.

The other headings use the exact same attributes on the <svg>
 and <text>
 elements.

The SVG includes a viewBox
 so the text will scale down (in both directions) when there is not enough room. 
 Unfortunately, textLength
 alone cannot achieve the same effect: the textLength
 property cannot be expressed as a percentage of the coordinate system, only as a length (with units or as a number of user units).

This example again emphasizes how SVG text can be used within HTML to create short stretches of decorative text. However, there are also many text layouts that are easy with HTML (and basic CSS layout) but not SVG. For diagrams and data visualizations with a lot of text, it would be nice to use this HTML formatting within your SVG graphic. Chapter 12
 describes how.