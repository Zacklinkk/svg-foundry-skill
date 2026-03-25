CHAPTER 14
Using the SVG DOM
The methods and properties used in the scripting examples in Chapter 13 —creating
and selecting elements, getting and setting styles and attributes—are part of  the Docu‐
ment Object Model (DOM) specification, and are not unique to SVG. The SVG 1.1
specifications define numerous additional methods to make it easier to work with the
two-dimensional graphical layout of SVG. These methods allow you to figure out exactly
where your text or path elements have been drawn, control the timing of your animation
elements, and convert between transformed coordinate systems.
Determining the Value of Element Attributes
In “Scripting SVG” on page 213, we used getAttribute  and setAttribute  to access
element attributes. Those methods treat the attributes as simple strings, without wor‐
rying about what they represent. If you need to calculate the difference in width between
an element with width="10em"  and an element with width="100px" , you would need
to extract the numbers from the strings, find out the font size, and convert the meas‐
urements.
To make things easier, SVG element objects have properties  representing the key at‐
tributes for that element type. An SVGCircleElement  object,  which represents a
<circle>  in your SVG code, has properties cx, cy, and r. An SVGRectElement  (a <rect>
in markup) has properties x, y, width , and height .
These properties don’t store simple numbers, however. In most cases, the properties
each contain two subproperties: baseVal  and animVal . The animVal  object is read-only,
and is updated as an object is animated, so it always gives the current displayed state of
the attribute.
Equally important, the baseVal  and animVal  subproperties are themselves complex data
objects designed to make it easy to deal with attributes specified in different units. For
233

lengths and angles, baseVal.value  and animVal.value  always contain the value of the
property in user units (degrees for angles), regardless of what unit was used to set the
attribute. The baseVal  and animVal  objects also have methods to convert between units.
Example 14-1  shows different ways of accessing information about the x- and y-radii
of an animated ellipse. The output is shown in Figure 14-1  (the exact output will depend
on the default font size of your system).
Figure 14-1. Screenshot of animation with baseVal and animVal
Example 14-1. Using the baseVal and animVal properties
http://oreillymedia.github.io/svg-essentials-examples/ch14/baseval_animval.svg
Animation markup
<ellipse  id="el" cx="50%" cy="20" rx="40%" ry="1em">
    <animate  id="animation"  attributeName= "rx" to="20%"
             begin="indefinite"  dur="2s" fill="freeze" />
</ellipse>
Markup for text output
<text y="3em">
    <tspan x="1em" dy="1.5em">getAttribute("rx"): </tspan>
        <tspan x="50%" id="getRx"/>
    <tspan x="1em" dy="1.5em">getAttribute("ry"): </tspan>
        <tspan x="50%" id="getRy"/>
    <tspan x="1em" dy="1.5em">rx.baseVal.value: </tspan>
        <tspan x="50%" id="rxBase" />
    <tspan x="1em" dy="1.5em">ry.baseVal.value: </tspan>
        <tspan x="50%" id="ryBase" />
    <tspan x="1em" dy="1.5em">rx.baseVal.valueAsString: </tspan>
        <tspan x="50%" id="rxBaseString" />
234 | Chapter 14: Using the SVG DOM

    <tspan x="1em" dy="1.5em">ry.baseVal.valueInSpecifiedUnits: </tspan>
        <tspan x="50%" id="ryBaseUnits" />
    <tspan style="font-weight:bold;"  x="1em" dy="2.5em">After
        rx.baseVal.convertToSpecifiedUnits(): </tspan>
    <tspan x="1em" dy="1.5em">rx.baseVal.valueAsString: </tspan>
        <tspan x="50%" id="rxBaseUnits" />
    <tspan style="font-weight:bold;"  x="1em" dy="2.5em">After
        approx. 1 second of animation: </tspan>
    <tspan x="1em" dy="1.5em">rx.animVal.value: </tspan>
        <tspan x="50%" id="rxAnim" />
    <tspan x="1em" dy="1.5em">ry.animVal.value: </tspan>
        <tspan x="50%" id="ryAnim" />
</text>
Script
var doc = document ;
var el = doc.getElementById ("el"); 
doc.getElementById ("getRx").textContent  = el.getAttribute ("rx"); 
doc.getElementById ("getRy").textContent  = el.getAttribute ("ry");
doc.getElementById ("rxBase" ).textContent  = el.rx.baseVal.value; 
doc.getElementById ("ryBase" ).textContent  = el.ry.baseVal.value;
doc.getElementById ("rxBaseString" ).textContent  =
 el.rx.baseVal.valueAsString ; 
doc.getElementById ("ryBaseUnits" ).textContent  =
 el.ry.baseVal.valueInSpecifiedUnits ; 
el.rx.baseVal.convertToSpecifiedUnits (SVGLength .SVG_LENGTHTYPE_EMS ); 
doc.getElementById ("rxBaseUnits" ).textContent  =
  el.rx.baseVal.valueAsString ;
var animate = doc.getElementById ("animation" ); 
try {
  animate.beginElement (); //start the animation
} catch(e){/* catch exception if animation not supported */ }
setTimeout (getAnimatedValue , 1000); 
function  getAnimatedValue () { 
  try {
    animate.endElement (); //freeze the animation
  } catch(e){}
  doc.getElementById ("rxAnim" ).textContent  = el.rx.animVal.value;
  doc.getElementById ("ryAnim" ).textContent  = el.ry.animVal.value;
}
el is the "SVGEllipseElement  object” representing the ellipse in the markup.
el.getAttribute("rx")  returns the string value of the attribute with the units
used in the markup.
el.rx.baseVal.value  is the rx value converted to a number in user units.
Determining the Value of Element Attributes | 235

el.rx.baseVal.valueAsString  is the complete string, with units.
el.ry.baseVal.valueInSpecifiedUnits  is the value  of the attribute  as a
number, but in the units used when the attribute was set.
The convertToSpecifiedUnits( unitConstant ) method  can be used  to convert
the value to any unit you choose (in this case, ems). The method doesn’t return
anything  directly,  but the data  object’s  valueAsString  and
valueInSpecifiedUnits  properties  will be changed.  The value  property  (which
is always in user units) isn’t affected.
The markup defines an animation element that modifies the rx attribute of the
ellipse,  but which  has begin="indefinite" , preventing  it from  starting
automatically.  Instead,  we start  it by calling  the beginElement()  method.  The
method  is put in a try/catch  block  to avoid  errors  in browsers  that don’t  support
animation.
The setTimeout()  function call tells the computer to wait 1 second (1000ms),
and then run the getAnimatedValue  function.
getAnimatedValue()  halts  the animation  with  animate.endElement() , then
queries  the animVal  properties  for each  attribute.  Although  ry is not being
animated, ry.animVal  still exists; it is exactly equal to ry.baseVal .
The rx and ry properties used in Example 14-1  are both instances of
SVGAnimatedLength  objects.  To describe different types of geometrical data, SVG de‐
fines a variety of custom objects. Table 14-1  lists some of the most important objects
and what you can do with them.
236 | Chapter 14: Using the SVG DOM

