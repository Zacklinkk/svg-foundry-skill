Chapter 4.
 Multiline SVG Text
A single SVG <text>
 element creates a single line of text. It does not —
 in SVG 1.1, anyway — have any way of wrapping text to a new line. For this reason, when text consists of more than independent short labels, individual <text>
 elements positioned at explicit points on the page are usually insufficient.

For longer text, you need to break the text into smaller chunks to position them separately. However, you often still want to coordinate the position of different words to reflect that they are part of a continuous whole. This is true not only for normal paragraph-like text wrapping, but also for an area in which SVG excels: complex text layouts used in posters, advertisements, and poetry.

Individual spans of SVG text can be shifted from their natural position, or repositioned completely. This chapter discusses the basic attributes to position spans of text, showing how you can move the virtual typewriter to a new point on the page. However, many style options affect the final position of the characters, and the following chapters will introduce these complexities.
Stepping Up
The <tspan>
 element can be used to identify segments of text for positioning as well as for styling. By default, each <tspan>
 is aligned next to the previous character in the text string, but attributes can reset or adjust that position.
Tip
Although a <tspan>
 element can be positioned independently, it cannot be used on its own: it must be inside a <text>
 element, which declares a block of SVG text.

There are four positioning attributes for SVG text: the x
 and y
 that we have already seen, and also dx
 and dy
 . While the first two declare absolute positions within the coordinate system, the latter two declare differences (or *deltas*
 ) in position, which should be added to the position that otherwise would apply.

Any or all of these attributes can be applied to both <text>
 and <tspan>
 elements. Which to use depends on whether later parts of text should adjust if you change previous parts. If a <tspan>
 element has both x
 and y
 attributes, it is positioned independently of the previous content in the <text>
 element — and of the x
 and y
 attributes on the <text>
 itself.
Tip
Using a single <text>
 element provides a logical grouping — combining different pieces of text into a continuous whole — even if each <tspan>
 is positioned independently. This can affect copy-and-paste operations, screen readers, and search engine optimization.

Because x
 and y
 are independent attributes, you can control the position along one axis while letting the other axis be calculated automatically based on the text flow. 
 
 The default x
 /y
 behavior for <tspan>
 is automatic positioning, in contrast to <text>
 elements, where these attributes default to 0.

Example 4-1
 uses the y
 attribute to offset the vertical position of words within a text string. It uses the same text content and styles as Example 3-5
 . However, classes are used to set the styles so they don’t distract from the geometric attributes in the markup. Figure 4-1
 shows the result.

Example 4-1.
 Resetting the position of text using absolute attributes on tspan elements
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xml:lang=

"en"

 width=

"10cm"

 height=

"2.5cm"

>

    <title>

