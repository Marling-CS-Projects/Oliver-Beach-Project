# 2.2.4 Cycle 4: A goal / target

## Design

In this cycle I plan to make a central objective to the player. Some initial ideas I have come up with:&#x20;

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

This image shows the three game ideas I have right now: A ball of food that if the fish passes close too it eats, that could drop from the top and then rest in the middle of the screen.&#x20;

The second game would introduce a system where the food ball was controlled by the player, and the player had too evade a hunter, who was trying to eat the food.&#x20;

The third game would introduce a second fish, that chases the first off the screen. This would simulate a hunting behaviour, however would most likely require a new system of movement to be introduced.&#x20;

### Objectives

* [x] Decide on which style / feature of game I want to go for.&#x20;
* [x] Begin looking into the ways I am going to make that system fit into the game.&#x20;

### Usability Features

The game should look smooth, load quickly and not be complicated; I would like anyone to be able to look at the game and know what is happening, whilst still keeping it interesting.&#x20;

### Key Variables

| Variable Name | Use                                                                                                                                                                                                                                                                                                                                                                          |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| hide ()       | Stops whatever is being hidden from being displayed. Good for my game over screen, hides the buttons and text.                                                                                                                                                                                                                                                               |
| show ()       | Removes the hide effect, neccessary for a game over new game system where things are being hidden then need to be re displayed.                                                                                                                                                                                                                                              |
| width /2      | This system allows me to place objects at a certain point no matter the pixels available, for instance I had the width set to my monitor specifications (1920 x 1080) however when loaded in a smaller window the game just had a scroll bar. By introducing innerheight and innerwidth amongst other things I was able to keep the game regular no matter the screen size.  |

### Pseudocode

```
procedure do_something
    
end procedure
```

## Development

```javascript
let t;
let t2;
let slider;

const player = {
    moveX: 50,
    moveY: 50,
    speed: 5,
    up() {
        this.moveY -= this.speed;
    },
    down() {
        this.moveY += this.speed;
    },
    left() {
        this.moveX -= this.speed;
    },
    right() {
        this.moveX += this.speed;
    },
};

let hunter; // Declare the hunter object

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

    // Initialize the hunter object inside setup()
    hunter = {
        moveX: width / 2, // Start at the center
        moveY: height / 2, // Start at the center
        speed: 2, // Adjust as needed
    };
}



function hunterChase() {
    let dx = player.moveX - hunter.moveX;
    let dy = player.moveY - hunter.moveY;
    let distance = sqrt(dx * dx + dy * dy);

    if (distance > 5) {
        hunter.moveX += (dx / distance) * hunter.speed;
        hunter.moveY += (dy / distance) * hunter.speed;
    }
}


function draw() {
    background('lightblue');

    let velo = slider.value() / 1000;
    let x = width * noise(t);
    let y = height * noise(t + 5);

    fill('orange');
    ellipse(x, y, 20, 20);

    if (keyIsDown(UP_ARROW) || keyIsDown(87)) {
        player.up();
    }
    if (keyIsDown(LEFT_ARROW) || keyIsDown(65)) {
        player.left();
    }
    if (keyIsDown(RIGHT_ARROW) || keyIsDown(68)) {
        player.right();
    }
    if (keyIsDown(DOWN_ARROW) || keyIsDown(83)) {
        player.down();
    }

    if (player.moveX >= innerWidth / 2) {
        for (let i = 0; i < 5; i++) {
            fishLeft();
        }
    }
    if (player.moveX < innerWidth / 2) {
        for (let i = 0; i < 5; i++) {
            fishRight();
        }
    }

    // Update the hunter's position
    hunterChase();

    // Draw the hunter
    fill('black');
    ellipse(hunter.moveX, hunter.moveY, 40, 30);

    // Rest of your code...

    t = t + velo;
    t2 = t2 + velo;
    textSize(20);
    text(velo, 5, height - 30);
    text('Movement speed', 5, height - 10);
}



function fishLeft() {
    fill('white')
    triangle(player.moveX + 35, player.moveY + 20, player.moveX + 25, player.moveY, player.moveX + 35, player.moveY - 20)
    ellipse(player.moveX, player.moveY, 50, 15)
    fill('black')
    ellipse(player.moveX - 15, player.moveY, 5, 5)
}
function fishRight() {
    fill('white')
    triangle(player.moveX - 35, player.moveY + 20, player.moveX - 25, player.moveY, player.moveX - 35, player.moveY - 20)
    ellipse(player.moveX, player.moveY, 50, 15)
    fill('black')
    ellipse(player.moveX + 15, player.moveY, 5, 5)
}
```

