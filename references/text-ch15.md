Chapter 12.
 Extending Your Toolbox
SVG text, as we’ve mentioned a few times already, is designed for incredibly detailed control of layout, but at the cost of little support for automated multiline text blocks. That was quickly identified as its main weakness; SVG’s support for flowing, multiline text was dramatically strengthened in the proposed SVG 1.2 specification, but even the changes in the completed 1.2 Tiny specification were not significantly adopted by browsers. Until SVG 2 support is widespread, the SVG <text>
 element is restricted to manually positioned text lines.

However, this does not mean that SVG *cannot*
 include multiline flowing text — it only means that to do so, it needs to make use of a <foreignObject>

 element and render HTML within the SVG context.
A Foreigner in an SVG File
The <foreignObject>
 element can be seen as a type of portal or escape hatch, shifting the document out of the realm of SVG layout and into any other XML layout mode supported by the browser. Two types of XML content were in particular considered when the SVG specifications were created: XHTML and MathML.
Warning
Internet Explorer never supported the <foreignObject>
 element, although it has been implemented for Microsoft Edge.

There are a number of bugs and inconsistencies in other browsers’ implementations, so be sure to test thoroughly.

It is solely within the purview of the browser as to what types of content *can*
 be rendered. Web-browser SVG implementations can usually render basic HTML, but other SVG environments may not support it. 
 MathML — which defines mathematical equations and could be very useful for annotating a technical diagram — is not currently supported in web browsers other than Firefox; however, it could be useful for SVG within a textbook if you have full control over the software that will render the graphic.

Although <foreignObject>
 can render any content recognized by the browser, most of the time it’s used to add paragraphs of HTML text within a rectangular region. This is analogous to how the <svg>
 element in HTML 5 defines a region in that page for drawing a vector graphic.

The <foreignObject>
 element itself is positioned using x
 , y
 , width
 and height
 attributes, similar to an <image>
 . The child content is then drawn within this space as if that was the browser window or <iframe>
 .
Tip
The type of foreign content is identified by its XML namespace. In this case, that’s "http://www.w3.org/1999/xhtml"
 for XML-compatible HTML.

Example 12-1
 uses <foreignObject>
 elements to include blocks of descriptive text to a legend of all the SVG shape elements. Not only does the text wrap automatically, but the paragraphs scroll if required. Figure 12-1
 shows a screenshot of the page as it appears in the browser.

Example 12-1.
 Using foreign objects to include multiline, scrollable text in SVG
<svg

 

xmlns=

"http://www.w3.org/2000/svg"

 

xml:lang=

"en"

     

width=

"4in"

 

height=

"6.5in"

 

viewBox=

"0 0 400 650"

 

>

    

<title

>

ForeignObjects describing SVG Shapes

</title>

    

<style 

type=

"text/css"

>

        

@namespace

 

html

 

"http://www.w3.org/1999/xhtml"

;

  





        

.shape

 

{

            

fill

:

 

deepSkyBlue

;

                   





            

stroke

:

 

blueViolet

;

            

stroke-width

:

 

3px

;

            

stroke-linecap

:

 

round

;

        

}

        

.backdrop

 

{

            

fill

:

 

#CED

;

        

}

        

text

 

{

            

font-size

:

 

medium

;

            

font-family

:

 

serif

;

        

}

        

text

[

role

=

"heading"

]

 

