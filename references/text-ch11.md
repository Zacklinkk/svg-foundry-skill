Chapter 8.
 Lining Up
Text layout, as we have seen, involves both vertical and horizontal positioning. It only stands to reason that there should be control over both vertical and horizontal alignment.

The text-anchor
 property controls how a string of characters (text chunk) is aligned around the anchor point, *in the direction of text flow*
 (the “inline progression direction” in the specifications). In other words, it sets horizontal alignment for horizontal text and vertical alignment for vertical text. However, you often also want to control how the text is aligned in the perpendicular direction — for horizontal text, you want to control the vertical alignment.

This is especially true in graphical layouts. In flow diagrams or system architecture diagrams, one of the more complex tasks in putting things together is accurately centering lines of text. Not only do you have to center the content horizontally, but also vertically. Setting a y="50%"
 value is not sufficient, as it makes the text appear top-heavy: in most scripts, the part above the baseline is much larger than the part below.

It isn’t only a problem in technical drawings. The quintessential interface device, the button, usually consists of text within a shape. Centering text within such buttons can be difficult at the best of times.

Again, the matter of alignment is complicated by the many different writing systems in use around the world — and around the World Wide Web. Latin letters (such as the ones you’re now reading) normally sit upon their baseline, with the occasional tail hanging down; very few letters extend the full height defined by the font-size
 . In contrast, most Chinese traditional characters fit in equal-sized boxes, and are aligned either along the base of that box or its center line. A few scripts are aligned along the top — or a line close to the top — with the majority of each character hanging down.
Baseline Basics
Baselines are important in SVG for two reasons. First and most importantly, the baseline is used to align the text with the anchor point. Second, baselines are used to align differently styled sections of a text string together. When you change scripts, fonts, or font sizes, the different baselines of the characters will be spaced differently; they cannot *all*
 line up with the equivalent positions on the characters in the rest of the text.

SVG relies on two style properties to control baseline alignment: dominant-baseline
 to set the main baseline used for aligning a text element on a page, and alignment-baseline
 to set the baseline for aligning nested text with the parent text element. Both properties have equivalents in the XSLFO formatting language for printed documents; they are now being integrated into general CSS layout through the CSS Inline Layout Module. Neither property is inherited by default, although they *should*
 affect nested content indirectly.
Warning
At the time of writing, Internet Explorer has not implemented any of the baseline alignment properties; all text uses the default alphabetic
 baseline. The other browsers’ implementations are incomplete and inconsistent:

- 
 Firefox does not recognize alignment-baseline
 at all; however, when dominant-baseline
 is set on a nested <tspan>
 , it is treated the way alignment-baseline
 should be: that baseline in the nested text is aligned with the equivalent point on the parent text.

- 
 
 WebKit/Blink browsers treat alignment-baseline
 as a synonym for dominant-baseline
 ; when either value is set on a nested <tspan>
 , that baseline in the nested text is aligned with the current *y*
 -position of the text (or *x*
 -position for vertical text).

With those warnings in mind, you probably *do not*
 want to use the baseline properties for centering a single span of consistently styled text in a button. In an ideal world, that would be a perfect use case for a central baseline, but right now it is not reliable. Instead, use the dy
 property with a value of between 0.5ex and 0.5em, depending on whether you’re using mostly lowercase letters, uppercase letters, or ideographic characters. This will shift the text down until it is approximately centered around your anchor point.

Where the baseline alignment properties are essential, however, is when you’re mixing text of different sizes. In that situation, improving the alignment for most users may be worth losing perfect alignment in Internet Explorer.

To ensure a consistent result in the other browsers, follow these guidelines until implementations improve:

- Use dominant-baseline
 , *not*
 alignment-baseline

- Set all <tspan>
 elements to dominant-baseline: inherit

The inherit
 value is essential for Blink/WebKit; without it, any <tspan>
 content will be reset to use the default baseline relative to the text-anchor point.

So what are your baseline options? There are eight possibilities in the SVG specifications:

alphabetic

