Chapter 9.
 Beyond Straight Lines
Baselines ensure that glyphs are positioned to create a pleasing line of text. However, we’ve already said that, in graphical layout, you don’t always *want*
 text to display in perfectly straight lines. Sometimes it’s fun to make text move out of those boring lines and into more complex curves — circles, spirals, around the edges of various objects, and so forth.

This chapter introduces the <textPath>
 element, which allows you to use SVG path geometry to describe complex text layouts.
Creating Curved Text
We’ve shown (in Chapter 5
 ) how you can position and rotate individual characters. For some simple designs, that’s enough. Example 9-1
 spaces the letters of the word “Sunrise” in a semicircle, each letter positioned every 30° and rotated to match, as shown in Figure 9-1
 .

Example 9-1.
 Arranging letters around a shape with character position attributes
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

xmlns:xlink=

"http://www.w3.org/1999/xlink"

     

xml:lang=

"en"

     

width=

"4in"

 

height=

"2.3in"

 

viewBox=

"0 0 400 230"

>

    

<title

>

Text Positioned in a Curve

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

 

italic

 

48px

 

"Times New Roman"

,

 

Times

,

 

serif

;

            

fill

:

 

gold

;

            

stroke

:

 

orangeRed

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

    

<g

 

transform=

"translate(200,180)"

>

                





        

<text

 

x=

"-150 -130 -75 0 75 130 150"

              

y=

"0 -75 -130 -150 -130 -75 0"

              

rotate=

"-90 -60 -30 0 30 60 90"

              

>

Sunrise

</text>

                         





    

</g>



</svg>

To make the trigonometry *slightly*
 easier, the coordinate system origin is translated to the center of the semicircle.

There are seven letters in “Sunrise,” so there are seven values in each of the positioning attribute lists.

The exact positions of each letter required a little bit of trigonometry to calculate. Even then, it doesn’t look quite right: because the letters *start*
 at the specified anchor point, the final “e” sticks out below the starting “S” even though their anchors are on the same horizontal line. Using text-anchor: middle
 doesn’t help; it centers each letter over the anchor point *before*
 rotating them, so they end up shifted to the side, not shifted around the circle.

If we had more letters, we’d need more trigonometry, and longer lists of x
 , y
 , and rotate
 attributes. And if we had enough letters that we wanted the word to look like a continuous string of text, we’d have to deal with the fact that each letter should be spaced differently according to its own unique dimensions. This clearly isn’t a practical solution for pleasing text layout.

For cursive scripts such as Arabic, there’s another problem with absolutely positioning letters: no matter how close each letter is to the next, they are rendered as isolated letters, not parts of a continuous word.

This is where text on a path comes in handy. Text on a path is exactly what it says — each letter is placed such that its baseline is on the tangent of a curve, spaced out along that curve according to the normal spacing of that text sequence.
Warning
Although text on a path *should*
 be perfect for creating decorative layouts with cursive text, actual browser implementations are another matter, particularly for right-to-left scripts such as Arabic.

SVG text on a path is created with the <textPath>
 element. The content
 of the <textPath>
 is aligned along the outline of a separate <path>
 element. 
 Which path to use is specified with an xlink:href
 attribute.
Tip
Just like a <tspan>
 , a <textPath>
 *must*
 be within a <text>
 element; it does not draw anything on its own.

Example 9-2
 arranges the longer string “from Sunrise to Sunset” around a semicircular path (actually a cubic Bézier curve). The result is shown in Figure 9-2
 .

Example 9-2.
 Arranging a text string around a shape with textPath
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xmlns:xlink=

"http://www.w3.org/1999/xlink"

     xml:lang=

"en"

     width=

"4in"

 height=

"2.3in"

 viewBox=

"0 0 400 230"

>

    <title>

Text on a Curved Path</title>

    <style 

type=

"text/css"

