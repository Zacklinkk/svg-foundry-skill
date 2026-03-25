CHAPTER 13
Adding Interactivity
To this point, you, the author of the SVG document, have made all the decisions about
a graphic. Y ou decide what a static image should look like, and if there are any anima‐
tions, you decide when they start and stop. In this chapter, you will see how to hand
some of that control over to the person who is viewing your document.
The lowest level of interactivity is declarative interactivity —animation or other style
changes  created by telling the browser what should happen under certain situations,
without directly controlling the effect with a script. SVG provides a limited set of built-
in interactive states.
Using Links in SVG
The easiest sort of interactivity to provide is linking, accomplished with the <a> ele‐
ment.  By enclosing a graphic in this element, it becomes active; when clicked, you go
to the URL specified in the xlink:href  attribute. Y ou can link to another SVG file or,
depending upon your environment, a web page. In Example 13-1 , clicking the word
“Cat” will link to an SVG drawing of a cat; clicking the red, green, and blue shapes will
link to the World Wide Web Consortium’s SVG page. All the items within the second
link are individually linked to the same destination, not the entire bounding box. When
you test this example and move the cursor between the shapes, you will see that those
areas do not respond to clicks.
Example 13-1. Links in SVG
http://oreillymedia.github.io/svg-essentials-examples/ch13/svg_link.svg
<a xlink:href= "cat.svg" >
  <text x="100" y="30" style="font-size: 12pt;" >Cat</text>
</a>
<a xlink:href= "http://www.w3.org/SVG/" >
209

1.The Apache Batik SVG viewer, version 1.7, does not support CSS pseudoclass selectors.  <circle cx="50" cy="70" r="20" style="fill: red;" />
  <rect x="75" y="50" width="40" height="40" style="fill: green;" />
  <path d="M120 90, 140 50, 160 90 Z"  style="fill: blue;" />
</a>
In the <use>  element, xlink:href  specifies a resource that becomes  part of your graphic,
as described in “The <use> Element” on page 63 . For the <a> element, the xlink:href
attribute specifies a different resource to jump to.
Links in HTML are recognizable by color and underlining effects. Figure 13-1  shows
the results of Example 13-1 ; there’s nothing to tell you that the graphics are actually
linked, unless you notice the change of the cursor from an arrow to a “hand” icon.
Keyboard users have the same difficulty: some browsers outline elements if they have
keyboard focus, but others do not. Y ou can use CSS pseudoclasses to give users some
feedback about the interactive elements of your graphic. Like a CSS class, a pseudo‐
class  is used to apply styles to select instances of an element; unlike true classes, they are
applied automatically, not assigned in the class  attribute.1
Figure 13-1. A hyperlinked SVG, the results of Example 13-1
The :hover  pseudoclass applies when the main mouse pointer is over an element, while
the :focus  pseudoclass applies when an element has the keyboard focus. Y ou can often
use the same styles for both element:hover  and element:focus , because both indicate
the potential for user action.
Example 13-2  uses the same graphics as Example 13-1 , but now the links give feedback
when they are hovered or focused. The text will become bold and underlined, and the
shapes will get a light blue border.
Example 13-2. Links in SVG highlighted with CSS
http://oreillymedia.github.io/svg-essentials-examples/ch13/svg_css_link.svg
<style type="text/css" ><![CDATA[
    a.words:hover, a.words:focus {
       text-decoration : underline ;
       font-weight :bold;
    }
210 | Chapter 13: Adding Interactivity

    a.shapes:hover, a.shapes:focus {
       stroke: #66f;
       stroke-width : 2;
       outline: none; /* override default focus formatting */
    }
    ]]>
  </style>
  <a class="words" xlink:href= "cat.svg" >
    <text x="100" y="30" style="font-size: 12pt;" >Cat</text>
  </a>
  <a class="shapes"  xlink:href= "http://www.w3.org/SVG/" >
    <circle cx="50" cy="70" r="20" style="fill: red;" />
    <rect x="75" y="50" width="40" height="40" style="fill: green;" />
    <path d="M120 90, 140 50, 160 90 Z"  style="fill: blue;" />
  </a>
Controlling CSS Animations
What if you wanted more dynamic user feedback? CSS animations are defined as style
properties, so they can also be controlled by pseudoclasses. Example 13-3  starts an
animation when you hover the mouse over the shapes.
Example 13-3. Animating a link with :hover
http://oreillymedia.github.io/svg-essentials-examples/ch13/anim_css_link.svg
<style type="text/css" ><![CDATA[
  a.animatedLink  {
    animation-name : animKeys ;
    animation-iteration-count : infinite ;
    animation-duration : 0.5s;
    animation-direction : alternate ;
    animation-play-state : paused;
  }
  a.animatedLink :hover {
    animation-play-state : running;
  }
  @keyframes  animKeys  {
    0% {fill-opacity: 1.0;}
    100% {fill-opacity: 0.5;}
  }
  ]]>
  </style>
