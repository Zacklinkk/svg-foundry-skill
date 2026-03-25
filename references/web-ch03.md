Preface
Scalable Vector Graphics (SVG to its friends) has many applications.  It is used by graphic designers and by technical drafters.  But this book is specifically about its use in web design and development.

*Using SVG with CSS3 and HTML5* is, essentially, using SVG on the web.  But more than that, it’s about using SVG in complex web applications.  This is SVG not only as illustrations, but as graphical documents that can be integrated in HTML web pages, and styled with custom CSS.  Many chapters will be useful to designers creating images for the web, but the focus is on developers who are adapting designs to add data-based graphics, dynamic styles, interaction, or animation.
A Winding Path
This book traces its origins to 2011, when Kurt started work on a book called *HTML5 Graphics with SVG and CSS3*.  At the time, HTML5 and CSS3 were brand new, and SVG was just starting to get decent support in web browsers.

But life, as it often does, got in the way.  The book took much longer than planned to complete.  And time introduced its own complications.

When Kurt handed off the manuscript to Amelia in late 2014, the state of graphics on the web had changed considerably since when he’d started it.  HTML had acquired competely new graphics formats (the Canvas2D API and WebGL), which were completely separate from SVG and CSS.  And CSS3 was becoming a bigger and bigger topic every year, quickly outgrowing the one chapter planned for it.

So the decision was made to focus on SVG.  However, this book is still all about the intersection of the three web languages—and JavaScript, too!  The driving goal for the rewrite was to create a practical guide to using SVG on the web, in complex web pages, with full awareness of the need for cross-browser, multidevice support.

That turned out to be easier said than done.  It’s taken a few more years (and one more coauthor, Dudley) to complete this manuscript.  It’s also a considerably larger book than initially planned.  We hope it was worth the wait.
The Road Ahead
SVG is a complex topic, but we have tried to arrange this book in a logical progression.  Part I begins with the wide view, discussing how—and why—you use SVG in web design:

- 
The possibilities of SVG as an independent image format, in Chapter 1

- 
SVG on the web, with a focus on how it interacts with other coding languages, in Chapter 2

- 
How CSS can be used to style your SVG, and how SVG graphics can be used with CSS to style other documents, in Chapter 3

- 
Useful software for creating and testing SVG images, as well as some sources of ready-to-use SVG for less artistically inclined web developers, in Chapter 4

The remainder of the book will narrow in on each of the main features of SVG one chapter at a time.  Part II concentrates on the core drawing elements in SVG, and how to control their geometry and layout:

- 
Sizing and positioning basic shapes, in Chapter 5

- 
Defining custom shapes and lines, in Chapter 6

- 
Text layout, in Chapter 7

Part III dives into the technical details of how SVG documents are constructed and how vector shapes are positioned:

- 
Establishing coordinate systems and scale, in Chapter 8

- 
Redefining coordinate systems when embedding graphics in web pages, in Chapter 9

- 
Reusing content and embedding images, in Chapter 10

- 
Transforming coordinate systems to reposition and distort graphics, in Chapter 11

Part IV focuses more on the graphical side of the language:

- 
Filling the area of shapes and text, including gradients and patterns, in Chapter 12

- 
Drawing outlines around shapes and text, in Chapter 13

- 
Adding line markers (repeated symbols on the ends or corners of custom shapes), in Chapter 14

- 
Clipping and masking of graphics, in Chapter 15

- 
Filter effects and blend modes, in Chapter 16

Part V looks at how the basic structure of SVG images can be enhanced to create complete web applications, focusing on three main areas:

- 
Accessibility and metadata, in Chapter 17

- 
Interactive SVG, links, and event handling, in Chapter 18

- 
Animation using CSS, XML, or JavaScript, in Chapter 19

Once you have all the pieces in place, Chapter 20 returns to the big picture, discussing best practices for working with SVG.
Before You Begin
This book focuses on “using SVG” in web pages.  It assumes that you, the reader, are already familiar with creating web pages using HTML, CSS, and a little bit of JavaScript.  When the examples use relatively new features of CSS3 and HTML5, we’ll explain them briefly, but we’ll assume you know a <div> from a <li>, and a font-family from a font-style.

You’ll get the most out of the book by working through the code samples as you go.  It will help if you have a good code editor that recognizes SVG syntax, and if you know how to use the developer tools in your web browser to inspect the document structure and styles that create the visible result.
About This Book
Whether you’re casually flipping through the book, or reading it meticulously cover-to-cover, you can get more from it by understanding the following little extras used to provide additional information.
Conventions Used in This Book
The following typographical conventions are used in this book:

**Bold**

Indicates new terms or concepts.

*Italic*

Indicates URLs, email addresses, filenames, and file extensions, or simply emphasizes that a word is really important.

Constant width

Used for code listings, as well as within paragraphs to refer to code elements such as elements, attributes, keywords, and function names.

*Constant width italic*

Shows a variable or other part of code that should be replaced with user-supplied values or by values determined by context, so that "Hello, *Name*" becomes “Hello, Kurt,” or “Hello, Dudley,” or “Hello, Amelia.”
Tip
Tips like this will be used to highlight particularly tricky aspects of SVG, or simple shortcuts that might not be obvious at first glance.
Note
Notes like this will be used for more general asides and interesting background information.
Warning
Warnings like this will highlight compatibility problems between different web browsers (or other software), or between SVG as an XML file versus SVG in HTML pages.

