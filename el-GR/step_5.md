## Σούπερ Βραβεία!

Τώρα που έχεις νέα βραβεία, ήρθε η ώρα να το κάνεις κάτι άψογο! Ας το κάνουμε να «βρέχει» βραβεία για λίγα δευτερόλεπτα, αντί να δώσουμε μια επιπλέον ζωή.

Για αυτό πρέπει να δημιουργήσεις ένα άλλο κομμάτι κώδικα που θα ξεκινήσει ενώ το μπλοκ `αντίδραση-στον-παίκτη`{:class="block3myblocks"} ολοκληρώσει την εκτέλεσή του. Για να συμβεί αυτό, θα χρησιμοποιήσεις ένα μπλοκ `εκπομπής`{:class="block3events"} για να στείλεις ένα μήνυμα σε άλλο κομμάτι κώδικα μέσα σε αυτό το αντικείμενο.

--- task ---

Δημιούργησε αυτό το μπλοκ για το αντικείμενο `Βραβείο`. Ας ονομάσουμε την εκπομπή `βροχή-βραβείων`{:class="block3events"}.

```blocks3
+ when I receive [βροχή-βραβείων v]
+ όρισε [συχνότητα-βραβείου v] σε [0.000001]
+ wait (1) secs
+ όρισε [συχνότητα-βραβείου v] σε [1]
```

--- /task ---

--- collapse ---
---
title: Τι κάνει ο νέος κώδικας;
---

Αυτό το μπλοκ ορίζει απλώς τη `συχνότητα-βραβείου`{:class="block3variables"} σε πολύ μικρό αριθμό \(άλλαξέ το σε διαφορετικές τιμές και δες τι συμβαίνει!\) και στη συνέχεια περιμένει ένα δευτερόλεπτο και το αλλάζει ξανά σε `1`.

Αυτό δεν φαίνεται ότι πρέπει να κάνει πολλά, αλλά σκέψου τι συμβαίνει κατά τη διάρκεια αυτού του δευτερολέπτου: το `όταν γίνεται κλικ στην πράσινη σημαία`{:class="block3events"} ο κώδικας εξακολουθεί να εκτελείται και ο βρόγχος `επαναλαμβάνεται μέχρι`{:class="block3control"} μέσα σε αυτό, επαναλαμβάνεται. Κοίταξε τον κώδικα σε αυτόν τον βρόχο:

```blocks3
    repeat until <not <(δημιουργία-βραβείου ::variables) = [true]>>
        if < [50] = (pick random (1) to (50))> then
            όρισε [τύπος-βραβείου v] σε [2]
        αλλιώς
            όρισε [τύπος-βραβείου v] σε [1]
        τέλος
        περίμενε (συχνότητα-βραβείου ::variables) δευτερόλεπτα
        go to x: (pick random (-240) to (240)) y:(-179)
        create clone of [myself v]
    τέλος
```

Μπορείς να δεις ότι το μπλοκ `περίμενε` εδώ κάνει παύση τον κώδικα για το χρονικό διάστημα που ορίζεται από τη `συχνότητα-βραβείου`{:class="block3variables"}. Εάν η τιμή `συχνότητα-βραβείου`{:class="block3variables"} αλλάξει σε `0.000001`, το μπλοκ `περίμενε` κάνει παύση μόνο για **ένα εκατομμυριοστό** του δευτερολέπτου, που σημαίνει ότι ο βρόχος θα εκτελεστείται πολύ περισσότερες φορές από το κανονικό. Ως αποτέλεσμα, ο κώδικας πρόκειται να δημιουργήσει **πολλά** περισσότερα βραβεία από ό,τι συνήθως, μέχρι η `συχνότητα-βραβείου`{:class="block3variables"} να γίνει πάλι `1`. Μπορείς να σκεφτείς τυχόν προβλήματα που μπορεί αυτό να προκαλέσει; Θα υπάρξουν πολύ περισσότερες σούπερ-πορδές… τι θα συνέβαινε εάν συνέχιζες να τις πιάνεις;

--- /collapse ---

Τώρα έχεις το αντικείμενο έτοιμο να δεχτεί το μπλόκ μηνύματος `βροχή-βραβείων`{:class="block3events"}, αλλά δεν έχεις δημιουργήσει ακόμα τον κώδικα για την αποστολή του μηνύματος.

--- task ---

Στη συνέχεια, ενημέρωσε το μπλοκ `αντίδραση-στον-παίκτη`{:class="block3myblocks"} ώστε να μοιάζει έτσι, ώστε να μεταδίδει το μήνυμα `βροχή-βραβείων`{:class="block3events"} όταν ο παίκτης αγγίζει τον τύπο `2` του βραβείου.

```blocks3
    define αντίδραση-στον-παίκτη (τύπος)
    εάν <(τύπος ::variable) = [1]> τότε
        άλλαξε [πόντους v] κατά (τιμή-βραβείου ::variables)
    τέλος
    εάν <(τύπος ::variable) = [2]> τότε
+        broadcast [βροχή-βραβείου v]
    τέλος
```

--- /task ---
