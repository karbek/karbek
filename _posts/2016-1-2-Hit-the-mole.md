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
