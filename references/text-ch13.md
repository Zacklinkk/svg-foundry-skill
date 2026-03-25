Chapter 10.
 Fonts, Families, Faces
This book has so far covered, in extensive detail, all the ways in which you can control how letters (and other glyphs) are arranged within your SVG. The letters themselves, however, have been somewhat overlooked. This chapter addresses that gap. Letterforms are at least as important as layout in creating the final appearance of graphical text.

A full discussion of typography, fonts, and type selection in web design is a book unto itself. So we’re not going to discuss how you would choose (or design!) a typeface for your graphic. Instead, this chapter focuses on how you can get the browser to use the font you prefer. Chapter 11
 will then explain what you can do to minimize the impact if it uses a different font regardless of your style settings.
Generic Styles
The primary way to select a font, and therefore control the character style, is with the font-family
 style property. It is an inherited style that may be specified as an SVG presentation attribute or CSS style rule. With CSS rules, font-family
 may be set directly or as part of the font
 shorthand. 
 
 However it is set, the value is a comma-separated list of font-family names, ending with a generic fallback font description.

Most of the examples in this book have simply used the generic names to describe what type of font should be used. These keywords are defined in core CSS, and describe some of the most basic distinctions between font types. Browsers are expected to provide fonts for the five core generic font types defined in CSS:

serif

A font with formal flourishes on the strokes of the glyphs, usually with variation in the thickness of the stroke. For Latin scripts (like the letters you are reading), the font will have *serifs*
 , small horizontal lines at the top and bottom of strokes.

sans-serif

A font that uses simple, smooth strokes, often of uniform thickness. For Latin scripts, they are identified by the lack of serifs (*sans*
 means *without*
 ).

monospace

A font in which all the characters in each script have the same width, like typewriter text.

cursive

A font that appears to be handwritten. It may be an elegant calligraphic script, a childish block print, or anything in between.

fantasy

A decorative or display font.
Tip
The generic font name is a keyword, and should never be quoted.

In professional SVG work, using generic fonts may be acceptable for basic text labels on data visualizations or charts. Each operating system and browser renders fonts differently, and allowing the browser to pick the specific font usually results in clear and legible letters.

However, clear letters are only one part of legibility. The size and spacing of the glyphs can affect whether text overlaps other graphics, or fits within the dimensions of a button or border. Standard serif and sans-serif fonts can vary considerably from one to another in their effective size — how wide and tall the individual characters are relative to the font size — as well as their appearance. With the cursive
 and fantasy
 font classes, using generics means giving up control over the text appearance completely.

In addition, browsers are not currently very good at selecting an appropriate generic font for the script and language of the text. Most apply the same font family for all content, and fall back to their default font if the dedicated serif
 or monospace
 font family cannot display a given character.

For these reasons, in more complex graphical designs, the generic keywords are usually only used as fallbacks, after naming your prefered fonts.
Making the Most of System Fonts
Most web browsers have access to fonts on the user’s computer, provided by the operating system or installed individually by the user. Names in the font-family
 list are searched against this database of installed system fonts. The match should be case insensitive, and should consider any language translations for font-family names provided in the font data.
Tip
Font-family names may be quoted, but don’t have to be — unless they contain a comma or other special character in the name!

With a little research and experimentation, you can sometimes create a list of font families that have similar appearance and dimensions and are collectively available on most common operating systems. This approach was used in the text-on-a-path examples (in Chapter 9
 ) to select Times New Roman font or something like it:

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

}
Tip
The font-family
 list is always the *last*
 part of the font
 shorthand. It *must*
 be preceded by the font-size
 . The font-style
 and font-weight
 modifier keywords at the beginning are optional.

If you want to ensure contrast between different fonts in a graphic, even if the exact font cannot be matched, make sure that the last value in each font-family
 list is a different generic keyword. These will always match an available font, and will nearly always be distinct from each other.

Once you go beyond the old reliable font families like Times or Arial/Helvetica, however, there are very few fonts that have close matches between operating systems. Because there are very many operating systems to consider, including mobile devices, using system fonts results in some unpredictability. In Example 5-4
 , which created a comic book effect for the text “BAM! BOOM!”, we used the following font-family
 list:

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


The first three values all reference the same font family under different names. The remaining families were selected because they include heavyweight, very emphatic typefaces. 
 A font-weight
 of 900 was used to select the heaviest font face available in the family. However, the fonts are hardly interchangeable.

