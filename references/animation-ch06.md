Chapter 4. Chapter 4: Creating a Responsive SVG Sprite
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

The “scalable” part of SVG is perhaps the most powerful aspect of the graphics format. Using the viewBox attribute and our knowledge of shapes and paths, we can crop the SVG to any size on the fly, knowing that our intentions within the coordinate space will be preserved.

If we remove the width and height attributes from a common SVG, we’ll see something interesting. The SVG expands itself to the full width of the viewport, maintaining the aspect ratio of everything within the DOM.

If we use CSS keyframes or JavaScript to move SVG attributes such as circle or path while scaling this SVG up or down, the increments that they will move will scale as well, along with the graphic. **This means that if you scale a complex SVG using percentage, flexbox, or other techniques, your animation will also scale accordingly.** This means that you don’t have to adjust anything for mobile or other sizes, you can focus on writing it correctly one time.

The completed animation is completely scalable. 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

In the CodePen below, you can randomly resize the animation while it’s running and watch it instantly adjust.

http://codepen.io/sdras/full/jPLgQM/

This becomes very useful for responsive development. The above animation uses a completely fluid approach. We design the whole thing first, and then slowly reveal things. Our initial SVG before we add any animation in looks like so:

We could also design for responsive SVG in two other ways. 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Arial}

In this chapter, we’ll go into a deep dive of a technique that uses SVG sprites, similar to the ones we created in Chapter 3. This is easy to work with in CSS. In chapter 10, we’ll cover a more advanced JavaScript approach as we hide, show, collapse and rearrange our content.
Chapter 4. SVG Sprite and CSS for Responsive Development
Joe Harrison has shown a really nice way of collapsing SVG sprites for less information on mobile, shown below. We’re going to use that to our advantage and create a similarly, incrementally more complex sprite as we shift viewports from mobile to desktop.

As our screen sizes condense and grow, the graphic also follows suit and collapses or reveals visual complexity. It’s helpful to the user to be not be served overly visually complex graphics on small displays where too much information can become noise. Animation can modify with the same considerations as the typography and layout, adjusting to the viewport and clarifying the design.

Here’s what we’ll be making:

http://codepen.io/sdras/full/xbyopy/

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Arial}

We’re going to be working with this responsive illuminated drop-capital letter to show how a standalone illustration can adjust. The design was inspired by the incredibly decorated medieval manuscript, the Book of Kells, to show how a standalone drawing can adjust to different viewport sizes. We start from this design, which we’ll use as our “map”. Other people plan differently, working in-browser or making sketches; choose whatever method makes you most productive.
Chapter 4. Grouping and Drying It Out
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

Now that we know how the final product appears, we can refactor the design to group like sections together, based on what’s most important for the associated viewport width. We can also simplify the design by identifying shapes used in both the first and second versions, keeping just one copy of each shared shape.

All of the elements are assigned semantic ID names such as “mountains”, or “bridge”. The most detailed shapes also get a shared ID that we can progressively show for larger viewports. If the first illustration is kells_1, the group particular to the second illustration is kells_2, and the last is kells_3. 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

In order to make the SVG scalable to shared container values, the last illustration becomes the same size as the first; SVG’s built-in responsiveness will take care of the rest. The top graphic is more complex because it holds information for both the tablet and desktop viewport sizes.

What we end up creating only two areas of the sprite sheet, with both sharing the same width so that we can scale the whole image at once. The top graphic is more complex because it holds information for both the tablet and desktop instances.

Once we have this view, we run it through SVGOMG, using the web based-GUI to check that there’s no distortion, checking the box to keep the names on the IDs on the groups and paths. We can later change the IDs to classes if we wish and clean up some of the cruft from the export. I do this by hand or with find and replace, but there are myriad ways to accomplish this, depending on the programmer.

The optimized SVG is placed inline in the HTML rather than a source URL background image like in the previous techniques. Now we can set areas to hide and show with a mobile-first implementation:

@media screen and ( min-width: 701px ) {
  .kells3, .kells2 {
    display: none;
  }
}

We also adjust the animation parameters slightly depending on the viewport in order to refine the movement for each version:

[class^="mountain"], [class^="grass"] {
  ...
  transform: skew(1.5deg);
}

@media screen and ( min-width: 500px ) {
  [class^="mountain"], [class^="grass"] { 
    transform: skew(2deg);
  }
}

At this point the width and height are removed from the SVG and we can add in preserveAspectRatio="xMidYMid meet” (though that is the default, so it’s not strictly necessary) to make the SVG fluid. With these alterations, it will adjust to the container size instead, which we set based on percentages. Flexbox or any other responsive container would work here too.

.initial { 
  width: 50%;
  float: left;
  margin: 0 7% 0 0;
}
Chapter 4. The Viewbox Trick
There is one catch- even if we assign the bottom layer a class and hide it, there will be an empty gap where the viewbox still accounts for that space. In order to account for that area, we can change the viewbox in the SVG to show only the top portion:

viewBox="0 0 490 474"

and that will work, but only for the two larger versions. The smallest version is now obscured, as the viewbox is providing somewhat of a window into another portion of the SVG spritesheet, so we will need to adjust it. This is akin to changing the background position in CSS to show different areas of a spritesheet. 
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

But due to the fact that we’re adjusting an SVG attribute, we will need JavaScript, as CSS doesn’t yet have that capability. You can help vote or thumbs up on moving this into the CSS spec here:

https://github.com/w3c/fxtf-drafts/issues/7

var shape = document.getElementById("svg");

// media query event handler
if (matchMedia) {
	var mq = window.matchMedia("(min-width: 500px)");
	mq.addListener(WidthChange);
	WidthChange(mq);
}
// media query change
function WidthChange(mq) {
	if (mq.matches) {
    shape.setAttribute("viewBox", "0 0 490 474");
    shape.setAttribute("enable-background", "0 0 490 474");
	}
	else {
    shape.setAttribute("viewBox", "0 490 500 500");
    shape.setAttribute("enable-background", "0 490 500 500");
	}
};

At the time of publishing, Jake Archibald had just proposed moving these type of adjustments into a CSS spec: https://lists.w3.org/Archives/Public/www-style/2016Feb/0328.html. Hopefully it does gain adoption. If so, you will be able to update all of the viewbox changes in media queries and keep presentation implementation in one language.

Now when we scroll the browser window horizontally the viewport will shift to display only the part of the SVG we want to expose. Our code is now primed and ready to animate.
Chapter 4. Responsive Animation
Due to the fact that when we export from a graphics editor we have a unique id for every different element, if we only want to change the ids to classes, we can still target them using a CSS attributeStartsWith selector:

[class^="mountain"], [class^="grass"] {
  animation: slant 9s ease-in-out infinite both;
  transform: skew(2deg);
}

You’ll see here that we begin with a transform set on that element; this keeps the keyframe animation nice and concise. 
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

The animation will assume that the 0% keyframe corresponds to the initial state of the element; to create a very efficient loop, we can define only the changes halfway through the animation sequence.

@keyframes slant {
  50% { transform: skew(-2deg); }
}

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

Some elements, such as the dots and stars, share a common animation, so we can reuse the declaration, adjusting the timing and delay as needed. The delay is negative offset because we want it to appear as though it’s running from the start even though they are staggered. Animation keyframes will use the default positioning set on the element as 0% and 100% keyframes unless they are specified otherwise. We use this to our benefit to write the smallest amount of code possible:

@keyframes blink {
  50% { opacity: 0; }
}

[class^="star"] {
  animation: blink 2s ease-in-out infinite both;
}

[class^="dot"] {
  animation: blink 5s -3s ease-in-out infinite both;
}

We also need to add a viewport meta tag, which gives control over the page’s width and scaling on different devices. The most common one will do: <meta name="viewport" content="width=device-width, initial-scale=1">