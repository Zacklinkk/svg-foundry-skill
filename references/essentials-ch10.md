CHAPTER 9
Text
While it may be true that every picture tells a story, it’s perfectly all right to use words
to help tell the story. Thus, SVG has several elements that let you add text to your
graphics.
Text Terminology
Before investigating the primary method of adding text, the <text>  element, we should
define some terms you’ll see if you read the SVG specification or if you work with text
in any graphic environment:
Character
A character, as far as an XML document is concerned, is a byte or bytes with a
numeric value according to the Unicode standard. For example, what we call the
letter g is the character with Unicode value 103.
Glyph
A glyph is the visible representation of a character or characters.  A single character
can have many different glyphs to represent it. Figure 9-1  shows the word glyphs
written with two different sets of glyphs—look particularly at the initial g—it’s the
same character, but the glyphs are markedly different.
Figure 9-1. Two sets of glyphs
Multiple characters can reduce to a single glyph; some fonts have separate glyphs
for the letter combinations fl and ff to make their spacing look better (these are
called ligatures ). Other times, a single character can be composed of multiple glyphs;
125

a print program might create the character é (which has Unicode value 233) by
combining the e glyph with a nonspacing accent mark (´).
Font
A font is a collection of glyphs representing a certain set of characters.
All the glyphs in a font will normally have the following characteristics in common:
Baseline, ascent, and descent
All the glyphs in a font line up on the baseline.  The distance from the baseline to
the top of the tallest character in the font is the ascent; the distance from the baseline
to the bottom of the deepest character is the descent.  The total height of the char‐
acter, which is the sum of the ascent and descent, is also called the em-height. The
em-box is a square that has a width as large as an em-height.
Cap-height, ex-height
The cap-height is the height of a capital letter above the baseline.  The ex-height is,
logically enough, the distance from the baseline to the top of a lowercase letter x.
The ex-height is often a better measure of the subjective size (and readability) of a
font than the em-height.
The baseline, ascent, and descent of a typical Roman-letter font are marked in
Figure 9-2 . The upper dotted line shows the cap-height, while the lower dotted line
marks the ex-height.
Figure 9-2. Glyph measurements
Simple Attributes and Properties of the <text> Element
The simplest form of the <text>  element requires only two attributes, x and y, which
define the point where the baseline of the first character of the element’s content is
placed. The default style for text, as with all objects, is to have a fill color of black and
no outline. This, as it turns out, is precisely what you want for text. If you set the outline
as well as the fill, the text looks uncomfortably thick. If you set only the outline, you can
get a fairly pleasant set of outlined glyphs, especially if you lower the stroke width.
Example 9-1  uses the placement and stroke/fill characteristics for <text> ; the result is
Figure 9-3 .
126 | Chapter 9: Text

Example 9-1. Text placement and outlining
<!-- guide lines -->
<path d="M 20 10, 20 120 M 10 30 100 30 M 10 70 100 70
   M 10 110 100 110"  style="stroke: gray;" />
<text x="20" y="30">Simplest Text </text>
<text x="20" y="70" style="stroke: black;" >Outlined/filled </text>
<text x="20" y="110" style="stroke: black; stroke-width: 0.5;
    fill: none;" >Outlined only </text>
Figure 9-3. Text placement and outlining
Many of the other properties that apply to text are the same as they are in the Cascading
Style Sheets standard.  Here are some of the CSS properties and values that are imple‐
mented in the Apache Batik viewer version 1.7. They also work in most (but not all)
browsers:
font-family
The value is a whitespace-separated list of font family names or generic family
names. This is a list of fallback values; the SVG viewer will use the first family name
it recognizes. The generic names must be the last in the list. The SVG viewer is
required to recognize generic names and have fonts available for them. The generic
family names are serif , sans-serif , monospace , fantasy , and cursive . Serif fonts
have little “hooks” at the ends of the strokes; sans-serif fonts don’t. In Figure 9-1 ,
the word at the left is in a serif font and the word on the right is in a sans-serif font.
Both serif and sans-serif fonts are proportional; the width of a capital M is not the
same as the width of a capital I. A monospace font, which may or may not have
serifs, is one where all the glyphs have the same width, like the letters of a typewriter.
The fantasy  and cursive  default fonts can vary considerably from one browser or
SVG viewer to another.
font-size
The value is the baseline-to-baseline distance of glyphs if you were to have more
than one line of text. (In SVG, you must position multiline <text>  content yourself,
so the concept is somewhat abstract.) If you use units on this attribute, as in
style="font-size: 12pt" , the size will be converted to user units before being
rendered, so it can be affected by transformations and the SVG viewBox . If you use
Simple Attributes and Properties of the <text> Element | 127

relative units ( em, ex, or percentages), they are calculated relative to the inherited
font size.
font-weight
The two most commonly used values of this property are bold  and normal . Y ou
need the normal  value in case you want to place nonbold text in a group that has
been set to style="font-weight: bold" .
font-style
The two most commonly used values of this property are italic  and normal .
text-decoration
Possible values of this property are none , underline , overline , and
line-through .
word-spacing
The value of this property is a length, either in explicit units such as pt or in user
units. Make this a positive number to increase the space between words, set it to
normal  to keep normal space, or make it negative to tighten up the space between
words. The length you specify is added  to the normal spacing.
letter-spacing
The value of this property is  a length, either in explicit units such as pt or in user
units. Make this a positive number to increase the space between individual letters,
set it to normal  to keep normal space, or make it negative to tighten up the space
between letters. The length you specify is added  to the normal spacing.
Example 9-2  uses these styles to produce Figure 9-4 , with effects you’d expect from any
competent text application.
Example 9-2. Text weight, style, decoration, and spacing attributes
<g style="font-size: 18pt" >
<text x="20" y="20" style="font-weight:bold;" >bold</text>
<text x="120" y="20" style="font-style:italic;" >italic</text>
<text x="20" y="60" style="text-decoration:underline;" >under</text>
<text x="120" y="60" style="text-decoration:overline;" >over</text>
<text x="200" y="60" style="text-decoration:line-through;" >through</text>
<text x="20" y="90" style="word-spacing: 10pt;" >more word space </text>
<text x="20" y="120" style="word-spacing: -3pt;" >less word space </text>
<text x="20" y="150" style="letter-spacing: 5pt;" >wide letter space </text>
<text x="20" y="180"
      style="letter-spacing: -6pt;" >narrow letter space </text>