Align along the bottom of most letters in Western scripts

ideographic

Align along the bottom of East Asian ideographic characters; for Western scripts, this usually means the bottom of the *descenders*
 (the tails that drop down below the alphabetic baseline in letters such as “g” and “y”)

middle

Align down the middle of lowercase letters in Western scripts, one-half the ex-height above the alphabetic baseline

central

Align halfway between the top and bottom of East Asian ideographic characters; in Western scripts, this is approximately halfway between the top of *ascenders*
 (the projections above the ex-height in lowercase letters such as “f”) and the bottom of descenders

hanging

Align along a baseline in the upper half of characters; the hanging baseline is used in some Indic scripts where most letters have a strong horizontal line at this point

mathematical

Align along a baseline that looks best for setting mathematical equations with fractions and other characters; when the dominant baseline is alphabetic, this will usually be halfway between the alphabetic baseline and the ascender height; it should match up with the lines in mathematical symbols such as +, −, and ÷

text-before-edge

Align along the top edge of all normal (unaccented) characters, or the right edge for vertical text

text-after-edge

Align along the bottom edge (or left edge for vertical text) of all normal characters; this is usually the same as ideographic

The exact positions of each baseline should ideally be defined in the font file. If they aren’t, the browser should make a best guess based on the properties that are specified in the font.
Warning
Firefox currently treats mathematical
 as a synonym for central
 and hanging
 as a synonym for text-before-edge
 , if the font does not specify these baselines explicity. Versions prior to 40 also treated middle
 equivalent to central
 .

In well-designed fonts, the baselines are given as a matrix of values. If the overall layout (dominant-baseline
 ) uses the ideographic baseline, the position of central and mathematical baselines (for alignment-baseline
 ) may be different than if the overall layout uses an alphabetic baseline. Similarly, when switching between different fonts, the font from the parent element should be used to determine the dominant baseline position, and then the baseline from the nested font should be aligned with it.
Warning
Versions of Blink browsers prior to Chromium 44 do not correctly handle baselines other than alphabetic
 when the browser window is zoomed to any value other than 100%. As mentioned in Chapter 7
 , the same problem shows up with vertical text.

Example 8-1
 demonstrates all the different baselines for text with a mixture of fonts, sizes, and scripts. It draws gray lines in the background to mark the position of the anchor point and therefore of the baseline. Figure 8-1
 shows the SVG as it appears in Chrome (version 44), including the subtle differences between middle
 , central
 , and mathematical
 .

Example 8-1.
 Using alignment baselines to lay out SVG text
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xmlns:xlink=

"http://www.w3.org/1999/xlink"

     width=

"415"

 height=

"250"

 viewBox=

"0 0 415 250"

>

    <title>

Text Baseline Comparison</title>

    <style>

        svg

 {

            text-anchor

:

 middle

;

            font-family

:

 Arial

,

 Helvetica

,

 sans-serif

;

            font-size

:

 12pt

;

        }

        line

 {

            stroke

:

 lightGray

;

            stroke-width

:

 2px

;

        }

        .small

 {

            font-size

:

 75%

;

            font-family

:

 Times

 New

 Roman

,

 Times

,

 serif

;

        }

        tspan

 {

            dominant

-

baseline

:

 inherit

;

        }

    </style>

    <defs>

        <line

 id=

"baseline"

 x1=

"-100"

 x2=

"100"

/>

        <line

 id=

"spine"

 y2=

"250"

 />

    </defs>

    <g

 transform=

"translate(105,0)"

>

        <use

 xlink:href=

"#spine"

/>

        <g

 transform=

"translate(0,50)"

>

            <use

 xlink:href=

"#baseline"

/>

            <text

 dominant-baseline=

"alphabetic"

>

'alphabetic'
                <tspan

 class=

"small"

>

alignment您好</tspan></text>

        </g>

        <g

 transform=

"translate(0,100)"

>

            <use

 xlink:href=

"#baseline"

/>

            <text

 dominant-baseline=

"middle"

>

'middle'
                <tspan

 class=

