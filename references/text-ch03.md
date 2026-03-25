Preface
Scalable Vector Graphics (SVG) consist of a markup language and associated style rules for defining images and diagrams. SVG is an image format, and for most people, text isn’t the first thing they think of when considering the uses of SVG. From the beginning, however, a key feature of SVG has been its ability to encode text in a machine-readable form. Because SVG itself is a text-based markup language, the text characters in an SVG file can be viewed and edited in any text editor.

This book takes a deep dive into the use of text within SVG. It explores the creative possibilities, but also the potential pitfalls. It describes the basics, but also shows how you can use SVG to generate complex layouts. And, unfortunately, it also outlines the many inconsistencies and limitations of web browser implementations of SVG text (as of mid-2015, when this book was written).

This book was born from another project, an introduction to using SVG on the Web. In order to keep that book a manageable length — and keep it suitable for introductory audiences — many details and complexities had to be skimmed over. But those details and complexities add up to the full, wonderful potential of SVG as a graphics format. Once you understand the basics of SVG, you can start thinking about creating more intricate designs and more creative effects.
What We’ll Cover
If you’re reading this, hopefully you are already familiar with the basics of SVG: how to define a graphic as a set of shapes, and how to use that graphic either as a standalone image file or as markup in an HTML page. It will also help if you are familiar with the basics of CSS-styled
 HTML text, as SVG text builds upon the basic CSS style rules.

The book starts with a brief overview of how computers display text content, and then steadily introduces the many ways SVG allows you to control the appearance of text:

- 
Chapter 1
 reviews the basics of laying out text in word processors and websites, and introduces much of the technical terminology used to describe text.

- 
Chapter 2
 introduces the SVG <text>
 element and the basic attributes for positioning simple text labels within a graphic, including the particular issues of sizing text within a scalable coordinate system.

- 
Chapter 3
 briefly covers SVG’s fill
 and stroke
 properties for controlling the visual appearance of text.

- 
Chapter 4
 gets into more complex text layouts, using formatted poetry as examples.

- 
Chapter 5
 explores SVG’s ability to control the position and orientation of individual text characters.

- 
Chapter 6
 introduces the text-anchor
 property and discusses how it affects the alignment of text.

- 
Chapter 7
 considers the particular issues of multidirectional text, including right-to-left horizontal text and vertical text.

- 
Chapter 8
 introduces the properties that control the vertical alignment of horizontal text and the horizontal alignment of vertical text.

- 
Chapter 9
 looks at the <textPath>
 element and how it can be used to create curved or complex text layouts.

- 
Chapter 10
 explores fonts and their impact on SVG text, including the use of the @font-face
 rule to incorporate web fonts in your SVG graphics online.

- 
Chapter 11
 looks at the textLength
 attribute and font-size-adjust
 property, which can be used to standardize text layout when you don’t have full control over the fonts used; the chapter also describes decorative uses for textLength
 .

- Finally, Chapter 12
 introduces the SVG <foreignObject>
 element, and shows how it can be used to include CSS-formatted HTML text within an SVG image.

To complete the book, Appendix A
 provides a quick reference of the SVG text elements and their attributes, while Appendix B
 summarizes the related style properties.
About This Book
Whether you’re casually flipping through the book, or reading it meticulously from cover to cover, you can get more from it by understanding
 the following little extras used to provide additional information.
Conventions Used in This Book
The following typographical conventions are used in this book:

*Italic*

Indicates new terms, URLs, email addresses, filenames, and file extensions.

Constant width

Used for program listings, as well as within paragraphs to refer to program elements such as variable or function names, databases, data types, environment variables, statements, and keywords.

**
Constant width bold
**

Shows commands or other text that should be typed literally by the user.

*
Constant width italic
*

Shows text that should be replaced with user-supplied values or by values determined by context.
Tip
Tips like this will be used to highlight particularly tricky aspects of SVG, or simple shortcuts that might not be obvious at first glance.
Note
Notes like this will be used for more general asides and interesting background information.
Warning
Warnings like this will highlight compatibility problems between different web browsers (or other software), or between SVG as an XML file versus SVG in HTML pages.

In addition, sidebars like the following will introduce supplemental information:

Although these sidebars are not absolutely essential for understanding SVG colors, patterns, and gradients, they will hopefully add important context when planning a complete web project.
Using Code Examples
Supplemental material (code examples and figures) is available for download as a zip archive or Git repository:

*https://github.com/oreillymedia/SVG_Text_Layout*

