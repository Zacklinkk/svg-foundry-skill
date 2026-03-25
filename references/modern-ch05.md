Chapter 2. Tools of the Trade
From *Using SVG with CSS3 and HTML5*
Software and Sources to Make SVG Easier
The SVG examples in this book were for the most part created “from scratch”, by typing markup or standard JavaScript to build and manipulate the graphics.  However, that’s certainly not the only way to work with SVG, nor the most common one.

Most SVG art starts life inside graphical software, created by an artist working with shapes and colors instead of XML tags and attributes.  Most SVG data visualizations are built using JavaScript libraries full of shorthand convenience functions.  Most SVG icons are reused from one web application to another, and countless icon sets are available to license for your website.

By showing you the internal components of an SVG, stripped down to their skeletal form, we hope to give you a complete toolset to work with SVG: the skills to modify and extend *any* SVG you work with, no matter how it was created.  With this programmatic approach to SVG, you will be better able to manipulate graphics created by others or by software, in order to match your web design or to enable user interaction.  However, the mental toolset you’ll gain by understanding SVG shouldn’t detract from the software tools that other developers have created.

Software tools make it easier to create graphics, and easier to process your files so they are ready to deploy on your web server.  Tools discussed in this chapter include graphical editors that emphasize visual components instead of code, code editors that provide hints and immediate feedback, code libraries that streamline the creation of dynamic graphics with JavaScript, and the many rendering programs that display the SVG or convert it to other image formats.  In addition, we introduce the vast supply of free and licensable SVG content that can help you quickly enhance your web designs, even if your most artistic drawings are of the stick-figure variety.

This chapter specifically mentions some of the most popular software and services.  These are not the only options, and we don’t guarantee they are the best.  They are given as examples of what is out there, and of how different options differ from each other.  When choosing tools for your own projects, think about the features you need, and the budget you can afford.  Whatever you choose, remember that any standards-compliant SVG can be opened and modified with other SVG tools, so you’re not locked into any one product’s workflow.
Ready-to-Use SVG
The easiest way to get started with SVG—especially if you’re more of a programmer than a graphic designer—is to start with someone else’s art.  SVG may not be as ubiquitous on the Web as other image formats, but it’s getting there.  A simple web search for the word will retrieve numerous vendors and free collections of SVG, particularly for icons.

Prior to using clip art from a vendor or website, you should ascertain what kind of licenses are available for the graphic.  Early in the history of SVG, most graphics were released under some kind of Creative Commons license, but increasingly, high-quality artwork is produced by professional artists working within traditional copyright domains.  Although there are plenty of free-to-use graphics available (some with noncommercial restrictions or attribution requirements), others are offered under paid license systems similar to those used for stock photos or web fonts.
Tip
One benefit of SVG’s dual nature as both an image format and an XML format is that it is possible to embed copyright license information directly in the file using a <metadata> block.  We’ll discuss how you can do this for your own graphics in Chapter 16 of *Using SVG with CSS3 and HTML5*.

For accessing graphics created by others, remember that creative works are by default “all rights reserved”; the absence of copyright information doesn’t mean it is public domain.  Don’t use someone else’s work unless you are sure that the original creator has offered a license compatible with your intended use.

SVG will never replace JPEG for stock photographs (which can’t be efficiently represented in vectors), but it is now a standard option for vector graphic clip art and icons, including from commercial suppliers.

There are a number of tools and libraries that can convert simple SVG art into other vector formats and back again.  This can increase the flexibility of the vector graphics: for example, the encapsulated PostScript (EPS) format, long a staple in the realm of clip art, is still dominant in print.  For simpler graphics, icon fonts—which allow sets of single-color icons to be distributed as a web font file—are popular because they allow icon size and color to be controlled with familiar CSS properties.  Nonetheless, companies that produce clip art, maps, and related graphics for the Web are increasingly shifting to SVG for their vector graphic format.

Using the vector graphic source files, the stock art companies can also generate raster images (PNG, JPEG, and GIF) of any size.  For a web designer interested in purchasing raster graphics, however, it often makes more sense to license a single SVG and convert it into the associated raster format at various resolutions yourself, instead of purchasing raster graphics at different scales.

The following list of websites should help start you on your search for SVG:

Open Clip Art Project

The Open Clip Art Library (OCAL) Project is definitely the oldest, and perhaps the largest, repository of SVG content, all of it available either through Creative Commons or public domain licenses for unrestricted commercial use.  Established in 2004 by Jon Phillips and Bryce Harrington, the OCAL project was created to provide a public commons for clip art, using SVG for encoding primarily because SVG itself doesn’t have the same type of vendor encumbrances or royalty restrictions as other vector formats.  Moreover, because the source code for the graphics can be read with a text editor, it’s also possible to decompose clip art images into separate pieces, making SVG useful as a way of packaging collections of icons or images in a single file.  Figure 2-1 displays some of the diversity of artistic styles available.  The project is also integrated with the Flaming Text ImageBot graphics editor, which allows you to tweak some SVG style properties online.