"small"

>

alignment您好</tspan></text>

        </g>

        <g

 transform=

"translate(0,150)"

>

            <use

 xlink:href=

"#baseline"

/>

            <text

 dominant-baseline=

"hanging"

>

'hanging'
                <tspan

 class=

"small"

>

alignment您好</tspan></text>

        </g>

        <g

 transform=

"translate(0,200)"

>

            <use

 xlink:href=

"#baseline"

/>

            <text

 dominant-baseline=

"text-before-edge"

                  >

'text-before-edge'
                <tspan

 class=

"small"

>

alignment您好</tspan></text>

        </g>

    </g>

    <g

 transform=

"translate(310,0)"

>

        <use

 xlink:href=

"#spine"

/>

        <g

 transform=

"translate(0,50)"

>

            <use

 xlink:href=

"#baseline"

/>

            <text

 dominant-baseline=

"ideographic"

>

'ideographic'
                <tspan

 class=

"small"

>

alignment您好</tspan></text>

        </g>

        <g

 transform=

"translate(0,100)"

>

            <use

 xlink:href=

"#baseline"

/>

            <text

 dominant-baseline=

"central"

>

'central'
                <tspan

 class=

"small"

>

alignment您好</tspan></text>

        </g>

        <g

 transform=

"translate(0,150)"

>

            <use

 xlink:href=

"#baseline"

/>

            <text

 dominant-baseline=

"mathematical"

>

'mathematical'
                <tspan

 class=

"small"

>

alignment您好</tspan></text>

        </g>

        <g

 transform=

"translate(0,200)"

>

            <use

 xlink:href=

"#baseline"

/>

            <text

 dominant-baseline=

"text-after-edge"

                  >

'text-after-edge'
                <tspan

 class=

"small"

>

alignment您好</tspan></text>

        </g>

    </g>

</svg>

The default dominant-baseline
 is alphabetic
 , which is what we have seen in all examples prior to Example 8-1
 .

The default for alignment-baseline
 was supposed to be to use the intrinsic baseline defined in the font file for each particular character; given the overall poor support for alignment-baseline
 , it’s not suprising that this doesn’t happen. 
 
 
 Firefox’s default behavior is similar to the expected behavior of alignment-baseline: baseline
 , which is to use the parent’s dominant baseline to align the nested text. The dominant-baseline: inherit
 rule creates the same behavior in WebKit/Blink.

Baselines apply to vertical text as well. 
 
 The default baseline for vertical text is central
 : WebKit and Blink browsers follow this, and center vertical text under (or over) the anchor point (as was shown in Figure 7-2
 , the vertical text example in Chapter 7
 ).
Warning
Internet Explorer (with no baseline support) always uses a rotated alphabetic baseline for vertical text, which means it lays out characters to the left of the anchor.

In a well-designed font that includes characters frequently used in vertical text, explicit baselines will be defined for vertical text alignment. However, the browser or other layout program *should*
 calculate approximations if the font data is not provided.
Super (and Sub) Baselines
Of course, sometimes you don’t want the baseline in smaller text to perfectly line up with the same point in larger text. For instance, you may want it to appear as a subscript or superscript. SVG uses the baseline-shift
 property to handle this situation.

The baseline-shift
 value can be specified as an absolute length, a percentage of the parent element’s line-height
 (which for SVG 1.1 is always the font-size
 ), or as one of the keywords sub
 or super
 . The property is inherited, and can be specified as a presentation attribute or CSS rule.
Warning
The baseline-shift
 property is not currently supported in either Firefox or Internet Explorer.

Although you can always shift an SVG <tspan>
 using the dy
 property, it is often difficult to shift *back*
 to the original baseline after the span is over, particularly when the spans have different font sizes. In contrast, a baseline-shift
 effect does not change the current text position for the rest of the text.
Tip
For baseline-shift
 on horizontal text, positive values shift the text *up*
 and negative values shift it down. For dy
 , the reverse is true, as the *y*
 -axis increases going down.

