## **

The SVG examples in this book were for the most part created “from scratch,” with the markup or standard JavaScript being typed into a code editor.  However, that’s certainly not the only way to work with SVG, nor the most common one.

Most SVG drawings and original art start their life inside some kind of graphical software, created by an artist or designer working with shapes and colors rather than XML tags and attributes.

Most SVG data visualizations are created with JavaScript, and visualization libraries offer different degrees of abstraction between the author and the SVG code.

In most projects, SVG icons are imported from existing icon sets, with the SVG files manipulated entirely by project build scripts.

By showing you the internal components of an SVG, stripped down to their skeletal form, we hope to give you a complete toolset to work with SVG: the skills to modify and extend *any* SVG you work with, no matter how it was created.  With this programmatic approach to SVG, you will be better able to manipulate graphics created by others or by software, in order to match your web design or to enable user interaction.  But this mental toolset you’ll gain by understanding SVG shouldn’t detract from the software tools that other developers have created.

Software tools make it easier to create graphics and process files so they are ready to deploy on your web server and display on your pages.  The tools discussed in this chapter include:

- 
graphical editors that emphasize visual components rather than code

- 
code editors that provide hints and immediate feedback

- 
libraries that streamline the creation of dynamic graphics with JavaScript

- 
rendering programs that display the SVG or convert it into other image formats

In addition, we introduce the vast panoply of free and licensable SVG content that can help you quickly enhance your web development process and designs, even if your personal artistic skills don’t extend beyond stick figures.

This chapter specifically mentions some of the most popular software and services.  These are not the only options, and we don’t guarantee they are the best.  They are given as examples of what is out there, and of how different options vary from one another.  When choosing tools for your own projects, think about the features you need, and the budget you can afford.

Whatever you choose, remember that any standards-compliant SVG file can usually be opened and modified with other SVG tools, so you’re not locked into the workflow of a particular product or vendor.
Ready-to-Use SVG
The easiest way to get started with SVG—especially if you’re more of a programmer than a graphic designer—is to start with someone else’s art.  SVG may not be as ubiquitous on the web as other image formats, but it’s getting there.

The simplest method is to start searching for your term of interest plus “SVG,” which will yield a broad result in most search engines. For more precision, you will want to refine your search: Google Images is a good start, although you can refine your search by using the “Search tools” option and choosing “Line Art” or “Clip Art” under the “Type” menu option. Alternatively, typing “*subject* file​type:SVG” directly into Google will also work.

Prior to using clip art from a vendor or website, you should ascertain what license it is provided under.  Early SVG was mostly produced within open source communities, with graphics often released under a Creative Commons license, but today, high-quality SVG artwork is produced by professional artists expecting to be compensated for their work.

Although there are plenty of free-to-use graphics available (some with noncommercial restrictions or attribution requirements), others are offered under paid license systems similar to those used for stock photos or web fonts.
Tip
One benefit of SVG’s dual nature as both an image format and an XML format is that it is possible to embed copyright license information directly in the file using a <metadata> block.  We’ll discuss how you can do this for your own graphics in Chapter 17.

For accessing graphics created by others, remember that creative works are by default “all rights reserved”; the *absence* of a copyright declaration does not mean a work is public domain.  Don’t use someone else’s work unless you are sure that the original creator has offered a license compatible with your intended use.

SVG will never replace JPEG for stock photographs (which can’t be efficiently represented in vectors), but it is now a standard option for vector graphic clip art and icons, including those provided by commercial suppliers.

There are a number of tools and libraries that can convert simple SVG art into other vector formats and back again.  This can increase the flexibility of the vector graphics: for example, the encapsulated PostScript (EPS) format, long a staple in the realm of clip art, is still dominant in print.  For simpler graphics, icon fonts—which allow sets of single-color icons to be distributed as a web font file—are popular because they allow icon size and color to be controlled with familiar CSS properties.  Nonetheless, companies that produce clip art, maps, and related graphics for the web are increasingly shifting to SVG for their vector graphic format.

Using a vector graphic as a source file, stock art companies can  generate raster images (PNG, JPEG, and GIF) of any size on demand.  For a web designer interested in purchasing raster graphics, however, it often makes more sense to license a single SVG and convert it into the required raster format at the needed resolutions yourself, rather than  purchasing raster graphics at different scales.

The following sites should help start you on your search for SVG:

Open Clip Art Project

The Open Clip Art Library (OCAL) Project is definitely the oldest, and perhaps the largest, repository of SVG content, all of it available either through Creative Commons or public domain licenses for unrestricted commercial use.

Established in 2004 by Jon Phillips and Bryce Harrington, the OCAL project was created to provide a public commons for clip art, using SVG for encoding primarily because the format typically doesn’t have the same type of vendor encumbrances or royalty restrictions as other proprientary formats.  Moreover, because the source code for the graphics can be read with a text editor, it’s also possible to decompose clip art images into separate pieces, making SVG useful as a way of packaging collections of icons or images in a single file.  Figure 4-1 displays some of the diverse artistic styles available.

The project is also integrated with the Flaming Text ImageBot graphics editor, which allows you to tweak some SVG style properties online.

Wikimedia Commons

