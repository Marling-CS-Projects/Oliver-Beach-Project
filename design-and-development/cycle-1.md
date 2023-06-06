# 2.2.1 Cycle 1

## Design

### Objectives

In this first cycle I decided to aim to make a colored background withgrad that would be interactable with and would show on the screen. I decided to use Replit for this due to the easy html, css and java script integration needed to make a web browser game. Replit is also an IDE, meaning I could see what my changes where doing to my game in real time.&#x20;

* [x] Color the background of the world
* [ ] Create an interactable grid that can be clicked.&#x20;
* [ ] Create multiple colours in the grid, that when clicked change.

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

This code got closer to the result that I wanted, but didn't change colour when clicked. Due to this, I tried changing the way I was approaching the problem. I thought about trying to use a canvas approach, making a pixel canvas that the player could colour with the entities.&#x20;