<a class="animatedLink"  xlink:href= "http://www.w3.org/SVG/" >
  <circle cx="50" cy="70" r="20" style="fill: red;" />
Controlling CSS Animations | 211

2.If you’re familiar with JavaScript event handling, you’ll recognize that the event names are the same as used
in DOM event handling. If you’re not familiar with JavaScript and DOM events, keep reading.  <rect x="75" y="50" width="40" height="40" style="fill: green;" />
  <path d="M120 90, 140 50, 160 90 Z"  style="fill: blue;" />
</a>
The example isn’t a great demonstration of user design: if you move the mouse out of
the link area while the animation is going on, it simply pauses but doesn’t revert back
to full opacity. If you’re interested in CSS animations, the latest draft specifications
describe all the possibilities.
User-Triggered SMIL Animations
CSS animations are limited in what sort of changes they can create and what sort of
events they can respond to. If you are using SMIL animation elements in your SVG, you
can use an alternative format for the begin  and end attributes to declare that they should
respond to user actions.
The format for an interactive animation timing attribute is elementID.eventName . The
element referenced in the ID does not have to be the same element that is being animated.
Interaction using the SMIL begin and end attributes is event based : once an animation
is started, it will continue until the duration of the animation is complete or an ending
event occurs. In contrast, interaction using CSS pseudoclasses is state based : the style
or animation is only applied for so long as the state is true. For example, the CSS
#myElement:hover  pseudoclass selector describes the state of the element with ID
myElement  when the mouse pointer is over it; to define an animation to occur
during the same period, you would set the animation attributes
begin="myElement.mouseover"  and end="myElement.mouseout" .2
For greater control, you can optionally add a time offset, of the form
elementID.eventName + offset . The offset is specified in the same format as for other
SMIL animation timing attributes (see “How Time Is Measured”  on page 194, in Chap‐
ter 12 ), with a unit like 1.5min  or as a stopwatch time like 01:30 . Y ou could even use a
negative time offset, but since there is no such thing as Psychic Vector Graphics, the
animation won’t actually start before the event occurs. Instead, as soon as the event
occurs the computer will skip the first part of the animation (which “should” have oc‐
curred before the event) and continue on with the rest of the animation.
Example 13-4  creates a trapezoid and a button. When you click the button, the trapezoid
rotates 360 degrees. Screenshots, before and after a click, are shown in Figure 13-2 .
212 | Chapter 13: Adding Interactivity

Figure 13-2. Screenshot of two stages of scripting with animation
Example 13-4. Interactive Animation
http://oreillymedia.github.io/svg-essentials-examples/ch13/smil_event_animation.svg
<g id="button" >     
  <rect x="10" y="10" width="40" height="20" rx="4" ry="4"
    style="fill: #ddd;" />
  <text x="30" y="25"
    style="text-anchor: middle; font-size: 8pt" >Start</text>
</g>
<g transform= "translate(100, 60)" >
  <path d="M-25 -15, 0 -15, 25 15, -25 15 Z"
    style="stroke: gray; fill: #699;" >
    <animateTransform  id="trapezoid"  attributeName= "transform"
      type="rotate"  from="0" to="360"
      begin="button.click"
      dur="6s"/>   
  </path>
</g>
The start button is a simple rounded rectangle with text. The entire group gets
the id.
Instead  of giving  the begin  time  for the animation  in terms  of seconds,  we begin
whenever a click  event is detected on the button  object.
It is often  easier  to design  the SVG  first and add the scripting  later.
One  advantage  of this method  is that you can see if the base  draw‐
ing looks good before you start making it react to events.
Scripting SVG
The next step up from these declarative animations—and it’s a big step—is scripting.
Y ou can write a program in ECMAScript to interact with an SVG graphic.  (ECMAScript
is the standardized version of what is commonly called JavaScript, as defined by ECMA,
an organization formerly known as the European Computer Manufacturer’s
Scripting SVG | 213

3.Most browsers also allow you get or set CSS properties using the form element.style.propertyName  or
element.style[ "property-name "]. However, this isn’t part of the CSS object model standards  and isn’t
supported in some other SVG viewers, or even consistently between browsers.Association .) If you’re new to ECMA/JavaScript—or programming in general—you’ll
want to read Appendix C .
As the SVG viewer reads the markup in an SVG document, it creates a tree of nodes ,
which are objects in memory that correspond to the structure and content of the mark‐
up. This is the Document Object Model , and it is accessible to your scripts.
The first thing you need to do in order to deal with the DOM is to access the nodes.
The main function you will probably use to deal with the DOM is
document.getElementById( idString ). This function takes a string that is the id of an
SVG element and returns a reference to that element’s node in the DOM. If you want
all of the elements in a document that have a particular tag name (the tag name is the
“svg” in <svg>  or “rect” in <rect> ), you can use another document  method,
getElementsByTagName( name); this returns an array of nodes.
Once you have an element’s node, you can:
•Read its attributes by calling element.getAttribute( attributeName ), which re‐
turns the attribute value as a string.
•Change an attribute’s value by calling element.setAttribute( name, newValue );
if the attribute with the given name  does not exist, it will be created.
•Remove attributes by calling element.removeAttribute( name).
Y ou could modify inline styles using element.setAttribute("style",
newStyleValue ), but this overwrites all styles on the element. Instead, you can work
with the element.style  property. Use:
•element.style.getPropertyValue( propertyName ) to access a specific style,
•element.style.setProperty( propertyName , newValue , priority ) to change it
(priority  is usually null, but could be “important”), and
•element.style.removeProperty( propertyName ) to delete it.
If you do want to set all styles at once, then you can directly modify
element.style.cssText , which is a string representation of all the styles in
property-name : value  format.3
If you need to access or modify the text content of any node, use the
element.textContent  property.  When you read this property, it returns the
214 | Chapter 13: Adding Interactivity