The media repository arm of Wikipedia, Wikimedia Commons compiles images, audio, and video in a wide variety of formats.  All files are available under some sort of “copyleft” license; some require attribution or are restricted to noncommercial use or to use within similarly licensed work.  Detailed license information is available on each file’s catalogue page.

Wikimedia is actively pushing their contributors to use the SVG format for diagrams, clip art, icons, and other vector drawings because of its flexibility and ease of editing; their servers then automatically generate raster versions in various sizes.  The tagging and cataloguing of files on Wikipedia is often inconsistent, making searching a little difficult.  But there is plenty of great SVG content if you take the time to look around.  Figure 4-2 displays selections from the SVG Botanical Illustrations category, including a labeled diagram; because SVG files are easily editable, the file is available with labels in many languages.

SVG is used for most maps, logos, and diagrams on Wikipedia.

Iconic

Iconic is a commercial SVG icon library, but they offer a set of more than 200 icons completely free to use (MIT license; you must ensure that license information is available in the file).  This Open Iconic set includes most common user interface buttons in single-element icons that you style to any color you choose.  For their paid offerings, Iconic distinguishes themselves by taking full advantage of all the possibilities of SVG, supporting multicolor styling and using scripts to substitute in more detailed versions of the icons at larger sizes.  They even brag about their easy-to-read (and modify) XML markup.

The Noun Project

Another icon-focused library, the Noun Project aims to create a visual language for clear international communication.  Access to their entire library is by monthly subscription, but their database includes many public domain and Creative Commons–licensed icons, which users can search by concept using tags in dozens of languages.
Tip
Typically, SVG in the wild is stored as text-based XML files.  However, the SVG standards allow for Gzip-compressed SVG—typically having a *.svgz* filename extension—to reduce the file size. This is common for high-quality, photorealistic SVG files, which can occasionally get to be bigger than their PNG counterparts, and for maps and other complex charts that embed a lot of metadata within the graphics themselves.  However, using *.svgz* on the web can be tricky.  Even if you get your server configuration correct, browsers can get it wrong if the user then saves the received file.

File compression should nonetheless always be used by a performance-minded web developer (that’s you!) to compress an SVG, HTML, or CSS file for transmission from web server to browser—either with Gzip or the newer Brotli algorithm.  The compression is usually applied through web server settings, and is indicated via HTTP headers, rather than with a file extension.

Ready-to-use graphics can solve many web design problems.  For common interface icons, creating your own graphics may often feel like reinventing the wheel.

But for other projects, or other graphic types, stock art just won’t do.  You need to create a custom image that perfectly represents a new and unique concept.  It takes a little more artistic skill, but there are plenty of tools for creating your own SVG art.  After all, that’s how most of the graphics in these collections were created in the first place.
Click, Drag, Draw: Graphical SVG Editors
Once upon a time, one of the biggest issues facing adoption of the Scalable Vector Graphics standard was the lack of decent tools for creating SVG graphics. Most SVG needed to be coded by hand, or—if that was too daunting—to be converted from other proprietary graphical standards. This reliance on converted graphics meant that the full features of SVG weren’t always used.

On the other side, many vector graphics editors include features that are not part of the standard SVG specification. To ensure that this extra information and features are retained when you save and reload the SVG (a process called “round-tripping”), these programs either have separate, proprietary image formats (like Adobe Illustrator’s *.ai* format) or add extra markup to the SVG file (as Inkscape does).  To create graphics that will display consistently with other software, these programs also include commands that will “flatten” the extra features into standard SVG 1.1.

If you are creating SVG for the web, always ensure that you export the final version of your graphic in standard SVG.  Without this step, the file may be many times larger than the “pure” SVG version.  This will slow your site and complicate any future code you try to write.
Tip
You can often fix a bloated, proprietary SVG export with an optimization tool (discussed in “Processing and Packaging”).  But it’s always better to make the most of the web-export tools from the original software first.

There are now numerous graphical applications that can export files as SVG; this section lists just a sample.

The common feature of these apps is the visual, what-you-see-is-what-you-get (WYSIWYG) editor, where you can position shapes with your mouse (or stylus, or finger) and select colors from on-screen palettes.  They differ in the range of SVG features they support, and in how easy they are to use.  We offer some tips and warnings about how to get web-quality output from each.
Adobe Illustrator
Adobe Illustrator is the granddaddy of vector graphics programs, and debuted in 1987.  Illustrator not only set the expectations of what a vector graphics program should look like, but has consistently been at the cutting edge of vector graphics support for the past two decades.

Many aspects of SVG were inspired by the capabilities of Illustrator, and Illustrator has long supported export of its graphics to SVG format.  However, it’s definitely worth remembering that SVG is not a native format for the application (the *.ai* format is). This means that Illustrator must perform a conversion from its internal vector graphics format (built primarily around PostScript) to SVG.  For comparatively simple graphics, this is a straightforward process, but it is possible to create Illustrator images that have poor fidelity and large file sizes when rendered to SVG, with the application replacing complex vectors with embedded bitmap images.

The basic save-as-SVG option in Illustrator creates a complex file from which the native graphic can be reconstructed.  However, a much more streamlined export-as-SVG option was introduced in Adobe Illustrator CC 2015, which creates a vector graphic optimized for the web.

