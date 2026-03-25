CHAPTER 15
Generating SVG
The previous chapters have described the major features of SVG.  All the examples have
been relatively modest and have been written in an ordinary text editor. For graphics
of any great complexity, though, few people will write SVG from scratch. Let’s face it:
almost nobody does this by hand. Instead, graphic designers will use some sort of
graphic tool that outputs SVG, and programmers will take existing raw data and convert
it to SVG with a script.
If you’re dealing with a graphic program’s output that is already in SVG format, you can
sit back and relax; all the heavy lifting has been done for you. If you ever take a look at
the SVG that it generated, it may be hard to read. Some programs, for example, may not
use groups (the <g> element) efficiently or they may not optimize paths. When you use
these programs, you are trading off the ease of generating SVG for the absolute control
you have when you write the entire file by hand. Hopefully, with a better understanding
of what’s going on “under the hood, ” you will be better able to write code to adapt and
interact with the graphics program’s output.
Generating SVG code from a data file is a trickier topic. The possibilities will depend
on what type of data you have to start with and what type of programming languages
you can work with.
One option for generating SVG is to build up an SVG document object model using the
methods presented in Chapter 14 . D3.js , which we alluded to briefly in “Using JavaScript
Libraries” on page 256 , is specifically designed for using data files to dynamically build
SVG charts and graphs in the web browser. Scott Murray’s Interactive Data Visualization
for the Web  (O’Reilly) is a good introduction for beginners. There are also many good
tutorials by the library’s original author, Mike Bostock, and others listed on the project’s
wiki page .
A different approach to dynamically generating an SVG file is to piece together the
markup using the string manipulation and file-writing methods of your favorite
265

programming language. The first section of this chapter outlines how you could use a
custom program to convert geographical mapping data that’s not in an XML format to
an SVG file.
If your data is already in XML format, you may just need to extract the pertinent data
and plug it into an SVG framework. In such a case, you can use tools that implement
Extensible Stylesheet Language Transformations (XSLT). XSLT is an XML syntax for
defining how to convert one XML file into another. The second part of this chapter
shows how to use XSLT to convert an XML-formatted aeronautical weather report to
SVG.
Converting Custom Data to SVG
If anyone lives a life that revolves around graphics display, it’s a mapmaker.  Cartogra‐
phers are finding XML markup in general and SVG in particular to be excellent vehicles
for putting data into a portable format. At present, though, much of the data that is
currently available is in custom or proprietary formats.
One such proprietary format was developed by Environmental Systems Research In‐
stitute for use by their ArcInfo Geographic Information System. Data created in this
system can be exported in an ASCII ungenerate  form.  Such a file contains a series of
polygon descriptions, followed by a line with the word END on it. Each polygon starts
with a line that consists of an integer polygon identification number and the x- and y-
coordinates of the polygon’s centroid. This line is followed by the x- and y-coordinates
of the polygon’s vertices, one vertex per line. A line with the word END on it marks the
end of the polygon. Here is a sample file:
         1      -0.122432044171565E+03       0.378635608621089E+02
      -0.122418712172884E+03       0.378527169597E+02
      -0.122434402770255E+03       0.378524342437443E+02
      -0.122443301934511E+03       0.378554484803880E+02
      -0.122446316168374E+03       0.378610463416856E+02
      -0.122438565286068E+03       0.378683666259093E+02
      -0.122418712172884E+03       0.378527169591107E+02
END
         2      -122.36                      37.82
      -122.378                     37.826
      -122.377                     37.831
      -122.370                     37.832
      -122.378                     378.826
END
END
Converting such a file to SVG is a simple task of inserting the coordinates into the points
attribute of <polygon>  elements. The only twist is that ARC/INFO stores data in Car‐
tesian coordinates, so we will have to flip the y-coordinates upside-down. The program
266 | Chapter 15: Generating SVG

we’ll describe will take two parameters: the input-file  name and the desired width  of the
resulting SVG graphic in pixels.
In addition to those parameters, we’re going to need a few global variables to keep track
of the data:
•lineBuffer , an array of tokens from parsing each line of the file into whitespace-
separated words and numbers.
•singlePolygon , an array of coordinates for the current polygon.
•polygonList , an array of points  strings for all the polygons.
•minX , minY , maxX , maxY , numbers representing the extreme coordinates observed so
far in each direction. The minimum variables should be initialized to equal positive
Infinity (or the maximum number possible in the programming language), and the
maximum variables to negative Infinity. That way, any finite number compared to
them will become the new maximum or minimum.
The following algorithm assumes that you have a way of reading the input file one line
at a time, and printing the results to an output stream or file. The exact ways to do so
will depend on your programming language, but nearly every language can do both:
1.Create a utility subroutine that grabs one token at a time from the input file:
function get_token()
{
  if ( lineBuffer  is empty ) // out of data?
  {
    read the next line from input-file ;
    get rid of leading and trailing whitespace;
    split on whitespace to create an array of tokens;
    put into lineBuffer ;
  }
  remove first item in lineBuffer  and return it
}
2.The main program (after validating and storing the input parameters and initial‐
izing the other variables), uses nested loops to process the data file. The outer loop
handles the start and end of each polygon, and the inner loop processes the pairs
of coordinates. Each polygon starts with an index number and ends with the token
END. The file as a whole also ends with an additional END. Read an index number,
initialize singlePolygon  as a coordinate array, read coordinates until you reach
END, add that coordinate array to the polygonList , and then repeat unless the next
token is also END:
open input-file ;
while ((polygonNumber = get_token())  is not "END" )
{
Converting Custom Data to SVG | 267

  singlePolygon  = empty list;
  while ((xCoord = get_token())  is not "END" )
  {
    yCoord = get_token();
    append ( xCoord, yCoord ) to singlePolygon ;
    // keep track of minimum and maximum coordinates
    minX = min(xCoord, minX);
    maxX = max(xCoord, maxX);
    minY = min(yCoord, minY);
    maxY = max(yCoord, maxY);
  }
  append singlePolygon  to polygonList ;
}
close input-file  ;
3.After processing the input file, polygonList  is an array of arrays of coordinate pairs,
and minX , minY , maxX , and maxY  hold the largest and smallest coordinates in each
dimension. But before you can start building your SVG, you need to determine an
appropriate scale to fit the x-range of the data into the pixel width requested by the
user. Initialize additional variables to keep track of the input and output
dimensions:
deltaX = maxX - minX;
deltaY = maxY - minY;
scale = width / deltaX;
height = deltaY * scale;
height = int(height + 0.5);  // round up to integer
4.The SVG file itself is constructed by printing markup to a file, substituting in the
values from the data:
open output;
print the following to output, replacing variables in {}
with their values:
  <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
  <svg width="  {width} " height="  {height} "
    viewBox="0 0  {deltaX}  {deltaY} "
    xmlns="http://www.w3.org/2000/svg">
  <title>Map constructed from  {input-file } </title>
  <g style="fill: none; stroke: black;">
5.Process the data arrays to create polygon objects:
polygonNumber = 1;
foreach singlePolygon  in polygonList
{
268 | Chapter 15: Generating SVG

  print ' <polyline id="poly  {polygonNumber } " points=" ';
  remove first set of coordinates in singlePolygon ;
  n = 0; //coordinate index
  foreach coordinate  in singlePolygon
  {
    if (n % 2 == 1)    // y-coordinate
    {
      coordinate = (maxY - coordinate); // invert y coords
    }
    else
    {
      coordinate = (coordinate - minX);
    }
    print the coordinate followed by a space;
  }
  // To avoid long lines, place only 8 coordinates per line
    n = (n + 1) % 8;
    if (n == 0) {  print a newline }
  }
  print ' /> ';     // close off the polyline
  polygonNumber++;
}
6.Close off open tags and close the file:
print ' </g>\n</svg>\n ';
close output;
This pseudocode algorithm is a simplified version of a real program written in Perl.
Running the Perl program with the data for the state of Michigan with an output width
of 250 pixels produces Figure 15-1 . Michigan was chosen because it requires several
polygons to draw, and its outline is more visually interesting than that of, say, Colorado.
This data came from the US Census Bureau Cartographic Boundary Files website.
Figure 15-1. Conversion from ARC/INFO ungenerate to SVG
Converting Custom Data to SVG | 269

Using XSLT to Convert XML Data to SVG
If your data is in XML format, then Extensible Stylesheet Language Transformations
(XSLT) may be your best choice for doing the conversion to SVG.
Defining the Task
This example uses XSLT to extract information from an XML file and insert it into an
SVG file. The source data is a weather report that is retrieved from http://w1.weath‐
er.gov/xml/current_obs/NNNN.xml , where NNNN  represents a four-letter weather
station  identifier. It is formatted as a Weather Observation Markup Format (OMF)
document, according to the National Oceanic and Atmospheric Organization’s defini‐
tion of the data format . Here is some sample data (edited for length) from station KSJC:
<current_observation  version= "1.0">
  <credit> NOAA's National Weather Service </credit>
  <credit_URL> http://weather.gov/ </credit_URL>
  <location> San Jose International Airport, CA </location>
  <station_id> KSJC</station_id>
  <latitude> 37.37</latitude>
  <longitude> -121.93</longitude>
  <observation_time> Last Updated on Jul 15 2014, 7:53 am PDT
    </observation_time>
  <observation_time_rfc822> Tue, 15 Jul 2014 07:53:00 -0700
    </observation_time_rfc822>
  <weather> Overcast </weather>
  <temperature_string> 62.0 F (16.7 C) </temperature_string>
  <temp_f> 62.0</temp_f>
  <temp_c> 16.7</temp_c>
  <wind_string> West at 5.8 MPH (5 KT) </wind_string>
  <wind_dir> West</wind_dir>
  <wind_degrees> 290</wind_degrees>
  <wind_mph> 5.8</wind_mph>
  <visibility_mi> 10.00</visibility_mi>
  <copyright_url> http://weather.gov/disclaimer.html </copyright_url>
</current_observation>
The objective is to extract the reporting station, the date and time, temperature, wind
speed and direction, and visibility from the report. The data will be filled into the graphic
template of Figure 15-2 .
270 | Chapter 15: Generating SVG

Figure 15-2. Graphic weather template
The elements we’re interested in are listed here, along with the plan for displaying them
in the final graphic:
<observation_time_rfc822>
In the final graphic, the date and time will be represented in text, and the time will
also be shown on an analog clock. The color of the clock face will be light yellow to
indicate hours between 6 a.m. and 6 p.m., and light blue for evening and night hours.
<station_id>
The reporting station’s call letters. The final graphic will represent this as text.
<temp_c>
The air temperature in degrees Celsius. This will be displayed by coloring in the
thermometer to the appropriate level. If the temperature is greater than 0, the col‐
oring will be red; if less than or equal to 0, it will be blue.
<wind_degrees>
The direction is measured in degrees; 0 indicates wind blowing from true north,
and 270 indicates wind from the west. This will be represented by a line on the
compass.
<wind_mph>
The wind speed is expressed in miles per hour, which will be converted to meters
per second.
<wind_gust_mph>
If there are wind gusts, this element will give the speed in miles per hour, which will
also be converted to meters per second.
<visibility_mi>
Surface visibility in miles, which will be converted to kilometers. The final graphic
will represent this by filling in a horizontal bar. Any visibility above 40 kilometers
will be presumed to be unlimited.
Using XSLT to Convert XML Data to SVG | 271

How XSLT Works
To convert an OMF source file to its destination SVG format, we will create a list of
specifications that tells which elements and attributes in OMF are of interest. These
specifications will then detail what SVG elements to generate whenever the processor
encounters an item of interest. If you were asking a human to do the transformation by
hand, you could write out an English language description:
1.Begin a new SVG document by typing this:
<!DOCTYPE svg PUBLIC "-//W3C/DTD SVG 1.0//EN",
 "http://www.w3.org/TR/2001/REC-SVG-20010904/DTD/svg10.dtd">
2.Go through the source document. As you find each element, look for instructions
on how to process it.
3.To process the <current_observation>  element, add the following code to your
file, and then process any child elements as specified in comments:
<svg viewBox= "0 0 350 200"  height="200" width="350">
    <!-- process any child elements from <current_observation> -->
</svg>
4.To process a <station_id>  element, add this code and fill in the blanks:
<text font-size= "10pt" x="10" y="20">
    <!-- fill in the value of the element's content -->
</text>
5.To process a <temp_c>  element, use its content when following the instructions for
“how to draw a thermometer. ”
6.To process a <wind_dir>  element, use its contents as you follow the instructions
for “how to draw a wind compass. ”
Similarly, for each other element type, you would indicate where to find the instructions
to follow. Then you’d have supplemental instructions like these:
•To draw a thermometer:
—Calculate the height of the bar as 50 minus the value you got from the caller.
—Determine the appropriate color ( red or blue ) based on whether or not the value
is greater than 0.
—Insert those values where you see the italicized text in the following:
<path
  d = "M 25 height 25 90
  A 10 10 0 1 0 35 90
  L 35 height Z"
  style="stroke: none; fill: color ;"/>
<path
272 | Chapter 15: Generating SVG

  d= "M 25 0 25 90 A 10 10 0 1 0 35 90 L 35 0 Z"
  style="stroke: black; fill: none;"/>
There would be equally detailed instructions for “how to draw a wind compass” and all
the other elements.
Rather than writing the specifications in English and handing them to a human to
perform, you write the specifications in the XSLT markup format. Y ou can then hand
the XSLT file, along with the XML file, to an XLST processor, and it will process elements
and fill in the blanks to produce an output SVG file.
Here is a quick English-to-XSLT translation guide:
English XSLT
Create an output document of a given type <xsl:output method="xml"
  doctype-public="..." doctype-system="...">
Process an element  element <xsl:template match="element">
  <!-- output to produce -->
</xsl:template>
Process any items  within the current element <xsl:apply-templates select="items"/>
Fill in the value of an item <xsl:value-of select="item"/>
Use the value of an item  as a variable named var <xsl:variable name="var">
  <!-- instructions to produce item's value -->
</xsl:variable>
Call another template named some-name , and give it
a parameter  with some-value<xsl:call-template name="some-name">
  <xsl:with-param name="parameter"
    select="some-value"/>
</xsl:call-template>
Add the following content if the data passes a test <xsl:if test="some-test">
   <!-- content -->
</xsl:if>
Add  content  if data  passes  a test;  otherwise,  add  other
content<xsl:choose>
   <xsl:when test="some-test">
      <!-- content -->
   </xsl:when>
   <xsl:otherwise>
      <!-- other content -->
   </xsl:otherwise>
 </xsl:choose>
Developing an XSL Stylesheet
We’ll add details as we proceed, but this is more than enough to start.  The XSLT file
begins like this, and, for the purposes of this example, is stored in a file named
weather.xsl :
Using XSLT to Convert XML Data to SVG | 273

<xsl:stylesheet  version= "1.0"
  xmlns:xsl= "http://www.w3.org/1999/XSL/Transform"
  xmlns:xlink= "http://www.w3.org/1999/xlink"
  xmlns="http://www.w3.org/2000/svg" >
<xsl:output  method="xml" indent="yes"
  doctype-public= "-//W3C//DTD SVG 1.0//EN"
  doctype-system=
    "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd" />    
<xsl:template  match="current_observation" >     
<svg width="350" height="200" viewBox= "0 0 350 200"
  xmlns="http://www.w3.org/2000/svg" >
  <g style="font-family: sans-serif" >
  <!-- Process all child elements -->
  <xsl:apply-templates  />     
  </g>
</svg>
</xsl:template>
The <xsl:output>  specifies  that the output  will be an XML  file and that it should
be indented nicely. It also generates the appropriate <!DOCTYPE …>  instruction.
<xsl:template>  directs  the XSLT  processor  to generate  the specified  output
whenever it encounters a <current_observation>  element. This template will
be called  only  once,  because  there’s  only  one such  element  in the source
document.  It creates  the outermost  <svg>  element  and a <g> element  for
later use.
After  outputting  the <svg>  and <g>, <xsl:apply-templates>  directs  the
processor to find any child elements and generate whatever is specified by their
<xsl:template>  elements.
This is the markup to process the station_id  element:
<xsl:template  match="station_id" >
 <text font-size= "10pt" x="10" y="20">
    <xsl:value-of  select="."/>
  </text>
