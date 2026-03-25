Chapter 5. Chapter 5: UI/UX Animations with CSS
In the previous chapters we’ve mostly covered standalone SVG animations. In this chapter, we’ll go over more common use-cases of UI and UX elements that can be implemented with SVG and animated with CSS. In particular, we’ll work through a common UX pattern of a transforming icon, which will allow you to see how something is built from start to finish, integrating the workflow into your own development process.
Chapter 5. Context-Shifting in UX Patterns
Before we get into how to build typical UI/UX interactions into SVG Animations, let’s go over the *why.* It’s important to get the technique down, but it’s just as important to use animation correctly.

Have you ever had a day at work where people kept interrupting you and putting you to different types of tasks? Work feels more frustrating when you can’t get into a flow-based working style, and it makes you feel more disorganized and unproductive. It follows that using a website works the same way.

p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

When users visit your site, they use a phenomenon called “saccade”. Saccade is a rapid eye movement, used by your brain to create spatial relationships. Your never really “look” at a photo: your eye moves constantly to understand where things are placed in the picture, creating a mental map of the image.

When we create a website, we’re creating a mental map for our users. Changes we make to site interactions can break that mental map. Modals are a good example: they often pop up out of nowhere, shattering the user’s experience, and are an example of what I call “brute-force UX”.

p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

An animation that reduces friction in context-shifting succeeds by honoring the user’s mental map: the user will retrieve and access things from consistent areas, the UX flows with their needs, and the whole experience feels more fluid. Creating animations that help guide your users take a bit of thinking, so let’s break down some ways we can execute them.

- Morphing

- Revealing

- Isolation

- Style

- Anticipatory Cues

- Interaction

- Space Conservation

Before we dive into solutions, it’s important to note that any one of these can be *overdone*.
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

Our brains have evolved to take particular note of something in motion. This evolutionary trait is in place to keep us safe and alert; the part of your brain that kicks up adrenaline is also triggered when something unexpected moves on the screen. The web is a static, dull, box without animation: but when it comes to UX animation, subtlety is key.

Here’s an example I built to show how an animation can retain context for a user: http://codepen.io/sdras/full/qOdwdB/, we’ll refer back to this one a lot as we cover these premises.
Chapter 5. Morphing
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

Morphing is probably the simplest way to help retain context for a user. Morphing means that the same element can become multiple pieces of information in different contexts, guiding the user’s flow without changing anything very abruptly. Consider the animation above. There are many forms of morphing in the one interactive element used in the CodePen example. In the example, 
p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Arial}

one frame morphs into the next: the pin expands to create the dialog, the contact button becomes the title, the text boxes shrink to become the loader dots, and so on, to give the whole experience a smooth user experience.

Both SVG and CSS are good options for these kind of UI animations. I’ve found, from working with both, that each have their strength and weaknesses. CSS easily interpolates round to square and back again withborder-radius. It can also handle large quantities of scale transforms gracefully, while SVG, beyond a few multitudes, will appear pixelated before recovering. However, SVG is built for drawing. It is well suited for complex shapes.

You can tween path or even shape data with JavaScript and GreenSock’s MorphSVG plugin. This is an unbeatable tool for this kind of technique, as unlike Snap.svg or even the poorly supported SMIL, MorphSVG allows you to easily transform between ***uneven amounts*** of path data, which allows for tons of wonderful effects. If you’re interested in learning more about what you can do with SVG Morphing, please turn to Chapter 12 of this book, where we discuss it at length.
Chapter 5. Revealing
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

Revealing is a very simple method of retaining context for the user, but revealing can be done in a way that breaks the user’s context as well. Take your typical modal, for instance. This is an example of UX that comes when called, but it does the opposite of retaining context for the user: it suddenly shatters their focus and the spatial map they’ve created. As a user, I sometimes close modals with information I need because I find them so jarring.

p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

Modals themselves are not the culprit here, though: it’s the way we typically implement them. Here’s an example of a modal that retains the context instead: it opens from it’s origin, and replaces itself where it was. There’s a transition between these two states, and as a user I know where that information “lives” and where to retrieve it again.

http://codepen.io/sdras/full/yOjWdO/

You can see this in our original example as well. We reveal information from our location on the map, and see where it was put away. We don’t need everything on the page at once, but we can see where we would get it should we need it.
Chapter 5. Isolation
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