This code was what I had come to have. It features a new hunter, that chases the players current position and stops on top of the player. It obeys the pause and resume commands meaning the player now has a visible thing that is following them. I now need to create collision features for both the hunter and the orb, which I am thinking of removing. I believe the game has taken a different direction since I started it and I believe that the orb, if not easily useable, is easily replaceable. The main goal right now is to get the hunter to feel like a threat, so the player has some form of danger to worry about whilst they play as the fish.&#x20;

```javascript
let t;
let t2;
let slider;
let counter = 0; // Counter for collisions with orange orb

const player = {
    moveX: 50,
    moveY: 50,
    speed: 5,
    up() {
        this.moveY -= this.speed;
    },
    down() {
        this.moveY += this.speed;
    },
    left() {
        this.moveX -= this.speed;
    },
    right() {
        this.moveX += this.speed;
    },
};


let fishFacingRight = true;
let hunter; // Declare the hunter object


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

    // Initialize the hunter object inside setup()
    hunter = {
        moveX: width / 2, // Start at the center
        moveY: height / 2, // Start at the center
        speed: 2, // Adjust as needed
    };
}



function hunterChase() {
    let dx = player.moveX - hunter.moveX;
    let dy = player.moveY - hunter.moveY;
    let distance = sqrt(dx * dx + dy * dy);

    if (distance > 5) {
        hunter.moveX += (dx / distance) * hunter.speed;
        hunter.moveY += (dy / distance) * hunter.speed;
    }
}


function draw() {
    background('lightblue');

    let velo = slider.value() / 1000;
    let x = width * noise(t);
    let y = height * noise(t + 5);

    fill('orange');
    ellipse(x, y, 20, 20);

    
    if (keyIsDown(UP_ARROW) || keyIsDown(87)) {
        player.up();
    }
    if (keyIsDown(LEFT_ARROW) || keyIsDown(65)) {
        player.left();
        fishFacingRight = false; // Fish is facing left
    }
    if (keyIsDown(RIGHT_ARROW) || keyIsDown(68)) {
        player.right();
        fishFacingRight = true; // Fish is facing right
    }
    if (keyIsDown(DOWN_ARROW) || keyIsDown(83)) {
        player.down();
    }
    
    // Check for collision between hunter and fish
    let distanceToHunter = dist(player.moveX, player.moveY, hunter.moveX, hunter.moveY);
    if (distanceToHunter < 20) {
        // Collision occurred, remove fish
        fishFacingRight = true;
        gameOver()
    }
    
    if (fishFacingRight) {
            fishRight();
        } else {
            fishLeft();
        }
    // Update the hunter's position
    hunterChase();

    // Draw the hunter
    fill('black');
    ellipse(hunter.moveX, hunter.moveY, 30, 30);

    // Rest of your code...

    t = t + velo;
    t2 = t2 + velo;
    textSize(20);
    text(velo, 5, height - 30);
    text('Movement speed', 5, height - 10);
}



function fishLeft() {
    fill('white')
    // Change the triangle coordinates and ellipse position based on fishFacingRight
    if (fishFacingRight) {
        triangle(player.moveX + 35, player.moveY + 20, player.moveX + 25, player.moveY, player.moveX + 35, player.moveY - 20);
        ellipse(player.moveX, player.moveY, 50, 15);
    } else {
        triangle(player.moveX - 35, player.moveY + 20, player.moveX - 25, player.moveY, player.moveX - 35, player.moveY - 20);
        ellipse(player.moveX, player.moveY, 50, 15);
    }
    fill('black');
    ellipse(player.moveX - 15, player.moveY, 5, 5);
}


function fishRight() {
    fill('white')
    // Change the triangle coordinates and ellipse position based on fishFacingRight
    if (fishFacingRight) {
        triangle(player.moveX - 35, player.moveY + 20, player.moveX - 25, player.moveY, player.moveX - 35, player.moveY - 20);
        ellipse(player.moveX, player.moveY, 50, 15);
    } else {
        triangle(player.moveX + 35, player.moveY + 20, player.moveX + 25, player.moveY, player.moveX + 35, player.moveY - 20);
        ellipse(player.moveX, player.moveY, 50, 15);
    }
    fill('black');
    ellipse(player.moveX + 15, player.moveY, 5, 5);
}

function gameOver() {
    background('black');
    fill('white');
    textAlign(CENTER, CENTER);
    textSize(40);
    text('Game Over', width / 2, height / 2);

    gameOver = true;
    player.up = function () {};
    player.down = function () {};
    player.left = function () {};
    player.right = function () {};
    button.mousePressed
    noLoop();

    // Hide the buttons
    button.hide();
    button2.hide();
    fishLeft.hide();
}
```