Pixabay

Another stock art library, Pixabay includes photos, illustrations, and vector graphics.  However, be aware that many vector graphics are stored in Adobe Illustrator format, and you would need software able to convert them to SVG for use on the Web.  All images are released under the Creative Commons’ public domain license; you are encouraged to “buy a coffee” for the artists by donating funds when you download.

Wikimedia Commons

The media repository arm of Wikipedia, Wikimedia Commons compiles images, audio, and video in a wide variety of formats.  All files are available under some sort of “copyleft” license; some require attribution or are restricted to noncommercial use or to use within similarly-licensed work.  Detailed license information is available on each file’s catalog page.

Wikimedia is actively pushing their contributors to use the SVG format for diagrams, clip art, icons, and other vector drawings because of its flexibility and ease of editing; their servers then automatically generate raster versions in various sizes.  Although the tagging and cataloguing of files is often inconsistent, making searching a little difficult, there is plenty of great SVG content if you take the time to look around.  Figure 2-2 displays selections from the SVG Botanical Illustrations category, including a labelled diagram; because SVG text is easily editable, the file is available with labels in many languages.

Iconic

Iconic is a commercial SVG icon library, but they offer a set of more than 200 icons completely free to use (MIT license; just ensure that license information is available with the code).  This Open Iconic set includes most common user interface buttons in single-element icons that you style to any color you chose.  For their paid offerings, Iconic distinguishes themselves by taking full advantage of all the possibilities of SVG, supporting multicolor styling and using scripts to substitute in more detailed versions of the icons at larger sizes.  They even brag about their easy-to-read (and modify) XML markup.

The Noun Project

Another icon-focused library, the Noun Project’s goal is to create a visual language for clear international communication.  Access to their entire library is by monthly subscription, but their database includes many public domain and Creative Commons-licensed icons, searchable by concept using tags in dozens of languages.

There are, of course, numerous other sources for both free and for-sale SVG graphics; this is just a tiny sample.  Adding the word “SVG” to your query in any search engine can help target your results; Google’s Advanced image search also allows you to explicitly filter for SVG files.  The difficulty is then to find the original source of the graphic to determine licensing rights.

The vast majority of ready-to-use SVG files are static content—images created by Inkscape, Adobe Illustrator, and other similar tools that contain neither scripts nor animation.  A few clip art and icon libraries are starting to include animated SVG, while specialized applications such as mapping may include interactive features or links between files.
Tip
Typically, SVG in the wild is stored as text-based XML files.  However, the SVG standards allow for gzip-compressed SVG—typically having an *.svgz* filename extension—to reduce the file size. This is common for high-quality, photorealistic SVG files, which can occasionally get to be bigger than their PNG counterparts; and for maps and other complex charts that embed a lot of metadata within the graphics themselves.  It should also be used by a performance-minded web developer (that’s you!) to compress a file for transmission from web server to browser.

Ready-to-use graphics can solve many web design problems.  For common interface icons, creating your own graphics would seem like reinventing the wheel.

For other projects and other graphic types, stock art just won’t do.  You need to create a custom image that perfectly represents a new and unique concept.  It takes a little more artistic skill, but there are plenty of tools for creating your own SVG art.  After all, that’s how most of the graphics in the above libraries were created in the first place.
Click, Drag, Draw
Once upon a time, one of the biggest issues facing adoption of the Scalable Vector Graphics standard was the lack of decent tools for creating SVG graphics or applications. Most SVG needed to be created by hand, or if that was too daunting, to be converted from other proprietary graphical standards. This reliance on converted graphics meant that the full features of SVG weren’t always used.

On the other side, many vector graphics editors include features that are not part of standard SVG. To ensure that this extra information or features are retained when you save and reload the SVG (a process called “round-tripping”), these programs either have separate, proprietary image formats (as for Adobe Illustrator) or add extra markup to the SVG file (Inkscape).  In order to create graphics that will display consistently with other software, these programs also include commands that will “flatten” the extra features into standard SVG 1.1.  If your intent is to make content available for the Web, always ensure that you save a version of your graphic in standard SVG.  Be aware that sometimes the multiple representations of the image may add significantly to the file size.

There are now numerous graphical software programs that can export to SVG.  They all include visual, what-you-see-is-what-you-get (WYSIWYG) editors where you can position shapes with your mouse (or other pointing device) and select colors from visual palettes.  They differ in how much of SVG’s features they support, and in how easy they are to use.

