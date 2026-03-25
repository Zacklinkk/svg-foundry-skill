Chapter 3. Creating Colors
In this chapter, we examine more thoroughly the options you have for filling your graphics with solid blocks color.  It starts by outlining how color works on the web, and describes the different ways in which you can specify colors on the web: from the very readable (but not very rational) color keywords, to RGB and HSL color functions.

The color basics will be complemented by a discussion of partially transparent colors in Chapter 4.  Color concepts are also an essential prerequisite for the colored gradients that will be introduced starting in Chapter 6.
Misty Rose by Any Other Name
When writing code for other people to read (for example, the examples in this book!), it is nice to be able to use human-readable color names like red or gold or aquamarine.

It is even nicer if a computer can read the same values.  And with SVG, they can.  Web browsers and SVG editors should all understand red, gold, and aquamarine.  They will also recognize more fanciful names like mistyRose, peachPuff, and mediumSeaGreen.

Where do these names come from?  There are two sources: a very simple set of color keywords that was introduced in early versions of HTML and CSS, and a much more extensive set of keywords that were adopted by SVG (and later CSS) from the X11 windowing system for Unix computers.  Both sets of keywords are supported in all major SVG viewers.  In addition, all but the oldest browsers in use also support them for other CSS properties.

As user-friendly as they are, the keyword system has a number of limitations.

For starters, the 147 keywords only describe a small fraction of the millions of possible color variations that a modern computer monitor can display.

The keyword choices are also rather inconsistent and arbitrary.  The original web colors and the X11 colors sometimes conflict.  The colors cyan (from X11) and aqua (from CSS 1) are identical—but different from aquamarine.  The color darkGray (from X11) is actually *lighter* than the color gray (from CSS 1).
Tip
All the gray keywords can also be spelled using grey.  But that’s a feature, not a bug.

The X11 color names themselves are not particularly systematic, either: some have dark, medium, and light variants, others have pale variants.  And the variations aren’t always logical: darkSeaGreen is not really *darker* than seaGreen, lightSeaGreen and mediumSeaGreen; it’s just *duller*.

Nonetheless, if you enjoy the ease of using readable color names, all the recognized keywords are listed in Appendix A along with their numeric equivalents.  Figure 3-1 shows the resulting colors as an alphabetical patchwork from AliceBlue to yellowGreen.

The keyword names are case insensitive.  Most official references use all-lowercase, but this book uses camelCase (capitalizing the start of subsequent words) to make them easier to read.

The code used to create Figure 3-1 is provided as Example 3-1.  It uses XMLHttpRequest to load a separate file with the list of color keywords in alphabetical order, and then creates rectangles filled with each color.  Each rectangle has a child <title> element containing the color name; if you run the code in a browser, these will be available as tooltips when you hover over each color swatch.
Example 3-1. Creating a color keyword patchworkExample 3-1. SVG Markup:
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="400px" height="600px"
     viewBox="0 0 7 21" preserveAspectRatio="none" >   
    <title>SVG Color Keywords</title>

    <script><![CDATA[
        /* script goes here */
]]> </script>
</svg>

There are 147 keywords in the SVG specification (including the variant spellings for gray), which is conveniently 7*21.  The viewBox creates a grid of 7 columns and 21 rows, and uses preserveAspectRatio="none" to make that grid stretch to fill the entire <svg>.
Example 3-1. JavaScript:
(function(){
    var svgNS = "http://www.w3.org/2000/svg";
    var xlinkNS = "http://www.w3.org/1999/xlink";
    var svg = document.documentElement;

    var dataFileURL = "color-names.csv"                    
    var request = new XMLHttpRequest();
    request.addEventListener("load", draw);
    request.overrideMimeType("text/csv");
    request.open("GET", dataFileURL);
    request.send();

    function draw() {
        var w = 7; //swatches per row

        var colors = request.responseText.split("\n");     

        for (var i=0, n=colors.length; i<n; i++){
            var c = colors[i].trim();                      

            var swatch = document.createElementNS(svgNS, "rect");
            swatch.setAttribute("width", 1);
            swatch.setAttribute("height", 1);
            swatch.setAttribute("x", i % w );
            swatch.setAttribute("y", Math.floor(i / w) );  

            swatch.style.setProperty("fill", c);           

            var tip = document.createElementNS(svgNS, "title");
            tip.textContent = c;
            swatch.insertBefore(tip, null);                

            svg.insertBefore(swatch, null);                
        }
    }
})();

