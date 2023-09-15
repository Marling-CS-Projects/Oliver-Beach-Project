# 2.2.2 Cycle 2: Implementation System

## Design

This cycle focuses primarily on the way that the player will take something from a bar along the base of the screen, before then implementing said thing into one of the cells. I hope to have these things be such as a squirrel, a bush and a tree with possible water.&#x20;

### Objectives

The objective of creating water is a definite goal, but this cycle it may be difficult as I am still learning JavaScript and this requires making a cell "locked" to placement of other things- so that a squirrel can only travel on grass and trees, for example.&#x20;

* [x] Create a taskbar that displays the things that can be placed down.&#x20;
* [x] Allow the player to "grab" one of these things off the taskbar and place it into the world.&#x20;
* [ ] ~~Create a system that only allows things to be placed in certain areas.~~&#x20;

### Usability Features

It needs to be a **simple feature** that allows the player to **see what is unlocked and what they can implement**.&#x20;

The player needs to be able to **fluidly implement** the things from the taskbar, including the **implementation of rocks and plants**.&#x20;

### Key Variables

| Variable Name | Use                                                                                                              |
| ------------- | ---------------------------------------------------------------------------------------------------------------- |
| .velocity     | Decides the speed that the entities are chasing each other.                                                      |
| background    | Sets the area to use as the active site.                                                                         |
| map           | Sets the area that entities can travel in, as a representation of pixels. Allows x and y coordinate selection.   |
| fill          | Fills part of the canvas with the shape of the selected thing- could be a square or a single pixel for example.  |

### Pseudocode

```
Const taskbar
Const animal
Const plant

display button position bottom, allignment centre
on button interaction, change colour selection to animals representative colour. 

animal = brown
plant = green
```

## Development

<figure><img src="../.gitbook/assets/image (2) (3).png" alt=""><figcaption></figcaption></figure>

This was the first major issue I found- my code wasn't responding to being clicked as a button. As it turned out, the issue was that I had set everything as a class and not as an Id, meaning it didn't respond as I expected. When fixed, this code allowed me to select a color representative of the name of the button, ie a grey pixel for a rock. This allowed a user to "paint" an environment, however this style of coding didn't allow for any reactions in the game- no animals could interact as they were just pixels.

```javascript
var x,y;
var t = 0;
var T = 1000;
let button;
let button2;

function setup() {
    createCanvas(1920, 1080);
    background(255);
    frameRate (30);
    button = createButton ("Stop");
    button.position (0, 0);
    button.mousePressed (noLoop);
    button2 = createButton ("Resume");
    button2.position (60, 0);
    button2.mousePressed (loop);
}

function draw() { 
  fill(0,10000);
  noStroke(1000);
  ellipse(x,y,5,5);
  // x=100;y=200;
  x = noise(t);
  x = map(x,0,1,0,width);
  y = noise(T);
  y = map(y,0,1,0,height);
  t =t+0.005;
  T =T+0.005;
  console.log(x,y);
}// Some code
```

This code was my next progression point. I studied "Perlin noise", a technique to make a random point more even. This allowed me to make a random moving pixel that stuck to a logic and seemed less eratic. I used this to make the code above, which when ran creates an ellipsis that leaves a "trail" and moves in a random fashion around the screen. I wanted to use this to create a system where it would show animals as little pixels and they would move around the screen randomly.&#x20;