In the latest (CC, or Creative Cloud) versions of Illustrator, you can also copy individual graphics components from Illustrator and paste them into a text editor; the corresponding SVG code will be pasted.  (If it doesn’t work, look for the “On Copy: Include SVG Code” option in the “File Handling and Clipboard” settings.)  Alternatively, you can select “Copy to SVG” from the right-click context menu of a layer; this is your only option if the selected object is text (copying a text element normally just copies the text content).
Tip
Copying and pasting markup for individual elements is useful if you’re building a complex application or animation in SVG.  You can use the visual editor to draw shapes, without having Illustrator mangle the rest of your markup.

Avoid using “Illustrator filters” in your graphic, as the application doesn’t yet translate them well into SVG; use the “SVG filters” instead. Similarly, blending modes (Multiply, Dissolve, etc.) aren’t yet translated into SVG-compatible CSS blend modes (as described in Chapter 16).  If you use these, remove them before export and then edit the CSS yourself to add them back in.  Finally, be sure all stroked shapes use centered strokes, as inside/outside strokes are not yet supported in SVG.
Adobe Photoshop
The most recent (CC) versions of Adobe Photoshop can also export SVG documents.  While Photoshop is primarily a bitmap (raster image) editor, it supports vector shapes and text, making this a useful option to be aware of.

To export SVG from Photoshop, name the layer or group with the intended name of the file: for example, *icon.svg*. Then, use File → Generate → Image Assets.

The generated SVG document will be exported into a folder provided with the name of the originating PSD file, with your named SVG documents inside the folder. By default, this folder will appear on your desktop, although you can change this in Photoshop’s preferences.

At the time of writing, there are several features of the exported SVG file to keep in mind:

- 
The exported SVG is responsive (it has a viewBox attribute on the root <svg> element) but with default sizes (it also has width and height attributes).

- 
The viewBox is automatically cropped to the edges of the largest vector shape in the current design.

- 
Elements are given their own unique classes in an embedded style in the SVG.

- 
Any bitmap images will be turned into inline base-64 data URIs in the SVG.

As an alternative to generating an SVG as an image asset, you can use Photoshop’s File → Export options, including setting up preferences to make SVG the default “Quick Export” option.
One upside of the image assets approach is that Photoshop will automatically update the exported SVG with any changes made to the original PSD document. It’s expected that Adobe Illustrator will offer similar functionality in
the near future.

The open-source alternative to Photoshop, GIMP (the GNU Image Manipulation Program) also has some vector features and SVG export of certain graphic components.  GIMP’s SVG export is optimized for readability, which is great if you’re going to be manipulating it in a code editor—but less so if you are worried about minimizing file sizes.
Sketch
Sketch is a Mac-only program that has proved extremely popular with user interface designers.  Unfortunately, the SVG export capabilities (at the time of writing) are stuck where Adobe Illustrator was five years ago…although that’s not to say that the result can’t be improved and cleaned up.

First, Sketch must be informed that a shape can be exported as SVG: you’ll find this option at the bottom of the properties panel for a selected shape. Sketch will deliver each shape as its own separate SVG file, unless multiple shapes are merged.

By default, Sketch applies stroke to elements on the inside, which (as previously discussed) SVG does not yet support.  The latest versions adjust your path sizes to match the stroke; for more predictable export, ensure that Sketch’s preferences are set to stroke paths in their center.

A typical export of SVG code for a simple path will include a lot of extraneous code; Sketch adds its own proprietary code in the output, under the sketch namespace.

To create the cleanest possible SVG output from Sketch, adhere to the same rules we established for Illustrator, with a few additions:

- 
Create an artboard for each drawing (Insert → Artboard), and one drawing (such as an icon) per artboard.

- 
Remove any bounding boxes from the drawing.

- 
Don’t attempt to rotate your drawing in Sketch before export, as doing so will (in current versions) significantly distort the SVG export.

Following these rules will eliminate many of the current issues with SVG export from Sketch, but not all of them; you may have more success copying and pasting Sketch drawing objects into another vector application, such as Illustrator.
Inkscape and Sodipodi
Sodipodi was one of the earliest SVG editors, initially developed for Linux systems.  It drew its inspiration from Adobe Illustrator, but used SVG natively to store and retrieve content.  Inkscape started as a branch of Sodipodi, and is now the more actively developed program.

Inkscape has matured into a remarkably sophisticated, feature-rich vector graphics application, while never losing sight of its SVG roots.  It is available for Linux, Windows, and Mac, but the Mac version relies on the XQuartz adapter (for running Unix-based software on Mac), and can be difficult to use.

The interface (Figure 4-3) is somewhat crowded with features, but if you put in a little effort to learn all the options it allows for considerable control over the graphic.  In addition to supporting most static SVG features, it includes numerous filters and extensions to create graphical effects.  There are also controls that allow you to edit nongraphical aspects of the SVG, such as element IDs, alternative text, and even simple JavaScript event handling. You can also inspect the XML file and edit nodes and attributes directly.

There are a few features in Inkscape that can make SVG exports easier:

- 
Use File → “Clean up document” to remove unused <defs> elements.

- 
If you are copying an element for use elsewhere in the same drawing, create a Clone (Edit → Clone), which generates a <use> reference to the original element. This reduces output code size and makes later editing far easier.

- 
Use Path → Simplify to reduce the number of points in an over-drawn element, further reducing file size.

Inkscape uses SVG as its own native format, but with extra application-specific data in its own XML namespaces.
In addition, Inkscape implemented some features of SVG 1.2, particularly multiline text boxes, which were never widely supported by browsers; be sure to convert your text boxes to fixed-position SVG text when exporting for the web.

When saving your file, you’ll need to be very clear exactly what kind of SVG you are saving from Inkscape: “Inkscape” SVG, with its extra embedded code, or “plain” SVG.  Inkscape also has an “Optimized SVG” option.

For the web, you either want the plain version or the optimized version; if you use the optimized option, consider the export settings carefully so you don’t accidentally remove content that you need for later styling or scripting.
Draw SVG
Draw SVG is a comprehensive online SVG editor, developed primarily by Joseph Liard.  It is under active development, with intent to support SVG 2 features as they become supported by browsers, and performance has significantly improved since we first reviewed it.  Figure 4-4 shows the interface.

Draw SVG implements essentially all the SVG drawing and styling features commonly supported by browsers (except animation).  It even supports HTML <foreignObject> content for embedding audio, video, or formatted text paragraphs, and multilingual text using the <switch> element.

The interface uses standard SVG terminology for attributes and style properties, which is helpful if you will be alternating between using a graphics editor and writing the code yourself.  You can add IDs and classes to elements, and set the language attribute for text, although at the time of writing it does not seem to support alternative text (e.g., <title>).  Drawing complex curved shapes can be nonintuitive if you’re used to the instant feedback of other drawing tools.

The application also offers tools to create rasterized versions of the SVG, and to encode raster images as embedded data URIs.

There are now many other browser-based online SVG editors, some with more features than others, but all of which use the web browser to convert the code to graphics.  We already mentioned the Flaming Text ImageBot editor that is paired with the Open Clip Art Library; the commercial Vecteezy clip art library has their own online SVG editor, which is free to use as a stand-alone SVG editor.
Boxy SVG
A different approach to a browser-based SVG editor, Boxy SVG is a full graphical editor in the form of a Chrome plug-in; in use, it is indistinguishable from a desktop application.  A website demo works in any web browser.  It has an attractive interface and excellent support for SVG features, including new features such as blend modes.  Equally importantly, it is being actively developed.
Bringing SVG Alive: SVG in the Web Browser
To see and use the full power of SVG, as more than just an image, you need a dynamic SVG viewer that can update the graphic according to user interaction or timed animations.  And if you’re building SVG for the web, it’s best to test it in web browsers as early and often as possible.

When discussing web browser support for SVG, it helps to group the browsers according to the **rendering engine** (drawing code) that they use.  Many of the engines are open source; numerous other tools use the same code.  Different applications using the same rendering engines *usually* display web pages and SVG in the same way.  For that reason, we focus on the major browser rendering engines (Chrome/Blink, Safari/WebKit, Firefox/Gecko, Internet Explorer, and Microsoft EdgeHTML) in all the browser support warnings in the book.

Knowing the rendering engine also tells you which prefixes were used for experimental CSS features.  CSS prefixes are going out of fashion—all the major browsers have pledged not to introduce new prefixed CSS properties for web content—but some features are still only supported in some browsers with a prefixed syntax.

Conversely, some of the most widely used prefixed properties have now been adopted by all new browsers as deprecated synonyms for the standard CSS.  Content creators should, of course, use the unprefixed versions.

If you need to add prefixes to support older browsers, use a dedicated software tool to make the conversion.  Of course, you should also make sure your designs still function if a feature isn’t supported at all—so you may find that prefixes aren’t worth the hassle.

New features are now usually enabled with experimental browser modes (or “flags”) controlled by the user.

This section reviews the history of the main rendering engines when it comes to SVG, and summarizes major support issues.  However—with the exception of Presto and Trident—the browser SVG implementations are all under active development.  Feature support may have changed by the time you read this.  Consult the version release notes or the issue-tracking databases for the various browsers to determine if a specific feature is now supported.
Gecko for Firefox
The first web browser implementation of SVG was built within the Gecko rendering engine in 2003.  Gecko, originally built for Netscape 6, is the basis of the Mozilla Firefox browser as well as numerous niche browsers and tools.

The original SVG implementation was basic, focusing on simple vector shapes.  However, it has expanded steadily and continues to improve.  Until around 2014, dynamic SVG could be slow and jerky in Firefox; however, significant performance improvements have been made and some animations are now smoother in Firefox than in other browsers.

There are still some areas where Firefox/Gecko does not conform to the SVG specifications in the finer details, particularly around the way <use> elements are handled. The rendering engine also did not initially implement many of the style properties that offer nuanced control of the layout of SVG text; some of these features are now (mid-2017) being implemented in coordination with enhancements to CSS-styled HTML text.  SVG rendering may also differ slightly between operating systems, as Firefox uses low-level graphical rendering tools from the operating system to improve the performance of some actions.