Figure 10-1
 compares the same SVG code in our preferred font face (Gill Sans Ultra Bold), in the four fallback fonts, and finally in a *very*
 heavyweight decorative font (Goudy Stout). This last one was expressly left off the fallback list because “BAMBOO” does not have quite the same meaning as “BAM! BOOM!”.

Beyond the difficulty of *finding*
 appropriate fallbacks for a decorative font, there is the question of whether your users will have it available. Even if the font is on the user’s system, convincing a web browser to use it is not always easy.
Warning
To access font faces other than the usual regular, italic, and bold options, some browsers (Firefox) and operating systems will only match the simplified family name (e.g., “Gill Sans MT” or “Arial”) while others (Internet Explorer and Chrome) will only match the extended family name (e.g., “Gill Sans Ultra Bold” or “Arial Black”). Chrome on Windows was unable to locate Gill Sans Ultra Bold from the system font folder under any name.

In addition, Firefox will match the PostScript name (e.g., “GillSans-UltraBold” or “Arial-Black”), which is always unique to a particular typeface, rather than describing a complete font family — but only within the local()
 source indicator of a font-face rule (which we’ll get to in a moment!).

Including both versions of the font-family name, as done here, ensures the font will match if available. Unfortunately, according to the CSS font selection rules, a regular typeface from the font family will be used if it is available but the desired face is not, instead of using a similar face (ultra bold, condensed, etc.) from the next family in the fallback list. In other words, the text will be displayed in “Gill Sans MT” regular or bold, rather than in “Showcard Gothic” or “Cooper Black.” Consider this carefully when designing your font stack, and decide whether the font family or the typeface parameters are most important.

As Figure 10-1
 demonstrates, not only does the font affect the style and feeling of the text, it significantly affects the layout. The very *condensed*
 (narrow) Impact font takes up less than 2
 /3
 of the width of the design, while the very *expanded*
 (wide) Goudy Stout stretches off the edge of the graphic. This is true despite the fact that all the text uses the same font size.

The layout in this particular graphic could be improved by using absolute x
 positioning instead of dx
 offsets. However, that isn’t an option in most cases, where you want the letters to be spaced smoothly and naturally.

For these reasons, relying on the user’s system fonts is rarely going to be your preferred choice for artistic SVG images.
The Perfect Face
One of the most important aspects of SVG is that an SVG graphic is intended to be portable from one platform to the next. If the text is an artistic part of the graphic, most designers will expect it to have the same appearance whenever that graphic is displayed.

There are two distinct approaches to ensuring portability between systems. The first is to incorporate the font directly into the SVG itself. This provides the maximum degree of portability — in effect, the font is simply a set of reusable graphics, that are then drawn into the positions defined by the text.

Embedded fonts have been an option for PostScript files, including PDF, for decades. SVG was designed to have a similar facility, using the <font>
 and <glyph>
 elements among others. With these, you could declare the overall features and dimensions of the font as a whole and the specific shapes of individual letters and other glyphs.

SVG fonts allowed incredible flexibility, including multicolored letters, animation, and flexible coordinate systems; anything you could draw with SVG could be made into a font!

Unfortunately, as mentioned in Chapter 1
 , many browsers refused to support SVG fonts, and others have removed early implementations. SVG fonts therefore cannot be relied on to provide a consistent appearance on the Web.
Note
Efforts are underway to redefine SVG fonts in a structure compatible with Open Type/Web Open Font Format. This would not change support for the use of SVG fonts included directly within the SVG markup. However, it will hopefully increase support for SVG fonts — in all their multicolored glory — by including them as CSS web fonts.

The alternative to embedding fonts is to provide the fonts by reference, as an external resource. The font is contained in a separate file on the web server (or a different server). The main document links to it similar to how it links to an external image file or stylesheet. These web-distributed font resources are known as web fonts to distinguish them from the system fonts installed on your computer.

Web fonts are supported in the SVG 1 fonts specification, which provided XML elements to specify the location of font files in various formats. Multiple files could be given so that a browser could choose a file in a supported format. This structure — defining a font’s properties and the URL where it can be found — was designed to provide an XML parallel to the then-new web fonts syntax in CSS 2.

