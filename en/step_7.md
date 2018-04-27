## Level 2

What you’re going to do on this card is add a new level to the game that the player can get to just by pressing a button. Later, you can change it so they need a certain number of points, or something else, to get there.

+ First, create a new button sprite by either adding it from the library or drawing your own. I did a bit of both and came up with this: 

![](images/level1.png)

Now, the code for this button is kinda clever; it’s designed so that every time you click it, it will take you to the next level, how ever many levels there are.

+ Add this to your button sprite: 

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

![](images/level2and3.png)
 
`max-level`{:class="blockdata"} is the highest level
`min-level`{:class="blockdata"} is the lowest level
`current-level`{:class="blockdata"} is the level the player is on right now

+ These all need to be set by the programmer \(you!\), so if you add a third level, don’t forget to change `max-level`{:class="blockdata"}!

The code uses broadcasts to tell the other sprites which level to display, and to clear up the collectables.

Now you need to get the other sprites to respond to those broadcasts! Start with the easy one: clearing all the collectables. If you just tell them to `hide`, all the existing clones will. 

+ Add this to the `collectable` sprite: 

```blocks
    when I receive [collectable-cleanup v]
    hide
```

![](images/level4.png)

Since one of the first things any new clone already does is show itself, that means you don’t even have to worry about turning this off for them!

Now to switch the `platforms` sprite! You can design your own new level later, if you like, but for now let’s use the one I’ve already included \(you’ll see why on the next card!\). 

+ You just need to add this code to the `Platforms` sprite.

```blocks
    when I receive [level-1 v]
    show
```

```blocks
    when I receive [level-2 v]
    hide
```

It takes the messages sent out by **joining** the `level-`{:class="blockdata"} and the `current-level`{:class="blockdata"} variable and uses them to change the `platforms` costume. 


![](images/level5.png)

+ For the `Enemy` sprite, you just need to make sure it disappears on level 2 \(or move it to another platform!\), like this: 

```blocks
    when I receive [level-2 v]
    hide
```

![](images/level6.png)

+ Finally, the player character needs to separate out the coordinates from the `reset character`{:class="blockmoreblocks"} **more block**, so the character goes to the right place, and call the first level when the game starts. 

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

For each level, you set the starting coordinates and then **call** `reset-character`{:class="blockmoreblocks"}

```blocks
    define reset-character
    set [can-jump v] to [true]
    set [x-velocity v] to [0]
    set [y-velocity v] to [-0]
    go to x: (start-x) y: (start-y)
```
The variables are used for the starting coordinates instead of fixed **x** and **y** values in the `go to`{:class="blockmotion"} block

```blocks
    define reset-game
    set rotation style [left-right v]
    set [jump-height v] to [15]
    set [gravity v] to [2]
    set [x-speed v] to [1]
    set [y-speed v] to [1]
    set [lives v] to [3]
    set [points v] to [0]
    broadcast (join [level-](min-level))
```
The `min-level`{:class="blockdata"} is broadcasted to reset the character and the game.

![](images/level7.png)
 