Table 14-1. SVG Data Objects
Object Name Description Properties and Methods
SVGLength A length with units. The units are defined as constants of
the form SVGLength.SVG_LENGTHTYPE _unit ,
where unit  is one of NUMBER  (for user units),
PERCENTAGE , EMS  (em  units),  EXS  (ex units),  PX, CM,
MM, IN, PT, PC.Properties:
unitType
one of the constants defining allowed units;
value
the length in user units;
valueInSpecifiedUnits
the length in unitType  units;
valueAsString
the value and units together as a string.
Methods:
newValueSpecifiedUnits(  unitType , valueInSpecifiedUnits )
sets the value and the units;
convertToSpecifiedUnits(  unitType  )
change the unit type, while maintaining the same value in user units.
SVGAngle An angle with units. The unit constants are of the form
SVGAngle.SVG_ANGLETYPE _unit , where unit  is
one of UNSPECIFIED  (degrees by default) DEG , RAD ,
or GRAD .Same as for SVGLength .
SVGRect A rectanglular area in user coordinates. The SVGRect
object is not the same as a <rect>  element (which is
represented by the SVGRectElement  interface).Properties:
x, y, width  and height
all numbers in user coordinates.
Determining the Value of Element Attributes | 237

Object Name Description Properties and Methods
SVGPoint A point in user space. Properties:
x, y
both numbers in user coordinates.
Method:
matrixTransform(matrix)
returns the value of the point transformed by the given matrix, which must be an
SVGMatrix  object.
SVGMatrix A matrix in the form used for transformations, as
described in Appendix D .Properties:
a,b,c,d,e,f
all numbers, representing the variable values of the matrix in top-to-bottom, left-to-right
order.
Methods:
multiply( secondMatrix )
returns a matrix equal to the two matrixes multiplied together;
inverse()
creates the invert matrix if possible, or throws an exception if the matrix is not invertable
(for example if it represents a scale(0)  operation);
translate( x, y), scale(scaleFactor ), scaleNonUniform( scaleFactorX ,
scaleFactorY ), rotate(angle), rotateFromVector( x, y), flipX() , fli
pY() , skewX(angle), skewY(angle)
returns a new matrix that is this matrix multiplied by the specified transformation;
rotateFromVector  calculates the angle required to rotate the x-axis to align with that
vector; flipX  and flipY  are equivalent to scaling by an x or y factor of -1.
238 | Chapter 14: Using the SVG DOM

Object Name Description Properties and Methods
SVGTransform A single  transformation  command.  The type  of command
is defined as a constant of the form
SVG_TRANSFORM _type , where type  is one of
MATRIX , TRANSLATE , SCALE , ROTATE , SKEWX  or
SKEWY .Properties:
type
one of the type constants;
matrix
the SVGMatrix  object representing this transformation;
angle
the angle of rotation or skew in degrees, if applicable, or zero.
Methods:
setMatrix( matrix), setTranslate( x, y), setScale( scaleFactorX , scale
FactorY), setRotate( angle, cx, cy), setSkewX( angle), setSkewY( angle)
change this SVGTransform  object to the specified transformation.
SVGTransformList A list of transformations ( SVGTransform  objects), in
the order they would be specified in a transform
attribute. In addition to the specific methods for
transform lists, these objects also implement the
numberOfItems  property and all the methods
described below for generic lists.Methods:
createSVGTransformFromMatrix( matrix)
create a new SVGTransform  object from an SVGMatrix  object;
consolidate()
combine all the transformations in this list into a single matrix transformation, and then
return that SVGTransform  object.
Determining the Value of Element Attributes | 239

Object Name Description Properties and Methods
SVGXxxList For any data type that might be used in a list or an array
(and most animated data types), there is a
SVGDatatype List . Most Javascript
implementations  of the SVG  DOM  use arrays  to represent
these lists, so you may also be able to use array notation
to set or retrieve items.Property:
numberOfItems
the length of the list.
Methods:
clear()
removes all elements from the list;
initialize( newItem)
clears all existing items from the list, so that it only contains the one new item;
getItem( index)
accesses the list item at index  (the first index is 0);
insertItemBefore( newItem, index), replaceItem( newItem, index), re
moveItem( index), appendItem( newItem)
self-explanatory methods for modifying the list.
SVGAnimated Xxx For nearly every data type, there is an
SVGAnimated Datatype  interface, which is used as
the value of a property representing an animatable
attribute. For example, the cx property of an
SVGCircleElement  object is an
SVGAnimatedLength  object, while a transform
property is of type SVGAnimatedTransformList .Properties:
baseVal
an object containing the official value of the attribute or property;
animVal
a read-only object containing the current displayed value of the attribute or property, after
adjusting for animation.
240 | Chapter 14: Using the SVG DOM

SVG Interface Methods
Sometimes when scripting SVG, you want to calculate a geometric property that isn’t
defined directly by an attribute. For example, you might want to draw a rectangle around
a text label that will fit neatly, regardless of what font is used for the text. Or you might
want to find out where you are in the timing cycle of an animation at the time the
JavaScript code is run. Other times, you might want to manipulate part of a complex
attribute: one curve segment within a path, or one command within a transform
attribute.
There are a variety of useful properties and methods that can be called on element objects
—the objects returned by document.getElementById(id) —to perform these sorts of
calculations and manipulations. The specifications define these in terms of interfaces,
descriptions of common functionality that are implemented by specific object types.
Table 14-2  lists some of the interface features available for the elements described in this
book. The list is not exhaustive, but it should get you started.
SVG Interface Methods | 241

