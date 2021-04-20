## Ajouter de la difficulté

Ton jeu fonctionne et tu peux maintenant accumuler des points, obtenir des pouvoirs spéciaux grâce à des power-ups, et perdre. Voilà qui est mieux ! Peut-être serait-il amusant d'ajouter un peu de difficulté — que dis-tu d'ajouter un personnage qui se déplace un peu, mais que tu n'es pas censé toucher ? Cela ressemblera aux ennemis des jeux de plateforme traditionnels comme Super Mario, dont nous nous sommes inspirés.

--- task ---

Tout d'abord, choisis un sprite à ajouter comme ennemi. Comme notre personnage est dans le ciel, j'ai choisi un hélicoptère. Il y a beaucoup d'autres sprites que tu pourrais ajouter. J'ai également renommé le sprite `Ennemi`, juste pour que les choses soient plus claires pour moi.

Redimensionne le sprite à la bonne taille et place-le à un endroit approprié pour commencer. Voici à quoi ressemble le mien :

![Le sprite de l'hélicoptère ennemi](images/enemySprite.png)

--- /task ---

--- task ---

Commence par écrire le code le plus simple : configure son bloc pour le message `partie terminée`{:class="block3events"} afin de faire disparaître l'ennemi lorsque le joueur perd la partie.

```blocks3
+    quand je reçois [partie terminée v]
+    cacher
```

--- /task ---

--- task ---

Maintenant, tu dois écrire le code pour ce que fait l'ennemi. Tu peux utiliser le mien de cette carte, mais n'aie pas peur d'en ajouter plus ! (Et s'ils se téléportent sur différentes plateformes ? Ou s'il y a un power-up qui les rend plus rapides ou plus lents ?)

```blocks3
+    quand le drapeau vert est cliqué
+    montrer
+    mettre [ennemi-déplacement-pas v] à [5]
+    fixer le sens de rotation [gauche-droite v]
+    aller à x: (1) y: (59)
+    répéter indéfiniment
        avancer de (ennemi-déplacement-pas) pas
        si <not <touching [Platforms v] ?>> alors
            mettre [ennemi-déplacement-pas v] to ((ennemi-déplacement-pas) * (-1))
        fin
    fin
```

**Note** : si tu fais juste glisser le bloc `aller à`{:class="block3motion"} et ne change pas les valeurs `x` et `y` , elles seront les valeurs de l'emplacement actuel du sprite !

--- /task ---

Le code dans le bloc `si...alors`{:class="block3control"} fera tourner l'ennemi quand il arrivera au bout de la plateforme.

La prochaine chose dont tu as besoin est que le joueur perd une vie quand il touche l’ennemi. Tu dois t'assurer qu'ils **stoppent** de se toucher très rapidement, car sinon le code de toucher continuera à fonctionner et ils continueront à perdre des vies.

--- task ---

Voici comment je l'ai fait, mais n'hésite pas à essayer d'améliorer ce code ! J’ai modifié le bloc principal du sprite `Personnage`. Ajoute le code avant le bloc `si`{:class="block3control"} qui vérifie si tu es à court de vie.

```blocks3
+    si <touching [Enemy v] ?> alors
        cacher
        aller à x: (-187) y: (42)
        ajouter (-1) à [vies v]
        attendre (0.5) secs
        montrer
    fin
```

--- /task ---

--- collapse ---
---
title: Montre-moi l'ensemble du script mis à jour
---

Le bloc principal de mon sprite `Personnage` ressemble maintenant à ceci :

```blocks3
    quand le drapeau vert est cliqué
    réinitialiser-jeu :: custom
    répéter indéfiniment
        physique-principal :: custom
        si <(ordonnée y) < [-179]> alors
            cacher
            réinitialiser-personnage :: custom
            ajouter (-1) à [vies v]
            attendre (0.05) secs
            montrer
        fin
        si <touching [Enemy v] ?> alors
            cacher
            aller à x: (-187) y: (42)
            ajouter (-1) à [vies v]
            attendre (0.5) secs
            montrer
        fin
        si <(vies) < [1]> alors
            perdre :: custom
        fin
    fin
```

--- /collapse ---

Le nouveau code masque le personnage, les remet à leur position de départ, réduit `vies`{:class="block3variables"} à `1`, et après une demi-seconde les fait réapparaître.
