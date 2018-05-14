## Power-ups

On the last card you saw the collectable I created. It’s a fart cloud that just adds one point when you grab it. That’s pretty boring.

On this card, you’re going to create a new collectable, but you’re going to do it in a way that makes adding more collectables easy, so you can invent your own power-ups and bonuses and really make the game your own!

+ Add a new costume to the `Collectable` sprite for your new power-up. I've drawn a supersize fart cloud, but you can make whatever you like!

I’ve already included some pieces to make this easier for you with the `collectable-type`{:class="blockdata"} variable and the `pick-costume`{:class="blockmoreblocks"} **More** block. You’re going to need to improve on them though. 

--- collapse ---
---
title: How do these pieces work?
---

The `pick-costume`{:class="blockmoreblocks"} block works a bit like the `lose`{:class="blockmoreblocks"} block, but you might notice something extra that it has: it takes an **input**.

```blocks
    define pick-costume (type)
    if <(type) = [1]> then
        switch costume to [fartCloud v]
    end
```
    
When the block runs, what it will do is this:

 1. It looks at the input, which it calls `type`
 2. If the value of `type` is equal to `1`, then it switches to the fartCloud costume

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

First, you need to set the `type`. It’s just a number used to tell the program what costume, rules, etc. to use for the collectable. You’re going to want to pick the number at random to keep things interesting. 