Experimental CSS features for Gecko used the -moz- (for Mozilla) prefix; since mid-2016, Firefox also supports the most common -webkit- properties.
WebKit for Safari and iOS Devices
Apple’s Safari browser was built upon open source rendering and JavaScript engines originally created for the KDE operating system (for Linux/Unix computers).  Apple’s branch of the code—known as WebKit—is used in all Apple devices and was also originally the basis for the Google Chrome browser, among many other tools.  WebKit is also used in the PhantomJS browser simulator.

WebKit implemented most SVG 1.1 features between 2008 and 2010; many edge cases or areas of poor performance remain, but for most purposes it is a complete implementation.  Up until recently, many CSS3 features required a -webkit- prefix on Safari and related software, leading to the proliferation of those prefixes in the wild.  However, the development team has now committed to transitioning away from prefixes, and Safari 9 and 10 support unprefixed versions of the most commonly used properties, such as those for animations and transforms.

On iOS (the operating system used by iPhone and iPad), *all* web browsers and apps use WebKit, even Firefox and Chrome.  It’s a requirement of the Apple App Store.
Blink for Newer Versions of Chrome, Opera, and Android Devices
In 2013, Google’s Chromium project announced that they would no longer synchronize further development with the WebKit project.  The Google Chrome browser at that point used WebKit code to render web pages (and SVG) but had separate code for other functions including JavaScript processing.

The branch of the rendering engine, developed as part of the Chromium project, is now known as Blink.  In addition to being used by Chrome, Blink is used in the Opera browser (since version 13) and in native applications on newer Android devices.  It is also used by other new browsers, such as Vivaldi and Brave, and by the Samsung Internet browser on Samsung Android devices.

Blink browsers still support -webkit- CSS properties, although not necessarily those introduced since the split.  They have user settings (flags) to allow developers test out their own new features.

Initial development of the Google Chrome browser (and now Blink in general) was heavily focused on performance; animations are generally fast and smooth (although Firefox has since caught up).  Some edge-case features are not supported, particularly in areas where the SVG specifications work differently from CSS and HTML.  Blink has removed support for SVG fonts from most platforms, and the development team has indicated that they would eventually like to deprecate SVG animation elements (SMIL animation) in favor of CSS or scripted animations.
Presto for Older Opera Versions and Opera Mini
The Opera browser previously used its own proprietary rendering engine, known as Presto.  It is still used for server-side rendering for the Opera Mini browser, converting web pages to much simpler compressed files for transmission to mobile devices with low computing power or expensive and slow internet connections.  In Opera Mini, SVG is supported as static images, but not as interactive applications.

Presto supports nearly all of the SVG 1.1 specifications and some CSS3 properties.  However, it has not been (and will not likely be) substantially updated since 2013.  Furthermore, Opera Mini has intentionally chosen not to implement many decorative CSS effects that require too much memory or computation to recreate on a low-power mobile phone.  Opera Mini does support the @supports rule (see Chapter 3), so you can use that to adjust your styles if necessary.

Presto versions of Opera used an -o- prefix for experimental CSS features, but it is unlikely to be useful in modern websites.
Trident for Internet Explorer and Other Windows Programs
Internet Explorer was the last major browser to introduce SVG support.  Prior to the development of the SVG standard, Microsoft had introduced its own XML vector graphics language (the Vector Markup Language, or VML), used in Microsoft Office software and supported in Internet Explorer since version 5.

Basic SVG support was introduced (and VML phased out) with Internet Explorer version 9 in 2009.  Support for additional SVG features, such as filters, was added in subsequent versions.  Nonetheless, older Internet Explorer versions that do not support SVG (particularly Internet Explorer 8) continue to be used because newer versions of the software are not supported on older Windows operating systems.  As of the end of 2016, slightly more than 0.6% of global web traffic used Internet Explorer 8, a steady drop from previous years but still a meaningful share for very large commercial websites.1

As of Internet Explorer 11 (the final version of the browser), there were a number of small quirks and bugs in SVG support, and some features that were not supported at all.  The main area where Internet Explorer does not match the other web browsers is animation: there is no support for either SVG animation elements or CSS animation applied to SVG graphics.  Another key missing feature is the <foreignObject> element, which allows HTML content to be embedded in an SVG graphic.

The Trident rendering engine used for Internet Explorer is also used in other Microsoft programs and by some third-party software built for Windows devices.  It used the -ms- CSS prefix, but there are only a few properties where prefixes make a difference.
EdgeHTML for Microsoft Edge and Windows 10+ Programs
The Microsoft Edge browser developed for Windows 10 uses a new rendering engine, built from a clean codebase to emphasize performance and cross-browser interoperability.  The EdgeHTML engine is also used by other software in Windows 10.

Edge supports all the web standards supported in Internet Explorer, and many new ones.  Collaboration from Adobe developers helped advance support for a number of graphics and visual effects features.  Support for SVG <foreignObject> and CSS animations of SVG content has already been introduced, as has tabindex in SVG. The development team has indicated that they intend to implement many other SVG2/CSS3 features.  However, plans to eventually support SVG animation elements were shelved after the Chromium project announced their deprecation plans.

