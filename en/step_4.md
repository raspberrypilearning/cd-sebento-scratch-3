## Powerups

On the last card you saw the collectable I created. It’s a star that just adds one point when you grab it. That’s pretty boring.

On this card, you’re going to create a new collectable, but you’re going to do it in a way that makes adding more collectables easy, so you can invent your own powers and bonuses and really make the game your own!

+ Add a new costume to the `Collectable` sprite for your new power-up. I like the lightning bolt, but pick whatever you like.

Notice that I’ve already included some pieces to make this easier for you with the `collectable-type` variable and the `pick costume` **more block**. You’re going to need to improve on them though. 

![](images/powerup1.png)

In the code above, `collectable-type` is **passed** to `pick-costume` when it’s **called**, where it becomes `type` and can be used inside the **more block**

+ First, you need to set the collectable type. It’s just a number, used to tell the program what costume, rules etc. to use for the collectable. You’re going to want to pick it at random, to keep things interesting. This example gives a 1/50 chance: 

![](images/powerup2.png)

**Pro Tip!** 
There can be a different value set as the `collectable-type` for each clone. Think of it like creating a new copy of the variable on the main `collectable` sprite with the value that was in `collectable-type` the instant that clone was created. One of the things that makes clones special is that they cannot change the values of any variables they start with. They are effectively **constant** values.

Great! Now you’re setting a different value for the collectable type, but none of the code knows what to do with it yet! 

+ First, just teach the `pick-costume` **more block** to set the new costume when it gets the new type, like this \(using whatever costume you picked\): 

![](images/powerup3.png)

Now you need to decide what the powerup will do. We’ll start with something simple: giving the player a new life. On the next card, you’ll make it do something cooler. 

+ To create the powerup code, go into **more blocks** and **Make a Block**. Expand the **Options** section and add a **number input**. Name the block `react-to-player` and the number input `type`. Make the `react-to-player` block either give the same points prize that the star is already giving, or increase the player’s lives, depending on the `type` of powerup.  

![](images/powerup4and5.png)

+ Update the `when I start as a clone` code to replace the points increase with a **call** to `react-to-player`, **passing** `collectable-type`. Stars still boost points but the new powerup adds lives. 

![](images/powerup6.png)