Alternatively, you can view the examples online in your web browser:

*http://oreillymedia.github.io/SVG_Text_Layout*

The examples in this book have been tested in common web browsers in mid-2015. Bugs and inconsistencies are noted throughout. Hopefully, some of those bugs will be fixed in the future; web browsers are updated on a monthly basis, and some improvements have occured even as this book was being edited. However, there are likely other problems that we have overlooked. In addition, other software for manipulating SVG have their own limitations or quirks which are not outlined here. Test early, test often, test in any software your content needs to be displayed with.

This book is here to help you get your job done. In general, if example code is offered with this book, you may use it in your programs and documentation. You do not need to contact us for permission unless you’re reproducing a significant portion of the code. For example, writing a program that uses several chunks of code from this book does not require permission. Selling or distributing a CD-ROM of examples from O’Reilly books does require permission. Answering a question by citing this book and quoting example code does not require permission. Incorporating a significant amount of example code from this book into your product’s documentation does require permission.

We appreciate, but do not require, attribution. An attribution usually includes the title, author, publisher, and ISBN. For example: “*SVG Text Layout*
 by Amelia Bellamy-Royds and Kurt Cagle (O’Reilly). Copyright 2016 Amelia Bellamy-Royds, Kurt Cagle, 978-1-4919-3382-4.”

If you feel your use of code examples falls outside fair use or the permission
 given above, feel free to contact us at 
*permissions@oreilly.com*

 .
Safari® Books OnlineNote
*Safari Books Online*

 is an on-demand digital library that delivers expert content
 in both book and video form from the world’s leading authors in technology and business.

Technology professionals, software developers, web designers, and business and creative professionals use Safari Books Online as their primary resource for research, problem solving, learning, and certification training.

Safari Books Online offers a range of plans and pricing
 for enterprise
 , government
 , education
 , and individuals.

Members have access to thousands of books, training videos, and prepublication manuscripts in one fully searchable database from publishers like O’Reilly Media, Prentice Hall Professional, Addison-Wesley Professional, Microsoft Press, Sams, Que, Peachpit Press, Focal Press, Cisco Press, John Wiley & Sons, Syngress, Morgan Kaufmann, IBM Redbooks, Packt, Adobe Press, FT Press, Apress, Manning, New Riders, McGraw-Hill, Jones & Bartlett, Course Technology, and hundreds more
 . For more information about Safari Books Online, please visit us online
 .
How to Contact Us
Please address comments and questions concerning this book to the publisher:

- O’Reilly Media, Inc.

- 1005 Gravenstein Highway North

- Sebastopol, CA 95472

- 800-998-9938 (in the United States or Canada)

- 707-829-0515 (international or local)

- 707-829-0104 (fax)

We have a web page for this book, where we list errata, examples, and any additional information. You can access this page at 
*http://bit.ly/svg-text-layout*

 .

To comment or ask technical questions about this book, send email to 
*bookquestions@oreilly.com*

 .

For more information about our books, courses, conferences, and news, see our website at 
*http://www.oreilly.com*

 .

Find us on Facebook: 
*http://facebook.com/oreilly*

Follow us on Twitter: 
*http://twitter.com/oreillymedia*

Watch us on YouTube: 
*http://www.youtube.com/oreillymedia*
Acknowledgments
Parts of this book can trace their lineage through various manuscripts adapted and revised over multiple years. The list of people to thank for getting it finally to publication is therefore likewise long. Meg Foley is the editor of record, and deserves great appreciation for her unwavering cheerfulness in the face of stretched deadlines. Before her, Meghan Blanchette and Simon St.Laurent persevered through the unenviable task of herding authors of a technical manuscript in an ever-updating field.

The final book owes much to the team of technical reviewers — David Eisenberg, Robert Longson, Dudley Storey, and Tavmjong Bah — who identified errors and inconsistencies, suggested new figures and examples, and pointed out changing SVG implementations worth noting in the tips and warnings. As always, any lingering mistakes are entirely the responsibility of the imperfect authors. Taking care of a different type of technical issue, production editor Colleen Lobner and the rest of O’Reilly’s production team helped finesse the book into a professional final form. Among them, special appreciation goes to copyeditor Jasmine Kwityn for her uncanny ability to detect inconsistencies in style and terminology, even from one chapter to another.

Finally, sincere thanks go out to all the developers working with SVG, creating software to implement it, or extending and improving the SVG specifications. Many of the tips and warnings collected in this book were derived from the experiences of others, shared through blogs, mailing lists, and more.