The XMLHttpRequest object is used to load a separate data file with a list of color names.  An event listener is used to call our drawing function when the file has been loaded.  The MIME type, "text/csv", indicates that we’re expecting a delimited text file; that way, the browser won’t try to parse it as XML.

The draw() function uses the request’s responseText property to get the entire file as a single JavaScript string.  The data file has each keyword on a separate line; it was created by making a spreadsheet with a single column, and then saving that as a *.csv* comma-separated variable file.  The split(*token*) method creates an array from a string, broken around that token—in this case, the newline character, escaped in Javascript as \n.

A for loop cycles through each string in the array (i.e., each line from the data file).  The trim() method gets rid of any errant whitespace.

Each rectangle is set to width and height of 1 in the stretched SVG coordinate system; the horizontal and vertical positions are calculated from the index and the number of swatches per row.

The fill style property is set using the keyword name.

The keyword is also used as the text content of a <title> element, which is then added as a child of each <rect> to create the tooltips.

Finally, the styled <rect> (with its tooltip) is added to the SVG.

Looking at Figure 3-1, it becomes clear that the color keywords are not a representative sample of all possible colors.  In addition to the duplicated grays, there are quite a number of off-white colors, and relatively few dark tones.

Sensibly, there are many more options for defining colors than the 147 keywords.  To fully understand how custom colors work, however, we’re going to first diverge into a little bit of physics and a little bit of biology.
A Rainbow in Three Colors
In physics, color is a property of the wave frequency or energy level of light.  Visible light itself consists of a specific range of frequencies within the much wider spectrum of electro-magnetic radiation.  At lower energies, electro-magnetic radiation creates radio waves, at much higher energies it becomes x-rays.

The spectrum of light is continuous.  There are no clear distinctions between colors, just a smooth rainbow from red through orange, yellow, green, blue, and violet—and beyond, to the ultraviolet lights that our eyes cannot see.  Each color is associated with a specific frequency and energy level.

Just as you can tune your radio to be sensitive to one radio frequency and not others, so colored pigments are sensitive to a specific frequencies of light.  They absorb energy from light of the correct frequencies—colors—and reflect other light.  Similarly, chemical reactions that *emit* light do so at specific frequencies—colors—based on the energy released by the reaction.  Heat-based light sources can nonetheless be “tuned” to adjust the color.  As the efficiency of a flame’s combustion reaction increases with different fuel or oxygen levels, the energy per photon (unit of light) increases and the color changes from a dull red, like glowing coals, to yellow, to a bright blue, like the gas flame shown in Figure 3-2.

The frequencies in light don’t mix any more than different radio stations do.  Each photon preserves its own energy level and color.  You can separate the many colors of sunlight into a rainbow, by shining the light through a prism or through fine mist.

So why, then, does sunlight normally look white, and not technicolor?  That’s where the biology comes in.

Your eye is sensitive to a certain range of the electro-magnetic spectrum—the part we call visible light—because of pigment molecules that absorb the energy from that light and convert it to chemical signals in your brain.  Most human beings have four types of light-sensitive pigments in their eyes: a very sensitive but not color-specific pigment (responsible for night vision and some motion detection), and three color pigments.  Each color pigment is sensitive to different but overlapping regions of the spectrum.

Our eyes, therefore, *don’t* see color as a continuous spectrum of every possible frequency of light.  The only see how much light is absorbed by each of the three colored pigments.  Your brain compiles this information into the full range of colors that you see.

The pigments in our eyes are usually associated with the colors blue, green, and red, but that’s an oversimplification: the green and red pigments are sensitive to blue light as well, and all three are sensitive to medium-green light.  Another naming system describes them as S (for short wavelength), M (medium), and L (long wavelength), since the frequencies of light are usually defined by the light wavelength.
Note
Because light has a fixed speed, the frequency (number of waves per second) and the wavelength (distance between subsequent waves) are directly interchangeable.  Higher energy light has a higher frequency and a shorter wavelength.

