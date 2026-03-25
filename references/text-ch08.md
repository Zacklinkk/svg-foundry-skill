Chapter 5.
 Off-Kilter Characters
If you want full control over the layout of graphical text, why stop at positioning lines of text? Sometimes, for fun or fussy designs, you’ll want to position individual characters.

If you want to style individual letters with CSS, each one needs to be its own element. You might therefore expect that it would be necessary to wrap each character in its own <tspan>
 element in order to position individual letters in SVG. You *could*
 do this — but you don’t have to. The SVG text positioning options allow you to position individual characters within a larger text element.

You’re also not restricted to strict horizontal and vertical positions. This chapter introduces the rotate
 attribute for controlling the angle of individual characters.
Multiple Positions
All the text-positioning attributes (x
 , y
 , dx
 , and dy
 ) can take a list of values, which will get assigned character by character to the content.
Tip
As with most of SVG, the list of values can be either space-separated or comma-separated.

Any whitespace characters at the start of the text string are normally removed before assigning positions. Spaces in the middle of the text are collapsed, so that any amount of whitespace in the code counts as a single space character for positioning. These behaviors can be changed, as described in “Working with Whitespace”
 .

Example 5-1
 creates a simple example of this effect, using x
 and y
 lists to explicitly position the letters of the word “Wiggle” in an up-and-down wiggled layout. Figure 5-1
 shows the result.

Example 5-1.
 Positioning individual characters within SVG text
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xml:lang=

"en"

 width=

"4in"

 height=

"1in"

>

    <title>

Positioning Individual Characters</title>

    <style 

type=

"text/css"

>

        svg

 {

            font

:

 bold

 italic

 0.5in

 serif

;

            fill

:

 royalBlue

;

            stroke

:

 navy

;

        }

    </style>

    <rect

 fill=

"#CEE"

 stroke=

"none"

 width=

"100%"

 height=

"100%"

 />

    <text

 x=

"0     1.0in 1.6in 2.2in 3.0in 3.6in"

          y=

"0.8in 0.4in 0.8in 0.4in 0.8in 0.4in"

          >

Wiggle</text>

</svg>

Because there are six characters in “Wiggle” and six lengths given in both the x
 and y
 attribute lists, each character has its own explicit position. If there were more characters than values, the remaining characters would be positioned relative to the character before them using the normal text layout rules.
Tip
When the attribute only contains a single value —
 as in all the examples from Chapter 4
 — it is applied to the first character; the rest of the text string is automatically positioned relative to that point.

A similar-but-not-identical layout can be created by instead using a list of values for dx
 and dy
 :

<text

 x=

"0"

 y=

"0.8in"

      dx=

"0  0.5in 0.5in  0.5in 0.5in  0.5in"

      dy=

"0 -0.4in 0.4in -0.4in 0.4in -0.4in"

      >

Wiggle</text>


The results are not identical because the dx
 offsets are *added*
 to the offset created by the width of the letters themselves. The final position will depend on the font used. In contrast, absolute x
 values are not affected by the amount of space consumed by the text.

The positioning attributes are independent, and any or none can be specified as lists. If any character does not have positioning values assigned to it, it is positioned as normal beside the previous character in the string.

One use of dx
 is to mimic the effect of letter-spacing
 or word-spacing
 , which are not yet supported for SVG text in Firefox. These properties add a fixed amount of space after every letter or word, respectively; they are particularly useful in SVG to help space out text with thick outlines.

To replace letter-spacing
 , you need to create a dx
 attribute with the same value repeated for the length of the text. For word-spacing
 , you’ll need to mix the spacing value with zeros so that only whitespace characters are adjusted. Both the spacing properties and dx
 can also take negative values to compress the text.
Tip
If you’re using right-to-left text, you’ll have to use negative offset values for positive spacing and vice versa. Chapter 7
 discusses more issues with right-to-left text.

Positioning adjustments (and also any non-zero letter-spacing
 value) should normally turn off the use of optional ligatures (a single glyph to represent multiple adjacent characters) so that all letters are spaced evenly. However, some scripts or fonts have *required*
 ligatures — certain character sequences are always replaced by a combined glyph — or cursive connections between letters. Changing the spacing between letters in these cases is problematic. Some fonts and rendering software can stretch out cursive connections in a typographically acceptable manner, but others cannot.

Furthermore, many browsers do not correctly select the word-beginning, middle, and word-ending forms of a letter if the entire word is not positioned as a whole.

When using scripts or fonts that have cursive connections or different glyph forms for different positions in a word, be sure to test carefully before using character-by-character positioning.
Conflicting Positions
When you have <text>
 elements with nested <tspan>
 elements, you can specify the character positions on either the parent or child element. Positions specified on a parent element cascade through to characters inside nested <tspan>
 elements.

This means that is possible for two different values to be specified for the same character, if the same attribute is also set on the <tspan>
 . In this case, the value from the inner element replaces the inherited value for that character.

The character-by-character effect of positioning results in a non-intuitive result when using dx
 and dy
 on both <text>
 and <tspan>
 . If there are no characters within the <text>
 before the start of the <tspan>
 , the offset on the parent has no effect. The following two text elements result in the exact same position:

