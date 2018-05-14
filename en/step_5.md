## Power-ups

On the last card you saw the collectable I created. It’s a fart cloud that just adds one point when you grab it. That’s pretty boring.

On this card, you’re going to create a new type of collectable, but you’re going to do it in a way that will make adding other types of collectables easy, so you can invent your own power-ups and bonuses and really make the game your own!

+ Add a new costume to the `Collectable` sprite for your new power-up. I've drawn a supersize fart cloud, but you can make whatever you like!

I’ve already included some pieces to make this easier for you with the `collectable-type`{:class="blockdata"} variable and the `pick-costume`{:class="blockmoreblocks"} **More** block. You’re going to need to improve on them though. 

--- collapse ---
---
title: How do these pieces work?
---

The `pick-costume`{:class="blockmoreblocks"} block works a bit like the `lose`{:class="blockmoreblocks"} block, but you might notice something extra that it has: it takes an **input** variable called `type`.

```blocks
    define pick-costume (type)
    if <(type) = [1]> then
        switch costume to [fartCloud v]
    end
```
    
When the block runs, what it will do is this:

 1. It looks at the `type` input
 1. If the value of `type` is equal to `1`, then it switches to the `fartCloud` costume

Take a look at where the block is used:

```blocks
    when I start as a clone
    pick-costume (collectable-type) :: custom
    show
    repeat until <(y position) < [170]>
        change y by (collectable-speed)
        if <touching [Player Character v]?> then
            change [points v] by (collectable-value)
            delete this clone
```

The `collectable-type`{:class="blockdata"} variable is **passed** as an input to `pick-costume`{:class="blockmoreblocks"}. Inside the code for `pick-costume`{:class="blockmoreblocks"}, this then becomes the variable `type`. It has the same value as `collectable-type`{:class="blockdata"}.

--- /collapse ---

### Create the new type of collectable at random times

First, you need to set the `collectable-type`{:class="blockdata"}. This variable is just a number that tells the `pick-costume`{:class="blockmoreblocks"} block what costume, rules, etc., to use for the collectable you're telling it to make. You’re going to want to pick the number at random, to make a random collectable and keep things interesting. 

+ Find the `repeat until`{:class="blockcontrol"} loop inside the green flag code for the `Collectable` sprite and add the `if...else`{:class="blockcontrol"} code shown below.

```blocks
    repeat until <not <(create-collectables) = [true]>>
        if <[50] = (pick random (1) to (50))> then
            set [collectable-type v] to [2]
        else
            set [collectable-type v] to [1]
        end
        wait (collectable-frequency) secs
        go to x: (pick random (-240) to (240)) y: (179)
        create clone of [myself v]
```

This example gives a 1 in 50 chance of setting the `collectable-type`{:class="blockdata"} to `2`.

--- collapse ---
---
title: Pro tip!
---

You can set a different value as the `collectable-type`{:class="blockdata"} for each clone. 

Think of it like creating a new copy of the main `Collectable` sprite using the value that was in `collectable-type`{:class="blockdata"} the instant that `Collectable` clone was created. 

One of the things that makes clones special is that they cannot change the values of any variables they start with. They effectively have **constant** values.

--- /collapse ---

Great! Now you’re setting a different value for the `collectable-type`{:class="blockdata"}, but none of the code knows what to do with it yet! 

+ Tell the `pick-costume`{:class="blockmoreblocks"} **More** block to set the new costume whenever it gets the new value for `type`, like this \(using whatever costume you picked\): 

```blocks
    define pick-costume (type)
    if <(type) = [1]> then
        switch costume to [fartCloud v]
    end
    if <(type) = [2]> then
        switch costume to [superFart v]
    end
```

### Create the power-up code

Now you need to decide what the new collectible  will do. We’ll start with something simple: giving the player a new life. On the next card, you’ll make it do something cooler. 

+ Go into the **More** section and click **Make a Block**. Name the new block `react-to-player`{:class="blockmoreblocks"}.

![Type in the name for the block](images/powerupMakeName.png)

+ Expand the **Options** section and add a **number input**. Name it `type`.

![Adding a number input to the block](images/powerupMakeInput.png)

+ Click **OK**. 

+ Make the `react-to-player`{:class="blockmoreblocks"} block either increase the points or increase the player’s lives, depending on the `type` of power-up.  

```blocks
    define react-to-player (type)
    if <(type) = [1]> then
        change [points v] by (collectable-value)
    end
    if <(type) = [2]> then
        change [lives v] by [1]
    end
```

+ Update the `when I start as a clone`{:class="blockevents"} code to replace the points increase with a **call** to `react-to-player`{:class="blockmoreblocks"}, **passing** `collectable-type`{:class="blockdata"}. Normal fart clouds still boost points but the new power-up adds lives. 

```blocks
    if <touching [Player Character v] ?> then
        react-to-player (collectable-type) :: custom
        delete this clone
    end
```
