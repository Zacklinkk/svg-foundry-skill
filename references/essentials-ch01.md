XML/WEB DESIGNSVG Essentials
ISBN: 978-1-449-37435-8US $39.99   CAN $41.99“ The fi rst  e dition  o f  SVG 
Essentials  w as h ow  I  
first l earned  S VG, b ack  
in 2 002.  T hat  w orked   
out p retty  w ell  f or m e. 
 I t's g reat  t o  s ee a n  
updated  ed ition  f or  
modern  b rowsers  a nd 
today's  d evelopers  a nd 
designers ”
—Doug Schepers
W3C, SVG Working Group
Twitter: @oreillymedia
facebook.com/oreillyLearn the essentials of Scalable Vector Graphics, the markup language 
used by most vector drawing programs and interactive web graphics tools. SVG Essentials  takes you through SVG’s capabilities, beginning with simple 
line drawings and moving through complicated features such as filters, transformations, gradients, and patterns.
This thoroughly updated edition includes expanded coverage of animation, 
interactive graphics, and scripting SVG. Interactive examples online make it easy for you to experiment with SVG features in your web browser. Geared toward experienced designers, this book also includes appendices that explain basic concepts such as XML markup and CSS styling, so even if you have no web design experience, you can start learning SVG.
 ■Create and style graphics to match your web design in a way that looks great when printed or displayed on high-resolution screens
 ■Make your charts and decorative headings accessible to search engines and assistive technologies
 ■Add artistic effects to your graphics, text, and photographs using SVG masks, filters, and transformations
 ■Animate graphics with SVG markup, or add interactivity with CSS and JavaScript
 ■Create SVG from existing vector data or XML data, using 
programming languages and XSLT
J. David Eisenberg is a programmer and instructor in San Jose, California. He’s 
developed courses for CSS, JavaScript, CGI, and XML, and teaches Computer 
Information Technology courses at Evergreen Valley College. David has written 
Études for Erlang  (O’Reilly) and Let’s Read Hiragana  (Eisenberg Consulting), as well 
as SVG Essentials , First Edition.
Amelia Bellamy-Royds  is a freelance writer specializing in scientific and technical 
communication. She helps promote web standards and design through participation in online communities such as Web Platform Docs, Stack Exchange, and Codepen. SVG 
Essentials
PRODUCING SCALABLE VECTOR  
GRAPHICS WITH XML
J. David Eisenberg &
Amelia Bellamy-Royds  2nd Edition
SVG Essentials
SECOND 
EDITIONEisenberg & 
Bellamy-Royds
www.allitebooks.com


XML/WEB DESIGNSVG Essentials
ISBN: 978-1-449-37435-8US $39.99   CAN $41.99“ The fi rst  e dition  o f  SVG 
Essentials  w as h ow  I  
first l earned  S VG, b ack  
in 2 002.  T hat  w orked   
out p retty  w ell  f or m e. 
 I t's g reat  t o  s ee a n  
updated  ed ition  f or  
modern  b rowsers  a nd 
today's  d evelopers  a nd 
designers. ”
—Doug Schepers
W3C, SVG Working Group
Twitter: @oreillymedia
facebook.com/oreillyLearn the essentials of Scalable Vector Graphics, the markup language 
used by most vector drawing programs and interactive web graphics tools. SVG Essentials  takes you through SVG’s capabilities, beginning with simple 
line drawings and moving through complicated features such as filters, transformations, gradients, and patterns.
This thoroughly updated edition includes expanded coverage of animation, 
interactive graphics, and scripting SVG. Interactive examples online make it easy for you to experiment with SVG features in your web browser. Geared toward experienced designers, this book also includes appendices that explain basic concepts such as XML markup and CSS styling, so even if you have no web design experience, you can start learning SVG.
 ■Create and style graphics to match your web design in a way that looks great when printed or displayed on high-resolution screens
 ■Make your charts and decorative headings accessible to search engines and assistive technologies
 ■Add artistic effects to your graphics, text, and photographs using SVG masks, filters, and transformations
 ■Animate graphics with SVG markup, or add interactivity with CSS and JavaScript
 ■Create SVG from existing vector data or XML data, using 