<text

 dx=

"50"

><tspan

 dx=

"30"

>

content</tspan></text>

<text><tspan

 dx=

"30"

>

content</tspan></text>


All of the dx
 values apply to the “c” in “content,” and the value on the <tspan>
 overrides the value on the <text>
 . Similarly, positioning values on a <tspan>
 never affect content not included in that span.
Tip
This means that attributes on an *empty*
 <tspan>
 (with no characters) have no effect on the rest of the text, even if they include absolute x
 or y
 positions.

Example 5-2
 presents a more complicated example of conflicting character position attributes, which combine to create Figure 5-2
 . The use of conflicting values isn’t recommended, if for no other reason
 than it makes your code confusing to read. But it’s included here in case it can help you debug a layout that isn’t working the way you intend!

Example 5-2.
 Positioning individual characters and spans within SVG text
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xml:lang=

"en"

 width=

"10cm"

 height=

"3.5cm"

>

    <title>

Character and tspan Position adjustments</title>

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

"3cm"

          dy=

"-30 30 -30 30 -30 30 -30 30 -30 30 -30 30 -30 30"

          >

One,
        <tspan

 class=

"em"

 dy=

"-15"

 dx=

"3mm"

>

Two,</tspan>

        <tspan

 class=

"strong em"

 dx=

"3mm"

               dy=

"-15 10 10 10 10"

               >

Three!</tspan>

    </text>

</svg>

To understand how the code in Example 5-2
 created the layout in Figure 5-2
 , it helps to write out the text and offsets, character by character:

text
dy
-30
30
-30
30
-30
30
-30
30
-30
30
-30
30
-30
30




char
O
n
e
,
_













tspan
dy





-15












char





T
w
o
,
_








tspan
dy










-15
10
10
10
10



char










T
h
r
e
e



final
dy
-30
30
-30
30
-30
-15
-30
30
-30
30
-15
10
10
10
10
0


char
O
n
e
,
_
T
w
o
,
_
T
h
r
e
e
!



The alternating up-and-down dy
 values set on the <text>
 apply to individual characters *including the whitespace*
 (shown as underlines in the table), except where the values are replaced by a value from a <tspan>
 . For the word “Two,” there is a single value, which replaces the offset on the “T,” but not the wiggle pattern on the rest of the word. For the word “Three!”, all the letters have explicit values from the <tspan>
 , so the values on the <text>
 are ignored. However, by the time you reach the exclamation mark (!), the list of values on both elements have been exhausted. It is therefore positioned directly after the previous letter with no offset.
Twisted Letters
If letter-by-letter positioning is not enough for you, you can also specify letter-by-letter rotations. The rotate
 attribute on <text>
 or <tspan>
 accepts a list of numbers that will be treated as rotational angles in degrees. Each individual character is rotated fron the baseline by that amount.
Tip
As usual in SVG, positive rotations are clockwise and negative rotations are counterclockwise.

The angles in rotate
 are not cumulative: each angle is measured relative to the baseline.

Just as an x
 , y
 , dx
 , or dy
 value affects all the letters that come after it until the position is reset, so a rotate
 value affects all the characters until reset. If you give a single value to rotate
 , it applies to all the characters in that element. *Unlike*
 the positioning values, however, the rotation setting persists even if an intervening <tspan>
 set a different rotational value for a section of the text.

Example 5-3
 uses the rotate
 attribute on both <text>
 and <tspan>
 elements, with the result shown in Figure 5-3
 .

Example 5-3.
 Rotating individual characters within SVG text
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xml:lang=

"en"

 width=

"4in"

 height=

"0.7in"

>

    <title>

Rotating Individual Characters</title>

    <style 

type=

"text/css"

>

        svg

 {

            font

:

 bold

 italic

 0.5in

 serif

;

            fill

:

 royalBlue

;

            stroke

:

 navy

;

        }

    </style>

    <rect

 fill=

"#CEE"

 stroke=

"none"

 width=

"100%"

 height=

"100%"

 />

    <text

 x=

"0.2in"

 y=

"0.5in"

 rotate=

"-45 0 15 -15 15"

          >

Jiggle
        <tspan

 rotate=

"-25"

>

giggle</tspan>

        jig!
    </text>

</svg>

An important thing to note in Figure 5-3
 is that the rotations do not affect
 the normal text offsets between characters along the baseline. As a result, the letters end up overlapping or spaced apart, depending on whether they are rotating closer to or farther away from each other.

The origin of rotation is the current *text position*
 point: the point created by the x
 , y
 , dx
 , and dy
 attributes as well as any offsets from previous letters. For the examples so far, that has been the point where the left side of the letter intersects the baseline of the text.
Tip
The origin of rotation will be affected by the text direction and the text-anchor
 property (which we’ll discuss in Chapter 6
 ).

So what is all this character-by-character layout used for? Not for setting lines of text, which are best left to the browser’s default typesetting algorithms. This is for text as a graphic, when you want as much control over the letters as over the rest of your image.