Because the eyes see color as a mixture of three different values, we can “mix” colors in art if not in physics.  Figure 3-3 outlines how it works.  When your eye is exposed to pure yellow light, *both* the M and L pigments are triggered in equal measure, but the S pigment is not.  Your brain constructs the sensation of the color yellow from this information.

If your eye is exposed to an equal mixture of bright green and bright red light from the same location, the same pattern of pigment activation is created, and your brain still thinks it is looking at yellow.

Nearly every form of color communication in use, both print and screen, takes advantage of color mixing.  The colors involved differ between print (which uses pigments that absorb the light shining on them) and screen (which shines light directly to your eyes).

Color computer monitors—like color TVs before them—use red, green, and blue light of frequencies that capitalize on the differences between the pigments in your eyes. The finely spaced red, green, and blue lights that make up each pixel are modulated to recreate nearly every pattern of activation in your eye that natural light could stimulate.
Note
Because of the unevenly-overlapping patterns of color sensitivity between the different pigments in your eye, there are some colors—certain saturated greens, intense blues, and deep reds—that can never be accurately represented by RGB light.  Similarly, colored printing methods have certain natural colors that they can never completely recreate.  The spectrum of possible colors that can be represented by a color system is known as the *gamut* of that color system.

Digital graphics systems have a fixed number of brightness levels for each color in a pixel.  Early color computers had 4 levels for each color—including off—resulting in 64 (=4×4×4) colors overall.  The original list of “web safe” color keywords used in HTML can all be mapped to a 64-color monitor.  However, most modern computer displays support 256 levels (0-255) for each color.  This is the basis of the color encoding used on the web.
Custom Colors
There are two ways, in CSS or SVG, to define a custom color as a set of RGB values:

- 
function notation, like rgb(*red*,*green*,*blue*)

- 
hexadecimal notation, like #*RRGGBB* or #*RGB*

The values used in the function notation may be either integers between 0 and 255 or percentages.  You can’t mix integers and percentages—all values must be the same type.

The values in a six-digit hexadecimal format also represent the numbers from 0 to 255, but do so using hexadecimal numbers.  In hexadecimal, each digit can be a value between zero and fifteen, instead of only zero to 9 (*hexa* is six, *deci* is 10, so *hexadecimal* is a base-16 number system).  The extra digits are indicated by the letters A (ten) to F (fifteen).

The three-digit hexadecimal format is a shorthand for colors where both hexadecimal digits in each color are the same.
Tip
Hexadecimal digits are case-insensitive in CSS and SVG; #ACE and #ace are equivalent (and represent a light blue color that can also be written #AACCEE).

The following color definitions all specify the same RGB values:

- 
rgb(102, 51, 153)

- 
rgb(40%, 20%, 60%)

- 
#663399

- 
#639
Note
In some of the latest web browsers, you can also represent the color #639 with the keyword RebbecaPurple.  The name was added to the CSS Level 4 Colors module in tribute to Eric Meyer—a former member of the W3C CSS working group and author of many books on CSS—and his daughter Rebecca, who loved the color purple and who died on her sixth birthday.  It is the only color keyword that has been added since the SVG 1 specifications.

The CSS Color Level 3 module introduced an alternate way of describing colors, based on a more universal color theory instead of the RGB computer monitor.  The Hue-Saturation-Lightness (HSL) color model describes colors as a mixture of a “pure” color and black, white, or gray.  Specifically, the three values are:

hue

The pure color, defined as an angle on a color wheel where pure red is 0°, pure yellow is 60°, intense green is 120°, and so on: 300° is magenta and 360° is red again.

saturation

The intensity of the pure color (adjusted for lightness) in the mixture, where 0% saturation is a shade of gray and 100% saturation is a vibrant color.

lightness

The level of white or black in the mixture, where 0% lightness is pure black, 100% lightness is pure white, and 50% lightness is the most intense color.

Unlike with RGB values, HSL values are not always unique; different HSL combinations can create the same color.  For example, any value with 0% saturation will be a gray, regardless of the hue, and any value with 100% lightness will be white, regardless of the hue or saturation.
Note
Two other color models are confusingly similar to HSL: the Hue-Saturation-Value (HSV) model and the Hue-Saturation-Luminance (unfortunately, also abbreviated HSL) model.  The definitions for hue and saturation are the same, but the values for the third measure are not interchangeable.  If you’re trying to match colors defined in other graphics software, be sure you’re using the same color model.

