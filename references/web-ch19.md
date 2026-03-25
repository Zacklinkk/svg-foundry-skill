Part III. Putting Graphics in Their Place
In Parts I and II, we described the position and geometry of shapes and text using *x* and *y* coordinates.  But so far, we have not looked too closely at how those numbers are converted into positions within our SVG drawing.

The *x* and *y* coordinates used to position SVG shapes and text are measured relative to an overall coordinate system.  By manipulating the coordinate system, you can draw the same shapes at different positions and scales, or even stretched or slanted.

The next few chapters examine the SVG coordinate system more closely, and introduce SVG’s structural markup.  These structural elements allow you to not only control coordinate systems, but also to duplicate and embed graphical content.  Together, these tools allow you to create reusable graphics, such as you would want for an icon set or charting library.