# 2.2.3 Cycle 3: Model and Design

## Design

This cycle is focussed on attempting to merge the random movement system of the Perlin dot with a rotational system, to give the simulation / game a better flow. This would create the feeling of an animal roaming roaming around, facing a certain point.&#x20;

Another thing I would like to atleast attempt is trying to put one of the systems into the drawable background system, however I doubt this will work due to the difference between a **canvas** and a **map**. &#x20;

### Objectives

These are subject to change due to the difficulty of this cycle and the way in which I am unsure of whether the different codes will work together.&#x20;

* [x] Give the character a model
* [x] Have the character rotate around.
* [x] Try and set the background to a colour in my code.&#x20;

### Usability Features

The game needs to be able to run smoothly to allow the user to watch what is happening at a **smooth fps**.&#x20;

The game may not have much interactability at this stage but I would like for the player to be able to have an input, whether it be in regards to setting up the background or controlling the seek size, or the state of the game- paused or unpaused ie.&#x20;

### Key Variables

| Variable Name | Use                                                                                                                                                                                                                   |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| text ()       | Allows printing of both 'text' and also variables. Used to show the x value of the ball (constantly updating) and also to show the player what the slider does.                                                       |
| background () | Initially this is used to create the area that can be interacted with. However I learnt that if updated simultaneously with the ball's movement, the background itself can be updated and used as an opacity filter.  |
| textsize ()   | Adjusts the size of text displayed. Helps create a more uniform, sleek and professional look.                                                                                                                         |
| height ()     | Grabs the users inner window length of the screen. Means that the game adjusts for different window sizes on loading,                                                                                                 |

### Pseudocode

```
movement ()
    move vehicle noise (0, 8)
fade ()
    background (0, 100)
    
seek ()
    slider = 0, 50, 100
    if vehicle1 in vehicle2 ellipsis radius slider
        vehicle1 seek vehicle2


```

## Development

Initial development was focussed on the fade out system. I used my perlin noise system as the base, however I realised that in the original there was a trail left after the dot.&#x20;

<figure><img src="../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

As one can see, this appears fine initially. The trail can almost be a benefit, showing the way in which the dot is travelling and the random velocity it is travelling with, thanks to the spacing between the dots.&#x20;

<figure><img src="../.gitbook/assets/Image for project of mess.png" alt=""><figcaption><p>The black sphere shows where the currently moving dot is located. </p></figcaption></figure>

However, I hope one can see why this needed to change. It rapidly becomes an unclear mess of trails, where you can barely see the actual moving dot and instead you have to focus on the trail. To fix this issue, I decided the trail needed to fade out after it had been made. I spent a long time trying to make a system that remembered the position of the dot from, say, five positions previously. After spending the time trying to figure this out, I came up with a new solution thanks to inspiration from an art game on the p5 website. All I needed to do was set the background to a dark colour, then set the dot to a bright colour and create a second background that was an incredibly dark colour. The second background acts as a mask, covering the original. This means that when the trail is created, it dissapears behind the mask and leaves only the dot itself and a very short trail behind it. This allows for the appearance of a single point that is moving randomly arround the background.

```javascript
let t;
function setup() {
    createCanvas(innerWidth -10, innerHeight -10);
    background(0);
    t = 0;
}

function draw() {
    // fade the background by giving it a low opacity
    background(0, 100); // higher second = darker trail

    var x = width * noise(t);
    var y = height * noise(t + 5);

    noStroke();
    fill(10000, 1000);
    ellipse(x, y, 10, 10);

    t = t + 0.003;
}
```

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption><p>Note the lack of a visible trail, instead the singular point that appears to move on its own- closer to a wild animal moving around an environment semi-randomly. </p></figcaption></figure>

I then changed this code by adjusting the background colour from the dark black that it currently was to a nicer green colour, that would better represent the green field I wanted the game to occur in.&#x20;