This code now featured a hunter object, that when collided with the player, caused a game over screen to be displayed. This gave the game a definite end and helped me learn about collisions, hitboxes and ways to have better control over the flow of my game.&#x20;

I then added more to the game over screen, creating a new game button that, when clicked, started the game over from the death position, allowing the player to play again.&#x20;

### Outcome

Creating a system that allowed for a death, an entitity that could cause that screen to appear and a more finished game have been the major outcomes of this stage. I have a game that is starting to   run like a game, with movement controls, independent entities and a goal- for now, just not dying to the chasing entity.&#x20;

I hope to expand into a point system for an ammount of orbs collected, whilst also creating a scaling difficulty system that will allow the player to have a progressively tough experience.&#x20;

I have moved on from an environment simulation idea completely at this point- I am now going for a more game like experience, with the simulation still remaining in the idea of living a fish's life trying to survive, however it is not akin to my original idea of a large open world with many things existing at once. This is primarily due to the difficulty and scale of the project, whilst also being due to the way I thought it could run off of perlin noise, as I have grown in the code environment I have come to learn that this would not have created many randomly moving things, but rather an array of many things moving in sync, just seperately across the screen.&#x20;

### Challenges

The largest challenge was coming to the realisation I needed to shift my game's direction and goal to better fit the time frame and my abilities.&#x20;

Amongs this was the difficulty I had making my ideas become practical, often revolving around a system of movement or speed. I had to learn about vectors to begin to understand movement mechanics, and whilst I haven't used them I still needed to learn how they affect my game and how I can work to make them fit.&#x20;

## Testing

I tested constantly throughout my coding stage, learning how to effectively use Replit's development screen.&#x20;

![](../.gitbook/assets/image.png)

I also sent my game to a friend to let him play it and reviewed what he said:&#x20;

"What am I meant to be doing here"

"Why can't I pick this thing up and why is it moving so fast"

The first question is easy to analyse, I need to introduce a more obvious goal- However, I also don't want to bore the player with a system that just explains exactly what they need to do.&#x20;

The second question is a work in progress, I need to create collision between the perlin noise system and the fish. The second part is easily adjustable and will require further testing, as I do not want to make it to easy to obtain.&#x20;

### Tests

<table><thead><tr><th width="88">Test</th><th width="133">Instructions</th><th width="256">What I expect</th><th width="185">What actually happens</th><th>Pass/Fail</th></tr></thead><tbody><tr><td>1</td><td>Run code</td><td>The hunter seeks the fish, the orange orb moves. </td><td>The fish doesnt show on the screen but the hunter still chases it. </td><td>Fail</td></tr><tr><td>2</td><td>Run code</td><td>The hunter seeks the fish, the orange orb moves.</td><td>As expected</td><td>Pass</td></tr><tr><td>3</td><td>Run code</td><td>When the hunter kills the fish game over should appear</td><td>The screen goes black, game over appears</td><td>Pass</td></tr><tr><td>4</td><td>Run code</td><td>When the game over screen displays everything should be hidden, the game should freeze, then a new game button should appear that when clicked starts a new game</td><td>The screen goes black, game over text appears but there is no button, and the fish model is still on screen.</td><td>Fail</td></tr><tr><td>5</td><td>Run code</td><td>When the game over screen displays everything should be hidden, the game should freeze, then a new game button should appear that when clicked starts a new game</td><td>The game over button does appear this time and when clicked starts the game again. </td><td>Pass</td></tr></tbody></table>

### Evidence

![](<../.gitbook/assets/image (1).png>)

This code is where the game over is being incorrectly called at any point the characters collide. The game is stopped correctly however no game over screen is displayed nor is there any action to start a new game. On the other hand:

![](<../.gitbook/assets/image (2).png>)

This shows the location the player was killed, the new game button and the game over text. When clicked, new game causes this:

![](<../.gitbook/assets/image (3).png>)

As you can see the player is still in the same spot but the hunter has been sent back to the centre of the screen, giving the player the chance to escape. Eventually that button will be constantly useable and will also reset a score to 0 and game difficulty to 0.&#x20;