SVG fonts may have been poorly received, but CSS-based web fonts have become widely accepted. However, this is no “CSS versus SVG” issue: you can use the CSS font declarations in your SVG files. 
 And although the CSS declarations usually link to external font files, they can also be used to embed font data in the form of a data URI.

A data URI provides a complete file in a format that is treated as a single URI/URL string for the purpose of the parent document. A data URI-encoded font cannot be easily edited like SVG font markup, but it does allow you to encapsulate all the data for your graphic within a single file. This can be useful if portability is essential. However, data URIs can significantly increase the file size, without any potential to cache the font for use in multiple pages or graphics on your website.
Warning
As external resources, web fonts will not be downloaded when they are referenced from an SVG used as an embedded image (an HTML <img>
 or CSS background-image
 ).

Most browsers *will*
 use a font that is included as a data URI; Safari and related WebKit browsers will not.

Blink browsers will also use an external web font within an image if it declared in the CSS for *both*
 the SVG image file and the web page that embeds it.

If web fonts are important, use inline SVG or embed your SVG file as an <object>
 .

Web fonts are declared in CSS using an @font-face
 rule, which may be declared in a <style>
 element or an external stylesheet. The @font-face
 rule is properly called a *pragma*
 ; it defines an entity (a particular font file and its properties) that can be used by the rest of the stylesheet. It does not set the value of a style property on any particular element.

In order to *use*
 a web font, you first describe its properties in the @font-face
 rule, and then set an element to use a font with those same properties. The descriptors within an @font-face
 rule therefore look much the same as property declarations for an element. Each face (italic, bold, condensed, etc.) of a font requires a separate @font-face
 rule with descriptors for the styles that it will match.

Example 10-1
 uses two @font-face
 rules to specify a pair of fonts with a handwriting style, one cursive and one block-print, that are then used to create the graphic shown in Figure 10-2
 .

Example 10-1.
 Using web fonts in an SVG
<svg

 xmlns=

"http://www.w3.org/2000/svg"

 xml:lang=

"en"

     xmlns:xlink=

"http://www.w3.org/1999/xlink"

     width=

"4.3in"

 height=

"4.3in"

 viewBox=

"0 0 400 400"

>

    <title>

Dear Pen Pal</title>

    <style 

type=

"text/css"

>

@font-face

 {

    font-family

:

 "hand-writing"

;

    font-weight

:

 normal

;

    font-style

:

 italic

;

    src

:

 local

(

"Morado Marker"

),

 local

(

"morado-marker"

),

         url

(

"../Fonts/morado marker.woff"

)

 format

(

"woff"

),

         url

(

"../Fonts/morado marker.ttf"

)

 format

(

"truetype"

);

    /* Morado by Peter Wiegel

       Downloaded from: http://www.dafont.com/morado.font

    */

}

@font-face

 {

    font-family

:

 hand-writing

;

    src

:

 local

(

"CoolStory"

),

 local

(

"CoolStory Regular"

),

         url

(

"../Fonts/coolstory regular.woff"

)

 format

(

"woff"

),

         url

(

"../Fonts/coolstory regular.ttf"

)

 format

(

"truetype"

);

    /* Cool Story by Peter Olexa

       Downloaded from: http://www.dafont.com/coolstory.font

    */

}

svg

 {

    background-color

:

 lightSkyBlue

;

    border

:

 solid

 thin

;

    margin

:

 10px

;

    font-family

:

 "hand-writing"

,

 cursive

;

    font-size

:

 36px

;

}

    </style>

    <defs>

        <rect

 id=

"paper"

 width=

"300"

 height=

"350"

 rx=

"3"

/>

    </defs>

    <g

 transform=

"translate(-5,50) rotate(-20) skewY(5)"

>

        <use

 xlink:href=

"#paper"

 transform=

"translate(-10,15)"

             fill=

"black"

 fill-opacity=

"0.3"

/>

        <use

 xlink:href=

"#paper"

 fill=

"linen"

 stroke=

"bisque"

/>

        <g

 transform=

"translate(0,20)"

>

            <text

 font-style=

"italic"

 font-size=

"48px"

                  x=

"30"

 y=

"50"

 >

Dear Pen Pal,</text>

            <text>

                <tspan

 x=

"10"

 y=

"100"

>

Today, I received a</tspan>

                <tspan

 x=

"10"

 dy=