Edge uses two version numbers: one for the application interface version, and one for the EdgeHTML rendering engine.  This book uses the EdgeHTML numbers, since those are what affect web standards support.

For backward compatibility, Edge supports -ms- prefixed properties that were supported in Internet Explorer, and also introduced support for some -webkit- prefixes that are commonly used in existing websites.  However, you shouldn’t normally be adding prefixes for MS Edge support.
Servo
The Mozilla foundation is sponsoring the development of a new browser rendering engine, Servo, that may one day replace Gecko at the core of Firefox.  It is being built from scratch in Rust, a programming language optimized for parallel computing environments.  At the time of writing, developement work on SVG rendering within Servo is at the very early stages; you can track their open issue to add support.
Other Dynamic SVG Viewers
In addition to the web browsers, there are two other dynamic SVG rendering engines that have been important in the development of SVG:

Adobe SVG viewer

As mentioned in Chapter 1, the Adobe SVG viewer—a plug-in for Internet Explorer—was one of the first and most complete SVG environments.  Although it has not been developed for years, it can still be downloaded to enable SVG support on older Internet Explorer browsers.  To trigger the plug-in, the SVG must be included in the page via either an <object> or an <embed> tag.

Batik Squiggle viewer

The Apache Batik project is a complete implementation of SVG 1.1 in Java.  Batik can be used to generate and display SVG in other Java-based software, and has a rasterizer tool that can be used from the command line (or from command-line scripts).  It also comes with its own dynamic SVG viewer called Squiggle for viewing SVG files from your computer or the web.

Squiggle can display SVG animation and can process JavaScript and respond to user events, including following hyperlinks to new files.  Batik supports nearly all of the SVG 1.1 specification, but has not been updated for more recent CSS, DOM, and JavaScript methods.  It can also be more strict, compared to browser implementations, about requiring common values to be explicitly specified in markup and in scripts.

The web browsers and other dynamic SVG viewers do not merely display an image of the SVG—they present changing, interactive SVG documents.  To create such a document, you’ll need to use more than the graphical editing programs presented in “Click, Drag, Draw: Graphical SVG Editors”.  You’ll need to look inside the SVG, and work with the underlying code.
Markup Management: Code Editors
It is possible to write SVG code in any editor that can save in a plain-text format.  You can open up Notepad or something similar; type in your markup, scripts, and styles; save it with a *.svg* extension; and then open the same file in a web browser.

If you typed carefully, and didn’t forget any required attributes or misspell any values, your SVG will appear onscreen, ready to be used just as you intended.  However, if you’re human, chances are—at least some of the time—you’ll end up with XML validation errors displayed onscreen, with JavaScript errors printed to the developer’s console, or simply with a graphic that doesn’t look quite like you intended.

Text editors that are designed for writing code can help considerably.  They can color-code the syntax so it’s easy to detect a missing quotation mark or close bracket.  They can also test for major syntax errors before you save.  Many can autocomplete terms as you type.  The options for code editors are too numerous to list here; many are available only for specific operating systems.  Whatever you choose, be sure to confirm that the editor has—at a minimum—syntax rules for XML, or more preferably specific rules and hints for SVG.

Nonetheless, even the best syntax highlighting and code hints cannot help you *draw* with code.  When you’re working with complex shapes and graphical effects, it really helps to be able to see the graphical effect of your code as you write it.  SVG preview features (often as separately installed extensions) are thankfully becoming more popular in code editors.  This section covers only a few of the options.

Once you have tools that allow you to rapidly write and test your code, it becomes easier to think about SVG programmatically.  Working with the code forces you to consider the graphic from the perspective of the document object model rather than simply from the perspective of its final appearance.
Atom Plus SVG Preview
A code editor developed by GitHub, Atom has a friendly interface that opens up with lots of tips for new coders, but it also has many features for power users, and many more available via extensions.

For standalone SVG files, the SVG Preview extension will display a live version of the SVG in the editor as you type. The preview image is currently displayed as inline SVG code; this means that minor syntax errors and missing namespaces in a half-finished file do not break the preview.  Unfortunately, it also means that external files and stylesheets are not supported, and that <style> blocks from one preview affect another.  The SVG Preview feature—and the entire editor—uses the Blink rendering engine, via GitHub’s Electron framework for creating applications with web technologies.  Figure 4-5 shows side-by-side views of the same SVG file in code and preview mode.
Brackets Plus SVG Preview
A code editor developed by Adobe primarily for web developers, Brackets includes a feature whereby you can open the web page you’re working on in a browser and have it update as you type in the main editor.  At the time of writing (Brackets version 1.8), the live updates only work with HTML and CSS; SVG inline in those pages is displayed, but not updated live.  Nonetheless, the integrated local web server can be quite useful for testing.  It needs to be started with an HTML file, but you can switch to an SVG file by editing the URL in your browser.

There is an SVG Preview extension for Brackets.  It is independent of the Atom SVG Preview, but has many of the same features.  However, it should *only* be used for static SVG images, as script errors in your code can crash the editor.  Like the Atom preview, it uses inline code rendered (by Blink) within the HTML5 application, and has the same issues.  However, the code for the Brackets SVG Preview is currently injected without sandbox restrictions, and inline scripts can wreak havoc.  Unfortunately, there’s not an easy way to turn the preview on and off.  Figure 4-6 shows the editor with a live preview of SVG icons.