These next two code sets work together in p5 JavaScript to make a hunter prey system. This code came from ([https://editor.p5js.org/simontiger/sketches/B1GtGlDGb](https://editor.p5js.org/simontiger/sketches/B1GtGlDGb)) and I used it as an ideal example of a hunter / prey system. The game is simple yet effective and I really like the smooth way in which the characters turn to face either the other character or the nearest "escape".

```javascript
// script.js;
let v2;
let velocity;
let slider;
let debug;

function setup() {
    createCanvas(640, 360);
    velocity = p5.Vector.random2D();
    velocity.mult(random(1, 3));
    v1 = new Vehicle();
    v2 = new Vehicle();
    slider = createSlider(25, 150, 100);
    debug = createCheckbox();
}

function draw() {
    background(255);

    var seek = v1.seek(v2.location);
    v1.applyForce(seek);

    var neighborDist = slider.value();
    if (v2.location.dist(v1.location) < neighborDist) {
        var flee = v2.seek(v1.location);
        flee.mult(-5);
        v2.applyForce(flee);
    }

    v1.update();
    v1.edges();
    v1.display();

    v2.update();
    v2.edges();
    v2.display();

    if (debug.checked()) {
        stroke(0);
        noFill();
        ellipse(v2.location.x, v2.location.y, neighborDist * 2, neighborDist * 2);
    }

    fill(0);
    noStroke();
    textSize(32);
    text(neighborDist, 15, height - 25);
}
```

```javascript
// Vehicle.js
class Vehicle {
    constructor() {
        this.location = createVector(random(width), random(height));
        this.velocity = p5.Vector.add(velocity, p5.Vector.random2D());
        this.acceleration = createVector();
        this.r = 6;
        this.maxspeed = 4;
        this.maxforce = 0.1;
    }
}

Vehicle.prototype.update = function() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxspeed);
    this.location.add(this.velocity);
    this.acceleration.mult(0);
}

Vehicle.prototype.applyForce = function(force) {
    this.acceleration.add(force);
}

Vehicle.prototype.seek = function(target) {

    var desired = p5.Vector.sub(target, this.location);

    desired.normalize();
    desired.mult(this.maxspeed);

    var steer = p5.Vector.sub(desired, this.velocity);

    steer.limit(this.maxforce);

    return steer;
}

Vehicle.prototype.display = function() {

    var theta = this.velocity.heading() + radians(90);
    fill(127);
    stroke(0);
    strokeWeight(1);
    push();
    translate(this.location.x, this.location.y);
    rotate(theta);
    beginShape();
    vertex(0, -this.r * 2);
    vertex(-this.r, this.r * 2);
    vertex(this.r, this.r * 2);
    endShape(CLOSE);
    pop();
}

Vehicle.prototype.edges = function() {
    if (this.location.x > width) this.location.x = 0;
    if (this.location.x < 0) this.location.x = width;
    if (this.location.y > height) this.location.y = 0;
    if (this.location.y < 0) this.location.y = height;

}
```

This code was a tutorial from the p5 website. This code creates two "Vehicles", with a predator prey function. One character is chasing the other, which appears and disappears on the corners of the screen. It initiates flee settings when the predator is within a set distance in a sphere around itself. I wanted to use this code to create two dots that would move randomly until they came into close contact, at which point there was a chase setting.&#x20;

### Outcome

My three different sets of code have very different outcomes. The first set of code is the most user  interactivity game. It allows the player to build a setting for the background.&#x20;

The next set of code was a study into Perlin noise and my first time using p5. It runs well, allowing a dot to move fluidly around a screen whilst simultaneusly moving completely randomly. This inspired me to continue my original idea of interactions between entities, as I could have two dots that would move randomly and be seperate individuals.&#x20;

This is where the third piece of code comes into importance, the two dots will be able to move closer together and when they randomly move close enough they will chase and flee until they may be driven off the side of the map.&#x20;

### Challenges

Description of challenges

p5 Web editor. This platform was blocked on my school wifi, meaning I had to create code and only test it on a school laptop. This meant that I had difficulty checking errors as I was coding.&#x20;

When following a tutorial, I was being presented with ideas I hadn't dealt with before as an amateur at both JavaScript and now p5JS. This meant that I was learning about things like velocity, map placements and p5's system of a "setup" which initiates everything, creating buttons, characters ie.&#x20;

After this I tried to merge the codes together and faced great difficulty keeping the random movement system whilst simultaneously maintaining the hunting system. I am still in the process of modifying this system and making it merge succesfully.&#x20;

## Testing

Evidence for testing

### Tests

<table><thead><tr><th width="97">Test</th><th width="230">Instructions</th><th width="216">What I expect</th><th width="111">What actually happens</th><th>Pass/Fail</th></tr></thead><tbody><tr><td>1</td><td>Graph System ran, tested pressing buttons to change input colour. </td><td>The graph should have buttons that can be clicked. The player should be able to then click a pixel and the cell changes to the buttons respective colour.</td><td>As expected</td><td>Pass</td></tr><tr><td>2</td><td>Ran the Perlin Noise system, pressed pause and resume. </td><td>The Perlin Noise dot should appear somewhere in the screen, should move around and it should place an image of itself under where it is going. When pause is pressed it should freeze, resume should unpause it.</td><td>As expected</td><td>Pass</td></tr><tr><td>3</td><td>Ran the Hunter Prey system. Changed the slider to adjust the range at which they are chased at. </td><td>Expected the vehicles to seek each other out, with the slider deciding the required proximity to cause a flee system. </td><td>Nothing appeared in my web page. </td><td>Fail</td></tr><tr><td>4</td><td>Ran the Hunter Prey system. Changed the code to include the vehicle file, which contains the features of the vehicle classes. </td><td>Expected the vehicles to seek each other out, with the slider deciding the required proximity to cause a flee system. </td><td>As expected, there was a chase and the slider worked.</td><td>Pass</td></tr></tbody></table>

### Evidence

<figure><img src="../.gitbook/assets/image (1) (2).png" alt=""><figcaption><p>The graph drawing system with a representative little background drawn</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (2).png" alt=""><figcaption><p>The perlin noise systems random movement system. </p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption><p>A prey (with representative range circle) and the hunter turning into its circle to seek it out. Not my code, credit to <a href="https://editor.p5js.org/simontiger/sketches/B1GtGlDGb">https://editor.p5js.org/simontiger/sketches/B1GtGlDGb</a></p></figcaption></figure>
