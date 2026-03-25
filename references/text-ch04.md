Chapter 1.
 Understanding Text Layout
The history of human writing includes etchings in stone and wood, impressions in clay tablets, ink applied with brushes, and ink applied with quill pens. The different means of writing have each influenced the visual appearance of the text that results.

As the technology used to create writing changed, first with the printing press, then the typewriter, then computer displays, so has the written form. In addition, geopolitical history has had its influence on writing, spreading scripts from one part of the world to another, where the writing system is adapted to different spoken languages.

This chapter reviews the core concepts common to text layout in all web documents. It starts with an introduction to the terminology used to describe letters and writing systems. It then looks at how text content, fonts, and text-rendering software combine to create text on computer displays. In particular, we focus on how markup languages like HTML, XML, and SVG interact with styling rules in CSS to define text layout within web browsers. Finally, we review the main features of SVG text layout, as a big-picture introduction to the rest of the book.
The Language of Text
When describing written text, there are some important distinctions to make between the concepts of written language and its execution in physical form. If you are going to make sense of a book about text, you need to understand the words we use to describe the words we write.

Text is a physical embodiment of language. A *language*
 is a system of verbal or written communication whose practitioners can mostly understand one another. Written languages can be classified according to the script (or scripts) used to display them; for example, all the languages in Figure 1-1
 use the Latin alphabet.

A *script*
 is a writing system used by one or more languages. The written symbols used in a script may be phonetic, where each symbol represents a sound. This includes alphabets, where symbols represent distinct consonant and/or vowel sounds, and also syllabaries, where symbols represent entire syllables. Other scripts are ideographic, where each symbol represents an entire word or concept.

The Latin script used to write English is also used by most Western European languages, among others. Someone fluent in English would recognize the letters used to write Gaelic or Vietnamese, even if the meaning of the text was impenetrable. Nonetheless, the division between scripts and languages is not always clear-cut; the complete modern Latin script used in those languages — and French, German, Finnish, and many more — includes special characters and accents rarely used in English.

Some languages are written in multiple scripts, as alternatives or in combination.
 For example, Japanese is written with four different scripts:

- Kanji (ideographic characters similar to those used in Chinese and Korean)

- Hiragana (phonetic characters used to indicate words by their pronunciation or to express grammatical variations of kanji ideographs)

- Katakana (a distinct but related set of phonetic characters, mostly used for words of foreign origin and technical terms)

- Rōmaji (phonetic spelling using Latin — or Roman — letters, used for inputting text to computers or for some words adopted from European languages)

Most Japanese documents use kanji ideographs combined with kana (hiragana and katakana) syllables; Latin characters, however, are often integrated for special symbols, as demonstrated in the handwritten fishmarket sign shown in Figure 1-2
 .

Some characters, such as numeric digits and punctuation, are used in multiple scripts. On the other hand, some Latin letters look quite similar to letters in Greek or Cyrillic scripts, but they are not directly interchangeable, and may be associated with quite different sounds.

The *character*
 is the basic unit within the script. Phonetic letters are characters, ideographs are characters, but so are digits, punctuation marks, and the funny little faces called emoticons or emoji.

A character is a conceptual representation, independent of its specific presentation on screen or paper. In contrast, a *glyph*
 is the visual representation of the letter, digit, or symbol in a particular writing style. If you think of the other meaning of the word character — characters in a story or play — the glyph would be the actor who brings that character to life.

Glyphs can vary quite significantly depending on the way the text is formed: imagine what the paragraph you’re reading would look like written in a school child’s pencil, a calligrapher’s fountain pen, or a medieval monk’s Gothic brush strokes. Or if your imagination is not that powerful, consider Figure 1-3
 , which uses computer fonts to create the same contrast. The shapes of the glyphs are very different from one line of text to the next, but the meaning of the characters is the same.

Even within a given writing system and style, the correct glyph for a character sometimes depends on the language used, adjacent characters, or the position within a word, so there can be multiple glyphs per character. In other cases, multiple characters are represented by the same glyph, such as the minus sign and the hyphen. 
 Some characters are drawn by combining multiple glyphs (e.g., accented letters), while some sequences of characters are replaced by a combined glyph (known as a *ligature*
 ). In some cases, ligature substitutions are a standard feature of how the language is written, required for effective communication. In other cases, these are optional stylistic effects.