Despite active development of both the core Brackets code and extensions, as of early 2017 there has not been noticeable progress on SVG-focused features for a couple years.  Adobe is also developing software (Adobe Extract) to allow users of their commercial design software (e.g., Photoshop) to easily generate matching web code in Brackets; however, at the time of writing this tool primarily focuses on CSS and does not include any features related to SVG or Adobe Illustrator.
Oxygen XML SVG Editor
A commercial XML management program, Oxygen allows you to handle many types of XML-based data and formatting tools.  The SVG editor uses Batik to render graphics, and can render both SVG markup and SVG created via scripts.  It is intended primarily for creating SVG as the result of an XSLT (eXtensible Stylesheet Language Transformation) template applied to an XML data file, but can also be used for plain SVG.
Online Live Code Sites
In recent years, numerous web applications have become available that allow you to write web code and see its output in separate frames of the same web page.  Because the result is displayed right in your web page, you can use all the latest features supported by your web browser.  Most make it easy to import common JavaScript code libraries.  However, since you don’t control the web server, other external files can often be limited by browser security restrictions.

All these sites currently work with HTML5 documents, including inline SVG elements.  As with the live SVG previews for code editors, this means that they are more forgiving of syntax errors than SVG in XML files.  Some live code sites worth mentioning include:

- 
JSFiddle was one of the first sites to offer live execution of web code that you can save to a publicly accessible web link that you can send to collaborators or reference from help forums.  The stripped-down interface is best for small test cases and examples.

- 
CodePen is a more full-featured live code site that also serves as a social media network for web designers; you can explore other coders’ work, leave comments, or publish a blog with multiple embedded working examples in each post.  A paid “Pro” membership opens up additional collaboration tools and the ability to upload separate image files, scripts, or other resources.

- 
Tributary is specifically designed for data visualizations and other scripted SVG.  By default, it provides you with a blank HTML page containing a single inline <svg> element that you can manipulate with JavaScript.  You can also create separate data files accessible from the main script.  The interface offers convenient tools such as visual color pickers and GIF snapshots (including animation) of your work.

When you’re working on these sites, keep in mind that saving your work usually also means publishing to the web.  Some sites, such as CodePen, automatically apply a very-few-rights-reserved license to work published in this way (you can save work privately and control copyright with a paid CodePen membership).

Because the live code sites support live preview of JavaScript as well as markup, they are particularly useful when you’re dealing with dynamic and interactive SVG and when creating data visualizations.  If those areas interest you, the next set of tools you’ll want to investigate are JavaScript tools to make manipulating the DOM easier.
Ready-to-Use Code: JavaScript Libraries
There are two ways to create an SVG by writing code: writing out the XML markup and attributes, or writing JavaScript to create the corresponding DOM elements dynamically.  Scripting is preferred when you have a lot of similar elements or when the geometric attributes should be calculated based on a data file.  This book uses both approaches in the examples.
Note
There’s actually a third way to code SVG (which we mentioned briefly when discussing the Oxygen XML editor): using an XSLT stylesheet applied to an XML data file.

The XSLT stylesheet is an XML file.  It consists of SVG markup templates interspersed with formatting instruction elements that indicate how the data should be processed and inserted into the SVG file. XSLT is therefore another way to create SVG that should correspond with underlying data.

However, unlike with scripting, the XSL transformation can only be applied once, when the file is processed; it cannot be updated with new data or respond to user interactions.  With standardized JavaScript being well supported and efficiently implemented in browsers, the use of XSLT to generate SVG is falling out of favor.

The popularity of using JavaScript for the creation and manipulation of SVG has much to do with the availability of open source tools to make this easier.  These libraries of JavaScript code provide shorthand methods to perform common tasks, allowing your script to focus on graphics instead of underlying DOM function calls.

The following JavaScript libraries are particularly important for working with SVG.  Learning to use these JavaScript libraries is worth a book of its own (and many great books are available).  However, they don’t replace an understanding of the underlying SVG graphics.  It’s difficult to effectively manipulate SVG with scripts unless you already know what SVG is (and isn’t) capable of.
Raphaël and Snap.svg
The Raphaël library by Dmitry Baranovskiy was important in getting dynamic SVG into production web pages.  It provides a single interface that can be used to create either SVG graphics or Microsoft VML graphics, depending on which one the browser supports.  The library is therefore essential if you want to provide dynamic vector graphics to users of Internet Explorer 8.  The number of features Raphaël supports, however, is limited to the shared features of the two vector graphics languages (SVG and VML).

The terminology used by Raphaël includes a number of convenient shorthands that do not always directly correspond to the standard SVG element and attribute names.  The same terminology is used in the newer Snap.svg library, produced by Baranovskiy through his new employer, Adobe.  Unlike Raphaël, Snap.svg does not include support for VML graphics.  This keeps the size of the library code files down, and allows support for features such as clipping, masking, filters, and even groups, which aren’t supported in VML.  Snap can also load in existing SVG code, in order to manipulate complex graphics created in WYSIWYG editors.  Both Snap and Raphaël have convenient functions to create smooth JavaScript animations, allowing you to animate graphics in any version of Internet Explorer.
D3.js
The D3.js library, originally developed by Mike Bostock, has become the standard tool for creating dynamic SVG data visualizations.  D3 is short for *Data-Driven Documents*, and it reflects how the library works by associating JavaScript data objects with elements in the DOM.

