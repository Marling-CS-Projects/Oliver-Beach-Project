# Cycle 8: Minor tweaks and error fixes

## Design

This cycle was focussed on trying to fix some of the noticeable bugs my game has, whilst also adding little quality of life details to just push the game up a little more. These changes will not particularly change the way in which the game is played, however they will hopefully improve the players experience in the game.&#x20;

### Objectives

* [ ] Add little tweaks and details
* [ ] Fix the ability to take the fish out of bounds on the edge of the map.&#x20;
* [ ] Make it so that the player can die more than once after pressing the new game button.&#x20;

### Usability Features

I need to make it so that the game stays simple and I hope this cycle will not change that, instead this cycle will just build on what I have already created and hopefully improve the overall game experience.&#x20;

### Key Variables

| Variable Name | Use |
| ------------- | --- |
|               |     |
|               |     |

### Pseudocode

```javascript
Game start
Create hunter
Create player
Create orb

When player collides with hunter 
    game over
    
when game over = true
    display game over screen
    hide all entities
    new game button
        if new game button = true
        show all entities
        hide game over screen
        
w a s d move player 
hunter chase player, if position < 5px kill

if player dist < 5 from orange
    counter + 1

display counter 

orangeEllipse velocity = counter
hunter velocity = counter / 5

if player collides with border 
    bounce player 10 px
    
text align (center) // need to fix text placements in the game. 
```

## Development

First thing I decided I wanted to fix was the way in which the NEW GAME text on death was not centered. This type of fix doesn't change the gameplay in any way however I feel like it is a noticeable thing and takes away from the clean, "professional" game I have been attempting to create.&#x20;

```javascript
textAlign(CENTER, CENTER)
    fill('red');
    textSize(60);
    text('Game Over', (width / 2) - 170, height / 2);
```

![](<../.gitbook/assets/image (1) (1).png>)

This is the original text, as you can see it is off centre. Whilst this doesn't change the gameplay in any way I feel like the change has a massive effect and helps make it look more serious.

```javascript
    stroke("white");
    fill('red');
    textSize(60);
    text('Game Over', (width / 2) - 170, height / 2);
```

This code centres the text, whilst having the new benefit of creating an outline that helps the text stand out on the black background.  Whilst it isn't perfectly centered, the nice thing about the change I made is that it works no matter the size of the window of the game, something that couldn't be said for all previous versions.&#x20;

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

And with the small addition of:&#x20;

```javascript
strokeWeight(5);
```

The game over text no longer has blurry borders and now looks like:

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

The next area I was going to tackle fixing was the problem of how the fish (player) could escape the camera and canvas by going off the side of the screen, becoming completely lost.&#x20;

&#x20;**VIEDEO EXAMPLE OF CURRENT PROBLEM**

This problem can be an annoyance to the majority of players as they just end up off the screen, but for a younger player who is likely to just hold one direction at some point, that run would be over as the player would get lost off the screen then possibly either die to the hunter or just never find their way back to the screen.&#x20;

This has been a problem in the game for a long time however it has not been a priority to fix it as it doesn't ruin the game unless the player leaves the screen. However it is finally time to fix the issue, so I came up with two options to fix the problem.&#x20;

<table data-full-width="false"><thead><tr><th>Invisible border at edge .</th><th>Teleportation to opposite side. </th></tr></thead><tbody><tr><td>Very visible when the player is at the edge of the screen. </td><td>Could be hard to learn at the start</td></tr><tr><td>Simple to add.</td><td>Tougher to add</td></tr><tr><td>Prevents the game from changing too much.</td><td>Adds new dynamics to the game</td></tr><tr><td>However:</td><td>However:</td></tr><tr><td>Less interactive</td><td>Much more interactive, allows the player to escape the hunter or catch up to the orange orb. </td></tr><tr><td>Less interesting / fun</td><td>Could be more fun, as the player unlocks new options.</td></tr><tr><td>Adds no new features / ideas. </td><td>Adds new potential to the game. </td></tr></tbody></table>

### Outcome



### Challenges



### Tests

<table><thead><tr><th width="88">Test</th><th width="133">Instructions</th><th width="187">What I expect</th><th width="210">What actually happens</th><th>Pass/Fail</th></tr></thead><tbody><tr><td>1</td><td></td><td></td><td></td><td></td></tr><tr><td>2</td><td></td><td></td><td></td><td></td></tr><tr><td>3</td><td></td><td></td><td></td><td></td></tr><tr><td>4</td><td></td><td></td><td></td><td></td></tr></tbody></table>

### Evidence
