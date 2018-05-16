## Level 2

With this card, you're going to add a new level to the game that the player can get to by just pressing a button. Later, you can change your code to make it so they need a certain number of points, or something else, to get there.

+ First, create a new button sprite by either adding it from the library or drawing your own. I did a bit of both and came up with this: 

![The button sprite to switch levels](images/levelButton.png)

Now, the code for this button is kinda clever: it’s designed so that every time you click it, it will take you to the next level, however many levels there are.

+ Add these scripts to your button sprite: 

```blocks
    when green flag clicked
    set [max-level v] to [2]
    set [min-level v] to [1]
    set [current-level v] to [1]
```

```blocks
    when this sprite clicked
    change [current-level v] by (1)
    if <(current-level) > (max-level)> then
        set [current-level v] to (min-level)
    end
    broadcast [collectable-cleanup v]
    broadcast (join [level-](current-level))
```
 
+ `max-level`{:class="blockdata"} is the highest level
+ `min-level`{:class="blockdata"} is the lowest level
+ `current-level`{:class="blockdata"} is the level the player is on right now

+ These all need to be set by the programmer \(you!\), so if you add a third level, don’t forget to change the value of `max-level`{:class="blockdata"}!

The code uses broadcasts to tell the other sprites which level to display, and to clear up the collectables.

Now you need to get the other sprites to respond to these broadcasts! Start with the easy one: clearing all the collectables.  

+ Just tell all the existing clones to `hide` by adding this to the `Collectable` sprite: 

```blocks
    when I receive [collectable-cleanup v]
    hide
```

Since one of the first things any new clone already does is show itself, this new code means means you don’t even have to worry about turning this behaviour off for them!

Now to switch the `Platforms` sprite! You can design your own new level later, if you like, but for now let’s use the one I’ve already included \(you’ll see why on the next card!\). 

+ Add this code to the `Platforms` sprite:

```blocks
    when I receive [level-1 v]
    switch costume to [Level 1 v]
    show
```

```blocks
    when I receive [level-2 v]
    switch costume to [Level 2 v]
    show
```

It takes the `joined`{:class="blockoperators"} messages of `level-`{:class="blockdata"} and `current-level`{:class="blockdata"} that the button sprite sends out, and uses them to change the `Platforms` costume. 

+ For the `Enemy` sprite, you just need to make sure it disappears on level 2, like this: 

```blocks
    when I receive [level-1 v]
    show
```

```blocks
    when I receive [level-2 v]
    hide
```
If you wanted to, you could make it move to another platform instead – in that case you would use a `go to`{:class="blockmotion"} block instead of the `show`{:class="blocklooks"} and `hide`{:class="blocklooks"}.

Finally, the player character needs to separate out the coordinates from its reset code, so that it goes to the right place for each level. You also want to switch to the first level when the game starts. 

+ Start by creating variables for the starting coordinates, `start-x`{:class="blockdata"} and `start-y`{:class="blockdata"}, and plug them into the `go to`{:class="blockmotion"} block in `reset-character`{:class="blockmoreblocks"} instead of the fixed **x** and **y** values.

```blocks
    define reset-character
    set [can-jump v] to [true]
    set [x-velocity v] to [0]
    set [y-velocity v] to [-0]
    go to x: (start-x) y: (start-y)
```

+ Then for each level, set the starting coordinates and add a **call** to `reset-character`{:class="blockmoreblocks"}:

```blocks
    when I receive [level-1 v]
    set [start-x v] to [-183]
    set [start-y v] to [42]
    reset-character :: custom
```

```blocks
    when I receive [level-2 v]
    set [start-x v] to [-218]
    set [start-y v] to [-143]
    reset-character :: custom
```

+ Lastly, remove the call to `reset-character`{:class="blockmoreblocks"} from `reset-game`{:class="blockmoreblocks"} and instead broadcast the minimum level. The code you added above will then set up the correct starting coordinates and call `reset-character`{:class="blockmoreblocks"} for you.

```blocks
    define reset-game
    set size to (35) %
    set rotation style [left-right v]
    set [jump-height v] to [15]
    set [gravity v] to [2]
    set [x-speed v] to [1]
    set [y-speed v] to [1]
    set [lives v] to [3]
    set [points v] to [0]
    broadcast (join [level-](min-level))
```

--- collapse ---
---
title: Resetting the character and the game
---

Notice that the first block in the `Player Character` sprite's main green flag script is a call to the `reset-game`{:class="blockmoreblocks"} **more** block. 

This block sets up all the variables for a new game and then calls the `reset-character`{:class="blockmoreblocks"} **more** block, which places the character back in its starting position.

Having the `reset-character`{:class="blockmoreblocks"} code in its own block separate from `reset-game`{:class="blockmoreblocks"}  allows you to reset the character to different positions without having to also reset the whole game.

--- /collapse ---