Positioning tspan</title>

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

        .em

 {

            fill

:

 royalBlue

;

        }

        .strong

 {

            stroke

:

 navy

;

            font-style

:

 italic

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

"2.1cm"

 >

One,
        <tspan

 class=

"em"

 y=

"1.6cm"

>

Two,</tspan>

        <tspan

 class=

"strong em"

 y=

"1.1cm"

>

Three!</tspan>

    </text>

</svg>

The exact same result can also be achieved using the *relative*
 positioning attributes, as follows:

<text

 x=

"5mm"

 y=

"2.1cm"

 >

One,
    <tspan

 class=

"em"

 dy=

"-0.5cm"

>

Two,</tspan>

    <tspan

 class=

"strong em"

 dy=

"-0.5cm"

>

Three!</tspan>

</text>


The first dy
 shifts the current text position from 2.1cm to 1.6cm; the second span then starts from that position and the dy
 value shifts it up another half centimeter.

The <text>
 element as a whole is still positioned absolutely, but the <tspan>
 elements are positioned using dy
 to specify vertical offsets, instead of y
 to specify the final destination. The main benefit of using dy
 and dx
 is that you can move the entire element as a whole by changing the initial position value. All the pieces maintain their relative position.

Using dy
 and dx
 also allows you to specify the position of your text as a mix of length and percentage units. When an element (<text>
 or <tspan>
 ) has both absolute and relative position attributes for a given direction (horizontal or vertical), the delta adjustments are applied *after*
 moving to the absolute position. You can therefore set the base position (x
 or y
 ) using a percentage, then offset it by a fixed amount using dx
 or dy
 .

To ensure that the text from Example 4-1
 always remains vertically centered within the SVG — even after changing the SVG height — you could position the original <text>
 element as an offset from 50%:

<text

 x=

"5mm"

 y=

"50%"

 dy=

"0.85cm"

>

One,
    <tspan

 class=

"em"

 dy=

"-0.5cm"

>

Two,</tspan>

    <tspan

 class=

"strong em"

 dy=

"-0.5cm"

>

Three!</tspan>

</text>


In contrast, if you wanted the text to maintain its position relative to the *bottom*
 of the SVG (100% height), you would use the following:

<text

 x=

"5mm"

 y=

"100%"

 dy=

"-0.4cm"

>

One,
    <tspan

 class=

"em"

 dy=

"-0.5cm"

>

Two,</tspan>

    <tspan

 class=

"strong em"

 dy=

"-0.5cm"

>

Three!</tspan>

</text>



 Figure 4-2
 shows the difference by embedding both versions of the SVG in an HTML page with <object>
 elements. Width and height set on the <object>
 with CSS are propagated to the SVG, overriding the dimensions set in the SVG file:

object

 {

    display

:

 table-cell

;

    width

:

 9.5cm

;

    height

:

 2.5cm

;

    margin

:

 0.25cm

;

}

.stretch

 object

 {

    height

:

 7.5cm

;

}

.squish

 object

 {

    height

:

 1.5cm

;

}



Because the SVG files (which are identical to Example 4-1
 except for the text positioning attributes) do not have a viewBox
 attribute, the content does not scale. The percentages are calculated relative to the applied height and width.

In both cases, the adjustments on the <tspan>
 elements do not change. The effect of a dy
 attribute is always calculated from the final position of the previous text, regardless of whether that position was set with y
 , dy
 , or a combination of the two.
Tip
SVG 2 is expected to adopt the CSS calc()
 function, which will make percentage-plus-offset adjustments possible within any length attribute.

The preceding examples have all used the natural flow of the text to control horizontal position. However, for many uses of SVG text, you’ll need to reset the horizontal position with x
 and dx
 attributes. A dx
 value is also calculated from the final net position of previous text, including (for horizontal text) the offsets caused by the letters themselves. An x
 attribute establishes a new horizontal starting point, regardless of the previous text position.
Waxing Poetic
Using an absolute x
 attribute and a relative dy
 , you can create a line break. The x
 value is usually set to the same value for each line; it resets the horizontal flow of the text, like a carriage return on an old typewriter. The dy
 value is equivalent to the desired line height; it shifts the text down like a typewriter’s line feed motion.

Example 4-2
 uses x
 and dy
 to position the lines of a poem (Alice’s muddled morality lesson, “How doth the little crocodile,” from Lewis Carroll’s *Alice in Wonderland*
 ). Each verse is a separate <text>
 element containing four <tspan>
 elements for each line. Every second line is inset and styled differently. Figure 4-3
 displays the typeset result.

Example 4-2.
 Typesetting poetry using x
 , dy
 , and dx
 on SVG tspan elements
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

xml:lang=

"en-GB"

 

width=

"4.3in"

 

height=

"3in"

>

    

<title

>

How Doth the Little Crocodile - Lewis Carroll

</title>

    

<desc

>

From Alice in Wonderland

</desc>

    

<style

>



@import

  

url(http://fonts.googleapis.com/css?family=Miltonian+Tattoo)

;

 





        

svg

 

{

            

font-family

:

 

"Miltonian Tattoo"

,

 

serif

;

            

font-size

:

 

18pt

;

        

}

        

.verse

 

{

            

fill

:

 

darkGreen

;

            

stroke

:

 

#031

;

            

word-spacing

:

 

2px

;

                             





        

}

        

.verse

 

>

 

tspan

:nth-child

(

2n

)

 

{

                     





            

fill

:

 

navy

;

            

stroke

:

 

#013

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

 

class=

"verse"

>

                                   





        

<tspan

 

dy=

"1.2em"

 

x=

"10"

               

>

How doth the little crocodile

</tspan>

      





        

<tspan

 

dy=

"1.2em"

 

x=

"10"

 

dx=

"1em"

               

>

Improve his shining tail,

</tspan>

          





        

<tspan

 

dy=

"1.2em"

 

x=

"10"

               

>

And pour the waters of the Nile

</tspan>

        

<tspan

 

dy=

"1.2em"

 

x=

"10"

 

dx=

"1em"

               

>

On every golden scale!

</tspan>

      

</text>

    

<text

 

class=

"verse"

 

y=

"50%"

>

                           





        

<tspan

 

dy=

"1.2em"

 

x=

"10"

               

>

How cheerfully he seems to grin,

</tspan>

        

<tspan

 

dy=

"1.2em"

 

x=

"10"

 

dx=

"1em"

               

>

How neatly spreads his claws,

</tspan>

        

<tspan

 

dy=

"1.2em"

 

x=

"10"

               

>

And welcomes little fishes in

</tspan>

        

<tspan

 

dy=

"1.2em"

 

x=

"10"

 

dx=

"1em"

               

>

With gently smiling jaws!

</tspan>

    

</text>



</svg>

The SVG uses a decorative web font, Miltonian Tattoo by Pablo Impallari, accessed from Google Font’s repository of free typefaces.
 We’ll discuss more about web fonts in Chapter 10
 .

The letters are stroked, which can make them seem overly close together. A small amount of extra word-spacing
 helps maintain legibility.

An nth-child(2n)
 selector styles every other line differently, in blue instead of green.

There are no positioning attributes on the first <text>
 element: both x
 and y
 default to 0.

Each line (<tspan>
 ) resets the x
 position to a left margin of 10px. Each line also shifts the vertical position (dy
 ) by 1.2em. For the first line, this offset is measured from the *y*
 =0 position set by default on the <text>
 .

Every other line has a dx
 attribute, which adds an inset from the margin created by the x="10"
 attribute. Although you could combine the x
 and dx
 values, keeping them separate helps clarify the differing purposes, and also allows you to use different units for each.

The second verse follows the same structure, except that it starts from a *y*
 position of 50%, halfway down the graphic.

All these attributes might seem a little excessive to create simple line breaks. In a way, it is. SVG was not designed to set paragraphs of text. However, it can be used to set complicated text layouts where the exact position of each text element is important.

Example 4-3
 sets another poem from *Alice in Wonderland*
 : “The Mouse’s Tale.” In the book, Alice mishears the title as “The Mouse’s Tail,” and therefore imagines the words arranged in the shape of a long, curvy appendage that gets narrower toward the tip. This is known as a *concrete poem*
 , where the artistry of the text has as much to do with the way it is printed as the words themselves. Figure 4-4
 shows the final typeset text.

Example 4-3.
 Typesetting concrete poetry
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

xml:lang=

"en-GB"

 

width=

"100%"

 

height=

"47.5em"

>

    

<title

>

The Mouse's Tale - Lewis Carroll

</title>

    

<desc

>

From Alice in Wonderland

</desc>

    

<style

>

        

svg

 

{

            

font-family

:

 

serif

;

            

font-size

:

 

medium

;

        

}

        

text

 

{

            

font-size

:

 

150%

;

           





        

}

        

.em

 

{

            

font-style

:

 

italic

;

        





        

}

        

.smaller

 

{

            

font-size

:

 

85%

;

            





        

}

    

</style>

    

<text

>

                             





      

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-2.68em"

>

Fury said to

</tspan>

 





      

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.65em"

>

a mouse, That

</tspan>

      

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.03em"

>

he met in the

</tspan>

      

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-0.62em"

>

house, “Let

</tspan>

  





      

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.03em"

>

us both go

</tspan>

      

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.44em"

>

to law:
                

<tspan

 

class=

"em"

>

I

</tspan>

</tspan>

             





      

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-2.06em"

>

will prose-

</tspan>

      

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-2.06em"

>

cute
                

<tspan

 

class=

"em"

>

you.

</tspan>

 — 

</tspan>

      

<tspan

 

class=

"smaller"

>

                                   





        

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-2.19em"

>

Come, I’ll

</tspan>

        

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.7em"

>

take no de-

</tspan>

        

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-0.73em"

>

nial; We 

</tspan>

        

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-0.24em"

>

must have

</tspan>

        

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-0em"

>

the trial:

</tspan>

        

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-0.49em"

>

For really

</tspan>

        

<tspan

 

class=

"smaller"

>

                                 





          

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.14em"

>

this morn-

</tspan>

          

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.43em"

>

ing I’ve

</tspan>

          

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-2em"

>

nothing

</tspan>

          

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-2.57em"

>

to do.”

</tspan>

          

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-3.14em"

>

Said the

</tspan>

          

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-3.71em"

>

mouse to

</tspan>

          

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-4em"

>

the cur,

</tspan>

          

<tspan

 

class=

"smaller"

>

            

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-5.04em"

>

“Such a

</tspan>

            

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-4.7em"

>

trial, dear

</tspan>

            

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-4.03em"

>

Sir, With

</tspan>

            

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-3.36em"

>

no jury

</tspan>

            

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-2.69em"

>

or judge

</tspan>

            

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-2.02em"

>

would

</tspan>

            

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.34em"

>

be wast-

</tspan>

            

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.01em"

>

ing our

</tspan>

            

<tspan

 

class=

"smaller"

>

              

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-0.4em"

>

breath.”

</tspan>

              

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-0em"

>

“I’ll be

</tspan>

              

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"0.79em"

>

judge,

</tspan>

              

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"0.79em"

>

I’ll be

</tspan>

              

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"0.4em"

>

jury,”

</tspan>

              

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-0em"

>

said

</tspan>

              

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-0.79em"

>

cun-

</tspan>

              

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.19em"

>

ning

</tspan>

              

<tspan

 

class=

"smaller"

>

                

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-1.86em"

>

old

</tspan>

                

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-2.79em"

>

Fury:

</tspan>

                

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-3.26em"

>

“I’ll

</tspan>

                

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-3.72em"

>

try

</tspan>

                

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-4.19em"

>

the

</tspan>

                

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-4.19em"

>

whole

</tspan>

                

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-3.72em"

>

cause

</tspan>

                

<tspan

 

class=

"smaller"

>

                  

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-3.29em"

>

and

</tspan>

                  

<tspan

 

dy=

"1em"

 

x=

"50%"

 

dx=

"-2.19em"

                                    

>

condemn

</tspan>

            





                  

<tspan

 

dy=

"-0.7em"

>

you to

</tspan>

                  

<tspan

 

dy=

"-0.7em"

>

death!”

</tspan>

                

</tspan>

              

</tspan>

            

</tspan>

          

</tspan>

        

</tspan>

                                 





      

</tspan>

    

</text>



</svg>

The text will start out at a font size that is half-again as large as the user’s normal text. The medium
 font size is explicitly set on the <svg>
 itself to circumvent the font-size
 error in embedded SVG images in WebKit/Blink browsers.

Because SVG does not have an <em>
 element that would by default italicize text, emphasized words within the poem are styled using a special em
 class.

As the poem progresses, the font will get smaller and smaller, each time set to 85% of the previous value.

Again, the main <text>
 element uses the default position values; the horizontal position will be reset for each line, while the vertical position of the first line is calculated as a one-line offset from the top of the graphic.

Each line is shifted down with dy="1em"
 and is reset horizontally with x="50%"
 . The actual shape of the text is controlled by the dx
 offsets, which were generated by hand — viewing the file and then adjusting the attributes — to create the desired shape.

Unicode characters are used for typographically correct quotation marks and dashes; alternatively, numeric entities could have been used. Since this is an SVG file, HTML named entities such as &rdquo;
 or &mdash;
 are not available.

The italicized spans are simply more <tspan>
 elements, but without any positioning attributes.

The font-size
 change is also created by a <tspan>
 without positioning attributes, but in this case it *contains*
 all the remaining lines of the poem.

Subsequent smaller
 sections are nested inside the previous, so that the percentage reductions in font size accumulate.

The final curl of the tail is created by *not*
 resetting the horizontal position between spans, and by reversing the direction of the vertical dy
 adjustment.

At the end of it all, be sure to close up all your <tspan>
 elements, otherwise all you’ll see are XML validation errors!

The use of both x
 and dx
 on each line of text may seem excessive, but it allows the entire SVG to adjust to fit any width page and remain centered. Similarly, the use of em
 units for the height ensures that it will adjust vertically even if the user needs to increase the base font size in order to read the tiny text at the end.