4.If you’re familiar with using the .innerHTML  property to modify the combined text and markup of all de‐
scendants of an element, be warned that this property is only defined in the specifications for nodes of type
HTMLElement , and many browsers and SVG viewers do not support it on SVG elements.concatenated  text of all of the node’s descendants. If you set it, you will replace any
descendant nodes with a single text block.4
Example 13-5  uses these functions to access the attributes of an SVG element,  display
them in text format, and modify an attribute. (This isn’t interactive, but bear with us;
we’re building suspense in the plot line.)
Example 13-5. Accessing SVG with the DOM
http://oreillymedia.github.io/svg-essentials-examples/ch13/basic_dom_example.svg
<svg width="300" height="100" viewBox= "0 0 300 100"
  xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink= "http://www.w3.org/1999/xlink" >
  <title>Accessing Content in SVG </title>
  <rect id="rectangle"  x="10" y="20" width="30" height="40"
    style="stroke:gray; fill: #ff9; stroke-width:3" /> 
  <text  id="output"  x="10" y="80" style="font-size:9pt" ></text>
  <script type="application/ecmascript" >
  // <![CDATA[ 
    var txt = document.getElementById("output"); 
    var r = document.getElementById("rectangle");
    var msg =  r.getAttribute("x") + ", " + 
      r.getAttribute("y") + " " +
      r.style.getPropertyValue("stroke") + " " +
      r.style.getPropertyValue("fill");
    r.setAttribute("height", "30"); 
    txt.textContent= msg; 
    // ]]>
  </script>