</g>
128 | Chapter 9: Text

Figure 9-4. Text weight, style, decoration, and spacing
Text Alignment
The <text>  element lets you specify the starting point, but you don’t know, a priori , its
ending point. This would make it difficult to center or right-align text, were it not for
the text-anchor  property. Y ou set it to a value of start , middle , or end. For fonts that
are drawn left-to-right, these are equivalent to left, center, and right alignment. For fonts
that are drawn in other directions (see “Internationalization and Text”  on page 134), these
have a different effect. Example 9-3  shows three text strings, all starting at an x-location
of 100, but with differing values of text-anchor . A guide line is drawn to show the effect
more clearly in the result, Figure 9-5 .
Example 9-3. Use of text-anchor
http://oreillymedia.github.io/svg-essentials-examples/ch09/text_alignment.html
<g style="font-size: 14pt;" >
  <path d="M 100 10 100 100"  style="stroke: gray; fill: none;" />
  <text x="100" y="30" style="text-anchor: start" >Start</text>
  <text x="100" y="60" style="text-anchor: middle" >Middle</text>
  <text x="100" y="90" style="text-anchor: end" >End</text>
</g>
Figure 9-5. Result of using text-anchor
The <tspan> Element
Another consequence of not knowing a text string’s length in advance is that it is difficult
to construct a string with varying text attributes, such as this sentence, which switches
among italic , normal, and bold  text. If you had only the <text>  element, you’d need to
Text Alignment | 129

experiment to find where each differently styled segment of text ended in order to space
them properly. To solve this problem, SVG provides the <tspan> , or text span ele‐
ment. Analogous to the (X)HTML <span>  element, <tspan>  is a tabula rasa  that may
be embedded in text content, and upon which you may impose style changes. The
<tspan>  remembers the text position, so you don’t have to. Thus, Example 9-4 , which
produces the display in Figure 9-6 .
Example 9-4. Using tspan to change styles
<text x="10" y="30" style="font-size:12pt;" >
  Switch among
  <tspan style="font-style:italic" >italic</tspan> , normal,
  and <tspan style="font-weight:bold" >bold</tspan>  text.
</text>
Figure 9-6. Styles changed with tspan
In addition to changing presentation properties such as font size, color, weight, etc., you
can also use attributes with <tspan>  to change the positioning of individual letters or
sets of letters. If, for example, you want superscripts or subscripts, you can use the dy
attribute to offset characters within a span. The value you assign to this attribute is added
to the vertical position of the characters, and continues to affect text even outside the
span.  Negative values are allowed. A similar attribute, dx, offsets characters horizontally.
Example 9-5  uses vertical offsets to create the “falling letters” in Figure 9-7 .
Example 9-5. Using dy to change vertical positioning within text
<text x="10" y="30" style="font-size:12pt;" >
  F <tspan dy="4">a</tspan>
  <tspan dy="8">l</tspan>
  <tspan dy="12">l</tspan>
</text>
Figure 9-7. Vertical positioning with dy
If you wish to express the offsets in absolute terms rather than relative terms, use the x
and y attributes. This is handy for doing multiline runs of text.  As you will see in
“Whitespace and Text” on page 141, SVG never displays newline characters in text, so you
need to manually reset the x value for each line, and use y or dy to position it vertically.
Y ou should always use <tspan> s within a <text>  element to group related lines, not
130 | Chapter 9: Text

only to allow them to be selected as a unit, but also because it adds structure to your
document. Example 9-6  presents a verse of Edward Lear’s The Owl and the Pussycat
using <tspan>  elements with absolute x-coordinates and a mix of y and dy values.
Example 9-6. Use of absolute positioning with tspan
<text x="10" y="30" style="font-size:12pt;" >
  They dined on mince, and slices of quince,
  <tspan x="20" y="50">Which they ate with a
    runcible spoon; </tspan>
  <tspan x="10" y="70">And hand in hand, on the edge
    of the sand, </tspan>
  <tspan x="20" dy="20">They danced by the light of the moon. </tspan>
</text>
There’s no visual evidence in Figure 9-8  that all the text is in one <text>  element, but
trust us—they’re all connected.
Figure 9-8. Absolutely positioned poetry
Y ou may also rotate a letter or series of letters within a <tspan>  by using the rotate
attribute, whose value is an angle in degrees.
If you have to modify the positions of several characters, you can do it easily by speci‐
fying a series of numbers for any of the x, y, dx, dy, and rotate  attributes.  The numbers
you specified will be applied, one after another, to the characters within the <tspan> .
This is shown in Example 9-7 .
Example 9-7. Use of multiple values for dx, dy, and rotate in a text span
<text x="30" y="30" style="font-size:14pt" >It’s
<tspan dx="0 4 -3 5 -4 6"  dy="0 -3 7 3 -2 -8"
  rotate="5 10 -5 -20 0 15" >shaken</tspan> ,