</xsl:template>
The <xsl:value-of>  inserts the value of the selected item. In this case, the . means “the
current element. ”
274 | Chapter 15: Generating SVG

So far, this example uses only element names as the values of a match
or select . In reality,  you can put any XPath  expression  as a value.
XPath  is a notation  that lets you select  parts  of an XML  document
with  extreme  precision.  For example,  while  processing  an XHTML
document,  you could  select  only  the odd <td>  elements  that are with‐
in <tr>  elements that have a title  attribute.
While it would be possible to output all the relevant SVG for the temperature within
one <xsl:template> , a modular approach is easier to read and maintain. XSLT lets you
create templates that act somewhat like functions; they don’t correspond to any element
in the source document, but you may explicitly call them by name and pass parameters
to them. Here is the code to draw the thermometer:
<xsl:template  match="temp_c" >
  <xsl:call-template  name="draw-thermometer" >
    <xsl:with-param  name="t" select="."/>
  </xsl:call-template>
</xsl:template>
If the value of a parameter is an attribute value or the content of an element, the easiest
way to set it is with a select . Another way to set the value is to put the content between
a beginning and ending tag.
Now you can write the template for draw-thermometer . The passed-in parameter de‐
termines the height to fill the thermometer and whether the thermometer should be
filled with red or blue. Let’s build this up in stages. First, extract the parameter and draw
the parts that are static:
<xsl:template  name="draw-thermometer" >
  <xsl:param  name="t" select="0"/>
  <g id="thermometer"  transform= "translate(10, 40)" >
    <path id="thermometer-path"  stroke="black" fill="none"
      d= "M 25 0 25 90 A 10 10 0 1 0 35 90 L 35 0 Z" />
    <g id="thermometer-text"  font-size= "8pt" font-family= "sans-serif" >
      <text x="20" y="95" text-anchor= "end">-40</text>
      <text x="20" y="55" text-anchor= "end">0</text>
      <text x="20" y="5" text-anchor= "end">50</text>
      <text x="10" y="110" text-anchor= "end">C</text>
      <text x="40" y="95">-40</text>
      <text x="40" y="55">32</text>
      <text x="40" y="5">120</text>
      <text x="50" y="110">F</text>
      <text x="30" y="130" text-anchor= "middle" >Temp.</text>
  </g>
</g>
</xsl:template>
Using XSLT to Convert XML Data to SVG | 275

The <xsl:param>  element lets you provide a default value (in this case, 0) if no parameter
is passed in.
Next, add the code to display the temperature as text. If there was no temperature
present, display the value N/A. Notice that the parameter name is t, but to access its
contents, you must say $t. This code goes inside the <g id="thermometer-text"> :
<text x="30" y="145" text-anchor= "middle" >
  <xsl:choose>
    <xsl:when  test="$t != ''" >
      <xsl:value-of  select="round($t)" />&#176;C /
      <xsl:value-of  select="round($t div 5 * 9 + 32)" />&#176;F
    </xsl:when>
    <xsl:otherwise> N/A</xsl:otherwise>
  </xsl:choose>
</text>
The text is set conditionally with the <xsl:choose>  element, which contains one or
more <xsl:when>  elements. The first one whose test  succeeds is the one whose output
goes into the final document. The <xsl:otherwise>  element is a catch-all in case all the
preceding tests fail.
The formula  for conversion  of Celsius  to Fahrenheit  uses  div for
division; this is because the forward slash is already used in XPath to
separate levels of element nesting.
The next step is to fill the thermometer. This should be done only if the value of the t
parameter is not the empty string. The following code uses <xsl:variable>  to create
a variable named tint  and sets its value to either red or blue , depending on whether
the temperature is above 0 degrees Celsius or not. Variables in XSL are single-
assignment . Every time the template is called, the variable is set to an initial value, but
for the duration of the template, it cannot be changed further. Place this after the closing
</g>  tag of the thermometer-text  group:
<xsl:if test="$t != ''" >
  <xsl:variable  name="tint">
  <xsl:choose>
    <xsl:when  test="$t &gt; 0" >red</xsl:when>
    <xsl:otherwise> blue</xsl:otherwise>
  </xsl:choose>
  </xsl:variable>
  <!-- remainder of code -->
</xsl:if>
276 | Chapter 15: Generating SVG

Again, the code uses <xsl:choose>  to conditionally set the variable. The test  uses the
entity reference &gt;  for a greater-than symbol to avoid problems with some XSLT
processors; if you ever want to produce a less-than symbol, it must  be written as &lt; .
Here is the remainder of the code for filling the thermometer:
<!-- "fill" the thermometer by drawing a solid
  rectangle and clipping it to the shape of
  the thermometer -->
<xsl:variable  name="h">
  <xsl:choose>
    <xsl:when  test="$t &lt; -55" >
      <xsl:value-of  select="105"/>
    </xsl:when>
    <xsl:when  test="$t &gt; 50" >
      <xsl:value-of  select="0"/>
    </xsl:when>
    <xsl:otherwise>
      <xsl:value-of  select="50 - $t" />
    </xsl:otherwise>
  </xsl:choose>
</xsl:variable>
<clipPath  id="thermoclip" >
  <use xlink:href= "#thermometer-path" />
</clipPath>
<path d="M 10 {$h} h40 V 120 h-40 Z"
  fill="{$tint}"  clip-path= "url(#thermoclip)" />
The <xsl:choose > in the preceding code has two <xsl:when>  clauses; they limit the
height of the “mercury” in case temperatures go above or below the thermometer’s
limits. The <xsl:otherwise>  clause sets the height for all other in-range temperatures.
When referring to parameters or variables in the values of attributes of the output
document, as in the final <path>  element, you must enclose them with curly braces.
This would be a good time to test the transformation so far. Before you can test, you
have to add an empty template to handle text nodes. XSLT processors are set up with
default templates to ensure that they will visit all the elements and text in the source
document. The default behavior is to send the text within elements directly to the des‐
tination document. In this transformation, you want to throw away any text that you
don’t specifically want to process, so there’s an empty template for text nodes; they will
not appear in the SVG file. Finally, you need the closing </xsl:stylesheet>  tag:
<xsl:template  match="text()" />
</xsl:stylesheet>
Invoke your XSLT processor on the XML file that contains the weather report. The
resulting graphic, Figure 15-3 , shows the station name and the thermometer. If you
Using XSLT to Convert XML Data to SVG | 277

don’t have a standalone XSLT processor, you can add the following lines at the top of
the XML file:
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="weather.xsl"?>
Then open the file in your browser, and it will do the transformation for you.
Figure 15-3. XSL-generated SVG file showing thermometer
Y ou have seen that XSLT can do simple arithmetic; it can also do a reasonable amount
of string manipulation. Here is the XSLT to display the day and time. It uses the
substring  function to extract the necessary information.
<xsl:template  match="observation_time_rfc822" >
  <xsl:variable  name="time" select="."/> 
  <text font-size= "10pt" x="345" y="20" text-anchor= "end">
    <xsl:value-of  select="substring($time, 6, 11)" /> 
  </text>
  <xsl:call-template  name="draw-time-and-clock" > 
    <xsl:with-param  name="hour"
      select="number(substring($time, 18, 2))" />
    <xsl:with-param  name="minute"
      select="number(substring($time, 21, 2))" />
  </xsl:call-template>
</xsl:template>
For convenience,  store  the string  in a variable  rather  than  having  to do many
<xsl:value-of> s.
The substring()  function  needs  the string,  starting  character  index,  and
number  of characters  to extract.  The first character  of the string  is index  number
1, not 0, as in many other programming languages.
Pass  the hour  and minute  to a named  template  to do the heavy  lifting.  The
number()  function converts its string parameter to a true numeric value.
278 | Chapter 15: Generating SVG

And here is the template that draws the clock face and displays the time as text (the only
new construct here is the format-number()  function):
<xsl:template  name="draw-time-and-clock" >
  <xsl:param  name="hour">0</xsl:param>
  <xsl:param  name="minute" >0</xsl:param>
  <!-- clock face is light yellow from 6 a.m. to 6 p.m.,
       otherwise light blue -->
  <xsl:variable  name="tint">
    <xsl:choose>
      <xsl:when  test="$hour &gt;= 6 and $hour &lt; 18"
           >#ffffcc</xsl:when>
      <xsl:otherwise> #ccccff</xsl:otherwise>
    </xsl:choose>
  </xsl:variable>
  <!-- calculate angles for hour and minute hand
    of analog clock -->
  <xsl:variable  name="hourAngle"
    select="(30 * ($hour mod 12 + $minute div 60)) - 90" />
  <xsl:variable  name="minuteAngle"
    select="($minute * 6) - 90" />
  <text font-size= "10pt" x="345" y="40" text-anchor= "end">
    <xsl:value-of  select="format-number($hour,00)" /> 
    <xsl:text> :</xsl:text>  
    <xsl:value-of  select="format-number($minute,00)" />
  </text>
  <g id="clock" transform= "translate(255, 30)" >
    <circle cx="20" cy="20" r="20" fill="{$tint}"
            stroke="black"/>
    <line transform= "rotate({$minuteAngle}, 20, 20)"
      x1="20" y1="20" x2="38" y2="20" stroke="black"/>
    <line transform= "rotate({$hourAngle}, 20, 20)"
      x1="20" y1="20" x2="33" y2="20" stroke="black"/>
  </g>
</xsl:template>
The format-number($hour, 00) ensures  that the output  will have  a leading  zero
if it is less than two digits long.
The <xsl:text>  element places its contents, which must be pure text, into the
output  document  verbatim.  Using  <xsl:text>  helps  avoid  problems  with
whitespace; if I had not used it, the newline and indentation would have made
its way into the resultant  SVG  <text>  element,  which  would  have  produced  extra
space around the colon in the final graphic.
Here is the markup to draw the wind speed indicator:
Using XSLT to Convert XML Data to SVG | 279

<xsl:template  match="wind_degrees" >
  <xsl:call-template  name="draw-wind" >
    <xsl:with-param  name="dir" select="number(.)" />
    <xsl:with-param  name="speed"
      select="number(../wind_mph) * 1609.344 div 3600" /> 
    <xsl:with-param  name="gust"
      select="number(following-sibling::wind_gust_mph) *
        1609.344 div 3600" />
  </xsl:call-template>
</xsl:template>
<xsl:template  name="draw-wind" >
  <xsl:param  name="dir">0</xsl:param>
  <xsl:param  name="speed">0</xsl:param>
  <xsl:param  name="gust">0</xsl:param>
  <g id="compass"  font-size= "8pt" font-family= "sans-serif"
    transform= "translate(110, 70)" >
    <circle cx="40" cy="40" r="30" stroke="black" fill="none"/>
    <!-- tick marks at cardinal directions -->
    <path stroke="black" fill="none"
      d= "M 40 10 L 40 14
      M 70 40 L 66 40
      M 40 70 L 40 66
      M 10 40 L 14 40" />
    <xsl:if test="$speed &gt;= 0" >
      <path d="M 40 40 h 25"
        fill="none" stroke="black"
        transform= "rotate({$dir - 90},40,40)" /> 
    </xsl:if>
    <text x="40" y="9" text-anchor= "middle" >N</text>
    <text x="73" y="44">E</text>
    <text x="40" y="80" text-anchor= "middle" >S</text>
    <text x="8" y="44" text-anchor= "end">W</text>
    <text x="40" y="100" text-anchor= "middle" >Wind (m/sec) </text>
    <text x="40" y="115" text-anchor= "middle" > 
      <xsl:choose>
        <xsl:when  test="$speed &gt;= 0" >
          <xsl:value-of  select="format-number($speed,  0.)"/>
        </xsl:when>
        <xsl:otherwise> N/A</xsl:otherwise>
      </xsl:choose>
      <xsl:if test="$gust &gt; 0" >
        <xsl:text>  - </xsl:text>
        <xsl:value-of  select="format-number($gust,  0.)"/>
      </xsl:if>
    </text>
  </g>
</xsl:template>
280 | Chapter 15: Generating SVG

Here  is a more  complex  XPath  expression.  .. means  “this  node’s  parent,”
so ../wind_mph  will find all the <wind_mph>  elements  that are children  of the
parent of the <wind_degree>  element (in this XML file, there is only one such
element). The expression that gets the wind gust (if any) uses the more verbose
following-sibling::  specification.
The NOAA  specification  says that a true north  wind  is 360 degrees,  with  0
degrees implying no wind. Y ou have to subtract 90 degrees, because “north” is
–90 degrees in SVG.
This logic for displaying the wind speed (and gusts) as text works even if there
is no <wind_mph>  or <wind_gust_mph>  element in the weather report. When a
nonexistent element’s content is converted to a number, the result is NaN (Not a
Number).  When  you do any comparison  with  NaN, the result  is always  false.
Thus, if there is no <wind_mph> , the resulting text is N/A, and when there is no
<wind_gust_mph> , the dash and second number are never output.
Here are the XSLT commands to draw the visibility bar. The first template converts the
visibility to kilometers as it passes it to the second template. The visibility bar is 100
pixels wide, so any visibility greater than 40 km is set to 100; anything less is scaled:
<xsl:template  match="visibility_mi" >
  <xsl:call-template  name="draw-visibility" >
    <xsl:with-param  name="v" select="number(.) * 1.609344" /> 
  </xsl:call-template>
</xsl:template>
<xsl:template  name="draw-visibility" >
  <xsl:param  name="v">0</xsl:param>
  <g id="visbar"  transform= "translate(220,110)"
    font-size= "8pt" text-anchor= "middle" >
  <!-- fill in the rectangle if there is a visibility value -->
  <xsl:if test="$v &gt;= 0" >
    <xsl:variable  name="width"> 
      <xsl:choose>
      <xsl:when  test="$v &gt; 40" >100</xsl:when>
      <xsl:otherwise>
        <xsl:value-of  select="$v * 100.0 div 40.0" />
      </xsl:otherwise>
      </xsl:choose>
    </xsl:variable>
    <rect style="fill:green; stroke:none;"
      x="0" y="0" width="{$width}"  height="20"/>
  </xsl:if>
  <rect x="0" y="0" width="100" height="20"
        style="stroke:black; fill:none" />
Using XSLT to Convert XML Data to SVG | 281

  <path fill="none" stroke="black"
    d="M 25 20 L 25 25 M 50 20 L 50 25 M 75 20 L 75 25" />
  <text x="0" y="35">0</text>
  <text x="25" y="35">10</text>
  <text x="50" y="35">20</text>
  <text x="75" y="35">30</text>
  <text x="100" y="35">40+</text>
  <text x="50" y="60">
    Visibility (km)
  </text>
  <text x="50" y="75">
    <xsl:choose>
       <xsl:when  test="$v &gt;= 0" >
         <xsl:value-of  select="format-number($v,'0.###')" /> 
      </xsl:when>
      <xsl:otherwise> N/A</xsl:otherwise>
    </xsl:choose>
  </text>
</g>
</xsl:template>
The first template  passes  the visibility,  converted  to kilometers,  to the next
template.
The visibility bar is 100 pixels wide. Visibility greater than 40 km is set to 100;
anything less than that is scaled to 100 pixels.
This format string will print a leading zero before the decimal point and three
digits after the decimal point.
Putting this all together produces Figure 15-4 .
This is only a small sample of what you can do with XSLT. For more information, get
XSLT  by Doug Tidwell (O’Reilly). Chapter 9 of that marvelous book also contains an
example of using XSLT to generate SVG from an XML file. If you’re serious about ma‐
nipulating XML, you would be well advised to have that book on your shelf. 
282 | Chapter 15: Generating SVG

Figure 15-4. XSLT-generated SVG file showing complete data
Using XSLT to Convert XML Data to SVG | 283