{

            

font

:

 

bold

 

64px

 

sans-serif

;

            

text-decoration

:

 

underline

 

overline

;

            

white-space

:

 

pre

;

            

fill

:

 

url(#blue-shine)

 

darkBlue

;

        

}

        

html

|

*

 

{

            

box-sizing

:

 

border-box

;

              





        

}

        

html

 

{

            

height

:

 

100%

;

                        





            

width

:

 

100%

;

            

background-color

:

 

white

;

            

border

:

 

2px

 

solid

 

lightGray

;

            

border-radius

:

 

5px

;

            

overflow

:

 

auto

;

                      





        

}

        

p

 

{

            

font-family

:

 

sans-serif

;

             





            

width

:

 

100%

;

            

height

:

 

100%

;

            

margin

:

 

0

;

            

padding

:

 

0.1em

 

0.2em

;

        

}

    

</style>

    

<linearGradient

 

id=

"blue-shine"

 

y2=

"100%"

>

        

<stop

 

offset=

"0.1"

 

stop-color=

"darkBlue"

 

/>

        

<stop

 

offset=

"0.3"

 

stop-color=

"deepSkyBlue"

 

/>

        

<stop

 

offset=

"0.5"

 

stop-color=

"dodgerBlue"

 

/>

        

<stop

 

offset=

"0.7"

 

stop-color=

"darkBlue"

 

/>

    

</linearGradient>

    

<rect

 

width=

"100%"

 

height=

"100%"

 

class=

"backdrop"

 

/>

    

<g

>

        

<line

 

x1=

"10"

 

y1=

"10"

 

x2=

"90"

 

y2=

"40"

 

class=

"shape"

 

/>

        

<text

 

x=

"120"

 

y=

"30"

 

>

Line

</text>

        

<foreignObject

 

x=

"10"

 

y=

"50"

 

width=

"180"

 

height=

"90"

>

 





            

<html

 

xmlns=

"http://www.w3.org/1999/xhtml"

                  

tabindex=

"0"

>

                               





                

<p

>

The 

<code

>

&lt;

line

&gt;

</code>

 element draws
                    a straight line defined by its start and
                    end points.

</p>

                           





            

</html>

        

</foreignObject>

    

</g>

    

<g

 

transform=

"translate(200,0)"

>

        

<rect

 

x=

"10"

 

y=

"10"

 

width=

"80"

 

height=

"30"

 

rx=

"5"

              

class=

"shape"

 

/>

        

<text

 

x=

"120"

 

y=

"30"

 

>

Rectangle

</text>

        

<foreignObject

 

x=

"10"

 

y=

"50"

 

width=

"180"

 

height=

"90"

>

 





            

<html

 

xmlns=

"http://www.w3.org/1999/xhtml"

                  

tabindex=

"0"

>

                

<p

>

The 

<code

>

&lt;

rect

&gt;

</code>

 element draws
                    a rectangle, optionally with rounded
                    corners.

</p>

            

</html>

        

</foreignObject>

    

</g>

    

<g

 

transform=

"translate(0,150)"

>

        

<circle

 

cx=

"50"

 

cy=

"25"

 

r=

"20"

 

class=

"shape"

 

/>

        

<text

 

x=

"120"

 

y=

"30"

 

>

Circle

</text>

        

<foreignObject

 

x=

"10"

 

y=

"50"

 

width=

"180"

 

height=

"90"

>

            

<html

 

xmlns=

"http://www.w3.org/1999/xhtml"

                  

tabindex=

"0"

>

                

<p

>

The 

<code

>

&lt;

circle

&gt;

</code>

 element draws
                    a circle defined by its center-point and
                    radius.

</p>

            

</html>

        

</foreignObject>

    

</g>

    

<g

 

transform=

"translate(200,150)"

>

        

<ellipse

 

cx=

"50"

 

cy=

"25"

 

rx=

"40"

 

ry=

"20"

 

class=

"shape"

 

/>

        

<text

 

x=

"120"

 

y=

"30"

 

>

Ellipse

</text>

        

<foreignObject

 

x=

"10"

 

y=

"50"

 

width=

"180"

 

height=

"90"

>

            

<html

 

xmlns=

"http://www.w3.org/1999/xhtml"

                  

tabindex=

"0"

>

                

<p

>

The 

<code

>

&lt;

ellipse

&gt;

</code>

 element draws
                    an ellipse, which is like a stretched circle
                    or a rectangle with rounded corners that meet
                    in the middle.

</p>

            

</html>

        

</foreignObject>

    

</g>

    

<g

 

transform=

"translate(0,300)"

>

        

<polygon

 

points=

"10,10 50,40 90,10 50,25"

 

class=

"shape"

 

/>

        

<text

 

x=

"120"

 

y=

"30"

 

>

Polygon

</text>

        

<foreignObject

 

x=

"10"

 

y=

"50"

 

width=

"180"

 

height=

"90"

>

            

<html

 

xmlns=

"http://www.w3.org/1999/xhtml"

                  

tabindex=

"0"

>

                

<p

>

The 

<code

>

&lt;

polygon

&gt;

</code>

 element draws
                    a custom shape by connecting a series of
                    points.

</p>

            

</html>

        

</foreignObject>

    

</g>

    

<g

 

transform=

"translate(200,300)"

>

        

<polyline

 

points=

"10,10 50,40 90,10 50,25"

                  

class=

"shape"

 

style=

"fill: none"

 

/>

        

<text

 

x=

"120"

 

y=

"30"

 

>

Polyline

</text>

        

<foreignObject

 

x=

"10"

 

y=

"50"

 

width=

"180"

 

height=

"90"

>

            

<html

 

xmlns=

"http://www.w3.org/1999/xhtml"

                  

tabindex=

"0"

>

                

<p

>

The 

<code

>

&lt;

polyline

&gt;

</code>

 element
                    also connects a series of points,
                    but does not insert a final stroke from the
                    last point to the first.

</p>

            

</html>

        

</foreignObject>

    

</g>

    

<g

 

transform=

"translate(0,450)"

>

        

<path

 

d=

"M10,10 C15,50 85,50 90,10 L50,25 Z"

 

class=

"shape"

 

/>

        

<text

 

x=

"50"

 

y=

"75"

 

text-anchor=

"middle"

>

Path

</text>

        

<foreignObject

 

x=

"125"

 

y=

"0"

 

width=

"265"

 

height=

"90"

>

            

<html

 

xmlns=

"http://www.w3.org/1999/xhtml"

                  

tabindex=

"0"

>

                

<p

>

The 

<code

>

&lt;

path

&gt;

</code>

 element
                    can be used to draw any of the above,
                    or to create a custom shape with a mixture
                    of lines, Bézier curves, and arcs.

</p>

            

</html>

        

</foreignObject>

    

</g>

    

<text

 

x=

"50%"

 

text-anchor=

"middle"

 

transform=

"translate(0,625)"

          

textLength=

"400"

 

lengthAdjust=

"spacingAndGlyphs"

          

xml:space=

"preserve"

          

role=

"heading"

>

 SVG Shapes 

</text>



</svg>

An @namespace
 declaration at the top of the <style>
 block defines a prefix by which style rules may specifically target XHTML elements.

The SVG styles should be familiar by now; the text labels are quite basic, but a heading takes advantage of a number of styling features, including a gradient fill, preserved whitespace, and an adjusted text length.

To make it easier to size the HTML elements precisely without clipping, box-sizing
 is set to border-box
 on all elements in the HTML namespace. Considering that box-sizing
 does not have an effect on SVG elements, the namespace is optional — but it’s a reminder that you can use XML namespaces in CSS selectors!

To make the HTML content fill up the entire space allotted to it, a height of 100% is set explicitly on each <html>
 element. Background and border styles decorate the box.

The overflow
 property ensures that the paragraph will be scrollable if the content exceeds the set height.

The <p>
 paragraph elements are likewise styled using all the properties you can use in an HTML document.

In the markup, each <foreignObject>
 element is positioned and sized using x
 , y
 , width
 , and height
 .

The foreign content needs to be a valid XML document fragment, and is identified by the xmlns
 attribute on an <html>
 element. 
 )))Because these elements may need to be scrolled by the user, they have a tabindex
 attribute to ensure that keyboard users can access them.