A *typeface*
 or *font-face*
 is a specific collection of glyphs that have a consistent appearance. Many fonts only provide glyphs for characters in a specific script, but some try to provide a consistent appearance — as much as possible — across many different scripts. Nonetheless, many typographic traditions rely on inconsistencies in glyph appearance to express the structure of a complex text document, by distinguishing different sequences of text. 
 A *font family*
 is a set of related typefaces that differ in certain stylistic features but have a harmonious appearance such that they could be used effectively together.

The faces within a family may vary according to their weight (boldness), style (e.g., italics), spacing and proportions, or other features. Figure 1-4
 uses four different font families for Thai text to demonstrate how the overall appearance and proportions of the family are preserved between faces with different styles and weights.

In traditional typography — that is, typography based on arranging metal type in a printing press — a *font*
 consists of a specific typeface at a specific size. 
 Most modern digital fonts, however, use vector graphics to define a scalable shape. A single font file can be adapted to any size (although many look better at larger sizes and a few are better when small) so the file is technically a font-face file. However, it is still useful to distinguish between the typeface as a design and the font file as an implementation in a particular file format.
Note
Although most font formats only describe a single typeface per file, a few formats can define multiple faces of a font family within a single file.

Converting characters to glyphs is only the first step in text layout. Glyphs must be arranged on a page in a particular logical order to convey information. Greek, Latin, and Cyrillic scripts arrange characters in horizontal lines, left to right, as do many Indic scripts. Other scripts are written right to left, particularly Middle Eastern scripts such as Arabic and Hebrew. A few languages (primarily the Asian ideographic scripts) are written top to bottom in traditional or formal documents. Each script and language has standards for how sequences of text — which may or may not be grouped into distinct words — and associated punctuation should be arranged into lines for optimal readability.
Text Layout on the Web
Regardless of how a writing system evolved — from chiseled marble or delicate brush strokes — rendering it on a computer display involves three sources of data interacting:

- The character data, defining the text to be displayed and maybe
 additional details about the language used, the significance of certain sections of text, and how they should be styled

- 
 The font data, defining the glyphs to use for each character and maybe
 additional details on how to adjust the shape for better
 spacing, clearer rendering at low resolution, or ligature substitutions

- The text layout software’s rules for selecting and arranging font glyphs to match given character data, including interpreting text in different scripts and languages, how to rearrange characters from different scripts, how to identify word breaks, how to space words on a page, and many more possibilities depending on the complexity of the software

For web documents, the character data is contained in the document markup or inserted into the document object model (DOM) by a script. The font data may be accessed from the user’s operating system or downloaded as a supplementary resource; CSS style rules indicate which fonts should be used. The web browser (possibly aided by operating system software) is responsible for putting it all together, taking cues from the markup structure and the style rules provided by the web page author.
Character Data
Character data is a description of text in a form that the software may manipulate. The data may be derived from the user’s keystrokes, retrieved from a file, received from a web server, or generated by a software algorithm.

Character data can even be created by character-recognition software from an image of written text or the user’s movement on a tablet. However, without that interpretive step — translating the shapes of glyphs into corresponding characters — an image of text is not character data. Software cannot rearrange the text, display it in a different font, or read it aloud through a screen reader unless it can match that visual appearance to a standard representation of the character data in digital form.

Digital representations of text (i.e., computer files) use an *encoding*
 scheme to represent characters with binary data. Originally, there were separate encodings for each script, but in the late 1990s, *Unicode*
 started to change that. Unicode aims to describe all scripts in use — and many archaic ones — with a consistent encoding scheme. It’s not there yet, and new characters are added every year, but it is a vast improvement over the days of incompatible encoding systems for every language.

Unicode, however, isn’t a single character encoding; it is many. Unicode assigns a unique numerical *code point*
 to each character, but allows for multiple ways of representing that code point in binary data. 
 
 
 Currently, the most common Unicode encodings vary according to how large a block of binary data is by default allocated to store the code point for each character: UTF-8 uses 8 bits (1 byte) per block, UTF-16 uses 16 bits (2 bytes).
