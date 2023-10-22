# Code and Link to Code.

## My finished code:&#x20;

```javascript
// Global variables
let t;
let t2;
let velo = 0.001;
let counter = 0; // Counter for collisions with orange orb
let newGameButton; // Declare newGameButton in the global scope
let orangeOrbVisible = true;
let orangeEllipseSpeedFactor = 0.01; // Initial speed factor for the orange ellipse

// Player object
const player = {
    moveX: 50,
    moveY: 50,
    speed: 10,
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

// Fish direction and hunter objects
let fishFacingRight = true;
let hunter; // Declare the hunter object

// Setup function runs once at the beginning
function setup() {
    // Create the canvas
    createCanvas(innerWidth - 10, innerHeight - 10);
    background('lightblue');

    // Initialize time variables
    t = 0;
    t2 = 0;

    // Create pause and resume buttons
    button = createButton("Pause");
    button.position(10, 10);
    button.mousePressed(noLoop);

    button2 = createButton("Resume");
    button2.position(70, 10);
    button2.mousePressed(loop);

    // Initialize the hunter object
    hunter = {
        moveX: width / 2, // Making the hunter start in the center
        moveY: height / 2, // Make the hunter start in the center
        speed: 5 + counter * 10, 
    };

    // Create New Game button and hide it until the game over is called
    newGameButton = createButton('New Game');
    newGameButton.position(width / 2 - 50, height / 2 + 50);
    newGameButton.mousePressed(startNewGame);
    newGameButton.hide();
}

// Hunter chase behavior
function hunterChase() {
    let distanceX = player.moveX - hunter.moveX;
    let distanceY = player.moveY - hunter.moveY;
    let distance = sqrt(distanceX * distanceX + distanceY * distanceY);

    if (distance > 5) { // modifiable distance that the hunter will collide with the player. 5 seems to be an optimal value here. 
        hunter.moveX += (distanceX / distance) * hunter.speed;
        hunter.moveY += (distanceY / distance) * hunter.speed;
    }
}

// Draw function runs continuously
function draw() {
    background('lightblue');

    // Calculate Perlin noise-based orb position
    // Calculate Perlin noise-based orb position
    let x = width * noise(t);
    let y = height * noise(t + 5);

    // Constrain the position to stay within the canvas bounds
    x = constrain(x, 0, width);
    y = constrain(y, 0, height);


    // Handle player movement based on key inputs
    if (keyIsDown(UP_ARROW) || keyIsDown(87)) {
        player.up();
        player.moveY = constrain(player.moveY, 0, height); // Keep player within vertical bounds
    }
    if (keyIsDown(LEFT_ARROW) || keyIsDown(65)) {
        player.left();
        fishFacingRight = false; // Fish is facing left
        player.moveX = constrain(player.moveX, 0, width); // Keep player within horizontal bounds
    }
    if (keyIsDown(RIGHT_ARROW) || keyIsDown(68)) {
        player.right();
        fishFacingRight = true; // Fish is facing right
        player.moveX = constrain(player.moveX, 0, width); // Keep player within horizontal bounds
    }
    if (keyIsDown(DOWN_ARROW) || keyIsDown(83)) {
        player.down();
        player.moveY = constrain(player.moveY, 0, height); // Keep player within vertical bounds
    }
    
    // Game over function
    function gameOver() {
        // Display game over message
        background('black');
        stroke("white");
        fill('red');
        textSize(60);
        strokeWeight(5);
        text('Game Over', (width / 2) - 170, height / 2);

        // Disable player movement
        player.up = function() { };
        player.down = function() { };
        player.left = function() { };
        player.right = function() { };

        // Hide pause and resume buttons, show "New Game" button
        button.hide();
        button2.hide();
        newGameButton.show();
        orangeOrbVisible = false
        noLoop();
    }
    
    // Check for collision between hunter and player
    let distanceToHunter = dist(player.moveX, player.moveY, hunter.moveX, hunter.moveY);
    if (distanceToHunter < 20) {
        // Collision occurred, remove fish
        gameOver()
    }

    // Check for collision between player and orange ellipse
    let distanceToOrangeEllipse = dist(player.moveX, player.moveY, x, y);
    if (distanceToOrangeEllipse < 20 && orangeOrbVisible) {
        // Collision occurred with orange ellipse
        counter++; // Increase the counter value

        // Increase the speed factor of the orange ellipse when the counter increases
        orangeEllipseSpeedFactor += 0.0005;

        // Calculate a new position for the orange ellipse
        let newX = width * noise(t + 100);
        let newY = height * noise(t + 100);

        // Update the position of the orange ellipse
        x = newX;
        y = newY;

        // Hide the orange orb
        orangeOrbVisible = false;

        // Start a timer to show the orange orb again after a delay
        setTimeout(() => {
            orangeOrbVisible = true;
        }, 2000); // 1000 milliseconds = 1 seconds delay
    }

    // Draw the orange orb if it's currently meant to be visible
    if (orangeOrbVisible) {
        fill('orange');
        ellipse(x, y, 20, 20);
    }

    velo = (counter * 2) / 1000
    
    //  * orangeEllipseSpeedFactor / 5; // Update the overall game speed based on the counter, commented so it doesn't work, possible future use
    // Draw fish based on its direction
    if (fishFacingRight) {
        fishLeft();
    } else {
        fishRight();
    }

    // Update the hunter's position
    hunterChase();

    // Draw the hunter
    fill('black');
    ellipse(hunter.moveX, hunter.moveY, 30, 30);

    // Update the time variables for the Perlin noise calculations
    t = t + velo;
    t2 = t2 + velo;

    // Display the speed of the orange orb
    textSize(20);
    text(velo, 5, height - 30);
    text('Movement speed', 5, height - 10);

    textSize(40);
    text(counter, 200, 30);
}

// Fish facing left model 
function fishLeft() {
    fill('white');
    stroke('black'); // Set the stroke color, important so on death the fish stroke doesn't change colour. 
    // Draw fish facing left
    triangle(player.moveX - 35, player.moveY + 20, player.moveX - 25, player.moveY, player.moveX - 35, player.moveY - 20);
    ellipse(player.moveX, player.moveY, 50, 15);
    fill('black');
    ellipse(player.moveX + 15, player.moveY, 5, 5);

    noStroke(); // Reset to noStroke
}

// Fish facing right model
function fishRight() {
    fill('white');
    stroke('black'); // Set the stroke color
    // Draw fish facing right
    triangle(player.moveX + 35, player.moveY + 20, player.moveX + 25, player.moveY, player.moveX + 35, player.moveY - 20);
    ellipse(player.moveX, player.moveY, 50, 15);
    fill('black');
    ellipse(player.moveX - 15, player.moveY, 5, 5);

    noStroke(); // Reset to noStroke
}

// Start new game function
function startNewGame() {
    // Reset game variables to the base values
    gameOver = false;
    counter = 0; // Reset the counter to 0

    // Re enable player movement
    player.up = function() {
        this.moveY -= this.speed;
    };
    player.down = function() {
        this.moveY += this.speed;
    };
    player.left = function() {
        this.moveX -= this.speed;
        fishFacingRight = false; // Fish is facing left
    };
    player.right = function() {
        this.moveX += this.speed;
        fishFacingRight = true; // Fish is facing right
    };

    // Reset the hunter's position to center of the screen
    hunter.moveX = width / 2;
    hunter.moveY = height / 2;

    // Reset the stroke weight to its original value
    strokeWeight(1); 
    
    // Show the pause and resume buttons whilst hiding the New Game button
    button.show();
    button2.show();
    newGameButton.hide();
    orangeOrbVisible = true
    // Resume game loop
    loop();
}

```

## HTML:&#x20;

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width">
    <title>repl.it</title>
    <link href="style.css" rel="stylesheet" type="text/css" />
    <script src="https://cdn.jsdelivr.net/npm/p5@1.4.1/lib/p5.js"></script>
      
  </head>
  <body>
    <script src="script.js"></script>
  </body>
</html>
```

## CSS:

```css
body {
  overflow-y: hidden; /* Hide vertical scrollbar */
  overflow-x: hidden; /* Hide horizontal scrollbar */
}
```

## Link To Replit:

[https://replit.com/@OliverBeach/Final-Project-Fish-Game#script.js](https://replit.com/@OliverBeach/Final-Project-Fish-Game#script.js)
