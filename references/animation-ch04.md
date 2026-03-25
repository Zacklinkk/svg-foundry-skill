Chapter 2. Chapter 2: Animating with CSS
You 
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

may find working with SVG code feels very familiar, mostly because an SVG has a DOM, just like standard HTML markup. This is hugely valuable when working with CSS animations, as manipulating markup with CSS is already a very comfortable process for most Front-End Developers.

For a very brief review, let’s first establish that a CSS animation is created by defining two parameters. The keyframes themselves:

@keyframes animation-name-you-pick {
  0% {
   background: blue;
   transform: translateX(0);
 }
  50% {
   background: purple;
   transform: translateX(50px);
 }
  100% {
   background: red;
   transform: translateX(100px);
 }
}
Keyframe Syntax Hint:
You can also define from and to instead of percentages. If you declare nothing in either the initial keyframe or the ending keyframe, the animation will use the default or declared properties on the element. It may be worth double checking your work in all browsers if you do remove them though, due to strange and inconsistent bugs.

After you define the keyframe values, you have two options for animation syntax declaration. Long form, with each declaration defined separately:

.ball {
  animation-name: animation-name-you-pick;
  animation-duration: 2s;
  animation-delay: 2s;
  animation-iteration-count: 3;
  animation-direction: alternate;
  animation-timing-function: ease-in-out;
  animation-fill-mode: forwards;
}

Or shorthand (my preferred method as it uses less code):

.ball {
  animation: animation-name-you-pick 2s 2s 3 alternate ease-in-out forwards;
}

All of the above declarations are interchangeable in order in a space-separated list, except for the number values, which must be defined in the above order: duration, delay, and iteration count.

If we apply it to this very simple .ball div

.ball {
  border-radius: 50%;
  width: 50px;
  height: 50px;
  margin: 20px; //so that it’s not hitting the edge of the page
  background: black;
}

the result is this, with interstitial states shown less opaque:

All of the resulting code in action is available in this demo: http://codepen.io/sdras/pen/c00ac22c90b9232a7c5631a8158b48c1

For more information and further detail into each animation property, such as what animation-fill-mode is, what eases are available in CSS, and which properties are animatable, there is extensive information in Estelle Weyl’s book here: http://shop.oreilly.com/product/0636920041658.do

or you can also consult Dudley Storey’s CSS Animations book: http://www.amazon.ca/gp/product/1430247223
Chapter 2. Animating with SVG
Let’s say instead of drawing the ball with CSS, we had drawn it with SVG. We know how to do that from the last chapter. To get the same black circle as above, we would write:

<svg width="70px" height="70px" viewBox="0 0 70 70">
  <circle fill="black" cx="45" cy="45" r="25"/>
</svg>

We define the radius as half of 50, so 25px. Then we move the center of the circle on both the *x* and *y* axis (cx and cy) to half the radius plus that 20px margin we added in the CSS. We could also use margin on the SVG element to move it, but here I’m illustrating that you can draw coordinates alternatively directly in the SVG itself. If we move the circle over, the viewBox has to be larger to accommodate these coordinates, though, so it is the width plus the margin of space to the edge.

Now, if we place a class on the whole svg called ball, using the same animation declaration, we get this:

What happened here? It still moved across, as we were expecting. But the background is filling in the full background of the SVG, thus the entire viewBox. That’s not really what we want. So, what happens if we move that class and target the circle instead?

You may have guessed why we have this output. There are two reasons:

- 
	The circle is moving inside the viewBox. Remember, if we move an internal SVG attribute, the viewBox will quite literally be a window with which you view these elements. So if we move the circle without the viewBox being large enough to accommodate those coordinates, it will be cut off when it moves out of that area.

- 
	SVG elements look like the HTML DOM, but are slightly different. We don’t use background on SVG attributes, we use fill and stroke. An external stylesheet will also have a hard time overriding what is defined inline within the SVG. So let’s take out the fill definition, and move that into our stylesheet.

The resulting code should be this:

HTML:

<svg width="200px" height="70px" viewBox="0 0 200 70">
 <circle class="ball3" cx="45" cy="45" r="25"/>
</svg>

CSS:

.ball3 {
 animation: second-animation 2s 2s 3 alternate ease-in-out forwards;
}

@keyframes second-animation {
  0% {
   fill: blue;
   transform: translateX(0);
 }
  50% {
   fill: purple;
   transform: translateX(50px);
 }
  100% {
   fill: red;
   transform: translateX(100px);
 }
}

And the result is this, but with an SVG instead of an HTML div:
Chapter 2. Benefits of Drawing with SVG
So, why SVG? Why learn SVG when you could also build something in CSS-styled HTML and animate that way?

First of all, even that small, simple circle was 5 lines less code altogether than the CSS version. SVG was built for drawing, unlike CSS, which was built for presentational formatting. Let’s look at the code for the star from the first chapter of this book:

<polygon fill="white" stroke="black" points="279,5 294,35 328,40 303,62 309,94 279,79 248,94 254,62 230,39 263,35 "/>

It would be incredibly difficult to draw a star in CSS with such a small amount of code, and impossible to be that concise once compiled, if using a preprocessor.

Here’s something I drew in Illustrator:

We could also probably draw this in CSS, but to what end? If you’re working with a designer on a project, having them draw something for you in CSS is not typically an option, and drawings that you want to move can get much more complex than this. In SVG you can also make the whole image scale easily, and therefore, your whole animation can be responsive.

All of the information for the illustration is just** 2KB gzipped**, as well, and can fill up a whole screen. That’s pretty amazing if you consider raster image alternatives.

Applying what we just learned about the circle, we can look at some of these shapes and think about what we can do with them. We can group all of the cow together and make it jump over the moon. We can make the “surprise” expression disappear and appear. We can even make the helmet go up and down so it appears as though he’s looking up. (That is actually what I did in the final animation.)
Chapter 2. Silky-Smooth Animation
It’s tempting to use all of the same properties that you use to affect layout with CSS: margin, top, left, etc. But browsers do not update values for all properties equally. To animate cheaply, your best bet is transforms and opacity. That might seem limiting, but transforms offer translation (positioning), scale, rotation. The combination of these with opacity can be extremely powerful. It’s surprising how much can be achieved with these properties in standard animations. Throughout this book, I will use these properties wherever possible while demonstrating various techniques. It is important to note that SVG DOM elements can’t be hardware accelerated, but you should still be moving the SVG DOM with transforms and not margins or other CSS positioning.

For more information on how to properly keep your layout repaint costs low, check out:

http://jankfree.org/

http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/

and for information on the costs of individual properties, see:

http://csstriggers.com/