Some vector graphics programs you could consider include:

Adobe Illustrator

Adobe Illustrator is the granddaddy of vector graphics programs, and debuted in 1991 as part of a seminal period in Adobe’s history. It not only set the expectations of what a vector graphics program should look like, but has consistently been at the cutting edge of vector graphics support for the past two decades.  Many aspects of SVG were inspired by the capabilities of Illustrator, and Illustrator has long supported export of its graphics to SVG format.  However, it’s definitely worth remembering that SVG is not a native format for the application (the *.ai* format is). What that means is that Adobe has to perform a translation from their internal vector graphics format (built primarily around Postscript) to SVG.  For comparatively simple graphics, this is a straightforward proposition, but it is possible to create Illustrator images that have poor fidelity and large file sizes when rendered to SVG, with complex effects replaced by bitmap images.

The basic save-as-SVG option in Illustrator creates a complex file from which the native graphic can be reconstructed.  However, in 2015 Adobe introduced a much more streamlined export-as-SVG option that creates a graphic optimized for the Web. You can also copy individual graphics components from the Illustrator interface and paste them into a text editor; the corresponding SVG code will be pasted. This is useful if you’re building a complex application or animation with SVG, but want to use the visual editor to draw shapes.

Inkscape and Sodipodi

Sodipodi was one of the earliest SVG editors, initially developed for Linux systems.  It drew its inspiration from Adobe Illustrator, but used SVG natively to store and retrieve content.  Inkscape started as a branch of Sodipodi, and is now the more actively developed program.

Inkscape has matured into a remarkably sophisticated, feature-rich vector graphics application, while never losing sight of its SVG roots.  Its interface (Figure 2-3) is somewhat crowded with features, but with a little effort to learn all the options, it allows for considerable control over the graphic.  In addition to supporting most static SVG features, it includes numerous filters and extensions to create graphical effects.  There are also controls that allow you to edit nongraphical aspects of the SVG, such as element IDs, alternative text, and even simple JavaScript event handling. You can also inspect the XML file and edit nodes and attributes directly.

Inkscape implemented some features of SVG 1.2, particularly multiline text boxes, which were never widely supported by browsers; be sure to convert your text boxes to fixed-position SVG text when exporting for the Web.  Other Inkscape-specific markup is removed by saving as a “Plain SVG” format.  Export options (set under the “SVG Output” tab in software preferences) can help optimize the final file, stripping unused style and attributes or simplifying the data used to encode complex paths.

Microsoft Visio

Although not a general-purpose graphics editor, Microsoft’s Visio software is used for designing charts and diagrams.  SVG is remarkably well-suited for charts, and Visio provided the setting for Microsoft’s first foray into SVG (as an alternative export format).  It turned out to be one of the more favorably received of features for this product, as it allowed diagrams to be used outside of the normal Microsoft family of products, including on the Web.

LibreOffice and Apache Open Office

These two open-source office software suites—which share a common history but are now developed separately—both include support for vector graphics, either embedded in text documents or as stand-alone drawings.  They use a conversion program (based on Batik, which is discussed later) to translate between SVG and the internal vector graphics format.  The conversion is not perfect, and won’t support advanced SVG features.  However, these drawing programs can be user-friendly introductions to drawing basic vector graphics.

Google Docs

Google’s web application alternative to desktop office software, Google Docs, uses SVG natively to encode and display drawings.  Furthermore, because the SVG is being displayed live in your web browser, you are seeing it exactly as it will be displayed in a website, and you can open the file in multiple browsers to confirm consistent rendering.  The interface is easy to use, but it only supports a basic set of SVG features, in order to support conversion to the drawing formats used by other office software.

SVG-edit

Another online SVG graphics application originally sponsored by Google, SVG-edit runs in your web browser either from a downloaded file or directly from the web server.  In addition to most of the standard visual vector graphics options, you can easily set element id and class attributes from the editor, add hyperlinks, and can define dimensions and a title for the document. Unfortunately, at the time of writing the program is not well documented and is somewhat prone to errors when editing complex content.  Development and issues can be monitored via the GitHub repository.

Draw SVG

A more complete (and more completely documented) online SVG graphics editor is Draw SVG by Joseph Liard.  It implements nearly all the commonly supported SVG drawing and styling features (no filters or animation).  The dialog forms that control style properties use standard SVG terminology for attributes and style properties, which is helpful if you will be alternating between using a graphics editor and writing the code yourself.  The application also offers tools to create rasterized versions of the SVG, or to encode raster images as data that you can embed in the SVG itself.

The performance of the web application itself can be slow compared to desktop applications like Inkscape, and drawing complex shapes is difficult.  The tool would likely be most useful for customizing the styles on icons and clip art from other sources, especially if you aren’t yet comfortable writing the markup and stylesheets yourself.  Figure 2-4 shows the interface.