1
Note
Characters that require more than one block start with flags that indicate how many blocks of data must be combined to get the correct encoding. In this way, any Unicode character can be represented in a UTF-8 file.

Character encodings are usually hidden from the user in file metadata or operating system settings. On the Web, however, where information is transmitted between computers with different operating systems and default languages, encodings must always be clearly defined. 
 The HyperText Transfer Protocol (HTTP), used to pass web documents from servers to browsers, allows character encoding to be declared as part of the file’s content type. Although this is the preferred approach, most document formats used on the Web also allow you to declare an encoding in the file itself.

In HTML and XML markup files, the character encoding can be declared using markup tags at the top of the file. This is possible because most character encodings use the same binary representation for the basic characters used in the markup syntax.

In HTML 5, the encoding is indicated with a <meta>
 element that has a charset
 attribute, like the following:

<html>

    <head>

        <meta

 charset=

"UTF-8"

 />


In older versions of HTML, the http-equiv
 meta element was used to substitute for the HTTP header declaring the character set:

<meta

 http-equiv=

"Content-Type"

      content=

"text/html; charset=UTF-8"

>


Whichever format is used, the declaration should appear as early as possible in the file.

In XML documents, including standalone SVG documents, character encoding is indicated at the very start of the text markup, with a processing instruction such as the following:

<?xml version="1.0" encoding='UTF-8'?>


If the XML declaration is included, the version number is mandatory, and should usually be "1.0"
 for SVG. XML version 1.1 has greater support for non-Latin characters in element id
 attributes and tag names, but these may not be supported in many SVG viewers.

For XML (and therefore SVG), browsers should be able to distinguish between UTF-8 and UTF-16 automatically. For graphical SVG, UTF-8 is usually preferred, as it efficiently stores the characters used for the SVG markup itself. You don’t need to declare UTF-8 encoding with a processing instruction, but you do need to ensure that your code editor (or other software) saves the file in UTF-8 format.

Many text editors, and even code editors, save files in the older ASCII or ANSI encodings by default. Depending on the software, you may be able to change the default in user preferences. In other software, you will need to specify the encoding every time you save. Avoid future headaches by learning how to set the encoding in the software you are using!

If you are including many multibyte characters (e.g., if the text consists
 of mostly ideographic scripts), UTF-16 may be more appropriate. Other encodings should be avoided now that Unicode is widely supported, but if they are used, they should always be declared using a processing instruction. You may also need to change your web server’s setting to ensure that it is not declaring a conflicting encoding.
Tip
The official names for character encodings are registered with the Internet Assigned Numbers Authority
 .

SVG, HTML, and XML are text-based markup languages, where the structure and features of the document are indicated within the character data. The angle brackets (less-than/greater-than signs, <
 and >
 ) separate the markup from the plain-text content that will be displayed. Supplementary text may be included in quoted attributes within the markup tags.
Tip
In SVG, not all plain-text content of the document is displayed; some is used for metadata and alternative text descriptions of the graphics.

Because markup characters have special meaning when reading the file, they cannot be used to represent the actual character within the text content. 
 
 The Standard Generalized Markup Language (from which HTML, XML, and SVG are derived) introduced character entities, which start with an ampersand (&
 ) and end with a semicolon (;
 ), to represent these special characters.

In XML in general and SVG in particular, there are only five defined entities:

- 
&lt;
 for the less-than sign, <

- 
&gt;
 for the greater-than sign, >

- 
&amp;
 for the ampersand, &

- 
&apos;
 for the apostrophe or single straight quote, '

- 
&quot;
 for the double straight quotation mark, "

The less-than sign and ampersand *must*
 be encoded within XML text content; the others are usually optional.

In HTML, there are dozens of defined entities to represent common characters that cannot be represented in all character encodings or typed
 with all keyboard layouts. Examples include &hellip;
 for … (horizontal ellipsis) or &eacute;
 for é (lowercase *e*
 with an acute accent).

