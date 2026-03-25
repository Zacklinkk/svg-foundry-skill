Appendix A.
 Text Elements and Attributes
There are three text container elements in SVG: <text>
 , <tspan>
 , and <textPath>
 . However, both <tspan>
 and <textPath>
 must always be used *inside*
 a <text>
 element. Also described here is the <foreignObject>
 element, which is most commonly used to add HTML text to an SVG.

<text>

A self-contained sequence of text content to be included in the graphic, possibly including child elements with styled or positioned text.

x

The horizontal position of the anchor point or points

- A list of space- or comma-separated lengths (with units or as numbers of user units) or percentages (of the coordinate system width)

- Values are assigned to individual characters in the text content of this or child elements

- Default is a single value 0

y

The vertical position of the anchor point or points

- A list of space- or comma-separated lengths (with units or as numbers of user units) or percentages (of the coordinate system width)

- Values are assigned to individual characters in the text content of this or child elements

- Default is a single value 0

dx

The horizontal offsets to be applied to each character’s position

- A list of space- or comma-separated lengths (with units or as numbers of user units) or percentages (of the coordinate system width)

- Values are assigned to individual characters in the text content of this or child elements

- Default is an empty list (no additional offsets)

dy

The vertical offsets to be applied to each character’s position

- A list of space- or comma-separated lengths (with units or as numbers of user units) or percentages (of the coordinate system width)

- Values are assigned to individual characters in the text content of this or child elements

- Default is an empty list (no additional offsets)

rotate

A rotation to be applied to each character

- A list of space- or comma-separated numbers, representing angles in degrees

- Values are assigned to individual characters; however, a final value is repeated as necessary for all remaining characters within this element or its children

- Default is the single value 0

textLength

The expected or desired total offset length of this text element, including all child content

- A length (with units or as numbers of user units)

- Default is to use the textLength
 computed by the browser, without adjustment

lengthAdjust

The parts of the text that the browser may modify if required to match the textLength
 value

- One of the values spacing
 or spacingAndGlyphs

- Default spacing

<tspan>

A section of text content with distinct styles or positioning attributes. Attributes are the same as for <text>
 *except*
 for the following differences:

- Any values for a given character for x
 , y
 , dx
 , dy
 , or rotate
 supersede values specified for the same character on parent elements

- The defaults for x
 and y
 are empty lists: no absolute positioning is applied

<textPath>

A section of text that should be arranged along the outline of a path

xlink:href

A reference to the path that should be used to position the text

- A URL with a target fragment that matches the id
 of a <path>
 element

- Theoretically, the <path>
 could be in a different document, but this has limited support

- In XML documents (including SVG), the xlink
 prefix must be attached to the XLink namespace, *

*http://www.w3.org/1999/xlink*

*
 , using an xmlns:xlink
 attribute

- If not specified, or if the <path>
 cannot be located, the text content will not be drawn

startOffset

The position along the path at which to anchor the text

- A length (with units or as a number of user units) measured from the start of the path, or a percentage of the path’s length

- Default 0

method

Whether the browser should align
 individual characters along the path, or stretch
 them around curves while maintaining connections between them. No effect in web browsers currently.

spacing

A hint to the browser on whether glyphs should be positioned at exact
 (default) distances along the path or whether optimizations can be used (auto
 ). No effect in web browsers currently.

<foreignObject>

A container element for a block of XML content in a non-SVG namespace, which should be rendered into a specified region of the SVG.

x

Horizontal position of the corner of the foreign content area that has minimum coordinates

- A length (in user coordinates or with units) or percentage (of coordinate system width)

- Default 0

y

Vertical position of the corner of the foreign content area that has minimum coordinates

- A length (in user coordinates or with units) or percentage (of coordinate system height)

- Default 0

width

The width in which to position the foreign content

- A length (in user coordinates or with units) or percentage (of the parent coordinate system width)

- Default 0, which disables rendering

- Negative values are an error

height

The height in which to position the foreign content

- A length (in user coordinates or with units) or percentage (of the parent coordinate system height)

- Default 0, which disables rendering

- Negative values are an error