Within the HTML-namespaced content, you can use any HTML elements as usual, and the browser’s default styling will apply — such as monospaced font for <code>
 elements. Note, however, that you still cannot use HTML-specific entities, which cause XML validation errors before the browser even parses the markup. The &lt;
 and &gt;
 entities used here are defined for all XML.

The other <foreignObject>
 elements are similar, each one positioned within the transformed coordinate system created by the <g>
 elements.

Because the <foreignObject>
 is positioned within the local SVG coordinate system, it is affected by all transformations including rotations, skews, and scaling effects. The foreign object can also be affected by any graphical effects that can apply to a grouping element, such as masks and clipping.

Although the foreign HTML content is laid out as if it was in an <iframe>
 , it is still part of the same document. Just as with inline SVG in an HTML document, this means that the same style rules apply to both types of content. So, for example, you can add a hover/focus effect with rules like the following:

g

:hover

 html

,

 html

:focus

 {

    border-color

:

 crimson

;

}

g

:hover

 .shape

 {

    stroke

:

 crimson

;

}


If *either*
 the text or the graphics are moused-over, the parent <g>
 element will match the :hover
 pseudoclass, and both types of content will be highlighted to emphasize their connection. Figure 12-2
 shows the relevant part of the graphic when the rectangle description is hovered.

Making the effect keyboard accessible is more difficult, however. The :focus
 pseudoclass only applies on the directly focused element, not its ancestors. 
 You *could*
 add tabindex="0"
 and focusable="true"
 attributes to each <g>
 to make them focusable in most browsers. However, to allow users to scroll the text elements, it is the <html>
 elements that should be keyboard focusable. 
 
 The new :has(*selector*
 )
 pseudoclass selector (introduced by the Selectors Level 4 specification) will address this difficulty, allowing you to style the SVG graphics with the following rule:

g

:has

(

:focus

)

 .shape

 {

    stroke

:

 crimson

;

}


Unfortunately, :has()
 is not currently supported in web browsers. What’s worse, unrecognized selectors in CSS invalidate an entire rule, so you cannot simply add the future-focused syntax as a comma-separated alternative after the :hover
 selector. Instead, the entire rule needs to be duplicated with the new syntax.

Another potential use of <foreignObject>
 and HTML is to integrate form input elements and other interactive content within your graphic, allowing you to make use of all the native functionality of these elements in HTML. However, this is one area where implementations are currently very buggy. The appearance of input elements may not be correctly updated when the user interacts with them.
Tip
Although you can apply many SVG effects to foreign content, a <foreignObject>
 cannot be duplicated with <use>
 ; one reason for that restriction is to avoid multiple conflicting instances of the same input element.

When SVG is inline in an HTML 5 page, limited support for (and bugs within) <foreignObject>
 can sometimes be patched-over by simply making it *appear*
 that the HTML content is contained inside your SVG. Sibling HTML and SVG elements can be positioned on the same region of the page by including the <svg>
 and the HTML elements within a wrapper <div>
 or <figure>
 , and using CSS absolute positioning to place the HTML content. SVG graphical effects do not apply to the superimposed HTML content, but with increasing support for transformations and masks in HTML, that is less of an issue; many of the effects you would want to apply to a block of embedded text can be applied to the HTML elements directly.

Even as more graphical effects are being adopted into HTML, the need
 to embed foreign-namespaced text content in SVG should diminish. With multiline text support in SVG 2, HTML <foreignObject>
 will no longer be required simply to create a paragraph of flowing text.