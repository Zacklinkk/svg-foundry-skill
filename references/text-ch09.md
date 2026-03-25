Chapter 6.
 Casting Anchor
Every modern word processor has the option to left-align, right-align, center, or justify text. CSS text layout uses the text-align
 property to choose between these options for positioning text within the layout box.

For CSS or word processors, the page layout defines margins on either side of the text region. Alignment settings determine how extra whitespace is shifted within this space: accumulating on one side or the other, divided evenly on either side, or distributed in between words.

In SVG, there are no margins and no boxes. Each line of text is just as long as it needs to be. As we have seen, it might not even be a straight
 line. SVG *does*
 provide tools to center or right-align text. The options, however, are not as straightforward as the text-align
 property.
Start, Middle, or End
SVG text alignment is controlled by the text-anchor
 property. The point you specify with x
 and y
 is the *anchor point*
 for that span of text, and text-anchor
 controls whether this point is positioned at the start
 , middle
 , or end
 of the span.

The default for text-anchor
 is start
 . Other values (end
 or middle
 ) can be specified as a presentation attribute or via CSS.
Tip
The text-anchor
 property is inherited, so it can be set once for the entire SVG if desired.

Example 6-1
 displays three strings, with the same horizontal position but different text-anchor
 values; Figure 6-1
 shows the result. A vertical
 guideline marks the *x*
 position used to anchor all the text elements.

Example 6-1.
 Aligning SVG text around an anchor point
<svg

 xmlns=

"http://www.w3.org/2000/svg"

     xmlns:xlink=

"http://www.w3.org/1999/xlink"

     xml:lang=

"en"

     width=

"4.3in"

 height=

"1.72in"

 viewBox=

"0 0 400 160"

>

    <title>

Anchoring SVG Text</title>

    <style 

type=

"text/css"

