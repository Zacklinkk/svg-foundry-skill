Appendix B. Elements, Attributes, and Style Properties
This guide to SVG paint server elements provides a quick reference to the available attributes, with their default and allowed values.
<linearGradient>
A gradient in which color stops are drawn as parallel lines, extending perpendicularly (in the current coordinate system) from a gradient vector.

id

the value used to reference this gradient

- 
same restrictions as for any other element id

x1

horizontal position of the gradient vector’s start point

- 
a length (in user coordinates or with units) or percentage (of coordinate system width)

- 
default 0.

y1

vertical position of the gradient vector’s start point

- 
a length (in user coordinates or with units) or percentage (of coordinate system height)

- 
default 0.

x2

horizontal position of the gradient vector’s end point

- 
a length (in user coordinates or with units) or percentage (of coordinate system width)

- 
default 100%.

y2

vertical position of the gradient vector’s end point

- 
a length (in user coordinates or with units) or percentage (of coordinate system height)

- 
default 0.

gradientUnits

the coordinate system to use

- 
either userSpaceOnUse or objectBoundingBox

- 
default objectBoundingBox

gradientTransform

transformations to apply to the gradient content, independent of the shape it fills

- 
a list of whitespace-separated transformation functions: translate(*tx*,*ty*), scale(*s*), scale(*sx*,*sy*), rotate(*a*), rotate(*a*,*cx*,*cy*), skewX(*a*), and skewY(*a*)

- 
each transformation parameter is specified as a number without units (in the SVG 1 syntax); lengths default to user units (px), angles default to degrees

- 
default is no transformation

spreadMethod

the strategy to use for filling content beyond the start and end of the gradient vector

- 
one of pad, reflect or repeat

- 
default pad

xlink:href

a reference to another gradient which should be used as a template for this one

- 
a URL with a target fragment which must match the ID of a <linearGradient> or <radialGradient> element

- 
the URL may specify an element in a different file, but this is not well supported in web browsers

- 
all attributes on the referenced element become the default values for the current element

- 
if the current element does *not* contain any <stop> elements, then the stops from the referenced element will be used

- 
in XML documents (including SVG), the xlink prefix must be attached to the XLink namespace, **http://www.w3.org/1999/xlink**, using an xmlns:xlink attribute
<radialGradient>
A gradient in which color stops are drawn along rays from a starting point to an ending circle.

id

the value used to reference this gradient

- 
same restrictions as for any other element id

cx

horizontal position of the center-point

- 
a length (in user coordinates or with units) or percentage (of coordinate system width)

- 
default 50%

cy

vertical position of the center-point

- 
a length (in user coordinates or with units) or percentage (of coordinate system height)

- 
default 50%

r

radius of the circle

- 
a length (in user coordinates or with units) or percentage (proportional to the coordinate system diagonal length, as defined in <shapes-reference-percentages>)

- 
default 50%

- 
negative values are an error.

fx

horizontal position of the focal point

- 
a length (in user coordinates or with units) or percentage (of coordinate system width)

- 
default is to match cx

fy

vertical position of the focal point

- 
a length (in user coordinates or with units) or percentage (of coordinate system height)

- 
default is to match cy

gradientUnits

the coordinate system to use

- 
either userSpaceOnUse or objectBoundingBox

- 
default objectBoundingBox

gradientTransform

transformations to apply to the gradient content, independent of the shape it fills

- 
syntax and options are the same as same as for <linearGradient>

spreadMethod

the strategy to use for filling content beyond the ending circle

- 
syntax and options are the same as for <linearGradient>

xlink:href

a reference to another gradient which should be used as a template for this one

- 
syntax and options are the same as same as for <linearGradient>
<stop>
A fixed value within a gradient.

offset

The distance along the gradient vector or ray at which to position this value

- 
a number between 0 and 1, or a percentage

- 
values will be clamped to the range [0-1] or [0%-100%]

- 
stops must be listed in order of increasing offsets; if not, the offset will be adjusted to match the previous maximum value

- 
no official default, although most browsers will assume 0

stop-color (presentation attribute)

The color to use at this stop point

- 
any valid color definition supported by the browser

- 
default black

stop-opacity (presentation attribute)

The alpha value to use at this stop point

- 
a number between 0 and 1

- 
default 1
<pattern>
A paint server that defines a region of custom SVG content that should be used to fill other elements, repeating it as necessary in a rectangular grid.

id

the value used to reference this gradient

- 
same restrictions as for any other element id

x

horizontal offset of the top left corner of the reference pattern tile

- 
a length (in user coordinates or with units) or percentage (of coordinate system width)

- 
default 0

y

vertical offset of the top left corner of the reference pattern tile

- 
a length (in user coordinates or with units) or percentage (of coordinate system height)

- 
default 0

width

width of each pattern tile

- 
a length (in user coordinates or with units) or percentage (of coordinate system width)

- 
default 0, which disables rendering of the pattern

- 
negative values are an error

height

height of each pattern tile

- 
a length (in user coordinates or with units) or percentage (of coordinate system height)

- 
default 0, which disables rendering of the pattern

- 
negative values are an error

patternUnits

the coordinate system to use for x, y, width, and height

- 
either userSpaceOnUse or objectBoundingBox

- 
default objectBoundingBox

patternContentUnits

the coordinate system to use for drawing the child content of the pattern

- 
either userSpaceOnUse or objectBoundingBox

- 
default userSpaceOnUse

- 
has no effect if a viewBox is specified

- 
a value of objectBoundingBox is implemented as a non-uniform scale to the user units, it does not create a new reference definition for percentage lengths

viewBox

declaration of a custom coordinate system to use for the pattern contents

- 
a list of four numbers, separated by whitespace or commas

- 
the numbers represent, in order: minimum *x*, minimum *y*, width, and height

- 
the width and height values must be positive

- 
by default, the coordinate system is controlled by patternContentUnits

- 
the viewBox is implemented as a scaling transformation, and does not create a new reference definition for percentage lengths

preserveAspectRatio

the scaling and alignment strategy that should be used when the aspect ratio defined by the viewBox does not match the aspect ratio of the pattern tile

- 
Either none or an alignment value followed by meet or slice

- 
The alignment value is a single word of the form xM*xx*YM*xx* where M*xx* is one of Min, Mid, or Max

- 
default is xMidYMid meet

- 
has no effect unless a viewBox attribute is specified

patternTransform

transformations to apply to the pattern tiles and their content, independent of the shape it fills

- 
syntax and options are the same as same as for the gradientTransform attribute for <linearGradient>

- 
default is no transformation

xlink:href

a reference to another pattern which should be used as a template for this one

- 
a URL with a target fragment which must match the ID of a <pattern> element

- 
the URL may specify an element in a different file, but this is not well supported in web browsers

- 
all attributes on the referenced element become the default values for the current element

- 
if the current element does *not* contain any child elements, then the pattern content from the referenced element will be used

- 
in XML documents (including SVG), the xlink prefix must be attached to the XLink namespace, **http://www.w3.org/1999/xlink**, using an xmlns:xlink attribute

Index