Nearly all of these SVG editors have some ability to convert SVG graphics to raster images or other vector formats.  This can be useful to create fallbacks for old browsers or to create consistent rendering in print publications.  However, manually saving files in multiple formats from a graphics editor can be tedious.  On many web server set-ups, the process can be automated using dedicated rasterization and conversion tools.
SVG Snapshots
An SVG rasterizer is a program that converts the vector graphics elements into a visual bitmap format.  Broadly speaking, any program that can display an SVG on a screen is a rasterizer.  However, the programs described here are single-purpose rasterizers, used when incorporating SVG into print or when generating fallback alternatives for older browsers.  They are command-line programs or software libraries suitable for inclusion in automated routines.

Batik

The Apache Batik project is a complete implementation of SVG 1.1 in Java.  The project’s rasterizer utility has traditionally been used to render SVG in publishing pipelines, most typically to convert XSL Formatting Objects (XSL-FO) documents into printed output.  In general, the quality of the Batik rendering is quite high, and is particularly appropriate for generating images in raster formats such as PNG or JPEG from SVG graphics.  Batik supports nearly all features of SVG 1.1, but has not (at the time of writing) implemented CSS3 features that you might wish to use in SVG for modern web browsers.

Once downloaded, Batik can be run as a Java archive file. The static renderer is specifically *batik-rasterizer.jar*, part of the Batik distribution. There are a number of options for controlling output file format, width and height, area, quality, output directory, and so forth. Invoking the rasterizer without arguments should give you the list of options.

LibRSVG

The LibRSVG library is part of the Linux Gnome project, and provides support within Gnome for static SVG images as well as a library that lets developers generate SVG in their programs. It can also be run as a standalone program called *rsvg* to generate PNG or JPEG images from SVG files.  It supports core vector graphics, but not advanced effects.  The LibRSVG rendering engine is used in numerous other open source SVG tools.

ImageMagick

ImageMagick is best described as a Swiss army knife for command line manipulation of graphics, and is available on Windows, Macintosh, and Linux platforms. It is useful from a command line and also can be invoked from libraries in most modern processing languages, from C++ to Python, PHP, Ruby, and Node.js. Given this, it’s not surprising that it supports rasterization of SVG.

At its simplest, the ImageMagick convert command is trivial:

convert image.svg image.png

This converts the file indicated by the first argument from an SVG image to a corresponding PNG image.  When it is available, ImageMagick will use Inkscape’s command-line interface to render SVG; in that case, it will support most SVG 1.1 features.  As a second effort, ImageMagick will try to use LibRSVG. If that is not available, ImageMagick has its own rendering tools; these have less support for advanced features such as filters and stylesheets.  It is generally advisable to experiment with sample SVG images to see whether ImageMagick will meet your needs.

CairoSVG

Cairo is a complete vector graphics languages as a programming library for use by other software; it has implementations in many common programming languages such as C++ and Python.  Cairo graphics can be converted to vector files as PostScript, PDF, and SVG; can be output on various screen display modes on Linux and Macintosh systems; or can be used to generate raster images.  The CairoSVG library, from the web design company Kozea, parses SVG files and converts them to Cairo graphics—the result can be used to generate PDF, PostScript, or PNG versions of the SVG files.  Most basic vector graphics features from SVG 1.1 are supported.

As you may have gathered, a limitation of all these rasterization tools is that they do not keep up to date with the latest developments in other web platform specifications—if they even support the full SVG standard to begin with.

A final option for creating rasterized versions of SVG files is to use an actual web browser rendering engine.  To do this from a server routine or other command-line interface, you can use the PhantomJS implementation of the WebKit browser engines.  The sample *rasterize.js* script can be used to convert any web page or SVG file to PNG or PDF.  PhantomJS can also run your own JavaScript code, such as a script to build an SVG visualization from a data file, and then save the resulting SVG document.

With all these options for converting SVG to raster image formats, you may wonder about the reverse conversion.  Can you create an SVG from a PNG or JPEG?  That gets much more complicated.  Although SVG code contains information about how the shapes should look, raster images don’t contain information about the shapes they were constructed from.

Tracing or vectorizing tools use algorithms to try to calculate those shapes from the pixel data in a raster image, looking for high-contrast edges, then connecting them into smooth lines and curves.  The more comprehensive graphics programs, such as Illustrator and Inkscape, include auto-tracing tools.  There are also specialized tracing tools such as Vector Magic.  These can be particularly useful if you want to draw graphics by hand, then scan them into your computer and convert to SVG.
Bringing SVG Alive
Rendering SVG to other static file formats is useful, but the reason you’re using SVG in the first place is because it is so much more than a rasterized image.  To see and use the full power of SVG, you need a dynamic SVG viewer that can update the graphic according to user interaction or timed animations.