Example 5-4
 creates some comic-book exclamations using positioned text. The example also emphasizes the difference between a transform
 rotation — which changes the position of the baseline and the direction of *x*
 and *y*
 offsets — versus character-by-character rotations, which do not affect the axes. The result is shown in Figure 5-4
 .

Example 5-4.
 Using multiple attributes for precisely positioned text
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

xmlns:xlink=

"http://www.w3.org/1999/xlink"

     

xml:lang=

"en"

     

width=

"400px"

 

height=

"400px"

 

viewBox=

"0 0 400 400"

>

    

<title

>

Explosive Text

</title>

    

<style 

type=

"text/css"

>

        

.dynamic-lettering

 

{

            

font-family

:

 

"Gill Sans Ultra Bold"

,

                         

"Gill Sans"

,

 

"Gill Sans MT"

,

                         

"Showcard Gothic"

,

                         

"Cooper Black"

,

 

"Cooper"

,

                         

"Arial Black"

,

 

"Arial"

,

                         

"Impact"

,

  

sans-serif

;

                





            

font-size

:

 

100px

;

            

font-weight

:

 

900

;

                                  





            

fill

:

 

url(#yellow-red)

 

orangeRed

;

            

stroke

:

 

url(#red-brown)

 

red

;

            

stroke-width

:

 

3px

;

        

}

    

</style>

    

<defs

>

        

<linearGradient

 

id=

"yellow-red"

                        

gradientTransform=

"rotate(90)"

>

            

<stop

 

stop-color=

"yellow"

 

offset=

"0"

/>

            

<stop

 

stop-color=

"red"

 

offset=

"0.8"

/>

            

<stop

 

stop-color=

"maroon"

 

offset=

"1.0"

/>

           





        

</linearGradient>

        

<linearGradient

 

id=

"red-brown"

                        

gradientTransform=

"rotate(90)"

 

>

            

<stop

 

stop-color=

"orangeRed"

 

offset=

"0"

/>

            

<stop

 

stop-color=

"maroon"

 

offset=

"1.0"

/>

        

</linearGradient>

    

</defs>

    

<rect

 

height=

"100%"

 

width=

"100%"

 

fill=

"aquamarine"

/>

    

<g

 

class=

"dynamic-lettering"

>

        

<text

 

transform=

"translate(40,165) rotate(-15)"

              

dy=

"0 20 -30 20"

              

>

BAM!

</text>

                                     





        

<text

 

x=

"75"

 

y=

"375"

              

dx=

"0   -70  -30   8 -25"

              

dy=

"0   -70  -80  50  45"

          

rotate=

"-70 -15   35  10  30"

>

BOOM!

</text>

           





    

</g>



</svg>

A list of heavyweight, mostly sans-serif, fonts are provided as options.

If available, the heaviest bold variant of the font will be used.

The text is filled and stroked with linear gradients that are each oriented at a 90° rotation from default — in other words, from top to bottom.

The first text element (“BAM!”) is positioned and rotated using a transformation. A dy
 attribute then shifts the characters up and down, relative to the rotated baseline.

The second (“BOOM!”) text element is positioned using x
 and y
 , and then individual characters are rotated and offset vertically and horizontally using dx
 , dy
 , and rotate
 .

The character positions affect the gradients used in Example 5-4
 . In Chapter 3
 , we noted how gradients stretch to fit the bounding box of the entire <text>
 element, even if the gradient is only used on a <tspan>
 . That issue isn’t relevant here: there are two different <text>
 elements, so each one has its own gradient from gold to red. However, the bounding box *is*
 affected by the shifts in position of individual characters.

The SVG 1.1 specifications imply, but don’t explicitly say, that the bounding
 box of a <text>
 element should also include the individual character rotations applied with rotate
 . However, most browsers treat character rotations the same way they treat transformations: as a rotation of the entire coordinate system, including paint server content.
Warning
All browsers with the exception of Firefox will rotate gradient and pattern fills on individual rotated characters.

To demonstrate the difference between browsers, Figure 5-5
 repeats Figure 5-4
 , which is a screenshot from Firefox, comparing it side by side with the same SVG as rendered by Internet Explorer 11.

The first word (“BAM!”) looks the same, because all browsers rotate the gradient when the <text>
 element itself is transformed. However, the second word (“BOOM!”) looks uneven — with high contrast between letters — in Internet Explorer (and also Blink and WebKit browsers, which use the same painting strategy). The positions of the letters on the page, after character rotation, does not match their position used to calculate the gradient. The gradients are still affected by the dy
 and dx
 offsets, however, so the colors are not the same in each letter.

The gradients aren’t the only aspect of the SVG that differ from browser to browser. Even on the author’s own computer, not all browsers are able to access the “Gill Sans Ultra Bold” system font, despite the many variations on the name given. On another computer, this font might not even be available. In Chapter 10
 , we’ll revisit this example to examine the effects of fonts on layout.

We’ve now covered positioning text elements, positioning text spans, and positioning individual characters. In each case, the position you specify has been the *start*
 of a section of text. In Chapter 6
 , we explore how to position text centered over, or ending at, a point.