# 2.2.1 Cycle 1: World Design

## Design

I will make a grid that when interacted with will change colour. This will be the foundation I then use to create the creature implementation, as the player will select a button and then place it in a grid. For this to happen the game needs to recognise individual cells as the map and be able to change the state of said cells.&#x20;

### Objectives

In this first cycle I decided to aim to make a colored background withgrad that would be interactable with and would show on the screen. I decided to use Replit for this due to the easy html, css and java script integration needed to make a web browser game. Replit is also an IDE, meaning I could see what my changes where doing to my game in real time.&#x20;

* [x] Color the background of the world
* [x] Create an interactable grid that can be clicked.&#x20;
* [x] Create multiple colours in the grid, that when clicked change.

### Usability Features

The game needs to be able to run **without crashing** and also allow for **player interactions** and **world interactions**.&#x20;

### Key Variables

| Variable Name       | Use                                              |
| ------------------- | ------------------------------------------------ |
| background color    | Colours the background of the web page, in css.  |
| cursor              | Changes the cursor                               |
| grid                | Creates a grid that seperates text or items      |
| grid template color | Changes the background of a grid box.            |

### Pseudocode

```
Const canvas 
Const colours 
Const drawing

canvas = width "x", height "y"
when colours = selected, change colour 

on leftmouseclick {
    fill cell x, y  with colours
    }

```

### Evidence

```javascript
const divs = document.querySelectorAll('.grid-item');
Array.from(divs).forEach(div => {
    div.addEventListener('click', classToggler);
});

const colors = ['lake', 'tree'];
let	enumerator = 0;

function classToggler() {
if (enumerator < colors.length+1){
  	enumerator+=1;
  }
 else {enumerator=0};


this.classList.add(colors[enumerator]);
this.classList.remove(colors[enumerator-1]);
const yellows = document.querySelectorAll('.beach')
const infoLabel = document.querySelector('#info')
infoLabel.innerHTML = "There are " + yellows.length + " yellow box(es)." 
};
```

This was my first attempt at what I would quickly find to be a deceptively easy task. I was trying to make a grid that had cells that could be interacted with. When clicked, the cell was meant to change colour.&#x20;

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

This was the attempt, I like the way the grid came out initially- The problem is when you try to interact with a cell, nothing happened. I then moved onto a different code, hoping to fix this issue. I attempted to make the code more concise, relying more on looping statements to minimise the code and keep it simple.&#x20;

```javascript
var gridContainer = document.getElementById("gridContainer");
var nRows = 5;
var nCols = 6;

for (let i = 0; i < nRows; i++) {
    var row = document.createElement("tr");
    for (let j = 0; j < nCols; j++) {
        let cell = document.createElement("td");
        cell.className = "grid-item";
        cell.addEventListener("click", function() {
            this.classList.toggle("activated");
        });
        row.appendChild(cell);
    }
    gridContainer.appendChild(row);
}
<table id= "gridContainer"></table>
```

This code got closer to the result that I wanted, but didn't change colour when clicked. Due to this, I tried changing the way I was approaching the problem. I thought about trying to use a canvas approach, making a pixel canvas that the player could colour with the entities. This method has become closer to being like a pixel art layout and will need adjustments but for the first time what I want to happen is happening; "button" like grid cells are appearing and nearly change colour when interacted with.&#x20;

### Final Code for an interactable grid element.

```html
<!DOCTYPE html>
<html>

<head>
    <!--
    This area gives the browser the information to load the code on a webpage. 
    -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale = 1.0">
    <title>replit</title>
    <link href="style.css" rel="stylesheet" type="text/css" />
</head>
<!--
    This area initiates the canvas, setting its dimensions and also creating a colour picker and a clear button.  
    -->
<body>
    <div>
        <canvas width="1920" height="850" id="canvas"></canvas>
    </div>
    <div>
        <label for="colorInput">Set Color: </label>
        <input type="color" id="colorInput">
    </div>
    <div>
        <button type="button" id="clearButton">Temp Clear</button>
    </div>
    <script src="script.js"></script>

    <!--
  This script places a badge on your repl's full-browser view back to your repl's cover
  page. Try various colors for the theme: dark, light, red, orange, yellow, lime, green,
  teal, blue, blurple, magenta, pink!
  -->
    <script src="https://replit.com/public/js/replit-badge-v2.js" theme="dark" position="bottom-right" size = "10%"></script>
</body>

</html>
```

This HTML mainly focuses on the creation of the view the webpage viewer experiences.&#x20;

