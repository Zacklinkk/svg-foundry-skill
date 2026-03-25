Appendix B.
 Text and Font Style Properties
This appendix summarizes all the text and font-related style properties that currently have an impact on SVG 1.1 text elements. Except where explicitly noted otherwise, they can all be defined either with CSS stylesheets, inline style attributes, or presentation attributes. Most properties are inherited by default; those that aren’t are clearly indicated. Any property can be forced to inherit with the inherit
 keyword, or reset to the default value with initial
 .

You’ll need to consult the main text or the specifications for details about the effect of each style value; this is primarily intended as a reference that you can flip to whenever you need to confirm the default for a property or the spelling of a keyword.

As multiline text is introduced for SVG 2, many other CSS properties will become relevant. In addition, new style properties introduced for CSS 3 may apply to SVG text.

alignment-baseline

Defines which point in each text glyph, perpendicular to the inline
 text layout orientation, should be aligned with the equivalent point in the parent text content.

- 
*Allowed values:*

- One of the baseline keywords: alphabetic
 , ideographic
 , hanging
 , mathematical
 , central
 , middle
 , text-before-edge
 , text-after-edge
 (CSS 3 replaces the last two with text-top
 and text-bottom
 )

- 
auto
 or baseline

- In CSS 3: top
 , bottom
 , and center
 , which would use the total block dimensions instead of font baselines

- 
*Default:*
 in SVG 1.1, auto
 (not well implemented); in CSS 3, baseline

- 
*Not inherited by default*

- 
*Applies to:*
 text elements

- 
*Defined in:*
 SVG 1.1, CSS Inline Layout Level 3

baseline-shift

Defines an offset from the normal alignment-baseline that should apply for the extent of this element. Positive values raise the baseline, negative values lower it.

- 
*Allowed values:*

- A percentage of the line height (which defaults to match the font size for single-line SVG)

- A length with units

- One of the keywords baseline
 , sub
 , or super
 (CSS 3 removes baseline
 — use a length of 0 instead)

- 
*Default:*
 in SVG 1.1, baseline
 ; in CSS 3, the length 0 (which has the same effect)

- 
*Applies to:*
 text elements

- 
*Defined in:*
 SVG 1.1, CSS Inline Layout Level 3

direction

Defines the direction of inline text layout.

- 
*Allowed values:*
 ltr
 (left-to-right) or rtl
 (right-to-left)

- 
*Default:*
 ltr

- 
*Applies to:*
 text elements

- 
*Defined in:*
 CSS2, CSS Writing Modes Level 3

dominant-baseline

Defines which point in each text glyph, perpendicular to the inline text layout orientation, should be aligned with the anchor point.

- 
*Allowed values:*

- One of the baseline keywords: alphabetic
 , ideographic
 , hanging
 , mathematical
 , central
 , middle
 , text-before-edge
 , text-after-edge
 (CSS 3 replaces the last two with text-top
 and text-bottom
 )

- 
auto
 , which means alphabetic
 for horizontal text and central
 for vertical text (CSS 3 clarifies that alphabetic baseline should also be used for sideways vertical text)

- 
*Default:*
 auto

- 
*Not inherited by default*

- 
*Applies to:*
 text elements

- 
*Defined in:*
 SVG 1.1, CSS Inline Layout Level 3

font

Shorthand property to set all the font-related properties (and reset those not specified to defaults). *Not defined as a presentation attribute; use the individual properties instead.*

- 
*Allowed values:*

