## Βραβεία

At the moment you have just one type of collectible: a fart cloud that gives you one point when you grab it. Σε αυτήν την καρτέλα, θα δημιουργήσεις έναν νέο τύπο βραβείων, με τρόπο που θα διευκολύνει την προσθήκη κι άλλων τύπων βραβείων. Στη συνέχεια, μπορείς να εμπνευστείς τα δικά σου βραβεία και μπόνους και να κάνεις πραγματικά το δικό σου παιχνίδι!

Έχω ήδη συμπεριλάβει κώδικα για να το κάνεις αυτό με τη μεταβλητή `τύπος-βραβείου`{:class="block3variables"} και το μπλοκ`επιλογή-ενδυμασίας`{: class="block3myblocks"} από τις **Εντολές μου**. Ωστόσο, θα πρέπει να τον βελτιώσεις.

Ας ρίξουμε μια ματιά στο πώς λειτουργεί το βραβείο αυτή τη στιγμή.

Στον κώδικα για το αντικείμενο `Βραβείο`, βρες την εντολή `όταν ξεκινήσω ως κλώνος`{:class="block3events"}. The blocks you should look at are the ones that give you points for collecting a fart:

```blocks3
    αν <touching [Player Character v]?> τότε
        άλλαξε [πόντους v] κατά (τιμή-βραβείου ::μεταβλητές)
        διέγραψε αυτόν τον κλώνο
```

και αυτό που επιλέγει μια ενδυμασία για τον κλώνο:

```blocks3
    επιλογή-ενδυμασίας (τύπος-βραβείου ::μεταβλητές) :: προσαρμοσμένο
```

--- collapse ---
---
title: Πώς λειτουργεί η επιλογή ενδυμασίας;
---

Το μπλοκ `επιλογή-ενδυμασίας`{:class="block3myblocks"} λειτουργεί περίπου σαν το μπλοκ `χάνει`{:class="block3myblocks"}, αλλά έχει κάτι επιπλέον: παίρνει μια**παράμετρο** που ονομάζεται `τύπος`.

```blocks3
    όρισε επιλογή-ενδυμασίας (τύπος)
    εαν <(τύπος ::μεταβλητή) = [1]> τότε
        εναλλαγή ενδυμασίας σε [fartCloud v]
    τέλος
```

Όποτε εκτελείται το μπλοκ `επιλογή-ενδυμασίας`{:class="block3myblocks"}, αυτό που κάνει είναι:

 1. Εξετάζει την παράμετρο εισόδου `τύπος`{:class="block3myblocks"}
 1. Εάν η τιμή της `τύπος`{:class="block3myblocks"} είναι `1`, τότε αλλάζει στην ενδυμασία `fartCloud`

Ρίξε μια ματιά στο μέρος του κώδικα που χρησιμοποιεί το μπλοκ:

```blocks3
    όταν εκκινώ ως κλώνος
    επιλογή-ενδυμασίας (τύπος-βραβείου ::μεταβλητές) :: προσαρμογή
    εμφάνιση
    επανάληψη έως <(θέση y) > [170]>
        αλλαγή y κατά (ταχύτητα-βραβείου ::μεταβλητές)
        εάν <0 > τότε
            αλλαγή [πόντοι v] κατά (τιμή-βραβείου ::μεταβλητές)
            διαγραφή κλώνου
```

Μπορείς να δεις ότι η μεταβλητή `τύπος-βραβείου`{:class="block3variables"} **περνάει** στο μπλοκ `επιλογή-ενδυμασίας`{:class="block3myblocks"}. Μέσα στον κώδικα του `επιλογή-ενδυμασίας`{:class="block3myblocks"}, ο `τύπος-βραβείου`{:class="block3variables"} χρησιμοποιείται στη συνέχεια ως παράμετρος εισόδου (`τύπος`{:class="block3myblocks"}).

--- /collapse ---

### Πρόσθεσε μια ενδυμασία για το νέο είδος βραβείου