APPENDIX A
The XML You Need for SVG
The purpose of this appendix is to introduce you to XML. A knowledge of XML is
essential if you wish to write SVG documents directly rather than having them generated
by some graphics utility.
If you’re already acquainted with XML, you don’t need to read this appendix. If not,
read on. The general overview of XML given in this appendix should be more than
sufficient to enable you to work with the SVG documents that you will create. For further
information about XML, the O’Reilly books Learning XML  by Erik T. Ray and XML in
a Nutshell  by Elliotte Rusty Harold and W . Scott Means are invaluable guides.
Note that this appendix makes frequent reference to the formal XML 1.0 specification,
which can be used for further investigation of topics that fall outside the scope of SVG.
Readers are also directed to Tim Bray’s “ Annotated XML Specification” , which provides
an illuminating explanation of the XML 1.0 specification, and Norm Walsh’s technical
introduction to XML .
Y ou may have noticed that these are not recent publications. Don’t be surprised; XML
is a solid, long-established standard.
What Is XML?
XML, the Extensible Markup Language, is an Internet-friendly format for data and
documents, invented by the World Wide Web Consortium (W3C). The Markup  denotes
a way of expressing the structure of a document within the document itself. XML has
its roots in a markup language called SGML (Standard Generalized Markup Language),
which is used in publishing and shares this heritage with HTML. XML was created to
do for machine-readable documents on the Web what HTML did for human-readable
documents—that is, provide a commonly agreed-upon syntax so that processing the
underlying format becomes a commodity and documents are made accessible to
all users.
285

1.To clarify XML ’s relationship with SGML: XML is an SGML subset . By contrast, HTML is an SGML applica‐
tion. SVG uses XML to express its operations and thus is an XML application .Unlike HTML, though, XML comes with very little predefined. HTML developers are
accustomed both to the notion of using angle brackets < > for denoting elements (i.e.,
syntax) , and also to the set of element names themselves (i.e., head, body, etc.). XML
shares only the former feature, the notion of using angle brackets for denoting elements.
Unlike HTML, XML has no predefined elements, but is merely a set of rules that lets
you write other languages like HTML.1 Because XML defines so little, it is easy for
everyone to agree to use the XML syntax, and then to build applications on top of it. It’s
like agreeing to use a particular alphabet and set of punctuation symbols, but not saying
which language to use. However, if you’re coming to XML from an HTML background,
then prepare yourself for the shock of having to choose what to call your tags!
Knowing that XML ’s roots lie with SGML should help you understand some of XML ’s
features and design decisions. Note that, although SGML is essentially a document-
centric technology, XML ’s functionality also extends to data-centric applications, in‐
cluding SVG. Commonly, data-centric applications do not need all the flexibility and
expressiveness that XML provides and limit themselves to employing only a subset of
XML ’s functionality.
Anatomy of an XML Document
The best way to explain how an XML document is composed is to present one. The
following example shows an XML document you might use to describe authors:
<?xml version="1.0" encoding="us-ascii"?>
<authors>
    <person id="lear">
        <name>Edward Lear </name>
        <nationality> British</nationality>
    </person>
    <person id="asimov" >
        <name>Isaac Asimov </name>
        <nationality> American </nationality>
    </person>
    <person id="mysteryperson" />
</authors>
The first line of the document is known as the XML declaration . This tells a processing
application which version of XML you are using (the version indicator is mandatory)
and which character encoding you have used for the document. In the previous example,
the document is encoded in ASCII. (The significance of character encoding is covered
later in this chapter.) If the XML declaration is omitted, a processor will make certain
assumptions about your document. In particular, it will expect it to be encoded in
UTF-8, an encoding of the Unicode character set. However, it is best to use the XML
286 | Appendix A: The XML You Need for SVG