The core D3 library is open-ended, allowing you to manipulate groups of DOM elements (SVG or HTML) simultaneously by defining how their attributes and styles should be calculated from the corresponding data objects.  Changes in values can be set to smoothly transition over time to create animated effects.

D3 includes a number of convenient functions for calculating the geometric properties of common data visualization layouts, such as the angles in a pie graph.  It also includes SVG-specific convenience functions for converting data and geometrical values into the actual instructions you’ll use in the attributes of an SVG <path> element.  However, D3 does not draw the charts directly; many extensions and add-ons have been developed to make it easier to draw common chart types.
GSAP
An animation-focused commercial library, the GreenSock Animation Platform focuses on making animated HTML and SVG content fast, smooth, and cross-browser compatible.  The GSAP library can be freely used on many commercial projects (and most noncommercial ones); a paid license is required if the site’s end users pay a subscription or other fees, or to access various extra plug-in scripts.  A number of those plug-ins are specifically focused on working with SVG paths, or circumventing browser support issues at the intersection of SVG and CSS3.
SVG.js
SVG.js is a lightweight library for drawing, manipulating, and animating SVG elements.  It doesn’t offer much new functionality compared to “vanilla JS,” but it offers a much friendlier, more compact API for creating elements and setting attributes.  It also allows you to create simple animations and transitions.
Processing and Packaging
You have your SVG ready to go, whether it came from a clip art library, was drawn in a graphics editor, or was carefully written as code.  There are still a few tools that you may want to use while uploading SVG to your web server.  A sample routine, which could be automated, would be to:

- 
Run your SVG code through an optimizing tool such as SVGO or Scour to eliminate extra markup from graphics tools and to otherwise condense the file (being sure not to use any settings that will remove IDs or classes that you’ll use in scripts or stylesheets).  SVGO, in particular, has a community of tools built around it, integrating it into most popular web development toolchains.

- 
Generate raster fallback images for Internet Explorer 8 and Android 2.3 users (using any of the rasterization tools mentioned in *https://oreillymedia.github.io/Using_SVG/extras/ch04-rasterizers.html*).

- 
Compile a folder full of all individual SVG icons into a single file that can be sent to the user as a whole (the SVGStore Grunt plug-in does this on a Node/Grunt server configuration).

- 
Use Gzip or Brotli compression to further reduce file size; if you do the compression in advance, instead of dynamically as part of the server request, be sure that your server is set to correctly indicate the compression scheme in the HTTP headers sent with the file.
Tip
Automated optimizing tools like SVGO can be risky, sometimes altering the image in ways that affect the shape of curves.  If using an automated tool, stick to the milder optimizations.

Alternatively, there are two online graphical interfaces to SVGO: SVG Editor and SVG-OMG. Both allow you to adjust options and see the results immediately, while also allowing you to look at the parsed code.  SVG OMG has the benefit that, after you have visited the site once, it can run offline through the power of Service Workers.

There are almost certainly many more tools and techniques that can be used, depending on how your website and server are set up, and on how you intend to use the SVG.  These examples should get you started.
Summary: Software and Sources to Make SVG Easier
The purpose of this chapter hasn’t been to tell you what software or which websites to use, although hopefully it has given you some suggestions if you did not know where to start.

More importantly, the review should have helped you understand the diversity of ways you can create and use SVG.  It *also* should have reminded you of the compatibility issues that you must always keep in mind when working on the web.  And finally, it should have helped you get some SVG files onto your computer—whether downloaded from a clip-art library or created in an editor—that you can experiment with as you work through the rest of the book.

This chapter has been revised many times since it was first started in 2011, in part due to the dramatic changes in the SVG software landscape.  It will surely be the first chapter in the book to become obsolete.

In the past few years, SVG has in many ways become a de facto standard for maps and information graphics on the web, is becoming a commercially viable alternative for clip art, is making its way into graphics usage for component diagrams of everything from houses to aircraft to cars, and is factoring into web interfaces (and even operating system interfaces) in subtle but increasingly ubiquitous ways.

While the example sites and applications given here are a good start, other places to find out more about SVG include infographics and data visualization meetups, or online forums on LinkedIn or Google+ (both of which have a number of active SVG and data visualization groups).

As you’re following along with the rest of the book, feel free to use downloaded clip art or SVG you created in a graphics program to experiment with styles and effects.  It’s what you’ll often do in practice.  Opening the files in a code editor that highlights the syntax (and particularly one that can “tidy up” or “pretty print” the XML) can help you identify the core structure of group and shape elements in the code.  From there, you can add new attributes or CSS classes.

For Part II, however, we will focus on creating graphics entirely with code, defining the raw shapes that make up your image using elements and attributes.

1 Data from *http://gs.statcounter.com*