Example 8-2
 uses baseline-shift
 to position subscripts in a chemical formula as well as superscripts used to identify footnotes. It uses a mathematical
 baseline for the formula, which results in more pleasing subscripts; as shown in Figure 8-2
 , they are centered around the alphabetical baseline of the letters.

Example 8-2.
 Using baseline-shift to create subscript and superscript text
<?xml version="1.0" encoding="UTF-8"?>



<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

width=

"410px"

 

height=

"100px"

 

viewBox=

"0 0 410 100"

>

    

<title

>

Baseline Shift in SVG Text

</title>

    

<style 

type=

"text/css"

>

    

tspan

 

{

        

dominant

-

baseline

:

 

inherit

;

                        





    

}

    

.formula

 

{

        

font-size

:

 

22pt

;

        

text-anchor

:

 

middle

;

        

dominant

-

baseline

:

 

mathematical

;

    

}

    

.super

 

{

        

font-size

:

 

50%

;

        

baseline

-

shift

:

 

super

;

    

}

    

.sub

 

{

        

font-size

:

 

50%

;

        

baseline

-

shift

:

 

sub

;

    

}

    

.footnote

 

{

        

dominant

-

baseline

:

 

alphabetic

;

        

font-size

:

 

14pt

;

    

}

    

.footnote-divider

 

{

        

stroke

:

 

dimGray

;

        

stroke-width

:

 

2px

;

    

}

    

</style>

    

<rect

 

fill=

"lightYellow"

 

width=

"100%"

 

height=

"100%"

 

/>

    

<text

 

x=

"50%"

 

y=

"1em"

 

class=

"formula"

>

You

&#8217;

re as sweet 





        as C

<tspan

 

class=

"sub"

>

6

</tspan>

H

<tspan

        

class=

"sub"

>

12

</tspan>

O

<tspan

 

class=

"sub"

>

6

</tspan
        >

.

<tspan

 

class=

"super"

>

(1)

</tspan>

</text>

               





    

<g

 

transform=

"translate(10,70)"

>

        

<line

 

x2=

"40"

 

class=

"footnote-divider"

 

/>

        

<text

 

dy=

"1em"

 

class=

"footnote"

>

<tspan

            

class=

"super"

>

(1)

</tspan>

            Chemical signature of glucose sugar

</text>

    

</g>



</svg>

Because we’re changing the dominant-baseline
 on text elements, we need to be sure that the nested <tspan>
 elements will follow suit in WebKit/Blink browsers (which are the only ones that currently support baseline-shift
 ).

The &#8217;
 entity references the Unicode character for a curly apostrophe (right single quote). Because this is an SVG file, you cannot use the HTML &rsquo;
 entity.

To avoid introducing whitespace in the actual text content, the code is broken into lines *inside*
 the markup tags.

There are a couple of subtleties at play here. The first point worth considering is that, unlike the HTML <sub>
 and <super>
 elements, SVG will not automatically set font-size
 to a smaller size for text with super/sub baselines. As is typically the case, SVG gives you greater granular control over the visual appearance than HTML, but at the cost of requiring that you define each style change yourself.

Because the sub
 and super
 classes use percentages to scale down the font-size
 , nested elements can compound the effect. For instance, if one mathematical power was raised to another mathematical power, the next superscript up would be half again as small as the first, or 1
 /4
 of the original font size.
Mimicking Baseline Control
Given the poor support for baseline control in web browsers, how can you create SVGs for the Web with subscripts, superscripts, and other combinations of text of different sizes? Once again, you can replace the missing layout properties using SVG’s manual text positioning attributes. Once again, the resulting markup is not pretty.

The dy
 attribute shifts the baseline of (horizontal) text relative to the anchor point. As mentioned earlier in the chapter, this can be used to approximately center text around the anchor point. However, a dy
 offset affects all subsequent characters in the text. If you switch font sizes and want to re-center the alignment, you’ll need to first cancel out the adjustment from the previous font size. If you want a temporary baseline shift for a subscript or superscript, you’ll need to cancel it out at the end.