Table 14-2. Interfaces for SVG elements
Applies to Method or property Result
SVGElement
(any element in the SVG
namespace).ownerSVGElement() Returns a reference to the nearest ancestor <svg>  in the hierarchy; returns null if called on a top-
level SVG element.
.viewportElement() Returns a reference to the <svg> , <pattern> , <symbol> , or <marker>  element that
establishes the viewport for this element.
SVGLocatable
(any element that takes up
coordinate space or takes a
transform attribute: graphics
elements, <g>  or <svg> ).nearestViewportElement The <svg> , <pattern> , <symbol> , or <marker>  element that establishes the viewport for
this element.
.farthestViewportElement The top-level <svg>  element that contains this element.
.getBBox() Returns  the object  bounding  box as an SVGRect  object  with  properties  x, y, width , and  height ,
representing  the coordinates  and  extent  of the smallest  rectangle  in the current  user  coordinate  system
that can contain the graphic. The bounding box is not affected by stroke width, clipping, masking, or
filter effects.
 .getCTM() Returns a cumulative transformation matrix, an SVGMatrix  object representing the net
transformation from the coordinate system of this element to the coordinate system of the
nearestViewportElement .
 .getScreenCTM() Returns  a cumulative  transformation  matrix  to convert  from  the user  coordinate  system  of this element
to the “screen”  or client  coordinates  used  to represent  points  on the root-level  document.  This  method
is useful in event handlers when converting between mouse/pointer coordinates and the coordinate
system of your graphics.
 .getTransformToElement(
SVGElement )Returns an SVGMatrix  representing the net transformations required to convert between the
coordinate system for this element and the coordinate system for the other element.
SVGTransformable
(any element that can take a
transform  attribute).transform The SVGAnimatedTransformList  representing the base and animated values of the
transformations defined on this element.
242 | Chapter 14: Using the SVG DOM

Applies to Method or property Result
SVGStylable
(any element that can take a
style  attribute).style A CSSStyleDeclaration  object  representing  any inline  styles  set on this element.  See “Scripting
SVG” on page 213  in Chapter 13  for methods of the style  object.
SVGSVGElement
(<svg> ).suspendRedraw(
maxWaitTimeInMilliseconds )Tells the browser to hold off on redrawing the graphic for the specified wait time (max one minute).
Useful if you are going to be making a lot of changes and you want them all to apply at once. The
method returns an ID number which you can then pass to unsuspendRedraw  function.
.unsuspendRedraw( suspendID ) Cancels the specific suspendRedraw  call associated with the ID value.
.unsuspendRedrawAll() Cancels all suspendRedraw  calls on the SVG, causing the graphic to refresh again.
 .pauseAnimations() Pauses the time clock for all SMIL animations within the SVG.
 .unpauseAnimations() Resumes the time clock for all SMIL animations within the SVG.
 .animationsPaused() Returns  true  or false,  depending  on whether  animations  have  been  paused  using  the above  methods.
 .getCurrentTime() Returns  the value  of the time  clock  used  for SMIL  animations.  Normally,  this is the number  of seconds
since  the document  was  loaded,  but it can be affected  by pausing  or unpausing  animations  or setting
the time directly.
 .setCurrentTime( timeInSeconds
)Sets the SMIL time clock to the specified value, affecting all animations
 .getIntersectionList(
rectangle , referenceElement )Returns  a list of elements  that  (a) overlap  the specified  rectangle  (an SVGRect  object)  based  on the
current  <svg>  element’s  coordinate  system,  and  that  (b) are children  of the reference  element.  Only
parts  of the element  that  are sensitive  to pointer  events  (based  on the pointer-events  property,
by default  any visible  painted  areas)  are considered  when  determining  overlap.  The reference  element
can be null, in which case all children of this <svg>  are included.a
 .getEnclosureList(  rectangle ,
referenceElement )Similar to getIntersectionList() , except that it only returns elements that are entirely
within the rectangle
SVG Interface Methods | 243

Applies to Method or property Result
 .checkIntersection( element,
rectangle )Returns true  or false , depending on whether the given element intersects the given SVGRect
object, in this <svg>  element’s coordinate system.
 .checkEnclosure( element,
rectangle )Returns true  or false , depending on whether the given element is completely contained in the
given SVGRect  object, in this <svg>  element’s coordinate system.
 .createSVG Xxx() The SVGSVGElement  supports methods to create new instances of each of the data object types
(SVGPoint , SVGAngle , SVGMatrix , etc.) from Table 14-1 . The methods don’t take any
parameters; the resulting objects are initialized with their properties set to 0 (except for
createSVGMatrix() , which returns an identity matrix).
SVGUseElement
(<use> ).instanceRoot Contains the top-level node of a shadow DOM tree representing the graphics drawn by the <use>
element. The elements in the shadow DOM ( SVGElementInstance  objects) have limited
function: you cannot manipulate attributes or styles directly, but they can be targets of user events.
Each SVGElementInstance  has a correspondingElement  attribute that links to the
source graphics element that it duplicates, and a correspondingUseElement  property that
links back to the <use>  element.b
SVGPathElement
(<path> ).getTotalLength() Returns  the computed  length,  in user  units,  of the entire  path  (not  counting  any move-to  commands).
This  value  may  not be exactly  the same  in every  agent  because  approximations  may  be used  for some
curves.
.getPointAtLength(distance) Returns an SVGPoint  object with properties x and y, representing the coordinates of the point on
the path that is distance  user units from the start, using the same method of calculating path
length as for getTotalLength() .
244 | Chapter 14: Using the SVG DOM

Applies to Method or property Result
SVGPathData
(<path>  elements and any other
elements that support a path data
attribute, such as
<animateMotion>  elements).pathSegList Contains a list of object representing each segment of the path. The list can then be modified or
queried in an object-oriented manner; see the SVG specifications  for methods. The pathSegList
property returns the path corresponding to the actual d attribute value; use the
animatedPathSegList  property to access the current state of the path in case the d attribute
is being animated.
.normalizedPathSegList A simplified version of the path segment list, where each segment has been converted to either a
move, line-to, cubic curve, or close path command in absolute coordinates. There is also an
animatedNormalizedPathSegList  property.
SVGAnimatedPoints
(<polygon>  and <polyline>
elements).points Returns the points associated with this element as an SVGPointList . The property
animatedPoints  is similar but is updated when the points attribute is animated.
SVGTextContentElement
(any text element, including
<text> , <tspan> , and
<textPath> ).getNumberOfChars() Returns  the total  number  of characters  in this element,  including  text  in all child  <tspan>  elements.
Multibyte unicode characters are counted according to the number of UTF-16 characters required to
represent them.
.getComputedTextLength() Returns the length, in user coordinate units, required to write out the text after applying all CSS
properties and dx and dy attributes. Does not include any adjustments made based on a
textLength  attribute.
.getSubStringLength( charNum,
nChars)Returns the computed text length for a substring of the text, starting with the character that is
charNum  number of characters into the text (the first character is numbered 0), and continuing for
nChars  or until the end of the text, whichever comes first.
 .getStartPositionOfChar(
charNum)Returns an SVGPoint  object with properties x and y, representing the position of the specified
character within user coordinate space. How the character is drawn relative to this point depends on
the writing mode (vertical or horizontal, left-to-right or right-to-left) and the baseline-alignment
property. For default properties on left-to-right text, the point will be where the left edge of the
character intercepts the baseline.
SVG Interface Methods | 245