HTML is also more lenient about bare ampersands in text content; if they are not followed by the rest of a valid character entitity, they will be treated as plain text.
Warning
HTML entities may be used within SVG markup included inline in an HTML 5 document, but not in standalone SVG files.

In XML or HTML, characters that cannot be encoded directly or by a defined character entity can be represented using the Unicode code point. The numeric code point value can be expressed using either the decimal or hexadecimal notation for the number: for example, &#8756;
 and &#x2234;
 These numeric character entities both represent the mathematical “therefore” sign (∴), which can also be represented in HTML by &there4;
 .

To ensure the correct interpretation of your text, particularly by accessibility technologies, you should also declare the human language of the content. This is done with the lang
 attribute in HTML or the xml:lang
 attribute in XML and SVG. In both cases, the value of the attribute is a language code consistent with the Internet Engineering Task Force’s “Tags for Identifying Languages” — currently, RFC 5646
 .

In most cases, a two-letter language tag is sufficient, such as en
 for English or de
 for German (*Deutsch*
 ). In other cases, a precise description of the language includes subtags, which add a country code (e.g., pt-BR
 for Brazilian Portuguese) or a script type (e.g., zh-Hans
 for simplified Chinese characters).

In both HTML and SVG, the language attribute applies to the text content and other attributes of the current element, as well as all nested elements, unless a nested element has its own language declaration. For single-language documents, therefore, it only needs to be specified once on the root <html>
 or <svg>
 element.
Font Data
Characters, as we have made clear, are not glyphs. On their own, the characters encoded in an SVG or HTML file do not have any visual representation. To display that character data on a screen, or print it on a page, the computer needs to pair it with a font.

The word *font*
 originates from metal-working foundries that created the type used in early printing presses. The mechanization of the written word standardized the appearance of individual glyphs within each printed page, but it also prompted the development of contrasting type designs for different purposes. Each font was a collection of letters and symbols which could be arranged to create a continuous section of text; different fonts set text at different sizes or with different styles.

The earliest computer fonts were collections of bitmapped images for each character: a fixed-size grid of points which should either be colored or not. The program displaying the text lined up each image one after the other on the display in the same way that metal type was lined up in a printing press.

Just as with metal type, each bitmapped font corresponds to a single size of text. If you need a different size of text, you need an alternative set of glyph data. If you want to print it on a device that allows finer resolution of colored points, you again need alternative glyphs.

Vector fonts addressed this issue by using mathematical lines and curves (quadratic or cubic Bézier curves) to define the shapes of each glyph, regardless of how many points of color fit within that shape. 
 Vector fonts were first used in printers, particularly with Adobe’s PostScript typesetting tools. 
 Apple and Microsoft collaborated (imagine!) to introduce vector fonts to computer interfaces with the TrueType font format.

Vector fonts, however, are limited by the resolution of the display in another way: the curves may be infinitely scalable, but computer monitors are not. Elegant shapes become distorted and illegible when forced to fit the pixel grid at small scales. Both PostScript and TrueType fonts include additional data or instructions (known as font *hints*
 ) for adjusting the curves to fit the display grid at small sizes. Figure 1-5
 shows how these hints modify the vector outlines to create results similar to the blocky shapes of a purely bitmapped version; at higher resolutions, a much more accurate representation of the outline is possible.

TrueType was widely successful, but designers already had their favorite PostScript fonts. The OpenType specification was developed to make it easier for PostScript fonts to be used with software designed for TrueType. It allowed either format of vector data to be packaged in a file with a consistent structure and metadata.
Note
The OpenType file structure was an extension of the TrueType format, and TrueType fonts are also valid OpenType fonts. As a result, TrueType/OpenType fonts have continued to use the *.ttf*
 file extension for backward compatibility. Old software might not use new OpenType features, but can still access the basic font data.

There have been many other font file formats, using various mathematical models to define the shapes of glyphs and various programming languages to describe how those glyphs should be adjusted in different uses. On the Web, however, the OpenType fonts are currently dominant. Newer font formats such as WOFF (Web Open Font Format) are variations on the OpenType structure, with improved data compression and added metadata information.