- Any keywords for font-style
 , font-variant
 , and font-weight
 , followed by font-size
 , optionally followed by line-height
 separated with a */*
 character, then finally a font-family
 list.

- One of the keywords caption
 , icon
 , menu
 , message-box
 , small-caption
 , or status-bar
 , which should set all font properties to match system defaults for that type of interface text.

- 
*Default:*
 As for individual properties

- 
*Applies to:*
 Text elements

- 
*Defined in:*
 CSS 2, SVG 1.1

font-family

Specifies a list of typeface families from which to select the font, from most to least preferred.

- 
*Allowed values:*
 A comma-separated list of the following options:

- Typeface names to be selected from the operating system’s (quoted if they contain whitespace or special characters).

- Font-family names defined in an @font-face
 rule.

- One of the five generic font keywords: serif
 , sans-serif
 , monospace
 , cursive
 , fantasy
 (should be last value in the list, as these will always match a browser font).

- 
*Default:*
 Browser-specific

- 
*Applies to:*
 Text elements

- 
*Defined in:*
 CSS 2, SVG 1.1

font-size

Sets the size of text, by defining the height of a single line of text.

- 
*Allowed values:*

- A length or percentage; percentages and font-based relative units such as em
 and ex
 are calculated relative to the inherited font size.

- For SVG presentation attributes only, a unitless number that will be interpreted as a length in user coordinates.

- One of the keywords xx-large
 , x-large
 , large
 , medium
 , small
 , x-small
 , or xx-small
 .

- One of the keywords larger
 or smaller
 , which will adjust the inherited font size.

- 
*Default:*
 medium
 ; however, be cautious about relying on default font sizes for SVG text within images because of browser bugs.

- 
*Applies to:*
 Text elements

- 
*Defined in:*
 CSS 2, SVG 1.1

font-size-adjust

Indicates that browsers should adjust the font size to maintain a specified ex
 height. The value is the expected ratio of ex
 to em
 units in the preferred font. If the font used by the browser has a different ratio, it should adjust the displayed font size in order to maintain the expected ex
 size; *however*
 , the actual values for em
 and ex
 units and other properties such as line-height
 will not change.

- 
*Allowed values:*
 A number between 0.0 and 1.0, or the keyword none

- 
*Default:*
 none

- 
*Applies to:*
 Text elements

- 
*Defined in:*
 CSS 2, SVG 1.1, CSS Fonts Level 3

font-stretch

Indicates which typeface from the specified family should be used, according to the width of the characters from narrow to wide.

- 
*Allowed values:*
 one of the keywords normal
 , wider
 , narrower
 , ultra-condensed
 , extra-condensed
 , condensed
 , semi-condensed
 , semi-expanded
 , expanded
 , extra-expanded
 , ultra-expanded

- 
*Default:*
 normal

- 
*Applies to:*
 text elements

- 
*Defined in:*
 CSS 2

font-style

Indicates whether an italic typeface from the specified family should be used.

- 
*Allowed values:*
 One of the keywords normal
 , italic
 , or oblique
 ; unless a given font-family actually has both italic
 and oblique
 faces defined (highly unlikely), italic
 and oblique
 will be treated as synonyms.

- 
*Default:*
 normal

- 
*Applies to:*
 Text elements

- 
*Defined in:*
 CSS 2, SVG 1.1

font-variant

Indicates which typographical options should be used from the font.

- 
*Allowed values:*

- In CSS 2/SVG 1.1: normal
 or small-caps

- In CSS 3, font-variant
 becomes a shorthand for a number of properties for selecting OpenType font features

- 
*Default:*
 normal

- 
*Applies to:*
 text elements

- 
*Defined in:*
 CSS 2, CSS Fonts Level 3

font-weight

Indicates which typeface from the specified family should be used, according to the thickness of the strokes from light to heavy.

- 
*Allowed values:*

- One of the absolute keywords normal
 or bold

- One of the relative keywords lighter
 or bolder
 (which adjust relative to the inherited value)

- A numeric weight, as a multiple of 100 between 100 and 900, where 400 is normal
 and 600 is bold

- 
*Default:*
 normal

- 
*Applies to:*
 text elements

- 
*Defined in:*
 CSS 2

glyph-orientation-horizontal

How glyphs should be aligned when the text writing mode is horizontal.

- 
*Allowed values:*
 0
 , 90
 , 180
 , or 270

- 
*Default:*
 0

- 
*Applies to:*
 text elements with horizontal writing mode

- 
*Defined in:*
 SVG 1.1; deprecated by CSS Writing Modes Level 3 (use the rotate
 attribute instead)

glyph-orientation-vertical

How glyphs should be aligned when the text writing mode is vertical.

- 
*Allowed values:*

- An angle value: 0
 , 90
 , 180
 , or 270

- The keyword auto
 , which equates to 0 for full-width characters and 90 for other characters

- 
*Default:*
 auto

- 
*Applies to:*
 text elements with vertical writing mode

- 
*Defined in:*
 SVG 1.1; deprecated by CSS Writing Modes Level 3 (use text-orientation
 or the rotate
 attribute instead)

kerning

Whether font-specific spacing adjustments between glyphs should be applied.

- 
*Allowed values:*

- 
auto
 to allow normal kerning

- A length to impose arbitrary spacing; should only be used with length 0 to disable automatic kerning (use letter-spacing
 to alter spacing)

- 
*Default:*
 auto

- 
*Applies to:*
 text elements

- 
*Defined in:*
 SVG 1.1; deprecated and replaced by font-kerning
 in CSS Fonts Level 3, which only supports auto
 (kerning at the browser’s discretion), normal
 (kerning as defined in the font), or none
 (no kerning)

letter-spacing

Determines whether and how much extra space should be added in between individual character glyphs. A non-zero value disables all non-essential ligatures; results may be poor if the text contains essential ligatures.

- 
*Allowed values:*

- 
normal

- A length, with units or (for presentation attributes only) as a number of SVG user units

- 
*Default:*
 normal

- 
*Applies to:*
 text elements

- 
*Defined in:*
 CSS 2, SVG 1.1, CSS Text Level 3

text-anchor

Defines how each chunk of continuous text in the inline flow direction (horizontal or vertical) should be aligned relative to the anchor point in that axis (*x*
 or *y*
 ).

- 
*Allowed values:*
 start
 , middle
 , or end

- 
*Default:*
 start

- 
*Applies to:*
 text elements

- 
*Defined in:*
 SVG 1.1

text-decoration

Sets the type of emphasis line, if any, that should be added along the length of the span of text.

- 
*Allowed values:*

- 
none

- Any combination of the keywords underline
 , overline
 , or line-through

- CSS 3 makes this a shorthand, and so would allow a combination of the values for text-decoration-position
 , text-decoration-style
 , and text-decoration-color
 (which would not have a direct effect for SVG)

- 
*Default:*
 none

- 
*Not inherited by default*

- 
*Applies to:*
 text elements

- 
*Defined in:*
 CSS 2, SVG 1.1, CSS Text Decoration Level 3

text-orientation

How characters should be oriented in vertical text; replaces glyph-orientation-vertical
 .

- 
*Allowed values:*
 One of the keywords mixed
 , upright
 , sideways-right
 , sideways-left
 , sideways
 , or use-glyph-orientation

- 
*Default*
 : mixed

- 
*Not yet supported as an SVG presentation attribute*

- 
*Applies to:*
 text elements

- 
*Defined in:*
 CSS Writing Modes Level 3

unicode-bidi

Determines whether the browser should apply the Unicode bidirectional algorithm to rearrange the character content to suit the layout direction.

- 
*Allowed values:*

- 
normal
 , embed
 , or bidi-override

- CSS 3 adds isolate
 , isolate-override
 , and plaintext

- 
*Default:*
 normal

- 
*Applies to:*
 text elements

- 
*Defined in:*
 CSS 2, CSS Writing Modes Level 3

word-spacing

Determines whether and how much extra space should be added to whitespace characters that serve as word breaks in the text.

- 
*Allowed values:*

- 
normal

- A length, with units or (for presentation attributes only) as a number of SVG user units

- CSS 3 adds a percentage option, where the percentage is an increase relative to the normal spacing for that whitespace character

- 
*Default:*
 normal

- 
*Applies to:*
 text elements

- 
*Defined in:*
 CSS 2, SVG 1.1, CSS Text Level 3

writing-mode

Defines the layout orientation for text.

- 
*Allowed values:*

- In SVG 1.1, lr
 , lr-tb
 , rl
 , rl-tb
 for horizontal text; in CSS 3, horizontal-tb

- In SVG 1.1, tb
 and tb-rl
 for vertical text; in CSS 3, vertical-rl

- In CSS 3, vertical-lr

- 
*Default:*
 in SVG 1.1, lr-tb
 ; in CSS 3, horizontal-tb

- 
*Applies to:*
 text elements

- 
*Defined in:*
 SVG 1.1, CSS Writing Modes Level 3