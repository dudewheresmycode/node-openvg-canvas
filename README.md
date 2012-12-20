# node-openvg-canvas
## Canvas implementation on node-openvg

This module implements a HTML5 Canvas on top of OpenVG (node-openvg). It is targeted to the raspberry-pi.
By using the OpenVG APIs on the Raspberry PI, all graphics code will run on the GPU with hardware acceleration.

This library aims for API compatibility with [node-canvas](https://github.com/learnboost/node-canvas) where it applies and makes sense. While node-canvas is targeted to create images off screen, node-openvg-canvas is targeted to main screen usage, but not yet for user interaction.

Currently there are only plans to implement the 2d context. Implementing the 3d context (web gl) should be possible by mapping OpenGL/ES.

## 0. Installation

This module is targeted for node 0.8.x. Node >= 0.8.10 will compile out of the box on the raspberry. For building instructions please refer to this [gist](https://gist.github.com/3245130).

### Prerequisites

node-openvg-canvas depends on freetype and freeimage. To install these libraries
on the raspberry - assuming a raspbian distribution - use:

    sudo apt-get install libfreetype6 libfreetype6-dev libfreeimage3 libfreeimage-dev

### Source code install

Fetch the source:

    git clone https://github.com/luismreis/node-openvg-canvas.git

Build the package:

    cd node-openvg-canvas
    npm install

To test:

    export PATH=$PWD/bin:$PATH
    examples/swissClock.js

### NPM / module install

Either run on your command line:

    npm install openvg-canvas

or add it to your package.json:

    [...]
      "dependencies": {
        "openvg-canvas" : "1.1.0"
      },
    [...]

## 1. Documentation

### Reference

* [WHATWG](http://www.whatwg.org/specs/web-apps/current-work/multipage/the-canvas-element.html)
* [w3c](http://www.w3.org/TR/2dcontext/)

### Canvas implementation status

The project now implements the full HTML 5 Canvas Level 1 specification, plus most of
the non interactive features of Level 2. The current focus on the project for releases in the short term is performance.

Items marked as "✘" are not planned for implementation. Some due to insufficient information (eg. focus ring), others because they don't make sense in this implementation (eg. scrollPathIntoView).

<table>
  <thead>
    <tr><th>Object / Feature</th><th>Status</th><th>Notes</th></tr>
  </thead>
  <tbody>
    <tr><td>CanvasRenderingContext2D</td><td></td><td></td></tr>
    <tr><td style="padding-left:2em;">state - save/restore</td><td>✔</td><td>Need optimization.</td></tr>
    <tr><td style="padding-left:2em;">matrix transformations: scale, transform, etc</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">compositing - alpha, composite operation</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">image smoothing</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">stroke/fill style</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:4em;">solid colors</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:4em;">gradients</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:4em;">patterns</td><td>✔</td><td>OpenVG doesn't support one-directional patterns. For now only 'no-repeat' and 'repeat' work.<br>No support for CanvasPattern's setTransform.</td></tr>
    <tr><td style="padding-left:2em;">shadows</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">clear/fill/stroke rect</td><td>✔</td><td>fill/stroke Rect commands are too slow.</td></tr>
    <tr><td style="padding-left:2em;">beginPath, paths / path methods, fill, stroke</td><td>✔</td><td>See Path methods below</td></tr>
    <tr><td style="padding-left:2em;">focus ring</td><td>✘</td><td></td></tr>
    <tr><td style="padding-left:2em;">scrollPathIntoView</td><td>✘</td><td></td></tr>
    <tr><td style="padding-left:2em;">clipping region</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">isPointInPath</td><td>to do</td><td>Really _hard stuff_™ here. Not supported by OpenVG.</td></tr>
    <tr><td style="padding-left:2em;">fill/stroke text</td><td>✔</td><td>Reasonably slow.</td></tr>
    <tr><td style="padding-left:2em;">measure text</td><td>✔</td><td>hanging and ideographic baselines not implemented.</td></tr>
    <tr><td style="padding-left:2em;">drawImage</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">hit regions</td><td>✘</td><td></td></tr>
    <tr><td style="padding-left:2em;">create/get/put image data</td><td>✔</td><td></td></tr>
    <tr><td>CanvasDrawingStyles</td><td></td><td></td></tr>
    <tr><td style="padding-left:2em;">line caps/joins - line width, cap, join, miter limit</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">dashed lines</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">text - font, textAlign, textBaseline</td><td>✔</td><td></td></tr>
    <tr><td>CanvasPathMethods</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">beginPath</td><td>✔</td><td>Also available on the Path object.</td></tr>
    <tr><td style="padding-left:2em;">moveTo, lineTo</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">quadraticCurveTo, bezierCurveTo</td><td>✔</td><td>Untested</td></tr>
    <tr><td style="padding-left:2em;">arcTo</td><td>✔</td><td>circular <strong>and</strong> elliptical</td></tr>
    <tr><td style="padding-left:2em;">rect</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">arc</td><td>✔</td><td>See issue #2.</td></tr>
    <tr><td style="padding-left:2em;">ellipse</td><td>✔</td><td>See Issue #2.</td></tr>
    <tr><td>CanvasGradient</td><td></td><td></td></tr>
    <tr><td style="padding-left:2em;">addColorStop</td><td>✔</td><td></td></tr>
    <tr><td>CanvasPattern</td><td></td><td></td></tr>
    <tr><td>TextMetrics</td><td>✔</td><td></td></tr>
    <tr><td>HitRegionOptions</td><td>✘</td><td></td></tr>
    <tr><td>ImageData</td><td>✔</td><td></td></tr>
    <tr><td>Path</td><td>✔</td><td>see CanvasPathMethods</td></tr>
    <tr><td style="padding-left:2em;">(constructor)</td><td>✔</td><td>SVG path constructor after v1.0</td></tr>
    <tr><td style="padding-left:2em;">addPath</td><td>✔</td><td></td></tr>
    <tr><td style="padding-left:2em;">addPathByStrokingPath</td><td>✘</td><td></td></tr>
    <tr><td style="padding-left:2em;">addText</td><td>✔</td><td>Position and Path variants</td></tr>
    <tr><td style="padding-left:2em;">addPathByStrokingText</td><td>✘</td><td></td></tr>
  </tbody>
</table>

### Differences from the HTML5 Canvas object / node-canvas

On browsers, the Canvas rendering is controlled by the browser runtime - this is referred in the w3c docs as the ' "update the rendering" step'.

On node-canvas, user code explicitly calls toBuffer or similar functions to produce output. Note that this behavior can be reproduced on node-openvg-canvas by calling ImageData.saveToBuffer and/or Canvas.toBuffer.

Code running on node-openvg-canvas must explicitly swap display buffers, to do so, either call ```Canvas.vgSwapBuffers()``` or use the included requestAnimationFrame shim (for more information look at the clock examples) that does this after calling your paint (callback) function.

## License

(The MIT License)

Copyright (c) 2012 Luis Reis

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
