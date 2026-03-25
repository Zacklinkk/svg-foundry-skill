Chapter 7. Gradients in All Shapes and Sizes
So far, we’ve discussed how to create gradients, and how to position color and opacity stops in between the beginning (left) and end (right) of the gradient.  Now, if all of your graphical needs involved gradients that went from left to right, then you’d be set. However, it’s likely that you might want gradients that go from top to bottom, or perhaps at a 60° angle, or otherwise transformed.

This chapter discusses the two ways in which you can control the postion of a linear gradient, and also how you can control its scale.  We’re going to assume you’re already comfortable with basic SVG shape elements (particularly <line>), coordinate systems, and transformations.  There are a number of tricky little distinctions to be aware of, and once again there are details that differ between SVG and CSS, so we’ll be highlighting those as well.

One thing we can’t do much about is the significant quality differences between web browsers when it comes to how smoothly gradients are rendered.  Particularly when using sharp color transitions, be sure to test your code to see if the results are acceptable.
The Gradient Vector
The first approach to positioning a gradient uses x1, y1, x2, and y2 attributes to position the start and end of the gradient, similar to drawing a line.  By default, x2 is 100% and the other attributes are 0.  If you drew a line with these attributes, it would go from left to right across the top of the drawing:

<line x1="0" y1="0" x2="100%" y2="0" />

In contrast, if you wanted a diagonal line from the top left to the bottom right, you would use the following:

<line x1="0" y1="0" x2="100%" y2="100%" />

What do the same attributes mean for a linear gradient?  The line they define is known as the *gradient vector*.  Stop offsets—and the blended values in between them—are measured according to the distance along that line.  The colors are then extended infinitely to either side of the line.  So for the following gradient, the blue stop is in the top left corner (the point defined by x1 and y1), while the green stop is in the bottom right corner (defined by x2 and y2):

<linearGradient id="blue-green"
                x1="0" y1="0" x2="100%" y2="100%" >
    <stop stop-color="blue" offset="0"/>
    <stop stop-color="darkSeaGreen" offset="1"/>
</linearGradient>

You can of course take advantage of the default values for the attributes, and write the same gradient using only the following attributes:

<linearGradient id="blue-green" y2="100%" >

Figure 7-1 shows rectangles filled with linear gradients using all possible combinations of 0% and 100% for the gradient vector attributes, except for the combinations that would make the start and end points the same.  Each column starts the vector in a different corner—defined by the (*x1*,*y1*) values—and then each row uses a different end point.
Warning
If the start and end points of the gradient vector are the same, the shape will be filled with a solid color.  *Which* solid color, however, is currently dependent on the browser.  The SVG specifications say that the entire area should be painted with the color of the *last* gradient stop, but at the time of writing Blink browsers and Internet Explorer use the *first* gradient stop.

Example 7-1 presents part of the code used to draw Figure 7-1.  The code makes use of a convenient shorthand for gradients: if you use the xlink:href attribute of one gradient to reference another, the new gradient is built by starting from the old one and then changing it to match any new attributes.  If the new gradient does not have any <stop> elements—as in the example—the stops from the referenced gradient are used.
Warning
The xlink:href value is a URL, and can theoretically point to gradients defined in external files.  This is currently only supported in Firefox.