```javascript
/**
 * @type HTMLCanvasElement
 */
//initiates the html elements in javascript
const canvas = document.getElementById("canvas");
const colorInput = document.getElementById("colorInput");
const toggleGuide = document.getElementById("toggleGuide");
const clearButton = document.getElementById("clearButton");
const drawingContext = canvas.getContext("2d");

//creates the cell dimensions in the canvas, which is dependant on the width on the size of the canvas width so in my case it is manually set to 1920
const CELL_SIDE_COUNT = 100;
const cellPixelLength = canvas.width / CELL_SIDE_COUNT;
const colorHistory = {};

// Set default color
colorInput.value = "#009578";

// Initialize the canvas background
drawingContext.fillStyle = "#006000";
drawingContext.fillRect(0, 0, canvas.width, canvas.height);

// This function manages the actuall creation of a coloured pixel upon the input of the user. 
function handleCanvasMousedown(e) {
    // Ensure user is using their primary mouse button
    if (e.button !== 0) {
        return;
    }

    const canvasBoundingRect = canvas.getBoundingClientRect();
    const x = e.clientX - canvasBoundingRect.left;
    const y = e.clientY - canvasBoundingRect.top;
    const cellX = Math.floor(x / cellPixelLength);
    const cellY = Math.floor(y / cellPixelLength);
    const currentColor = colorHistory[`${cellX}_${cellY}`];

    if (e.ctrlKey) {
        if (currentColor) {
            colorInput.value = currentColor;
        }
    } else {
        fillCell(cellX, cellY);
    }
}

// This function manages the clear button, ensuring that it is only pressed on purpose with a check system and if the user confirms they do want to clear, I have set it to clear the screen back to the original colour. This button will be here in the developmental phase so I can watch what my inputs change and do, but in the end game it should be in an "options" menu, to restart the game. 
function handleClearButtonClick() {
    const yes = confirm("Are you sure you wish to clear the canvas?");

    if (!yes) return;

    drawingContext.fillStyle = "#006000";
    drawingContext.fillRect(0, 0, canvas.width, canvas.height);
}

// This is the code that manages the coloured cell creation phase, taking the width and height of the cell and then filling the resultant area. 
function fillCell(cellX, cellY) {
    const startX = cellX * cellPixelLength;
    const startY = cellY * cellPixelLength;

    drawingContext.fillStyle = colorInput.value;
    drawingContext.fillRect(startX, startY, cellPixelLength, cellPixelLength);
    colorHistory[`${cellX}_${cellY}`] = colorInput.value;
}
//This just handles the interactions from the viewer. 
canvas.addEventListener("mousedown", handleCanvasMousedown);
clearButton.addEventListener("click", handleClearButtonClick);

```

And this JavaScript primarily handles the cell feature of the interactable area, ensuring that the colour entered is the one wanted by the player. This will change over time, but offers me a stable basepoint that completes my original goals.&#x20;

* Color the background of the world
* Create an interactable grid that can be clicked.&#x20;
* Create multiple colours in the grid, that when clicked change.

The background is coloured a green to show the grass that will be the basis of the game. There is an interactable grid that recognises when the player clicks an area and gives a response. The last point changed to a colour picker but the idea remained the same- the player chooses a colour and clicks on the grid, where clicked that colour appears.&#x20;

### Challenges

The main challenge was the ammount of time I spent trying to create a grid that when interacted with caused a change. I had roughly four iterations of code, all trying to figure this one problem out. I went over it with friends and family and still never found what was preventing it from working, so I instead had to move on to a new idea, which was something I had never worked on or in before - HTML canvas.

### Testing&#x20;

Evidence of my testing of the code.&#x20;

<table><thead><tr><th width="149">Test</th><th width="238">Expected outcome</th><th width="278">Actual outcome</th><th>Result</th></tr></thead><tbody><tr><td>1- Interactable grid</td><td>A grid appears that when clicked randomly chooses a colour from a given list</td><td>A grid appeared but when interacted with nothing happened- The grid just remained as it started</td><td>Fail</td></tr><tr><td>2- Interactable grid code 2</td><td>A revised grid appears that when clicked turns from black squares to red squares. </td><td>A grid appears nicely lain out in black, but when interacted with, no colour change occurred.</td><td>Fail</td></tr><tr><td>3- Pixel canvas</td><td>A flat plane appears that when interacted with turns individual cells a different colour</td><td>One colour appears with no seams, when interacted with a cell changes to the selected colour. </td><td>Pass</td></tr></tbody></table>
