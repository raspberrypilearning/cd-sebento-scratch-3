## Super power-ups !

Maintenant que tu as un nouveau power-up qui fonctionne, il est temps de faire quelque chose de cool ! Faisons en sorte qu'il pleuve des power-ups pendant quelques secondes, au lieu de simplement donner une vie supplémentaire.

Pour cela, tu dois créer un autre morceau de code qui démarrera pendant que le bloc `réagir-au-joueur`{:class="block3myblocks"} se termine. Pour faire en sorte que cela arrive, tu utiliseras un bloc `envoyer à tous`{:class="block3events"} pour envoyer un message à un autre bout de code à l'intérieur de ce sprite.

--- task ---

Crée ce bloc pour le sprite `Collectable`. Appelons la diffusion `collectable-pluie`{:class="block3events"}.

```blocks3
+    quand je reçois [collectable-pluie v]
+    mettre [collectable-fréquence v] à [0.000001]
+    attendre (1) secs
+    mettre [collectable-fréquence v] à [1]
```

--- /task ---

--- collapse ---
---
title: Que fait le nouveau code ?
---

Ce bloc définit simplement `collectable-fréquence`{:class="block3variables"} à un très petit nombre (change-le en différentes valeurs et vois ce qui se passe !) puis attend une seconde et le remet à `1`.

Cela ne semble pas devoir faire grand-chose. mais pense à ce qui se passe au cours de cette seconde : le code `quand le drapeau vert est cliqué`{:class="block3events"} est toujours en cours d'exécution, et la boucle `répéter jusqu'à ce que`{:class="block3control"} qu'il contient tourne en boucle. Regarde le code dans cette boucle :

```blocks3
    répéter jusqu'à ce que <not <(create-collectables ::variables) = [true]>>
        si < [50] = (nombre aléatoire entre (1) et (50))> alors
            mettre [collectable-type  v] à [2]
        sinon
            mettre [collectable-type v] à [1]
        fin
        attendre (collectable-fréquence :: variables) secondes
        aller à x: (nombre aléatoire entre (-240) et (240)) y: (-179)
        créer un clone de [moi-même v]
    fin
```

Tu peux voir que le bloc `attendre`{:class="block3control"} met ici en pause le code pour la durée définie par `collectable-fréquence`{:class="block3variables"}. Donc si la valeur de `collectable-fréquence`{:class="block3variables"} change à `0.000001`, le bloc `attendre` ne met en pause qu'**un millionième** de seconde, ce qui signifie que la boucle s'exécutera beaucoup plus de fois que la normale. Par conséquent, le code va créer **beaucoup** plus de power-ups qu'il ne le ferait normalement jusqu'à ce que `collectable-fréquence`{:class="block3variables"} soit remis à `1`. Peux-tu penser aux problèmes que cela pourraient causer ? Il y aura beaucoup plus de super pets…et si tu continuais à les attraper ?

--- /collapse ---

Maintenant tu as le sprite prêt à recevoir le bloc de diffusion `collectable-pluie`{:class="block3events"}, mais tu n'as pas encore fait de code pour l'envoi de la diffusion.

--- task ---

Ensuite, mets à jour le bloc `réagir-au-joueur`{:class="block3myblocks"} pour ressembler à ceci, donc il diffuse `collectable-pluie`{:class="block3events"} quand le joueur touche un power-up de type `2`.

```blocks3
    définir réagir-au-joueur (type)
    si <(type :: variable) = [1]> alors
        ajouter (collectable-valeur :: variables) à [points v]
    fin
    si <(type :: variable) = [2]> alors
+        envoyer à tous [collectable-pluie v]
    fin
```

--- /task ---
