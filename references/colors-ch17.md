Chapter 14. Motion Pictures
SVG colors, gradients, and patterns may be collectively referred to as paint, but there is one important difference from oils or watercolors: SVG paint can move.

SVG within a web browser is dynamic.  It can be animated, either on a continuous loop or in response to user interactions.

Animation complicates the discussion of SVG painting properties and paint servers in a few ways.  This chapter sums up the major issues and approaches to address them.  It starts with a review of the different ways in which SVG can be animated, using animated fill colors as an example.  It then addresses the animation of paint servers under two situations: synchronized animations of many painted elements, or animation of a single element without affecting others.

Screenshots have been included for the animation examples, but a full appreciation will require running the code in a web browser.

A complete discussion of SVG animation is worth several books on its own, so this chapter does not attempt to exhaustively describe all the options and syntax.  Instead, it focuses on the aspects that are unique to working with colors and paint servers.  If you’re not already comfortable with CSS, SMIL, and JavaScript animations, you may need to consult other references to fully understand how the code creates the final effect.
Animation Options
There are three different tools you can use to animate SVG graphics:

- 
Include animation elements (<animate>, <set>, <animateTransform>, and <animateMotion>) within the markup to modify other elements.

- 
Add CSS animation or transition properties to your graphics’ styles.

- 
Use JavaScript to sequentially manipulate the styles or attributes of your graphics.

All three methods offer at least a limited potential for interactive animations.  Animation elements can be triggered to start or end on user events such as click or mouseover.  CSS animations and transitions can be triggered by interactive pseudoclasses (:hover, :focus, :active).  And JavaScript can of course be used for all sorts of user interactions and programming.

Any of these strategies can manipulate simple color values for fill and stroke, or most other presentation attributes including fill-opacity or stroke-opacity.

Example 14-1 showcases the basic syntax of animation elements, used to cycle through different fill colors in an image of three stars.  The color cycle lasts 3 seconds, but repeats indefinitely.  The same sequence of colors is used for each star, but offset to start at different times.  Figure 14-1 shows a single instant in the cycle.
Example 14-1. Animating fill colors with animation elements
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="2.15in" viewBox="0 0 200 100">
    <title>Simple SMIL Animation</title>
    <symbol id="star" viewBox="0 0 200 200">
        <path d="M100,10 L150,140 20,50 180,50 50,140 Z" />
    </symbol>
    <rect height="100%" width="100%" fill="#222"/>
    <g fill="gold">
        <use xlink:href="#star" width="50" height="50"
             transform="translate(10,20) rotate(-10)">
            <animate attributeName="fill"
                     values="gold;lightYellow;gold;tomato;gold"
                     dur="3s" repeatDur="indefinite" />
        </use>
        <use xlink:href="#star" width="40" height="40"
             transform="translate(140,10) rotate(20)">
            <animate attributeName="fill" begin="-1s"
                     values="gold;lightYellow;gold;tomato;gold"
                     dur="3s" repeatDur="indefinite" />
        </use>
        <use xlink:href="#star" width="35" height="35"
             transform="translate(80,60) rotate(-5)">
            <animate attributeName="fill" begin="-2s"
                     values="gold;lightYellow;gold;tomato;gold"
                     dur="3s" repeatDur="indefinite" />
        </use>
    </g>
</svg>

The colors transition according to their RGB values.  The intermediary values are calculated using the same rules as for gradients.  The only difference is that the transition happens in time, instead of across space.
Tip
The color animation should be affected by the color-interpolation property.  However, as mentioned in Chapter 3, this property is not well supported in web browsers.

The SVG animation elements use a syntax developed for the Synchronized Multimedia Integration Language (SMIL), which was intended for coordinating audio, video, and XHTML content.  As you can imagine, the syntax includes a number of options for synchronizing one animation with another, either simultaneously or one after the other.  It can therefore be used to script complicated sequences of animation.  However, these complicated animation sequences do not combine well with the interactive features.  The declarative format of SMIL animations does not have any way to include decision logic that would alter the response to a user event based on the current state of the animation.