>

        svg

 {

            font

:

 bold

 50px

 serif

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

    <line

 stroke=

"gray"

 x1=

"50%"

 x2=

"50%"

 y2=

"100%"

 />

    <text

 text-anchor=

"start"

 fill=

"darkGreen"

          x=

"50%"

 y=

"1em"

 >

Start</text>

    <text

 text-anchor=

"middle"

 fill=

"navy"

          x=

"50%"

 y=

"2em"

 >

Middle</text>

    <text

 text-anchor=

"end"

 fill=

"darkRed"

          x=

"50%"

 y=

"3em"

 >

End</text>

</svg>

For left-to-right English text like this, start
 -anchored text ends up on the right of the anchor point (left-aligned to the guideline), middle
 -anchored text is centered over the anchor, and end
 -anchored text is positioned on the left of the anchor point (right-aligned relative to the guideline).

Aligning a single line of text like this is straightforward and predictable. But how do you align text that has a mixture of repositioned spans and characters? The answer is that you break it up into *text chunks*
 .
Text Chunks
For SVG text layout, a text chunk (yes, that is the technical term from the specifications!) is a sequence of characters anchored on an absolutely positioned point. 
 Because we are, so far, dealing with horizontal alignment of horizontal text, only the absolute x
 values matter. Changes in y
 position won’t affect the horizontal alignment.

The text chunks used for alignment purposes can span multiple <tspan>
 elements, or they can be individual characters.
Tip
Any character that is assigned an absolute position in the text’s inline direction becomes the start of a new chunk.

Example 6-2
 presents text in which some, but not all, characters have explicit x
 values. 
 The text — and a guideline that shows the anchor position — is duplicated with a <use>
 element that resets both the fill
 color and the text-anchor
 property. As an inherited style property, text-anchor
 can be changed when text is reused (assuming the original text used an inherited or default value).
Tip
Duplicated text should only be used for graphical effects, such as reflections or shadows. It is not recognized as text by assistive technologies such as screen readers.

Figure 6-2
 shows the result.

Example 6-2.
 Aligning SVG text with character and span positioning
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

     

xmlns:xlink=

"http://www.w3.org/1999/xlink"

     

xml:lang=

"en"

     

width=

"4.3in"

 

height=

"2.4in"

 

viewBox=

"0 0 400 225"

>

    

<title

>

Anchoring Custom-Positioned Text

</title>

    

<style 

type=

"text/css"

>

        

svg

 

{

            

font

:

 

bold

 

50px

 

serif

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

 

text-anchor=

"start"

       

fill=

"darkGreen"

 

transform=

"translate(20,0)"

>

       





        

<g

 

id=

"sample"

>

                                    





            

<line

 

stroke=

"gray"

 

y2=

"100%"

 

/>

               





            

<text

 

x=

"0 0 0"

 

y=

"40"

                  

dy=

"0 25 25 25 25 25"

                  

>

Anchor                                  





                

<tspan

 

x=

"0"

 

dy=

"1em"

>

SVG

</tspan>

</text>

   





        

</g>

    

</g>

    

<use

 

xlink:href=

"#sample"

 

text-anchor=

"middle"

         

fill=

"navy"

 

transform=

"translate(200,0)"

 

/>

       





    

<use

 

xlink:href=

"#sample"

 

text-anchor=

"end"

         

fill=

"darkRed"

 

transform=

"translate(380,0)"

 

/>



</svg>

The first block of text uses start
 alignment and dark green fill; these styles are set on a wrapping <g>
 element so they won’t be copied when the text is duplicated.

An inner <g>
 element groups together the text and the guideline; this group has an id
 value and will be duplicated by the <use>
 elements.

The <line>
 has a single attribute, y2
 ; the others all default to 0. The guideline will therefore be drawn along the *y*
 -axis (*x*
 =0) in the transformed coordinate system.

Every character in the first word is given its own vertical position with dy
 ; the first three characters have their horizontal position reset to *x*
 =0.

The second word is its own <tspan>
 , with a single x
 value.

The <use>
 elements copy the inner group with the guideline and text, but translate the *x*
 =0 line to the right so the samples don’t overlap. The <use>
 elements also specify the text-anchor
 and fill
 properties as presentation attributes.

Because of the mix of character and span positioning, the text chunks are “A”, “n”, “chor ”, and “SVG”. The alignment for the single letters is straightforward and works as expected. Each letter-chunk is aligned against or centered over the guideline. For the larger chunks, even though the *first*
 character in the chunk has the position value, it is the chunk as a whole that is horizontally aligned.

The chunk as a whole includes the space after the word “Anchor,” which gets added to the diagonal letters. This pushes the “r” away from the end
 guideline, and unbalances the centered text.
Tip
When aligning a horizontal text chunk around an anchor point, the *entire*
 horizontal offset for that chunk is aligned, including any whitespace, letter-spacing
 , word-spacing
 , or dx
 offsets on characters other than the first one.

A dx
 offset on the first character in the chunk — the character that has the x
 anchor value — does not add to the length of text being centered or aligned. Instead, it shifts the anchor point. Whether it moves the text away from the x
 anchor point, or pulls the text over top of it, will depend on which part of the text chunk (start or end) is being anchored and which direction the text is written in. An offset on the anchor point is always measured in the coordinate system: negative offsets move left and positive offsets move right.
Working with Whitespace
The inclusion of starting and ending whitespace within the aligned text chunk is a frequent source of frustration in SVG text layout. There are a number of workarounds you can use:

- Remove all whitespace at the start and end of the text content, so that the opening and closing <
 and >
 markup characters are immediately adjacent to the words.

This is not recommended for multiple <tspan>
 elements within a single <text>
 element. It ruins the text for accessibility purposes — or for copy-and-paste selections — jamming all the letters together into a single word, regardless of their appearance on the screen.

- Ensure that the whitespace is always at the side of the text chunk where it won’t affect alignment. For start
 anchors, put the whitespace at the end of the chunk. For end
 anchors, put it at the start. For middle
 , add whitespace at both ends.

This has been used throughout the book in the examples with multiple <tspan>
 lines. However, it means that you cannot later change the alignment without editing the text.

- Create a separate text chunk to contain the whitespace — which is absolutely positioned in the *x*
 direction but doesn’t affect the *y*
 -position — by inserting the whitespace at the start of the <tspan>
 and using an extra value at the start of each positioning attribute list:
<text

 x=

"0 0 0"

 y=

"40"

      dy=

"0 25 25 25 25 25"

      >

Anchor<tspan

    x=

"0 0"

 dy=

"0 1em"

>

 SVG</tspan></text>




This strategy is the most robust, preserving the layout *and*
 the word breaks if you later change the alignment, as shown in Figure 6-3
 .

None of these options are ideal. The layout is improved in Figure 6-3
 , but at the cost of rather confusing markup.

In contrast, whitespace at the very start or end of a <text>
 element is ignored. It does not affect layout or the assignment of character-by-character positioning attributes.
Warning
Internet Explorer does not ignore whitespace at the end of the text string. For consistent alignment, always place the closing </text>
 tag immediately after the last character.

The previous discussion describes “whitespace” as a single, amorphous entity. For the most part, that’s how it works in SVG. Any number of consecutive space, tab, or newline characters in your SVG code gets collapsed into a single space character when the text is inserted into the SVG graphic. Other blank characters, such as the non-breaking space, do not collapse. 
 This whitespace collapsing works the same as the default behavior for HTML.

Rarely, you will want to include multiple spaces. SVG 1.1 uses the xml:space
 attribute for this purpose. The default behavior is equivalent to xml:space="default"
 . Setting a value of preserve
 will prevent the collapsing of multiple whitespace characters, or the trimming of spaces at the start and end of the string.
Warning
Internet Explorer and WebKit do not currently support xml:space="preserve"
 . 
 Firefox supports xml:space
 on any element, with the effect applied to all child content. 
 Blink browsers only support xml:space
 when directly applied to a text-containing element.

For the most part, xml:space="preserve"
 isn’t recommended when writing your own code: you can get better control of the SVG layout using dx
 to add extra space between characters. However, many graphics programs use preserved spaces to implement What-You-See-Is-What-You-Get (WYSIWYG) behavior. Designers working with a visual editor expect to be able to type spaces in the text and have them reflected in the graphic.
Tip
You don’t need to declare a namespace prefix for xml:space
 ; the xml
 prefix is reserved in all XML documents, including SVG.

If you do use xml:space
 , be aware that it only preserves whitespace as *spaces*
 : each tab or line break is transformed into a space character.