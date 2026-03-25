## **

An image is more than just a collection of details, and a complex SVG document is more than just a series of independent elements and styles.  When you are starting a project, it is worthwhile to take some time to plan your approach.

This chapter summarizes some of our best advice for managing SVG on the web.  If you are building an important website component using SVG, a little bit of up-front strategy and analysis can help save time and frustration later.
Planning Your Project
Got a great idea for a design, a game, an unusual interface widget?  Wonderful!  Time to bring it to life!

But before you start writing code—even before you start drawing in Illustrator or Sketch—make a plan.  You aren’t just creating a picture, you’re building a product.  It needs to work for your end users, and it needs to be maintainable and adaptable for future design changes.
Does Your Project Need SVG at All?
It may seem strange to ask this question in a book devoted to SVG, but, as much as we love vector graphics, we also understand that it isn’t applicable to absolutely *everything* in web design and development.

If your diagram can be represented as colored squares on a web page, it’s usually easier to create it using HTML elements with a little CSS, rather than using an inline SVG. You can even make the elements quickly scalable by using % or vw units for width and height.

When SVG first became available in web browsers, it replaced 1px-wide repeating raster images as the best solution for background gradients.  It was also the easiest solution for rounded rectangles and circles.  But CSS gradients and border-radius have rightly replaced both use cases.

Throughout the book, we’ve been comparing SVG and CSS-only solutions so you can make better decisions about which tool to use.  If your all-CSS approach involves multiple nested elements, transforms, and hidden overflow to draw a shape that could be defined with a simple SVG <polygon>, this is a good time for SVG.

But if you can create the effect with well-supported CSS properties on the elements you’ve got, you can skip the SVG.  We won’t be offended.

While SVG’s abilities extend well beyond those of CSS-only graphics, SVG must be considered as part of the *continuum* of web technologies, never as a sole answer.

Similarly, once an SVG drawing is started, there’s a tendency to create *everything* inside it.  But the best solution may sometimes be a hybrid one.  Create a graph in SVG, but add text labels as HTML elements.  Or use multiple separate <svg> elements and then combine them with CSS layout so they can be easily rearranged.  You’ve now got a firm handle on a new tool for your toolbox, but that doesn’t mean it should be the only tool you use.
Identify Your Browser Support Requirements
When building for the web, you are designing for an uncertain environment.  The code you generate will be rendered differently depending on which web browser views it, in which device.

Throughout the book, we’ve warned you about browser quirks and support limitations.  And while those warnings can be discouraging, they should have also clarified that things *are* getting better.  Newer browsers have better support and fewer SVG bugs.

But not everyone who visits your website will be on the newest browser.  You need to decide early on which older browsers you will be trying to support.  There’s little point in developing a site heavily infused wih SVG only to learn later that the client wants it to work in Internet Explorer 6.

If you have analytics data for the website you’re working on, wonderful.  But even if you don’t, you can find general data about browser usage for your country, and adjust it based on your website’s audience.  If you’re only interested in tech-savvy users with plenty of spending cash, you might only code for fairly recent software.  But for many businesses, and especially for public services or health care, you need to consider a much broader audience who might be using much older computers or phones.

Once you’ve identified the browsers and devices you need to support, ask: does your graphic need to look the same in every browser?  Or are some graphical effects optional enhancements, for which fallbacks are acceptable?  If you are willing to accept compromises on older browsers, you can use more of the new features and shorthand options that simplify your code—and reduce file sizes—for the browsers that do support them.
Tip
For supporting really old browsers (Internet Explorer 8 or under, and Android versions under 4), you have to accept that some users won’t have SVG at all.  That means planning for plain-text fallbacks or alternative image formats.

A related decision is the **performance budget** for your site and for the graphics in particular.  How many kilobytes are acceptable?  How much battery power is that animated effect worth?  How many seconds will users wait for this graphic to load?  A good starting point might be “three seconds on 3G”: every page should be ready to read and use in less than three seconds on a third-generation mobile data connection.

There are no hard-and-fast rules for how fast and lean a website has to be, or for how to make it so.  Your standard should be negotiated to balance the needs of the web development team, your client or boss, the expected audience, and other stakeholders.  But by having standards—based on real user data whenever possible—you have benchmarks against which you can test your work, and make adjustments as you go.

SVG often means a smaller file size than alternative graphics formats, but not always.  SVG can also mean more processing on the end device.  Some features, like filters and animation (and especially animated filters), use much more processing power than others.  By testing your works-in-progress against preset performance budgets, you can identify when you need to change strategies.
Decide How SVG Will Integrate in Your Website
SVG on the web doesn’t exist in isolation; it nearly always will be integrated in a larger website.  We’ve discussed, in many chapters, the differences in SVG behavior that relate to *how* the SVG is integrated with HTML.  Inline SVG has different requirements and possibilities than SVG as an image.

For this reason, it helps to plan early on for the embedding method.
To recap, the three options and their key differences are as follows:

SVG as an image

SVG embedded with <img> or as a CSS background image will be limited in its animation and interactivity.  CSS inside the SVG document will be used for styling vector elements in the document itself, but these styles cannot be read or manipulated from the outside. Scripts, interactivity, and alternative text written inside the SVG will be ignored.  There are also currently limitations in browser support for declarative animation.

The primary advantage of SVG-as-image is its *encapsulation*: just like a bitmap image, the SVG is contained in a single file that will behave predictably when it is embedded across multiple pages.

SVG as an embedded object

SVG embedded with <object> or <iframe> also has encapsulation and reusability, but it can have interactivity and scripting.  It is often a good compromise solution, but it isn’t used as much these days, which means it isn’t tested as thoroughly.  New bugs in scaling, sizing, and accessibility sometimes pop up in browsers.

Inline SVG

Inline SVG is also fully interactive and scriptable, but as part of the larger document.  One of the biggest benefits is that SVG elements can be dynamically styled as part of the main document’s stylesheets.  One of the biggest limitations is that those styles can cross over even when you don’t want them to.  Inline SVG also currently has the best browser support for keyboard and screen-reader accessibility.

All other factors being equal, inline SVG will usually load faster than a separate file reference, but at the cost of more complicated maintainability.  For most websites, you will need content management systems and/or build tools that can integrate the SVG markup in your HTML.  If the same SVG code needs to be reused across pages, you’ll usually require server-side includes or similar technologies to share the same source file between them.  The repeated code also needs to be downloaded for each page, slowing down subsequent page loads compared to cached image files.

Sometimes you’ll be able to decide on the embed method that best suits your project, while other times you’ll be restricted by the structure of your existing codebase.
Whichever approach you use, development of the SVG code will be easier if you know up front what the final capabilities will be.
Design for All Users