>

        text

 {

            font

:

 bold

 italic

 48px

 "Times New Roman"

,

 Times

,

 serif

;

            fill

:

 gold

;

            stroke

:

 orangeRed

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

    <path

 id=

"path"

 d=

"M50,200 C50,0 350,0 350,200"

          fill=

"none"

 stroke=

"darkOrange"

 />

    <text>

        <textPath

 xlink:href=

"#path"

>

from Sunrise
            to Sunset</textPath>

    </text>

</svg>

The letters in Figure 9-2
 are spaced much more smoothly than you could expect to achieve by placing each character yourself.
Warning
Blink/WebKit browsers currently render each letter within text on a path as if it was its own text chunk. This doesn’t affect the spacing, but it has other consequences. For right-to-left scripts within a left-to-right layout, this means that the letters are not rearranged into the correct reading order. In cursive scripts, it also means that the isolated glyph forms for each letter are used instead of the word forms.

The path itself is stroked in Example 9-2
 , but it does not have to be; you can define the path within a <defs>
 section without drawing it at all. Here, we draw it to emphasize that the baseline of the text is aligned with the path. If you used a different baseline, the characters would move in or out relative to the curve.

Regardless of whether the path itself was drawn to the screen or not, the text will be positioned as if the path was drawn in the same coordinate system as the <text>
 element itself.
Positioning on a Path
The text string in Example 9-2
 doesn’t quite fit the full length of the path, making it appear slightly off-balance. A text-anchor: middle
 setting could center the text, but only if we can correctly position the anchor point. By default, it’s at the start of the path. If we centered the text around that point, half of it would disappear off the start of the path.
Tip
Any text that extends beyond the length of the path — at the beginning or end — will not be drawn at all.

The startOffset
 attribute of a <textPath>
 element defines the position along the path at which the text should be anchored. It can be given as a length — measured from the normal start of the path — or as a percentage of the path’s length. To center text within the path length, you can therefore use text-anchor: middle
 with a startOffset
 of 50%, as follows:

<text

 text-anchor=

"middle"

>

    <textPath

 xlink:href=

"#path"

 startOffset=

"50%"

        >

from Sunrise to Sunset</textPath>

</text>



Figure 9-3
 shows the much more balanced result of this change.

The startOffset
 attribute is particularly useful when arranging text around an existing shape, which might not have been drawn with the start of the text in mind. When animated, a changing startOffset
 can create a marquee effect, sliding text the length of the path.
Warning
Blink/WebKit browsers currently treat negative startOffset
 values as 0, and do not draw any text with a startOffset
 greater than 100% of the path length, even when using an end
 value for text-anchor
 . This rather limits the potential for scrolling marquees of appearing and disappearing text.

On closed shapes (such as a complete circle), be aware that text will not continue from the end of the path back to the beginning. To position text across this seam in the path definition, create a version of the path with the entire path string duplicated. Then reduce any percentage values for startOffset
 by half, to account for the fact that the path is now twice as long.
1

The SVG 1.1 specifications did not define how startOffset
 — or text paths in general — would work for right-to-left text direction. 
 Firefox is currently the only browser that provides legible results with correct bidirectional ordering.
Warning
Internet Explorer and Blink/WebKit browsers do not correctly process right-to-left and bidirectional text on a path. For cursive scripts (such as Arabic) Blink/WebKit also use the isolated glyph versions of each character.

Even for Firefox (version 40) the startOffset
 value is used as an *end*
 -offset position when text-anchor
 is start
 and the reverse for a text-anchor
 of end
 . This ensures that text is still visible with the default offset value of 0, but it is otherwise un-intuitive and inconsistent with the rest of SVG text layout.

Figure 9-4
 shows how Arabic text on a path could look (screenshot from Firefox 40). The code is given in Example 9-3
 .

Example 9-3.
 Displaying right-to-left cursive text within textPath
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xmlns:xlink=

"http://www.w3.org/1999/xlink"

     width=

"4in"

 height=

"2.3in"

 viewBox=

"0 0 400 230"

>

    <title

 xml:lang=

"en"

>

Arabic Text on a Curved Path</title>

    <style 

type=

"text/css"

>

        text

 {

            font

:

 bold

 italic

 48px

 "Times New Roman"

,

 Times

,

 serif

;

            fill

:

 gold

;

            stroke

:

 orangeRed

;

        }

        @supports

 (

filter

:

 drop-shadow

(

0

 0

 0

))

{

            text

 {

                stroke

:

 none

;

                filter

:

 drop

-

shadow

(

orangeRed

 0.5px

 1px

 1px

);

            }

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

    <path

 id=

"path"

 d=

"M50,200 C50,0 350,0 350,200"

          fill=

"none"

 stroke=

"darkOrange"

 />

    <text

 text-anchor=

"middle"

 dir=

"rtl"

 xml:lang=

"ar"

>

        <textPath

 xlink:href=

"#path"

 startOffset=

"50%"

            >

جميل الخط النسخية على منحنى</textPath>

    </text>

</svg>

Although mostly similar to the centered version of Example 9-2
 , Example 9-3
 uses a drop-shadow filter instead of a stroke to avoid interrupting the cursive connections. 
 
 
 
 When letters are stroked, Firefox currently does not apply ligatures; even if it did, the strokes would include the edges between each glyph. A text shadow is not an improvement; it is also painted one glyph at a time, and so also overlaps the cursive connections. 
 In contrast, the drop-shadow filter (introduced in the CSS Filters module, although it could also be created with SVG filter elements) is applied to the final shaped text, after combining all the glyphs into a single layer.

By comparison, Figure 9-5
 shows the result with stroked text (also in Firefox 40). Although the connections between letters are much more awkward, this is still much closer to proper Arabic typography than any of the other web browsers are able to render at the time of writing.

The SVG 2 specifications will include new rules for how text on a path *should*
 behave. At the time of writing, they have not been finalized. One option would be to replace the default startOffset
 with an auto
 value that adapts according to the text direction.
Integrating Other Text Effects
Most of the other text features we have discussed so far can be used with text on a path, some with more success than others.

The text within a <textPath>
 element can have <tspan>
 sections that change the styling. Example 9-4
 adds stroke and fill changes for the keywords in the string, as displayed in Figure 9-6
 .

Example 9-4.
 Styling text spans within textPath
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xmlns:xlink=

"http://www.w3.org/1999/xlink"

     xml:lang=

"en"

     width=

"4in"

 height=

"2.3in"

 viewBox=

"0 0 400 230"

>

    <title>

Styled Text on a Curved Path</title>

    <style 

type=

"text/css"

>

        text

 {

            font

:

 bold

 italic

 48px

 "Times New Roman"

,

 Times

,

 serif

;

            fill

:

 orangeRed

;

        }

        .bright

 {

            fill

:

 gold

;

            stroke

:

 orangeRed

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

    <path

 id=

"path"

 d=

"M50,200 C50,0 350,0 350,200"

          fill=

"none"

 stroke=

"darkOrange"

 />

    <text

 text-anchor=

"middle"

>

        <textPath

 xlink:href=

"#path"

 startOffset=

"50%"

            >

from
            <tspan

 class=

"bright"

>

Sunrise</tspan>

            to
            <tspan

 class=

"bright"

>

Sunset</tspan

                ></textPath>

    </text>

</svg>

The styles on the <tspan>
 elements are applied with the bright
 class, to override the styles set on the <text>
 as a whole.
Warning
Avoid setting styles using a CSS selector for the textPath
 tag name. Older Blink and WebKit browsers do not correctly match mixed-case tag names for SVG elements (case sensitive) that are inline in HTML 5 documents (case insensitive). The latest versions of both platforms have workarounds for this problem.

If you can use <tspan>
 within <textPath>
 , can you use the normal text path positioning attributes? You can, but they don’t have the normal effect. Instead of moving text horizontally or vertically, they move them along the path or perpendicular to the path.
Tip
Although the x
 , y
 , dx
 , dy
 , and rotate
 positioning attributes on <text>
 and <tspan>
 *affect*
 the characters within a <textPath>
 , SVG 1.1 does not allow you to use these attributes directly on the <textPath>
 element.

For horizontal writing mode, therefore, a positive dx
 attribute moves characters futher along the path. A positive dy
 value shifts the text toward the inside of the path, while a negative dy
 value shifts it outward.
Warning
Internet Explorer does not render any of the text if you use dx
 or dy
 in combination with text-anchor: middle
 ; it renders these offsets correctly for text-anchor: start
 .

The other browsers have no problem with relative position attributes, but every browser tested was inconsistent and buggy when absolute positioning (x
 and y
 attributes) was used.

For vertical writing-mode
 , *y*
 -offsets move the text along the path and *x*
 -offsets move it perpendicularly. For right-to-left character sequences — whether or not they are embedded in a left-to-right layout — you would need to use negative dx
 values to add space between characters, the same as for normal SVG text.
Warning
Browsers are currently very inconsistent about how <textPath>
 contents are laid out when the layout direction (i.e., direction
 property) is right to left. Unfortunately, the SVG 1.1 specifications did not discuss this situation carefully.

Example 9-5
 uses dx
 and dy
 on both the <text>
 element that contains the <textPath>
 and the <tspan>
 elements within it. The resulting layout is shown in Figure 9-7
 .

Example 9-5.
 Using relative positioning attributes within textPath
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

xmlns:xlink=

"http://www.w3.org/1999/xlink"

     

width=

"4in"

 

height=

"2.3in"

 

viewBox=

"0 0 400 230"

>

    

<title

>

Text Offset from a Curved Path

</title>

    

<style 

type=

"text/css"

>

        

/* omitted to save space */

                        





    

</style>

    

<rect

 

fill=

"#CEE"

 

width=

"100%"

 

height=

"100%"

 

/>

    

<path

 

id=

"path"

 

d=

"M50,200 C50,0 350,0 350,200"

          

fill=

"none"

 

stroke=

"darkOrange"

 

/>

    

<text

 

dy=

"0.5ex"

 

text-anchor=

"middle"

>

                 





        

<textPath

 

xlink:href=

"#path"

 

startOffset=

"50%"

            

>

from
            

<tspan

 

class=

"bright"

 

dy=

"-1ex"

 

dx=

"10px"

            

>

Sunrise

</tspan>

                               





            

<tspan

 

dy=

"+1ex"

>

to

</tspan>

                    





            

<tspan

 

class=

"bright"

 

dy=

"+1ex"

 

dx=

"10px"

            

>

Sunset

</tspan>

</textPath>

                     





    

</text>



</svg>

The styles would be the same as for Example 9-4
 .

A dy
 attribute on the <text>
 element applies to the first character on the path. It shifts the first chunk of text on the path down by 0.5ex, so the lowercase letters half-overlap the path. Using a middle
 baseline would have had much the same effect, if it could be relied upon for consistent browser support.

The “Sunrise” span starts with some extra spacing (dx
 ), and is shifted outward (dy
 ) by the full ex-height, so it ends up 0.5ex beyond the path.

A span around the word “to” is used to cancel out the dy
 value and reset the baseline; if baseline-shift
 had better browser support, it could have been used on “Sunrise” instead, and this extra span would not be required.

The “Sunset” span also starts with a dx
 offset, but its dy
 value shifts it down, into the interior of the path.

In Figure 9-7
 , you’ll notice that the letters in “Sunrise” are spaced farther apart than usual, while the letters in “Sunset” look rather cramped. This is because each letter is shifted perpendicular to its particular point on the path. On a curved path, those different perpendicular
 lines spread out on one side and come together on the other.
Tip
For tightly curved paths, the cramped or stretched effect can be visible even without a perpendicular shift. Convex curves, like this, will space out letters above the baseline and compress them below the baseline. Concave curves will do the opposite. As you can guess, the choice of baseline will also affect whether or not the letters end up uncomfortably spaced.

The SVG specifications include two other attributes to control text path layout, neither of which currently have an effect in browsers:

spacing

How the glyphs should be positioned along the path. The default value supported in browsers is exact
 : each glyph takes up the same space on the path as it would in a straight line of text. The alternative value, auto
 , would allow the SVG rendering agent to adjust the spacing “in order to achieve visually appealing results,” although what that means is not defined.

method

How the text string should be bent to fit along the path. The default value supported in browsers is align
 (each glyph is aligned with the path without distorting it); the alternative, unsupported value is stretch
 (the tops and bottoms of each glyph are stretched or condensed to fill the available space).

The lack of support for the stretch
 method is particularly problematic with cursive scripts and fonts, whose glyphs may no longer overlap each other correctly when each character has a different rotation. In the Arabic text from Example 9-3
 , this is visible as cracks between adjacent glyphs, as shown in Figure 9-8
 (a zoomed-in view of Figure 9-4
 ). Nonetheless, the lack of support is perhaps not surprising, considering that there is no support anywhere else in SVG for a stretch-type distortion effect (technically called a non-affine transformation).

As mentioned briefly earlier, you can also use the x
 and y
 attributes to set absolute positioning within text on a path. These are only supposed to have an effect in the direction of the text, creating a new start offset; absolute positions perpendicular to the path would be ignored. In other words, for horizontal text on a path, the x
 attribute could be used to reposition the offset along the path. In combination with a dy
 attribute, this could (theoretically) be used to create multiline text above and below a path.

The specifications were short on details of how this would work, and browser implementations are correspondingly inconsistent. If you want to create multiline text on a path, use two <textPath>
 elements referencing the same <path>
 shape, with different dy
 offsets for each. Example 9-6
 uses this approach to create the multiline text shown in Figure 9-9
 .

Example 9-6.
 Using multiple textPath elements to create multiline text on a path
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xmlns:xlink=

"http://www.w3.org/1999/xlink"

     xml:lang=

"en"

     width=

"4in"

 height=

"2.3in"

 viewBox=

"0 0 400 230"

>

    <title>

Multiline Text on a Curved Path</title>

    <style 

type=

"text/css"

>

        text

 {

            font

:

 bold

 italic

 48px

 "Times New Roman"

,

 Times

,

 serif

;

            fill

:

 gold

;

            stroke

:

 orangeRed

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

    <path

 id=

"path"

 d=

"M50,200 C50,0 350,0 350,200"

          fill=

"none"

 stroke=

"darkOrange"

 />

    <text

 text-anchor=

"middle"

>

        <textPath

 xlink:href=

"#path"

 startOffset=

"50%"

            letter-spacing=

"-2.5px"

><tspan

 dy=

"-0.2em"

            >

Text above a path</tspan></textPath>

        <textPath

 xlink:href=

"#path"

 startOffset=

"50%"

            letter-spacing=

"5px"

><tspan

 dy=

"0.8em"

            >

and below it, too!</tspan></textPath>

    </text>

</svg>

The <textPath>
 elements in Example 9-6
 use letter-spacing
 to adjust for the expansion and compression caused by the vertical offsets from the curved path. As mentioned previously, letter-spacing
 is not currently supported for SVG text in Firefox; the screenshot is from Chrome version 44.

1

 Thanks to Israel Eisenberg for the doubled-path solution for text on a closed shape.