In addition, sidebars like the following will introduce complementary topics:

Although these sidebars are not absolutely essential for understanding SVG, they will hopefully add important context when you are planning a complete web project.
Supplementary Material
There is so much to say about SVG on the web, we couldn’t fit it all in one book.

We have a couple dozen extra sections, most with extra examples, that go a little deeper into the ideas from the main text.
We’ve also put together reference sections, to make it easier for you to look up syntax details later.

This supplementary material is available online, and also includes the full code for all the examples and figures, and other assets such as JPEG files:

- *https://oreillymedia.github.io/Using_SVG*

Whenever there is extra explanatory or reference material available, a note box like the following will let you know:

The online supplementary material is also available for download as a zip archive or Git repository:

- *https://github.com/oreillymedia/Using_SVG*
About the Examples
The examples in this book have been tested in common web browsers in mid-2017.  Bugs and inconsistencies are noted throughout.  Hopefully, some of those bugs will be fixed in the future; web browsers are updated on a monthly basis, and some improvements have occurred even as this book was being edited.  However, there are likely other problems that we have overlooked.  In addition, other software for manipulating SVG has its own limitations or quirks which are not outlined here. Test early, test often, test in any software your content needs to be displayed with.

The full example code is available for you to experiment with:

- 
On GitHub, you can download the entire supplementary repo, or can find an individual file and view the code.  For SVG files that don’t require JavaScript, the GitHub code view also shows you what the graphic looks like.

- *https://github.com/oreillymedia/Using_SVG*

- 
On the website, you can test out the working versions of each example in your browser, and use your browser’s developer tools (or view-source mode) to see the code.

- *https://oreillymedia.github.io/Using_SVG*

This book is here to help you get your job done. In general, if example code is offered with this book, you may use it in your programs and documentation. You do not need to contact us for permission unless you’re reproducing a significant portion of the code. For example, writing a program that uses several chunks of code from this book does not require permission. Selling or distributing a CD-ROM of examples from O’Reilly books does require permission. Answering a question by citing this book and quoting example code does not require permission. Incorporating a significant amount of example code from this book into your product’s documentation does require permission.

We appreciate, but do not require, attribution. An attribution usually includes the title, author, publisher, and ISBN. For example: “*Using SVG with CSS3 and HTML5* by Amelia Bellamy-Royds, Kurt Cagle, and Dudley Storey (O’Reilly). Copyright 2018 Amelia Bellamy-Royds, Kurt Cagle, Dudley Storey, 978-1-491-92197-5.”

If you feel your use of code examples falls outside fair use or the permission given above, feel free to contact us at *permissions@oreilly.com*.
O’Reilly SafariNote
*Safari* (formerly Safari Books Online) is a membership-based training and reference platform for enterprise, government, educators, and individuals.

Members have access to thousands of books, training videos, Learning Paths, interactive tutorials, and curated playlists from over 250 publishers, including O’Reilly Media, Harvard Business Review, Prentice Hall Professional, Addison-Wesley Professional, Microsoft Press, Sams, Que, Peachpit Press, Adobe, Focal Press, Cisco Press, John Wiley & Sons, Syngress, Morgan Kaufmann, IBM Redbooks, Packt, Adobe Press, FT Press, Apress, Manning, New Riders, McGraw-Hill, Jones & Bartlett, and Course Technology, among others.

For more information, please visit *http://oreilly.com/safari*.
How to Contact Us
Please address comments and questions concerning this book to the publisher:

- O’Reilly Media, Inc.

- 1005 Gravenstein Highway North

- Sebastopol, CA 95472

- 800-998-9938 (in the United States or Canada)

- 707-829-0515 (international or local)

- 707-829-0104 (fax)

We have a web page for this book, where we list errata, examples, and any additional information. You can access this page at *http://bit.ly/usingSVG_with_CSS3_HTML5*.

To comment or ask technical questions about this book, send email to *bookquestions@oreilly.com*.

For more information about our books, courses, conferences, and news, see our website at *http://www.oreilly.com*.

Find us on Facebook: *http://facebook.com/oreilly*

Follow us on Twitter: *http://twitter.com/oreillymedia*

Watch us on YouTube: *http://www.youtube.com/oreillymedia*
Acknowledgments
A book of this size does not get published without contributions from many people.
The authors want to thank the many editors, reviewers, and other production staff who have worked on this book over its many-year gestation.

Special thanks go to former O’Reilly editor Meghan Blanchette, who helped shape the scope and organization of the book; to the final technical reviewers—Ana Tudor, Gabi, and Taylor Hunt—who suggested numerous clarifications for tricky topics (and identified many additional bug warnings); and to the O’Reilly tools and production teams, who crafted numerous custom style and layout features for the print book.

Finally, thanks go to the readers who offered feedback and encouragement based on the early release drafts of the book.  In the hyperconnected, fast-paced world of web development, writing a book is still a lonely, long-term endeavor; it is important to be reminded that the explanations and examples we create are helping real human beings working on the web.

Thank you, thank you very much.