</svg>
In order to easily access an element from a script, give it a unique id.
The <![CDATA[  is used to ensure that any stray < or > signs are not interpreted
as markup.
Select elements from the document by id, and save the results in variables.
The results  of getAttribute()  and style.getPropertyValue()  are strings;
these are concatenated together with + to create the message string.
This changes the height of the rectangle to convert it to a square.
Finally, set the content of the <text>  element to display the attributes.
Scripting SVG | 215

The script  in Example  13-5  is included  in the SVG  file after  the <rect>
and <text>  elements that it uses. This ensures that the elements ex‐
ist in the DOM before the script is run.
Events: An Overview
Interaction occurs when graphic objects respond to events. There are several categories
of events. The text for many of these descriptions comes directly from the World Wide
Web Consortium’s specification .
User interface events
The focusIn  and focusOut  events occur when an element receives or loses focus,
such as selecting or unselecting text. The activate  element occurs when an element
is activated through a mouse click or keypress.
Mouse events
The mousedown  and mouseup  events occur when a pointing device button is pressed
or released on an element. If the screen location for these events is the same, then
a click  event is generated.
The mouseover , mousemove , and mouseout  events occur when the pointing device
is moved over an element, moved while over an element, and moved away from an
element.
Mutation events
The SVG viewer will generate events when the DOM changes (by another script);
for example, DOMNodeInserted  occurs when a node has been added as a child of
another node; DOMAttrModified  occurs when an attribute has been modified on a
node. This book will not cover these events in detail.
Document events
The SVGLoad  event is triggered when the SVG viewer has fully parsed a document
and is ready to act upon it (e.g., display it on a device). SVGUnload  occurs when a
document is removed from a window. SVGAbort  occurs when page loading is stop‐
ped before loading completes; SVGError  occurs when an element does not load
properly or an error occurs during script execution.
The SVGResize , SVGScroll , and SVGZoom  events occur when the viewer changes the
document in the way that the name suggests.
Animation events
The SVG viewer generates beginEvent , endEvent , and repeatEvent  when an ani‐
mation element begins, ends, or repeats; repeatEvent  is not generated for the first
iteration.
216 | Chapter 13: Adding Interactivity

5.This is a very simplistic definition. See the DOM Events specification  for the full details.Key events
There are no events built into SVG for keypresses, but  some viewers may support
nonstandard keydown  and keyup  events.
Listening for and Responding to Events
To allow an object to respond to an event, you must first tell the object to listen for the
event. Y ou do this by calling the addEventListener()  function. This function has two
required arguments. The first is a string with the name of the type of event you want to
listen for. The second argument is the name of a function that will handle the event. An
optional third argument is a boolean that tells whether you want to respond to the event
when the viewer is passing the event down the DOM hierarchy from parent elements
to children to find the specific target (“capture” stage). A value of false  (the usual)
causes your listener to wait to handle the event until after any child elements have dealt
with it; this is the “bubbling” stage, as events float to the top of the DOM tree.5
The function that handles the event takes one argument: an event object that contains
information about the event that triggered the call. The most important property of the
event object is the target  property,  which is the object to which the event was dis‐
patched. Other important event properties are clientX  and clientY , which give the
coordinates at which the event occurred relative to the DOM implementation’s client
area, which is the area occupied by the entire .svg file or web page.
Example 13-6  adds listeners for the mouseover , mouseout , and click  events to a cir‐
cle. Moving the mouse in and out of the circle will cause its radius to grow or shrink;
clicking the mouse will increase or decrease the circle’s stroke width.
Example 13-6. Adding mouse movement listeners
http://oreillymedia.github.io/svg-essentials-examples/ch13/simple_event.svg
  <circle id="circle"  cx="50" cy="50" r="20"
    style="fill: #ff9; stroke:black; stroke-width: 1" />
  <script type="application/ecmascript" ><![CDATA[
    function  grow(evt) {
      var obj = evt.target;
      obj.setAttribute ("r", "30");
    }
    function  shrink(evt) {
      this.setAttribute ("r", "20");
    }
Scripting SVG | 217

    function  reStroke (evt) {
      var w = evt.target.style.getPropertyValue ("stroke-width" );
      w = 4 - parseFloat (w); /* toggle between 1 and 3 */
      evt.target.style.setProperty ("stroke-width" , w, null);
    }
    var c = document .getElementById ("circle" );
    c.addEventListener ("mouseover" , grow);
    c.addEventListener ("mouseout" , shrink);
    c.addEventListener ("click", reStroke );
    // ]]>
  </script>
The first event handler, grow() , uses evt.target  to access the element that received the
event, and stores it in a separate variable. The second event handler, shrink() , uses the
reserved word this  to refer to the element that is attached to the event listener (which
in this case, but not always, is the same as the event target). The last event handler,
reStroke() , again uses evt.target , but without the use of a temporary variable.
While we could  have used c instead of evt.target  (because it is the only element with
a listener), this would have been a terrible idea, because you will often need to attach
the same event handler to many different targets, as in the next example.
Changing Attributes of Multiple Objects
Sometimes you will want an event that occurs on object A to affect attributes of both
object A and some other object B. Example 13-7  presents possibly the world’s crudest
example of SVGcommerce. Figure 13-3  shows a T-shirt whose size changes as the user
clicks each labeled button. The currently selected size button is highlighted in light
yellow.
218 | Chapter 13: Adding Interactivity

Figure 13-3. Screenshots of different selections
Example 13-7. Changing multiple objects in a script
http://oreillymedia.github.io/svg-essentials-examples/ch13/shirt1.svg
The XML code:
<svg width="400" height="250" viewBox= "0 0 400 250"
  xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink= "http://www.w3.org/1999/xlink"
  onload="init(evt)" > 
  <defs>
    <style type="text/css"  > <![CDATA[
      /* style rules will go here */
    ]]></style>
    <script type="application/ecmascript" > <![CDATA[
      /* script will go here */
    ]]></script>
    <path id="shirt-outline"
      d="M -6 -30 -32 -19 -25.5 -13 -22 -14 -22 30 23 30
        23 -14 26.5 -13 33 -19 7 -30
        A 6.5 6 0 0 1 -6 -30" /> 
  </defs>
  <g id="shirt" >
    <use xlink:href= "#shirt-outline"  x="0" y="0"/>
  </g>
  <g id="scale0"  >
    <rect x="100" y="10" width="30" height="30" />
    <text x="115" y="30">S</text>
  </g>
  <g id="scale1"  class="selected" > 
    <rect x="140" y="10" width="30" height="30" />
    <text x="155" y="30">M</text>
  </g>
Scripting SVG | 219

  <g id="scale2"  >
    <rect x="180" y="10" width="30" height="30" />
    <text x="195" y="30">L</text>
  </g>
</svg>
As soon  as the document  finishes  loading,  the SVGLoad  event  occurs,  and the
onload  handler  will call the init function,  passing  it the event  information.  Many
scripts  will use this event  handler  to make  sure  all their  variables  are set up
properly.  This  allows  you to put the <script>  before  the SVG  elements  to be
manipulated. Attributes of the form oneventname  can be used to listen to many
events,  but are discouraged  (in favor  of addEventListener() ) because  they  mix
your scripting functionality with your XML structure; document loading is an
exception.
The shirt outline is centered at (0, 0), so that it will scale from the center, and is
then positioned with transformations in the script.
The medium button is selected initially.
The styles:
    svg { /* default values */
       stroke: black;
       fill: white;
    }
    g.selected  rect {
       fill: #ffc; /* light yellow */
    }
    text {
       stroke: none;
       fill:black;
       text-anchor: middle;
    }
The “selected” class is used to indicate which size option is active, by filling the button
in light yellow.
The script:
    var scaleChoice  = 1;  
    var scaleFactor  = [1.25, 1.5, 1.75];
    function  init(evt) { 
      var obj;
      for (var i = 0; i < 3; i++) {
        obj = document .getElementById ("scale" + i);
        obj.addEventListener ("click", clickButton , false);
      }
      transformShirt ();
    }
220 | Chapter 13: Adding Interactivity

    function  clickButton (evt) {
      var choice = evt.target.parentNode ; 
      var name = choice.getAttribute ("id");
      var old = document .getElementById ("scale" + scaleChoice );
      old.removeAttribute ("class"); 
      choice.setAttribute ("class", "selected" );
      scaleChoice  = parseInt (name[name.length - 1]); 
      transformShirt ();
    }
    function  transformShirt () { 
      var factor = scaleFactor [scaleChoice ];
      var obj = document .getElementById ("shirt");
      obj.setAttribute ("transform" ,
        "translate(150, 150) "  +
        "scale("  + factor + ")");
      obj.setAttribute ("stroke-width" ,
        1 / factor);
    }
This  script  works  by keeping  track  of which  button  (S, M, or L) has been  chosen,
and indexing  into the corresponding  entry  in the scaleFactor  array.  The default
is index number one, medium.
The init()  function gets each rectangle-and-text <g> and tells it to listen for a
click  event. The function then displays the shirt at its current size.
A click  could  occur  on either  the text or the interior  of the rectangle.  Using
parentNode  puts the <g> object into variable choice .
Remove  the “selected”  class  from  the button  <g> corresponding  to the previously
selected size, and add it to the newly selected <g>.
Extract  the number  at the end of the button  group’s  id; this is the new
scaleChoice . It’s stored  in a global  variable,  accessible  by the transformShirt()
function.
The shirt  is resized  and positioned  by setting  its transform  attribute.  The stroke
width is rescaled by the inverse factor, so that it appears to stay the same when
the shirt is scaled up and down.
Dragging Objects
Let us expand this example by adding “sliders” that can be dragged to set the color of
the shirt, as shown in Figure 13-4 .
Scripting SVG | 221

Figure 13-4. Screenshot of color sliders
Y ou can experiment with the slider in the online example:
http://oreillymedia.github.io/svg-essentials-examples/ch13/drag_objects.svg
This script needs some more global variables. The first of these, slideChoice , tells which
slider (0, 1, or 2) is currently being dragged; its initial value is -1, meaning no slider is
active. The script also uses an array called rgb to hold the percent of red, green, and
blue; the initial values are all 100, because the shirt is initially white:
var slideChoice  = -1;
var rgb = [100, 100, 100];
Next, draw the sliders themselves. The color bar and the slide indicator are drawn on a
white background, and they are grouped together. The id attribute goes on the indicator
<line>  element, because its y-coordinate will be changing. The event handlers will be
attached to the enclosing <g> element. The group will then capture the mouse events
that happen on any of its child elements (this is why we drew the white rectangle; the
mouse will still track even if you drag outside the colored bar):
  <g id="sliderGroup0"  transform= "translate( 230, 10 )" >
    <rect x="-10" y="-5" width="40" height="110"/>
    <rect x="5" y="0" width="10" height="100" style="fill: red;" />
    <line id="slide0"  class="slider"
      x1="0" y1="0" x2="20" y2="0" />
  </g>
  <g id="sliderGroup1"  transform= "translate( 280, 10 )" >
    <rect x="-10" y="-5" width="40" height="110"/>
    <rect x="5" y="0" width="10" height="100" style="fill: green;" />
    <line id="slide1"  class="slider"
      x1="0" y1="0" x2="20" y2="0" />
  </g>
  <g id="sliderGroup2"  transform= "translate( 330, 10 )" >
    <rect x="-10" y="-5" width="40" height="110"/>
    <rect x="5" y="0" width="10" height="100" style="fill: blue;" />
    <line id="slide2"  class="slider"
222 | Chapter 13: Adding Interactivity

      x1="0" y1="0" x2="20" y2="0" />
  </g>
New style rules handle everything except for the slider-specific colors:
    line.slider {
       stroke: gray;
       stroke-width : 2;
    }
In the init()  function, add three event listeners to each slider group:
obj = document .getElementById ("sliderGroup"  + i);
obj.addEventListener ("mousedown" , startColorDrag , false);
obj.addEventListener ("mousemove" , doColorDrag , false);
obj.addEventListener ("mouseup" , endColorDrag , false);
The corresponding functions are as follows.
function  startColorDrag (evt) { 
  var sliderId  = evt.target.parentNode .getAttribute ("id");
  endColorDrag ( evt );
  slideChoice  = parseInt (sliderId [sliderId .length - 1]);
}
function  endColorDrag (evt) { 
  slideChoice  = -1;
}
function  doColorDrag (evt) { 
  var sliderId  = evt.target.parentNode .getAttribute ("id");
  chosen = parseInt (sliderId [sliderId .length - 1]);
  if (slideChoice  >= 0 && slideChoice  == chosen) {  
    var obj = evt.target; 
    var pos = evt.clientY - 10;
    if (pos < 0) { pos = 0; }
    if (pos > 100) { pos = 100; }
    obj = document .getElementById ("slide" + slideChoice ); 
    obj.setAttribute ("y1", pos);
    obj.setAttribute ("y2", pos);
    rgb[slideChoice ] = 100-pos; 
    var colorStr  = "rgb(" + rgb[0] + "%," +  
      rgb[1] + "%," + rgb[2] + "%)";
    obj = document .getElementById ("shirt");
    obj.style.setProperty ("fill", colorStr , null);
  }
}
Scripting SVG | 223

6.Other values for pointer-events  let you respond to an object’s events in the filled areas only ( fill ), outline
areas only ( stroke ), or the fill and outline together ( painted ), whether visible or not. Corresponding at‐
tribute values of visibleFill , visibleStroke , and visiblePainted  take the object’s visibility into account
as well.
startColorDrag(evt)  is called  on mousedown.  It stops  dragging  the current
slider (if any) and sets the current slider to the one specified ( 0 = red, 1 = green,
2 = blue).
endColorDrag(evt)  is called  on mouseup  or by other  functions.  It sets the slider
choice to -1, indicating that no slider is being dragged. No access to the event is
needed for this function.
doColorDrag(evt)  is called  on mousemove.  It uses  both  the event’s  target
property  (to determine  which  slider  is being  dragged)  and the clientY  property
to determine the mouse position relative to the top of the SVG.
Check that a slider is active and that the event is on the chosen slider.
Get the slider  indicator  line object,  and the mouse  position  (adjusted  by the
position of the top of the color bar). Clamp the position values to the range 0–
100.
Move the slider line to the new mouse position.
Calculate the new color value for this slider.
Compile  the color  values  into rgb()  notation  and change  the shirt’s  color
accordingly.
There’s only one minor point to take care of—the document will respond to an
onmouseup  only if it occurs within the slider area. So, if you click the mouse on the red
color bar, drag the mouse down to the shirt, then release the mouse button, the docu‐
ment will be unaware of it. When you then move the mouse over the red slider again,
it will still follow the mouse. To solve this problem, we insert a transparent rectangle
that completely covers the viewport, and it responds to a mouseup  event by calling
stopColorDrag . It will be the first, and therefore bottom-most object in the graphic. To
make the rectangle as unobtrusive as possible, it will be set to style="fill: none;" .
“But wait, ” you interject. “ A transparent area cannot respond to an event!” No, ordinarily
it can’t, but you can set the pointer-events  attribute to visible , meaning that an object
can respond to events as long as it is visible, no matter what its opacity:6
<rect id="eventCatcher"  x="0" y="0" width="400" height="300"
  style="fill: none;"  pointer-events= "visible"  />
The init()  function is modified to add the appropriate event listener:
document .getElementById ("eventCatcher" ).
  addEventListener ("mouseup" , endColorDrag , false);
224 | Chapter 13: Adding Interactivity

Interacting with an HTML Page
There are two ways to put interactive SVG into an HTML document, as described in
Chapter 2 . If you have a small amount of SVG, just put it directly into the HTML. If you
have a large SVG graphic, you can include it by using the <object>  element. The fol‐
lowing markup shows how you would do this for the preceding SVG example:
<object id="externalShirt"  data="shirt_interact.svg"
  type="image/svg+xml" >
  <p>Alas, your browser does not support SVG. </p>
</object>
The relevant attributes are the data  source for the graphic (a URL—in this case, a relative
pathname) and the type  attribute, which will be image/svg+xml . The HTML between
the opening and closing <object>  tags is only displayed if the object cannot be loaded.
Y ou can now add code to the SVG script and the HTML page’s script so they can com‐
municate with one another; the id attribute will come into play for that.
The web page will have a form that lets users type in the red, green, and blue percentages.
The values they enter will be reflected in the sliders. If users adjust the sliders, the values
in the form fields will be updated accordingly.
Here is the HTML document, with references to the (as yet unwritten) updateSVG()
function. This function will take the input field number and the value currently within
the input field as its arguments:
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" >
<head>
  <meta http-equiv= "content-type"  content= "text/html; charset=utf-8" >
  <title>SVG and HTML </title>
  <style type="text/css" >
    /* make form entries begin on a new line */
    label {display: block;}
    h1 {font-size : 125%;}
  </style>
  <script type="text/javascript" >
    /* script goes here */
  </script>
</head>
<body>
<h1>SVG and HTML </h1>
<div style="text-align:center" >
  <object id="shirt" data="shirt_interact.svg"
    type="image/svg+xml" >
    <p>Alas, your browser cannot load this SVG file. </p>
  </object>
  <form id="rgbForm" >
Scripting SVG | 225

    <label>Red: <input id="fld0" type="text" size="5" value="100"
      onchange= "updateSVG(0, this.value)"  />% </label>
    <label>Green: <input id="fld1" type="text" size="5" value="100"
      onchange= "updateSVG(1, this.value)"  />% </label>
    <label>Blue: <input id="fld2" type="text" size="5" value="100"
      onchange= "updateSVG(2, this.value)"  />%</label>
  </form>
</div>
</body>
</html>
In the interest  of keeping  the code  sample  short,  we’ve  used  poor
coding style and mixed our script in with our HTML. The attribute
onchange="updateSVG(0,  this.value)"  is equivalent  to adding  a
listener for the <input>  element’s change  event, and setting it to run
the command updateSVG(0, this.value) .
Here is the script that goes into the head of the HTML document. Function updateSVG
checks to see that the input value is an integer (it will discard any decimal part), and, if
so, calls function setShirtColor . This is actually a reference to a function that exists in
the SVG document, and it will be the SVG document’s responsibility to connect the
function to this HTML reference. (Y ou will see this happen later in the chapter.)
Function updateHTMLField  will be called from the SVG document’s script. It will receive
a form field number and a percent value, which it will display in the appropriate form
field:
function  updateSVG (which, amount) {
  amount = parseInt (amount);
  if (!isNaN(amount) && window.setShirtColor ) {
    window.setShirtColor (which, amount);
  }
}
function  updateHTMLField (which, percent) {
  document .getElementById ("fld" + which).value = percent;
}
Now you have to modify the SVG document. There are now two ways to set shirt color
—from the slider and from the HTML. Thus, the first task is to separate the setting of
the shirt color from the slider dragging. The modified doColorDrag(evt)  function de‐
tects the active slider and calculates the position of the slider, but it then calls a new
svgSetShirtColor  function to implement the change:
function  doColorDrag (evt) {
  if (slideChoice  >= 0) {
    var sliderId  = evt.target.parentNode .getAttribute ("id");
    chosen = parseInt (sliderId [sliderId .length - 1]);
    if (slideChoice  == chosen) {
226 | Chapter 13: Adding Interactivity

      svgSetShirtColor (slideChoice , 100 - (evt.clientY - 10));
    }
  }
}
Function svgSetShirtColor  will do what the remainder of doColorDrag  used to do,
with two major differences. It uses the slider number it is given as the first parameter,
not the global slideChoice  variable. Also, it takes the percentage as its second param‐
eter. These are the sort of changes you have to make when you decide to modularize
simple code that was written for an ad hoc example:
function  svgSetShirtColor (which, percent) {
  var obj;
  var colorStr ;
  var newText;
  if (percent < 0) { percent = 0; } 
  if (percent > 100) { percent = 100; }
  obj = document .getElementById ("slide" + which); 
  obj.setAttribute ("y1", 100 - percent);
  obj.setAttribute ("y2", 100 - percent);
  rgb[which] = percent;
  colorStr  = "rgb(" + rgb[0] + "%," + 
    rgb[1] + "%," + rgb[2] + "%)";
  obj = document .getElementById ("shirt");
  obj.style.setProperty ("fill", colorStr , null);
}
Y ou still have to check that the percent value is within the correct range.
The slider line is moved, to the new position.
The color-changing code is the same.
Now, use the reserved word parent  in the init  function to connect the SVG document’s
svgSetShirtColor  function to the HTML page’s setShirtColor  reference. This works
because when one document is embedded in another, the parent  global variable in the
child document will be a reference to the other document’s window  object. As
setShirtColor  will be a property of the window in which the web page is running, the
HTML document will be able to access it. The following code accomplishes the HTML
to SVG communication. Before using the parent  variable, it tests to confirm that the
parent object exists (meaning that the SVG is actually embedded in another document):
function  init( evt ) {
  // add event listeners
  if (parent) {
        parent.setShirtColor  = svgSetShirtColor ;
   }
Scripting SVG | 227

  transformShirt ();
}
The last step is to communicate from SVG back to HTML if the user decides to choose
colors with the slider. Rather than continuously update the HTML fields, we made the
design decision to update the HTML when the mouse drag stops. Add the boldface code
to function endColorDrag . If a slider was being moved, this sends the slider number
and its value back to the updateHTMLField function in the parent web browser window
(if it exists):
function  endColorDrag ( ) {
  if (slideChoice  >= 0) {
    if (parent)
      parent.updateHTMLField (slideChoice , rgb[slideChoice ]);
  }
  // In any case, nobody's being dragged now
  slideChoice  = -1;
}
The result is shown in Figure 13-5 ; the screenshot has been edited to eliminate unnec‐
essary whitespace.
Figure 13-5. Screenshot of HTML and SVG interaction
Try it out yourself online:
http://oreillymedia.github.io/svg-essentials-examples/ch13/shirt_interact.html
228 | Chapter 13: Adding Interactivity

Creating New Elements
In addition to modifying the attributes of existing elements, a script can also create new
elements.  The next addition we will make to the shirt example is the ability to add rings
to the shirt in a bull’s-eye pattern. Figure 13-6  shows the result, which we’re sure will
soon be a hugely popular fashion.
Figure 13-6. Screenshots of different selections
Y ou can create your own designs with the online example:
http://oreillymedia.github.io/svg-essentials-examples/ch13/shirt_create.html
The HTML has to be modified to add a new form field for specifying the number of
rings:
<label>Rings: <input id="nRings"  type="text" size="3" value="0"
  onchange= "createRings(this.value)"  /></label>
The SVG lives in an external document, and the script in the HTML file needs a way to
access it. In the previous section, the script in the SVG file used parent  to access the
environment where the HTML scripts were running. In this example, the HTML script
will use the <object>  element’s getSVGDocument()  method to directly access—and then
modify—the SVG DOM. The SVG file is the same as in the previous section.
To establish the connection with the SVG document, an initialization function is called
on the web page’s <body>  when the entire page has loaded:
<body onload="init()" >
The init()  function accesses the SVG document and stores it in a global variable:
Scripting SVG | 229

var svgDoc;
function  init() {
  var obj = document .getElementById ("shirt");
  svgDoc = obj.getSVGDocument ();
}
With the stage set, the function createRings()  in the HTML page’s script can add and
remove elements in the SVG document, as shown in Example 13-8 .
Example 13-8. JavaScript code to create elements
function  createRings (nRings) {
  var shirt = svgDoc.getElementById ("shirt"); 
  var rings = shirt.getElementsByTagName ("circle" ); 
  var i;
  var radius;
  var circle;
  for (i = rings.length - 1; i >= 0; i--) { 
    shirt.removeChild (rings[i]);
  }
  /* Pin the range to 0-5 */
  if (nRings < 0) { nRings = 0; }
  else if (nRings > 5) { nRings = 5; }
  radius = nRings * 4;
  for (i = 0; i < nRings * 2; i++) {
    circle = svgDoc.createElementNS ("http://www.w3.org/2000/svg" ,  
      "circle" );
    circle.setAttribute ("cx", "0");
    circle.setAttribute ("cy", "0");
    circle.setAttribute ("r", radius);
    if (i % 2 == 0) { 
        circle.style.cssText = "fill:black; stroke:none" ;
    }
    else {
        circle.style.cssText = "fill:white; stroke:none;" ;
    }
    shirt.appendChild (circle); 
    radius -= 2;
  }
}
Get the <g> from the SVG document
Retrieve all the <circle>  elements in that group.
Remove all the rings (in reverse order) by calling removeChild( nodeToRemove )
on the parent <g> stored in the shirt  variable.
230 | Chapter 13: Adding Interactivity

The element  that is being  created  is part of an SVG  document,  so you must  create
an element in the namespace ( NS) for SVG. Note that you define the namespace
using the namespace URL string, not a prefix.
The assigned  style  alternates  for even  and odd numbered  rings.  Because  we’re
setting multiple styles on a new, unstyled element, we set them as a block using
element.style.cssText .
Finally,  append  the newly  created  <circle>  element  and reduce  the radius  in
preparation  for the next  circle  by calling  addChild( newNode) on the shirt
group. The new nodes will be inserted in the DOM in the order they are added,
largest  to smallest,  so the smaller  circles  will be drawn  on top of the
larger ones.
Scripting SVG | 231