Basic digital fonts, whether bitmapped or vector, follow the model of metal type. Individual characters map to individual glyphs that can then be lined up in neat rows. By default, this can create an unpleasantly chunky appearance. 
 Most font formats include *kerning*
 instructions to adjust the spacing between certain pairs of characters.

For many scripts, particularly those based more on handwriting than on printed type, kerning is not sufficient. Glyphs need to adjust not only in spacing, but also in shape or even position, according to the character sequence.

OpenType has introduced numerous features for defining optional and required substitutions of glyphs for given sequences of characters. However, not all fonts will include these options, and not all software will know how to use them. Other font formats incorporate more complex *text shaping*
 rules directly in the font data, but for OpenType much of the text shaping decisions must be made by the layout engine.

Even when all substitutions and rearrangements are made, the font data still consists of individual glyphs (although not only one glyph per character). The appearance of connected *cursive*
 text is created by overlapping the ending stroke of one glyph with the starting stroke of the next.
Text Layout Instructions
The printing-press typographer slid sequences of metal type on to alignment rails. Each letter took up just as much space as it needed, and the font came with a variety of spacers to place in between words, as necessary to adjust the lengths of each line for pleasing balance. The lines of text were then fit together with additional metal spacers to create a page.
Note
These spacers, made out of lead, are the source of the typographic term *leading*
 (pronounced *led-ing*
 ) to describe spacing between lines of text.

Modern word processors — and related text-layout software such as the web browser — attempt to re-create that pleasing balance with the application of clear rule sets. The font data indicates how much space each glyph should consume in a line. The software may also use the font to insert ligatures or adjust kerning.

The layout software, however, is solely responsible for arranging the font glyphs into a logical document according to the standards of the script and language. Most text layout software uses rules to determine appropriate word breaks — for the language and script — at which to start a new line or insert extra space for a justified alignment. (Rules for determining appropriate hyphenation breaks are more complex, and therefore less common.) Other language-sensitive rules may be used to transform the case of text or re-arrange the order of characters when scripts with different directions are mixed together.

Because the breaks and spacing are determined automatically, a word processor can adjust and reflow the lines of text if the content or styles are changed, removing and inserting line breaks as required. This is a key feature of web browser display of HTML text; it flows to fit the size of the display.

The automatically generated layout may not be quite as pleasing as text positioned by a skilled typographer, but it is much more flexible. On the Web, this is particularly important for web layouts that are responsive to devices with different sized screens. To display a photograph or other image on a smaller screen, it needs to be scaled down in all directions. Text, however, can wrap to fill more lines with the same size font.

Although web browsers are quite content to lay out plain HTML text according to default rules, Cascading Style Sheets (CSS) offer many ways to customize the output. Text styling instructions can be loosely classified into categories:

- Character manipulation properties, such as text-transform
 , direction
 , and unicode-bidi
 , define transformations to the character data that should be applied before converting characters into glyphs.

- Font properties, such as font-weight
 , font-stretch
 , or font-variant
 , determine which font file is used and what features of the font are activated.

- Text styling properties, such as text-decoration
 or letter-spacing
 , modify the appearance of continuous sequences of glyphs.

- Text layout properties, such as text-justify
 , text-indent
 , line-height
 , and white-space
 , control how rows of glyphs are divided and arranged into blocks of text.

- Page layout properties, such as width
 , height
 , padding
 , and margin
 , determine how blocks of text are positioned on the page, and indirectly set the maximum length of each text line.

This book assumes that you are at least moderately familiar with using CSS to style HTML text. The categories distinguished here are emphasized because they correspond to the areas where CSS-styled SVG text layout and CSS-styled HTML text layout overlap, and where they diverge.
Text Within Scalable Vector Graphics
SVG is a graphic language, used to define geometric shapes and graphical effects for rendering them. SVG images are often embedded within HTML text, and SVG markup may be included directly with HTML 5 files.

Text within SVG itself is often an afterthought. Nonetheless, words within graphics are indispensable as annotations for charts, presentations, and maps, assigning context to the size of a pie chart wedge or forming a label for a color in a legend. There is also a more artistic side of SVG text: words as *art*
 .

