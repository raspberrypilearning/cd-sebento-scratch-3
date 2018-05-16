## Super power-ups!

Now that you have a new power-up working, it’s time to make it do something cool! How about making it 'rain' power-ups for a few seconds, instead of just giving out an extra life? 
 
To make that work, you need to create another piece of code that you can start while the `react-to-player`{:class="blockmoreblocks"} block finishes running. The way to make this happen is to use the `broadcast`{:class="blockevents"} block to send a message to another piece of code inside this sprite. 

+ Create this block for the `Collectable` sprite. Let’s call the message `collectable-rain`{:class="blockevents"}, since that’s basically what it does!

```blocks
    when I receive [collectable-rain v]
    set [collectable-frequency v] to [0.000001]
    wait (1) secs
    set [collectable-frequency v] to [1]
```

--- collapse ---
---
title: What does the new code do?
---

This block just sets `collectable-frequency`{:class="blockdata"} to a very small number \(change it to different values, see what happens!\) and then waits a second and changes it back to `1`.

This doesn’t look like it should do much, but if you think about what’s happening during that second, the `when green flag clicked`{:class="blockevents"} code is still running, and the `repeat until`{:class="blockcontrol"} loop in it is looping. Look at the code in that loop: 

```blocks
    repeat until <not <(create-collectables) = [true]>>
        if < [50] = (pick random (1) to (50))> then
            set [collectable-type v] to [2]
        else
            set [collectable-type v] to [1]
        end
        wait (collectable-frequency) secs
        go to x: (pick random (-240) to (240)) y:(-179)
        create clone of [myself v]
    end
```

Instead of pausing the code here for a second, it’s only pausing for **one millionth** of a second, meaning that the loop will run many more times than normal because of the smaller value of `collectable-frequency`{:class="blockdata"}. This means that the code is going to create **a lot** more power-ups in that second than it normally would. Can you think of any problems that might cause? There’ll be a lot more super-farts…what if I kept catching them?

--- /collapse ---

Now you have that `broadcast`{:class="blockevents"} block ready, but it’s not being used yet. 

+ This next part’s easy. Just update `react-to-player`{:class="blockmoreblocks"} to look like this, so it broadcasts `collectable-rain`{:class="blockevents"} when the player touches a type `2` power-up. 

```blocks
    define react-to-player (type)
    if <(type) = [1]> then
        change [points v] by (collectable-value)
    end
    if <(type) = [2]> then
        broadcast [collectable-rain v]
    end
```

#### Get creative!
 
+ Based on this card and the previous one, you can now make as many different power-ups as you want! What about one that gives out 20 times the usual number of points, adds three lives, or maybe means the player can’t run out of lives while it’s on? Come up with some cool power-ups and see if you can make them!