not stirred.
</text>
Notice in Figure 9-9  that the effects of dx and dy persist after the <tspan>  ends. The text
after the closing </tspan>  is at the same offsets as the letter n in shaken . The text does
not return to the baseline established by the first letter in the <tspan> .
The <tspan> Element | 131

Figure 9-9. Multiple horizontal and vertical offsets
Although you can use the dy attribute to produce superscripts and subscripts, it’s easier
to use the baseline-shift  style,  as in Example 9-8 . This style property has values of
super  and sub. Y ou may also specify a length, such as 0.5em , or a percentage, which is
calculated in terms of the font size. baseline-shift ’s effects are restricted to the span
in which it occurs.
Example 9-8. Use of baseline-shift
<text x="20" y="25" style="font-size: 12pt;" >
C<tspan style="baseline-shift: sub;" >12</tspan>
H<tspan style="baseline-shift: sub;" >22</tspan>
O<tspan style="baseline-shift: sub;" >11</tspan>  (sugar)
</text>
<text x="20" y="70" style="font-size: 12pt;" >
6.02 x 10 <tspan baseline-shift= "super">23</tspan>
(Avogadro's number)
</text>
In Figure 9-10 , the subscripted numbers appear too large. In an ideal case, you’d set the
font-size  as well, but we wanted this example to concentrate on only one concept.
Figure 9-10. Subscripts and superscripts
Setting textLength
Although, as mentioned previously, there’s no a priori  way to determine the endpoint
of a segment of text, you can explicitly specify the length of text as the value of the
textLength  attribute. SVG will then fit the text into the given space. It does so by ad‐
justing the space between glyphs and leaving the glyphs themselves untouched, or it can
fit the words by adjusting both the spacing and glyph size.  If you want to adjust space
only, set the value of the lengthAdjust  to spacing  (this is the default). If you want SVG
to fit the words into a given length by adjusting both spacing and glyph size, set
lengthAdjust  to spacingAndGlyphs . Example 9-9  uses these attributes to achieve the
results of Figure 9-11 .
132 | Chapter 9: Text

Example 9-9. Use of textLength and lengthAdjust
http://oreillymedia.github.io/svg-essentials-examples/ch09/text_length.html
<g style="font-size: 14pt;" >
<path d="M 20 10 20 70 M 220 10 220 70"  style="stroke: gray;" />
<text x="20" y="30"
  textLength= "200" lengthAdjust= "spacing" >Two words </text>
<text x="20" y="60"
  textLength= "200" lengthAdjust= "spacingAndGlyphs" >Two words </text>
<text x="20" y="90">Two words
  <tspan style="font-size: 10pt;" >(normal length) </tspan></text>
<path d="M 20 100 20 170 M 100 100 100 170"  style="stroke: gray;" />
<text x="20" y="120"
  textLength= "80" lengthAdjust= "spacing" >Two words </text>
<text x="20" y="160"
  textLength= "80" lengthAdjust= "spacingAndGlyphs" >Two words </text>
</g>
Figure 9-11. Effects of varying textLength and lengthAdjust
Vertical Text
When you use SVG to create charts, graphs, or tables, you will often want labels running
down the vertical axes. One way to achieve vertically oriented text is to use a transfor‐
mation to rotate  the text 90 degrees. Another way to achieve the same effect is to change
the value of the writing-mode  style property to the value tb (meaning top to bottom).
Sometimes, though, you want the letters to appear in a vertical column with no rotation.
Example 9-10  does this by setting the glyph-orientation-vertical  property  with a
value of 0. (Its default value is 90, which is what rotates top-to-bottom text 90 degrees.)
In Figure 9-12 , this setting tends to display the inter-letter spacing as unnaturally
large. Setting a small negative value for letter-spacing  solves this problem.
Example 9-10. Producing vertical text
<text x="10" y="20" transform= "rotate(90,10,20)" >Rotated 90 </text>
<text x="50" y="20" style="writing-mode: tb;" >Writing Mode tb </text>
Vertical Text | 133

<text x="90" y="20" style="writing-mode: tb;
   glyph-orientation-vertical: 0;" >Vertical zero </text>
If you have been trying out these examples, you may have noticed that a number of
features (such as baseline-shift , spacing, and vertical text) are poorly supported in
some browsers. It’s always a good idea to test out your designs in any SVG viewers that
you want to support.
Figure 9-12. Vertical text
Internationalization and Text
If your graphic has text that needs to be translated into multiple languages,  SVG’s sup‐
port for Unicode and ability to display many languages in a single document will save
you the trouble of creating separate documents for each language.
Unicode and Bidirectionality
XML is based on the Unicode standard (fully documented at the Unicode Consortium’s
website ). This lets text display in any language the underlying viewer software can dis‐
play, as you can see in Figure 9-13 . Some languages such as Arabic and Hebrew are
written right to left, so when text in these languages is mixed with text written left to
right, as English is, the text is bidirectional, or bidi for short. The system software knows
which characters go in which direction and works out their positions accordingly.
Example 9-11  also overrides the implicit directionality of a segment of text by setting
its direction  style property to rtl, which stands for right- to-left. If you wish to change
the direction of Hebrew or Arabic text, set it to ltr, which is left-to-right. Y ou must also
explicitly override the underlying Unicode bidirectionality algorithm by setting the
unicode-bidi  style property to bidi-override .
Example 9-11. International text using Unicode
<g style="font-size: 14pt;" >
<text x="10" y="30">Greek: </text>
134 | Chapter 9: Text

<text x="100" y="30">
  αβγδε
</text>
<text x="10" y="50">Russian: </text>
<text x="100" y="50">
  абвгд
</text>
<text x="10" y="70">Hebrew:</text>
<text x="100" y="70">
    אבגדה( written right to left)
</text>
<text x="10" y="90">Arabic:</text>
<text x="100" y="90">
  ا ب ج د( written right to left)
</text>
<text x="10" y="130">
  This is
    <tspan style="direction: rtl; unicode-bidi: bidi-override;
      font-weight: bold;" >right-to-left </tspan>
  writing.
</text>
</g>
Figure 9-13. Multilingual text
The <switch> Element
The ability to display multiple languages in a single document is useful for such things
as a brochure for an event that receives international visitors. Sometimes, though, you
would like to create one document with content in two languages—say, Spanish and
Russian. People viewing the document with Spanish system software would see the
Spanish text, and Russians would see Russian text.
SVG provides this capability with the <switch>  element. This element searches through
all its children until it finds one whose systemLanguage  attribute  has a value that
Internationalization and Text | 135

1.The <switch>  element can also be used for other tests; in “Foreign Objects in SVG” on page 20 , we showed
how to use a switch to test for support for specific features. If you use multiple test attributes on the children
of a <switch>  element, all of them must match for the content to be displayed.matches the language the user has chosen in the viewer software’s preferences.1 The
value of systemLanguage  is a single value or comma-separated list of language names.
A language name is either a two-letter language code, such as ru for Russian, or a lan‐
guage code followed by a country code, which specifies a sublanguage. For instance,
fr-CA  denotes Canadian French, while fr-CH  denotes Swiss French.
Once a matching child element is found, all its children will be displayed. All the other
children of the <switch>  will be bypassed. Example 9-12  shows text in UK English, US
English, Spanish, and Russian. A match of language code alone is considered a match,
and country codes are used only to “break a tie, ” so the text for UK English must come
first.
Example 9-12. Use of the switch element
<circle cx="40" cy="60" r="20" style="fill: none; stroke: black;" />
<g font-size= "12pt">
  <switch>
    <g systemLanguage= "en-UK">
      <text x="10" y="30">A circle </text>
      <text x="10" y="100">without colour. </text>
    </g>
    <g systemLanguage= "en">
      <text x="10" y="30">A circle </text>
      <text x="10" y="100">without color. </text>
    </g>
    <g systemLanguage= "es">
      <text x="10" y="30">Un círculo </text>
      <text x="10" y="100">sin color. </text>
    </g>
    <g systemLanguage= "ru">
      <text x="10" y="30">Круг</text>
      <text x="10" y="100">без света. </text>
    </g>
  </switch>
</g>
Figure 9-14  is a combination of screenshots taken with the language set to each of the
choices in Example 9-12 . Y ou should normally provide a fallback (a group without any
systemLanguage  attribute, as the last element in the <switch>  block) to display some‐
thing  in case none of the languages match. Ideally, you would like to give users a way of
selecting a language from the ones you have available.
136 | Chapter 9: Text

Figure 9-14. Combined screenshots as seen with different language preferences
Using a Custom Font
Sometimes you need special symbols that are not represented in Unicode, or you want
a subset of the Unicode characters without having to install an entire font.  An example
is Figure 9-15 , which needs only a few of the over 2,000 Korean syllables. Y ou can create
a custom font as described in Appendix E  and give its starting <font>  tag a unique id.
Here is the relevant portion of a file containing six of the Korean syllables exported from
the Batang TrueType font. The file is called kfont.svg :
<font id="kfont-defn"  horiz-adv-x= "989" vert-adv-y= "1200"
  vert-origin-y= "0">
  <font-face  font-family= "bakbatn"
    units-per-em= "1000"
    panose-1= "2 3 6 0 0 1 1 1 1 1"
    ascent="800" descent= "-200" baseline= "0" />
    <missing-glyph  horiz-adv-x= "500" />
    <!-- glyph definitions go here -->
  </font-face>
</font>
Figure 9-15. Korean syllables from an external font
Once that is done, Example 9-13  can reference the font in that external file. For the sake
of consistency, the value of the font-family  you use in this SVG file should match the
value in the external file.
Example 9-13. Use of an external font
<defs>
  <font-face  font-family= "bakbatn" >
    <font-face-src>
      <font-face-uri  xlink:href= "kfont.svg#kfont-defn" >
        <font-face-format  string="svg" />
Internationalization and Text | 137

      </font-face-uri>
    </font-face-src>
  </font-face>
</defs>
<text font-size= "28" x="20" y="40"
  style="font-family: bakbatn, serif;" >
    서울 - 대한민국
</text>
SVG fonts are currently not supported in Internet Explorer brows‐
ers (including  version  11) or Firefox  browsers  (version  30). For these
browsers, you can include a second <font-face-src>  element, with
the URI of an alternate font file in a different format. Alternatively,
you can use a <font-face-name>  element  with  the single  attribute
name , containing the name of a system font to use. All of these ele‐
ments  have  the same  interpretation  as their  equivalent  CSS font-
face properties.
If none  of your  specified  fonts  can be used,  the browser  will try to
find any font on its system that can display the Unicode characters
used in the text.
Text on a Path
Text does not have to go in a straight horizontal or vertical line. It can follow any arbitrary
path; simply enclose the text in a <textPath>  element that uses  an xlink:href  attribute
to refer to a previously defined <path>  element.  Letters will be rotated to stand “per‐
pendicular” to the curve (i.e., the letter’s baseline will be tangent to the curve). Text along
a gently curving and continuous path is easier to read than text that follows a sharply
angled or discontinuous path.
Referencing  a <path>  within  a <textPath>  element  does  not auto‐
matically  display  that path.  In Example  9-14 , the <path> s are de‐
fined  in a <defs>  section,  so they  wouldn’t  normally  be displayed.
The example has <use>  elements to draw the visible lines.
Example 9-14. Examples of textPath
http://oreillymedia.github.io/svg-essentials-examples/ch09/text_path.html
<defs>
<path id="curvepath"
    d="M30 40 C 50 10, 70 10, 120 40 S 150 0, 200 40"
    style="stroke: gray; fill: none;" />
<path id="round-corner"
138 | Chapter 9: Text

    d="M250 30 L 300 30 A 30 30 0 0 1 330 60 L 330 110"
    style="stroke: gray; fill: none;" />
<path id="sharp-corner"
    d="M 30 110 100 110 100 160"
    style="stroke: gray; fill: none;" />
<path id="discontinuous"
    d="M 150 110 A 40 30 0 1 0 230 110 M 250 110 270 140"
    style="stroke: gray; fill: none;" />
</defs>
<g style="font-family: 'Liberation Sans';
  font-size: 10pt;" >
  <use xlink:href= "#curvepath" />
  <text>
    <textPath  xlink:href= "#curvepath" >
    Following a cubic Bézier curve.
    </textPath>
  </text>
  <use xlink:href= "#round-corner" />
  <text>
    <textPath  xlink:href= "#round-corner" >
    Going 'round the bend
    </textPath>
  </text>
  <use xlink:href= "#sharp-corner" />
  <text>
    <textPath  xlink:href= "#sharp-corner" >
    Making a quick turn
    </textPath>
  </text>
  <use xlink:href= "#discontinuous" />
  <text>
      <textPath  xlink:href= "#discontinuous" >
      Text along a broken path
      </textPath>
  </text>
</g>
Example 9-14  produces Figure 9-16 ; Figure 9-17  shows you what it looks like if we draw
the text without the underlying paths.
Text on a Path | 139

Figure 9-16. Text along a path (with paths shown)
Figure 9-17. Text along a path (paths not shown)
Y ou may adjust the beginning point of the text along its path by setting the startOffset
attribute to a percentage or to a length. For example, startOffset="25%"  will start the
text one-fourth of the distance along the path, and startOffset="30"  will start the text
at a distance of 30 user units from the beginning of the path.  If you wish to center text
on a path, as in Example 9-15 , set textanchor="middle"  on the <text>  element and
startOffset="50%"  on the <textPath>  element. Text falling beyond the ends of the
path will not be displayed, as shown in the left half of Figure 9-18 .
Example 9-15. Text length and startOffset
http://oreillymedia.github.io/svg-essentials-examples/ch09/start_offset.html
<defs>
  <path id="short-corner"  transform= "translate(40,40)"
      d="M0 0 L 30 0 A 30 30 0 0 1 60 30 L 60 60"
    style="stroke: gray; fill: none;" />
  <path id="long-corner"  transform= "translate(140,40)"
      d="M0 0 L 50 0 A 30 30 0 0 1 80 30 L 80 80"
    style="stroke: gray; fill: none;" />
</defs>
<g style="font-family: 'Liberation Sans'; font-size: 12pt" >
  <use xlink:href= "#short-corner" />
  <text>
    <textPath  xlink:href= "#short-corner" >
      This text is too long for the path.
      </textPath>
140 | Chapter 9: Text

  </text>
  <use xlink:href= "#long-corner" />
  <text style="text-anchor: middle;" >
    <textPath  xlink:href= "#long-corner"  startOffset= "50%">
      centered
    </textPath>
  </text>
</g>
Figure 9-18. Effects of long text and startOffset
Whitespace and Text
Y ou may change the way SVG handles whitespace (blanks, tabs, and newline characters)
within text by changing the value of the xml:space  attribute. If you specify a value of
default  (which, coincidentally, is the default value), SVG will handle whitespace as
follows:
•Remove all newline characters
•Change all tabs to blanks
•Remove all leading and trailing blanks
•Change any run of intermediate blanks to a single blank
Thus, this string, where \t represents a tab and \n represents a newline, and an under‐
score represents a blank:
   \n\n___abc_\t\t_def_\n\n__ghi
will render as:
   abc_def_ghi
The other setting of xml:space  is preserve . With this setting, SVG will simply convert
all newline and tab characters to blanks, and then display the result, including leading
and trailing blanks. The same text:
   \n\n___abc_\t\t_def_\n\n__ghi
then renders as:
   _____abc____def_____ghi
Whitespace and Text | 141

SVG’s  handling  of whitespace  is not like that of HTML.  SVG’s  de‐
fault  handling  eliminates  all newlines;  HTML  changes  internal  new‐
lines  to a space.  SVG’s  preserve  method  converts  newlines  to blanks;
HTML ’s  <pre>  element  does  not. There  is no newline  in SVG  1.0;
this bothers  people  until  they  realize  that SVG  text is oriented  to‐
ward graphic display, not textual content (as in XHTML).
Case Study: Adding Text to a Graphic
Figure 9-19  adds Korean and English text to the Korean national symbol shown in
Figure 7-6 . The text is centered along an elliptical path. The additional SVG in
Example 9-16  is shown in boldface.
Example 9-16. Text case study
<defs>
  <font-face  font-family= "bakbatn" >
    <font-face-src>
      <font-face-uri  xlink:href= "kfont.svg#kfont-defn" >
        <font-face-format  string="svg" />
      </font-face-uri>
    </font-face-src>
  </font-face>
  <path id="upper-curve"  d="M -8 154 A 162 130 0 1 1 316 154" />
  <path id="lower-curve"  d="M -21 154 A 175 140 0 1 0 329 154" />
</defs>
<ellipse  cx="154" cy="154" rx="150" ry="120" style="fill: #999999;" />
<ellipse  cx="152" cy="152" rx="150" ry="120" style="fill: #cceeff;" />
<!--
  large light red semicircle fills upper half,
  followed by small light red semicircle that dips into
  lower-left half of symbol
-->
<path d="M 302 152 A 150 120, 0, 1, 0, 2 152
  A 75 60, 0, 1, 0, 152 152"  style="fill: #ffcccc;" />
<!--
  light blue semicircle rises
  into upper-right half of symbol
-->
<path d="M 152 152 A 75 60, 0, 1, 1, 302 152"
  style="fill: #cceeff;" />
<text font-family= "bakbatn, serif"
  style="font-size: 24pt; text-anchor: middle;" >
  <textPath  xlink:href= "#upper-curve"  startOffset= "50%">
    서울 - 대한민국
142 | Chapter 9: Text

  </textPath>
</text>
<text style="font-size: 14pt; text-anchor: middle;" >
  <textPath  xlink:href= "#lower-curve"  startOffset= "50%">
  Seoul - Republic of Korea
  </textPath>
</text>
Figure 9-19. Text along path added to graphic
Case Study: Adding Text to a Graphic | 143

1.Y ou can change this behavior by setting the overflow  style property to visible .CHAPTER 10
Clipping and Masking
Sometimes you don’t want to see an entire picture. For example, you might wish to draw
a picture as though it were seen through binoculars or a keyhole; everything outside the
boundary of the eyepieces or keyhole will be invisible. Or, you might want to set a mood
by showing an image as though viewed through a translucent curtain. SVG accomplishes
such effects with clipping and masking.
Clipping to a Path
When you create an SVG document, you establish its viewport by specifying the width
and height of the area you’re interested in.  This becomes by default your clipping area;
anything drawn outside these limits will not be displayed.1 Y ou can establish a clipping
area of your own with the <clipPath>  element.
Here’s the simplest case: establishing a rectangular clip path. Inside the <clipPath>
element will be the <rect>  you want to clip to. The rectangle itself is not displayed; we
only love it for its coordinates. Thus, you are free to add any fill or stroke styles you wish
to the elements within the <clipPath> . On the object to be clipped, you add a clip-path
style property whose value references the <clipPath>  element. Note that the property
is hyphenated and not capitalized; the element is capitalized and not hyphenated. In
Example 10-1 , the object being clipped is a small version of the cat picture from Chap‐
ter 1 . The result is in Figure 10-1 .
Example 10-1. Clipping to a rectangular path
http://oreillymedia.github.io/svg-essentials-examples/ch10/clip_path.html
<svg width="350" height="200" viewBox= "0 0 350 200"
  xmlns="http://www.w3.org/2000/svg"
145

  xmlns:xlink= "http://www.w3.org/1999/xlink" >
  <defs>
  <clipPath  id="rectClip" >
    <rect id="rect1" x="15" y="15"
      width="40" height="45"
      style="stroke: gray; fill: none;" />
  </clipPath>
  </defs>
  <!-- clip to rectangle -->
  <use xlink:href= "minicat.svg#cat"
    style="clip-path: url(#rectClip);" />
  <!--
    for reference, show entire picture
    with clipping area outlined
  -->
  <g transform= "translate(100,0)" >
    <use xlink:href= "#rect1" />    <!-- show clip rectangle -->
    <use xlink:href= "minicat.svg#cat" />
  </g>
</svg>
Figure 10-1. Simple rectangular clipping
As the name <clipPath>  implies, you can clip to any arbitrary path. Indeed, the
<clipPath>  element can contain any number of basic shapes, <path>  elements, or
<text>  elements. Example 10-2  shows a group of shapes clipped to a curved path and
the same group of shapes clipped by text.
Example 10-2. Complex clip paths
http://oreillymedia.github.io/svg-essentials-examples/ch10/complex_clip_path.html
<defs>
  <clipPath  id="curveClip" >
    <path id="curve1"
      d="M5 55 C 25 5, 45 -25, 75 55, 85 85, 20 105, 40 55 Z"
      style="stroke: black; fill: none;" />
  </clipPath>
  <clipPath  id="textClip" >
    <text id="text1" x="20" y="20" transform= "rotate(60)"
      style="font-family: 'Liberation Sans';
        font-size: 48pt; stroke: black; fill: none;" >
146 | Chapter 10: Clipping and Masking

      CLIP
    </text>
  </clipPath>
  <g id="shapes" >
    <rect x="0" y="50" width="90" height="60" style="fill: #999;" />
    <circle cx="25" cy="25" r="25" style="fill: #666;" />
    <polygon  points="30 0 80 0 80 100"  style="fill: #ccc;" />
  </g>
</defs>
<!-- draw with curved clip-path -->
<use xlink:href= "#shapes"  style="clip-path: url(#curveClip);"  />
<!-- draw with text as clip-path -->
<use transform= "translate(100, 0)"
  xlink:href= "#shapes"  style="clip-path: url(#textClip);" />
<g transform= "translate(0, 150)" >
  <use xlink:href= "#shapes" />
  <use xlink:href= "#curve1" />   <!-- show clip path -->
</g>
<g transform= "translate(100,150)" >
  <use xlink:href= "#shapes" />
  <use xlink:href= "#text1" />
</g>
To help you see the areas better, the preceding SVG draws the clipping path above the
entire figure; you see this in the right half of Figure 10-2 .
Figure 10-2. Complex path clipping
The coordinates for the preceding clip paths have been specified in user coordinates. If
you wish to express coordinates in terms of the object bounding box, then set
Clipping to a Path | 147

clipPathUnits  to objectBoundingBox  (the default is userSpaceOnUse ). Example 10-3
uses a clip path that will produce a circular (or oval) window on any object it’s
applied to.
Example 10-3. clipPathUnits using objectBoundingBox
<defs>
 <clipPath  id="circularPath"  clipPathUnits= "objectBoundingBox" >
   <circle cx="0.5" cy="0.5" r="0.5"/>
 </clipPath>
  <g id="shapes" >
    <rect x="0" y="50" width="100" height="50" style="fill: #999;" />
    <circle cx="25" cy="25" r="25" style="fill: #666;" />
    <polygon  points="30 0 80 0 80 100"  style="fill: #ccc;" />
  </g>
  <g id="words">
    <text  x="0"  y="19" style="font-family: 'Liberation Sans';
      font-size: 14pt;" >
    <tspan x="0"  y="19">If you have form'd a circle </tspan>
    <tspan x="12" y="35">to go into, </tspan>
    <tspan x="0"  y="51">Go into it yourself </tspan>
    <tspan x="12" y="67">and see how you would do. </tspan>
    <tspan x="50" y="87">&#8212;William Blake </tspan>
    </text>
  </g>
</defs>
<use xlink:href= "#shapes"  style="clip-path: url(#circularPath);"  />
<use xlink:href= "#words"  transform= "translate(110,0)"
    style="clip-path: url(#circularPath);" />
In Figure 10-3 , the geometric figures happen to have a square bounding box, so the
clipping appears circular. The text is bounded by a rectangular area, so the clipping area
appears to be an oval.
For <marker> , <symbol> , and <svg>  elements,  which  define  their  own
viewport, you can also clip content to the viewport by using a style of
overflow:  hidden . However,  if the content  has a meet  value  for
preserveAspectRatio , the viewport may be larger than the viewBox .
To clip to the viewBox , create  a <clipPath>  element  containing  a
rectangle  that matches  the minimum  x, minimum  y, width,  and height
of the viewBox .
148 | Chapter 10: Clipping and Masking

Figure 10-3. Use of a circular/oval clipping path
Masking
A mask in SVG is the exact  opposite of the mask you wear to a costume party. With a
costume party mask, the parts that are opaque hide your face; the parts that are trans‐
lucent let people see your face dimly, and the holes (which are transparent) let people
see your face clearly. An SVG mask, on the other hand, transfers its transparency to the
object it masks. Where the mask is opaque, the pixels of the masked object are opaque.
Where the mask is translucent, so is the object, and the transparent parts of the mask
make the corresponding parts of the masked object invisible.
Y ou use the <mask>  element to create a mask.  Y ou may specify the mask’s dimensions
with the x, y, width , and height  attributes. These dimensions are in terms of the masked
objectBoundingBox . If you want the dimensions to be in terms of user space coordi‐
nates, set maskUnits  to userSpaceOnUse .
Between the beginning <mask>  and ending </mask>  tags are any basic shapes, text, im‐
ages, or paths you wish to use as the mask. The coordinates on these elements are
expressed in user coordinate space by default. If you wish to use the object bounding
box for the contents of the mask, set maskContentUnits  to objectBoundingBox . (The
default is userSpaceOnUse .)
The question then becomes: how does SVG determine the transparency, or alpha value,
of the mask? We know each pixel is described by four values: its red, green, and blue
color value, and its opacity. While at first glance it would seem logical to use only the
opacity value, SVG decides to use all the information available to it rather than throwing
away three-fourths of a pixel’s information. SVG uses this formula:
(0.2125 *  red value  + 0.7154 *  green value  + 0.0721 *  blue value ) * opacity value
where all of the values are floating-point numbers in the range 0 to 1. Y ou may be
surprised that the proportions aren’t equal, but if you look at fully saturated red, green,
and blue, the green appears to be the brightest, red darker, and blue the darkest. (Y ou
can see this in Figure 10-4 .) The darker the color, the smaller the resulting alpha value
will be, and the less opaque the masked object will be.
Masking | 149

Figure 10-4. Effect of mask color values on transparency
Figure 10-4  was drawn with Example 10-4 , which creates black text and a black circle
masked by a totally opaque red, green, blue, and white square. The text and circle are
grouped together, and the group uses a mask  style property to reference the appropriate
mask.  The yellow horizontal bars in the background show you that, indeed, the text and
circles are partially transparent.
Example 10-4. Masking with opaque colors
<defs>
  <mask id="redmask"  x="0" y="0" width="1" height="1"
    maskContentUnits= "objectBoundingBox" >
    <rect x="0" y="0" width="1" height="1" style="fill: #f00;" />
  </mask>
  <mask id="greenmask"  x="0" y="0" width="1" height="1"
    maskContentUnits= "objectBoundingBox" >
    <rect x="0" y="0" width="1" height="1" style="fill: #0f0;" />
  </mask>
  <mask id="bluemask"  x="0" y="0" width="1" height="1"
    maskContentUnits= "objectBoundingBox" >
    <rect x="0" y="0" width="1" height="1" style="fill: #00f;" />
  </mask>
  <mask id="whitemask"  x="0" y="0" width="1" height="1"
    maskContentUnits= "objectBoundingBox" >
    <rect x="0" y="0" width="1" height="1" style="fill: #fff;" />
  </mask>
</defs>
<!-- display the colors to show relative brightness (luminance) -->
<rect x="10" y="10" width="50" height="50" style="fill: #f00;" />
<rect x="70" y="10" width="50" height="50" style="fill: #0f0;" />
<rect x="130" y="10" width="50" height="50" style="fill: #00f;" />
<rect x="190" y="10" width="50" height="50"
    style="fill: #fff; stroke: black;" />
<!-- background content to show transparency -->
<rect x="10" y="72" width="250" height="5" style="fill: yellow" />
<rect x="10" y="112" width="250" height="5" style="fill: yellow" />
150 | Chapter 10: Clipping and Masking

<g style="mask: url(#redmask);
    font-size: 14pt; text-anchor: middle;" >
  <circle cx="35" cy="115" r="25"  style="fill: black;" />
  <text x="35" y="80">Red</text>
</g>
<g style="mask: url(#greenmask);
    font-size: 14pt; text-anchor: middle;" >
  <circle cx="95" cy="115" r="25" style="fill: black;" />
  <text x="95" y="80">Green</text>
</g>
<g style="mask: url(#bluemask);
    font-size: 14pt; text-anchor: middle;" >
  <circle cx="155" cy="115" r="25" style="fill: black;" />
  <text x="155" y="80">Blue</text>
</g>
<g style="mask: url(#whitemask);
    font-size: 14pt; text-anchor: middle;" >
  <circle cx="215" cy="115" r="25" style="fill: black;" />
  <text x="215" y="80">White</text>
</g>
Figuring out the interaction between color, opacity, and final alpha value is not exactly
intuitive. If you fill and/or stroke the mask contents in white, the “color factor” adds up
to 1.0, and the opacity will then be the only factor that controls the mask’s alpha value.
Example 10-5  is written this way, and the result is in Figure 10-5 .
Figure 10-5. Alpha value equal to opacity
Example 10-5. Mask alpha using opacity only
http://oreillymedia.github.io/svg-essentials-examples/ch10/alpha_opacity_mask.html
<defs>
<mask id="fullmask"  x="0" y="0" width="1" height="1"
    maskContentUnits= "objectBoundingBox" >
    <rect x="0" y="0" width="1" height="1"
        style="fill-opacity: 1.0; fill: white;" />
</mask>
<mask id="three-fourths"  x="0" y="0" width="1" height="1"
    maskContentUnits= "objectBoundingBox" >
    <rect x="0" y="0" width="1" height="1"
        style="fill-opacity: 0.75; fill: white;" />
Masking | 151

</mask>
<mask id="one-half"  x="0" y="0" width="1" height="1"
    maskContentUnits= "objectBoundingBox" >
    <rect x="0" y="0" width="1" height="1"
        style="fill-opacity: 0.5; fill: white;" />
</mask>
<mask id="one-fourth"  x="0" y="0" width="1" height="1"
    maskContentUnits= "objectBoundingBox" >
    <rect x="0" y="0" width="1" height="1"
        style="fill-opacity: 0.25; fill: white;" />
</mask>
</defs>
<g style="font-size: 14pt; text-anchor:middle; fill:black;" >
    <g style="mask: url(#fullmask);" >
    <circle cx="35" cy="35" r="25"/>
    <text x="35" y="80">100%</text>
    </g>
    <g style="mask: url(#three-fourths);" >
    <circle cx="95" cy="35" r="25"/>
    <text x="95" y="80">75%</text>
    </g>
    <g style="mask: url(#one-half);" >
    <circle cx="155" cy="35" r="25"/>
    <text x="155" y="80">50%</text>
    </g>
    <g style="mask: url(#one-fourth);" >
    <circle cx="215" cy="35" r="25"/>
    <text x="215" y="80">25%</text>
    </g>
</g>
Case Study: Masking a Graphic
Example 10-6  adds a JPG image to the graphic that was constructed in “Case Study:
Adding Text to a Graphic”  on page 142 . As you can see in Figure 10-6  (reduced to save
space), the image obscures the curve inside the main ellipse, and the blue sky intrudes
horribly on the pale red section.
Example 10-6. Unmasked <image> inside a graphic
<defs>
  <font-face  font-family= "bakbatn" >
    <font-face-src>
      <font-face-uri  xlink:href= "kfont.svg#kfont-defn" />
    </font-face-src>
152 | Chapter 10: Clipping and Masking

  </font-face>
</defs>
<!-- draws ellipse and text -->
<use xlink:href= "ksymbol.svg#ksymbol" />
<image xlink:href= "kwanghwamun.jpg"  x="72" y="92"
    width="160" height="120"/>
Figure 10-6. Unmasked <image> inside a graphic
The solution is to fade out the edges of the picture, which is easily done by using a radial
gradient as a mask.  Here’s the code to be added to the <defs>  section of the document:
<radialGradient  id="fade">
  <stop offset="0%" style="stop-color: white; stop-opacity: 1.0;" />
  <stop offset="85%" style="stop-color: white; stop-opacity: 0.5;" />
  <stop offset="100%" style="stop-color: white; stop-opacity: 0.0;" />
</radialGradient>
<mask id="fademask"  maskContentUnits= "objectBoundingBox" >
  <rect x="0" y="0" width="1" height="1"
    style="fill: url(#fade);" />
</mask>
Then add a mask reference to the <image>  tag, resulting in Figure 10-7 :
<image xlink:href= "kwanghwamun.jpg"  x="72" y="92"
  width="160" height="120"
  style="mask: url(#fademask);" />
Case Study: Masking a Graphic | 153

Figure 10-7. Masked image
Using less of the picture can substantially improve the graphic as a whole. 
154 | Chapter 10: Clipping and Masking