The phrase “word art” has a somewhat besmirched reputation, thanks to the ease by which colorful distorted words can be created in some office software — and the corresponding overuse by some office managers to decorate every office memo. But a tool is only as useful as the person wielding it, and it should not be discarded just because it has been misused.

Calligraphy — literally, beautiful writing — is even today considered an art form. The modern typographer is far more artist than technician, extending the art of beautiful, engaging, and sometimes horrific or amusing, writing into the electronic realm.

It is thus perhaps not surprising that SVG included a fairly rich library for handling text, both for laying out lines of text and for the creation
 of fonts and font glyphs (the graphics that describe each letter).
Note
Unfortunately, SVG fonts were sufficiently different from the OpenType font formats used by web browsers that Firefox and Internet Explorer never implemented them. In particular, the SVG font specification did not include any equivalents to the more advanced OpenType glyph-selection features essential for the correct rendering of some scripts.

SVG fonts are still supported on WebKit and iOS devices, but the Chromium project has removed support for SVG fonts from Blink-based browsers. This book therefore focuses on the layout of text, and the selection and use of existing fonts.

It is important to realize that SVG uses nearly the same CSS properties for selecting and styling fonts that HTML does. This means in practice that if you know how to style text in HTML, you already know many of the ways to style text within SVG.

An equally important realization is that SVG uses a completely *different*
 model from CSS/HTML when it comes to positioning text on the screen. SVG text layout has as much in common with the layout of SVG shapes as it does with CSS layout of flowing text in an HTML page.

Text in SVG is drawn exactly where you position it, and does not re-position itself if it bumps into other text or overflows the edge of the image. If the graphic as a whole changes size, the text scales down with the imagery; it does not reflow.

SVG text layout is a hugely complex topic. At its most basic, it consists of an instruction to the browser to “write this text here.” At its most complex, it allows you to carefully position individual letters in geometric patterns, with nearly as much control as you position your SVG shapes.

*Nearly*
 as much control, but not quite. Text positioning within SVG is always a balance between the designer who knows what is best for the graphic, and the software that knows (or should know) what is best for the particular font and linguistic scripts being used.

You can minimize the variability by trying to ensure that the browser will use the font you designed with, either by using a common system font or by making a web font available by reference. However, the use of these fonts is still not guaranteed. Careful design is required to ensure the layout is acceptable with alternative fonts. Additional properties and attributes are available to tell the browser how much space you expected the text to fill.

Unfortunately, text is one of the worst areas in SVG for cross-browser inconsistencies. Many of the more nuanced layout options defined in the specifications cannot be relied on for documents that will be distributed on the Web. As much as possible, this book warns you about the major incompatibilities at the time of writing (mid-2015). However, the best defense against unexpected results is to test in as many browsers and operating systems as you need to support.

This is particularly true when working outside Latin scripts. The SVG specifications introduced a number of features that were intended to offer support for all types of writing systems, including right-to-left and top-to-bottom scripts. The well-meaning but overly complicated internationalization options have never been well implemented, and are in the midst of being rewritten by new CSS specifications. Nonetheless, they are worth keeping in mind, whether you create multilingual documents or whether you would like to use vertical text for graphical effect. In the meantime, you can re-create
 many of these layouts using SVG’s manual positioning options. Chapter 7
 discusses both the standard features and the workarounds.
Tip
A key feature of SVG text is that it can be filled and stroked like any SVG shape, including with gradients and patterns. This book does not go into detail about SVG’s painting options, but it does highlight a few of the ways in which painting text is unique.

After working through this book, you will find that there are very few text layouts that you *can’t*
 create with SVG. However, that does not mean it is always the best tool for the job.

The control that SVG text layout offers comes at the cost of the automatic line layout and reflowing text available with CSS-styled HTML. In many cases, it is much easier and more responsive to use HTML and CSS text layout. The SVG specifications even allow you to embed HTML within SVG (using the <foreignObject>
 element that we’ll discuss in Chapter 12
 ) but again, incomplete browser support has limited its use.

1

 This is a vastly oversimplified discussion of character encodings in general and Unicode in particular. Joel Spolsky’s 2003 article “The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)”
 should help fill you in on the rest.