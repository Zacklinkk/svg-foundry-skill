Chapter 3. Chapter 3: CSS Animation and Hand-Drawn SVG Sprites
SVG performs extremely well as an icon format, but we’ll move a step further and use SVG sprites in performant complex animations using three different techniques. The first two techniques are closely related to cel animation, while the third, detailed in chapter four, is a technique I recommend for more complex responsive, and even interactive development.
Chapter 3. Keyframe Animation With Steps() and SVG Sprites, Two Ways.
If you’ve ever seen a Looney Tunes or old Disney animation, you might have been impressed with the fluid movement, considering that every frame was hand-drawn. Such effects are possible on the web with SVG sprites, and we can stand on the shoulders of previous animators while employing new development techniques.

Of all web-based animation techniques, step animation most closely resembles these old hand-drawn cel animations. Cel is short for “celluloid” which is a type of transparent sheet. This material was used by animators to draw on top of their previous drawings, thereby defining a sequence and creating the illusion of movement. It functioned a bit like a flip-book. Each drawing was captured on film frame by frame. There were usually several layers to these drawings to save time- you wouldn’t want to redraw the background again and again just to show the same scene.

In order to save steps in drawing, the background would be painted, and then the character, sometimes even pieces of the characters face, like the mouth or eyes, would be adjusted other layers to keep from having to repaint the character’s body.
Cel Animations as Scoping
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

You can think of this technique like writing web page templates: you start from the base template and create smaller pieces so you can manage the individual thing that’s happening in one piece separate from everything else.

Here, we mimic this analog process by using a single motionless background, then quickly showing a series of images on top. This gives the illusion of movement without any real interpolation. Instead of a series of separate image files though, we will simultaneously reduce HTTP requests and simplify our keyframes by using a single SVG sprite sheet. This technique is great for more complex shapes and expressive movement than simple transforms can offer.

Because this technique relies heavily on design, we’ll go through the design workflow first and then go through the code. Here what the final animation looks like:

http://codepen.io/sdras/pen/LEzdea/

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

Typically when showing interpolated (i.e. rapidly changing) images on the web we should push the maximum frames per second possible to achieve the silkiest animation. This technique is one exception that rule. Since we have to draw every single frame, we’re going to try to get as much bang for our hand-drawn buck as possible. 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

Years ago animators spent a lot of time trying to find a good balance between realistic movement and the fewest number of drawings. Old film was shot at 24fps, and animators largely regarded “shooting on twos” (meaning one drawing over two frames, or 12fps) the standard for an illusion of movement. Dropping to anything lower than this, and your eye will discern a slight choppiness (which some animators even used as a creative decision!) We use their work in finding these bounds of illusion to our benefit, stick to the 12fps rule, and create a 21 part drawing for a 1.8s animation. The 21 here comes from the the number of frames that we chose, but can be any number you like.

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

There are two ways of creating the series of drawings for this type of animation; both work equally well, but use different automation processes for the images. The challenge we face in each workflow is keeping the drawing steadily placed in the center of the frame throughout a large sprite: even the best drawing will look flawed if the drawing jumps as we run through each frame.
Chapter 3. The “Drawing in Illustrator with a Template” way
I use Illustrator for this technique but you could theoretically use Sketch or any other graphics editor. 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; line-height: 15.0px; font: 18.0px Arial; color: #232323}
span.s1 {font-kerning: none}

First, we decide how big the animation is and multiply that number by 21 in one direction (the number of frames in our animation), determining the length of our artboard. We drag a box around that area and choose **Object > Path > Split Into Grid**. Then we enter the rows we want (or columns if you wish to make a horizontal sprite sheet) and click OK. Then choose **View > Guides > Make Guides** and our template is all set.

If you’re drawing directly in the graphics editor, I recommend placing your first drawing within the first box, and creating a box around it that frames it within the guides. We can then copy everything into the next box (including the box frame) using the alignment line or shift + drag, which will keep it steady. Use the box frame again to fit it into the next guides space.

Using the direct selection tool (the white arrow) you can then drag and reshape the pieces of your image for each frame. Fair warning: don’t be tempted to front-load your work here by copy-pasting it all at the start- this process works best if you build each frame from the next.

You can also do a screencast of something so that you can walk through the stills and place each image in the Illustrator doc and trace it, either through Illustrator’s native trace functionality, or with the pen tool for a hand-drawn feel, and more concise paths. At the end of this process we will have a long sprite sheet. We can export that directly as an SVG, as well as a PNG, which we’ll use as a fallback with a body class hook in modernizr. At this point, though, fallbacks might not be so necessary for you, so it’s recommended you compare the caniuse.com tables for SVG support and compare to your analytics.

.splash {
  background: url(‘splash-sprite2.svg’);
 ...
  animation: splashit 1.8s steps(21) infinite;
}

/* fallback */
.no-svg .splash {
  background: url(‘splash-sprite2.png’);
}
Chapter 3. The “Drawing in an SVG Editor or on Paper Frame-By-Frame and Using Grunt to Sprite” Way
The first process will still look like a an illustrator drawing, and you might find you like more of a hand-drawn feel. If this is the case, it’s very easy to still draw by hand and scan it in. Old animation studios used lightboxes and celluloid sheets so that they could trace their previous drawing incrementally. You don’t necessarily need these materials to try this technique, though. By placing a lamp underneath a glass table, you can easily make a poor-man’s lightbox. This set-up shines enough light so that you can see through even regular opaque copy paper. To create each new frame, place a piece of paper or vellum over your last drawing and change the drawing slightly until you have a series. You can then scan this set of drawings and vectorize them, placing them correctly with reduced opacity and guides.