When discussing web browser support for SVG, it helps to group the browsers according to the rendering engine they use.  Many of the engines are open-source, and there are numerous other tools that use the same code, and therefore display web pages and SVG in the same way (although these tools may not always be up-to-date with the latest features).  Knowing the rendering engine also helps you know which prefix to use when testing experimental CSS features, although CSS prefixes are going out of fashion in favor of experimental browser modes controlled by the user.

The main rendering engines are as follows:

Gecko for Firefox

The first web browser implementation of SVG was built within the Gecko rendering engine in 2003.  Gecko, originally built for Netscape 6, is the basis of the Mozilla Firefox browser as well as numerous niche browsers and tools.

The original SVG implementation was basic, focusing on simple vector shapes.  However, it has expanded steadily and continues to improve.  Until recently, dynamic SVG could be slow and jerky in Firefox; however, over the course of 2014 significant performance improvements were made and some animations are now smoother in Firefox than in other browsers.

There are still some areas where Firefox/Gecko does not conform to the SVG specifications in the finer details, particularly around the way <use> elements are handled. They also did not implement many of the style properties that offer nuanced control of the layout of SVG text; some of these features are now (early 2016) being implemented in coordination with enhancements to CSS-styled HTML text.  SVG rendering may also differ slightly between operating systems, as Firefox uses low-level graphical rendering tools from the operating system to improve the performance of some actions.

Experimental CSS features for Gecko used the -moz- (for Mozilla) prefix; however, the development team has phased out the use of prefixed experimental CSS for new features.

WebKit for Safari and iOS devices

Apple’s Safari browser was built upon open-source rendering and JavaScript engines originally created for the KDE operating system (for Linux/Unix computers).  Apple’s branch of the code—known as WebKit—is used in all Apple devices and was also originally the basis for the Google Chrome browser, among many other tools.  As previously mentioned, WebKit is also used in the PhantomJS browser simulator.

WebKit implemented most SVG 1.1 features between 2008 and 2010; many edge cases or areas of poor performance remain, but for most purposes it is a complete implementation.  Many CSS3 features require a -webkit- prefix on Safari and related software.

Blink for newer versions of Chrome, Opera, and Android devices

In 2013, Google’s Chromium project announced that they would no longer synchronize further development with the WebKit project.  The Google Chrome browser at that point used WebKit code to render web pages (and SVG) but had separate code for other functions including JavaScript processing.

The branch of the rendering engine, developed as part of the Chromium project, is now known as Blink.  In addition to being used by Chrome, Blink is used in the Opera browser (since version 13) and some native applications on newer Android devices.

Blink browsers still support -webkit- CSS properties, but they have been transitioning to using standard (unprefixed) syntax.  Users can opt in to experimental features through advanced browser settings.

The development of the Google Chrome browser (and now Blink in general) has been heavily focused on performance; their SVG implementation is one of the best, and animations are generally fast and smooth (although Firefox has recently caught up).  Some edge-case features are not supported, particularly in areas where the SVG specifications work differently from CSS and HTML.  Blink has removed support for SVG Fonts from most platforms and the development team has indicated that they consider SVG animation elements (SMIL animation) to be deprecated.  At the time of writing (early 2016), animation elements still work, but a warning is displayed in the developer’s console.

Presto for older Opera versions and Opera Mini

The Opera browser previously used its own proprietary rendering engine, known as Presto.  It is still used for server-side rendering for the Opera Mini browser, converting web pages to much simpler compressed files for transmission to mobile devices with low computing power or expensive and slow Internet connections.  In Opera Mini, SVG is supported as static images, but not as interactive applications.

Presto supports nearly all of the SVG 1.1 specifications and some CSS3 properties.  However, it has not been (and will not likely be) updated since 2013.  Presto versions of Opera used an -o- prefix for experimental CSS features.

Trident for Internet Explorer and other Windows programs

Internet Explorer was the last major browser to add support for SVG.  Prior to the development of the SVG standard, Microsoft had introduced its own XML vector graphics language (the Vector Markup Language, VML), used in Microsoft Office software and supported in Internet Explorer since version 5.

Basic SVG support was introduced (and VML phased out) with Internet Explorer version 9 in 2009.  Support for additional SVG features, such as filters, was added in subsequent versions.  Nonetheless, older Internet Explorer versions that do not support SVG (particularly Internet Explorer 8) continue to be used because newer versions of the software are not supported on older Windows operating systems.  As of the end of 2015, slightly more than 1% of global web traffic used Internet Explorer 8, a two-third drop from a year previous but still a meaningful share for large commercial websites.1