Another limitation of the animation element syntax is its verbosity.  Each element being animated requires its own animation element.  There is no way to quickly apply variations on the same animation to many graphical components, such as the three stars in Example 14-1.

Nonetheless, the most significant limitation to widespread use of SVG animation elements is poor browser support.
Warning
Internet Explorer has no support for animation elements.  The Chromium team has, at the time of writing, announced plans to deprecate the feature.  In the short term, that means printing warnings to the developer’s console for web pages that use the animation syntax.  In the long term, the animations may cease to work.

For simple animations of presentation styles, CSS animations provide a more concise and flexible syntax.  Example 14-2 creates the exact same twinkling star effect, but with CSS animation keyframes.  A class is used to apply the same animation sequence to all three stars, and then nth-of-type selectors are used to alter the start time on individual shapes.
Example 14-2. Animating fill colors with CSS keyframes
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="2.15in" viewBox="0 0 200 100">
    <title>Simple CSS Animation</title>
    <style type="text/css">
        .star {
            fill: gold;
            animation: twinkle 3s infinite;
        }
        .star:nth-of-type(3n+2){
            animation-delay: -1s;
        }
        .star:nth-of-type(3n+3){
            animation-delay: -2s;
        }
        @keyframes twinkle {
            25%  {fill: lightYellow;}
            50%  {fill: gold; }
            75%  {fill: tomato;}
        }
    </style>
    <symbol id="star" viewBox="0 0 200 200">
        <path d="M100,10 L150,140 20,50 180,50 50,140 Z" />
    </symbol>
    <rect height="100%" width="100%" fill="#222"/>
    <use xlink:href="#star" width="50" height="50" class="star"
         transform="translate(10,20) rotate(-10)"/>
    <use xlink:href="#star" width="40" height="40" class="star"
         transform="translate(140,10) rotate(20)"/>
    <use xlink:href="#star" width="35" height="35" class="star"
         transform="translate(80,60) rotate(-5)"/>
</svg>

The main limitation of CSS animations of SVG are that only presentation attributes can be manipulated.  Geometric attributes (that are only specificied in the XML) cannot be animated with CSS rules.  The SVG 2 specifications are re-defining a number of layout attributes as presentation attributes, but many features of SVG will remain inacessible to CSS animation.

CSS animations are also not well suited to long sequences of consecutive animations, and can only respond to a limited set of user actions.
Warning
CSS animations are not supported in some older browsers still in use.  In all but the latest Webkit browsers, they require the -webkit- experimental prefix, which means you’ll need to duplicate your animation properties and your keyframe rules.

For more universal support of animation, you can manipulate your DOM with JavaScript.  For simple linear animations, it is fairly straightforward to cycle through all the intermediary values, using the requestAnimationFrame method to only update the graphic when the browser is able to update the display.  For more complex, multi-stage animations (such as the staggered color changes), you may want to use a dedicated JavaScript animation library, which converts your declarative statements of which attributes to animate into optimized frame-by-frame adjustments.
Tip
JavaScript can also be used to trigger, pause, or restart declarative animations, by using DOM methods on animation elements or by changing CSS classes.

JavaScript animations cannot be used in SVG embedded in a web page as an image (the HTML <img> element or a CSS background image), because scripts are not run for security reasons.  In contrast, non-interactive declarative animations work as intended in images.
Coordinated Animation
Animating color values for fill and stroke is straightforward.  With either animation elements or CSS animations, the browser transitions in between the colors for you.  But what about paint servers?  That gets more complicated.  The actual value used in the fill and stroke properties is a URL.  The browser cannot generate intermediary values in between two URLs.
Tip
The specifications do allow URL values to be modified by animations, in discrete steps.  However, browser support is poor, restricted to SMIL animations in Firefox at the time of writing.

What *can* be animated, however, are the paint server elements themselves.  Gradient stop colors and opacity values can be animated with CSS, as can presentation styles on the contents of a pattern.  In future, gradient and pattern transformations will also be affected by CSS animations of the transform property.  These and many more structural attributes can be animated with SVG markup or with JavaScript.