Figure 3-4 uses circular color wheels to demonstrate the relationship between hue angle and color, with different lightness values shown at different radii and different saturation values shown in separate wheels.

The brightest, most intense colors are created when saturation is 100% and lightness is 50%.  In the RGB model, these colors have at least one color channel at 100% and one channel at 0%.  In general, when converting from RGB to HSL:

- 
the saturation value is calculated as 100% minus the percentage of the *maximum* channel made up by the *minimum* RGB channel

- 
the lightness value is the *average* between the minimum and maximum color channels (as a percentage)

- 
the hue is determined from the ratio between the maximum and medium color channels, *after* subtracting the value from the minimum channel.

To specify a color as an HSL value in CSS, you use the hsl(*h*,*s*%,*l*%) function.  The hue value, although theoretically measured in degrees, is given as a number without units.  Saturation and lightness are always expressed as percentages.  Some examples:

- 
the color lime, rgb(0%, 100%, 0%), is hsl(120, 100%, 50%)

- 
the color green, rgb(0%, 50%, 0%), is hsl(120, 100%, 25%)

- 
the color purple, rgb(50%, 0%, 50%), is hsl(300, 100%, 25%)

- 
RebeccaPurple, rgb(40%, 20%, 60%), is hsl(270, 50%, 40%)
Warning
Although hsl() color functions are supported in all modern browsers (Internet Explorer 8 being the only browser in common use without support), they may not be supported in other tools for editing, displaying, or converting SVG.  The same is true for other CSS 3 color values which will be discussed in this chapter.

Example 3-2 presents the SVG and JavaScript code used to create Figure 3-4.  It creates <use> elements for each colored pie segment, and uses the style object to set the fill to an hsl() color value.  Each piece is also rotated into place using the hue value as the parameter to a rotate() transformation function.
Example 3-2. Building an HSL color wheel using scripted SVGExample 3-2. SVG Markup:
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     width="400px" height="400px" viewBox="0 0 200 200" >
    <title>HSL Color Wheel</title>
    <defs>                                                 
        <circle id="center" r="1.5" fill="black"/>
        <path id="inner" transform="rotate(-15)"
              d="M0,0L15,0A15,15 0 0 1 12.99,7.5 L0,0Z" /> 
        <path id="middle" transform="rotate(-15)"
              d="M15,0L28,0A28,28 0 0 1 24.25,14
                 L12.99,7.5 A15,15 0 0 0 15,0Z" />
        <path id="outer" transform="rotate(-15)"
              d="M28,0L40,0A40,40 0 0 1 34.64,20
                 L24.25,14 A28,28 0 0 0 28,0Z" />
        <circle id="edge" r="39" fill="none"
                stroke="white" stroke-width="2"/>
    </defs>
    <style type="text/css">
        text {
            text-anchor: middle;
            font-size: 8px;
            font-family: sans-serif;
        }
    </style>
    <rect fill="#888" width="100%" height="100%"/>         
    <svg class="wheel" width="100" height="100" x="0" y="0"
         viewBox="-40,-40 80,80">                          
        <use xlink:href="#center"/>
        <use xlink:href="#edge"/>                          
    </svg>
    <svg class="wheel" width="100" height="100" x="100" y="0"
         viewBox="-40,-40 80,80">
        <use xlink:href="#center"/>
        <use xlink:href="#edge"/>
    </svg>
    <svg class="wheel" width="100" height="100" x="0" y="100"
         viewBox="-40,-40 80,80">
        <use xlink:href="#center"/>
        <use xlink:href="#edge"/>
    </svg>
    <svg class="wheel" width="100" height="100" x="100" y="100"
         viewBox="-40,-40 80,80">
        <use xlink:href="#center"/>
        <use xlink:href="#edge"/>
    </svg>
    <script><![CDATA[
        /* script goes here */
]]> </script>
</svg>

All the shapes are pre-defined to be re-used many times.  They are encoded to be drawn within an 80×80 centered coordinate system—each complete wheel will be a circle with radius 40.

