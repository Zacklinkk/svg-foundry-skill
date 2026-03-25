Chapter 1. Chapter 1: The Anatomy of an SVG
Scalable Vector Graphics are becoming increasingly popular as a means of serving images on the web. The format’s advantages can be drawn from its name:

- SVG images are *Scalable*, which in an age of increasingly varied viewport sizes, is a huge boon to development. Even with srcset and the picture element, we have to cut different sizes. SVG avoids any need for image replacement, along with it’s subsequent potential HTTP requests.

- *Vector* (rather than raster) meaning that because they are drawn with math, SVG files tend to have greater performance and smaller file size.

SVG is an XML file format, which means that one is able to describe shapes, lines, and text, while still offering a navigable DOM, which also means it can be performant *and* accessible.

In this first chapter, we’ll lay the foundation of understanding of what this DOM is comprised of, because we’ll be reaching within it in order to create complex animations. We’ll be going over some of the syntax within the SVG DOM so that you know exactly what you’re manipulating and can debug as needed. We won’t be doing a deep dive into everything that the SVG DOM has to offer because it’s out of the scope of this book. If you’d like more backstory, SVG Essentials and SVG Colors, Patterns, and Gradients are great resources.
Chapter 1. SVG DOM Syntax
Consider this SVG graphic, with its resulting code:

<svg x="0px" y="0px" width="450px" height="100px" viewBox="0 0 450 100">
 <rect x="10" y="5" fill="white" stroke="black" width="90" height="90"/>
 <circle fill="white" stroke="black" cx="170" cy="50" r="45"/>
 <polygon fill="white" stroke="black" points="279,5 294,35 328,40 303,62 309,94 279,79 248,94 254,62 230,39 263,35 "/>
 <line fill="none" stroke="black" x1="410" y1="95" x2="440" y2="6"/>
 <line fill="none" stroke="black" x1="360" y1="6" x2="360" y2="95"/>
</svg>

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 13.0px Arial; color: #232323}
span.s1 {font-kerning: none}

Looking at the SVG structure, most of the markup may appear familiar to you. The syntax is easy to read because of the commonalities shared with HTML. 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 13.0px Arial; color: #232323}
span.s1 {font-kerning: none}

In the root <svg> element, we see a declaration of x and y values- both set to zero here, for the points in the coordinate matrix that we’re starting at. The width and height are both designated, and you’ll see that they correspond to the last two values in the viewBox.
Chapter 1. ViewBox and PreserveAspectRatio
The SVG viewBox is a very powerful attribute, as it allows the SVG canvas to be truly be infinite,
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 13.0px Arial; color: #232323}
span.s1 {font-kerning: none}

while controlling and refiining the viewable space. The four values here are as follows: x, y, width, and height. This space does not refer to pixels, but rather a more malleable space that can be adjusted to many different scales. Think of this as mapping out shapes and drawings on a piece of graph paper:

We can define coordinates based on this system, and the system itself can be self contained. We can then alter the size of this sheet of paper, and everything within it. If we designate half the width and height for the SVG, but retain the same viewbox, the result would be this:

This is part of the reason why SVG is such a powerful tool for responsive development- it can adjust to multiple viewports very easily.

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 13.0px Arial; color: #232323}
span.s1 {font-kerning: none}

SVG also stores information outside the viewBox area. If we move a shape outside this space, we’ll see this:

The white area is what the viewer sees, while the white and grey area together hold the information that the SVG actually *contains*. This feature allows the SVG to be both scalable and easy to crop on the fly. This comes in very handy in responsive applications, particularly sprites.

There is one more tool you should be aware of with the viewBox and it is invisible in this example. Most SVGs you will see on the web won’t even specify it because the default is what most people will want more than nine times out of ten. It is preserveAspectRatio=”xMidYMidmeet”. This forces the drawing area to adjust itself with uniform scaling.

There are several other options as well. The first parameter, xMidYMid, determines whether or not to uniformly scale the element, and which part of the viewport to scale from, in camelCased naming. The default is to scale from the center, or Mid, but there are also Min or Max parameters as well, such as xMinYMax, and so on. You may also designate none, in which case the aspect ratio at its default percentages will be ignored, and it will be squashed and stretched to fill the available space.

The second value is defined by meet or slice. meet will attempt to scale the graphic as much as possible to fit inside containing viewBox, while keeping the aspect ratio consistent. This functionality is similar to background-size: contain; in that the image will stay contained in the boundaries of the containing unit.