As of Internet Explorer 11 (the final browser to use the Trident engine), there were a number of small quirks and bugs in SVG support, and some features that were not supported at all.  The main area where Internet Explorer does not match the other web browsers is animation: there is no support for either SVG animation elements or CSS animation applied to SVG graphics.  Another key missing feature is the <foreignObject> element, which allows XHTML content to be embedded in an SVG graphic.

The Trident rendering engine used for Internet Explorer is also used in other Microsoft programs and by some third-party software built for Windows devices.  It uses the -ms- CSS prefix.

EdgeHTML for Microsoft Edge and Windows 10+ programs

The Microsoft Edge browser developed for Windows 10 uses a new rendering engine, built from a clean codebase to emphasize performance and cross-browser interoperability.  The EdgeHTML engine is also used by other software in Windows 10.

Edge supports all the web standards supported in Internet Explorer, and many new ones.  Collaboration from Adobe developers helped advance support for a number of graphics and visual effects features.  Support for SVG <foreignObject> and CSS animations of SVG content have already been introduced, and the development team has indicated that they intend to implement many other SVG 2/CSS3 features.  However, plans to eventually support SVG animation elements were shelved after the Chromium project announced their deprecation plans.  Edge supports -ms- prefixed properties that were supported in Internet Explorer, and has also introduced support for some -webkit- prefixes that are commonly used in existing websites.

The browser SVG implementations (with the exception of Presto and Trident) are the subject of active development.  Feature support may have changed by the time you read this.  Consult the version release notes or the issue-tracking databases for the various browsers to determine if a specific feature is now supported.

In addition to the web browsers, there are a two other dynamic SVG rendering engines that have been important in the development of SVG:

Adobe SVG Viewer

As mentioned in Chapter 2 of *Using SVG with CSS3 and HTML5*, the Adobe SVG Viewer—a plug-in for Internet Explorer—was one of the first and most complete SVG environments.  Although it has not been developed for years, it can still be downloaded to enable SVG support on older Internet Explorer browsers.  In order to trigger the plug-in, the SVG must be included in the page using either an <object> or an <embed> tag.

Batik Squiggle Viewer

We mentioned the Apache Batik project in the context of SVG rasterizers; however, the rasterizer is only one part.  Batik can be used to generate and display SVG in other Java-based software.  It also comes with its own dynamic SVG viewer called Squiggle for viewing SVG files from your computer or the Web.

Squiggle can display SVG animation and can process JavaScript and respond to user events, including following hyperlinks to new files.  Batik supports nearly all of the SVG 1.1 specification, but has not been updated for more recent CSS, DOM, and JavaScript methods.  It can also be more strict, compared to browser implementations, about requiring common values to be explicitly specified in markup and in scripts.

These dynamic SVG viewers do not merely display an image of the SVG, they present changing, interactive SVG documents.  In order to create such a document, you’ll need to use more than the graphical editing programs presented in “Click, Drag, Draw”.  You’ll need to look inside the SVG, and work with the underlying code.
Markup Management
It is possible to write SVG code in any editor that can save in a plain text format.  You can open up Notepad or something similar, type in your markup, scripts, and styles, save it with a *.svg* extension, then open the same file in a web browser.

If you typed carefully, and didn’t forget any required attributes or misspell any values, your SVG will appear on screen, ready to be used just as you intended.  However, if you’re human, chances are—at least some of the time—you’ll end up with XML validation errors displayed on screen, with JavaScript errors printed to the developer’s console, or simply with a graphic that doesn’t look quite like you intended.

Text editors designed for writing code can help considerably.  They can color-code the syntax so it’s easy to detect a missing quotation mark or close bracket.  They can also test for major syntax errors before you save.  Many can autocomplete terms as you type.  The options for code editors are too numerous to list here; many are available only for specific operating systems.  Whatever you choose, be sure to confirm that the editor has—at a minimum—syntax rules for XML, or more preferably specific rules and hints for SVG.

Nonetheless, even the best syntax highlighting and code hints cannot help you *draw* with code.  When working with complex shapes and graphical effects, it really helps to be able to see the graphical effect of your code as you write it.

Oxygen XML SVG Editor

A commercial XML management program, Oxygen allows you to handle many types of XML-based data and formatting tools.  The SVG editor uses Batik to render graphics, and can render both SVG markup and SVG created via scripts.  It is intended primarily for creating SVG as the result of an XSLT (eXtensible Stylesheet Language Transformation) template applied to an XML data file, but can also be used for plain SVG.

Brackets plus SVG Preview

A code editor developed by Adobe primarily for web developers, Brackets includes a feature whereby you can open the web page you’re working on in a browser and have it update as you type in the main editor.  At the time of writing (Brackets version 1.1), this only works with HTML and CSS; SVG inline in those pages is displayed, but not updated live.  For standalone SVG files, the SVG Preview extension will display a live version of the SVG in the editor as you type; however, this should currently only be used for static SVG images, as script errors in your code can crash the editor.

