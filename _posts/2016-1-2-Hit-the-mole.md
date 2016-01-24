---
layout: post
title: Create a simple HTML5 game - Hit the mole
---

To dive into HTML5 game development, we create a simple "hit the mole" game. The game 5 molehills and a mole appearing each time from behind one molehill. If you can hit him, you get some points.


The game ist playable in the web browser and on mobile devices, as well. The complete source code can be found on github: [https://github.com/physalisio/HitTheMole.git] https://github.com/physalisio/HitTheMole.git

Our first step is to create the directory structure we need:

www
>css
>images
>js
>index.html

Download the images and the css file and store them in the "images" and the "css" directory.

The index.html file containing the game is stored in the www directory.


```html
<!DOCTYPE html>
<html>
    <head>
        <title>Hit the mole</title>
        <meta charset="UTF-8">
        <link rel="resource" type="application/l10n" href="locales/locales.ini" />
        <meta id="Viewport" name="viewport" content="width=device-width,initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
        <link href="css/hitTheMole.css" rel="stylesheet"/>
    </head>
    <body>
        <img id="background" src="images/background.png" alt="background"/>
        <div id="hole1" class="hole"></div>
        <div id="holeBackground1" class="holeBackground"/></div>
        <div id="hole2" class="hole"/></div>
        <div id="holeBackground2" class="holeBackground"/></div>
        <div id="hole3" class="hole"/></div>
        <div id="holeBackground3" class="holeBackground"/></div>
        <div id="hole4" class="hole"/></div>
        <div id="holeBackground4" class="holeBackground"/></div>
        <div id="hole5" class="hole"/></div>
        <div id="holeBackground5" class="holeBackground"/></div>
        <div id="mole"></div>
        <div id="moleSprite"></div>
        <div id="stars"></div>
        <span class="points"><span data-l10n-id="points">Points: </span><span id="points">0</span></span>
        <script src="js/underscore-min.js"></script>
        <script src="js/hitTheMole.js"></script>
    </body>
</html>

```

Each molehill consists of a black background sprite and the colored molehill sprite visible in the foreground. As we will let the mole appear from inside the molehill, we work with different z-indices.

The javascript file "hitTheMole.js" contains the game logik; without it, you would just see the background image and the other sprites in the upper left corner.

In the first step, we will determine the game size. We introduce the following variables:

```javascript
var aspectRatio;
var enlargement = 1;
var borderWidth = 0;
```

As the game shall start in fullscreen mode, we have to detect the aspect ratio, the enlargement and the width of the border. Our background image is very large to match very large screens. For screens with a smaller aspect ratio, we center the background image and do not show the left and right border.

The following method calculates the enlargement and the border width:

```javascript
function setBackgroundSize() {
    var longestAspectRatio = 1136 / 640;
    var documentWidth = window.innerWidth;
    var documentHeight = window.innerHeight;
    aspectRatio = documentWidth / documentHeight;
    var background = document.getElementById("background");

    if (aspectRatio >= longestAspectRatio) {
        background.style["height"] = documentHeight + "px";
        document.body.style["height"] = documentHeight + "px";
        document.body.style["width"] = documentWidth + "px";
    } else {
        var backgroundAspectRatio = longestAspectRatio;
        var backgroundWidth = backgroundAspectRatio * documentHeight;
        background.style["height"] = documentHeight + "px";
        document.body.style["height"] = documentHeight + "px";
        background.style["width"] = backgroundWidth + "px";
        document.body.style["width"] = backgroundWidth + "px";
        borderWidth = (backgroundWidth - documentWidth) / 2;
        background.style["left"] = "-" + borderWidth + "px";
    }

    enlargement = background.offsetHeight / 640;
    movementHeight = movementHeight * enlargement;
}
```

We use css to set the background and border size.

When the Game page is loaded, we must execute "setBackgroundSize". This is done by introducing the method "onDeviceReady" initializing all game parameters. So far, "onDeviceReady" only calls "setBackgroundSize". Other method calls will follow later:

```javascript
function onDeviceReady() {
    setBackgroundSize();
}

```

"onDeviceReady" is called as soon as the game web page is loaded:

```javascript
window.onload = onDeviceReady;
```

Here is a synopsis of our actual javascript code in "hitTheMole.js":

```javascript
window.onload = onDeviceReady;

var aspectRatio;
var enlargement = 1;
var borderWidth = 0;

function onDeviceReady() {
    setBackgroundSize();
}

function setBackgroundSize() {
    ...
}
```

Our next step is to init the mole and the stars appearing when the user hits it. We encapsulate this in the method 'initMole':

```javascript
function initMole() {
    var mole = document.getElementById("mole");
    mole.style["height"] = mole.offsetHeight * enlargement + "px";
    mole.style["width"] = mole.offsetWidth * enlargement + "px";
    
    var stars = document.getElementById("stars");
    stars.style["height"] = stars.offsetHeight * enlargement + "px";
    stars.style["width"] = stars.offsetWidth * enlargement + "px";
}
```

The only thing we have to do is to scale the mole and the star div.

Our next step is to place the molehills. For that purpose, we introduce a global array containing all possible molehill positions. The variable declaration is done at the beginning.

```javascript
var holePositions = [
    {x: 242, y: 184},
    {x: 638, y: 184},
    {x: 54, y: 482},
    {x: 436, y: 482},
    {x: 816, y: 482}];
```

All the molehills have to be scaled and correctly placed on the screen. In the 'index.html' you can see that each molehill sprite has an individual identifier 'hole1', 'holeBackground1', 'hole2', 'holeBackground2', ...  
As aforementioned, each molehill is composed of a black background sprite and the colored sprite with a higher z-index. This method performs the sizing operations for one molehill:

```javascript
function initHole(holeId, holeBackgroundId, holePosition) {
    var hole = document.getElementById(holeId);
    var height = hole.offsetHeight * enlargement + "px";
    var width = hole.offsetWidth * enlargement + "px";
    var positionX = holePosition["x"] * enlargement - borderWidth;
    var positionY = holePosition["y"] * enlargement;
    hole.style["height"] = height;
    hole.style["width"] = width;
    hole.style["left"] = positionX + "px";
    hole.style["top"] = positionY + "px";

    var holeBackground = document.getElementById(holeBackgroundId);
    holeBackground.style["height"] = height;
    holeBackground.style["width"] = width;
    holeBackground.style["left"] = positionX + "px";
    holeBackground.style["top"] = positionY + "px";
}
```

The method 'initHole' is called for all 5 molehills:

```javascript
function initHoles() {
    initHole("hole1", "holeBackground1", holePositions[0]);
    initHole("hole2", "holeBackground2", holePositions[1]);
    initHole("hole3", "holeBackground3", holePositions[2]);
    initHole("hole4", "holeBackground4", holePositions[3]);
    initHole("hole5", "holeBackground5", holePositions[4]);
}
```

To make the mole react when the user clicks on it, we create the method 'initMoleClickHandler'. The folling steps are executed by the onClick handler of the mole div:
1. When the user clicks on the div, the handler is eliminated.
2. 
This method creates an onClick handler for the mole div. When the user clicks on the div, the handler is eliminated