In order to correctly cancel out a text positioning attribute that uses font-relative units (em
 or ex
 ), you’ll need to apply the reverse dy
 value to the same element or an element with the same font. 
 
 Because dy
 values only have an effect when applied to a character, you may need to use the non-breaking space (&#160;
 ), zero-width space (&#8203;
 ), or zero-width non-joiner (&#8204;
 ) characters to provide a non-collapsible whitespace character or a character that does not affect layout at all.
Note
When SVG is inline in HTML, you can use the defined entities &nbsp;
 for the non-breaking space and &zwnj;
 for the zero-width non-joiner character. The numerical versions are required in pure SVG documents.

Example 8-3
 uses these techniques to create a cross-browser version of Example 8-2
 . The code makes extensive use of the zero-width non-joiner as a reset character. For the subscript numbers, the reset is contained within the same <tspan>
 and multiple values are given for dy
 . For the footnote superscript, an extra <tspan>
 is inserted (nested within the one that changes the font size) so that the dy
 attribute does not need to be padded with zeros for each superscript character. Figure 8-3
 shows the end result.

Example 8-3.
 Using dy and zero-width characters to simulate baseline properties
<?xml version="1.0" encoding="UTF-8"?>



<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

width=

"410px"

 

height=

"100px"

 

viewBox=

"0 0 410 100"

>

    

<title

>

Faking Baseline Shift in SVG Text

</title>

    

<style 

type=

"text/css"

>

    

.formula

 

{

        

font-size

:

 

22pt

;

        

text-anchor

:

 

middle

;

    

}

    

.super

,

 

.sub

 

{

        

font-size

:

 

50%

;

                





    

}

    

.footnote

 

{

        

font-size

:

 

14pt

;

    

}

    

.footnote-divider

 

{

        

stroke

:

 

dimGray

;

        

stroke-width

:

 

2px

;

    

}

    

</style>

    

<rect

 

fill=

"lightYellow"

 

width=

"100%"

 

height=

"100%"

 

/>

    

<text

 

x=

"50%"

 

y=

"1em"

 

dy=

"1ex"

 

class=

"formula"

>

You

&#8217;

re  





        as sweet as C

<tspan

 

class=

"sub"

 

dy=

"0.5ex -0.5ex"

        

>

6

&#8204;

</tspan>

H

<tspan

 

class=

"sub"

 

dy=

"0.5ex 0 -0.5ex"

        

>

12

&#8204;

</tspan>

O

<tspan

 

class=

"sub"

 

dy=

"0.5ex -0.5ex"

        

>

6

&#8204;

</tspan>

.

<tspan

 

class=

"super"

 

dy=

"-0.8em"

        

>

(1)

</tspan>

</text>

                                      





    

<g

 

transform=

"translate(10,70)"

>

        

<line

 

x2=

"40"

 

class=

"footnote-divider"

 

/>

        

<text

 

y=

"1em"

 

class=

"footnote"

>

                          





            

<tspan

 

class=

"super"

 

dy=

"-0.8em"

>

(1)

<tspan

 

dy=

"0.8em"

            

>

&#8204;

</tspan>

</tspan>

            Chemical signature of glucose sugar

</text>

    

</g>



</svg>

The styles have been simplified to remove the poorly supported baseline properties.

The mathematical baseline for the text as a whole is approximated with a single dy
 value.

The subscripts now have dy
 attributes and extra zero-width characters at the end of the text content. For the single-digit subscripts, the dy
 value consists of one downward (positive) shift followed by an equal and opposite shift for the zero-width character. The multidigit subscript “12” requires an extra dy
 value to maintain the shift for the second character. Finally, the superscript at the end of the line is shifted up with a negative dy
 value; because no characters follow it, the baseline is not reset.

In contrast, for the superscript in the footnote, a reset *is*
 required. An extra <tspan>
 is inserted with a single zero-width character to make the shift.

Again, the workaround creates an acceptable appearance, but requires confusing and difficult-to-maintain markup.