Φυσικά, τώρα το αντικείμενο`Βραβείο` έχει μόνο μια ενδυμασία, αφού υπάρχει μόνο ένας τύπος βραβείου. You're about to change that!

--- task ---

Add a new costume to the `Collectable` sprite for your new power-up. I've drawn a supersize fart cloud, but you can make whatever you like!

--- /task ---

--- task ---

Next you need to tell the `pick-costume`{:class="block3myblocks"} **My blocks** block to set the new costume whenever it gets the new value for `type`, like this \(using whatever costume name you picked\):

```blocks3
    define pick-costume (type)
    if <(type ::variable) = [1]> then
        switch costume to [fartCloud v]
    end
+    if <(type ::variable) = [2]> then
        switch costume to [superFart v]
    end
```

--- /task ---

### Create the power-up code

Now you need to decide what the new collectable will do. We’ll start with something simple: giving the player a new life. On the next card, you’ll make it do something cooler.

--- task ---

Go into the **My blocks** section and click **Make a Block**. Name the new block `react-to-player`{:class="block3myblocks"} and add a **number input** named `type`{:class="block3myblocks"} .

![Type in the name for the block](images/powerupMakeName.png)

Click **OK**.

--- /task ---

--- task ---

Make the `react-to-player`{:class="block3myblocks"} block either increase the points or increase the player’s lives, depending on the value of `type`{:class="block3myblocks"} .

```blocks3
+    define react-to-player (type)
+    if <(type ::variable) = [1]> then
        change [points v] by (collectable-value ::variables)
    end
+   if <(type ::variable) = [2]> then
        change [lives v] by [1]
    end
```

--- /task ---

--- task ---

Update the `when I start as a clone`{:class="block3events"} code to replace the block that adds a point with a **call** to `react-to-player`{:class="block3myblocks"}, **passing** `collectable-type`{:class="block3variables"}. By using this **My blocks** block, normal fart clouds still add a point, and the new power-up adds a life.

```blocks3
    if <touching [Player Character v] ?> then
+        react-to-player (collectable-type ::variables) :: custom
        delete this clone
    end
```

--- /task ---

### Using `collectable-type`{:class="block3variables"} to create different collectables at random

Right now, you might be wondering how you'll tell each collectable the game makes what type it should be.

You do this by setting the value of `collectable-type`{:class="block3variables"}. This variable is just a number. As you've seen, it's used to tell the `pick-costume`{:class="block3myblocks"} and `react-to-player`{:class="block3myblocks"} blocks what costume, rules, etc., to use for the collectable.

--- collapse ---
---
title: Working with variables in a clone
---

For each clone of the `Collectable` sprite, you can set a different value for `collectable-type`{:class="block3variables"}.

Think of it like creating a new copy of the `Collectable` sprite using the value that is stored in `collectable-type`{:class="block3variables"} at the time `Collectable` clone gets created.

One of the things that makes clones special is that they cannot change the values of any variables they start with. They effectively have **constant** values. That means that when you change the value of `collectable-type`{:class="block3variables"}, this doesn't affect the `Collectable` sprite clones that are already in the game.

--- /collapse ---

You're going to set the `collectable-type`{:class="block3variables"} to either `1` or `2` for each new clone that you make. Let's pick the number at random, to make a random collectable every time and keep things interesting.

--- task ---

Find the `repeat until`{:class="block3control"} loop inside the green flag code for the `Collectable` sprite, and add the `if...else`{:class="block3control"} code shown below.

```blocks3
    repeat until <not <(create-collectables ::variables) = [true]>>
+        if <[50] = (pick random (1) to (50))> then
            set [collectable-type v] to [2]
        else
            set [collectable-type v] to [1]
        end
        wait (collectable-frequency ::variables) secs
        go to x: (pick random (-240) to (240)) y: (-179)
        create clone of [myself v]
```

--- /task ---

This code gives a 1 in 50 chance of setting the `collectable-type`{:class="block3variables"} to `2`.

Great! Now you have a new type of collectable that sometimes shows up instead of the fart cloud, and that gives you an extra life instead of a point when you collect it!