Applies to Method or property Result
 .getEndPositionOfChar( charNum
)Similar to getStartPositionOfChar  but returns the point where the baseline meets the end
edge of the character.
 .getExtentOfChar( charNum) Returns an SVGRect  object, similar to getBBox() , except that it only represents the boundaries
for a single character.
 .getRotationOfChar( charNum) Returns  the rotation  value  (angle  in degrees)  for the specified  character,  after  taking  into  consideration
any rotate  attribute and any rotation from text on a path, but not including any transformation
of the coordinate system.
 .getCharNumAtPosition( point) Returns the index number for the character that includes the specified point, or returns -1 if no
characters in the string contain that point. The point is specified as an SVGPoint  object, either one
returned from another interface method, or one created by calling createSVGPoint()  on an
<svg>  element and then setting the returned object’s x and y properties.
ElementTimeControl  and
SVGAnimationElement
(any of the SVG animation
elements: <animate> , <set> ,
<animateTransform> , and
<animateMotion> )c.targetElement The SVGElement  that this animation element modifies.
.beginElement() Start the specified animation immediately, if it isn’t prevented by a restart  attribute of never
or whenNotActive .
.beginElementAt( offset) Start the specified animation after offset  number of seconds; if offset is negative, the animation will
start immediately but be calculated as if it had started that many seconds previously.
 .endElement() End the current run of the animation (including all repeats) immediately.
 .endElementAt( offset) End the animation after offset  number of seconds.
 .getStartTime() If the animation is running, returns the start time at which it began, in seconds relative to the time
clock  for the SVG.  If the animation  is waiting  to begin,  returns  the time  at which  it will begin.  Otherwise,
throws an exception.
 .getCurrentTime() Returns the time in seconds of the animation time clock for the SVG that this element is part of, the
same as calling getCurrentTime()  on that SVG.
246 | Chapter 14: Using the SVG DOM

Applies to Method or property Result
 .getSimpleDuration() Returns the number of seconds for the duration of each cycle of the animation (the value of the dur
attribute); throws an exception if the duration is undefined.
a At the time of writing, Firefox (version 30) does not support the methods getIntersectionList , getEnclosureList , checkIntersection , or
checkEnclosure .
b Firefox (as of version 30) also does not make the shadow DOM tree of a <use>  element accessible to scripts, and does not implement the SVGElementInstance  interface.
c None of the animation-related properties and methods are implemented in Internet Explorer (version 11), which does not support SMIL animation. The Apache Batik SVG viewer,
version 1.7, throws errors when using beginElement  and beginElementAt .
SVG Interface Methods | 247

Constructing SVG with ECMAScript/JavaScript
The next example is a simple analog clock, as shown in Figure 14-2 ; Example 14-2  gives
the SVG code.
Figure 14-2. Analog clock
Example 14-2. SVG for a basic analog clock
<svg xmlns="http://www.w3.org/2000/svg"
    id="clock" width="250" height="250" viewBox= "0 0 250 250" >
<title>SVG Analog Clock </title>
<circle id="face" cx="125" cy="125" r="100"
        style="fill: white; stroke: black" />
<g id="ticks" transform= "translate(125,125)" >
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(30)"   />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(60)"   />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(90)"   />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(120)"  />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(150)"  />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(180)"  />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(210)"  />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(240)"  />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(270)"  />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(300)"  />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(330)"  />
    <path d="M95,0 L100,-5 L100,5 Z"  transform= "rotate(360)"  />
</g>
<g id="hands" style="stroke: black;
                     stroke-width: 5px;
                     stroke-linecap: round;" >
  <path id="hour" d="M125,125 L125,75"
        transform= "rotate(0, 125, 125)" />
  <path id="minute"  d="M125,125 L125,45"
        transform= "rotate(0, 125, 125)" />
  <path id="second"  d="M125,125 L125,30"
248 | Chapter 14: Using the SVG DOM

        transform= "rotate(0, 125, 125)"
        style="stroke: red; stroke-width: 2px"  />
</g>
<circle id="knob" r="6" cx="125" cy="125" style="fill: #333;" />
</svg>
The code isn’t particularly complicated, but it is rather redundant. Each of the 12 hour-
marker elements has nearly the exact same syntax, just a different rotation  attribute.
Y ou could use <use>  elements to avoid repeating the path data, but it really wouldn’t
simplify things very much. If you wanted to add numbers for the hours, you’d have 12
<text>  elements, too. And if you wanted to add minute markers, you’d need 60 separate
elements for the marks.
In programming, whenever you’re doing the same thing many times, you really ought
to be using a loop or function to do it. In “Creating New Elements” on page 229 in
Chapter 13 , JavaScript was used to create an arbitrary number of SVG elements based
on user input. The same methods can also be used to create the initial graphic, and they
can be especially helpful if your graphic is repetitive and geometrical.
Example 14-3  creates the same output as the previous one in an SVG viewer that sup‐
ports JavaScript. It uses both the basic DOM methods and the SVG DOM features
introduced in this chapter. The <svg>  contains only two markup elements: a <title>
and a <script> .
Example 14-3. Basic analog clock created with ECMAScript
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg"
    id="clock" width="250" height="250" viewBox= "0 0 250 250"
     onload="init()"  >
