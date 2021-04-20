## Perdre la partie

Tu as peut-être remarqué que le bloc `perdre`{:class="block3myblocks"} **Mes blocs** sur le sprite `Personnage` est vide. Tu vas remplir ceci et mettre en place toutes les pièces nécessaires pour un bel écran « Partie terminée ».

--- task ---

Tout d'abord, trouve le bloc `perdre`{:class="block3myblocks"} et complète-le avec le code suivant :

```blocks3
    définir perdre
+    stop [autres scripts dans sprite v] :: control stack
+    envoyer à tous [partie terminée v]
+    aller à x:(0) y:(0)
+    dire [Partie terminée !] pendant (2) secs
+    dire [C'est quasi impossible d'attraper tout le méthane, non ?] pendant (5) secs
+    dire [Ce serait mieux de réduire la quantité produite en premier.] pendant (6) secs
+    dire [En prenant en compte les conséquences de notre mode de production alimentaire...] pendant (5) secs
+    dire [...nous pouvons le faire d'une manière plus durable et meilleure pour tous..] pendant (6) secs
+    stop [tout v]
```

--- /task ---

--- collapse ---
---
title: Que fait le code ?
---

À chaque fois que le bloc `perdre`{:class="block3myblocks"} s'exécute, il :

 1. Arrête la physique et les autres scripts du jeu sur le `personnage`
 2. Indique à tous les autres sprites que le jeu est terminé en **diffusant** un message afin qu'ils puissent se modifier en fonction de çà
 3. Déplace le `personnage` au centre de l'écran et dit au joueur que la partie est terminée
 4. Arrête tous les scripts du jeu

--- /collapse ---

Maintenant tu dois t'assurer que tous les sprites savent quoi faire quand le jeu est terminé, et comment se réinitialiser quand le joueur démarre une nouvelle partie. **N'oublie pas que tous les nouveaux sprites que tu ajoutes pourraient également avoir besoin de code pour cela !**

### Cacher les plateformes et les bords

--- task ---

Commencer par les bases : Les sprites `Plateformes` et `Bords` ont tous deux besoin de code pour apparaître lorsque le jeu commence et disparaître à « Partie terminée », donc ajoute ceci à chacun d'eux :

```blocks3
    quand je reçois [partie terminée v]
    cacher
```

```blocks3
    quand le drapeau vert est cliqué
    montrer
```

--- /task ---

### Arrêter les pets

Maintenant, pour quelque chose d'un peu plus délicat ! Si tu regardes le code du sprite `Collectable` , tu verras qu'il fonctionne en se **clonant**. C'est-à-dire qu'il fait des copies de lui-même qui suivent les instructions spéciales `quand je commence comme un clone`{:class="block3events"} .

Nous reviendrons sur ce qui fait la spécificité des clones lorsque nous aborderons la carte relative à la création de collectables nouveau et différent. Pour l'instant, ce que tu dois savoir, c'est que les clones peuvent faire **presque** tout ce qu'un sprite normal peut faire, y compris les messages `envoyer à tous`{:class="block3events"}.

Regarde comment le sprite `Collectable` fonctionne. Regarde si tu peux comprendre une partie de son code :

```blocks3
    quand le drapeau vert est cliqué
    mettre la taille à (35) % de la taille initiale
    cacher
    mettre [collectable-valeur v] à [1]
    mettre [collectable-vitesse v] à [1]
    mettre [collectable-fréquence v] à [1]
    mettre [créer-collectables v] à [true]
    mettre [collectable-type v] à [1]
    répéter jusqu'à ce que <not <(create-collectables) = [true]>>
        attendre (collectable-fréquence) secs
        aller à x: (nombre aléatoire entre (-240) et (240)) y: (-179)
        créer un clone de [moi-même v]
    fin
```

 1. Tout d'abord, il rend invisible le collectable original.
 2. Puis il met en place les variables de contrôle. Nous y reviendrons plus tard.
 3. La variable `créer-collectables`{:class="block3variables"} est le commutateur marche/arrêt pour le clonage : la boucle crée des clones si `créer-collectables`{:class="block3variables"} est `vraie`, et ne fait rien si ce n’est pas le cas.

--- task ---

Maintenant, configure un bloc sur le sprite `Collectable` pour qu'il réagisse au message `partie terminée` :

```blocks3
    quand je reçois [partie terminée v]
    cacher
    mettre [créer-collectables v] à [faux]
```

--- /task ---

Ce code est similaire au code qui contrôle les sprites `Bords` et `Plateformes`. La seule différence est que tu définis également la variable `créer-collectables`{:class="block3variables"} à `faux` afin qu'aucun nouveau clone ne soit créé lorsque c'est « Partie terminée ».

Note que tu peux utiliser cette variable pour passer des messages d'une partie de ton code à une autre ! 