The SVG Preview feature—and the entire Brackets editor—uses the Blink rendering engine.  The preview image is currently displayed as inline SVG code within the HTML 5 application code; this means that minor syntax errors and missing namespaces in a half-finished file do not break the preview.  However, it also means that inline scripts can wreak havoc, and external files and stylesheets are not supported.  Figure 2-5 shows the editor with a live preview of SVG icons.

Both the core Brackets code and extensions are being rapidly developed; however, as of early 2016 progress on SVG-focused features appear to have stalled.  Adobe is also developing software (Adobe Extract) to allow users of their commercial design software (e.g., Photoshop) to easily generate matching web code in Brackets; however, at the time of writing this tool primarily focuses on CSS and does not include any SVG- or Adobe Illustrator-related features.

Online live code sites

In recent years, numerous web applications have become available that allow you to write web code and see its output in separate frames of the same web page.  Because the result is displayed right in your web page, you can use all the latest features supported by your web browser.  Most make it easy to import common JavaScript code libraries.  However, since you don’t control the web server, other external files can often be limited by browser security restrictions.

All these sites currently work with HTML 5 documents, including inline SVG elements.  As with the Brackets SVG preview, this means that they are more forgiving of syntax errors than SVG in XML files.  Some live code sites worth mentioning include:

- 
JSFiddle was one of the first sites to offer live execution of web code that you can save to a publically accessible web link and send to collaborators or reference from help forums.  The stripped-down interface is best for small test cases and examples.

- 
CodePen is a more full-featured live code site that also serves as a social media network for web designers; you can explore other coders’ work, leave comments, or publish a blog with multiple embedded working examples in each post.  A paid membership opens up additional collaboration tools and the ability to upload separate image files, scripts, or other resources.

- 
Tributary is specifically designed for data visualizations and other scripted SVG.  By default, it provides you with a blank HTML page containing a single inline <svg> element that you can manipulate with JavaScript.  You can also create separate data files accessible from the main script.  The interface offers convenient tools such as visual color pickers and GIF snapshots (including animation) of your work.

When you’re working on these sites, keep in mind that saving your work usually also means publishing to the Web.  Some sites, such as CodePen, automatically apply a very-few-rights-reserved license to work published in this way (you can save work privately and control copyright with a paid CodePen membership).

Once you have tools that allow you to rapidly write and test your code, it becomes easier to think about SVG programmatically.  Working with the code forces you to consider the graphic from the perspective of the document object model rather than simply from the perspective of its final appearance.

This approach is particularly useful when dealing with dynamic and interactive SVG and when creating data visualizations.  If those areas interest you, the next set of tools you’ll want to investigate are those that make manipulating the DOM easier.
Ready-to-Use Code
There are two ways to create an SVG by writing code: writing out the XML markup and attributes, or writing JavaScript to create the corresponding DOM elements dynamically.  Scripting is preferred when you have a lot of similar elements or when the geometric attributes should be calculated based on a data file.  This book uses both approaches in the examples.
Note
There’s actually a third way to code SVG (which we mentioned briefly when discussing the Oxygen XML editor): using an XSLT stylesheet applied to an XML data file.

The XSLT stylesheet is an XML file.  It consists of SVG markup templates interspersed with formatting instruction elements that indicate how the data should be processed and inserted into the SVG file. XSLT is therefore another way to create SVG that should correspond with underlying data.  However, unlike with scripting, the XSL transformation can only be applied once, when the file is processed; it cannot update with new data or respond to user interactions.  With standardized JavaScript being well supported and efficiently implemented in browsers, the use of XSLT to generate SVG is falling out of favor.

The popularity of using JavaScript for the creation and manipulation of SVG has much to do with the availability of open-source tools to make this easier.  These libraries of JavaScript code provide shorthand methods to perform common tasks, allowing your script to focus on the graphics instead of on the underlying DOM function calls.  The following JavaScript libraries are particularly important when working with SVG:

Raphaël and Snap.svg

The Raphaël library by Dmitry Baranovskiy was important in getting dynamic SVG into production web pages.  It provides a single interface that can be used to create either SVG graphics or Microsoft VML graphics, depending on which one the browser supports.  The library is therefore essential if you want to provide dynamic vector graphics to users of Internet Explorer 8.  The number of features Raphaël supports, however, is limited to the shared features of the two vector graphics languages.