The wedge-segments are created using path notation and a little bit of trigonometry.  Each wedge will be a 30° segment, centered on the *x*-axis.  To reduce the trigonometry, the wedges are defined with one edge flat against the *x*-axis and then are rotated back until they are centered over it.  This way, the coordinates of one end of each arc can be determined as (*r*,0), where *r* is the radius.  The coordinates of the other end are (*r**cos(30°), *r**sin(30°)), which is equal to (*r**0.866, *r**0.5).

A rectangle filled with 50% gray (#888) provides a background to the image.

Each color wheel is defined by an <svg> element that positions it within the image and creates the correct 80×80 centered coordinate system using its viewBox attribute.

Each <svg> also contains the black center and white edges of the wheel, since these do not need any special calculations.
Example 3-2. JavaScript:
(function(){
    var svgNS = "http://www.w3.org/2000/svg";
    var xlinkNS = "http://www.w3.org/1999/xlink";

    var wedge = 30; //angle span of each pie piece, in degrees
    var saturation = ["100%", "75%", "50%", "25%"];             
    var lightness = {outer:"75%", middle:"50%", inner:"25%"};   

    var wheels = document.getElementsByClassName("wheel");      
    var h,s,l,w,p,u;
    for (var i=0, n=wheels.length; i<n; i++){
        w = wheels[i];
        s = saturation[i];                                      
        for (h=0; h < 360; h += wedge ) {
            for (p in lightness){                               
                l = lightness[p];
                u = document.createElementNS(svgNS, "use" );
                u.setAttributeNS(xlinkNS, "href", "#"+p );
                u.setAttribute("transform", "rotate("+h+")" );  
                u.style.setProperty("fill", "hsl("+[h,s,l]+")" ); 
                w.insertBefore(u, w.firstChild);                
            }
        }
        var t = document.createElementNS(svgNS, "text" );       
        t.textContent = "s = "+s;
        t.setAttribute("y", "35");
        w.insertBefore(t, null);
    }
})();

The saturation values for each wheel are stored in an array, ready to be assigned to the individual <svg> elements.  Since we don’t need to do any mathematical calculations on these values, they are stored as strings including the % marker.

Rather than using two separate arrays to hold the lightness values and the id values for the different wedge segments, they are paired up using a key-value data object.

The nested <svg> elements are selected by class name.

A for loop cycles through each <svg> and each saturation value by index.  A nested for loop is then used to cycle through the hue values, incrementing them at each cycle by the 30-degree angle span of each wedge.

Finally, a different type of for loop is used to traverse through all the data keys in the lightness object.  The p variable gets set to the key string at each cycle (e.g., "outer", "middle", or "inner").  The key can then be used to access the value from the object, but it is also used directly to set the xlink:href attribute.

The hue value is used to rotate the wedge into place; because the wheels use a centered coordinate system, a simple rotation pivots the path around the center of the wheel.  The value of the transform attribute is a string representation of the transformation function, rotate(*h*); the parentheses are part of the string, not part of a JavaScript function!

The fill is set, as an inline style property, by creating a JavaScript string that contains the CSS hsl() function—again, including the parentheses.  The comma-separated list of parameters is created by grouping the values into an array; when an array is concatenated to a string, the string equivalent of each value is printed out, separated with commas.

The wedge piece is then added to the <svg> *before* any existing elements, so that it will be drawn underneath the black center and white rim declared in the markup.

The final block of code runs once per <svg> and creates the text labels.  They are inserted *after* all the other elements (“before” nothing, or null) so that the text is visible on top of the color wheel.

The introduction of HSL color functions was intended to make it easier to create sets of matching colors: the same hue in different lightness or saturation values, or the same lightness and saturation but opposite hues.  Nonetheless, the HSL model was also chosen because browsers can rapidly convert between HSL and RGB values, and it has its limitations.  The hue color wheel does not correspond well with the red-yellow-blue color wheels we know from mixing paint as a child.  The lightness model doesn’t reflect the perceived differences in brightness between equally-intense colors of different hues.

For these reasons, and because the history of computer hardware led to certain optimizations being standard in every graphical processor, a more complex color model is used in gradients and color blending.
Mixing and Matching
Your eye’s sensitivy to light does not correspond directly with the brightness of the pixels on your computer monitor.  You can detect changes between black and dark gray more easily than the equivalent change between white and light gray.  With colors, a fully-saturated blue light still seems much darker than the same intensity of red, and neither is as bright—from your mind’s perspective—as the full intensity of the green pixel components.

As a result, mixing equal proportions of red and blue (or blue and green) light doesn’t create a color that matches what our brain considers to be halfway in-between.  The same is true for nearly every other color combination, and also for shades of gray.

Computer monitors—especially the CRT monitors used when the web first became popular—add their own distortion to the effective brightness of every color value.  Without standardization, the same colors could look very different on different displays.

When blending between colors in computer graphics, most modern computer displays use the sRGB model.  It defines functions to convert RGB display colors into a standardized scale that reflects the perceived differences in brightness on a sRGB-compatible computer monitor.

The use of sRGB primarily affects the *blending* of colors.  Figure 3-5 compares the results when using the sRGB model to blend colors or shades of gray, versus calculating the intermediary colors as based on simple linear arithmetic (shown in the bottom gradient in each pair).

The sRGB model makes numerous assumptions about the type of equipment and lighting that will be used to view the image.  In addition, there is considerable variation from person to person in how colors are perceived—even before you factor in color blindness, a shortage of one or more of the light-sensitive pigments in the eyes.  You may not agree that the sRGB color blend is better than the linear model, let alone that it is the best possible blend.  But it has become the standard for computer graphics, particularly on the web.
Note
The sRGB model—and RGB color spaces in general—are less popular in print graphics because they cannot encode all colors that can be created on high-quality printers.  The SVG specifications include ways in which alternate color definitions may be used to ensure high-fidelity printed color results.  These alternate definitions use color profiles as defined by the International Color Consortium (ICC), a partnership of major digital graphics companies.  ICC color profiles are not implemented in web browsers.

The sRGB color model is used by default for CSS and most of SVG.  The SVG specifications define a property, color-interpolation, which could allow authors to switch to the mathematically simpler linear blending of colors (linearRGB mode).  This might be preferable if your graphic consists of many very bright, high-saturation colors of contrasting hues.  The sRGB blending model can generate unpleasantly dark in-between colors when blending contrasting bright colors.
Warning
None of the major web browsers support linearRGB blending at the time of writing.  Figure 3-5 was generated in the Apache Batik SVG viewer.  Even Batik only supports linear blending for gradients, and not for blending layered, partially-transparent colors.

The exception to the sRGB dominance is for filter calculations.  SVG filters allow you to directly manipulate the RGB color channels, and by default these calculations run in linearRGB mode.  There is much better browser support for setting the color-interpolation-filters to sRGB, to turn *on* sRGB adjustments in filters.

Both of the color interpolation options are style properties that can be set on individual elements or the SVG as a whole, using presentation attributes or CSS rules.  For gradients and filters, the property used is the one set on the gradient/filter element, not on the element being painted.

Certain filter operations, as well as SVG masking, use their own explicit means of compensating for the relative brightness of different colors.  These operations are based on the *luminance* of colors.  Luminance is a measure of brightness, and is distinct from the *lightness* value used in the HSL color functions.  Instead, luminance is adjusted based on the fact that a fully-saturated green or yellow is perceived as brighter than a fully-saturated red, which is nonetheless brighter than a fully-saturated blue.

The luminance adjustments used in these tools have a similar purpose to the sRGB adjustments, but they are not equivalent.  sRGB affects the scaling of grays, or of colors with different lightness values.  The luminance weighting only affects differences in hue.  Furthermore, the luminance factors create a linear scaling of each color channel, while sRGB defines a curved (*gamma*-adjusted) relationship between the color value and perceived intensity.

What does all this matter to you?  Mostly, it is a warning.  If calculating colors yourself as part of a script, or manipulating them with filters, be aware that simple mathematical averaging of R, G, and B values will create different results from colors created by the browser when blending gradients or combining partially transparent colors.  Similarly, changing color hues using filters (luminance-adjusted, and sometimes sRGB-adjusted) will result in different colors than changing the hue value in the hsl() color function.1

This section has been mostly theoretical.  The visible impact of the way colors are blended comes when working with transparency (the subject of Chapter 4) and with gradients (the chapters after that).

1 Thanks to Noah Blon for bringing my attention to the discrepancies between hsl() color functions and hue-rotate filter operations.