We’ve established that we’re constantly scanning to create a spacial map with saccade, and thus isolation of pieces of our helps us wade through information faster. UIs can become cluttered: narrowing choices decreases the number of decisions, which helps users feel more empowered. Consider the following demo:

http://codepen.io/sdras/full/qOdWEP

p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

At first there’s so much information on the page that it’s hard to focus on one thing. But if we adjust the UI slightly (in this case adding a :hover state) we can clarify our user’s attention.
Chapter 5. Style
Style, design, branding and eases are all very closely tied. If you keep your animation style unified across your brand (and you should), this becomes your **Motion Design Language**. Motion Design Languages are important because it means that everyone is on the same page about what types of animations you’re going to be creating. For this very reason, you can keep your code DRY (don’t repeat yourself) by reusing eases in variables, interactions in functions, and keep consistent behavior across your site and even multiple platforms. I don’t code Java for Android or Swift for iOS (yet),
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

but I can retain consistency across these platforms and the web by nailing down a styleguide for animations that will apply to all of them.

p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

How do eases come into play in this? Eases are a really strong piece of the branding of an animation. If you work for a stoic company like a bank or financial institution, your eases are more likely to be Sine or Circ; if you work for a more playful company like MailChimp or Wufoo, a Bounce or Elastic ease would be more suitable. (see accents in eases section below for a visual illustration of sine vs bounce eases)

Here are some sites that allow you to pick out the eases you could be using for your project:

- CSS: http://cubic-bezier.com/

- CSS: http://easings.net/

- GSAP: http://greensock.com/ease-visualizer

- React-Motion: http://chenglou.github.io/react-motion/demos/demo5-spring-parameters-chooser/
Chapter 5.  Chapter 5. Anticipatory Cues
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

Users providing information to a site experience a period of unrest: they’re not sure what happened, who they gave their information to, or whether it worked.  It often takes more than a second for their information to be processed, which makes anticipatory actions extremely important.

Other small examples of anticipation states are:

- A dropdown selection changing other contexts on the page

- A loading state

- A button being pushed

- A login being rejected

- A form being processed after submission

- Some data being saved

When these type of changes occur, it might not make sense to have a grand change, but still signify that the state of the page has changed or is in the process of transitioning- creating a context in and of itself. Considering the techniques we spoke about earlier, we might ask ourselves:

- Are we captivating the viewer during the transitional state, or is it simply a small means to arrive at the end state?

- Will this transitional state be reused for other instances? Does it need to be designed to be flexible enough for multiple placements and failure conditions?

- Does the movement need to express the activity? An example of this would be the user saving something that’s not complete yet, in which case an anthropomorphization of “wait” would help communicate this.

Giving the user a loading state not only informs them that something is going on in the background, but if it’s a custom loader, it can make the wait time *feel* less long and obtrusive, giving your site/app the **appearance of higher performance**.

Consider the following demo:

http://codepen.io/sdras/full/LEorev/

The wait transforms directly from the button, giving a smooth transition state. We get a bright green confirmation screen, but not before the loader animates. We were actually waiting for a second or two before the final confirmation, but it was almost unnoticeable.
Chapter 5. Interaction
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

You learn more by *doing*. It’s an old adage, but an accurate one. When a user engages with your UI, they are forming more meaningful structural awareness than they could by viewing it alone.

Rather than simply selecting an item and having it transition before the viewer’s eyes,
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

interconnectivity between UI states can be strongly reinforced when the user carries the action forward. Consider these very well done Drag and Drop Interactions by Mary Lou (Manoela Ilic) on Codrops.

As a user, you know where you put the item, and you know where to retrieve it later. It’s at the bottom, right? There is no bottom- there is no drawer- it’s just a div. But because we built the animation and interaction in a way that makes it seem like it occupies a space, and mimics a real-world interaction that they already know (a cabinet drawer) we’ve built a space that they feel they control easily.
Chapter 5. Space Conservation
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

When we use animation to hide and display information that is not persistent on the page, we’re able to offer the user more: more to access, more tools, and more controls, in a more limited amount of space. This becomes increasingly important as we build out responsive environments that need to collapse a lot of material without feeling cluttered. Consider the animation below:

http://codepen.io/sdras/full/Kwjyzo/