If you’d rather draw each piece frame by frame in the editor but don’t know how many frames you will be creating, you can draw each one separately, shifting the image slightly each time, and saving every new version to a folder. Illustrator’s new export settings are good enough that that you can do so without all the old cruft and comments. Be sure to export with **Export as > SVG** rather than save as>SVG, as this will yield better results. You must sure that what you’re initially saving is indeed an SVG and not an AI (or any other) filetype. You can then use Grunticon to compress and sprite them automatically. Here’s a great article explaining how to do so: http://css-tricks.com/inline-svg-grunticon-fallback/. Notably, Grunticon also generates a fallback PNG automatically.

Personally, I think if you draw each frame by hand, it makes the most sense to just make sure the placement on each artboard is consistent and use Grunticon, but the Illustrator template technique has the benefit of allowing you to see all of your work at once, which gives you more of a holistic understanding of what you’re making.
Chapter 3. Simple Code for Complex Movement
This type of sprite makes use of the smallest amount of code for the most amount of believable movement. We intentionally keep the code DRY (don’t repeat yourself), simple, and clean. The greatest thing about this type of movement is that we rely enough on the sprite to not need a lot of code to achieve an illusion of movement through space.

We absolutely position a smaller area of movement because we want to show a consistent experience across desktop and mobile. Our aim is to cycle through the entire image, but stop momentarily at each individual picture in the image, and thankfully, steps() in CSS allows us to do just that. We’ve already done a lot of the heavy lifting in our design, so the code to create the effect is very small.

There’s no need for complex percentages and keyframes. All we need to do is take the image height, and specify the background-position with that number as a negative integer on the 100% keyframe value based on the pixel value of the final frame:

@keyframes splashit {
	100% { background-position: 0 -3046px; }
}

Here, we don’t have to make the SVG fluid to its container, because it’s easy to have it take up the whole screen on mobile. On the splash div, we animate using steps() for the number of frames we had in the SVG:

.splash {
  background: url(‘splash-sprite2.svg’);
 ...
  animation: splashit 1.8s steps(21) infinite;
}

Using an SVG rather than a PNG gives us the advantage of a crisp image on all displays, but providing a fallback is easily done. We use modernizr to create a class hook on body and can still animate it with the PNG we created. We don’t simply use the PNG because on different resolutions, it will look fuzzy while the SVG will remain crisp.

/* fallback */
.no-svg .splash {
  background: url(‘splash-sprite2.png’);
}
Chapter 3. Simple Walk Cycle
If you take the steps() value out of the last animation, you’ll see something interesting. Instead of creating a seamless moving drawing, the background rolls through. We can use that to our advantage for a nice layered background with spatial placement and movement.

Let’s consider this walk cycle, which shows a ghostly figure walking through a looping, multi-dimensional, outlined landscape:

http://codepen.io/sdras/pen/azEBEZ

We use the previous technique with the cels/steps, with drawings that show a walk cycle. We can use a poor man’s animation technique to change the color by shifting the color in each frame. Alternately, we could have used a filter with a shift for hue-rotate:

But as long as we are creating all of these frames by hand, the amount of work to change the color here is minimal, and the cost of the filters on performance, while not huge, is a cost we can do without.

It’s still important that the steps() and animation-duration ratio still fall around the 12fps range. We can scroll through each version of images presented by animating the background position of the SVG sprite sheet. In order to keep everything consistent, we’ve made all of the background images the same size, which lends itself well to the use of an @extend if you’re working with Sass.

/*--extend--*/
.area {
  width: 600px;
  height: 348px;
}

.fore, .mid, .bk, .container { @extend .area; }

To create the impression of fluid linear infinite movement, the 3 background images must be able to repeat seamlessly on the X axis so that when they scroll through. This can be achieved by making each end identical, or in this case, using an image that is sparse enough that it can completely flow through. If you’re working with the latter, it’s important to marry the beginning state and end state in a graphics editor like Illustrator or Sketch to ensure it looks ok while you’re building the graphic.

Each element uses the same keyframe values, but we set apart their animations with an incremental decrease in seconds the further back their z-index. If you look around you, things that are closer to you are in sharper contrast and appear to move faster than things that are further away. Our animation will mimick this effect by increasing the second integer (and thus having longer animations) for the SVGs in the background.ATLAS-CURSOR-HERE This yields a nice parallax effect. There are three parallaxed background images in this pen that don’t include the figure.

.fore {
  background: url(‘fore.svg’);
  animation: bk 7s -5s linear infinite;
}

.mid {
  background: url(‘mid.svg’);
  animation: bk 15s -5s linear infinite;
}

.bk {
  background: url(‘bkwalk2.svg’);
  animation: bk 20s -5s linear infinite;
}

@keyframes bk {
  100% { background-position: 200% 0; }
}

We don’t need multiple intervals for this kind of animation because keyframes will interpolate values for us. In the event that the amount of pixels in the scrolling sprite sheets change in the future, we don’t have to designate the amounts by setting a percentage. The use of staggered negative delays ensures that the animation is running from the start. All of the SVGs are optimized and have a PNG fallback.