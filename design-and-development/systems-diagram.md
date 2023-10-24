# 2.1 Design Frame

## Systems Diagram

<div align="left" data-full-width="false">

<figure><img src="../.gitbook/assets/RYG Systems Diagram (1).png" alt=""><figcaption></figcaption></figure>

</div>

![](https://github.com/Marling-School/alevel-project-template/blob/docs/.gitbook/assets/System%20Diagram.jpg)

This diagram shows the different parts of the game that I will focus on creating. I have split each section into smaller sub-sections. Throughout the development stage, I will pick one or two of these sections to focus on at a time to gradually build up and piece together the game. I have broken the project down this way as it roughly corresponds to the success criteria.

## Usability Features

Usability is an important aspect to my game as I want it to be accessible to all. There are 5 key points of usability to create the best user experience that I will be focusing on when developing my project. These are:

### Effective

Users can achieve a goal with completeness and accuracy. To do this, I will make it easy for the players to realise that they need to reach a goal in order to continue the game. I will make this goal clear to see so there is no confusion over what the players need to do.

#### Aims

* Create multiple, clear goals to reach&#x20;
* Make it clear when a goal is complete

### Efficiency

The speed and accuracy to which a user can complete the goals. To do this, I will create a menu system which is easy to navigate through in order for to find what you are looking for. The information which is more important can be found with less clicks.

#### Aims

* Create a menu system that is quick and easy to navigate through
* Create a controls system that isn't too complicated but allows the player to do multiple actions

### Engaging

The game is engaging for the user to play. To do this, I will create an interesting map and creatures to keep the players engaged and allow them to have fun while playing the game. Using vector style art will also make the game nicer to look at than blocks, so will draw more people in, keeping them engaged.

#### Aims

* Create a nice looking world
* Create lots of interesting creatures and plants.
* Incorporate a style of game art the suits the game

### Error Tolerant

The solution should have as few errors as possible and if one does occur, it should be able to correct itself. To do this, I will write my code to manage as many different game scenarios as possible so that it will not crash when someone is playing it.

#### Aims

* The game doesn't crash
* The game does not contain any bugs that damage the user experience

### Easy To Learn

The solution should be easy to use and not be over complicated. To do this, I will create simple controls for the game. I will make sure that no more controls are added than are needed in order to keep them as simple as possible for the players.

#### Aims

* Create a list of controls for the game
* Create an in-level help system that helps players learn how to play the game initially, reducing in help as the game progresses

## Pseudocode for the Game

This is the pseudocode for my revised game.&#x20;

### Pseudocode for game

This is the basic layout of the object to store the details of the game. This will be what is rendered.

```
function setup
    createCanvas
    background (colour)
    time = 
    create button (pause)
    create button (resume)
    create button (New Game)
```

### Pseudocode for the main

This shows the basic layout of code for my game.

```
cosnt player
    speed
    up
        move up
    down
        move down
    left
        move left
    right
        move right
        
function setup
    createCanvas
    background (colour)
    time = 
    create button (pause)
    create button (resume)
    create button (New Game)
    
function hunterChase
    if hunter is != player XY
        make hunter X Y = player X Y

function draw
    background
    let x = perlin noise
    let y = perlin noise
    if W or up arrow is pressed
        move up
    if S or down arrow is pressed
        move down
    if A or left arrow is pressed
        move left
    if D or right arrow is pressed
        move right 
        
    function gameOver
        if hunter X Y = player X Y 
            display game over
            bacground = black
            
    function orrangeEllipseCollision
        if orangeEllipse = player X Y 
            collide
            counter + 1
            
function fishRight
    fish right model
    
function fishLeft
    fish left model
```