The terminology used by Raphaël includes a number of convenient shorthands that do not always directly correspond to the standard SVG element and attribute names.  The same terminology is used in the newer Snap.svg library, produced by Baranovskiy through his new employer, Adobe.  Unlike Raphaël, Snap.svg does not include support for VML graphics.  This allows the library code files to be smaller, and allows support for features such as clipping, masking, filters, and even groups, which aren’t supported in VML.  Snap can also load in existing SVG code, in order to manipulate complex graphics created in WYSIWYG editors.  Both Snap and Raphaël have convenient functions to create smooth JavaScript animations, allowing you to animate graphics in any version of Internet Explorer.

D3.js

The D3.js library, originally developed by Mike Bostock, has become the standard tool for creating dynamic SVG data visualizations.  D3 is short for *Data-Driven Documents*, and it reflects how the library works by associating JavaScript data objects with elements in the document object model (DOM).

The core D3 library is open-ended, allowing you to manipulate groups of DOM elements (SVG or HTML) simultaneously by defining how their attributes and styles should be calculated from the corresponding data objects.  Changes in values can be set to smoothly transition over time to create animated effects.

D3 includes a number of convenient functions for calculating the geometric properties of common data visualization layouts, such as the angles in a pie graph.  It also includes SVG-specific convenience functions for converting data and geometrical values into the actual instructions you’ll use in the attributes of an SVG <path> element.  However, D3 does not draw the charts directly; many extensions and add-ons have been developed to make it easier to draw common chart types.

GSAP

An animation-focused commercial library, the GreenSock Animation Platform focuses on making animated HTML and SVG content fast, smooth, and cross-browser compatible.  The GSAP library can be freely used on many commercial projects (and most noncommercial ones); a paid license is required if the website’s end users pay subscription or other fees, or to access various extra plug-in scripts.  A number of those plug-ins are specifically focused on working with SVG paths, or circumventing browser support issues at the intersection of SVG and CSS3.

Learning to use these JavaScript libraries is worth a book of its own (and many great books are available).  However, they don’t replace an understanding of the underlying SVG graphics.  It’s difficult to effectively manipulate SVG with scripts unless you already know what SVG is (and isn’t) capable of.
Processing and Packaging
You have your SVG ready to go, whether it came from a clip art library, was drawn in a graphics editor, or was carefully written as code.  There are still a few tools that you may want to use while uploading SVG to your web server.  A sample routine, which could be automated, would be to:

- 
Run your SVG code through an optimizing tool such as SVGO or Scour to eliminate extra markup from graphics tools and to otherwise condense the file (being sure not to use any settings that will remove IDs or classes that you’ll use in scripts or stylesheets).

- 
Generate raster fallback images for Internet Explorer 8 and Android 2.3 users (using any of the rasterization tools mentioned in “SVG Snapshots”).

- 
Compile a folder full of all individual SVG icons into a single file that can be sent to the user all at once (the SVGStore Grunt plugin does this on a Node/Grunt server configuration).

- 
Use gzip compression to further reduce file size (being sure that your server is set to correctly indicate the compression scheme in the HTTP headers sent with the file).

There are almost certainly many more tools and techniques that can be used, depending on how your website and server are set up, and on how you intend to use the SVG.  These examples should get you started.
Summary: Software and Sources to Make SVG Easier
The purpose of this chapter hasn’t been to tell you what software or which websites to use, although hopefully it has given you some suggestions if you did not know where to start.

More importantly, the review should have helped you understand the diversity of ways you can create and use SVG.  Furthermore, it should have helped remind you of the compatibility issues that must always be kept in mind when working on the web.  And finally, it should have helped you get some SVG files onto your computer—whether downloaded from a clip art library or created yourself in an editor—that you can experiment with as you work through the rest of the book.

This chapter has been revised many times since it was first started in 2011, in part due to the dramatic changes in the SVG software landscape.  It will surely be the first chapter in the book to become obsolete.  In the past few years, SVG has in many ways become a *de facto* standard for maps and information graphics on the Web, is becoming a commercially viable alternative for clip art, is making its way into graphics usage for component diagrams of everything from houses to aircraft to cars, and is factoring into web interfaces (and even operating system interfaces) in subtle but increasingly ubiquitous ways.

While the example sites and applications given here are a good start, other places to find out more about SVG include Infographics and Data Visualization Meetups, code libraries such as Google Code Projects, or online forums on LinkedIn or Google+ (both of which have a number of active SVG and data visualization groups).

As you’re following along with the rest of the book, feel free to use downloaded clip art or SVG you created in a graphics program to experiment with styles and effects.  It’s what you’ll often do in practice.  Opening the files in a code editor that highlights the syntax (and particularly one that can “tidy up” or “pretty print” the XML) can help you identify the core structure of group and shape elements in the code.  From there, you can add new attributes or CSS classes.

1 Data from *http://caniuse.com/usage_table.php*.