programming languages and XSLT
J. David Eisenberg is a programmer and instructor in San Jose, California. He’s 
developed courses for CSS, JavaScript, CGI, and XML, and teaches Computer 
Information Technology courses at Evergreen Valley College. David has written 
Études for Erlang  (O’Reilly) and Let’s Read Hiragana  (Eisenberg Consulting), as well 
as SVG Essentials , First Edition.
Amelia Bellamy-Royds  is a freelance writer specializing in scientific and technical 
communication. She helps promote web standards and design through participation in online communities such as Web Platform Docs, Stack Exchange, and Codepen. SVG 
Essentials
PRODUCING SCALABLE VECTOR  
GRAPHICS WITH XML
J. David Eisenberg &
Amelia Bellamy-Royds  2nd Edition
SVG Essentials
SECOND 
EDITIONEisenberg & 
Bellamy-Royds
www.allitebooks.com


J. David Eisenberg and Amelia Bellamy-RoydsSECOND EDITION
SVG Essentials
www.allitebooks.com


SVG Essentials, Second Edition
by J. David Eisenberg and Amelia Bellamy-Royds
Copyright © 2015 J. David Eisenberg and Amelia Bellamy-Royds. All rights reserved.
Printed in the United States of America.
Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.
O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are
also available for most titles ( http://safaribooksonline.com ). For more information, contact our corporate/
institutional sales department: 800-998-9938 or corporate@oreilly.com .
Editors: Simon St. Laurent and Meghan Blanchette
Production Editor: Matthew Hacker
Copyeditor: Jasmine Kwityn
Proofreader: Sharon WilkeyIndexer: Ellen Troutman Zaig
Cover Designer: Karen Montgomery
Interior Designer: David Futato
Illustrator: Rebecca Demarest
February 2002: First Edition
October 2014: Second Edition
Revision History for the Second Edition:
2014-10-02: First release
2015-05-01: Second release
See http://oreilly.com/catalog/errata.csp?isbn=9781449374358  for release details.
The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. SVG Essentials , the image of a great argus
pheasant, and related trade dress are trademarks of O’Reilly Media, Inc.
Many of the designations used by manufacturers and sellers to distinguish their products are claimed as
trademarks. Where those designations appear in this book, and O’Reilly Media, Inc., was aware of a trade‐
mark claim, the designations have been printed in caps or initial caps.
While every precaution has been taken in the preparation of this book, the publisher and authors assume
no responsibility for errors or omissions, or for damages resulting from the use of the information contained
herein.
ISBN: 978-1-449-37435-8
[LSI]
www.allitebooks.com


To my late mother and father, for their advice and love through the years.—JDE
For Bill, who would have been so proud.—ABR
www.allitebooks.com


www.allitebooks.com


Table of Contents
Preface. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  xi
1.Getting Started. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  1
Graphics Systems                                                                                                               1
Raster Graphics                                                                                                               1
Vector Graphics                                                                                                              2
Uses of Raster Graphics                                                                                                 2
Uses of Vector Graphics                                                                                                 3
Scalability                                                                                                                            3
SVG’s Role                                                                                                                           5
Creating an SVG Graphic                                                                                                 6
Document Structure                                                                                                      6
Basic Shapes                                                                                                                    6
Specifying Styles as Attributes                                                                                      7
Grouping Graphic Objects                                                                                            8
Transforming the Coordinate System                                                                         9
Other Basic Shapes                                                                                                       10
Paths                                                                                                                               11
Text                                                                                                                                 12
2.Using SVG in Web Pages. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  15
SVG as an Image                                                                                                              15
Including SVG in an <img> Element                                                                        16
Including SVG in CSS                                                                                                  17
SVG as an Application                                                                                                     18
SVG Markup in a Mixed Document                                                                             20
Foreign Objects in SVG                                                                                               20
Inline SVG in XHTML or HTML5                                                                            22
SVG in Other XML Applications                                                                               25
v
www.allitebooks.com


3.Coordinates. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  27
The Viewport                                                                                                                    27
Using Default User Coordinates                                                                                    28
Specifying User Coordinates for a Viewport                                                               30
Preserving Aspect Ratio                                                                                                  32
Specifying Alignment for preserveAspectRatio                                                       33
Using the meet Specifier                                                                                              34
Using the slice Specifier                                                                                               35
Using the none Specifier                                                                                              36
Nested Systems of Coordinates                                                                                      36
4.Basic Shapes. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  39
Lines                                                                                                                                   39
Stroke Characteristics                                                                                                      40
stroke-width                                                                                                                  40
Stroke Color                                                                                                                  41
stroke-opacity                                                                                                               43
stroke-dasharray Attribute                                                                                          44
Rectangles                                                                                                                         45
Rounded Rectangles                                                                                                     46
Circles and Ellipses                                                                                                          47
The <polygon> Element                                                                                                 48
Filling Polygons That Have Intersecting Lines                                                        49
The <polyline> Element                                                                                                  51
Line Caps and Joins                                                                                                         52
Basic Shapes Reference Summary                                                                                 53
Shape Elements                                                                                                             53
Specifying Colors                                                                                                         54
Stroke and Fill Characteristics                                                                                    54
5.Document Structure. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  57
Structure and Presentation                                                                                             57
Using Styles with SVG                                                                                                     58
Inline Styles                                                                                                                   58
Internal Stylesheets                                                                                                      58
External Stylesheets                                                                                                      59
Presentation Attributes                                                                                                60
Grouping and Referencing Objects                                                                               61
The <g> Element                                                                                                          61
The <use> Element                                                                                                      63
The <defs> Element                                                                                                     63
The <symbol> Element                                                                                               66
vi | Table of Contents
www.allitebooks.com


The <image> Element                                                                                                  67
6.Transforming the Coordinate System. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  69
The translate Transformation                                                                                        69
The scale Transformation                                                                                               71
Sequences of Transformations                                                                                       74
Technique: Converting from Cartesian Coordinates                                                  76
The rotate Transformation                                                                                             78
Technique: Scaling Around a Center Point                                                                  81
The skewX and skewY Transformations                                                                      81
Transformation Reference Summary                                                                            83
CSS Transformations and SVG                                                                                      83
7.Paths. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  85
moveto, lineto, and closepath                                                                                         85
Relative moveto and lineto                                                                                             88
Path Shortcuts                                                                                                                   88
The Horizontal lineto and Vertical lineto Commands                                           88
Notational Shortcuts for a Path                                                                                  89
Elliptical Arc                                                                                                                     90
Converting from Other Arc Formats                                                                            93
Bézier Curves                                                                                                                    93
Quadratic Bézier Curves                                                                                             94
Cubic Bézier Curves                                                                                                    96
Path Reference Summary                                                                                                99
Paths and Filling                                                                                                             100
The <marker> element                                                                                                 100
Marker Miscellanea                                                                                                       104
8.Patterns and Gradients. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  107
Patterns                                                                                                                            107
patternUnits                                                                                                                108
patternContentUnits                                                                                                  110
Nested Patterns                                                                                                           112
Gradients                                                                                                                         113
The linearGradient Element                                                                                     113
The radialGradient Element                                                                                     118
Gradient Reference Summary                                                                                  121
Transforming Patterns and Gradients                                                                        122
9.Text. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  125
Text Terminology                                                                                                           125
Table of Contents | vii
www.allitebooks.com


Simple Attributes and Properties of the <text> Element                                         126
Text Alignment                                                                                                               129
The <tspan> Element                                                                                                    129
Setting textLength                                                                                                          132
Vertical Text                                                                                                                    133
Internationalization and Text                                                                                       134
Unicode and Bidirectionality                                                                                   134
The <switch> Element                                                                                               135
Using a Custom Font                                                                                                 137
Text on a Path                                                                                                                 138
Whitespace and Text                                                                                                     141
Case Study: Adding Text to a Graphic                                                                        142
10. Clipping and Masking. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  145
Clipping to a Path                                                                                                          145
Masking                                                                                                                           149
Case Study: Masking a Graphic                                                                                   152
11. Filters. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  155
How Filters Work                                                                                                           155
Creating a Drop Shadow                                                                                               156
Establishing the Filter’s Bounds                                                                               156
Using <feGaussianBlur> for a Drop Shadow                                                         157
Storing, Chaining, and Merging Filter Results                                                      158
Creating a Glowing Shadow                                                                                         159
The <feColorMatrix> Element                                                                                 160
More About the <feColorMatrix> Element                                                            161
The <feImage> Filter                                                                                                     163
The <feComponentTransfer> Filter                                                                            164
The <feComposite> Filter                                                                                            169
The <feBlend> Filter                                                                                                     172
The <feFlood> and <feTile> Filters                                                                            173
Lighting Effects                                                                                                              174
Diffuse Lighting                                                                                                          175
Specular Lighting                                                                                                       177
Accessing the Background                                                                                            179
The <feMorphology> Element                                                                                    181
The <feConvolveMatrix> Element                                                                              182
The <feDisplacementMap> Element                                                                          184
The <feTurbulence> Element                                                                                       186
Filter Reference Summary                                                                                            187
viii | Table of Contents
www.allitebooks.com


12. Animating SVG. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  191
Animation Basics                                                                                                           192
How Time Is Measured                                                                                                 194
Synchronizing Animation                                                                                            194
Repeated Action                                                                                                             196
Animating Complex Attributes                                                                                   197
Specifying Multiple Values                                                                                           198
Timing of Multistage Animations                                                                               199
The <set> Element                                                                                                         200
The <animateTransform> Element                                                                             200
The <animateMotion> Element                                                                                  202
Specifying Key Points and Times for Motion                                                            204
Animating SVG with CSS                                                                                             205
Animation Properties                                                                                                206
Setting Animation Key Frames                                                                                207
Animating Movement with CSS                                                                              207
13. Adding Interactivity. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  209
Using Links in SVG                                                                                                       209
Controlling CSS Animations                                                                                        211
User-Triggered SMIL Animations                                                                               212
Scripting SVG                                                                                                                 213
Events: An Overview                                                                                                 216
Listening for and Responding to Events                                                                 217
Changing Attributes of Multiple Objects                                                               218
Dragging Objects                                                                                                        221
Interacting with an HTML Page                                                                              225
Creating New Elements                                                                                             229
14. Using the SVG DOM. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  233
Determining the Value of Element Attributes                                                           233
SVG Interface Methods                                                                                                 241
Constructing SVG with ECMAScript/JavaScript                                                      248
Animation via Scripting                                                                                                251
Using JavaScript Libraries                                                                                             256
Event Handling in Snap                                                                                                261
Clicking Objects                                                                                                         262
Dragging Objects                                                                                                        262
15. Generating SVG. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  265
Converting Custom Data to SVG                                                                                266
Using XSLT to Convert XML Data to SVG                                                                270
Table of Contents | ix

Defining the Task                                                                                                       270
How XSLT Works                                                                                                       272
Developing an XSL Stylesheet                                                                                  273
A. The XML You Need for SVG. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  285
B. Introduction to Stylesheets. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  299
C. Programming Concepts. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  309
D. Matrix Algebra. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  319
E. Creating Fonts. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  327
F. Converting Arcs to Different Formats. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  331
Index. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .  335
x | Table of Contents

Preface
SVG Essentials  introduces you to the Scalable Vector Graphics XML file format. SVG,
a recommendation from the World Wide Web Consortium, uses XML to describe
graphics that are made up of lines, curves, and text. This rather dry definition does not
do justice to the scope and power of SVG.
Y ou can add SVG graphics to an Extensible Stylesheet Language Formatting Objects
(XSL-FO) document, and convert the combined document to Adobe PDF format for
high-quality printouts. Mapmakers and meteorologists are using SVG to create highly
detailed graphic images in a truly portable format. Web developers are embedding SVG
in web pages to create high-resolution, responsive graphics with small file sizes. All of
the diagrams in this book were originally created in SVG. As you learn and use SVG,
you’re sure to think of new and interesting uses for this technology.
Who Should Read This Book?
Y ou should read this book if you want to
•Create SVG files in a text or XML editor
•Create SVG files from existing vector data
•Transform other XML data to SVG
•Use JavaScript to manipulate the SVG document object tree
Who Should Not Read This Book?
If you simply want to view SVG files, you need only acquire a viewer program or plug-
in for the Web, download the files, and enjoy them. There’s no need for you to know
what’s going on behind the scenes unless you wish to satisfy your lively intellectual
curiosity.
xi

If you wish to create SVG files with a drawing program that has SVG export capability,
just read that program’s documentation to learn how to use that program feature.
If You’re Still Reading This…
If you’ve decided that you should indeed read this book, you should also be aware that
most of the people who use this book will be fairly advanced users, quite probably from
a technical background rather than a graphics design background. We didn’t want to
burden them with a lot of basic material up front, but we did want the book to be
accessible to people with no background in XML or programming, so we created a
number of introductory chapters—and then put them in the back of the book as ap‐
pendixes. If you haven’t used XML or stylesheets (and this could include some of the
technical folks!) or have never programmed, you might want to turn first to the appen‐
dixes. A complete list of all the chapters and appendixes with details on what they contain
is given later in this preface.
If you’re one of the technical types, you definitely need to be aware that this book will
not make you a better artist, any more than a book on word processing algorithms will
make you a better writer. This book gives the technical details of scalable vector graphics;
to create better art, you need to learn to see, and the book you should read in addition
to this one is The New Drawing on the Right Side of the Brain  by Dr. Betty Edwards
(Tarcher).
This book gives you the essentials of SVG; if you want to find out all the details, you
should go straight to the source, the W3C SVG specifications .
About the Examples
The examples in this book, except for those that involve HTML pages, have been tested
with the Batik SVG viewer on a system running GNU/Linux. The Batik SVG viewer is
an application of the software developed by the Apache Software Foundation’s Batik
project. This cross-platform software, written in Java, is available as open source under
the Apache Software License and can be freely downloaded from the project website .
All the examples (including those in Chapters 2, 13, and 14 that involve JavaScript and
HTML) were tested by being loaded into the Firefox and Chrome web browsers. The
level of support for the more sophisticated features of SVG differs depending upon the
browser.
As you look through the illustrations in this book, you will find that they are utterly
lacking in artistic merit. There are reasons for this. First, each example is intended to
illustrate a particular aspect of SVG, and it should do so without additional visual
distractions . Second, one of the authors (David) becomes terribly depressed when he
looks at other books with impossibly beautiful examples; “I can never draw anything
xii | Preface

that looks like this, ” he thinks. In an effort to save you from similar distress, the examples
are purposely as simple (or simplistic) as possible. As you look at them, your immediate
reaction will be: “I can certainly use SVG to draw something that looks far better than
this!” Y ou can, and you will.
Organization of This Book
Chapter 1, Getting Started
This chapter gives a brief history of SVG, compares raster and vector graphics sys‐
tems, and ends with a brief tutorial introducing the main concepts of SVG.
Chapter 2, Using SVG in Web Pages
This chapter shows you the various methods that you can use to put SVG into your
HTML5 documents.
Chapter 3, Coordinates
How do you determine the position of a point in a drawing? Which way is “up”?
This chapter answers those questions, showing how to change the system by which
coordinates are measured in a graphic.
Chapter 4, Basic Shapes
This chapter shows you how to construct drawings using the basic shapes available
in SVG: lines, rectangles, polygons, circles, and ellipses. It also discusses how to
determine the colors for the outline and interior of a shape.
Chapter 5, Document Structure
In a complex drawing, there are elements that are reused or repeated. This chapter
tells you how to group objects together so they may be treated as a single entity and
reused. It also discusses use of external images, both vector and raster.
Chapter 6, Transforming the Coordinate System
If you draw a square on a sheet of stretchable material, and stretch the material
horizontally, you get a rectangle. Skew the sides of the sheet, and you see a paral‐
lelogram. Now tilt the sheet 45 degrees, and you have a diamond. In this chapter,
you will learn how to move, rotate, scale, and skew the coordinate system to affect
the shapes drawn on it.
Chapter 7, Paths
All the basic shapes are actually specific instances of the general concept of a path.
This chapter shows you how to describe a general outline for a shape by using lines,
arcs, and complex curves.
Chapter 8, Patterns and Gradients
This chapter adds more to the discussion of color from Chapter 4 , discussing how
to create a color gradient or a fill pattern.
Preface | xiii

Chapter 9, Text
Graphics aren’t just lines and shapes; text is an integral part of a poster or a schematic
diagram. This chapter shows how to add text to a drawing, both in a straight line
and following a path.
Chapter 10, Clipping and Masking
This chapter shows you how to use a clipping path to display a graphic as though
it were viewed through a circular lens, keyhole, or any other arbitrary shape. It also
shows how to use a mask to alter an object’s transparency so that it appears to “fade
out” at the edges.
Chapter 11, Filters
Although an SVG file describes vector graphics, the document is eventually ren‐
dered on a raster device. In this chapter, you’ll learn how to apply raster-oriented
filters to a graphic to blur an image, transform its colors, or produce lighting effects.
Chapter 12, Animating SVG
This chapter shows you how to use SVG’s built-in animation capabilities.
Chapter 13, Adding Interactivity
In addition to SVG’s built-in animation, you can use both CSS and JavaScript to
dynamically control a graphic’s attributes.
Chapter 14, Using the SVG DOM
This chapter goes further in depth with using JavaScript to manipulate the Docu‐
ment Object Model. It also gives a brief introduction to a JavaScript library designed
for working with SVG.
Chapter 15, Generating SVG
Although you can create an SVG file from scratch, many people will have existing
vector data or XML data that they wish to display in graphic form. This chapter
discusses the use of programming languages and XSLT to create SVG from these
data sources.
Appendix A, The XML You Need for SVG
SVG is an application of XML, the Extensible Markup Language. If you haven’t used
XML before, you should read this appendix to familiarize yourself with this re‐
markably powerful and flexible format for structuring data and documents.
Appendix B, Introduction to Stylesheets
Y ou can use stylesheets to apply visual properties to particular elements in your
SVG document. These are exactly the same kind of stylesheets that can be used with
HTML documents. If you’ve never used stylesheets before, you’ll want to read this
brief introduction to the anatomy of a stylesheet.
xiv | Preface

Appendix C, Programming Concepts
If you’re a graphic designer who hasn’t done much programming, you’ll want to
find out what programmers are talking about when they throw around words like
object model  and function .
Appendix D, Matrix Algebra
To fully understand coordinate transformations and filter effects in SVG, it’s helpful,
though not necessary, to understand matrix algebra, the mathematics used to com‐
pute the coordinates and pixels. This appendix highlights the basics of matrix
algebra.
Appendix E, Creating Fonts
TrueType fonts represent glyphs (characters) in a vector form. This appendix shows
you how to take your favorite fonts and convert them to paths for use in SVG
documents.
Appendix F, Converting Arcs to Different Formats
Many applications represent arcs in a center-and-angles format. This appendix
provides code to convert from that format to SVG’s format for arcs and back again.
Conventions Used in This Book
Italic
Indicates new terms, URLs, email addresses, filenames, and file extensions.
Constant width
Used for program listings, as well as within paragraphs to refer to program elements
such as variable or function names, databases, data types, environment variables,
statements, and keywords.
Constant width bold
Used to highlight a section of code being discussed in the text.
Constant width italic
Shows text that should be replaced with user-supplied values or by values deter‐
mined by context.
This element signifies a tip, suggestion, or general note.
Preface | xv

This element indicates a warning or caution.
This book uses callouts  to denote points of interest  in code listings. A callout is shown
as a number in a filled circle; the corresponding number after the listing gives an ex‐
planation. Here’s an example:
Roses are red,
   Violets are blue. 
Some poems rhyme;
   This one doesn't. 
Violets actually have a color value of #9933cc .
This poem uses the literary device known as a surprise ending .
Many of the examples are available to test out online; the URL is indicated in the text.
Some of the online examples have markup that you can edit; click the Refresh button
to see the results of your changes. Y ou may also click the Reset button to return the
example to its original state.
Safari® Books Online
Safari Books Online  is an on-demand digital library that
delivers  expert  content  in both  book  and video  form  from
the world’s leading authors in technology and business.
Technology professionals, software developers, web designers, and business and crea‐
tive professionals use Safari Books Online as their primary resource for research, prob‐
lem solving, learning, and certification training.
Safari Books Online offers a range of plans and pricing  for enterprise , government ,
education , and individuals.
Members have access to thousands of books, training videos, and prepublication manu‐
scripts in one fully searchable database from publishers like O’Reilly Media, Prentice
Hall Professional, Addison-Wesley Professional, Microsoft Press, Sams, Que, Peachpit
Press, Focal Press, Cisco Press, John Wiley & Sons, Syngress, Morgan Kaufmann, IBM
Redbooks, Packt, Adobe Press, FT Press, Apress, Manning, New Riders, McGraw-Hill,
Jones & Bartlett, Course Technology, and hundreds more . For more information about
Safari Books Online, please visit us online .
xvi | Preface

How to Contact Us
Please address comments and questions concerning this book to the publisher:
O’Reilly Media, Inc.
1005 Gravenstein Highway North
Sebastopol, CA 95472
800-998-9938 (in the United States or Canada)
707-829-0515 (international or local)
707-829-0104 (fax)
We have a web page for this book, where we list errata, examples, and any additional
information. Y ou can access this page at http://shop.oreilly.com/product/
0636920032335.do .
To comment or ask technical questions about this book, send email to bookques‐
tions@oreilly.com .
For more information about our books, courses, conferences, and news, see our website
at http://www.oreilly.com .
Find us on Facebook: http://facebook.com/oreilly
Follow us on Twitter: http://twitter.com/oreillymedia
Watch us on Y ouTube: http://www.youtube.com/oreillymedia
Acknowledgments for the First Edition
I’d like to thank Simon St. Laurent, the editor of this book, for his guidance and com‐
ments, which were always right on the mark. He also told me in an email, “we already
know that you know how to write, ” which is one of the nicest things anyone has ever
told me.
Thanks also to Edd Dumbill, who wrote the document that I modified only slightly to
create Appendix A . Of course, any errors in that appendix have been added by my
modifications.
Thanks also go to the technical reviewers of this book: Antoine Quint and David Kla‐
phaak and the SVG Quality Engineering team at Adobe, who did the technical review
of the manuscript. Y our comments have helped improve many aspects of this book.
Jeffrey Zeldman is the person who first put the idea in my head that I, too, could write
a book, and for that I thank him most sincerely.
I also want to thank all the people, foremost among them my brother, Steven, who, when
I told them I was writing a book, believed in me enough to say, “Wow, that’s great. ”
Preface | xvii

Acknowledgments for the Second Edition
We would like to thank Shelly Powers for her excellent technical review. Our thanks also
go to Simon St. Laurent and Meghan Blanchette for their fantastic job of editing and to
Matthew Hacker and the O’Reilly tools and production teams for getting all the finishing
touches just right, despite the best efforts of stubborn software and fussy authors.
From David:  I’d like to give special thanks to Amelia Bellamy-Royds. She was initially
doing technical review of the book, and her comments were so lucid and well written
that I found myself lifting them verbatim and realized that she should be a coauthor.
Her corrections and additions have made the book far better than I could have imagined.
From Amelia:  I’d like to thank David for being decent enough to recognize when I’d
exceeded my original job description and deserved extra credit. His original book was
a wonderfully welcoming introduction to SVG. As someone who had puzzled through
all the quirks of web browser implementations on my own, I really wanted the revised
book to have clear explanations for all the things that confused me when learning SVG
as it currently works in practice.
I also need to send special thanks to my husband, Chris, who has been hugely supportive,
but who has also regularly reminded me when I need to step away from the computer,
eat, sleep, or get some fresh air.
xviii | Preface