"50"

>

new letter from my</tspan>

                <tspan

 x=

"10"

 dy=

"50"

>

pen pal.  Hooray!</tspan>

            </text>

            <text

 font-style=

"italic"

 font-size=

"48px"

                  x=

"100"

 y=

"250"

 >

Sincerely,</text>

            <text

 x=

"200"

 y=

"300"

 font-size=

"larger"

>

Me</text>

        </g>

    </g>

</svg>

The first line in each @font-face
 rule is a font-family
 description:

@font-face

 {

    font-family

:

 "hand-writing"

;


This font-family
 descriptor is required. It declares the font-family name that will be used when matching the font-family
 settings of specific elements in the document. It does *not*
 have to be the normal name for that font; it could be a name that describes the specific way in which you are using the font. The name may be quoted — and should be if it has apostrophes, quotes, or commas in it — but it does not have to be.

Both font faces defined in Example 10-1
 will be referenced by the family name hand-writing
 . Which typeface gets used depends on the other descriptors. The first face is declared to match normal-weight italic text:

font-weight

:

 normal

;

font-style

:

 italic

;


The second font-face rule does not have any font-weight
 or font-style
 descriptors. These are set to the default values: normal weight and normal style. As with the font-family name, these declared features do not have to match the features defined in the font file. In Example 10-1
 , a bold-weight, normal-style cursive writing font (Morado Marker) is used as the italic version of a block print hand-writing font (Cool Story).

After the @font-face
 header has been parsed, the name given by the font-family
 description is the name by which the SVG graphic refers to the font in font-family
 properties of individual elements. The typeface name defined in the font file itself will not match.

When you use the fonts in style declarations, you should also give appropriate fallback fonts. In Example 10-1
 , the natural fallback was a generic cursive
 font:

font-family

:

 "hand-writing"

,

 cursive

;


The font-family
 style is applied to the entire <svg>
 . Because both of the font faces were declared to have the same family, the distinction comes from whether or not the text is given a font-style
 of italic
 . The font-style could be set using any of the ways in which style properties are set. In Example 10-1
 , it is set using presentation attributes on individual <text>
 elements:

<text

 font-style=

"italic"

 font-size=

"48px"

      x=

"100"

 y=

"250"

 >

Sincerely,</text>



 
 The final descriptor in each @font-face
 rule, src
 , tells the browser where the font file is located:

src

:

 local

(

"Morado Marker"

)

,

 local

(

"morado-marker"

)

,

     url("../Fonts/morado marker.woff")

 format

(

"woff"

)

,

     url("../Fonts/morado marker.ttf")

 format

(

"truetype"

);


The src
 descriptor is also required — otherwise, there’s no purpose to having the @font-face
 rule. It accepts a list of comma-separated options, in order of preference. There are two ways to specify the location:

- 
 As the name of a font to look up on the user’s system, wrapped in the local()
 function;

- 
 As a URL to a location on the Web (absolute or relative), in a url()
 function, with an optional font format descriptor — wrapped in the format()
 function — so the browser knows whether it can use the file *before*
 it downloads it.

Within a single src
 list, the *first*
 value that the browser can match will be used, similar to the font-family
 list for an individual element. However, because the syntax for CSS fonts has changed since they were first introduced, you may see multiple src
 declarations, with the earlier versions not including format()
 or local
 (). In that case, the normal CSS error-handling and cascade rules apply: the *last*
 src
 list that is in a syntax the browser recognizes will be used.

In the case of local fonts, you may sometimes need to include a couple variants of the name to support equivalent fonts on different operating systems or in different file formats. However, only the US English version of the name should match.
Tip
The requirement to use American English names is unique to local()
 . For historical reasons, system fonts specified in the font-family
 fallback list should match in any language.

Within a src
 declaration, local font names should always be listed first. Both you and your website’s vistor will save bandwidth if a local version of the font is used when available, instead of downloading a web font file.
Warning
When using local fonts, Blink browsers will synthesize italic or bold effects if there is a font-family name match but not a match for the specified typeface weight or style. In contrast, the same font file will not be modified if it is provided as a web font.

The font-synthesize
 property (not yet supported in Blink) will allow content authors to control whether or not *faux*
 bold or italic is allowed when styling text, but it is not specifically supported for local()
 font matches within an @font-face
 rule.

If there is no local match, the browser then examines the URL options, and in particular the information about font formats. The format()

 parameter src
 is an optional — but highly recommended — hinting mechanism. Browsers can determine the font type from the MIME type in the file header, but because similar fonts don’t always have unique MIME types, the format()
 statement provides an additional hint. More importantly, if a font can be identified by type, the browser doesn’t need to request and start downloading all of the fonts listed before finding one that it can work with.

Many different font formats are available as web fonts, but not all browsers support all formats equally. For most modern browsers, you want the WOFF or WOFF2 formats. (If you are going to embed a font as a data URI, WOFF is currently your best bet for size and support.) For older browsers, you might need TrueType/OpenType, Microsoft’s proprietary Embedded OpenType, or even SVG fonts, which were the only web font supported on early iPhones.
Note
The Web Open Font Format, format("woff")
 , is a font format standardized by the W3C Web Fonts Working Group. The format is essentially a modified OpenType or TrueType Font that makes use of a compression scheme to reduce the overall size of such fonts by 50%–60%. Because font download times can be significant — it is not uncommon for some larger fonts to run upward of several megabytes in size — this compression scheme is essential for website performance.

The specification for the WOFF 2.0 font format, with even better compression, has not been finalized at the time of writing. Indicated in CSS by format("woff2")
 , it is already supported in the latest Blink and Firefox browsers.

If it is vital to your design that your chosen font is used on any device, you can take advantage of numerous font conversion tools to provide your font in legacy formats. Be sure to present the different sources in the correct order — the *first*
 value in the *last*
 src
 descriptor is the preferred option:

@font-face

 {

    font-family

:

 "MyFont"

;

    /* IE9 Compatible */

    src

:

 url

(

"my_font.eot"

);

    /* Modern Syntax */

            /* Try local first */

    src

:

 local

(

"My Font"

),

 local

(

"MyFont"

),

            /* the latest browsers */

         url

(

"my_font.woff2"

)

 format

(

"woff2"

),

            /* most modern browsers */

         url

(

"my_font.woff"

)

 format

(

"woff"

),

            /* older Safari, Android, iOS */

         url

(

"my_font.ttf"

)

 format

(

"truetype"

),

            /* oldest iOS */

         url

(

"my_font.svgz"

)

 format

(

"svg"

);

}


Nonetheless, be aware that some browser settings may still prevent the download of web fonts, regardless of formats. In particular, the Opera Mini browser — which is designed for affordable mobile phones on expensive mobile networks — never downloads web fonts.
Finding Fonts
@font-face
 has a long and somewhat troubled history. The idea that web pages should be able to set fonts in the same way as word-processing software has been around practically from the earliest days of the Web. However, typeface files — which take dozens to hundreds of hours to create — typically have been closely guarded by the font foundries.

Putting a font onto the Web meant that such fonts could be grabbed for free. Needless to say, this did not sit well with the various foundries. There have consequently been a number of different schemes for trying to protect these fonts, including proposals within versions of the CSS Fonts specifications.

In practice, what has happened is much the same as happened in most other media areas: the combination of large numbers of font producers (many of them working as amateurs or producing fonts that they initially developed as part of other projects) and the availability of file sharing essentially rendered the point moot. Like clip-art and icons, there are many sources of free fonts and many more sources of paid ones. Some of these services also host the font files on their own web servers.

The free fonts tend to lean toward decorative fonts that cover limited scripts and only look good when drawn large — but that is often all you need for SVG. Web fonts used in this book were either downloaded from DaFont.com
 (a database of free and shareware fonts with various licence restrictions) or are imported from Google Fonts
 (completely free web fonts).

When considering a web font, be sure the licence — whether free or paid — allows you to *distribute*
 the font. Even if you have the right to unlimited commercial use of the font, that only applies to using the font to create print products, not making the font itself available on your website.
Tip
For no-restriction web fonts, look for fonts released under the SIL Open Font Licence.

As web fonts have become popular, a new controversy has arisen. Fonts are not small files. As more web fonts are available, and more websites use them, fonts have started to compete with raster images as one of the largest contributors to website downloads. The two fonts used in Example 10-1
 add up to nearly 50KB if the compressed WOFF files are used, and more than 90KB for the uncompressed TrueType files. Unless you’re using those same font files throughout your website, that cancels out the file size benefits of using SVG.

Theoretically, a document can load any number of fonts via the @font-face
 mechanism, but performance considerations should drive you to use fonts sparingly. Font downloads not only cost your users in bandwidth, they can effectively block out users on slower connections.

Font-loading takes time. In some browsers, it is an asynchronous operation: the SVG graphic will render the text with the first available font given in a font-family list, and if a more appropriate one loads afterward, the browser will redraw. 
 The user may be confused or disoriented from seeing a block of text styled one way transform suddenly into a different look. Other browsers avoid this “flash of unstyled text” by not drawing anything at all while waiting for a font download — but that just creates a “flash of invisible text,” making the site unusable while waiting for large downloads.

For certain graphics, such as company logos, you can address the size and therefore the speed by *subsetting*
 the font to only contain the characters you will use. There are various programs that process a font to create a subset, or if you’re using the Google Fonts API you can include the characters in your font request; some paid font subscriptions offer similar options.

If you’re embedding the font as a data URI in your SVG file, you will almost
 always want to subset it to only include the necessary characters. Embedding a subset font should *not*
 be done if the same font is used in many documents or graphics on the website, as you lose the benefit of having a single asset file that can be used by all documents.
Tip
If using one of the most popular Google Fonts (such as Open Sans, used by Google itself), avoid creating a custom subset. The browser likely has already cached the font from another website.

Once a web font is downloaded, it is cached just like an image or script.
 This means that if you plan to display an SVG graphic that makes extensive use of fonts you expect will have to be downloaded — such as a complex data visualizations or maps — it may be worthwhile to include the same font declarations (with the exact same src
 files) in a placeholder loading graphic. That way, the fonts will start downloading while waiting for the more complex SVG to load.

There is one last restriction on using web fonts that can cause headaches if you aren’t aware of it. Fonts are binary resources that are interpreted, and as with any code that can execute content, they can launch viruses or other code nasties. For this reason, web fonts are subject to the same cross-origin restrictions as files accessed by scripts. If the font file URL is not on the same web server — and accessed by the same HTTP protocol — as your document, it must be served with cross-origin headers that explicitly allow your web domain to use it.

The security benefit may be questionable (font files are unlikely to contain personal data), but cross-origin restrictions provide extra copyright control for the many paid-subscription web font servers. Each subscribing website accesses the font with a URL that will return HTTP headers allowing its use on that specific web domain. In contrast, Google’s free font servers use headers that allow the files to be accessed from any domain.
Faking Fonts
With all the complications of getting fonts to work as expected on the Web, one option popular with designers is to replace the text with SVG shapes. The main graphical editing programs (e.g., Illustrator, Inkscape) include an option for converting text to path — effectively “printing” text to a predictable graphical representation, by generating shapes of the individual letters’ outlines in your chosen font.

Creating paths from text effectively subsets and embeds the font, avoiding any rendering problems if the user does not have access to the same font. It also circumvents many licencing restrictions: because you’re not distributing complete font files, any font that you have the right to use in print publications can be used as a text-to-path representation.

For all these reasons — and because it is well supported by graphics programs, but embedded CSS web fonts are not — converting text to path is still popular for logos and advertisements where consistent font rendering is considered essential.

However, converting text to shapes removes all the meaning associated with the text. It cannot be easily edited and no longer makes sense to search engines or screen readers. If you use this technique, be sure to include equivalent, machine-readable text. Unfortunately, most graphics tools will not do this automatically.

There are two options for including readable text in addition to the text-to-path shapes:

- 
 
 
 Use a <title>
 or <desc>
 element, or aria-label
 attribute, in inline SVG, or an alt
 attribute on the HTML image that embeds it. The latest versions of Inkscape use an aria-label
 attribute to preserve text content for screen readers when converting text to path.

- Include <text>
 elements in the SVG that draw transparent letters (in a generic font) over top of the visible shapes.

The invisible <text>
 elements have the benefit of making the text available not only to accessibility technologies but also to regular users trying to select it (e.g., to copy it into a search engine or translation software). 
 To make the invisible text interactive, you will need to set the pointer-events
 property to painted
 so that the text can be selected even when its opacity
 is 0. For best results, you’ll also want to make sure the size and spacing of the generic font text is adjusted to match your custom font, using the methods that we’ll discuss in Chapter 11
 .