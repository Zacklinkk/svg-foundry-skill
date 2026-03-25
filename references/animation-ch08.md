Chapter 6. Chapter 6: Animating Data Visualizations
Data Visualizations are an extremely useful way to present myriad information. Luckily, due to the relative popularity of some libraries such as d3 and chartist, small pieces of animation are not only possible, but easy to achieve if you’ve been following so far. These are by far not the only libraries that can create data visualizations, but there are so many to choose from that I picked my favorite of the many that I’ve worked with.

Chartist, at the time of publishing, uses the now-deprecated SMIL to animate, so I don’t recommend that you use their native animations functions. D3 also offers some native animations, but you may find that now that you’ve learned some CSS implementations, it may be simpler and certainly more performant to draw the data visualization on the screen, and then animate it.
Chartist vs. D3 for Configuration
p.p1 {margin: 0.0px 0.0px 12.8px 0.0px; line-height: 15.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

It’s very simple to create responsive charts and graphs in Chartist, making it very beginner-friendly. The library creates a wrapper for the SVG, so some JavaScript functionality becomes a little obfuscated and less straightforward as one might think. For this reason, I strongly suggest using Chartist to draw up a simple graphs with simple CSS animations.

D3.js, on the other hand, is not quite as beginner friendly but very easy to work with and make extendable. The sky is the limit on what you can create in D3, which has made it the library of choice for many beautiful data visualizations across the web.

Simply put, there’s no one right way, and you should work with what works for your workflow, site implementation, and even what you’re the most excited about.

Teaching how to work with either Chartist or D3 to build charts and graphs is out of the scope of this book, but Chartist has wonderful live, interactive documentation, and there’s another incredible O’Reilly book that’s a great resource for learning D3, Interactive Data Visualization. http://chimera.labs.oreilly.com/books/1230000000345 I used this book to learn this technology and I can’t recommend it highly enough.
Chapter 6. Why Use Animation in Data Visualization?
Animation in data visualization can be extremely powerful as a performant piece of the data’s structure. Here are a few ways that animation can aid a data visualization:

- Filtering

- Reordering

- Storytelling

- Change over time

- Staggering pieces for clarity

p.p1 {margin: 0.0px 0.0px 12.8px 36.0px; text-indent: -36.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

In the last chapter we discussed the importance of retaining context for users. Filtering data allows us to retain consistent *elements*, while shifting their *meaning* by rearranging them. Consider this data visualization:

http://www.nytimes.com/interactive/2012/02/13/us/politics/2013-budget-proposal-graphic.html

p.p1 {margin: 0.0px 0.0px 12.8px 72.0px; text-indent: -72.0px; line-height: 17.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

The New York Times presents the same data in many different contexts, allowing the user to process the information in an extremely powerful, multi-dimensional way. The user has greater insight into the information by seeing it in a variety of different contexts, while the area of the representation remain unchanged.

Even the most informative data lacks meaning if it is not engaging.1 That’s why storytelling is extremely important when it comes to data visualization.

p.p1 {margin: 0.0px 0.0px 12.8px 72.0px; text-indent: -72.0px; line-height: 15.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

I live in San Francisco where there is an ongoing housing crisis. Many families are being thrown out of their homes through a loophole in the law called the Ellis Act. The Ellis Act evictions are shown very powerfully in the data visualization below, by showing the change in evictions over time in a timeline. We’ll learn how to make interactive timeline animations like this one in Chapter 11.

http://www.antievictionmappingproject.net/ellis.html

p.p1 {margin: 0.0px 0.0px 12.8px 72.0px; text-indent: -72.0px; line-height: 15.0px; font: 18.0px Times; color: #232323}
span.s1 {font-kerning: none}

If we look under the hood, we can see that that it’s all SVG: the visualization is hiding and displaying the bursts of evictions depending on when they occur are in the timeline. This linear story is very effective, as more and more locations on the map “explode” in the animation.

Let’s build out our own small version of something like this so you can see how it’s done.
Chapter 6. D3 with CSS Animation Example
As a very beginning starting point, D3 has a ton of nice blocks that you can work with and modify. Blocks are demos that often show the code and implementation details of a D3 example. Take care: blocks are not part of the library; they are examples individual contributors have posted, and licenses and versions may vary.

You may find that beautiful as the readymade blocks may be, you still need to style them for your own site or animate them to bring them to life.

In our case, we chose a map of 3000 Walmart locations across the US:

With just a few styles and a few simple SCSS functions, we can convert this static document into something that presents itself progressively:

http://codepen.io/sdras/full/qZBgaj/

p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 12.0px Arial}

In order to change the base styling, we will need classes to distinguish the different types of SVG paths. In this case, the D3 code already assigns the necessary classes using the .attr() function.
Chapter 6. JavaScript:
svg.append("path")
      .datum(topojson.feature(us, us.objects.land))
      .attr("class", "land")
      .attr("d", path);

  svg.append("path")
      .datum(topojson.mesh(us, us.objects.states, function(a, b) { return a !== b; }))
      .attr("class", "states")
      .attr("d", path);

  svg.append("g")
      .attr("class", "hexagons")
    .selectAll("path")
Chapter 6. SCSS
svg {
  position: absolute;
  left: 50%;
  margin-left: -500px;
}

path {
  fill: none;
  stroke-linejoin: round;
}

.land {
  fill: #444;
}

.states {
  stroke: #555;
}

It doesn’t make much sense to add an extra class on every hexagon path in order to animate them as we can use the nth-child selector. Sass also helps us create a stagger in our animations by allowing us to create a function. We set the hexagons to opacity: 0; initially in order to bring them in slowly:

.hexagons path {
  opacity: 0;
}

$elements: 2000;
@for $i from 0 to $elements {
  .hexagons path:nth-child(#{$i}) {
    $per: $i/50;
    animation: 2s #{$per}s ease hexagons both;
  }
}

@keyframes hexagons {
  100% {
    opacity: 1;
  }
}

The result is a pretty slim amount of code for a beautiful and exciting way to progressively show data. For a timeline showing progression, please refer to further chapter __, where we tie a GSAP timeline together with jQuery UI pieces to create interaction and progression.
Chapter 6. Chartist With CSS Animation Example
Let’s also make a simple Chartist example for comparison. Working from the point where we have a full line chart that’s styled for our needs, we’ve decided it would be most interesting to have these lines animate in. This allows for the user to see the data unveil itself, and the staggering pieces are easier for our user to process.

In order create the illusion of an SVG drawing, we need to get the length of the SVG path, which we can do with .getTotalLength(). For a full list of SVG path interface operations, MDN has a great resource: https://developer.mozilla.org/en-US/docs/Web/API/SVGPathElement

setTimeout (
  function() { 
    var path = document.querySelector('.ct-series-d path');
    var length = path.getTotalLength();
    console.log(length);
  }, 
3000);

//output
a: 1060.49267578125
b: 1665.3359375
c: 1644.7210693359375
d: 1540.881103515625

We’re going to use that data to animate the path in. We can make it look like it’s drawing itself with CSS.

First, let’s set a stroke-dasharray on one of the paths

.ct-series-a {
  fill: none;
  stroke-dasharray: 20;
  stroke: $color1;
}

That can be as long as we want it to be. We can also set a stroke-dashoffset, which can also be as long as we want it to be. As the name suggests, stroke-dashoffset offsets the stroke by any amount, and, thankfully, it’s also an animatable property.

We can now use the console output and our little table to create an animation that takes the full length of the whole stroke and also offsets it by that much. This makes our data visualization look like it was drawn into the viewBox. We are also using the same information a few times, so we can use a mixin to dry it out. We also have different values for the dashoffset and dasharray, so to keep it dry, we animate to 0 instead of the other way around.

@mixin pathseries($length, $delay, $strokecolor) {
  stroke-dasharray: $length;
  stroke-dashoffset: $length;
  animation: draw 1s $delay ease both;
  fill: none;
  stroke: $strokecolor;
  opacity: 0.8;
}

.ct-series-a {
  @include pathseries(1093, 0s, $color1);
}

@keyframes draw {
  to {
    stroke-dashoffset: 0;
  }
}

http://codepen.io/sdras/full/oxNmRM

This is just one way we can animate chartist visualizations, you can see many examples on their page here, https://gionkunz.github.io/chartist-js/examples.html- and the way you code entrances, exists, and persistent states in Chartist and CSS is entirely up to you- the sky’s the limit.

We’re going to get into even more exciting ways of working with data visualization in future chapters, but for that we’ll need to learn how to work with JavaScript. Up next is a quick comparison of animation techniques, and then we’ll switch languages.
Chapter 6. Animating with D3
In this section we’ll go over the simplest possible example of how to animate with d3 instead of CSS. We’ll use version 4 for these examples (versions 3 and 4 have breaking changes between version, so using version 3 of the library will not work with this example.)

If you recall from the first chapter that a polyline is a series of points plotted on an x,y coordinate, you can also see why it might be so useful for a very simple data visualization. If you look at the code below, you can probably make sense of it, with your prior knowledge of SVG:

var line = d3.line();
var data1 = [[0, 0], [200, 300], [400, 50], [500, 300], [550, 300], [600, 50], [700, 120], [775, 250]];
var data2 = [[0, 100], [220, 120], [300, 250], [500, 10], [520, 120], [575, 250], [600, 300], [775, 50]];

d3.select('#path1')
  .attr('d', line(data1))
  .transition()
  .attr('d', line(data2))
  .delay(1000)
  .duration(3000)
  .ease(d3.easeBounce);

We set the line to d3.line() method, we set the attribute of that line to the x and y coordinates of two fields of data. We then call transition between the two states of the line attributes, and optionally we can declare delays, durations, and eases.

The code above will transition this line from one state to another:

You can apply this same method of animating with other things as well- colors, coordinates, you name it. D3 is very flexible this way.
Animating Different Path Point Amounts
Even though d3 is flexible in that it allows for most things that SVGs are capable of, so there is almost no end to what you can make, SVGs are pretty finicky about animating between different path values, so D3 inherits that quirk. If our second dataset had a different length than the first, we’d find the transition effect to be pretty unweildy, ugly, or just fail entirely. That’s why GreenSock’s MorphSVG is extremely handy, and would work for this as well (see Chapter 12 for more details.)

There is also a library built outside of d3 that allows for graceful path animations, called d3-interpolate-path. The repo is available here:

https://github.com/pbeshai/d3-interpolate-path and there’s a nice blog post about it as well:

https://bocoup.com/weblog/improving-d3-path-animation

Staggers are pretty easy in d3, and shares similarities with CSS- in that you calculate a new delay for each element. If you’re familiar with for loops in JavaScript, this implementation will likely look familiar to you:

transition.delay(function(d, i) { return i * 10; });

If we were to use this in a color interpolation, it would look something like this: (we’ll update the last code sample to a scatterplot so that the colors are easier to see)

var dataset = [ 5, 17, 15, 13, 25, 30, 15, 17, 35, 10, 25, 15],
    w = 300,
    h = 300;

//create svg
var svg = d3.select("body")
            .append("svg")
            .attr("width", w)
            .attr("height", h);

//create shapes in svg with data
svg.selectAll("circle")
   .data(dataset)
   .enter()
   .append("circle")
   .attr("class", "circles")
   .attr("cx", function(d, i) {
      return 10 + (i * 22)
    })
   .attr("cy", function(d) {
     return 200 - (d * 5)
   })
   .attr("r", function(d) {
     return (d / 2)
   })
  .transition()
    .style("fill", "teal")
    .duration(1500)
    .delay(function(d, i) { return i * 100; });

Codepen example available here:

http://bit.ly/2fpuPe3
Chapter 6. Chaining and Repeating
For more complex effects, we could also add multiple transitions and even create loops. To chain animations, we would add a .transition() method between two states as we did before, but to make the whole thing repeat, we would have update our syntax a little and use some recursion. Here’s an example of both:

.transition()
    .on("start", function repeat() {
        d3.active(this)
            .style("fill", "purple")
          .transition()
            .style("fill", "blue")
          .duration(2000)
          .transition()
          .duration(2000)
            .on("start", repeat);
      });

Codepen here:

http://bit.ly/2goB8mh

Please keep in mind that if you’d like to create very complex chaining or interaction, you might consider switching to GreenSock for animation. We’ll cover GreenSock in some later chapters and you’ll find that it plays nicely with D3’s output while giving you a lot more finite control of timelines and sequencing.

1 See “Storytelling in data visualization” by Emma Whitehead and Tobias Sturt, from the Graphical Web, 2014 https://www.youtube.com/watch?v=cmmjTLCyqlY