```javascript
let t;
let t2;
let velo = 0.003;
let slider;

function setup() {
    createCanvas(innerWidth - 10, innerHeight - 10);
    background('lightblue');
    t = 0;
    t2 = 0;
    button = createButton("Pause");
    button.position(10, 10);
    button.mousePressed(noLoop);
    button2 = createButton("Resume");
    button2.position(70, 10);
    button2.mousePressed(loop);
    slider = createSlider(1, 50, 10);
}

function draw() {
    // fade the background by giving it a low opacity
    background('lightblue'); // higher second = darker trail
    let velo = slider.value() / 1000;
    let x = width * noise(t);
    let y = height * noise(t + 5);
    let x2 = width * noise(t2);
    let y2 = height * noise(t2 + 5);
    
    function fishLeft() {
        triangle(x + 35, y + 20, x + 25, y, x + 35, y - 20)
        ellipse(x, y, 50, 15)
        ellipse(x - 15, y, 5, 5)
    }
    function fishRight() {
        triangle(x2 - 35, y2 + 20, x2 - 25, y2, x2 +-35, y2 - 20)
        ellipse(x2, y2, 50, 15)
        ellipse(x2 + 15, y2, 5, 5)  
    }
    //noStroke()
    if (x >= innerWidth /2){
        for (let i = 0; i < 5; i++) {
            fishLeft()
        }
    }
    if (x < innerWidth /2){
        for (let i = 0; i < 5; i++) {
            fishRight()
        }
    }
    t = t + velo;
    t2 = t2 + velo;
    //let velo = slider.value();   Need to figure this out first, make slider control movement
    textSize(20);
    text(x2, 5, height - 50);
    text(velo, 5, height - 30);
    text('Movement speed', 5, height - 10);
}
```

This code was a revised version, where the character was a fish and swapped depending on the side of the screen it was on.

### Outcome

<figure><img src="../.gitbook/assets/image (12).png" alt=""><figcaption><p>This is the fish swimming around, with a focus on the opposite side of the map than its current position. </p></figcaption></figure>

### Challenges

Making the background opacity system work well- I was often left with a "phantom" trail, a very light grey / black shadow of where the ball had been. This wasn't bad initially but as the code ran for a longer time, the shadows built up until you once again lost the ball in an unclear mess. To fix this I adjusted the opacity levels and the way it was being applied to the screen, moving it forwards a layer so it coated the back more completely.&#x20;

Another thing I struggled with, was the velocity slider. I wanted the player to be able to adjust the speed the simulation was running at, using a slider. However I struggled immensley getting the numerical value of the slider to work with the speed system. I fixed this eventually by dividing the slider value by 1000. Something I hadn't considered was the way in which the speed system was working on a value around 0.002 being the current speed. Anything above 0.1 was almost too fast too see. With my slider I was trying to set the speed to at minimum 1, up to 100 initially. Eventually I found that by dividing this by 1000 I got a value that could be shown on screen, fixing the issue.&#x20;

## Testing

Evidence for testing

### Tests

<table><thead><tr><th>Test</th><th>Instructions</th><th width="170">What I expect</th><th>What actually happens</th><th>Pass/Fail</th></tr></thead><tbody><tr><td>1</td><td>Run code</td><td>Dot spawns in a random area, stays in the screen and moves around. </td><td>As expected</td><td>Pass</td></tr><tr><td>2</td><td>Slider moved</td><td>Ball should speed up / slow down.</td><td>Slider addition stops code from running.</td><td>Fail</td></tr><tr><td>3</td><td>Slider moved 2</td><td>Ball should speed up / slow down.</td><td>Ball moves faster / slower. </td><td>Pass</td></tr><tr><td>4</td><td>Text displays correct information. </td><td>Text should show the movement speed value, the position of the ball and a description of the slider. </td><td>No text showed up</td><td>Fail</td></tr><tr><td>5</td><td>Text displays correct information. </td><td>Text should show the movement speed value, the position of the ball and a description of the slider. </td><td>As expected</td><td>Pass</td></tr></tbody></table>

### Evidence

![](<../.gitbook/assets/image (3) (1) (1).png>)![](<../.gitbook/assets/image (4) (1) (1) (1).png>)

The first image is the issue I had with the text, where a variable wsa being reassigned when it wasnt supposed to be, meaning it never showed up. On the right is how it is supposed to look, with a reading for the x position, the velocity of the character and a description of what the slider does.&#x20;