declaration wherever possible, both to avoid confusion over the character encoding and
to indicate to processors which version of XML you’re using.
Elements and Attributes
The second line of the example begins  an element , which has been named authors . The
contents  of that element include everything between the right angle bracket ( >) in
<authors>  and the left angle bracket ( <) in </authors> . The actual syntactic constructs
<authors>  and </authors>  are often referred to as the element start tag  and end tag ,
respectively. Do not confuse tags with elements! Note that elements may include other
elements, as well as text. An XML document must contain exactly one root element ,
which contains all other content within the document. The name of the root element
defines the type of the XML document.
Elements that contain both text and other elements simultaneously are classified as
mixed content . The SVG <text>  element is such an element; it can contain text and
<tspan>  elements.
The sample authors  document uses elements named person  to describe the authors
themselves. Each person element has an attribute  named id. Unlike elements, attributes
can contain only textual content. Their values must be surrounded by quotes. Either
single quotes ( ') or double quotes ( ") may be used, as long as you use the same kind of
closing quote as the opening one.
Within XML documents, attributes are frequently used for metadata  (i.e., data about
data)—describing properties of the element’s contents.  This is the case in our example,
where id contains a unique identifier for the person being described.
As far as XML is concerned, it does not matter in what order attributes are presented
in the element start tag. For example, these two elements contain exactly the same in‐
formation as far as an XML 1.0 conformant processing application is concerned:
<animal name="dog" legs="4"/>
<animal legs="4" name="dog"/>
On the other hand, the information presented to an application by an XML processor
on reading the following two lines will be different for each animal element because the
ordering of elements is significant:
<animal><name> dog</name><legs> 4</legs></animal>
<animal><legs> 4</legs><name> dog</name></animal>
XML treats a set of attributes like a bunch of stuff in a bag—there is no implicit ordering
—while elements are treated like items on a list, where ordering matters.
New XML developers frequently ask when it is best to use attributes to represent in‐
formation and when it is best to use elements. As you can see from the authors example,
Anatomy of an XML Document | 287

2.Actually, a name may also contain a colon, but the colon is used to delimit a namespace prefix  and is not
available for arbitrary use. For more information, see Tim Bray’s “XML Namespaces by Example. ”if order is important to you, then elements are a good choice. In general, there is no
hard-and-fast “best practice” for choosing whether to use attributes or elements.
The final author described in our document has no information available. All we know
about this person is his or her ID, mysteryperson . The document uses the XML shortcut
syntax for an empty element. The following is a reasonable alternative:
<person id="mysteryperson" ></person>
Name Syntax
XML 1.0 has certain rules about element and attribute names. In particular:
•Names are case-sensitive: e.g., <person/>  is not the same as <Person/> .
•Names beginning with xml (in any permutation of uppercase or lowercase) are
reserved for use by XML 1.0 and its companion specifications.
•A name must start with a letter or an underscore, not a digit, and may continue
with any letter, digit, underscore, or period.2
A precise description of names can be found in section 2.3 of the XML 1.0 specifica‐
tion. The rules for names in XML 1.1 are slightly different, primarily with regard to
Unicode characters. SVG uses the XML 1.0 rules.
Well-Formed
An XML document that conforms to the rules of XML syntax is known as well-
formed . At its most basic level, well-formedness means that elements should be properly
matched, and all opened elements should be closed. A formal definition of well-
formedness can be found in section 2.1 of the XML 1.0 specification . Table A-1  shows
some XML documents that are not well-formed.
288 | Appendix A: The XML You Need for SVG

3.A discussion of processing instructions (PIs)  is outside the scope of this book. For more information on PIs,
see section 2.6 of the XML 1.0 specification, at http://www.w3.org/TR/REC-xml#sec-pi .Table A-1. Examples of poorly formed XML documents
Document Reason why it’s not well-formed
<foo>
  <bar>
  </foo>
</bar>The elements are not properly nested because foo  is closed while inside its child
element bar.
<foo>
  <bar>
</foo>The bar  element was not closed before its parent, foo , was closed.
<foo baz>
</foo>The baz  attribute has no value. While this is permissible in HTML (e.g.,
<table border> ), it is forbidden in XML.
<foo baz=23>
</foo>The baz  attribute  value,  23, has no surrounding  quotes.  Unlike  HTML,  all attribute
values must be quoted in XML.
Comments
As in HTML, it is possible to include comments  within XML documents. XML com‐
ments are intended to be read only by people. With HTML, developers have occasionally
employed comments to add application-specific functionality. For example, the server-
side include functionality of most web servers uses instructions embedded in HTML
comments. XML provides other means of indicating application-processing instruc‐
tions,3 so comments should not be used for any purpose other than those for which they
were intended.
The start of a comment is indicated with <!-- , and the end of the comment with -->.
Any sequence of characters, aside from the string --, may appear within a comment.
Comments tend to be used more in XML documents intended for human consumption
than those intended for machine consumption. The <desc>  and <title>  elements in
SVG obviate much of the need for comments.
Entity References
Another feature of XML that is occasionally useful when writing SVG  documents is the
mechanism for escaping  characters.
Because some characters have special significance in XML, there needs to be a way to
represent them. For example, in some cases the < symbol might really be intended to
mean less than  rather than to signal the start of an element name. Clearly, just inserting
Anatomy of an XML Document | 289

the character without any escaping mechanism would result in a poorly formed docu‐
ment because a processing application would assume you were starting another element.
Another instance of this problem is needing to include both double quotes and single
quotes simultaneously in an attribute’s value. Here’s an example that illustrates both
these difficulties:
<badDoc>
  <para>
    I'd really like to use the < character
  </para>
  <note title="On the proper 'use' of the "  character" />
</badDoc>
XML avoids this problem by the use of the predefined entity reference . The word enti‐
ty in the context of XML simply means a unit of content. The term entity reference  means
just that, a symbolic way of referring to a certain unit of content. XML predefines entities
for the following symbols: left angle bracket ( <), right angle bracket ( >), apostrophe ( '),
double quote ( "), and ampersand ( &).
An entity reference is introduced with an ampersand ( &), which is followed by a name
(using the word name  in its formal sense, as defined by the XML 1.0 specification), and
terminated with a semicolon ( ;). Table A-2  shows how the five predefined entities can
be used within an XML document.
Table A-2. Predefined entity references in XML 1.0
Literal character Entity reference
< &lt;
> &gt;
' &apos;
" &quot;
& &amp;
Here’s the problematic document revised to use entity references:
<badDoc>
  <para>
    I'd really like to use the &lt; character
  </para>
  <note title="On the proper &apos;use&apos;
                of the &quot; character" />
</badDoc>
Being able to use the predefined entities is all you need for SVG; in general, entities are
provided as a convenience for human-created XML. XML 1.0 allows you to define your
own entities and use entity references as “shortcuts” in your document. Section 4 of the
XML 1.0 specification  describes the use of entities.
290 | Appendix A: The XML You Need for SVG

4.Y ou can obtain charts of all these characters online by visiting http://www.unicode.org/charts/ .
5.An octet  is a string of 8 binary digits, or bits. A byte is commonly, but not always, considered the same thing
as an octet.Character References
Y ou are likely to find character references  in the context of SVG documents. Character
references allow you to denote a character by its numeric position in the Unicode char‐
acter set (this position is known as its code point ). Table A-3  contains  a few examples
that illustrate the syntax.
Table A-3. Example character references in UTF-8
Actual character Character reference
1 &#49;
A &#65;
Ñ &#xD1;
® &#xAE;
Note that the code point can be expressed in decimal or, with the use of x as a prefix, in
hexadecimal.
Character Encodings
The subject of character encodings is frequently a mysterious one for developers.  Most
code tends to be written for one computing platform and, normally, to run within one
organization. Although the Internet is changing things quickly, most of us have never
had cause to think too deeply about internationalization.
XML, designed to be an Internet-friendly syntax for information exchange, has inter‐
nationalization at its very core. One of the basic requirements for XML processors is
that they support the Unicode standard character encoding. Unicode attempts to in‐
clude the requirements of all the world’s languages within one character set. Conse‐
quently, it is very large!
Unicode Encoding Schemes
Unicode 3.0 has over 57,700 code points, each of  which corresponds to a character.4 If
one were to express a Unicode string by using the position of each character in the
character set as its encoding (in the same way as ASCII does), expressing the whole
range of characters would require four octets5 for each character. Clearly, if a document
is written in 100 percent American English, it will be four times larger than required—
Character Encodings | 291

all the characters in ASCII fitting into a 7-bit representation. This places a strain both
on storage space and on memory requirements for processing applications.
Fortunately, two encoding schemes for Unicode alleviate this problem: UTF-8  and
UTF-16 . As you might guess from their names, applications can process documents in
these encodings in 8- or 16-bit segments at a time. When code points are required in a
document that cannot be represented by one chunk, a bit pattern is used that indicates
that the following chunk is required to calculate the desired code point. In UTF-8, this
is denoted by the most significant bit of the first octet being set to 1.
This scheme means that UTF-8 is a highly efficient encoding for representing languages
using Latin alphabets, such as English. All of the ASCII character set is represented
natively in UTF-8—an ASCII-only document and its equivalent in UTF-8 are byte-for-
byte identical.
This knowledge will also help you debug encoding errors. One frequent error arises
because of the fact that ASCII is a proper subset of UTF-8—programmers get used to
this fact and produce UTF-8 documents, but use them as if they were ASCII. Things
start to go awry when the XML parser processes a document containing, for example,
characters such as Á. Because this character cannot be represented using only one octet
in UTF-8, this produces a two-octet sequence in the output document; in a non-Unicode
viewer or text editor, it looks like a couple of characters of garbage.
Other Character Encodings
Unicode, in the context of computing history, is a relatively new invention.  Native op‐
erating system support for Unicode is by no means universal. For instance, older systems
like Windows 95 and 98 do not have it.
XML 1.0 allows a document to be encoded in any character set registered with the
Internet Assigned Numbers Authority (IANA). European documents are commonly
encoded in one of the ISO Latin  character sets, such as ISO-8859-1. Japanese documents
commonly use Shift-JIS , and Chinese documents use GB2312  and Big 5 .
A full list of registered character sets is maintained by the Internet Assigned
Numbers Authority (IANA) .
XML processors are not required by the XML 1.0 specification to support any more
than UTF-8 and UTF-16, but most commonly support other encodings, such as US-
ASCII and ISO-8859-1. Although most SVG transactions are currently conducted in
ASCII (or the ASCII subset of UTF-8), there is nothing to stop SVG documents from
containing, say, Korean text. Y ou will, however, probably have to dig into the encoding
support of your computing platform to find out if it is possible for you to use alternative
encodings.
292 | Appendix A: The XML You Need for SVG

Validity
In addition to well-formedness, XML 1.0 offers another level of verification, called
validity . To explain why validity is important, let’s take a simple example. Imagine you
invented a simple XML format for your friends’ telephone numbers:
<phonebook>
  <person>
    <name>Albert Smith </name>
    <number> 123-456-7890 </number>
  </person>
  <person>
    <name>Bertrand Jones </name>
    <number> 456-123-9876 </number>
  </person>
</phonebook>
Based on your format, you also construct a program to display and search your phone
numbers. This program turns out to be so useful, you share it with your friends. How‐
ever, your friends aren’t so hot on detail as you are, and try to feed your program this
phone book file with a <phone>  element instead of a <number>  element:
<phonebook>
  <person>
    <name>Melanie Green </name>
    <phone>123-456-7893 </phone>
  </person>
</phonebook>
Note that, although this file is perfectly well-formed, it doesn’t fit the format you pre‐
scribed for the phone book, and you find you need to change your program to cope
with this situation. If your friends had used number  as you did to denote the phone
number, and not phone , there wouldn’t have been a problem. However, as it is, this
second file is not a valid phone book document.
For validity to be a useful general concept, you need a machine-readable way of saying
what a valid document is; that is, which elements and attributes must be present and in
what order. XML 1.0 achieves this by introducing document type definitions  (DTDs).
For the purposes of SVG, you don’t need to know much about DTDs. Rest assured that
SVG does have a DTD, and it spells out in detail exactly which combinations of elements
and attributes make up a valid document.
Document Type Definitions (DTDs)
The purpose of a DTD is to express the allowed elements and attributes in a certain
document type and to constrain the order in which they must appear within that docu‐
ment type. A DTD contains declarations defining the element types and attribute lists.
A DTD may span more than one file, and the SVG 1.1 specification uses a modularized
Validity | 293

6.See http://www.docbook.org .DTD spread over more than a dozen files. However, the mechanism for including one
file inside another—parameter entities—is outside the scope of this book. It is common
to mistakenly conflate element and element types. The distinction is that an element is
the actual instance of the structure as found in an XML document, whereas the element
type is the kind of element that the instance is.
Putting It Together
What is important to you is knowing how to link a document to its defining DTD. This
is done with a document type declaration <!DOCTYPE  …>, inserted at the beginning of
the XML document, after the XML declaration in our fictitious example:
<?xml version="1.0" encoding="us-ascii"?>
<!DOCTYPE authors SYSTEM "http://example.com/authors.dtd">
<authors>
  <person id="lear">
    <name>Edward Lear </name>
    <nationality> British</nationality>
  </person>
  <person id="asimov" >
    <name>Isaac Asimov </name>
    <nationality> American </nationality>
  </person>
  <person id="mysteryperson" />
</authors>
This example assumes the DTD file has been placed on a web server at example.com .
Note that the document type declaration specifies the root element of the document,
not the DTD itself. Y ou could use the same DTD to define person , name , or nationali‐
ty as the root element of a valid document. Certain DTDs, such as the DocBook DTD
for technical documentation,6 use this feature to good effect, allowing you to provide
the same DTD for multiple document types.
A validating XML processor is obliged to check the input document against its DTD. If
it does not validate, the document is rejected. To return to the phone book example, if
your application validated its input files against a phone book DTD, you would have
been spared the problems of debugging your program and correcting your friend’s XML
because your application would have rejected the document as being invalid. Some of
the programs that read SVG files have a validating XML processor built into them to
assure they have valid input (and to keep you honest!). The kinds of XML processors
that are available are discussed in “Tools for Processing XML ” on page 295 .
294 | Appendix A: The XML You Need for SVG

XML Namespaces
XML 1.0 lets developers create their own elements and attributes, but this leaves open
the potential for overlapping names. <title>  may mean the name of a book in one
context, but it may mean the prefix for a person’s name (Ms., Dr., etc.) in a different
context. The Namespaces in XML specification  provides a mechanism developers can
use to identify particular vocabularies using Uniform Resource Identifiers (URIs).
SVG uses the URI http://www.w3.org/2000/svg  for its namespace. The URI is just an
identifier—opening that page in a web browser reveals some links to the SVG, XML 1.0,
and Namespaces in XML specifications. Programs processing documents with multiple
vocabularies can use the namespaces to figure out which vocabulary they are handling
at any given point in a document.
SVG applies the namespace in the root element of SVG documents:
<svg xmlns="http://www.w3.org/2000/svg"  width="100" height="100">
....
</svg>
The xmlns  attribute, which defines the namespace, is actually provided as a default value
by the SVG DTD. However, some browsers will not render an SVG document if you
don’t use the namespace explicitly. (If the namespace does appear, it must have the exact
value shown earlier.) The namespace declaration applies to all of the elements contained
by the element in which the declaration appears, including the containing element. This
means that the element named svg is in the namespace
http://www.w3.org/2000/svg .
SVG uses the “default namespace” for its content, using the SVG element names without
any prefix. Namespaces can also be applied using prefixes, as shown here:
<svgns:svg  xmlns:svgns= "http://www.w3.org/2000/svg"
    width="100" height="100">
....
</svgns:svg>
In this case, the namespace URI http://www.w3.org/2000/svg  would apply to all ele‐
ments using a prefix of svgns . The SVG 1.0 DTD won’t validate against such documents.
Namespaces are very simple on the surface but are a well-known field of combat in XML
arcana. For more information on namespaces, see XML in a Nutshell  or Learning
XML  (both O’Reilly).
Tools for Processing XML
Many parsers exist for using XML with many different programming languages. Most
are freely available, the majority being open source.
XML Namespaces | 295

Selecting a Parser
An XML parser typically takes the form of a library of code that you interface with your
own program. The SVG program hands the XML over to the parser, and it hands back
information about the contents of the XML document. Typically, parsers do this either
via events or via a Document Object Model (DOM).
With event-based parsing, the parser calls a function in your program whenever a parse
event is encountered. Parse events include things like finding the start of an element,
the end of an element, or a comment. Most Java event-based parsers follow a standard
API called SAX , which is also implemented for other languages such as Python and Perl.
DOM-based parsers work in a markedly different way. They consume the entire XML
input document and hand back a tree-like data structure that the SVG software can
interrogate and alter. The DOM is a W3C standard with its own documentation .
As XML matures, hybrid techniques that give the best of both worlds are emerging. If
you’re interested in finding out what’s available and what’s new for your favorite pro‐
gramming language, keep an eye on the following online sources:
XML.com Resource Guide
http://www.xml.com/resourceguide/
Free XML Tools Guide
http://www.garshol.priv.no/download/xmltools/
XSLT Processors
Many XML applications involve transforming one XML document into another XML
document or into HTML. The W3C has defined a special language called XSLT  for
doing transformations. XSLT processors are becoming available for all major program‐
ming platforms.
XSLT works by using a stylesheet , which contains templates that describe how to trans‐
form elements from an XML document. These templates typically specify what XML
to output in response to a particular element or attribute. Using a W3C technology
called XPath  gives you the flexibility to say not only “do this for every person  element, ”
but to give instructions as complex as “do this for the third person  element whose name
attribute is Fred . ”
Because of this flexibility, some applications have sprung up for XSLT that aren’t really
transformation applications at all, but take advantage of the ability to trigger actions on
certain element patterns and sequencers. Combined with XSLT’s ability to execute cus‐
tom code via extension functions, the XPath  language has enabled applications such as
document indexing to be driven by an XSLT processor. Y ou can see a brief introduction
to XSLT in Chapter 15 .
296 | Appendix A: The XML You Need for SVG

The W3C specifications for XSLT and XPath can be found at http://w3.org/TR/xslt  and
http://w3.org/TR/xpath , respectively.
Tools for Processing XML | 297

APPENDIX B
Introduction to Stylesheets
As mentioned in Chapter 5 , some attributes of SVG elements control the element’s
geometry.  An example of one such attribute would be the cx (center x) attribute of a
<circle> . Other attributes, such as fill , control the element’s presentation. Stylesheets
provide a way for you to separate the presentation from the geometric structure; this
lets you control the visual display of many different SVG elements (and even documents)
by changing one stylesheet referenced by all the documents.
Anatomy of a Style
A style  is a specification of a visual property for an element and the value that you would
like that property to have. The property name and the value are separated by a colon.
For example, to say that you want the stroke color for some element to be blue, the
appropriate style specifier would be stroke: blue .
To specify multiple properties in a style, you separate the specifiers with semicolons.
The following style specifier sets the stroke color to red, the stroke width to three pixels,
and the fill color to a light blue. The last property-value pair is followed by a semicolon.
This is not necessary, but is done to give the style a more consistent look.
stroke: red; stroke-width : 3px; fill: #ccccff;
Inline Styles: The style Attribute
Once you have determined the visual properties you’d like, you must select the element
or elements to which they apply. The simplest way to apply a style specification to a
single element is to make that specification the value of a style  attribute.  So, if you want
the preceding specification to apply to a particular <circle>  in your document, you
write this:
299

<circle cx="50" cy="40" r="12"
    style="stroke: red; stroke-width: 3px; fill: #ccccff;" />
Internal Stylesheets
If you want the style specification to apply to all <circle>  elements in a single document,
add an internal stylesheet. A stylesheet consists of selectors  (the names of the elements
you want to affect) and the style specifications for those selectors. The style specification
is enclosed in curly braces. The following applies styles to <circle>  and <rect>
elements:
<style type="text/css" ><![CDATA[
  circle {
    stroke: red; stroke-width: 3px;
    fill: #ccccff;
  }
  rect { fill: gray; stroke: black; }
]]></style>
When you put a <style>  element into an SVG document, you should enclose its con‐
tents within <![CDATA[  and ]]>. This notation tells XML parsers that the contents are
pure character data  and should not, under any circumstances, be treated as information
for XML to parse.
Because this stylesheet is within a document, it applies to that document alone. If you
have many documents, all of whose circles and rectangles appear as specified in the
preceding example, take the specifiers, without the <style>  or <![CDATA[  tags, and put
them into a separate file named myStyle.css . In each SVG document, insert the following
processing instruction:
<?xml-stylesheet href="myStyle.css" type="text/css"?>
Then, at a later point, if you decide that all rectangles should be filled with a light green
and outlined in dark green, you can simply change the specification in myStyle.css  to
read as follows:
rect {fill: #ccffcc; stroke: #006600;}
and all your documents, once redisplayed, will have green rectangles instead of gray
rectangles.
Style Selector Classes
The preceding stylesheet affects all <rect>  and <circle>  elements.  Let’s say, though,
that you want only some circles in your documents to be styled. Write your stylesheet
with a class specifier as follows, where the dot after circle  indicates that the following
identifier is a class name:
300 | Appendix B: Introduction to Stylesheets

circle.special  {
  stroke: red; stroke-width : 3px;
  fill: #ccccff;
}
If, in your SVG document, you had the following elements, the first circle would show
up as the default (black fill, no stroke), and the second would take on the style attributes
as its class name matches the class identifier in the stylesheet:
<circle cx="40" cy="40" r="20"/>
<circle cx="60" cy="20" r="10" class="special" />
It is possible to specify a generic class that can apply to any element. Presume that several
different graphic objects serve as warning symbols. Y ou would like them to have a yellow
fill and a red border. Y ou could write this selector, which consists only of a class name
and its style specifier:
.warning  { fill: yellow; stroke: red; }
This generic class may now be applied to any SVG element. In the following example,
both the rectangle and triangle will have yellow interiors and red outlines:
<rect class="warning"  x="5" y="10" width="20" height="30"/>
<polygon  class="warning"  points="40 40, 40 60, 60 50" />
The class  attribute may contain the names of several classes separated by whitespace;
their combined properties will be applied to the element in question. The following
markup adds a generic class named seeThrough  for translucency to the previous ex‐
ample and then applies both classes to the polygon:
<svg width="100" height="100" viewBox= "0 0 100 100" >
  <style type="text/css" ><![CDATA[
    .warning { fill: yellow; stroke: red; }
    .seeThrough { fill-opacity: 0.25; stroke-opacity: 0.5; }
  ]]></style>
  <rect class="warning"  x="5" y="10" width="20" height="30"/>
  <polygon  class="warning seeThrough"  points="40 40, 40 60, 60 50" />
</svg>
Using CSS with SVG
The question then becomes: which attributes in SVG elements can also be specified in
a stylesheet? Table B-1  is a list of the properties you may use in a stylesheet, the valid
values (with default value shown in boldface where appropriate), and the elements to
Using CSS with SVG | 301

1.Copyright © 2001 World Wide Web Consortium (Massachusetts Institute of Technology, Institut National
de Recherche en Informatique et en Automatique, Keio University). All Rights Reserved. http://www.w3.org/
Consortium/Legal/ .which they may be applied.  It is a modified version of the property index from the SVG
specification .1
The value of fill  and stroke  is a paint  value, which is one of the following:
•none
•currentColor
•A color specification, as described in “Stroke Color” on page 41
•A construction of the form url(…)  that refers to a gradient or pattern
Y ou can specify fallback paint options, in case there is an error loading a gradient or
pattern; the options are given in a whitespace-separated list starting with the preferred
value.
302 | Appendix B: Introduction to Stylesheets

Table B-1. CSS property table for SVG
Name Values Applies to
alignment-baseline auto  | baseline  | before-edge  | text-before-edge  |
middle  | after-edge  | text-after-edge  | ideographic  |
alphabetic  | hanging  | mathematical<tspan> , <tref> , <altGlyph> , <textPath>
baseline-shift baseline  | sub  | super  | percentage  | length <tspan> , <tref> , <altGlyph> , <textPath>  elements
clip-path uri Container elements and graphics elements
clip-rule nonzero  | evenodd  | class=noxref Graphics elements within a <clipPath>  element
color color Used to provide a potential indirect value ( currentColor ) for
fill , stroke , stop-color , flood-color , and
lighting-color
color-interpolation auto  | sRGB  | linearRGB Container elements, graphics elements, and <animateColor>
color-interpolation-filters auto  | sRGB  | linearRGB Filter primitives
color-profile auto  | sRGB  | name  | uri <image>  elements that refer to raster images
color-rendering auto  | optimizeSpeed  | optimizeQuality Container elements, graphics elements and <animateColor>
cursor uri  | auto  | crosshair  | default  | pointer  | move  | e-resize
| ne-resize  | nw-resize  | n-resize  | se-resize  |
sw-resize  | s-resize  | w-resize  | text  | wait  | helpContainer elements and graphics elements
direction ltr  | rtl <text> , <tspan> , <tref> , and <textPath>  elements
Using CSS with SVG | 303

Name Values Applies to
display inline  | block  | list-item  | run-in  | compact  | marker  |
table  | inline-table  | table-row-group  |
table-header-group  | table-footer-group  | table-row
| table-column-group  | table-column  | table-cell  |
table-caption  | none<svg> , <g> , <switch> , <a> , <foreignObject> , graphics
elements  (including  the <text>  element),  and  text  subelements  (i.e.,
<tspan> , <tref> , <altGlyph> , <textPath> ). All values
except none  are treated the same for elements in an SVG, enabling
display of the graphic.
dominant-baseline auto  | use-script  | no-change  | reset-size  | alphabetic
| hanging  | ideographic  | mathematical  | central  | middle
| text-after-edge  | text-before-edge  | text-top  |
text-bottomText content elements
enable-background accumulate  | new [  x y width height  ] Container elements
fill See description of paint  at end of table for possible values; the default
is blackShapes and text content elements
fill-opacity opacity-value  (default 1) Shapes and text content elements
fill-rule nonzero  | evenodd Shapes and text content elements
filter uri  | none Container elements and graphics elements
flood-color currentColor  | color specifier  (default black ) <feFlood>  elements
flood-opacity alphavalue  (default 1) <feFlood>  elements
font font-style , font-variant , font-weight , font-size
line-height , font-family  | caption  | icon  | menu  |
message-box  | small-caption  | status-barText content elements
font-family series of family-name  or generic-family Text content elements
font-size absolute-size  | relative-size  | length  | percentage Text content elements
304 | Appendix B: Introduction to Stylesheets

Name Values Applies to
font-size-adjust number  | none Text content elements
font-stretch normal  | wider  | narrower  | ultra-condensed  |
extra-condensed  | condensed  | semi-condensed  |
semi-expanded  | expanded  | extra-expanded  |
ultra-expandedText content elements
font-style normal  | italic  | oblique Text content elements
font-variant normal  | small-caps Text content elements
font-weight normal  | bold  | bolder  | lighter  | 100  | 200  | 300  | 400  | 500
| 600  | 700  | 800  | 900Text content elements
glyph-orientation-horizontal angle  (default 0deg ) Text content elements
glyph-orientation-vertical auto  | angle Text content elements
image-rendering auto  | optimizeSpeed  | optimizeQuality Images
kerning auto  | length Text content elements
letter-spacing normal  | length Text content elements
lighting-color currentColor  | color specification  (default white )<feDiffuseLighting>  and <feSpecularLighting>
elements
marker , marker-end , marker-mid ,
marker-startnone  | uri <path> , <line> , <polyline> , and <polygon>  elements
mask uri  | none Container elements and graphics elements
opacity alphavalue  (default 1) Container elements and graphics elements
Using CSS with SVG | 305

Name Values Applies to
overflow visible  | hidden  | scroll  | auto Elements that establish a new viewport, <pattern>  elements, and
<marker>  elements
pointer-events visiblePainted  | visibleFill  | visibleStroke  |
visible  | painted  | fill  | stroke  | all  | noneGraphics elements
shape-rendering auto  | optimizeSpeed  | crispEdges  |
geometricPrecisionShapes
stop-color currentColor  | color specification  (default black )<stop>  elements
stop-opacity alphavalue  (default 1) <stop>  elements
stroke See description of paint  at end of table for possible values; the default
is noneShapes and text content elements
stroke-dasharray none  | dasharray Shapes and text content elements
stroke-dashoffset dashoffset  (default 0) Shapes and text content elements
stroke-linecap butt  | round  | square Shapes and text content elements
stroke-linejoin miter  | round  | bevel Shapes and text content elements
stroke-miterlimit miterlimit  (default 4) Shapes and text content elements
stroke-opacity opacity-value  (default 1) Shapes and text content elements
stroke-width width  (default 1) Shapes and text content elements
text-anchor start  | middle  | end Text content elements
text-decoration none  | underline  | overline  | line-through  | blink Text content elements
306 | Appendix B: Introduction to Stylesheets

Name Values Applies to
text-rendering auto  | optimizeSpeed  | optimizeLegibility  |
geometricPrecision<text>  elements
unicode-bidi normal  | embed  | bidi-override Text content elements
visibility visible  | hidden  | collapse Graphics elements (including the <text>  element) and text sub-
elements (i.e., <tspan> , <tref> , <altGlyph> , <textPath> ,
and <a> )
word-spacing normal  | length Text content elements
writing-mode lr-tb  | rl-tb  | tb-rl  | lr | rl | tb <text>  elements
Using CSS with SVG | 307

308 | Appendix B: Introduction to Stylesheets

1.Unlike the actual practice of Santería and voudon, which are much more complex and not inherently evil.APPENDIX C
Programming Concepts
Many graphic designers want to use the scripting capability of SVG as described in
Chapter 13 . If they’re not familiar with programming, they tend to practice what might
be called voodoo scripting . In the popular-culture stereotype,1 voodoo works by reciting
a mysterious spell and hoping that your enemies die horribly. Voodoo scripting works
by copying someone else’s mysterious script into your SVG document and hoping that
your document continues to live. We’re under no illusion (nor even a spell) that reading
this brief, purposely oversimplified summary will turn you into a master programmer.
Our goal is simply to introduce enough of the elementary programming concepts to
remove some of the mystery from the scripts that you copy and modify. The particular
programming language that we will discuss in this appendix is called ECMAScript; it is
the standardized version of the JavaScript language. The concepts used in ECMAScript
are common to many other programming languages.
Constants
A constant  is a fancy word for a number or string of characters that never changes.
Examples are 2, 2.71828 , "message" , and 'communication' . The last two are called
string constants . In ECMAScript, you can use either single or double quotes to mark the
boundaries of a string. This is good if you ever need to write things like
"O'Reilly Media"  or 'There is no "there" there.'
Y ou will sometimes see the two Boolean constants true  and false , which are used for
yes-or-no situations.
309

Variables
A variable  is a block of memory reserved to hold some value that may change from time
to time. Y ou can think of it as a mailbox with a name on it; the mailbox holds a slip of
paper with information written on it. Let’s say you need to keep track of the current
width of a rectangle and need to store a changeable message; in ECMAScript you define
these variables like this:
var currentWidth ;
var message;
Y ou may visualize them as shown in Figure C-1 .
Figure C-1. Two empty variables
Variables defined this way have nothing in their “mailbox”; the technical term is that
these variables contain the undefined value. Variable names must start with a letter or
an underscore and can contain only letters, digits, and underscores. They are case sen‐
sitive, so width , Width , and WIDTH  are names of three different variables.
Assignment and Operators
Y ou can put a value into a variable by using an assignment statement,  which starts with
a variable name, an equal sign, and the value. Examples:
currentWidth  = 32;
message = "I love SVG." ;
Y ou can read these as “set the value of currentWidth  equal to 32” and “set the value of
message  equal to " I love SVG ."” In reality, this statement works from right to left;
whatever is on the righthand side of the equal sign is placed into the variable on the
lefthand side. Note that all our ECMAScript statements end with a semicolon. There
are cases where you don’t need one, but we’d rather have the semicolon and not need it
than need it and not have it. Figure C-2  shows the “after” picture for these assignments.
Figure C-2. Two assigned variables
310 | Appendix C: Programming Concepts

Actually, we told you a small lie a few sentences ago. Whatever the righthand side of the
equal sign works out to  goes into the variable on the lefthand side. This lets us do math‐
ematical operations, as in the following code:
var info;     
info = 7 + 2;     
info = 7 * 2;    
info = info + 1;     
info = "door";     
info = info + "bell"     
Create an empty variable named info .
info  will now  contain  the value  9 (7 plus 2). Y ou use the minus  sign (-) for
subtraction.
The asterisk is used for multiplication, because the multiplication symbol is too
easily confused with the letter x. info  will contain 14 (7 times 2). The previous
value of 9 will be discarded. Y ou use the forward slash ( /) for division.
This  is illegal  in high  school  algebra,  but not in ECMAScript.  Start  with  the
righthand  side of the equal  sign:  take the current  value  of info , which  is 14, and
add 1 to it. The righthand  side works  out to 15. This  result  goes  into the variable
on the left side of the equal,  which  just happens  to be info . At the end of this
statement, info  will contain the value 15.
This  takes  the string  "door"  and puts  it into info . In ECMAScript,  a variable
can hold any sort of information at any time.
This takes the current value of info , which is "door" , and “adds” (appends) the
string  constant  "bell"  to the end of it. The resulting  value  is the word
"doorbell" , which  goes  back  into info  on the left side of the equal  sign.  The
plus sign is the only  operator  that works  with  strings;  you can’t  subtract,  multiply,
or divide  them.  Be careful  when  mixing  addition  and string  concatenation:
"The answer is " + 2 + 2 produces  "The answer is 22";
"The answer is " + (2 + 2)  produces "The answer is 4"
Y ou can define a variable and set its initial value all in one fell swoop. This is called
initializing  a variable. Y ou can have more than one operation on the righthand side of
the equal sign. In the following code, an empty variable named celsius  is created, then
a variable named fahrenheit  with value 212, and then the Fahrenheit temperature is
converted to Celsius:
var celsius;
var fahrenheit  = 212;
celsius = ((fahrenheit  - 32) / 9 ) * 5;
Assignment and Operators | 311

2.This is not done to be contrary; it’s because programs often use a mathematical formula to select the relevant
item. These formulas are invariably easier when you start counting at zero.Arrays
An array  is an ordered collection of data, indexed by number.  We compared a simple
variable to a mailbox (send mail to the “Smith Residence”). An array is like a set of
numbered apartment mailboxes (send mail to the “ A-List Apartments, #12”). The only
difference is that array index numbers start at 0 rather than 1.2 Here’s a declaration of
an array of radius sizes for circles. This form initializes the array. The second statement
sets the value of the last element of the array to 9. Y ou access one of the “slots” of an
array by putting its index number in square brackets. Figure C-3  shows the results after
the code has finished:
var radiusSizes  = [8.5, 6.4, 12.2, 7];
radiusSizes [3] = 9;
Figure C-3. Depiction of an array
Comments
Comments  provide a way to document your programs so that other people can figure
out what you did.  There are two kinds of comments in ECMAScript. If you place two
forward slashes in a row ( //), they and everything to the end of that line are considered
to be a comment. If you want a multiline comment, start with /* and end with */ as
shown here:
var interest ;   // this is accumulated on a daily basis
var rate;       // expressed as a decimal; 75% is a rate of 0.75
/* Figure out the payment amount given a principal
   of $10,000 and 180 monthly payments. */
Conditional Statements
Ordinarily, your program statements are carried out in the order in which they ap‐
pear. Sometimes you may want to do different calculations depending upon some con‐
dition. Y ou use the if statement to do this. Here’s a calculation for wages that depends
upon the number of hours worked. We presume that all the var statements have been
set up appropriately:
312 | Appendix C: Programming Concepts

3.There  are also the === and !== operators;  when  you use them,  Boolean  values  and strings  containing
numbers won’t automatically be converted into numbers when compared against a number.if (hours <= 40)     
{
  pay = hours * rate;     
}
else     
{
  pay = 40 * rate + (hours - 40) * rate * 1.5;     
}
The expression  in the parentheses  is called  the condition . It always  asks a yes-
or-no  question.  In this case,  the question  is “is the value  of the hours  variable
less than  or equal  to 40?”  Other  comparison  operations  are less than  (<), greater
than  (>) greater  than  or equal  (>=),  equal  (==),  and not equal  (!=).3 Note  that
asking if two things are equal to each other requires two equal signs, not one!
If the answer  to the question  is yes, then  the program  will do everything  between
the opening and closing braces { and }…
…otherwise ( else )…
…do  everything  between  the other  set of curly  braces.  The curly  braces  are used
to signify that one or more statements should be grouped together, much in the
way that XML opening and closing tags tell where content begins and ends.
Repeated Actions
Sometimes you want to repeat an action a specific number of times (“fill 10 2-liter
containers from a large water tank”). Y ou use a for loop to do the first sort of task.  It’s
called a loop because, if you were to draw arrows representing the path the computer
takes through your program, they would form a loop as the program repeated the ac‐
tions. Here’s the container-filling scenario translated into ECMAScript, with variables
for the water tank and the containers presumed to be defined. The loop body, that is,
the actions you want repeated, are enclosed in curly braces:
var i;              // a counter variable
for (i = 0;         // start counting at zero
     i < 10;        // up to (but not including) 10
     i++)           // add one to the count at every repetition
{
  container [i] = 2;                // fill container number "i"
  waterTank  = waterTank  - 2;  // take 2 liters out of the tank
}
Repeated Actions | 313

Other times, you want to repeat an action as long as some condition is true (“keep filling
2-liter containers from a large water tank as long as there is any water left”). For this,
you use a while  loop:
i = 0;                          // start with container number zero
while ( waterTank  > 0 )         // while there is water left
{
  container [i] = 2;                // fill container number "i"
  waterTank  = waterTank  - 2;  // take 2 liters out of the tank
  i = i + 1;                  // move on to the next container
}
Functions
Y ou can accomplish some surprisingly sophisticated tasks with this small number of
programming concepts. Y ou collect sets of ECMAScript statements designed to perform
a specific task into functions. Think of a function as a recipe card that gives a list of
ingredients and instructions which, when followed, create a specific dish. A function
starts with the keyword function  followed by the function name. The name should be
indicative of the task that it does, and it follows the same rules that variable names do.
Following the function name, in parentheses, are the parameters of the function. A
parameter is extra information that the function needs when it does its task. Consider
this imaginary recipe:
Korean Kimch’i Surprise
Take 100 grams of kimch’i per serving, 25 grams of ko-ju-jang red pepper paste per
serving, and 50 grams of mushrooms per serving. Mix well. Serve.
Before you can make the recipe, you have to supply some extra information—the num‐
ber of servings you intend to make. Our script might look like this:
function  makeKimchiSurprise (numberOfServings )
{
  var kimchi = 100 * numberOfServings ;
  var kojujang  = 25 * numberOfServings ;
  var mushrooms  = 50 * numberOfServings ;
  var surprise  = kimchi + kojujang  + mushrooms ;
}
This is only the definition of the function. It does absolutely nothing  until it is invoked,
or called on. (Y ou may have hundreds of recipe cards in a file box at home. They just sit
there, inactive, until someone asks you to pull one card out and perform the cooking
tasks.) Y ou will often call a function as the result of an event. In the following example,
a click on the blue rectangle will call the function. The number 5 in the parentheses will
fill in the “extra information” required by the numberOfServings  parameter:
<rect x="10" y="10" width="100" height="30" style="fill: blue;"
  onclick="makeKimchiSurprise( 5 )"  />
314 | Appendix C: Programming Concepts

Even  if the function  doesn’t  need  any parameters,  you still have  to put
the parentheses after its name in order to call it. Without the paren‐
theses,  area  is just another  object  to be utilized.  Unlike  other  lan‐
guages, ECMAScript doesn’t let you declare both a variable area  and
a function area()  in the same namespace.
Functions can also call other functions. For example, a function that calculates com‐
pound interest might need to call upon another function that determines whether a year
is a leap year or not. A parameter lets the interest function tell the leap year function
what year it’s interested in. The return  statement will let the leap year function com‐
municate its result back to the caller. This allows you to modularize a program into
generally useful building blocks. In cooking terms, the makeHollandaiseSauce()  func‐
tion can be called from the makeEggsBenedict()  function as well as from
makeChickenFlorentine() .
Objects, Properties, and Methods
Take a power supply with its on-off switch, a plastic dial, a lever with a spring, and a
metal chassis with rectangular slots and coils of wire in it. Put all these parts together,
and you get a toaster.
Each of these parts is an object.  Some of them have characteristics that are of interest:
the power supply has a voltage of 110 or 220 volts, the chassis has a color and a number
of slots, and the dial has a minimum and maximum setting. (The lever has no interesting
characteristics.)
Y ou do actions with each of these objects: you push the lever down or pop it up, you
insert bread into the slots, you turn the power supply on or off, and you turn the dial
to the desired setting.
Let’s take this toaster into the world of ECMAScript. Now the mailboxes can hold many
slips of paper, each representing different data properties of the object. Simple data types
(like numbers or strings) are written down on the slip of paper directly. More complex
data types require their own “mailboxes” to hold all the information, so the “slip of
paper” contains information on where to find the full object (a pointer  to the data); as
a consequence, multiple variables can reference the same object. A mailbox can also
hold a set of instructions (functions, like our recipe cards) so that they can perform
actions. When a variable is inside another mailbox, we call the inner variable a proper‐
ty. When a function is inside a mailbox, we call it a method . The diagram for the toaster
looks like Figure C-4 .
Objects, Properties, and Methods | 315

4.If you insist upon reading left to right, adapt the suggestion for reading path names made by Elizabeth Castro
in her book, Visual Quickstart Guide to HTML for the World Wide Web : read the period as “contains. ” Then
toaster.powerSupply.voltage = 220;  is read as “the toaster contains a power supply, which contains a
voltage. Set that voltage to 220. ”
Figure C-4. The object diagram for a toaster
This is a very flexible way of modeling a toaster, but it’s introduced a problem. To set
the toaster’s color or voltage or to pop out the bread, you can’t just say things like this:
color = "gold";
voltage = 220;
popUp();
The color  property is nested inside the toaster  variable, the voltage  really belongs to
the powerSupply  inside the toaster , and it’s the toaster ’s lever  that does the popUp
function. For these reasons, you must say:
toaster.color = "gold";
toaster.powerSupply .voltage = 220;
toaster.lever.popUp();
These are easy to figure out if you read them from right to left and say “of ” whenever
you see a period: “Put gold into the color of the toaster. ” “Put 220 into the voltage of the
power supply of the toaster. ” “Call the pop up method of the lever of the toaster. ”4 Think
of this as the grown-up version of nested objects and methods that you learned as a
child: “This is the dog that chased the cat that killed the rat that ate the malt that lay in
the house that Jack built. ”
By using objects to model the behavior of a toaster, you’ve built a “Toaster Object Model. ”
Similarly, there is a Document Object Model (DOM) that lets ECMAScript access a
document’s properties and invoke its methods. Almost all of your access to an SVG
316 | Appendix C: Programming Concepts

document will be through methods that begin with the word set or get. To set the
radius of a <circle>  element with an id of wheel , you might write
svgDocument.getElementById("wheel").setAttribute("r", 3) . In some cases, you
will use properties. For example, if you receive a mouse click event and want to find its
x-coordinate, you would write evt.clientX .
The SVG Document Object Model is actually a superset of the XML
Document  Object  Model;  once  you learn  to manipulate  the struc‐
ture of an SVG  document,  you can immediately  apply  that knowl‐
edge to other XML documents, so the time you spend in learning the
DOM will be amply repaid.
What, Not How
We’ve given you an overview of the what  of programming, which can serve as a base
for reading programs that other people have written and making sense of them (the
programs, not the people). How  you define a task and lay out the programming steps
to solve it is another problem altogether, and far beyond the scope of this book. If you
enjoy solving crossword puzzles or brain teasers, or just solving problems in general,
you may well enjoy writing programs. If you would like to go in depth with JavaScript,
we recommend JavaScript: The Definitive Guide, 6th Edition  by David Flanagan
(O’Reilly). 
What, Not How | 317

APPENDIX D
Matrix Algebra
Matrix algebra  is a branch of mathematics that defines operations on matrices , which
are series of numbers arranged in rows and columns. In addition to its many uses in
science and engineering, matrix algebra makes the computations for graphic operations
very efficient. The purpose of this appendix is to introduce you to the fundamental
concepts of matrix algebra that SVG uses “behind the scenes. ”
Matrix Terminology
We describe a matrix by its number of rows and columns. Figure D-1  displays a matrix
that arranges a series of daily temperatures over a two-week period into two rows of
seven columns each. This matrix is called a 2-by-7 matrix . Matrices are enclosed in
square brackets when written.
Figure D-1. 2-by-7 matrix of daily temperatures
Here are some other terms that you may encounter when dealing with matrix operations:
a square matrix  is a matrix with the same number of rows as columns. A vector  is a
matrix with only one row, and a column vector  is a matrix with only one column. The
individual numbers in a matrix are called entries , and the technical term for a plain
number is a scalar . Now you can bring these sure-fire conversation stoppers to the next
party you attend.
Applying the concept of a matrix to SVG, you might express a set of x- and y-coordinates
as a 2-by-1 matrix. This isn’t the way we’ll ultimately end up representing coordinates,
319

but it’s a good place to start, as it’s easy to understand. In this representation, the point
(3,5) is expressed as shown in Figure D-2 .
Figure D-2. Coordinates expressed as a matrix
Matrix Addition
The easiest matrix operation is addition. To add two matrices, you add their corre‐
sponding elements. This, of course, requires that your matrices have exactly the same
number of rows and columns. Figure D-3  shows the addition of two matrices, each 3
by 2.
Figure D-3. Addition of two 3-by-2 matrices
Y ou can see that the translate  transformation in SVG could be accomplished easily by
matrix addition. For example, the matrix addition in Figure D-4  would implement
transform="translate(7, 2)"  for any point ( x,y).
Figure D-4. Simple method to translate coordinates
The order in which you add matrices doesn’t matter. Mathematicians say that matrix
addition is commutative  (A + B = B + A). It is also associative ; given three matrices A,
B, and C: (A + B) + C is the same as A + (B + C). There is such a thing as matrix
subtraction; just subtract the corresponding elements of the two matrices. Just as with
regular subtraction, matrix subtraction is not commutative.
Matrix Multiplication
Y ou may be thinking that matrix multiplication works in a similar manner, and that’s
how you can do a scale()  transformation. Unfortunately, the easy way doesn’t work
this time. Matrix multiplication is significantly more complicated than matrix addition.
320 | Appendix D: Matrix Algebra

In the first examples that follow, this complexity appears to be needless. Later in this
appendix, you’ll see that the usefulness of matrix multiplication far outweighs its
difficulty.
In order to multiply two matrices, the number of columns  of the first matrix must equal
the number of rows  of the second matrix. Such matrices are called compatible . This
means you can multiply a 3-by-5 matrix times a 5-by-4 matrix, but not a 3-by-5 matrix
times a 3-by-2 matrix. The matrices in Figure D-5  are compatible and can be multiplied.
Figure D-5. Two matrices to be multiplied
The resulting matrix will have the same number of rows as the first matrix, and the same
number of columns as the second matrix. Thus, the example’s 2-by-3 matrix times the
3-by-2 matrix will result in a 2-by-2 matrix.
The entry that will go in row one, column one of the result matrix is the dot product of
the first row of the first matrix and the first column of the second matrix. Dot product
is a fancy way of saying “add up the products of the corresponding entries in a row and
column” as shown in Figure D-6 .
Figure D-6. First entry in multiplied matrices
To find the quantity to place in row two, column one (the lower left) of the result matrix,
take the dot product of row two in the first matrix and column one in the second matrix,
as shown in Figure D-7 .
Figure D-7. Second entry in multiplied matrices
Calculating the remaining items produces the result shown in Figure D-8 .
Matrix Multiplication | 321

Figure D-8. Completed multiplication of matrices
Given this information, you can now use matrix multiplication to express the calcula‐
tions needed to scale a point ( x,y) by a factor of 3 horizontally and a factor of 1.5 vertically.
The transformation matrix will have to have two rows and two columns so it is com‐
patible with the two-row by one-column coordinates, as shown in Figure D-9 .
Figure D-9. Simple scaling by multiplying matrices
Unlike  multiplication  of single  numbers,  matrix  multiplication  is not
commutative. If two matrices, A and B, aren’t square matrices, then
A·B won’t  have  the same  number  of rows  and columns  as B·A (if
they’re  even  compatible  in both  directions).  Even  if A and B are both
3-by-3  square  matrices,  there’s  no guarantee  that A times  B will result
in the same  answer  as B times  A. In fact,  they’ll  come  out equal  in
only a very few special cases.
There is another limited form of multiplication: multiplying a matrix by a scalar  (a plain
number) will multiply every item in the matrix by the scalar, as shown in Figure D-10 .
We didn’t mention this in conjunction with scaling, because this construct scales uni‐
formly, and SVG scaling isn’t always the same horizontally and vertically.
Figure D-10. Multiplying a scalar by a matrix
There is no such thing as matrix division per se . There is a construct called a matrix
inverse , which is analogous to the reciprocal of a number; if you multiply matrix A times
B times the inverse of B, the result is simply A. Another way of describing an invert
matrix is that the inverse of B is the matrix that, when multiplied by B, creates an identity
matrix . An identity matrix is a square matrix with values of 1 along the diagonal and
values of 0 in every other position; multiplying by an identity matrix does not change
the original matrix.
322 | Appendix D: Matrix Algebra

In SVG, inverse matrix calculations are used for converting between coordinate systems,
where it is necessary to calculate the reverse of a transformation. Not all matrix trans‐
formations are invertable, however; if the original transformation resulted in the loss
of information about one or more dimensions of the matrix, no subsequent multipli‐
cation can re-create it. While multiplying by the identity matrix is analagous to multi‐
plying a number by 1, multiplying by a noninvertable matrix is analagous to multiplying
a number by 0. And just like trying to divide by 0, trying to invert a noninvertable matrix
will cause your processor to throw an error.
How SVG Uses Matrix Algebra for Transformations
The approach we’ve taken to translation and scaling works, but it’s not ideal. For in‐
stance, if you want to translate a point and then scale it, you need to do one matrix
addition and then a matrix multiplication. SVG uses a clever trick to represent coordi‐
nates and transformation matrices so you can do scaling and translation all with one
operation. First, it adds a third value, which is always equal to 1, to the coordinate matrix.
This means that the point (3,5) will be represented as shown in Figure D-11 .
Figure D-11. SVG representation of a coordinate
SVG uses a 3-by-3 matrix, again set up with extra zeros and ones, to specify the trans‐
formation. Figure D-12  shows a matrix that translates a point by a horizontal distance
of tx and a vertical distance of ty.
Figure D-12. SVG representation of translation
Figure D-13  shows a transformation matrix that will scale a point by a factor of sx in
the horizontal direction and sy in the vertical direction.
How SVG Uses Matrix Algebra for Transformations | 323

Figure D-13. SVG representation of scaling
What this buys you is a consistent notation; all the transformations, including rotation
and skewing, can be represented with 3-by-3 matrices. Furthermore, because everything
is 3-by-3, you can construct a chain of transformations by multiplying those matrices
together; they’re guaranteed to be compatible. For example, to do a translation followed
by a scaling, you multiply the matrices in that order (see Figure D-14 ).
Figure D-14. Translation followed by scaling
Again, it seems as if this is needlessly complicating matters. In order to transform the
point (3,5), you now need two matrix multiplications. To transform another point would
require two more multiplications. Given a <path>  element with several hundred points,
this could run into some serious computing time.
Here’s where SVG does something clever: it multiplies the first two matrices together,
and stores that result, as shown in Figure D-15 .
Figure D-15. Result of multiplying translation and scaling matrices
This “pre-multiplied” matrix now embodies both of the transformations. By multiplying
this new matrix times a coordinate point’s matrix, the translation and scaling occurs
with a single matrix multiplication (see Figure D-16 ). Now conversion of a hundred
points would require only 100 multiplications, not 200.
Figure D-16. Result of premultiplying translation and scaling matrices
324 | Appendix D: Matrix Algebra

If you had to do a translation followed by a rotation followed by a scale, you’d create
three 3-by-3 matrices; one to do the translation, one to do the rotation, and one to do
the scaling. Y ou’d multiply them all together (in that order), and the resulting matrix
would embody all the calculations needed to do all three transformations.
As mentioned in “Matrix Multiplication” on page 320 , matrix multi‐
plication  is not commutative.  If you change  the order  of the trans‐
formation matrices, you get a different result. This is the mathemat‐
ics behind  the fact that the sequence  of transformations  makes  a
difference in the resulting graphic, as described in Chapter 6 , in “Se‐
quences of Transformations” on page 74 .
This, then, is the power of matrix algebra; it lets you combine the information about as
many transformations as you want into one single 3-by-3 matrix, thus dramatically
reducing the amount of calculation necessary to transform points. The matrices in
Figure D-17  are the ones used to specify a rotation by an angle a, a skew along the x-
axis of ax, and a skew along the y-axis of ay.
Figure D-17. Rotate, skew x, and skew y transformation matrices
Y ou can use the matrix(a, b, c, d, e, f) transformation to specify six numbers that
fill in the entries in the transformation matrix; the relationship of the numbers to the
matrix is shown in Figure D-18 .
Figure D-18. Generic transformation matrix
SVG also uses matrix algebra quite heavily in the calculations associated with filters,
which are described in Chapter 11 . There, a pixel’s red, green, blue, and alpha (opaque‐
ness) values are described as a matrix with five rows and one column. It also adds a fifth
row so that a 5-by-5 transformation matrix can add a constant amount to any of the
values as well as multiply them by any desired factor. The economies of scale we get by
pre-multiplying coordinate transformation matrices work equally well when pre-
multiplying pixel manipulation matrices.
How SVG Uses Matrix Algebra for Transformations | 325

The <feColorMatrix>  filter, described in Chapter 11  in “The <feColorMatrix> Ele‐
ment” on page 160 , lets you specify all 20 values. Thus, this markup:
<feColorMatrix type="matrix"
  values=
   "a0  a1  a2  a3  a4
    a5  a6  a7  a8  a9
    a10 a11 a12 a13 a14
    a15 a16 a17 a18 a19"/>
would be placed in the pixel transformation matrix, as shown in Figure D-19 .
Figure D-19. Color transformation matrix
326 | Appendix D: Matrix Algebra

APPENDIX E
Creating Fonts
The fonts built into the system that renders your SVG documents will take care of the
vast majority of your needs. Sometimes, though, you will want to use a custom font. It
is possible to create a font for use with SVG from scratch. In brief, you use a <font>
element tag to describe the origin and default width of the font’s glyphs. Inside the
<font>  is the <font-face>  element, which has an immense number of attributes that
describe the font’s dimensions in excessive detail. Table E-1  summarizes some of the
more useful attributes. Y ou can see them all in detail in the SVG specification .
The SVG font specification was intended to allow designers to create accessible logos
and graphics. Search engines and screen readers would understand the text as a sequence
of characters, but the design could be completely customized and would look the same
on every system. At the time of writing, this ideal has not yet been realized, because two
major web browsers (Internet Explorer and Firefox) have not implemented SVG fonts.
If you create a custom SVG font, there are web services that can convert it to other font
formats for use with these browsers.
Table E-1. font-face attributes
font-family A list of font family names
font-style Values of normal , italic , or oblique .
font-variant normal  or small-caps .
font-weight normal , bold , or a number from 100 to 900 in steps of 100.
font-stretch Indicates  the condensed  or expanded  nature  of the face  relative
to others in its font family. Possible prefixes for condensed  or
expanded  are ultra- , extra- , and semi- .
font-size all , suitable for most scalable fonts, or if a font is designed for a
restricted range of sizes, a list of lengths (such as 18pt ).
unicode-range The range of Unicode characters covered by this font, in the form
Ustart-end .
327

font-family A list of font family names
units-per-em Coordinate units for the em square . This establishes a coordinate
system for the font. The following are all measured in these units.
cap-height Height of uppercase glyphs.
x-height Height of lowercase glyphs.
accent-height Distance from the origin to the top of accent characters.
ascent Maximum unaccented height of the font.
descent Maximum unaccented depth of the font.
widths A list of widths for the glyph corresponding to each character.
bbox The maximal bounding box for the font; a box in which the largest
character will fit.
underline-position Ideal position of an underline.
underline-thickness Ideal thickness of an underline.
Following the <font-face>  are <glyph>  elements, which contain path descriptions for
each of the glyphs you wish to have in your font.
While it is possible to create fonts from scratch, it’s a lot of work, and often a duplication
of effort, as the glyphs you need may be in an already-existing font. If you already have
a TrueType font with the desired glyphs, you are in luck. The quadratic Bézier curves
used in TrueType can be easily converted to SVG glyphs. Just be sure to include the
standard TrueType font as a fallback option (as a web font or as a reference to a local
font name) for web browsers that don’t support SVG fonts.
The ttf2svg Utility
The Apache Batik project has created a utility that will convert your TrueType fonts to
SVG.  The following summary is adapted from the Batik project’s documentation (copy‐
right 2013 The Apache Software Foundation. All rights reserved).
If you are using the Batik binary distribution, type the following at the command line:
java -jar batik-ttf2svg.jar [options]
If you are using the Batik developer distribution, type the following at the
command line:
build ttf2svg [options]
In both cases, the options are the same (these options will all go on the same line when
typed at the command line; they are placed on separate lines here for ease of reading):
ttf-path
[-l range-being ]
[-h range-end ]
[-ascii]
328 | Appendix E: Creating Fonts

[id id]
[-o output-path ]
[-testcard]
The options have the following meanings:
ttf-path
Specifies the TrueType font file that contains the characters to be converted.
-l range-begin  
-h range-end
The low and high value of the range of characters to be converted to SVG (ASCII
or Unicode values).
-ascii
Forces usage of the ASCII character map.
-id id
Specifies the value for the id attribute of the generated <font>  element.
-o output-path
Specifies the path for the generated SVG font file. If not specified, output goes to
the Java console.
-testcard
Specifies that a set of SVG <text>  elements should be appended to the SVG font
file to visualize and test the characters in the SVG font. This provides an easy way
to validate the generated SVG font file visually.
For example, to convert characters 48 to 57, that is, the characters 0 to 9, in myFont.ttf
into their SVG equivalent in the mySVGFont.svg  file, appending a test card so that the
font can be visualized easily, you would use this command:
java -jar batik-ttf2svg.jar /usr/home/myFont.ttf -l 48 -h 57
   -id MySVGFont -o mySVGFont.svg -testcard
Make sure you have the right to embed a font before you embed it
in an SVG  file. TrueType  font files contain  a flag that defines  the
“embeddability” of a font, and there are tools for checking that flag.
The ttf2svg Utility | 329

APPENDIX F
Converting Arcs to Different Formats
Converting from Center and Angles to SVG
The following JavaScript code converts an arc specified in center-and-angles format to
a form suitable for placing into an SVG <path> :
/*
  Convert an elliptical arc based around a central point
  to an elliptical arc parameterized for SVG.
  Parameters are:
    center x coordinate
    center y coordinate
    x-radius of ellipse
    y-radius of ellipse
    beginning angle of arc in degrees
    arc extent in degrees
    x-axis rotation angle in degrees
  Return value is an array containing:
    x-coordinate of beginning of arc
    y-coordinate of beginning of arc
    x-radius of ellipse
    y-radius of ellipse
    x-axis rotation angle in degrees
    large-arc-flag as defined in SVG specification
    sweep-flag  as defined in SVG specification
    x-coordinate of endpoint of arc
    y-coordinate of endpoint of arc
*/
function  centeredToSVG (cx, cy, rx, ry, theta, delta, phi)
{
  var endTheta , phiRad;
  var x0, y0, x1, y1, largeArc , sweep;
331

  /*
    Convert angles to radians. I need a separate variable for phi as
    radians, because I must preserve phi in degrees for the
    return value.
  */
  theta = theta * Math.PI / 180.0;
  endTheta  = (theta + delta) * Math.PI / 180.0;
  phiRad = phi * Math.PI / 180.0;
  /*
    Figure out the coordinates of the beginning and ending points
  */
  x0 = cx + Math.cos(phiRad) * rx * Math.cos(theta) +
    Math.sin(-phiRad) * ry * Math.sin(theta);
  y0 = cy + Math.sin(phiRad) * rx * Math.cos(theta) +
    Math.cos(phiRad) * ry * Math.sin(theta);
  x1 = cx + Math.cos(phiRad) * rx * Math.cos(endTheta ) +
    Math.sin(-phiRad) * ry * Math.sin(endTheta );
  y2 = cy + Math.sin(phiRad) * rx * Math.cos(endTheta ) +
    Math.cos(phiRad) * ry * Math.sin(endTheta );
  largeArc  = (delta > 180) ? 1 : 0;
  sweep = (delta > 0) ? 1 : 0;
  return [x0, y0, rx, ry, phi, largeArc , sweep, x1, y1];
}
Converting from SVG to Center and Angles
The following code, adapted from the Apache Batik project,  converts an SVG-style arc
to a center-and-angles format:
/*
  Convert an elliptical arc specified as SVG path parameters
  to an arc based around a central point.
  Parameters are:
    x-coordinate of beginning of arc
    y-coordinate of beginning of arc
    x-radius of ellipse
    y-radius of ellipse
    x-axis rotation angle in degrees
    large-arc-flag as defined in SVG specification
    sweep-flag  as defined in SVG specification
    x-coordinate of endpoint of arc
    y-coordinate of endpoint of arc
  Return value is an array containing:
332 | Appendix F: Converting Arcs to Different Formats

    center x coordinate
    center y coordinate
    x-radius of ellipse
    y-radius of ellipse
    beginning angle of arc in degrees
    arc extent in degrees
    x-axis rotation angle in degrees
*/
function  convertArc (x0, y0, rx, ry, xAngle, largeArcFlag ,
  sweepFlag , x, y)
{
  // Step 1: compute half the distance between the current
  // and final point.
  var dx2 = (x0 - x) / 2.0;
  var dy2 = (y0 - y) / 2.0;
  // convert angle from degrees to radians
  var xAngle = Math.PI * (xAngle % 360.0) / 180.0;
  var cosXAngle  = Math.cos(xAngle);
  var sinXAngle  = Math.sin(xAngle);
  // Compute x1, y1
  var x1 = (cosXAngle  * dx2 + sinXAngle  * dy2);
  var y1 = (-sinXAngle  * dx2 + cosXAngle  * dy2);
  // Ensure radii are large enough
  rx = Math.abs(rx);
  ry = Math.abs(ry);
  var rxSq = rx * rx;
  var rySq = ry * ry;
  var x1Sq = x1 * x1;
  var y1Sq = y1 * y1;
  var radiiCheck  = x1Sq / rxSq + y1Sq / rySq
  if (radiiCheck  > 1) {
    rx = Math.sqrt(radiiCheck ) * rx;
    ry = Math.sqrt(radiiCheck ) * ry;
    rxSq = rx * rx;
    rySq = ry * ry;
  }
  // Step 2: Compute (cx1, cy1)
  var sign = (largeArcFlag  == sweepFlag ) ? -1 : 1;
  var sq = ((rxSq * rySq) - (rxSq * y1Sq) - (rySq * x1Sq)) /
    ((rxSq * y1Sq) + (rySq * x1Sq));
  sq = (sq < 0) ? 0 : sq;
  var coef = (sign * Math.sqrt(sq));
  var cx1 = coef * ((rx * y1) / ry);
  var cy1 = coef * -((ry * x1) / rx);
  // Step 3 : Compute (cx, cy) from (cx1, cy1)
Converting from SVG to Center and Angles | 333

  var sx2 = (x0 + x) / 2.0;
  var sy2 = (y0 + y) / 2.0;
  var cx = sx2 + (cosXAngle  * cx1 - sinXAngle  * cy1);
  var cy = sy2 + (sinXAngle  * cx1 + cosXAngle  * cy1);
  // Step 4 : Compute the angleStart and the angleExtent
  var ux = (x1 - cx1) / rx;
  var uy = (y1 - cy1) / ry;
  var vx = (-x1 - cx1) / rx;
  var vy = (-y1 - cy1) / ry;
  var p, n;
  // Compute the angle start
  n = Math.sqrt((ux * ux) + (uy * uy));
  p = ux; // (1 * ux) + (0 * uy)
  sign = (uy < 0) ? -1.0 : 1.0;
  var angleStart  = 180.0 * (sign * Math.acos(p / n)) / Math.PI;
  // Compute the angle extent
  n = Math.sqrt((ux * ux + uy * uy) * (vx * vx + vy * vy));
  p = ux * vx + uy * vy;
  sign = (ux * vy - uy * vx < 0) ? -1.0 : 1.0;
  var angleExtent  = 180.0 * (sign * Math.acos(p / n)) / Math.PI;
  if(!sweepFlag  && angleExtent  > 0)
  {
    angleExtent  -= 360.0;
  }
  else if (sweepFlag  && angleExtent  < 0)
  {
    angleExtent  += 360.0;
  }
  angleExtent  %= 360;
  angleStart  %= 360;
  return( [cx, cy, rx, ry, angleStart , angleExtent , xAngle] );
}
334 | Appendix F: Converting Arcs to Different Formats

We’d like to hear your suggestions for improving our indexes. Send email to index@oreilly.com.Index
Symbols
@keyframes CSS rule, 205, 207
A
a element, 209
A or a command (elliptical arcs), 91
absolute versus relative path notation, 88
addEventListener function, 217
additive attribute, animation elements, 201
combining effects controlling numerical and
color attributes, 202
aliasing, 41
(see also anti-aliasing)
alignment
specifying for preserveAspectRatio, 33–36
text, 129
alpha value
changing with feColorMatrix element, 160
determining for masks, 149
feConvolveMatrix filter and, 183
final, interaction with color and opacity in
masks, 151
for rgba/hsla color functions, 43
luminanceToAlpha filter preset, 162
SourceAlpha filter input, 157
animate element, 192
(see also SMIL animation)animateMotion element, 202
animation along a complex path, 202
animation along a linear path, 202
rotate attribute, 203–204
specifying key points and times for motion,
204
using mpath, 204
animateTransform element, 200
animation, 191
(see also CSS; JavaScript/ECMAScript; SMIL
animation)
basics, 192
frame rate, 252
via scripting, 251–256
animation events (SMIL), 216
animVal and baseVal properties, 233
anti-aliasing, 4, 41
Apache Software Foundation Batik project, xii
application, SVG as, 18
arcs, 90–93
converting to different formats, 93, 331–334
from center and angles to SVG, 331
from SVG to center and angles, 332
path syntax, 91
arrays, in JavaScript, 312
ascent, in typography, 126
aspect ratio, preserving, 32
(see also preserveAspectRatio attribute)
assignment, 310
335

attributeName, animation elements, 192
attributes
accessing and changing using the DOM,
214, 218, 233
in XML markup, 287
presentation, 7, 60
(see also styles)
attributeType, animation elements, 192
B
background-image style, 17
background-position style, 18
background-size style, 18
BackgroundAlpha filter input, 179
BackgroundImage filter input, 179
baseline, 126
baseline-shift style, 132
baseVal and animVal properties, 233
Batik SVG viewer, xii
Bézier curves, 93–99
animation along a cubic Bézier curve path,
202
bidirectional text, 134
bitmaps, 1
temporary, filter operations on, 156
blanks, SVG handling of, 141
border-radius style (HTML), compared with
rectangle corner radius attributes, 47
brightness, changing with feComponentTrans‐
fer, 164
browsers (see web browsers)
bump map, in lighting filters, 175
C
C or c command (cubic Bézier curves), 97
CAD (Computer Assisted Drafting) programs, 3
calcMode attribute, animation elements, 199
cap-height, 126
Cartesian coordinates, converting from, 76, 266
character encodings, 291
other than Unicode, 292
character references, 291
characters, defined, 125
circles, 6, 47
SVGCircleElement object and <circle> ele‐
ment, 233
class attribute, 301classes (CSS), 300
(see also pseudoclasses)
click events, 216, 217
click handler in Snap.svg, 262
in user-triggered SMIL animations, 212–213
clientX and clientY event properties, 217
clip-path style, 145
clipPath element, 145
basic shapes, path elements, or text elements
in, 146
clipPathUnits attribute, 147
clipping area, 145
clipping graphics, 145–149
closepath command, 86–88, 88
code point, in character encodings, 291
color, 41, 54
color style, 42
colors
animating, 197, 198
changing with feColorMatrix, 160–163
CSS3 color specification, 42
defining the color space, 169
fill color, 45, 303
gradients, 113–122
interaction with opacity and final alpha val‐
ue in masks, 151
mask color values and transparency, 149
masking with opaque colors, 150
stroke color, 41, 303
comments
in ECMAScript, 312
in XML, 289
conditional statements, in scripting, 312
(see also switch element)
constants, 309
coordinate system, transforming, 9, 69–84
converting from Cartesian coordinates, 76
reference summary of transformations in
SVG, 83
rotate transformation, 78
scale transformation, 71
scaling around a center point, 81
sequences of transformations, 74
skewX and skewY transformations, 81
translate transformation, 69
coordinates, 27–38
absolute or relative, moveto and lineto com‐
mands, 88
default user coordinates, using, 28
336 | Index

nested systems of, 36
preserving aspect ratio, 32
specifying for lines, 39
specifying user coordinates for a viewport,
27, 30
CSS, 299
animating SVG with, 205–208
animating movement, 207
animation properties, 206
controlling using pseudoclasses, 211
setting animation key frames, 207
attributeType for animation elements, 192
CSS3 color specification, 42
properties
for img element in a web page, 17
for svg element inline in (X)HTML, 22
for text, 127
SVG file as value of, 17
table of properties and values for SVG,
301
pseudoclasses, 210
transformations and SVG, 83, 200
CSS sprites, 18
cubic Bézier curves, 96
currentColor keyword, 42
D
d (data) attribute, path element, 85
dashed lines, 44
data objects in SVG DOM, table of, 236
declarative interactivity, 209
defer specifier (preserveAspectRatio, 68
defs element, 63
desc element, 6
descent, in typography, 126
diffuse lighting, 175
document events, 216
document object
getElementById function, 214
getElementsByTagName function, 214
document structure, 57–68
basic, in SVG, 6
grouping and referencing objects, 61–68
structure and presentation, 7, 57
using styles with SVG, 58–61
Document Type Definitions (DTD), 293
DOM (Document Object Model), 214
accessing SVG with, 215
DOM-based XML parsers, 296events, 217
compared with CSS pseudoclasses, 212
in Snap.svg, 261
to control SMIL animation, 212–213
mutation events, 216
SVG DOM methods and properties, 233–
263
animVal and baseVal properties, 233
constructing SVG with ECMAScript/
JavaScript, 248
data objects and methods, table of, 236
determining value of element attributes,
233
SVG interface methods, 241
with JavaScript libraries, 256
dragging objects, 221, 262
drop shadow, creating, 156–159
DTD (Document Type Definitions), 293
dur (duration) attribute, animation elements,
199
E
ECMAScript, 213
(see also JavaScript/ECMAScript)
elements
creating new, with scripts, 229
reference tables
basic shapes, 53
DOM interfaces, 241
filters, 187
path commands, 99
XML, 287
ellipses, 48
elliptical (see arcs)
embed element, 19
embossing effect, using feConvolveMatrix, 183
enable-background attribute, 179
end attribute, animation elements, 195
entity references (XML), 290
escaping characters, 289
event-based interaction, 212
events, 216
categories of, 216
compared with CSS pseudoclasses, 212
event handling in Snap, 261
clicking objects, 262
dragging objects, 262
listening for and responding to, 217, 223
to control SMIL animation, 212–213
Index | 337

ex-height, 126
examples in this book, about, xii
Extensible Stylesheet Language Formatting Ob‐
jects (see XSL-FO files)
F
feBlend filter, 172
feColorMatrix filter, 160–163
feComponentTransfer filter, 164–169
gamma adjustment with, 165
feComposite filter, 169, 178
feConvolveMatrix filter, 182
embossing effect, 183
feDiffuseLighting filter, 175
feDisplacementMap filter, 184
feFlood and feTile filters, 173
feFuncR, feFuncG, feFuncB, and feFuncA ele‐
ments, 164
feGaussianBlur filter, 157
feImage filter, 163, 180
feMerge filter, 169
feMergeNode element, 158, 169
feMorphology filter, 181
feSpecularLighting filter, 177
feSpotLight filter, 179
feTurbulence filter, 186
files, external
for patterns and gradients, 123
for stylesheets, 59
for SVG fonts, 137
in SVGs inserted in web pages, 15, 18
with use element, 65
fill attribute, animation elements, 196
fill characteristics, table of, 55
fill style
for text, 126
specifying color, 7, 45
using gradients, 113–122
using patterns, 107–113
with polyline element, 51
fill-opacity style, 45, 55
fill-rule style
for paths, 100
for polygons with intersecting lines, 49
filters, 155–189
accessing the background, 179
creating a drop shadow, 156
establishing filter’s bounds, 156storing, chaining, and merging filter re‐
sults, 158
using feGaussianBlur, 157
creating a glowing shadow, 159
how they work, 155
lighting effects, 174
diffuse lighting, 175
specular lighting, 177
reference summary, 187
resolution of, 156
filterUnits attribute, 157
Flash, 3
focal point (radial gradients), 119
:focus pseudoclass, 210
font-face element, 137, 327
attributes for (table), 327
font-face-name element, 138
font-face-src element, 138
font-family style, 127
matching value in external font file, 137
font-size style, 127
font-style style, 128
font-weight style, 128
fonts
creating, 327–329
defined, 126
font family and font size for text label, 12
using a custom font, 137
for loop, 313
foreign objects, instream-foreign-object element
in XSL-FO, 25
foreignObject element, 20
frame rate, 252
functions, 314
G
g element (group), 8, 61
generating SVG, 265–282
converting custom data to SVG, 266–270
using XSLT to convert XML data to SVG,
270–282
getSVGDocument function, 229
glyph-orientation-vertical style, 133
glyphs, 125
gradients, 113–122
color values between gradient stops, 169
linear, 113
establishing a transition line, 115
spreadMethod attribute, 116
338 | Index

stop element, 113
radial, 118
spreadMethod attribute, 120
transition limits for, 118
using as a mask, 153
reference summary, 121
transforming, 122
using as a displacement map, 184
gradientTransform attribute, 122
gradientUnits attribute, 116, 120, 122
graphics programs, drawing Bézier curves, 94
graphics systems, raster versus vector, 1–3
grouping, 8, 61
transforming the group, 73
using symbol element, 66
within defs element, 63
within use element, 63
H
H or h command (horizontal lines), 88
horizontal lineto command, 88
:hover pseudoclass, 210
hsl and hsla color functions, 43
HTML (see HTML)
handling of whitespace, 142
inline SVG in, 22
inline SVG in HTML5, 22
interacting with embedded SVG, 225–229
SVG foreign object containing XHTML text,
20
I
icons within a single image file, 18
image element, 67
images
feImage filter, 163
masking, 152
img element, including SVG in, 16
inline styles, 58, 299
modifying, 214
stylesheets versus, 60
inline SVG, in XHTML or HTML5, 22–25
instream-foreign-object element, 25
interactivity, adding, 209–231
controlling CSS animations, 211
scripting SVG, 213–231
user-triggered SMIL animation, 212
using links in SVG, 209interfaces for SVG elements, 241
internationalization, text and, 134–138
Internet media types, 18
J
JavaScript/ECMAScript, 213–231
animation with, 251–256
animVal property, 233
requestAnimationFrame function, 252
using Snap.svg, 259
attributes, accessing and changing, 214, 218
creating new elements, 229
dragging objects, 221
events
listening for and responding to, 217
overview, 216
interacting with an HTML page, 225–229
getSVGDocument function, 229
programming concepts, 309–317
styles, accessing and modifying, 214
using code libraries, 256–261
K
key events, 217
key frames, setting for animation, 207
keyPoints attribute, animateMotion element,
205
keyTimes attribute
animateMotion element, 204
timing of multistage animations, 199
L
L or l command (lineto) (see lineto command)
languages
switching, 135
Unicode and bidirectional text, 134
letter-spacing style, 128, 133
lighting effects, 174
diffuse lighting, 175
specular lighting, 177
line caps and joins, 52
line element, 8, 39
stroke characteristics, 40–45
linear gradients, 113
establishing a transition line, 115
using spreadMethod attribute, 116
specifying stop-opacity, 114
Index | 339

stop element in, 113
linear RGB color space, 169
lineto command, 85
horizontal and vertical lineto commands, 88
multiple pairs of coordinates after, 89
relative coordinates, 88
links in SVG, 209
highlighting with SVG pseudoclasses, 210,
211
loops, 313
M
M or m command (moveto), 88
marker element, 100–105
marker-end style, 102
marker-mid style, 102
marker-start style, 101
orient attribute, 103
setting viewBox and preserveAspectRatio to
control display, 104
markerUnits attribute, marker element, 104
mask element, 149
mask style, 150
maskContentUnits attribute, 149
masking graphics, 149–154
maskUnits attribute, 149
matrix algebra, 319–326
matrix multiplication, 320
matrix terminology, 319
use by SVG for transformations, 323
measurement units
explicit use of, 29
specifying for the viewport, 27
meet specifier (preserveAspectRatio), 34
metadata, 287
methods, of JavaScript/ECMAScript objects,
315
MIME types, 18
mouse events, 216
adding mouse movement listeners, 217
moveto command, 85
multiple pairs of coordinates after, 89
relative coordinates, 88
mpath element, using in animateMotion ele‐
ment, 204
mutation events, DOM, 216N
namespaces, 10, 295
nested markers, 105
nested patterns, 112
newlines, SVG handling of, 141
nodes, 214
none specifier (preserveAspectRatio), 36
notational shortcuts (paths), 89
number function, XSLT, 278
O
object element, HTML, 18, 225
embed element versus, 19
objectBoundingBox units
stroke and, 123
with clipPathUnits, 147
with filterUnits, 157
with gradientUnits, 116, 122
with maskContentUnits, 149
with maskUnits, 149
with patternContentUnits, 110
with patternUnits, 108
with primitiveUnits, 157
objects, in scripting, 315
object interfaces for SVG elements, 233, 242
SVG data objects (table), 236
offset attribute, stop element, 113
opacity
fill-opacity style, 45
interaction between color, opacity, and final
alpha value in masks, 151
stop-opacity style, gradient stops, 114
stroke-opacity style, 43
operators, in JavaScript/ECMAScript, 311
orient attribute, marker element, 103
origin (coordinate system), 28
rotation around, 79
overflow style, 148
P
paced animation, 204
parsers (XML), 296
path element, 85
paths, 11, 85–105
animating, 198
Bézier curves, 93–99
clipping to, 145
340 | Index

closepath command, 86
converting from other arc formats, 93
elliptical arcs, 90
filling, 100
for a pattern tile, 107
key points for animateMotion element, 205
lineto command, 85
marker element, 100–105
moveto command, 85
reference summary of commands, 99
relative moveto and lineto, 88
shortcuts, 88
horizontal and vertical lineto commands,
88
notational shortcuts, 89
text on, 138
patternContentUnits attribute, 110
patterns, 107–113
nested, 112
tiles, 107
transforming, 122
patternTransform attribute, 122
patternUnits attribute, 108
pluginspage attribute, embed element, 19
points attribute, polygon and polyline elements,
48, 51, 51
poly-Bézier curve, 95
polygon element, 48–51
animating, 198
ArcInfo Geographic Information in, 266
having intersecting lines, filling, 49
polyline element, 10, 51, 89
PostScript, 3
presentation
as attributes, 60
versus structure, 7, 57
preserveAspectRatio attribute
default value, 34
defer specifier, 68
meet specifier, 34
none specifier, 36
slice specifier, 35
specifying alignment, 33
using to scale a pattern, 111
with marker element, 104
preserveAspectRation attribute
defer specifier, 68
primitives, filter, 156
primitiveUnits attribute, filter element, 157programming concepts, 309–317
arrays, 312
assignment and operators, 310
comments, 312
conditional statements, 312
constants, 309
functions, 314
objects, properties, and methods, 315
repeated actions, 313
variables, 310
properties
CSS property table for SVG, 302
of JavaScript/ECMAScript objects, 316
of SVG element objects, 233
pseudoclasses, 210–212
Q
Q or q command (quadratic Bézier curves), 95
quadratic Bézier curves, 94
R
radialGradient element, 118
establishing transition limits for, 118
spreadMethod attribute, 120
using as a mask, 153
raster graphics, 1
and filter effects, 155
scalability, lack of, 4
uses of, 2
rectangles, 45–47
default attributes, 46
rounded, 46
SVGRectElement object, 233
using explicit units, 30
referencing external files (see files, exteral)
relative coordinates, in path commands, 88
repeated actions, in JavaScript/ECMAScript,
313
repeated animations, 196
alternating between two values, 199
requestAnimationFrame function, 252
requiredExtensions attribute, 21
requiredFeatures attribute, 21
reusing graphics with use element, 63
using defs element, 63
using symbol element, 66
rgb color function, 41
rgba color function, 43
Index | 341

rotate transformation, 78
S
S or s command (smooth cubic Bézier curve),
98
scale transformations, 9, 71
scale followed by translate, 75
scaling around a center point, 81
translate followed by scale, 75
when converting from Cartesian coordi‐
nates, 78
scripts (see JavaScript/ECMAScript)
set element, 200
setTimeout function, 253
shape-rendering style, 41
shapes
basic, 6, 39–55
circles, 7
circles and ellipses, 47
line caps and joins, 52
lines, 8, 39–45
paths, 11
polygon element, 48–51
polyline element, 10, 51
rectangles, 45–47
summary table, 53–55
in clipPath element, 146
transformation applied to, 74
skewX and skewY transformations, 81
slice specifier (preserveAspectRatio), 35
SMIL animation, 192–205
animate element, 192
animateMotion element, 202–204
animateTransform element, 200
animation events, 216
beginning and ending of, 193
complex attributes, 197
key points and times for motion, 204
key times and calculation, 199
multiple animations on an object, 193
multiple values, 198
repeated action, 196
set element, 200
synchronizing, 194
time measurement, 194
user-triggered, 212–213
SMIL3 specifiation, 192
smooth cubic curve command, 98
smooth quadratic curve command, 95Snap.svg library (JavaScript), 256
animate function and easing options, 259
drawing graphics, 257
event handling in, 261
clicking objects, 262
dragging objects, 262
including the library script, 256
Paper object, 256, 258
specular lighting, 177
spotlight filter, 179
spreadMethod attribute, 116, 122
for linear gradients, 116
for radial gradients, 120
sprites, CSS image, 18
src attribute, img element, 16
standard RGB (sRGB) color space, 169
startOffset attribute, textPath element, 140
state-based interaction, 212
stop element, 113
stop-color attribute, 113
stop-opacity style, 114
stroke characteristics, table of, 54
stroke style
for rectangles, 45
for text, 126
path closure and, 87
position relative to shape coordinates, 46
scale transformations and, 72
nonuniform scaling, 73
specifying color, 7, 41
using gradients and patterns, 123
stroke-dasharray style, 44
stroke-linecap style, 52
stroke-linejoin style, 52
stroke-miterlimit style, 53
stroke-opacity style, 43
stroke-width style, 40
scaling transformations and, 81
structure (see document structure)
style attribute, 7, 299
styles, 58–61, 299
font family and font size for text, 12
inline, 58, 299
presentation attributes versus, 7, 60
table of style properties for SVG, 301
text, changing with tspan element, 130
stylesheets, 299–308
classes as style selectors, 300
external, 59
342 | Index

inline styles versus, 60
internal, 58, 300
XSLT stylesheet, 273
subscripts and superscripts
using baseline-shift with tspan element, 132
using dy attribute of tspan, 130
SVG
defined, 1
role of, 5
scalability of vector graphics, 3
svg element, 6
viewBox attribute, 30
width and height attributes, 27
xmlns:xlink attribute, 10
SVGAngle object, 237
SVGAnimatedLength object, 236, 240
SVGCircleElement object, 233
SVGEllipseElement object, 235
SVGLength object, 237
SVGMatrix object, 238
SVGPoint object, 238
SVGRect object, 237
SVGRectElement object, 233
SVGTransform object, 239
SVGTransformList object, 239
switch element, 20, 135
symbol element, 66
synchronization of animations, 194
with repetition, 197
Synchronized Multimedia Integration Language
(see SMIL animation)
systemLanguage attribute, 21, 135
T
T or t command (smooth quadratic curves), 95
tabs, SVG handling of, 141
target property, events, 217
text, 125–143
adding to a graphic, 12, 142–143
alignment, 129
CSS styles and values for, 127
drop shadow applied to, 159
glowing shadow for, 159
in vector versus raster graphics, 2
internationalization, 134
custom font, 137
switching languages with switch element,
135
Unicode and bidirectionality, 134lengthAdjust attribute, 132
modifying text content of a node, 214
on a path, 138
overview, 126
terminology, 125
text elements in clipPath element, 146
textLength attribute, 132
tspan element, 129
(see also tspan element)
vertical, 133
whitespace and, 141
text element, 12
text-anchor style, 129, 140
text-decoration style, 128
textPath element, 138
referencing a path element in, 138
startOffset attribute, 140
this keyword, 218
tiles, 107
(see also patterns)
feTile filter, 173
spacing in patterns, 108
time measurement for animation, 194, 212
timing of animations, 199
title element, 6
transform style (see transformations, CSS)
transformations, 9, 69
(see also coordinate system, transforming)
animateTransform element, 200
converting from Cartesian coordinates, 76
CSS, 83, 207
matrix used for, 238
of patterns and gradients, 122
rotate transformation, 78
scale transformation, 71
sequences of, 74
skewX and skewY transformations, 81
summary table, 83
use of matrix algebra by SVG for, 323
transition limits, in gradients
and spreadMethod attribute, 116
for radial gradients, 118
transition line for linear gradients, 115
translate transformation, 9, 69
in Cartesian coordinates conversion, 78
scale followed by translate, 75
translate followed by scale, 75
using CSS, 208
transparency (see alpha value; opacity)
Index | 343

transparent specifier (CSS3), 43
tspan element, 129
rotate attribute, 131
using absolute positioning with, 130
using baseline-shift, 132
using multiple values for dx, dy, and rotate
attributes, 131
vertical positioning with dy attribute, 130
ttf2svg utility, 328
type attribute, object element, 18
U
Unicode, 134
code points, 291
encoding schemes, 291
use element, 63
height and width attributes, 66
including SVG file with, 65
moving a graphic with, 69
transform attribute, 69
xlink:href attribute, 210
user interface events, 216
userSpaceOnUse setting
for clipPathUnits, 148
for filterUnits, 157
for gradientUnits, 116, 120, 122
for markerUnits, 104
for maskContentUnits, 149
for maskUnits, 149
for patternContentUnits, 110
for patternUnits, 109
for primitiveUnits, 157
V
V or v command (vertical lines), 89
validity (XML), 293
values attribute, animation elements, 198
variables, 310
assigning values to, 310
vector graphics, 2
scalability, 3
uses of, 3
vertical lineto command, 88
viewBox attribute
clipping content to, 148
marker element, 104
svg element, 30
using to scale a pattern, 111viewport, 27
aspect ratio, 32
clipping area, 145
clipping content to, 148
nested, 37
possible declarations, 28
specifying user coordinates for, 30
W
web browsers
and SVG used as an image, 16
background filter inputs and, 180
file types, determining support for, 18
foreign object support, 20
Internet Explorer and SVG animation, 191
support for SVG as images, 17
SVG fonts and, 138
SVG support, 6
web pages, using SVG in, 15–25
foreign objects in SVG, 20
including SVG in an img element, 16
inline SVG in (X)HTML, 22–25
SVG as an application, 18
SVG files as CSS property values, 17
SVG in other XML applications, 25
well-formed XML, 288
while loop, 314
whitespace
in path commands, 90
SVG handling of, in text, 141
word-spacing style, 128
World Wide Web Consortium Recommenda‐
tion (SVG), 5
writing-mode style, 133
X
xlink:href attribute
a element, 209
feImage element, 164
image element, 67
linearGradient and radialGradient elements,
115
mpath element, 204
textPath element, 138
use element, 63
XML, 285–297
attributeType in animation elements, 192,
200
344 | Index

character encodings, 291
character references, 291
comments, 289
defined, 285
document structure, 286
for SVG graphic, 6
Document Type Definitions (DTDs), 293
elements and attributes, 287
entity references, 289
linking a document to its DTD, 294
name syntax, 288
namespaces, 6, 10, 295
parsers, 296
SVG as XML application, 5
SVG in other XML documents, 25
validity, 293
well-formed, 288
XML declaration, 286xml:space attribute, 141
xmlns attribute, 295
svg element, 6
xmlns:xlink attribute, 10
XPath, 275, 276, 281, 296
XSL-FO files, xi
SVG in, 25
XSLT (Extensible Stylesheet Language Transfor‐
mations), 266
converting XML data to SVG, 270–282
how XSLT works, 272
objectives, 270
stylesheet structure, 273
processors, 296
Z
Z command (closepath), 86
Index | 345

About the Authors
J. David Eisenberg  is a programmer and instructor living in San Jose, California. David
has a talent for teaching and explaining. He has developed courses for HTML and CSS,
JavaScript, XML, and Perl. He teaches computer and information technology courses
at Evergreen Valley College in San Jose. He has also developed online courses providing
introductory tutorials for Korean, Modern Greek, and Russian. David has been devel‐
oping education software since 1975, when he worked with the Modern Foreign Lan‐
guage project at the University of Illinois to develop computer-assisted instruction on
the PLATO system. He is coauthor of Introducing Elixir . When not programming, David
enjoys digital photography, caring for a feral cat colony at work, and riding his bicycle.
Amelia Bellamy-Royds  is a freelance writer specializing in scientific and technical
communication. She helps promote web standards and design through participation in
online communities such as Web Platform Docs, Stack Exchange and Codepen. Her
interest in SVG stems from work in data visualization and builds upon the programming
fundamentals she learned while earning a BSc in bioinformatics. A policy research job
for the Canadian Library of Parliament convinced her that she was more interested in
discussing the big-picture applications of scientific research than doing the laboratory
work herself, leading to graduate studies in journalism. She currently lives in Edmonton,
Alberta. If she isn’t at a computer, she’s probably digging in her vegetable garden or out
enjoying live music.
Colophon
The animal on the cover of SVG Essentials  is a great argus pheasant ( Argusianus ar‐
gus). This pheasant can be found in Malaysia, Thailand, Sumatra, and Borneo, where it
lives in tropical rainforests. The males have blue faces, black crowns, and short crests;
their underparts are mottled brown. The iridescent spots on their wings and tail feathers
aid in attracting females. Female argus pheasants are smaller than males and lack their
ornate plumage.
The great argus pheasant’s wings can continue to grow into the bird’s sixth year. Its tail
feathers are the longest of all birds, measuring up to 5.7 feet. Some cultures use these
feathers in their headdresses.
Many of the animals on O’Reilly covers are endangered; all of them are important to
the world. To learn more about how you can help, go to animals.oreilly.com .
The cover image is a 19th-century engraving from the Dover Pictorial Archive. The
cover fonts are URW Typewriter and Guardian Sans. The text font is Adobe Minion
Pro; the heading font is Adobe Myriad Condensed; and the code font is Dalton Maag’s
Ubuntu Mono.