We can honor the larger touch points that are needed for a mobile build while collapsing the navigation in a smaller space when it’s not needed. This navigation was built with Sara Soueidan’s Circulus tool, which build out a really nice circular SVG navigation that is easily animatable. https://sarasoueidan.com/tools/circulus/
Chapter 5. Putting it All Together
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

These animation theories and concepts work best when combined. There’s no right answer: the ability to be creative with SVG animation is part of it’s strength. Understanding the core concepts means that we have all the base understanding that we need; the code follows naturally.
Chapter 5. Icons that Transform
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

Now that we’ve discussed the “why” for animation in UI/UX patterns, let’s go over the “how”. For this example, we’ll build out a pretty common use-case so you can see step-by-step how to create an interaction. This doesn’t mean we’ll always use the same approach, but if you follow along, you can see how we’d break down a simple interaction like this one to build it into our site using an SVG.

p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

Icons are a nice way to add simple, useful, and informative animation to a site. Subtlety in this type of animation is key. If it’s too verbose or flashy, sometimes it can distract rather than serving the user.

This type of interaction should never feel like it takes too long. A common practice is keeping the transition between 0 - 300ms. Anything longer than that and the user feels like the transition is less than instantanious.

It’s also important to remember that any common UI or UX pattern that a user might see again and again should be subtle enough that it doesn’t feel ardous on repeated viewing.

In our example, we’re going to make a magnifying glass that morphs into a search field. We consider the beginning and intermediary states:

We’re going to morph the stem into the line, and make the circle turn into the container for the x. Let’s start with the magnifying glass.

In this example, we’re going to reveal the input when the event fires. In the case of simple UI animations, we’re moving a couple of small shapes from here to there, so simple storyboards are very helpful for planning them.

Focusing first on the stem being lengthened, let’s consider the things that need to happen between states. The stem itself must get longer, it has to rotate slightly, and it has to transform into place.

Let’s accommodate for the change in the size of the stem by lengthening the viewbox, so considering where we’re starting with the svg:

<svg class="magnifier" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 34">
    <title>magnifier</title>
    <circle class="cls-1" cx="12.1" cy="12.1" r="11.6"/>
    <line class="cls-1" x1="20.5" y1="20" x2="33.1" y2="32.6"/>
  </svg>

We adjust the viewbox to  <svg class="magnifier" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 300 34">

We’ll also make sure the SVG is primed in CSS (SCSS) for the future transformation, and back.

.magnifier {
  line {
    transform: rotate(0deg) translateY(0px);
  }
  circle {
    transform: scale(1);
  }
}

p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

We can change the state here in a few ways- in jQuery we would use a simple class operation, and in React we would address the state directly by calling getInitialState initially and then setting state with event handlers. Due to the fact that most people at the time of publishing are more familiar with jQuery, I’ve used it to demonstrate this, though we’ll address React in future chapters. We’ll use jQuery 3.0 (now backported to 1.X and 2.X as well) because it supports class operations on SVG.

If we think back to how the line element draws on to the SVG DOM, all we need to do to update the length of the line is alter value on the x2 attribute. In this case, we’ll lengthen it from 33.1 to 300.

$( document ).ready(function() {
  $(".main").on("click", function() {
    var magLine = $(this).find(".magnifier line");

    if ($(this).hasClass("open")) {
      $(this).removeClass("open");
      magLine.attr("x2", 33.1);
    } else {
      $(this).addClass("open");
      magLine.attr("x2", 300);
    }
    
  });
});

At this point, the line is lengthened but it’s drawn outside the viewbox because we haven’t rotated and transformed it yet. Let’s do that in CSS:

.open .magnifier {
  line {
    transform: rotate(-2.5deg) translateY(14px);
  }
  circle {
    transform: scale(0.5);
  }
}
CSS Transforms on SVG DOM Elements
As you experiment with CSS and SVG with transforms, you might notice that cross-browser stability begins to become hairy with complex movement, particularly when you’re adjusting something with a transform-origin. This is a major reason I work with GreenSock. GreenSock not only makes your SVG animations stable, but also fixes some transform-origin stacking behaviors that are defined counter-intuitively in the spec.

We don’t really need a full CSS animation with keyframes to interpolate, because it’s just from point A to point B, so we’re going to use a transition. We’ll also use a couple of custom eases in SCSS, which we’ll reuse as variables. One nice trick, and a possible gotcha is that ease-out functions are nice for entrances, while ease-in functions are great for exits. With that in mind, we’re going to use a quad easing function:

$quad: cubic-bezier(0.25, 0.46, 0.45, 0.94);
$quad-out: cubic-bezier(0.55, 0.085, 0.68, 0.53);

.open .magnifier {
  line {
    transition: 0.65s all $quad;
    transform: rotate(-2.5deg) translateY(14px);
  }
  circle {
    transition: 0.35s all $quad;
    transform: scale(0.5);
  }
}

You’ll notice we’re using the entrance animations on the open state. This part maybe seem a little backwards: the .open animations will be treated as our entrance animation state while our exit animations should be added to the initial property. It seems a little counter-intuitive at first, but makes sense the more you work with it. The exit animations make more visual sense when they collapse together and we’ll make them a little faster because it feels a little better when they fade more quickly.

.magnifier {
  line {
    transition: 0.25s all $quad-out;
    transform: rotate(0deg) translateY(0px);
  }
  circle {
    transition: 0.25s all $quad-out;
    transform: scale(1);
  }
}

Next let’s work on the circle and the x-out. In this case, we’ve added the x-out as it’s own SVG and positioned it appropriately, but we could have just as easily included it in the initial SVG. I didn’t do so because when I was initially creating the animation, I wasn’t sure where I would position it. Keeping it separate when creating our assets allowed for me to retain a little more flexibility in iterations. If your storyboards and designs are more formalized, it might serve for better cross-browser stability to have all elements contained within the same SVG DOM.

p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

The other reason to separate the elements was due to transform-origin values. When the line is encapsulated within its own SVG, I could easily declare 50% 50%, and refer to the center of the x; if I was using a larger SVG structure, the values would be more difficult to define. In future chapters, I’ll cover some GreenSock features that help a great deal with transform-origin values and designation, but for CSS, it’s worth it to tread lightly, due to cross-browser bugs and only one option for declaration.

.x-out {
  width: 6px;
  padding: 5px 6px;
  transition: 0.5s all $quad;
  cursor: pointer;
  line {
    stroke-width: 2px;
    opacity: 0;
    transform: scale(0);
    transform-origin: 50% 50%;
  }
}

//firefox hack for padding on x, as we mentioned previously in 
//the warning about cross-browser stability issues
@-moz-document url-prefix() { 
  .x-out {
   padding: 5px !important;
  }
}

.open .x-out line {
  opacity: 1;
  transform: scale(1);
  transition: 0.75s all $quad;
}

Finally, we can see that we’ll need to add an input for this to truly work. We’ll make sure the SVG and the input are in the same height placement with some absolute positioning:

.magnifier, input, .x-out {
  margin-left: 30vw;
  margin-top: 40vh;
  pointer: cursor;
  position: absolute;
}

.magnifier, input {
  width: 400px;
}

Then we’ll make sure that the input has no default native browser styling but that the font-size matches that of the size of the SVG:

input {
  font-size: 35px;
  padding-left: 30px;
  background: none;
  cursor: pointer;
  box-shadow: none;
  border: none;
  outline: none;
}

Now that the input is close to invisible, we want to allow the user to know that the area is focused and ready for user execution. We also want to clear the selection in the event that they close the search open state. We’ll do so with JavaScript:

$( document ).ready(function() {
  $(".main").on("click", function() {
    var magLine = $(this).find(".magnifier line"),
        mainInput = $(this).find("input");

    if ($(this).hasClass("open")) {
      $(this).removeClass("open");
      magLine.attr("x2", 33.1);
      mainInput.val("");
    } else {
      $(this).addClass("open");
      magLine.attr("x2", 300);
      mainInput.focus();
    }
    
  });
});

http://codepen.io/sdras/full/BKaYyG

If you’re completely morphing an entire path in SVG, please check out chapter_, because JavaScript, and GreenSock’s MorphSVG in particular, is the best option for that kind of movement. But typical simple movements can be achieved without any additional libraries.

This is, of course, just one way of working with one UX pattern. Most patterns, you’ll find, will follow suit with this type of decision-making and problem-solving in order to achieve some nice effects and moments.

There are some nice open source libraries that already do this type of interaction out of the box, such as Sara Soueidan’s https://sarasoueidan.com/demos/navicon-transformicons/, or Dennis Gaebel’s fork http://www.transformicons.com/. These might be worth checking out if you don’t desire something custom.