The example then re-draws each gradient vector as a line overtop the gradient; line markers are used to draw arrows at the end of the line so you can tell which way it is pointing.
Example 7-1. Using attributes to position the gradient vector
<svg xmlns="http://www.w3.org/2000.svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="400px" height="300px" viewBox="0 0 400 300">
  <title>Same Gradient, Different Vectors</title>
  <style type="text/css">
      line {
          stroke: darkMagenta;                   
          stroke-width: 2;
          marker-start: url(#start);
          marker-end: url(#end);
      }
      marker {
          fill: darkMagenta;
      }
      svg {
          overflow: visible;
      }
  </style>
  <defs>
    <linearGradient id="blue-green" >            
        <stop stop-color="blue" offset="0"/>
        <stop stop-color="darkSeaGreen" offset="1"/>
    </linearGradient>
    <marker id="start" viewBox="-2 -2 4 4">      
      <circle r="1.5"/>
    </marker>
    <marker id="end" viewBox="-4 -2 4 4" orient="auto">
      <polygon points="-4,-2 0,0 -4,2"/>
    </marker>
  </defs>

  <g transform="translate(2,2)">                 
    <svg width="1in" height="1in">               
      <linearGradient id="blue-green-1" xlink:href="#blue-green"/>
      <rect width="100%" height="100%" fill="url(#blue-green-1)"/>
      <line x1="0" y1="0" x2="100%" y2="0"/>     
    </svg>

    <svg width="1in" height="1in" x="0" y="100">
      <linearGradient id="blue-green-2" xlink:href="#blue-green"
                      x2="100%" y2="100%"/>      
      <rect width="100%" height="100%" fill="url(#blue-green-2)"/>
      <line x1="0" y1="0" x2="100%" y2="100%"/>
    </svg>

    <!-- 9 more samples omitted for length -->   

    <svg width="1in" height="1in" x="300" y="200">
      <linearGradient id="blue-green-12" xlink:href="#blue-green"
                      x1="100%" y1="100%" x2="0" y2="100%"/>  
      <rect width="100%" height="100%" fill="url(#blue-green-12)"/>
      <line x1="100%" y1="100%" x2="0" y2="100%"/>
    </svg>
  </g>
</svg>

The stroke property must be set in order for a <line> to be visible; the two marker properties create the arrow effect.

The first linear gradient is never used directly; instead, it defines the gradient stops for all the other gradients.

The <marker> elements draw the arrowhead and the pivot point for the vectors.

Each swatch is drawn as a 1-inch square (96px); to give a little padding, the swatches are positioned at 100px intervals, starting at a 2px offset from the top left.

Each <linearGradient> uses the xlink:href attribute to duplicate the stops from the master gradient; the blue-green-1 gradient uses the default positioning attributes to create a left-to-right gradient.

The <line> explicitly declares the same positioning attributes to display the vector.

For the second (and subsequent) swatches, the gradient vector must be defined using explicit values for some or all of the positioning attributes.

You should be able to figure out the rest of the code from the arrows displayed in Figure 7-1; it’s much too repetitive to print it all in the book.  If you wanted to go for bonus points, you could write JavaScript to generate all the elements instead of repeating the markup.

For the final gradient, all of the positioning attributes are set explicitly to create a gradient vector the runs from right-to-left across the bottom of the rectangle.  Note, however, that the final gradient looks exactly the same as the one with a vector from right-to-left across the *top* of the rectangle.

All of the gradient samples in Figure 7-1 extend across the entire square, from one corner to another.  This is not required.  If the vector does not cover the entire shape, the solid color will (by default) be extended on either side.  This effect is similar to that created by offsetting the first and last stop away from the start and end of the gradient vector.
Tip
Changing the gradient vector length and changing the offsets have different effects when using repeating gradients, which we’ll get discuss in Chapter 8.

The start and end points of the gradient vector can also extend *outside* the 0 to 100% range; unlike with stop offsets, there is no clamping.  However, at this point you may be asking “100% of what?”
The Object Bounding Box
In Example 7-1, we used nested <svg> elements to create local coordinate systems in which each rectangle—and therefore each gradient—took up 100% height and width.  This meant that we could use percentages for the coordinates of the <line> elements and have them directly match the percentages used for the gradient vector.

In general, however, the shape that you are filling with the gradient will not take up the entire coordinate system.  So it’s important to understand that the values used in the gradient vector attributes are *not*, by default, measured in the local coordinate system that is used when drawing a <line>.  Instead, they are defined in new coordinate system based on the *object bounding box* of the shape being filled.

What’s an object bounding box?  For a rectangle like the ones used in Example 7-1 it’s, well, the rectangle.  Specifically, the geometric area of the rectangle itself, without any strokes.  For any other shape, it’s the tightest rectangle that can enclose that shape within its (possibly transformed) coordinate system.  Again, the bounding box does not include any strokes or markers.

For non-rectangular shapes, the gradient vector might start and end outside the shape even if the vector start and end points are all within the 0 to 100% range.  Figure 7-2 shows what happens if you replace all the rectangles from Example 7-1 with circles of the form:

<circle cx="50%" cy="50%" r="50%" fill="url(#*gradient-id*)"/>

Each circle fills up its local coordinate system as much as it can, but it doesn’t reach to the corners.  The gradient definitions haven’t changed, so the gradient vectors *do* reach to the corners.  The result for the diagonal vectors is that the stop colors at the start and end of the gradient are clipped out.  To revisit our paint server analogies, the painted effect is scaled to fit the entire object bounding box and only later is the masking tape removed to reveal the shape underneath.

As mentioned, the object bounding box is an entire new coordinate system.  Not only are percentages scaled to fit the box, but so are basic user units.  One unit in the horizontal direction equals 100% of the bounding box width, and one unit in the vertical direction equals 100% of the height.  In other words, you can use decimals between 0 and 1 in place of the percentages.

You can even use lengths with units, but—as with every other form of scaling in SVG—the length units scale with the user units.  A 1px length equals 1 user unit equals 100% of the width or height under this new coordinate system.  Every other length unit would equal many times the width or height of the shape.

Since units scale to match the width and height of the bounding box, a horizontal unit may no longer match a vertical unit.  In Example 7-1 they did, because the shapes being filled were square.  As a result, the diagonal gradient vectors were perfect 45° lines.  It also means that lines *perpendicular* to (i.e., at right angles to) the gradient vector were perfect 45° lines in the opposite direction, the same angle as a diagonal between the opposite corners.

This is important, because these perpendicular lines equate to parts of the gradient that have the same color.  The stop color values are extended on either side of the gradient vector following perpendicular lines.  When the gradient vector connects opposite corners of the box on a diagonal, the perpendicular line to the mid-point of the gradient—equivalent to a stop offset of 50%—stretches along the diagonal between the other corners of the box.  Example 7-2 constructs a gradient with a sharp color transition at a 50% offset to emphasize this relationship.
Example 7-2. Creating a diagonal stripe from a diagonal gradient
<svg xmlns="http://www.w3.org/2000.svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="100%" height="100%">
    <title>Stretched Object Bounding Box gradients</title>
    <style type='text/css'>
    </style>
    <linearGradient id="diagonal" y2="100%">
        <stop offset="0" stop-color="white"/>
        <stop offset="0.5" stop-color="red"/>
        <stop offset="0.5" stop-color="blue"/>
        <stop offset="1" stop-color="white"/>
    </linearGradient>
    <rect height="100%" width="100%" fill="url(#diagonal)"/>
</svg>

In Example 7-2, both the SVG as a whole and the gradient-filled rectangle are set to fill 100% of the width and height of the browser window or image area the SVG is drawn into.  Figure 7-3 shows how it looks when drawn into a square region.  The gradient vector is the 45° diagonal from the top left to bottom right, and the stripe at the 50% gradient offset makes a perfect diagonal from the top right to the bottom left.

If the shape (or more to the point, the object bounding box) that you’re filling is *not* a perfect square, what happens?  A diagonal is no longer a 45° angle.  However, thanks to the stretched effect of the object bounding box coordinate system, the midpoint of the gradient still follows a diagonal line between the opposite corners, as shown in Figure 7-4.

The lines of equal color are no longer perpendicular to the gradient vector in the normal sense of the word—or at least, not in the outside coordinate system.  In the stretched coordinate system, they have the same mathematical relationship between *x* and *y* coordinates as perpendicular lines normally have.  In mathematics, this “officially-perpendicular” line is actually called a *normal* vector, but mathematicians have strange ideas about what is normal.

Object bounding box coordinate systems therefore create a non-uniform scale, similar to a two-parameter scale(*sx*,*sy*) transformation or a preserveAspectRatio="none" setting.
Drawing Outside the Box
If you were reading carefully, you would have noticed those sneaky little words “by default” not that long ago.  Object bounding box units are the *default* for defining a gradient vector, but they aren’t the only option.  You can use the gradientUnits attribute on the <linearGradient> element to change things up.

Setting gradientUnits to the keyword userSpaceOnUse makes the browser interpret your x1, y1, x2, and y2 attributes within the coordinate system used to draw the shape that is being filled.
Tip
If you wanted to explicitly set the default behavior, the other value for gradientUnits is objectBoundingBox.

Figure 7-5 shows the result of modifying Example 7-2 to change the gradientUnits value:

<linearGradient id="diagonal" y2="100%"
                gradientUnits="userSpaceOnUse">

Because the rectangle completely fills the coordinate system, the gradient vector still goes from the top left corner of the rectangle to the bottom right.  However, because the coordinate system itself has not been distorted, the 50% stop is drawn at a real-world right angle to this diagonal, and no longer connects the other corners.

Avoiding distorted coordinates is only one reason to use userSpaceOnUse.  With userSpaceOnUse units, you can create a single gradient that continues from one shape to another.  This is demonstrated in Example 7-3, with the results shown in Figure 7-6.
Example 7-3. Using userSpaceOnUse gradient units to fill multiple shapes
<svg xmlns="http://www.w3.org/2000.svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="400px" height="300px" viewBox="0 0 400 300">
    <title>Tropical Sunset User-Space Gradient</title>
    <linearGradient id="sunset" gradientUnits="userSpaceOnUse"
                    y1="1em" x2="0" y2="250px">                
        <stop stop-color="midnightBlue" offset="0"/>
        <stop stop-color="deepSkyBlue" offset="0.25"/>
        <stop stop-color="yellow" offset="0.5"/>
        <stop stop-color="lightPink" offset="0.8"/>
        <stop stop-color="darkMagenta" offset="0.99"/>
        <stop stop-color="#046" offset="0.99"/>                
    </linearGradient>

    <rect height="100%" width="100%" fill="dimGray" />         

    <g fill="url(#sunset)">                                    
        <rect x="20" y="20" width="100" height="120" />
        <rect x="280" y="20" width="100" height="120" />
        <rect x="20" y="160" width="100" height="120" />
        <rect x="280" y="160" width="100" height="120" />
    </g>
    <rect x="140" y="0" width="120" height="300"
          fill="url(#sunset)" />                               
</svg>

The userSpaceOnUse value for gradientUnits allows us to use a lengths such as 1em or 250px to position the gradient vector.  The x1 attribute is left as the default 0, while the x2 attribute (100% by default) is reset to 0 to create a straight vertical gradient.

The final two gradient stops have the same offset value, creating a sharp stripe for the line between the sunset sky and the still water; all points in the gradient beyond the end of the gradient vector will be painted with the dark blue-green color #046.

A gray rectangle fills the entire SVG, behind the elements that will be filled with the gradient.

As always, you can specify a fill value on a grouping element, and it will be inherited by the child shapes.  However, the gradient is still calculated for each shape individually; if they use an objectBoundingBox gradient, each shape would have a gradient based on its own bounding box, not on the group.

To confirm that the continuous nature of the gradient has nothing to do with the group, the central rectangle is drawn separately.  It is the full height of the SVG so you can see the entire gradient, including the solid-colored parts beyond the ends of the gradient vector.
Warning
The initial code for Example 7-3 placed the final, sharp gradient transition (between the sky and the sea) at offset="1".  According to the specifications, when multiple stops have the same offset the final color value should be used for that point, and any padded colors beyond.  However, Blink browsers ignored the final stop with that offset value and filled in the remaining space with dark magenta.  For consistent results between browsers, avoid using sharp transitions at 0% or 100% offset.

The image in Figure 7-6 *could* have been drawn by filling the background with a single continuous gradient, and then creating complex paths to recreate the shapes of the patio doors opening onto that tropical sunset.  Instead, the gray door frames are actually the only places where the back drop is visible, and the windows and opening are actually rectangles drawn on top.  They appear continuous because they are all filled with the same continuous gradient.  However, keep in mind that the “user space” is still not an absolute coordinate system—it is affected by any transformations or nested coordinate systems that apply to the shape being filled.
Warning
Although all browsers apply transformations to user-space paint servers, nested coordinate systems are more buggy.  Webkit, Blink, & Internet Explorer use the parent SVG of the *gradient*, not the shape, to convert percentages to user units.
Gradients, Transformed
We mentioned at the start of the chapter that there were two ways to control the direction of a linear gradient in SVG.  The second approach uses the gradientTransform attribute.

The value of the gradientTransform attribute a list of the same transformation functions you can use to manipulate the position and orientation of shapes:

- 
translate(*tx*,*ty*) to shift without distorting

- 
scale(*s*) or scale(*sx*,*sy*) to zoom in or out, stretch, or flip into a mirror image

- 
rotate(*a*) or rotate(*a*,*cx*,*cy*) to rotate, around the origin or the specified center-point

- 
skewX(*a*) or skewY(*a*) to tilt one axis or the other

Any number of these functions can be listed in series.

The effects you can create by transforming a gradient are similar to the effects you create by manipulating the gradient vector.  For any given graphic, one or the other may be easier to calculate.
Tip
You can combine transformations with positioning attributes.  Just as when transforming shapes, the positioning attributes are calculated in the transformed coordinate system.

What does it mean to transform a gradient?  It means that the wallpaper created by the paint server is transformed, *before* cutting out the shape being filled.  In contrast, when you transform a shape that is filled with a gradient, *both* the shape and the gradient are transformed.

Figure 7-8 compares the two effects, using transformed and un-transformed gradients in transformed and un-transformed shapes.  Example 7-5 provides the code.
Example 7-5. Transforming shapes and gradients
<svg xmlns="http://www.w3.org/2000.svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="400" height="500" viewBox="0,0 400,500">
    <title>Transforming shapes, gradients, or both</title>

    <linearGradient id="stripe" x2="0" y2="100%">        
        <stop offset="0" stop-color="yellow"/>
        <stop offset="0.1" stop-color="gold"/>
        <stop offset="0.5" stop-color="tomato"/>
        <stop offset="0.5" stop-color="blueViolet"/>
        <stop offset="0.9" stop-color="indigo"/>
        <stop offset="1" stop-color="midnightblue"/>
    </linearGradient>
    <linearGradient id="stripe-transformed" xlink:href="#stripe"
                    gradientTransform="skewY(25)" />     

    <g fill="url(#stripe)" >                             
        <rect height="190" width="190" x="5" y="5" />
        <rect height="190" width="190" x="5" y="5"
              transform="translate(200,0) skewY(25)"/>   
    </g>
    <g transform="translate(0,200)"
       fill="url(#stripe-transformed)">
        <rect height="190" width="190" x="5" y="5" />    
        <rect height="190" width="190" x="5" y="5"
              transform="translate(200,0) skewY(25)"/>   
    </g>
</svg>

The basic gradient is oriented top to bottom, as defined by the positioning attributes: the start point is the default (0,0) and the end point is (0,100%).

The second gradient uses xlink:href to duplicate the same pattern of color stops; the gradientTransform attribute applies a skew transformation to the pattern.

The first two <rect> elements are grouped together to apply the fill value; each will be filled with the un-transformed gradient.

The second square is translated horizontally into the top right of the graphic and then skewed.  However, the gradient used to fill it was not skewed separately, so the color stops still align exactly with the top and bottom edges of the square.

The squares on the bottom row use the transformed gradients.  On the left, the square itself has not been skewed, only the gradient.

The final square is both skewed itself and filled with the skewed gradient; the effect on the final gradient angle is therefore compounded.

The transformation on the shape maintains the relationship between the gradient colors and the edges of the shape: squares in the same row of Figure 7-8 have the same colors in the corners, regardless of whether or not the shape as a whole is skewed.  In contrast, when the gradient is transformed it moves separately from the shape, changing which parts of the gradient are visible.  The deep midnight-blue color is almost completely cut off, while large sections of solid yellow are visible.

There is no direct way to transform a shape but *not* the paint server content used to fill it.  Even userSpaceOnUse gradients will reflect the coordinate system transformations on the shape using the gradient.
Tip
If you want un-transformed paint server content inside a transformed shape, you can use a clipping path to apply the transformed shape boundaries to a larger, untransformed rectangle filled with the paint.

Skewing, clearly, is one way gradient transformations can be used to create a diagonal gradient.  Rotating is another.  Example 7-6 creates rainbow gradients rotated at 45° angle-intervals, and then uses these to fill ellipses arranged around the edges and corners of the SVG.  Figure 7-9 shows the result.
Example 7-6. Transforming gradients using rotations
<svg xmlns="http://www.w3.org/2000.svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
    width="400" height="300" viewBox="-200,-150 400,300">  
    <title>Rotated Gradients</title>
    <defs>
        <linearGradient id="rainbow" >
            <stop stop-color="darkViolet" offset="0"/>
            <stop stop-color="blue" offset="0.143"/>       
            <stop stop-color="cyan" offset="0.286"/>
            <stop stop-color="limeGreen" offset="0.429"/>
            <stop stop-color="yellow" offset="0.572"/>
            <stop stop-color="orange" offset="0.715"/>
            <stop stop-color="red" offset="0.857"/>
            <stop stop-color="maroon" offset="1"/>
        </linearGradient>
        <linearGradient id="rainbow45" xlink:href="#rainbow"
                        gradientTransform="rotate(45)"/>   
        <linearGradient id="rainbow90" xlink:href="#rainbow"
                        gradientTransform="rotate(90)"/>
        <linearGradient id="rainbow135" xlink:href="#rainbow"
                        gradientTransform="rotate(135)"/>
        <linearGradient id="rainbow180" xlink:href="#rainbow"
                        gradientTransform="rotate(180)"/>
        <linearGradient id="rainbow225" xlink:href="#rainbow"
                        gradientTransform="rotate(225)"/>
        <linearGradient id="rainbow270" xlink:href="#rainbow"
                        gradientTransform="rotate(270)"/>
        <linearGradient id="rainbow315" xlink:href="#rainbow"
                        gradientTransform="rotate(315)"/>
    </defs>

    <ellipse rx="60" ry="40" cx="130" cy="0"
             fill="url(#rainbow)"/>                        
    <ellipse rx="60" ry="40" cx="130" cy="100"
             fill="url(#rainbow45)"/>
    <ellipse rx="60" ry="40" cx="0" cy="100"
             fill="url(#rainbow90)"/>
    <ellipse rx="60" ry="40" cx="-130" cy="100"
             fill="url(#rainbow135)"/>
    <ellipse rx="60" ry="40" cx="-130" cy="0"
             fill="url(#rainbow180)"/>
    <ellipse rx="60" ry="40" cx="-130" cy="-100"
             fill="url(#rainbow225)"/>
    <ellipse rx="60" ry="40" cx="0" cy="-100"
             fill="url(#rainbow270)"/>
    <ellipse rx="60" ry="40" cx="130" cy="-100"
             fill="url(#rainbow315)"/>
</svg>

The SVG uses a centered coordinate system.

The rainbow gradient has eight stops, so the offsets are multiples of 1/7 (0.143).

Again, the almost-identical gradients are created by using xlink:href to link the original gradient, and then adding the new attributes: in this case, the gradientTransform.

The original gradient is arranged in the default left-to-right orientation; the ellipse filled with that gradient is positioned on the right edge of the SVG.  The remaining ellipses are positioned in clockwise manner around the SVG to match the increasing clockwise rotation in the gradient transform angle.

Wait—why are some of the shapes filled with solid violet instead of a gradient?  Because, while the ellipse may have been arranged around the center of the SVG coordinate system, the *gradients* are rotated around the center of *their* coordinate system: the object bounding box coordinates for each shape.  As with all SVG coordinate systems, the default origin is the top left corner—and for gradients, there’s no viewBox attribute to allow you to change that.

The gradient vector—initially pointing left to right along the top of each shape’s bounding box—is rotated around the origin of each box—the top left corner.  Rotating the vector by 90° (as in the bottom center ellipse) will shift it so that the gradient changes from top to bottom.  However, rotate any further and the vector is pointing outside of the shape’s bounding box.  Most or all of the shape is in the region before the start of the gradient, and is therefore painted solid with the color of the first gradient stop (the afore-mentioned darkViolet).

To effectively use rotational transformations of linear gradients in object bounding box space, you’ll need additional manipulations.  Some options:

- 
Use translations within the gradientTransform attribute to reposition the gradient into the shape,

- 
Use the three-value rotate(*a*, 0.5, 0.5) function to rotate around the coordinate system’s center point (or use transform-origin if using CSS rules).

- 
Change the vector’s positioning attributes so that it will be on the correct side of the origin *after* the rotation.

There are further complications.  Although you’ve transformed the gradient’s direction, a rotation does not change its length; the stop offsets will still be judged relative to the horizontal length of the vector.  Because of the distorted object bounding box coordinates, the default length of a horizontal gradient (1 horizontal unit) will still fill the box vertically (1 vertical unit) after a 90° rotation.  However, it won’t match the diagonal length (1.141 stretched units) after a 45° rotation, so a 50% offset will *not* neatly connect the other corners of the box.  So the gradient stretches and shrinks as it rotates, but not quite the way you might expect.

If you were to use userSpaceOnUse units for the gradient, it would be rotated around the origin of the entire SVG’s coordinate system—which in this case, is centered in the middle of the image.  Figure 7-10 shows this effect; the only change in the code, relative to Example 7-6, is in the attributes for the main gradient element:

<linearGradient id="rainbow" gradientUnits="userSpaceOnUse"
                        x1="20%" x2="50%">

The change in x1 and x2 values is required because the percentages will now be calculated relative to the SVG’s width instead of the width of each shape.  These new attributes only need to be set once; they are automatically duplicated to all the other gradients that reference this one.

If you look closely, there is another difference between Figure 7-10 and Figure 7-9.  To make it easier to see, Figure 7-11 extracts the bottom-right ellipse from each figure—the one with a gradient transform of rotate(45).  The ellipse on the left uses the object bounding box gradient, while the ellipse on the right uses user space coordinates.  The angles aren’t the same.  With the object bounding box gradient, 45° looks more like 60°.

The reason is that the transformation angles are calculated in the stretched coordinate system created by the object bounding box.  If the bounding rectangle is roughly square, the final angle will match the angle you specify.  If not—as with these ellipses—the angles will be squished or stretched accordingly.  A 45° angle in object bounding box space always matches the diagonal of the box, but it won’t necessarily match the same angle in any other coordinate system.