Animating a paint server has an important difference from animating a fill or stroke paint property.  The animation automatically affects *all* elements using that paint server, changing them all simultaneously.

Whether this feature is desired or not really depends on the particulars of your graphic.   “Animated Interactions” will explore alternative approaches for when you *don’t* want every element to change simultaneously.  In particular, that section will focus on using animation to highlight a particular element the user is interacting with.  In contrast, for many decorative animations it is acceptable or even desirable to have synchronized animations.

Example 14-3 uses synchronized animations in an adaptation of the twinkling stars animation.  Now, instead of animating the fill colors on the stars themselves, the animation is applied to the stop-color values in a series of <stop> elements.  Otherwise, the animation code looks quite similar.  Figure 14-2 shows a screenshot of the gradient-filled stars, but you’ll need to run the code to get the full twinkling effect.
Example 14-3. Animating gradient stop colors with CSS
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="2.15in" viewBox="0 0 200 100">
    <title>CSS-Animated Gradient</title>
    <style type="text/css">
        .star {
            fill: url(#shine);
        }
        #shine stop {
            stop-color: gold;
            animation: twinkle 3s infinite;
        }
        #shine stop:nth-of-type(3n+2){
            animation-delay: -1s;
        }
        #shine stop:nth-of-type(3n+3){
            animation-delay: -2s;
        }
        @keyframes twinkle {
            25%  {stop-color: lightYellow;}
            50%  {stop-color: gold; }
            75%  {stop-color: tomato;}
        }
    </style>
    <symbol id="star" viewBox="0 0 200 200">
        <path d="M100,10 L150,140 20,50 180,50 50,140 Z" />
    </symbol>
    <linearGradient id="shine" gradientTransform="rotate(20)">
        <stop offset="0" />
        <stop offset="0.25" />
        <stop offset="0.5" />
        <stop offset="0.75" />
        <stop offset="1" />
    </linearGradient>
    <rect height="100%" width="100%" fill="#222"/>
    <use xlink:href="#star" width="50" height="50" class="star"
         transform="translate(10,20) rotate(-10)"/>
    <use xlink:href="#star" width="40" height="40" class="star"
         transform="translate(140,10) rotate(20)"/>
    <use xlink:href="#star" width="35" height="35" class="star"
         transform="translate(80,60) rotate(-5)"/>
</svg>

The oscillating color stops could also be created with SMIL-style animation elements, by adding an <animate> element within each <stop>.  However, it is more useful to use animation elements for something that CSS animations cannot do: modifying the geometric attributes.

