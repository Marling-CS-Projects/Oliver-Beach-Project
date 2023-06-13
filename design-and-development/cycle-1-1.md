# 2.2.2 Cycle 2: Implementation System

## Design

This cycle focuses primarily on the way that the player will take something from a bar along the base of the screen, before then implementing said thing into one of the cells. I hope to have these things be such as a squirrel, a bush and a tree with possible water.&#x20;

### Objectives

The objective of creating water is a definite goal, but this cycle it may be difficult as I am still learning JavaScript and this requires making a cell "locked" to placement of other things- so that a squirrel can only travel on grass and trees, for example.&#x20;

* [x] Create a taskbar that displays the things that can be placed down.&#x20;
* [ ] Allow the player to "grab" one of these things off the taskbar and place it into the world.&#x20;
* [ ] Create a system that only allows things to be placed in certain areas.&#x20;

### Usability Features

It needs to be a **simple feature** that allows the player to **see what is unlocked and what they can implement**.&#x20;

The player needs to be able to **fluidly implement** the things from the taskbar, including the **implementation of rocks and plants**.&#x20;

### Key Variables

| Variable Name | Use |
| ------------- | --- |
|               |     |
|               |     |
|               |     |
|               |     |

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

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

This was the first major issue I found- my code wasn't responding to being clicked as a button. As it turned out, the issue was that I had set everything as a class and not as an Id, meaning it didn't respond as I expected.&#x20;

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