<title>Scripted Analog Clock </title>
<script type="application/ecmascript" > <![CDATA[
  /* the <svg> object that will contain the drawing */
  var clock; 
  function  init() { 
    /* select the empty <svg> */
    clock = document .getElementById ("clock");
    var svgns = clock.namespaceURI ,
        doc   = document ;
    clock.suspendRedraw (1000); 
    /* create the clock face */  
    var face = doc.createElementNS (svgns, "circle" );
    face.cx.baseVal.value = 125;
    face.cy.baseVal.value = 125;
Constructing SVG with ECMAScript/JavaScript | 249

    face.r.baseVal.value = 100;
    face.style.cssText = "fill: white; stroke: black" ;
    clock.appendChild ( face );
    /* create a group for the ticks */
    var ticks = clock.appendChild (
          doc.createElementNS (svgns, "g") );
    ticks.setAttribute ("transform" , "translate(125,125)"  );
    /* create the tick marks */
    var tickMark ;
    for (var i = 1; i <= 12; i++) { 
      tickMark  = doc.createElementNS (svgns, "path");
      tickMark .setAttribute ( "d",
                              "M95,0 L100,-5 L100,5 Z"  );
      tickMark .setAttribute ( "transform" ,
                              "rotate("  + (30*i) + ")" );
      ticks.appendChild ( tickMark  );
    }
    /* create the hands */
    var hands = clock.appendChild (
                    doc.createElementNS (svgns, "g") );
    hands.style.cssText =
      "stroke: black; stroke-width:5px; stroke-linecap: round;" ;
    var hourHand  = hands.appendChild (
                    doc.createElementNS (svgns, "path") );
    hourHand .id = "hour";
    hourHand .setAttribute ("d", "M125,125 L125,75" ); 
    hourHand .setAttribute ("transform" , "rotate(0, 125, 125)" );
    /* similar code for minute and second hands
      omitted to save space */
    /* add the center knob */  
    var knob = doc.createElementNS (svgns, "circle" );
    knob.setAttribute ("cx", "125");
    knob.setAttribute ("cy", "125");
    knob.setAttribute ("r", "6");
    knob.style.setProperty ("fill", "#333", null);
    clock.appendChild ( knob );
    clock.unsuspendRedrawAll (); 
  }
  // ]]>
</script>
</svg>
250 | Chapter 14: Using the SVG DOM

The script declares a global variable to hold the <svg>  element; the bulk of the
code,  however,  is in an initialization  function  (init() ), which  will run when  the
SVG loads.
The init()  function  starts  by selecting  the <svg>  element.  It also declares
convenience  variables;  accessing  the .namespaceURI  property  on any existing
SVG  element  is a good  way to avoid  retyping  the URL  each  time  you create  a
new element.
Although  not required,  suspending  drawing  updates  on your  SVG  before
making a lot of DOM changes can improve performance in some viewers.
The clock  face <circle>  is created,  styles  and attributes  are set using  SVG  DOM
properties, and then it is added to the SVG.
Here’s  the magic  of looping.  The for loop  runs  the same  code  12 times  to create
each  hour  marker,  and calculates  the rotation  angle  for each.  Note  that for
initializing  complex  attributes  like d or transform , passing  a string  to
setAttribute()  is generally  easier  than  manipulating  the complex  DOM
objects that represent the values.
The clock  hands  are initialized  pointing  to midnight,  with  zero-rotation
transformation attributes to make it easier to later change the rotation.
For comparison  with  the face circle,  the knob  <circle>  is initialized  using  setter
methods instead of DOM properties. It still requires six lines of code.
Don’t forget to cancel any suspendRedraw()  calls when you’re finished building
the DOM!
Using scripting shortened the code required to make the tick-marks, but it considerably
increased the amount of code required to draw simple elements like the circles for the
clock face and knob. This is the main reason why JavaScript libraries like Snap.svg are
so popular; they have shortcut functions for the most common operations, like creating
elements and setting attributes.
We’ll get to that in a moment, but until we get to a discussion of libraries (the easy way
to do it), we’ll continue to draw the clock face using plain SVG. After all, there’s a more
pressing issue: our clock doesn’t tell time.
Animation via Scripting
One option to get the clock ticking is to use <animateTransform>  elements on the
hands.  Y ou can set the second hand to rotate 360 degrees every minute, the minute hand
Animation via Scripting | 251

1.The SMIL specifications do define a format for synchronizing the start time of animations with the system
clock, but it isn’t implemented in most browsers or SVG viewers.to rotate 360 degrees every hour, and the hour hand to rotate 360 degrees every 12 hours.
What <animateTransform>  cannot do is get the hands to show the correct time.1
There are other things animation elements can’t do easily. They can’t keep track of past
user events and change the way they respond to new events accordingly. If your ani‐
mation relies on logic, data, or complex user interactions, it makes more sense to control
it from a script (with the other application functionality) than to try to define it in the
XML (the document structure).
To create an animation with JavaScript, you repeatedly modify the attribute or style
property to change it from the starting value to the ending value. If you’re just starting
out in programming, you might think that you could create a loop to continually update
the attributes by wrapping the update code in a while(true)  block.
This would ensure your clock is always up-to-date, but we wouldn’t recommend it. The
while(true)  technique is akin to constantly looking at your watch and asking, “Is it
time yet?” It gives you no time to sleep or do anything else. In an SVG script, it gives
the computer no chance to attend to other tasks.
It takes your computer only a tiny fraction of a millisecond to run through a simple loop
like the one in the preceding script; each increment of the clock hands will be essentially
meaningless. In comparison, most film and video consists of images that are updated
30–60 times per second. That speed is called the frame rate  of the video, and it is sufficient
to convince your eyes that you’re watching smooth motion.
Computer displays have a frame rate,  too. As content changes, part or all of the display
will be updated to match. However, the effective frame rate of the computer display
depends on how much other work the computer is doing in the background. If you bog
down the computer with endless loops, it won’t have time to repaint the screen, and
your animation will become slow and choppy, regardless of how often you update the
attributes.
To persuade your computer to create smooth animations, you have to be polite. By
calling the method requestAnimationFrame( animationfunction ), you are effectively
saying, “Computer, next time you’re ready to repaint the screen, please run this function
first. ” The function will be passed a timestamp value, which will be the same for all
functions being called for a given frame, allowing you to coordinate multiple animation
functions. (The timestamp is based on the document time clock, not the system clock,
so we won’t be using it to set the time.)
If the last line of your animation function also calls requestAnimationFrame  and passes
itself as the function to run, then it will be called as often as the computer can draw the
material to the screen, allowing you to create a smooth animation without exhausting
252 | Chapter 14: Using the SVG DOM

2.Y ou may have heard of a programming technique called recursion , which happens when a function calls itself.
This is not recursion, because your function calls requestAnimationFrame() , not itself.your computer’s resources.2 Importantly, if the window that contains this script is
currently  minimized or hidden, then the function won’t be called at all until there is
something visible to animate.
Mimicking requestAnimationFrame with setTimeout
The requestAnimationFrame()  method is a relatively new  addition to the DOM spec‐
ifications, and is not supported in older browsers or Batik. To allow your script to run
smoothly, you can test whether the function exists, and create a substitute method if it
doesn’t, as shown here. The substitute method makes use of the
setTimeout( function ,waitTime ) method, which tells the computer to run your func‐
tion after the specified waitTime  (in milliseconds). This code should go at the very
beginning of your <script> :
if (!window.requestAnimationFrame ) { 
  window.requestAnimationFrame  = function (animationFunction ) { 
    function  wrapperFunction () { 
      animationFunction (Date.now());
    }
    setTimeout (wrapperFunction , 30); 
  }
}
If a requestAnimationFrame  method doesn’t exist…
Create  your  own  function  and save it as the requestAnimationFrame  property
of the global  object.  Y our  new function  must  accept  an animation  callback
function as a parameter.
Take  the passed-in  animation  function,  and wrap  it in a function  that can be
run without  parameters.  The wrapper  function  calls  the animation  function  with
a timestamp  value  as a parameter.  The timestamp  returned  by Date.now()  is
just the system timestamp as an integer.
The setTimeout  method  calls  the animation  function  when  the computer  is
free, but no sooner than 30 milliseconds, which works out as approximately 33
frames per second.
The setTimeout()  function is not as polite as requestAnimationFrame() , because it
doesn’t adjust for anything else your computer might be doing, and it will run regardless
of whether the animation will be visible. This sample code also does not replace all the
Animation via Scripting | 253

functionality of requestAnimationFrame() ; specifically, it does not synchronize mul‐
tiple animation calls, synchronize with the SMIL animation clock, or provide a way to
cancel an animation frame request. Nonetheless, with a sensible wait time, it should
provide acceptably smooth animation for simple programs. Just insert it at the top of
your script (or in its own script tag at the top of your file).
Example 14-4  presents the code for updating the clock’s hands, the polite way, using
requestAnimationFrame() .
Example 14-4. Animating your scripted SVG clock
Global variables
/* references to the SVGPathElements for the clock hands */
var hourHand ,
    minuteHand ,
    secondHand ;
/* references to SVGTransform object that rotates each hand*/
var secondTransform ,
    minuteTransform ,
    hourTransform ;
/* time conversion constants */
var secPerMinute   = 60,
    secPerHour     = 60*60,
    secPer12Hours  = 60*60*12;
Variable initialization (in the init()  function)
function  init() {
  /*
    Access the SVGPathElements for the clock hands
  */
  hourHand  = document .getElementById ("hour");
  minuteHand  = document .getElementById ("minute" );
  secondHand  = document .getElementById ("second" );
  /* Access the SVGTransform objects that represent
     the current rotate(0, 125, 125) transform on each hand:
  */
  secondTransform  = secondHand .transform .baseVal.getItem(0);
  minuteTransform  = minuteHand .transform .baseVal.getItem(0);
  hourTransform    = hourHand .transform .baseVal.getItem(0);
  updateClock (); /* start the clock going */
}
updateClock  function
function  updateClock () {
    /* get the system time */  
    var date = new Date();
    /* calculate the number of seconds since midnight */
    var time = date.getMilliseconds ()/1000 +
254 | Chapter 14: Using the SVG DOM

               date.getSeconds () +
               date.getMinutes ()*60 +
               date.getHours ()*60*60; 
    /* calculate the rotation angles */  
    var s = 360*( time % secPerMinute  )/secPerMinute ,
        m = 360*( time % secPerHour  )/secPerHour ,
        h = 360*( time % secPer12Hours  )/secPer12Hours ;
    /* use SVGTransform.setRotate(angle, cx, cy)
       to update the rotation angle:
    */
    secondTransform .setRotate ( s, 125, 125); 
    minuteTransform .setRotate ( m, 125, 125);
    hourTransform .setRotate ( h, 125, 125);
    window.requestAnimationFrame ( updateClock  ); 
    // repeat for the next frame
}
The constructor  new Date()  returns  the local  system  date and time  (with
precision of thousandths of a second) as a JavaScript object.
The date.getPart() functions  return  part of the date as an integer.  Using  these
methods,  the time  of day is calculated  as the number  of seconds  since  midnight.
Using  the predefined  constants,  the rotational  position  of each  clock  hand  (in
number of degrees past midnight) is calculated. The % is the modulus  operator,
which returns the remainder part of integer division.
In the initialization  function,  we access  the first (and  only)  item  in the
SVGTransformList  in the baseVal  of each  hand’s  transform  attribute,  and store
that SVGTransform  object in a variable. At each update, we directly change the
rotation  of each  transform.  By modifying  the objects  directly,  instead  of using
setAttribute , we skip the time required for the browser to parse the attribute
string.
The final  line of updateClock  uses  requestAnimationFrame  to schedule  itself  to
be run again the next time the screen refreshes.
The animation cycle is started by the updateClock()  call at the end of the initialization
function. The initialization function is called by adding an onload="init()"  attribute
to the opening <svg>  tag. The final, working clock can be seen on the book’s website:
http://oreillymedia.github.io/svg-essentials-examples/ch14/animated_clock_js.svg
Animation via Scripting | 255

Using JavaScript Libraries
In Example 14-3 , we created  the entire clock by constructing each element “by hand”
with JavaScript calls. As we noted at the time, building elements this way can be painfully
slow. There must be a better way, and, indeed, there is. Y ou can use free, open source
JavaScript libraries such as D3.js , Raphaël , or Snap.svg  to simplify the task. These li‐
braries are all just external script files as far as the web browser is concerned. For you,
however, they are collections of useful functions that you can “borrow” from as needed.
Which library to use depends on your needs. D3.js is “a JavaScript library for manipu‐
lating documents based on data. ” It is best for manipulating sets of similar elements all
at once, defining their attributes, styles, and reaction to events according to a corre‐
sponding value in a data array. If you wanted, for example, a highly interactive bar chart,
D3.js would be an excellent choice.
Raphaël and Snap.svg are more-generic libraries with a goal of making it easy to modify
or create animated graphics. Raphaël is compatible with older browsers, converting your
graphics commands to forms they understand. Snap, created by the Adobe Web Plat‐
form team, is designed for modern browsers and uses SVG exclusively. This example
will use Snap.
All three libraries work by wrapping  the DOM element objects in their own custom
objects, which have additional properties and methods for you to use. In Snap, the <svg>
element that holds your graphic is wrapped up in a Paper  object.  Snap provides function
calls that let you add graphics elements to the paper, modify their attributes, and handle
events that occur in them.
There are numerous JavaScript libraries available designed to work
with  HTML.  Be cautious  about  using  them  for SVG.  It’s not only  that
they  do not have  any methods  for specifically  dealing  with  graph‐
ics; even  basic  tasks  can become  problematic  if the library  is not
aware  of XML  namespaces.  For example,  the popular  JQuery  li‐
brary does not (at the time of writing) have any way of creating new
elements  in the SVG  namespace;  if you ask JQuery  to make  you a
circle,  it will return  an HTMLUnknownElement  object.  In contrast,
SVG-aware  libraries  like D3 and Snap  recognize  the SVG  element
names and will know that circle  means SVGCircleElement .
Here is the XML for a Snap-based version of the animated clock:
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink= "http://www.w3.org/1999/xlink"
     id="clock" width="250" height="250" viewBox= "0 0 250 250"
     onload="init()"  >
256 | Chapter 14: Using the SVG DOM

  <title>Snap.svg Analog Clock </title>
  <script type="application/ecmascript"
          xlink:href= "snap.svg-min.js" ></script>
  <script type="application/ecmascript" >
  /* Initialization and update functions go here */
  </script>
</svg>
The first <script>  element brings in the Snap library. We’re hosting the source code
directly on our server in a minified form, meaning that the code has been processed to
remove whitespace and comments and shorten variable names. The minified version
of Snap.svg v0.3.0 is a 72 KB file, without file compression. In other words, downloading
the Snap library uses up about the same bandwidth for your web page’s visitors as
downloading a moderately complex PNG diagram.
To use an external library in an HTML file, use the attribute src for the file URL, and
be sure to include both an opening and closing tag; empty script elements will not work
in older browsers.
The second script will follow the same structure as the previous examples, with an
init()  function to draw the clock and an updateClock()  to get it ticking.
If you’re trying out these examples yourself, be sure you’re using the
latest  version  of snap.svg-min.js . The library  was initially  designed
to modify  inline  SVG  code  within  an HTML  page,  and versions  prior
to 0.3.0 had bugs when run in standalone SVG files .
Example 14-5  presents the Snap script to draw the clock.  It follows the same structure
(and creates the same graphic) as Example 14-3 , but demonstrates many of Snap’s
shortcut methods.
Example 14-5. Drawing a basic analog clock with Snap.svg
/* the Paper object where the clock is drawn */
var clock;
/* references to the Snap Elements for the clock hands */
var hourHand ,
    minuteHand ,
    secondHand ;  
/* time conversion constants */
var secPerMinute   = 60,
    secPerHour     = 60*60,
    secPer12Hours  = 60*60*12;
function  init() { 
Using JavaScript Libraries | 257

  /* select the empty <svg> as a snap Paper object */
  clock = Snap("#clock" );
  /* create the clock face */  
  var face = clock.circle(125, 125, 100);
  face.attr({fill: "white", stroke: "black"});
  /* create a group for the ticks */
  var ticks = clock.g();
  ticks.transform ("t125,125" ); 
  var tickMark ;
  for (var i = 1; i <= 12; i++) { 
    tickMark  = clock.path("M95,0 L100,-5 L100,5 Z" );
    tickMark .transform ("rotate(" + (30*i) + ")");
    ticks.add(tickMark );
  }
  /* create the hands */
  hourHand  = clock.path("M125,125 L125,75" );
  minuteHand  = clock.path("M125,125 L125,45" );
  secondHand  = clock.path("M125,125 L125,30" );
  var hands = clock.g(hourHand , minuteHand , secondHand );  
  hands.attr({stroke: "black",
              "stroke-width" : 5, 
              "stroke-linecap" : "round"});
  secondHand .attr({stroke: "red", strokeWidth : "2px"});
  /* add the center knob */
  clock.circle(125, 125, 6).attr({fill: "#333"}); 
  updateClock (); 
}
function  updateClock ()
{
  /* adjust the hands */
}
The global variable names are the same, but their content will differ; clock  will
be a Snap Paper  object, and the variables for the hands will point to the Snap-
wrapped elements.
In the init()  function,  the Snap(selector ) method  creates  the Paper  object
containing  the <svg>  identified  by the query  string  (in CSS selector  format).  An
alternative version of the function, Snap(width, height) will create a new SVG
of the specified dimensions.
258 | Chapter 14: Using the SVG DOM

Paper.circle( cx,cy,r) creates  a <circle>  with  the given  center  coordinates
and radius,  and adds  it to the SVG.  The Snap-wrapped  circle  element  is assigned
to a variable  so that we can change  its attributes  on the next  line.  The
Element.attr(attrValues ) function  sets multiple  attributes  at once,  given  in
JavaScript  object  notation.  (We’re  using  presentation  attributes  because,  as of
v0.3.0, Snap doesn’t have a shorthand way to set inline styles.)
An empty  <g> element,  ticks , is created  with  Paper.g() . The transform
attribute could be set with ticks.attr() , but there’s a Snap shorthand method
for this common task. There’s also a shorthand notation for the transformation
commands: "t125,125"  is short for "translate(125,125)" .
The for loop  creates  the hour  marks  with  Paper.path(pathData ); they  are then
moved  into the <g> with  the command  ticks.add(tickMark) . Just to show  it
can be done, the transform attribute is set with standard SVG notation.
The clock  hands  are created,  and then  grouped  together  all in one line:  the
Paper.g(Element, Element, …) both  creates  the group  and moves  the specified
elements into it.
When  using  JavaScript  object  notation  to set attributes  that have  a hyphen  (-)
in the name,  you either  quote  the name  ("stroke-width" ) or convert  it to camel
case form ( strokeWidth ).
Because the Snap methods that create elements also return those elements, you
can chain  multiple method calls together with dot notation.
As before, the last line of the initialization function is a call to updateClock() .
If you load this file into your browser, you will see that it looks exactly the same as
Figure 14-2 . The preceding SVG does what Example 14-3  did, but in a much more
readable fashion.
The only task remaining is to get our Snap-ified SVG clock working again. Example 14-6
gives the code. Instead of animating the motion ourselves with
requestAnimationFrame , we let Snap’s Element.animate()  function handle the move‐
ment. It uses requestAnimationFrame  behind the scenes and, for old browsers, defines
its own substitute method that coordinates multiple animation calls.
Element.animate()  has two required and two optional parameters:
•An attributes object (in the same form as Element.attr() ), giving the final  value
for each attribute in the animation.
•A duration in milliseconds indicating how long the animation should take to reach
the final value.
Using JavaScript Libraries | 259

•Optionally, a function defining the rate of change of the attribute over the course
of the duration (an easing  function). The Snap source code defines a number of
functions you can use as properties of Snap’s mina  object: mina.easeinout  will give
you smooth acceleration and deceleration, while mina.bounce  will hit the final value
quickly, and then bounce back a few times before settling down. For steady motion
throughout the duration, use mina.linear .
•Optionally, a callback function that will run when the animation is complete. This
can be used to cause the animation to repeat indefinitely.
Example 14-6. Animating a clock with Snap.svg
function  updateClock ()
{
    /* get the system time */  
    var date = new Date();
    /* calculate the number of seconds since midnight */
    var time = date.getMilliseconds ()/1000 +
               date.getSeconds () +
               date.getMinutes ()*60 +
               date.getHours ()*60*60;
    /* calculate the rotation angles */
    var s = 360*( time % secPerMinute )/secPerMinute ,
        m = 360*( time % secPerHour  )/secPerHour ,
        h = 360*( time % secPer12Hours  )/secPer12Hours ;
    secondHand .transform ("r" + s +",125,125" ); 
    minuteHand .transform ("r" + m +",125,125" );
    hourHand .transform ("r" + h +",125,125" );
   secondHand .animate({transform : "r" + [s + 360, 125, 125]}, 
                      60000, mina.linear);
   minuteHand .animate({transform : "r" + [m + 6, 125, 125]},
                      60000, mina.linear);
   hourHand .animate({transform : "r" + [h + 0.5, 125, 125]},
                      60000, mina.linear, updateClock ); 
}
The calculations are the same as in Example 14-4 .
Snap shorthand functions and transformation notation are used to set the time.
The animate  calls  get the clock  ticking.  The first parameter  to each  animate
function  gives  the position  of the hand  in 1 minute’s  time;  the second  hand  turns
360 degrees, the minute hand moves 6 degrees, while the hour hand moves half
a degree.  The second  parameter  indicates  that the animation  should  take 60,000
milliseconds  (1 minute)  to run, and the third  parameter  sets the animation  to
move at a steady (linear) rate.
260 | Chapter 14: Using the SVG DOM

When  the minute  is up, the callback  parameter  passed  to the final  animate
function  causes  updateClock()  to be run again.  This  resynchronizes  the
animated  clock  with  the system  clock,  and triggers  another  minute’s  worth  of
animated  motion.  Note  that only  one of the animate  functions  needs  the
callback, as running updateClock  will restart all three.
Y ou can see the clock in action online:
http://oreillymedia.github.io/svg-essentials-examples/ch14/snap_animated_clock.svg
Event Handling in Snap
Using a library like Snap also makes event handling much easier. The following example,
which uses Snap in an HTML page, is very simple: it displays a circle and a button. Y ou
can drag the circle, and clicking the button returns the circle to the center.
The main Snap functions you need for this script are Snap() , click() , and drag() . The
Snap()  function takes a string in the form #idName  and returns the corresponding el‐
ement as an object that wraps a DOM element with extra functionality . Once you have
an element, you can use its click()  and drag()  functions to have that element respond
to the appropriate events.
Example 14-7  shows the necessary HTML.
Example 14-7. HTML for Snap events example
<html xml:lang= "en" lang="en"
    xmlns="http://www.w3.org/1999/xhtml" >
<head>
  <title>Click and Drag Events in Snap </title>
  <meta http-equiv= "Content-Type"  content= "text/html; charset=utf-8"  />
  <script type="text/javascript"  src="snap.svg-min.js" ></script>
  <script type="text/javascript" >
    function  init() {
    }
  </script>
</head>
<body onload="init()" >
  <h1>Click and Drag Events in Snap </h1>
<div style="text-align:center" >
  <svg width="200" height="200" viewBox= "0 0 200 200"
    xmlns="http://www.w3.org/2000/svg"
    xmlns:xlink= "http://www.w3.org/1999/xlink" >
    <circle id="circle"  cx="100" cy="100" r="30"
      style="fill:#663399; stroke: black" />
Event Handling in Snap | 261

    <rect id="button"  x="60" y="170"
        rx="5" ry="5" width="80" height="25"
        style="stroke:black; fill:#ddd; cursor:pointer" />
    <text id="buttonText"  x="100" y="187" class="buttonText"
      style="fill:black; stroke:none;
      font-family: sans-serif; font-size: 12pt;
      text-anchor:middle; cursor:pointer" >Reset</text>
  </svg>
  </div>
</body>
</html>
Clicking Objects
To set a click handler, call a Snap element’s click()  function and pass it the name of
the function that handles the click. Here is the code you need to insert in order to handle
a click on the button and its text:
function  init() {
  Snap("#button" ).click(resetFcn );
  Snap("#buttonText" ).click(resetFcn );
}
function  resetFcn (evt) {
  Snap("#circle" ).attr({cx: 100, cy: 100});
}
The handler function gets the triggering event as its parameter, but in this case, the
resetFcn()  function doesn’t need to use it. If you try the code at this point, nothing will
appear to happen, as the circle is already at the center of the drawing. Change the value
of either cx or cy to see that the handler is really working.
Dragging Objects
Now that the button is handled, you can add drag handling to the circle. The drag()
method has three arguments: the name of a function to handle moving, the name of a
function to handle the drag start, and the name of a function to handle the drag end.
The drag start function takes three parameters: the starting x postion, the starting y
position, and the DOM event object that triggered the start.
The drag end function takes only one parameter: the DOM event object at the end of
the drag.
The drag move function has five parameters:
•dx, the shift in x from the start point
•dy, the shift in y from the start point
262 | Chapter 14: Using the SVG DOM

•x, the x position of the mouse
•y, the y position of the mouse
•event , the DOM event for the mouse movement
Y ou will need to remember where the circle’s starting point is:
var startX = 100;
var startY = 100;
Here is the code you need to add to init()  to assign the drag handlers to the circle:
Snap("#circle" ).drag(dragMove , dragStart , dragEnd);
And here are those functions (in logical order of start, move, end):
function  dragStart (x, y, evt) {
  // figure out where the circle currently is
  startX = parseInt (Snap("#circle" ).attr("cx"), 10);
  startY = parseInt (Snap("#circle" ).attr("cy"), 10);
}
function  dragMove (dx, dy, x, y, evt) {
  Snap("#circle" ).attr({cx: (startX + dx), cy: (startY + dy)});
}
function  dragEnd(evt) {
  // no action required
}
Figure 14-3  shows the result, edited to save vertical space.
Figure 14-3. Screenshot of circle being dragged
To click and drag it yourself, test out the online interactive version:
http://oreillymedia.github.io/svg-essentials-examples/ch14/snap_events.html
These examples are only the very minimum of what you can do with the Snap library.
For much more sophisticated examples, go to the website and see the demos. D3, Snap,
and Raphaël are not the only SVG libraries out there, but they all have one thing in
common: they make it easy for you to use JavaScript to create and manipulate SVG
dynamically. 
Event Handling in Snap | 263