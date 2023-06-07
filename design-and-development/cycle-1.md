# 2.2.1 Cycle 1

## Design

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
procedure do_something
    
end procedure
```

## Development

### Outcome

### Challenges

Description of challenges

## Testing

Evidence for testing

### Tests

| Test | Instructions  | What I expect     | What actually happens | Pass/Fail |
| ---- | ------------- | ----------------- | --------------------- | --------- |
| 1    | Run code      | Thing happens     | As expected           | Pass      |
| 2    | Press buttons | Something happens | As expected           | Pass      |

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

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

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

This code got closer to the result that I wanted, but didn't change colour when clicked. Due to this, I tried changing the way I was approaching the problem. I thought about trying to use a canvas approach, making a pixel canvas that the player could colour with the entities. This method has become closer to being like a pixel art layout and will need adjustments but for the first time what I want to happen is happening.&#x20;

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
```