Example 14-4 uses two animations to modify the x1 and y2 attributes of the <linearGradient> element.  A still screenshot of the graphic would look quite similar to Figure 14-2, but the dynamic effect is quite different: instead of having each color twinkle in place, they stretch and shift like gentle waves.
Example 14-4. Animating the gradient vector with animation elements
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="4.3in" height="2.15in" viewBox="0 0 200 100">
    <title>SMIL-Animated Gradient</title>
    <style type="text/css">
        .star {
            fill: url(#shine);
        }
    </style>
    <symbol id="star" viewBox="0 0 200 200">
        <path d="M100,10 L150,140 20,50 180,50 50,140 Z" />
    </symbol>
    <linearGradient id="shine" spreadMethod="repeat"
                    gradientTransform="rotate(20)" >
        <animate attributeName="y2" values="1;1.5;1;0.75;1"
                 dur="2s" repeatDur="indefinite" />
        <animate attributeName="x1" values="0;0.5;0"
                 dur="3s" repeatDur="indefinite" />
        <stop offset="0" stop-color="gold" />
        <stop offset="0.25" stop-color="lightYellow" />
        <stop offset="0.5" stop-color="gold" />
        <stop offset="0.75" stop-color="tomato" />
        <stop offset="1" stop-color="gold" />
    </linearGradient>
    <rect height="100%" width="100%" fill="#222"/>
    <use xlink:href="#star" width="50" height="50" class="star"
         transform="translate(10,20) rotate(-10)"/>
    <use xlink:href="#star" width="40" height="40" class="star"
         transform="translate(140,10) rotate(20)"/>
    <use xlink:href="#star" width="35" height="35" class="star"
         transform="translate(80,60) rotate(-5)"/>
</svg>

Once again, all three stars animate in perfect synchonization.  For some graphics, you may be able to disrupt the repetition by using a repeating userSpaceOnUse graphic that is slightly different under each shape.  For the three stars, that approach does not have an effect: each star is drawn in the same position within its own <symbol> coordinate system, so each star would be drawn at the exact same point of a user-space gradient.
Animated Interactions
One of the most important uses of animation in modern websites is to provide user feedback and continuity.  Content should visibly change when users interact with it, but it should do so in a smooth way so that the user can intuitively understand the connection between the old and new states.

If you are using animations to represent the user’s interaction with a particular element, you clearly cannot have the same animation happening simultaneously on all similar elements in the web page.  One solution is to have a dedicated paint server element (or elements) that applies the animation effects and is only used when needed.  Other paint servers render the static states of the element.

Animating one element (a paint server) when the user interacts with another (a graphical icon) is difficult to impossible with CSS animations.  Therefore, we’ll start with a SMIL-based approach, then show how to create the same effect—with more work but more flexibility and good browser support—in JavaScript.

The SMIL animation approach takes advantage of timing attributes for synchronizing multiple animations.  It uses the <set> element to switch the paint server from the shared, static gradient to an animatable paint server, and then uses <animate> elements to implement the effect.  Finally, another <set> element switches the graphic to the final state, which is again a shared paint server.
Warning
Blink and Webkit browsers will not animate URL references to paint servers, even with a <set> element.  Given the Chromium project’s plan to deprecate SMIL support, this is not expected to be fixed.  The example therefore only works on Firefox.

Example 14-5 uses this method to implement a click effect on star icons, such as might be used to indicate favourite or bookmarked elements.  The stars start out filled with a silver-gray gradient.  Upon activation, a gold fill radiates out from the center of the element, before fading into a gold-shine linear gradient.  The animated effects involve multiple gradients, layered within a <pattern> element; in addition to animating the radius of the radial gradient, the opacity of the pattern layers are modified for smooth transitions.

Figure 14-3 shows a screenshot in which one icon is in the final gold state, one is in the initial silver state, and the other has just been clicked and is partway through the radiating animation.
Example 14-5. Animating a single element’s paint upon user interaction, with animation elements
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Interactive Animated Gradient with SMIL</title>
    <style type='text/css'>
        html {
            background-color: #222;
            color: lightSkyBlue;
        }
        svg.defs-only {
            display: block;
            position: absolute;
            height: 0; width: 0;
            overflow: hidden;
        }

        [role="checkbox"]{
            display: inline-block;
            max-width: 33%;
            min-width: 5em;
            padding: 0; margin: 0;
            font-size: larger;
        }
        figcaption {
            display: block;
            text-align: center;
        }
        [role="checkbox"] > svg {
            display: block;
            margin: auto;
            width: 4em;
            height: 4em;
        }
        .star {
            fill: url(#silver-shine);
        }
    </style>
</head>
<body>
    <svg class="defs-only">
        <linearGradient id="silver-shine" spreadMethod="repeat"
                        gradientTransform="rotate(20)" >
            <stop offset="0" stop-color="gray" />
            <stop offset="0.35" stop-color="silver" />
            <stop offset="1" stop-color="gray" />
        </linearGradient>
        <linearGradient id="gold-shine" spreadMethod="repeat"
                        gradientTransform="rotate(20)" >
            <stop offset="0" stop-color="gold" />
            <stop offset="0.35" stop-color="lightYellow" />
            <stop offset="1" stop-color="gold" />
        </linearGradient>
        <radialGradient id="gold-ripple" r="0.2">
            <animate id="ripple"
                     attributeName="r" from="0.1" to="1"
                     dur="0.7s" fill="freeze"
                     begin="reaction.begin + 0.1s" />
            <stop offset="0" stop-color="gold" />
            <stop offset="0.5" stop-color="lightYellow" />
            <stop offset="0.75" stop-color="silver" />
            <stop offset="1" stop-color="gray" />
        </radialGradient>
        <pattern id="turn-gold" width="1" height="1"
                 patternContentUnits="objectBoundingBox">
            <rect fill="url(#gold-shine)" width="1" height="1" />
            <rect fill="url(#silver-shine)" width="1" height="1" >
                <set attributeName="opacity" to="0"
                     begin="reaction.begin + 0.5s" />
            </rect>
            <rect fill="url(#gold-ripple)" width="1" height="1" >
                <animate id="reaction"
                         attributeName="opacity"
                         values="0;1;1;0"
                         keyTimes="0;0.2;0.8;1"
                         dur="1s"
                         begin="switch1.begin;
                         switch2.begin; switch3.begin" />
            </rect>
        </pattern>

        <symbol id="star" viewBox="0 0 200 200">
            <path d="M100,10 L150,140 20,50 180,50 50,140 Z" />
        </symbol>
    </svg>
    <h1>Pick your favourites:</h1>
    <div>
        <figure id="opt1" role="checkbox" tabindex="0">
            <figcaption>Option 1</figcaption>
            <svg><use class="star" xlink:href="#star">
                <set id="switch1"
                     attributeName="fill" to="url(#turn-gold)"
                     dur="1s" begin="opt1.click; opt1.activate"/>
                <set attributeName="fill" to="url(#gold-shine)"
                     dur="indefinite" begin="switch1.end" />
            </use></svg>
        </figure>
        <figure id="opt2" role="checkbox" tabindex="0">
            <figcaption>Option 2</figcaption>
            <svg><use class="star" xlink:href="#star">
                <set id="switch2"
                     attributeName="fill" to="url(#turn-gold)"
                     dur="1s" begin="opt2.click; opt2.activate"/>
                <set attributeName="fill" to="url(#gold-shine)"
                     dur="indefinite" begin="switch2.end" />
            </use></svg>
        </figure>
        <figure id="opt3" role="checkbox" tabindex="0">
            <figcaption>Option 3</figcaption>
            <svg><use class="star" xlink:href="#star">
                <set id="switch3"
                     attributeName="fill" to="url(#turn-gold)"
                     dur="1s" begin="opt3.click; opt3.activate"/>
                <set attributeName="fill" to="url(#gold-shine)"
                     dur="indefinite" begin="switch3.end" />
            </use></svg>
        </figure>
    </div>
    <script>
        /* do something based on the selected options */
    </script>
</body>
</html>

There are limitations to this approach beyond the poor browser support:

- 
If the user selects two items less than 1 second apart, the first animation will be reset, synchronizing with the second one.

- 
The animation element that drives the main reaction must reference all events that could trigger it in its begin attribute.  Example 14-5 simplifies it somewhat by having that animation respond to the <set> animations, rather than the original user events.  However, adding new interactive icons to the web page would still require new values in the begin attribute.

- 
The selection does not toggle and the animation does not reverse the next time the icon is clicked; subsequent clicks restart the same animation sequence, but do not de-select the item.  If you wanted to implement a reverse animation, you would need to add a separate transparent SVG element that appears overtop of the main icon when it is selected (using another <set> animation) and catches all user events (using pointer-events:all).  This element would then be used to trigger the de-selection.1  You would also need new <animate> and <set> elements to control the reverse animation sequence.

- 
Although the icons have been given the ARIA role of checkbox, so that screen reader users can activate them, the checked state of that element is not currently updated when the icon is selected.  This would need to be implemented within the script, since SMIL animations cannot modify attributes on HTML elements in any browser.

These issues are solved by Example 14-6.  It also integrates keyboard support and adds a new feature: when an icon is activated based on a mouse click or tap, the radial gradient spreads outward from the touch point.  This type of subtle reaction to user events is recommended by many interaction design guidelines, including Google’s Material Design style guide.
Example 14-6. Animating a single element’s paint upon user interaction, with JavaScriptExample 14-6. HTML Markup:
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Interactive Animated Gradient with JavaScript</title>
    <style type='text/css'>
        /* styles unchanged */
    </style>
</head>
<body>
    <svg class="defs-only">
        <linearGradient id="silver-shine" spreadMethod="repeat"
                        gradientTransform="rotate(20)" >
            <stop offset="0" stop-color="gray" />
            <stop offset="0.25" stop-color="silver" />
            <stop offset="1" stop-color="gray" />
        </linearGradient>
        <linearGradient id="gold-shine" spreadMethod="repeat"
                        gradientTransform="rotate(20)" >
            <stop offset="0" stop-color="gold" />
            <stop offset="0.25" stop-color="lightYellow" />
            <stop offset="1" stop-color="gold" />
        </linearGradient>
        <radialGradient id="gold-ripple" r="0.2">
            <stop offset="0" stop-color="gold" />
            <stop offset="0.5" stop-color="lightYellow" />
            <stop offset="0.75" stop-color="silver" />
            <stop offset="1" stop-color="gray" />
        </radialGradient>
        <pattern id="turn-gold" width="1" height="1"
                 patternContentUnits="objectBoundingBox">
            <rect class="transition"
                  fill="url(#gold-ripple)" width="1" height="1" />
            <rect class="on"
                  fill="url(#gold-shine)" width="1" height="1" />
            <rect class="off"
                  fill="url(#silver-shine)" width="1" height="1" />
        </pattern>
        <symbol id="star" viewBox="0 0 200 200">
            <path d="M100,10 L150,140 20,50 180,50 50,140 Z" />
        </symbol>
    </svg>
    <h1>Pick your favourite:</h1>
    <div>
        <figure id="opt1" role="checkbox">
            <figcaption>Option 1</figcaption>
            <svg><use xlink:href="#star"/></svg>
        </figure>
        <figure id="opt2" role="checkbox">
            <figcaption>Option 2</figcaption>
            <svg><use xlink:href="#star"/></svg>
        </figure>
        <figure id="opt3" role="checkbox">
            <figcaption>Option 3</figcaption>
            <svg><use xlink:href="#star"/></svg>
        </figure>
    </div>
    <script>
        /* Script included at end of file
           or as an async-loaded external resource */
    </script>
</body>
</html>
Example 14-6. JavaScript:
(function(){
    var toggles = document.querySelectorAll("[role='checkbox']");
    var selectGraphic = "[*|href='#star']";
    var svg = document.querySelector("svg");
    var paint = {
                  off: "url(#silver-shine)",
                  animate: "url(#turn-gold)",
                  on: "url(#gold-shine)"
                };
    var animating = false,
        animatingOption,
        nextFrame;

    for (var i=0, n=toggles.length; i<n; i++){
        toggles[i].setAttribute("aria-checked", false);
        toggles[i].querySelector(selectGraphic).style.fill
            = paint.off;
        toggles[i].addEventListener("click", toggleState);
        toggles[i].addEventListener("activate", toggleState);
        toggles[i].addEventListener("keyup", checkKey);
    }
    function checkKey(e) {
        //check for spacebar or enter key,
        //using both the new standard syntax
        //and the old keycode syntax
        if ( (e.key == " ")||(e.key == "Enter") ||
             (e.keyCode == 32 )||(e.keyCode == 13) )
            toggleState.apply(this, arguments);
    }
    function toggleState(e){
        var currentlyChecked =
                (this.getAttribute("aria-checked")
                        === true.toString() );

        //update the actual state
        this.setAttribute("aria-checked", !currentlyChecked);
            /* maybe do something based on the selected options */

        if (currentlyChecked) {
            //animate turning off quickly
            animateStar(this, e, 300, true);
        }
        else {
            //animate turning on, more slowly
            animateStar(this, e, 1000);
        }
    }

    function animateStar(option, event, dur, reverse) {
        if ((!dur)||isNaN(dur)) return;
            //must have a valid animation duration

        /* animation parameters */
        var effects = [
                    {selector:"#gold-ripple", attr:"r",
                     from:0.1, by:0.9, t1:0.1, t2:0.8},
                    {selector:"#turn-gold .on", attr:"opacity",
                     from:0, by:1, t1:0.8, t2:1},
                    {selector:"#turn-gold .off", attr:"opacity",
                     from:1, by:-1, t1:0, t2:0.2}
                    ];
        var selectTracker = "#gold-ripple";
        var star = option.querySelector(selectGraphic);
        var startTime = 0;

        if (animating){
            //abort current animation
            cancelAnimationFrame(nextFrame);
            if (animatingOption != option) {
                //tidy up the current animation by setting
                //it to the correct end state
                animatingOption.querySelector(selectGraphic)
                    .style.fill =
                    (animatingOption.getAttribute("aria-checked")
                            === true.toString() ) ?
                    paint.on : paint.off;
            }
            else {
                //set the new animation to start from the current state
                effects.forEach(function(effect){
                    var current = parseFloat(
                            document.querySelector(effect.selector)
                                .getAttribute(effect.attr) );
                    if (reverse) {
                        effect.by = current - effect.from;
                    }
                    else {
                        effect.by = (effect.from + effect.by) - current;
                        effect.from = current;
                    }
                });
            }
        }

        var track = track = document.querySelector(selectTracker);
        if (event instanceof MouseEvent) {
            //re-center the radial gradient
            //to track the mouse event
            //by converting mouse coordinates first to
            //userSpace coordinates for the star,
            //and then to bounding box coordinates
            var bbox = star.getBBox(),
                CTM = star.getScreenCTM().inverse(),
                p = svg.createSVGPoint(),
                p2;
            p.x = event.clientX;  //NOT screenX and screenY
            p.y = event.clientY;
            p2 = p.matrixTransform(CTM);
            track.setAttribute("cx", (p2.x - bbox.x)/bbox.width);
            track.setAttribute("cy", (p2.y - bbox.y)/bbox.height);
        }
        else {
            track.setAttribute("cx", 0.5);
            track.setAttribute("cy", 0.5);
        }

        //set overall animation parameters & initialize
        animating = true;
        animatingOption = option;
        requestAnimationFrame(function(t){
            startTime = t;
            star.style.fill = paint.animate;
        });

        //create a function to transform a time point
        //into a position in the animation effects
        var getProgress = reverse ?
            function(t){ return 1 - (t-startTime)/dur; }
            :
            function(t){ return (t-startTime)/dur; };

        //determine which element will be animated for each effect
        effects.forEach(function(effect){
            effect.node = document.querySelector(effect.selector);
        });

        function applyEffects(t){
            var a = getProgress(t),
                node,
                val;
            effects.forEach(function(effect){
                if(a <= effect.t1) {
                    val = effect.from;
                }
                else if (a >= effect.t2) {
                    val = effect.from + effect.by;
                }
                else {
                    val = effect.from + effect.by*(
                        (a - effect.t1)/(effect.t2 - effect.t1) );
                }
                effect.node.setAttribute(effect.attr, val);
            });

            if ( (reverse&&(a>0))
                || ((!reverse)&&(a < 1)) ) {
                //loop
                nextFrame = requestAnimationFrame(applyEffects);
            }
            else {
                //animation is complete
                star.style.fill = reverse? paint.off : paint.on;
                animating = false;
            }
        }
        //start updating
        nextFrame = requestAnimationFrame(applyEffects);
    }
})();

The markup is almost the same as Example 14-5, minus the animation elements.  The one change is to the ordering of the layers within the <pattern>.  They have been changed so that each animation effect can be described by a single-direction animation, defined by a starting (from) value and the net change (by) amount.  Instead of having the animated layer fade in and then out, it is drawn as the bottom layer and the other layers are faded on top.

The overall pattern of actions after a user event is as follows:

- 
Determine whether the action is toggling on or off, and update the ARIA state (the toggleState function).

- 
Call the animation set-up function (animateStar), to either run a 1000ms turn-on animation or a 300ms turn-off animation.  Times are in milliseconds because most JavaScript and DOM timing methods use milliseconds.  The turn-off animation runs faster because we want to quickly get the effect out of the user’s way.

- 
Check if another animation is running, and if so cancel the pending animation request.  If that animation was on a different star, immediately switch that star to its final state.  Otherwise, if it was on the same star, adjust the parameters for the current animation so that it smoothly continues from the current state.

This step would be slightly easier if the effects were defined using a from-to syntax (both end points specified as absolute values) instead of the relative from-by syntax.  The from-by approach, however, makes for slightly simpler animation loops.

- 
If this was a mouse event (which includes clicks created by taps on a touch screen), center the radial gradient on the mouse point.  This section uses a number of methods unique to the SVG DOM:

- 
*element*.getBBox() returns the bounding box of an object in its local coordinate system as an object with properties x, y, width, and height.

- 
*element*.getScreenCTM() returns the cumulative transformation matrix (CTM) between that element’s coordinate system and the root coordinate system for the page.  Despite the unfortunate name, this is not the *screen* coordinates used in mouse events, but instead the *client* coordinate system.

- 
*matrix*.inverse() returns a new transformation matrix that exactly reverses the transformation created by the original matrix.

- 
*svg*.createSVGPoint() generates an SVG data object that can hold x and y numerical values.  SVGPoint objects are used when accessing the points of a polygon or polyline.  The only reason to create your own is for the next method.

- 
*point*.matrixTransform(*matrix*) calculates the position of the given point after applying the specified matrix transformation.

Using these methods, the mouse coordinates from the event are converted to the coordinate system used for the star, and then the star’s bounding box is used to convert to object bounding box coordinates for the gradient’s cx and cy attributes.  If the toggle was triggered by a keyboard event, cx and cy are re-set to the center of the element.

- 
Call a one-time animation function to initialize the animation, by changing the fill property of the star to the animating pattern, and recording the start time for the animation loop.

- 
Define a function (getProgress) to convert between document timestamps and a fraction of the time between the start and end of the animation effect.  The reverse direction is factored in at this point.  If you wanted to use an easing function to slow down or speed up the animation as it progresses, this would be the place to incorporate it.

- 
Identify which element will be animated for each effect, and store it in a variable for quick access.

- 
Finally, apply the changing effects at each stage of the animation, according to the timestamp passed to the callback (applyEffects) function by the browser in response to an animation frame request.  The effects are defined using an array of data objects; each object has the same structure so that it can be applied using a single function that calculates the current value and update’s the corresponding attribute of a specified element.

The method used here only supports the interpolation of simple numerical values, and only supports changes to attributes (including presentation attributes).  If you wanted to animate colors, complex lists of data, or animate style values, additional code would be required.

- 
Within the animation callback, check whether the animation is complete; if not, request another animation frame; if so, switch the star to its final gradient state.

The only limitation remaining is that the animation on one star is aborted if another star is interacted with.  This is necessary to avoid conflict, since there is only a single paint server used to draw the animation states of all the stars.  To work around this, you would need to dynamically create separate copies of the paint servers (the <radialGradient> and the <pattern> that references it) for each element being animated.  These DOM manipulations would have performance impacts, so you would need to decide if they were worth the small chance of overlapping interactions.  Of course, the likelihood of two interactions overlapping will depend on how long each animation lasts.  A 1-second animation is actually considered quite long for user interaction animations.

This is all a lot of code for a subtle interaction effect.  For this reason, it is rare to write a complete custom animation script for each project.  JavaScript libraries such as Snap.svg, D3.js, or the Greensock Animation Platform provide methods to efficiently control the timing of animation and interpolation of values.  As browser support increases for the Web Animations API, that will become the preferred method of defining specific animation effects.

Nonetheless, the overall strategy of creating an animation by switching between paint servers and animating the paint server components would still apply when using those tools.  That said, this particular effect will get much easier when there is good browser support for layered fills, CSS gradients as a fill value, and CSS transitions of gradient functions.

1 If you are interested in creating toggled SMIL animations (for other types of animation, such as shape morphing, that have better browser support), Michael J. Harris has written a good tutorial at *http://codepen.io/mikemjharris/blog/svg-toggling*