slice will allow the graphic within the viewBox to expand beyond what the user sees in the direction specified, while filling up the available area. You can think about it like background-size: cover; in that the image will push beyond the boundaries of the containing unit to fill up the available user space.
Further Resources
Sara Soueidan has an extremely intuitive and helpful interactive demo for you to play with in order to see this system in action: http://sarasoueidan.com/demos/interactive-svg-coordinate-system/index.html.

Amelia Bellamy-Royds has a great resource on CSS-Tricks with tons of cool demos: https://css-tricks.com/scale-svg/

Joni Trythall has a really nice resource about the viewBox and viewport as well: http://jonibologna.com/svg-viewbox-and-viewport/
Chapter 1. Drawing Shapes
Within the SVG, we’ve defined five shapes. rect refers to rectangle or square. the x and y values, just as with the SVG itself, are where the shape begins
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

: in this case, it’s upper left corner. 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

The shape’s width and height use the same coordinate system.

<rect x="10" y="5" fill="white" stroke="black" width="90" height="90"/>

The fill and the stroke are designated here as black and white, and if nothing was specified here, the fill would default to black and the stroke would be 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

none (i.e. invisible.

<circle fill="white" stroke="black" cx="170" cy="50" r="45"/>

Circle refers to, you guessed it, a circle- cx is the point where the center of the circle lies on the x axis, cy is the point where the center of the circle lies on the y axis, and r is the radius. You can also use an ellipse for oval shapes, the only difference being there are two radius values- rx and ry.

Polygon passes an array of values in a space-separated list, defined by points:

<polygon fill="white" stroke="black" points="279,5 294,35 328,40 303,62 309,94 279,79 248,94 254,62 230,39 263,35"/>

As you might assume, the first value refers to the x coordinate position, comma-separated from its matching y value to plot the points of this shape. 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

The sides do not need to be equal in length.

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

Lines are fairly straightforward:

<line fill="none" stroke="black" x1="410" y1="95" x2="440" y2="6"/>

<line fill="none" stroke="black" x1="360" y1="6" x2="360" y2="95"/>

The first point is plotted at the x1 and y1 values, and end at the x2 and y2 values. I’ve shown two here so that you can see the syntax stay consistent whether or not the line is straight or on a diagonal. One difference is that if we
Chapter 1. Responsive SVG, Grouping and Drawing Paths
Now, let’s consider the following graphic and it’s resulting code:

<svg viewBox="0 0 218.8 87.1">
 <g fill="none" stroke="#000">
   <path d="M7.3 75L25.9 6.8s58.4-6.4 33.5 13-41.1 32.8-11.2 30.8h15.9v5.5s42.6 18.8 0 20.6" />
   <path d="M133.1 58.2s12.7-69.2 24.4-47.5c0 0 4.1 8.6 9.5.9 0 0 5-10 10.4.9 0 0 12.2 32.6 13.6 43 0 0 39.8 5.4 15.8 15.4-13.2 5.5-53.8 13.1-77.4 5.9.1 0-51.9-15.4 3.7-18.6z" />
 </g>
</svg> 

The first thing we can notice about this SVG is that we’ve removed the width and height definitions. You can declare it elsewhere (usually in the CSS, though other methods work as well), which makes it very maleable, especially for responsive.
Width and Height Overrides
It’s nice and easy to have CSS control all of the sizing and keep it in one place, but I also sometimes leave the width and height in for times I’m worried about the CSS not loading properly. If there’s no fallback to the width and height inline, the SVG will scale to the available space, which can look pretty ostentatious. For that reason, you may consider writing it inline as well. The CSS will override the presentational attributes (but not inline codes).

It can now scale in percentage, viewport units, and even be affected by media queries. The one catch is that you must declare a viewBox in this instance, it is no longer optional. The default behavior of an svg with width and height removed and viewBox declared is to scale to the maximum parameters of the 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

containing element, which may be the <body>, a <div>, or most anything else.

The second thing I’d like to point out is the g element. g stands for Group, and it’s a way to nest and assemble multiple elements together in the SVG DOM. You may also notice that rather than defining the fill and stroke on elements themselves, we’ve done so on the group, and you can see it apply across the descendants.

The last and very pertinent thing to note is the path and path syntax. The path begins with d for data and is always designated with the M or m for moveTo command as the first value. This establishes a new point. Unlike the polygon/polyline command, the coordinates specified here are not always points on the final line.

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

The table below shows what each letter in a path means. Letters may be capital or lowercase. Capital letters specify an absolute coordinate, while lowercase establishes a relative coordinate.

Path Letter
			Path Meaning
			Image, where applicable
		
	
	
		
			M, m
			moveTo
			start of the path, start of a new path
		
		
			L, l
			lineTo
			
		
		
			H, h
			horizontal line drawn from current position
			
		
		
			V, v
			vertical line drawn from current position
			
		
		
			Curve Commands
			 
			 
		
		
			C,c
			cubic-bezier
			
		
		
			S, s
			reflecting cubic-bezier
			
		
		
			Q, q
			quadratic bezier- where both sides share the same control point
			
		
		
			T, t
			command control point that’s been reflected
			
		
		
			A,a
			elliptical arc
			
		
		
			Ending Path
			 
			 
		
		
			Z, z
			joins the end of a path to the most recent moveTo command
			end of the path
		
	

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

Revisiting the graphic and code above, you can see the difference between the paths by noting which one has a z at the end of its path data.

Delving further into path data is out of the scope of this book, but for a great interactive demo on how path and path syntax works, check out: http://codepen.io/netsi1964/pen/pJzWoz
Chapter 1. SVG on Export, Recommendations, and Optimization
You can absolutely create an SVG by hand, or create the SVG drawing with JavaScript with tools like d3.js. However, there are times when you may want to design and build the SVG in a graphics editor such as Adobe Illustrator, Sketch, or Inkscape. Layers inside the graphical interface will be exported as groups, complete with ids assigned as you have named your layers. You may find, though, that upon export, your SVG has a lot of information that the code in the examples above does not:

<?xml version="1.0" encoding="utf-8"?>
<!-- Generator: Adobe Illustrator 18.1.1, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
<svg version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
     width="218.8px" height="87.1px" viewBox="0 0 218.8 87.1" enable-background="new 0 0 218.8 87.1" xml:space="preserve">
  <g>
    <path fill="#FFFFFF" stroke="#000000" stroke-miterlimit="10" d="M133.1,58.2c0,0,12.7-69.2,24.4-47.5c0,0,4.1,8.6,9.5,0.9
        c0,0,5-10,10.4,0.9c0,0,12.2,32.6,13.6,43c0,0,39.8,5.4,15.8,15.4c-13.2,5.5-53.8,13.1-77.4,5.9C129.5,76.8,77.5,61.4,133.1,58.2z"
        />
    <path fill="#FFFFFF" stroke="#000000" stroke-miterlimit="10" d="M6.7,61.4c0,0-3.3-55.2,20.8-54.8s-7.2,18.1,4.1,29.9
        s8.6-31.2,32.1-15.8S86.7,41,77.2,61.8C70.4,76.8,76.8,79,37.9,79c-0.4,0-0.9,0.1-1.3,0.1C9,81,40.1,58.7,40.1,58.7"/>
  </g>
</svg>

Here’s the earlier code again for comparison:

<svg viewBox="0 0 218.8 87.1">
 <g fill="none" stroke="#000">
   <path d="M7.3 75L25.9 6.8s58.4-6.4 33.5 13-41.1 32.8-11.2 30.8h15.9v5.5s42.6 18.8 0 20.6" />
   <path d="M133.1 58.2s12.7-69.2 24.4-47.5c0 0 4.1 8.6 9.5.9 0 0 5-10 10.4.9 0 0 12.2 32.6 13.6 43 0 0 39.8 5.4 15.8 15.4-13.2 5.5-53.8 13.1-77.4 5.9.1 0-51.9-15.4 3.7-18.6z" />
 </g>
</svg> 

p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

You can see it’s much smaller: without proper optimization, you can easily bloat SVG code.
Illustrator Tip
Be sure to use Illustrator’s “Export as” setting to save off an SVG rather than using “Save As”. This is only available in Illustrator CC 2015.2 or later.

Save as has older presets and is not fully optimized the way that the export setting has been adjusted to work. You’ll end up with a much smaller and more precise output this way. I personally always retain a copy or several copies of the .ai source, because sometimes heavily modified SVGs don’t backport well into Illustrator.

Some of this information is useful, and some we can do away with. The comment about Illustrator generating the code can certainly be removed. We also do not need the version, or layer information, as the web will not use it and we’re trying to transmit as few bytes as possible.

If the x and y are defined as zero (usually the case), we can strip that out, too. The only case where we’d want to leave them in is if we’re working with a child SVG nested inside of another SVG.

We can also strip away the xml definitions if we are using an SVG inline. I will recommend using inline SVG for animations throughout this book because the support for animation is stronger and it has less gotchas. However, there are times when using an SVG as a background image works well for animation as well, particular in chapter 3, when we talk about sprites. If you decide to use the SVG in an object or image, you should keep this xml markup, and sometimes leaving it out can cause issues in Microsoft browsers:

xmlns="http://www.w3.org/2000/svg"

If you’re not sure whether to use it or not, it’s better to err on the side of leaving it in.

You can also optimize paths. There are times that a graphics editor like Illustrator will export path data in decimal places that are extraneous and can be removed. It may also export with additional and unnecessary groups that clutter your code. These are only a few examples of possibilities for compression.
Chapter 1. Reduce Path Points
If you’re going to create a hand drawing, you can trace, it, but past that point you should use **Object > Path > Simplify**.

You will need to check the box that allows for preview because this can potentially ruin the image. It’s also worth it to say that the image degrades quickly, so usually the most I can get away with is 91% or so. This still gives me a good return, with a high number of path point reduction.

This is also probably the quickest way to accomplish this type of reduction. A more labor intensive way, that I will use for smaller pieces that are unnecessarily complex, is to redraw it with the pen tool.

Sometimes this is very little effort for a large payoff, but it really depends on the shape. You can also put a few shapes together, merge them with the path tool, and then modify the points with the white arrow, to simulate existing shapes.

It may seem intimidating at first, but you can use the pen tool to really quickly make more complex areas. Then take all of these shapes and use the pathfinder tool to merge them all together. If it doesn’t look quite right, don’t fear! You can still reduce the opacity on what you made by a little (helps so that you can see what you’re trying to emulate in the shape underneath). Then you can grab the direct selection tool, (A in quickkeys, the white arrow on the toolbar), and drag those small points around until you get a more refined shape. Never hurts to zoom in a bit to see the details there.
Chapter 1. Optimization Tools
You don’t need to strip this information out by hand, though. There are many great tools for optimizing SVG available, and they offer far more to help trim your code as well, such as rounding and rewriting numbers, merging path data, removing unneeded groups, and more. The list below are some of the open-source tools available, please consider that the options with a visual GUI of the output tend to be the most helpful as you can see what the optimization materializes in:

- 
	SVGOMG- https://jakearchibald.github.io/svgomg/- Jake Archibald created a really nice web-based GUI for the terminal-based SVGO discussed later. This is the most robust and easy to work with method, with many different methods for optimizations featured with toggles. Shows the relative visual output and the byte saving comparison with the optimization and gzip.

- 
	Peter Collingridge’s SVG Editor- http://petercollingridge.appspot.com/svg-editor- very similar to SVGOMG, with slightly less options. A nice feature is that you can edit the SVG right in another panel in case you need to adjust the output just slightly. Also web-based with a nice visual interface.

- 
	SVGO- https://github.com/svg/svgo- terminal-based, with no visual GUI- however you can add one with the SVGO-GUI- https://github.com/svg/svgo-gui. This requires a bit more setup but is a workflow boon if you’re more comfortable working in your terminal than popping in and out of the browser. The functionality is what SVGOMG is powered by as well.

Please be aware that you will need to be changing and adjusting settings depending on what you’re trying to achieve in your animation. Get comfortable with adjusting these options rather than settling for the defaults, 
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

as doing so will save you considerable time later. You may find that a very busy animation requires repeated optimizations while you’re developing; for this reason, I recommend leaving the graphics editor and optimization tool open while working with your code editor, making your workflow as seamless as possible.
Default export settings to be aware of
Be mindful of some of the defaults when you’re exporting. The ones that I find myself checking and unchecking the most are:

- **Clean IDs**- this will remove any carefully named layers you may have.

- **Collapse useless groups**- you might have grouped them to animate them all together, or just to keep things organized.

- **Merge paths**- nine times out of ten this one is ok, but sometimes merging a lot of paths keeps you from being about to move elements in the DOM around independently.

- **Prettify**- This is only necessary when you need to working within the SVG, for animation or other manipulation purposes.