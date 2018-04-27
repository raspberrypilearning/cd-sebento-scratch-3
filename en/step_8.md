## Moving platforms

The reason I asked you to use my version of level 2 is the gap you might have noticed in the middle. You’re going to create a platform that moves through that gap, that the player can jump onto and ride. 

![Another level with different platforms](images/movingPlatforms.png)

+ First, you’ll need the sprite for the platform. You can create this by adding any sprite, naming it `Moving-Platform` and using the costume customisation tools in the **costumes** tab to make it look like the other platforms \(use vector mode\).

Time to start adding some code! 

+ Begin with the basics: To make a never-ending set of platforms moving up the screen, you’ll need to **clone** the platform at some sort of interval. I picked 4 seconds. 

+ You also need to make sure that there’s an on/off switch for making the platforms, so they don’t show up on level 1. I’m using a **variable** called `create-platforms`{:class="blockdata"}. 

Here's how my code looks so far for the new sprite:

```blocks
    when green flag clicked
    hide
    forever
        wait (4) secs
        if <(create-platforms) = [true]> then
            create clone of [myself v]
        end
    end
```

+ Then add the clone's code:

```blocks
    when I start as a clone
    show
    forever
        if <(y position) < [180]> then
            change y by (1)
            wait (0.02) secs
        else
            delete this clone
        end
    end
```

This code is simple: Move up to the top of the screen, slowly enough for the player to jump on and off, and then disappear. 

+ You need to make the platforms disappear/reappear based on the level changing events and on the `game over` event. 

```blocks
    when I receive [level-1 v]
    set [create-platforms v] to [false]
    hide

    when I receive [level-2 v]
    set [create-platforms v] to [true]

    when I receive [game over v]
    hide
    set [create-platforms v] to [false]
```

+ Now, if you try to actually play the game, the Player Character falls through the platform! Any idea why? 

It’s because the physics code doesn’t know about the platform. It’s actually an easy fix: on the `Player Character` sprite you need to replace every `touching “Platforms”`{:class="blocksensing"}  with an `OR`{:class="blockoperators"} operator, where you check for **either** `touching “Platforms”`{:class="blocksensing"}  **OR** `touching “Moving Platform”`{:class="blocksensing"} 
 
+ Go through the code on the `Player Character` sprite and everywhere you see this block

```blocks
    <touching [Platforms v] ?>
```

replace it with

```blocks
    <<touching [Platforms v] ?> or <touching [Moving-Platform v] ?>>
```

