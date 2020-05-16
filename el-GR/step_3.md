## Χάνοντας το παιχνίδι

Μπορεί να έχεις παρατηρήσει ότι το μπλοκ `χάνει`{:class="block3myblocks"} από τις **Εντολές μου** στο αντικείμενο `Παίκτης` είναι άδειο. Θα το συμπληρώσεις και θα δημιουργήσεις όλα τα κομμάτια που χρειάζεται για να εμφανίζεται μια ωραία οθόνη «Τέλος Παιχνιδιού».

--- task ---

Αρχικά, βρες το μπλοκ `χάνει`{:class="block3myblocks"} και συμπλήρωσε τον ακόλουθο κώδικα:

```blocks3
    όρισε το χάνω
+ σταματα [άλλα σενάρια στο αντικείμενο v] :: στοίβα ελέγχου
+ μετάδοση [τέλος παιχνιδιού v]
+ μετάβαση στο x: (0) y: (0)
+ πες [Τέλος παιχνιδιού!] για (2) δευτερόλεπτα
+ πες [Είναι σχεδόν αδύνατο να συλλεγεί όλο το μεθάνιο, έτσι δεν είναι;] για (5) δευτερόλεπτα
+ πες [Θα ήταν καλύτερα να μειώθει το ποσό που παράγεται εξ αρχής.] για (6) δευτερόλεπτα
+ πες [Εξετάζοντας τις συνέπειες του τρόπου παραγωγής τροφίμων ...] για (5) δευτερόλεπτα
+ πες [... μπορούμε να το κάνουμε με πιο βιώσιμο τρόπο που είναι καλύτερο για όλους.] για (6) δευτερόλεπτα
+ σταματα [ όλα v]
```

--- /task ---

--- collapse ---
---
title: Τι κάνει ο κώδικας;
---

Όποτε εκτελείς το μπλοκ `χάνει`{:class="block3myblocks"}, αυτό που κάνει είναι:

 1. Σταματάει τη φυσική και άλλα σενάρια παιχνιδιού στο αντικείμενο `Παίκτης`
 2. Ενημερώνει όλα τα άλλα αντικείμενα ότι το παιχνίδι τελείωσε με **μετάδοση** ενός μηνύματος ώστε να μπορούν να αλλάξουν βάσει αυτού
 3. Μετακινεί τον `Παίκτη` στο κέντρο της οθόνης και του λέει ότι το παιχνίδι τελείωσε
 4. Σταματάει όλα τα σενάρια στο παιχνίδι

--- /collapse ---

Τώρα πρέπει να βεβαιωθείς ότι όλα τα αντικείμενα ξέρουν τι να κάνουν όταν τελειώσει το παιχνίδι και πώς να επαναφέρουν τον εαυτό τους όταν ο παίκτης ξεκινά ένα νέο παιχνίδι. **Μην ξεχνάς ότι τυχόν νέα αντικείμενα που προσθέτεις ενδέχεται να χρειάζονται επιπλέον κώδικα για αυτό!**

### Απόκρυψη των πλατφορμών και των ορίων

--- task ---

Ξεκίνα με τα βασικά: Τα αντικείμενα `Πλατφόρμες` και `Όρια` χρειάζονται και οι δύο κώδικα για να εμφανίζονται όταν ξεκινά το παιχνίδι και εξαφανίζεται στο «Τέλος παιχνιδού», οπότε πρόσθεσέ το παρακάτω σε καθένα από αυτά:

```blocks3
    όταν λαμβάνω [τέλος παιχνιδιού v]
    απόκρυψη
```

```blocks3
    όταν γίνει κλικ στην πράσινη σημαία
    εμφάνιση
```

--- /task ---

### Stopping the farts

Τώρα, κάτι λίγο πιο δύσκολο! If you look at the code for the `Collectable` sprite, you’ll see it works by **cloning** itself. That is, it makes copies of itself that follow the special `when I start as a clone`{:class="block3events"} instructions.

We’ll talk more about what makes clones special when we get to the card about making new and different collectables. For now, what you need to know is that clones can do **almost** everything a normal sprite can, including receiving `broadcast`{:class="block3events"} messages.

Look at how the `Collectable` sprite works. See if you can understand some of its code:

```blocks3
    when green flag clicked
    set size to (35) %
    hide
    set [collectable-value v] to [1]
    set [collectable-speed v] to [1]
    set [collectable-frequency v] to [1]
    set [create-collectables v] to [true]
    set [collectable-type v] to [1]
    repeat until <not <(create-collectables) = [true]>>
        wait (collectable-frequency) secs
        go to x: (pick random (-240) to (240)) y: (-179)
        create clone of [myself v]
    end
```

 1. First it makes the original collectable invisible.
 2. Then it sets up the control variables. We’ll come back to these later.
 3. The `create-collectables`{:class="block3variables"} variable is the on/off switch for cloning: the loop creates clones if `create-collectables`{:class="block3variables"} is `true`, and does nothing if it’s not.

--- task ---

Now set up a block on the `Collectable` sprite so that it reacts to the `game over` broadcast:

```blocks3
    when I receive [game over v]
    hide
    set [create-collectables v] to [false]
```

--- /task ---

This code is similar to the code controlling the `Edges` and `Platforms` sprites. The only difference is that you’re also setting the `create-collectables`{:class="block3variables"} variable to `false` so that no new clones are created when it's 'Game over'.

Note that you can use this variable to pass messages from one part of your code to another! 
