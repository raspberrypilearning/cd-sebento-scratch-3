## Niveau 2

Avec cette étape, tu vas ajouter un nouveau niveau au jeu auquel le joueur peut accéder en appuyant simplement sur un bouton. Plus tard, tu pourras modifier ton code pour qu’ils aient besoin d’un certain nombre de points ou de quelque chose d’autre pour y parvenir.

### Passer au niveau suivant

--- task ---

Tout d'abord, crée un nouveau sprite en tant que bouton en l'ajoutant à partir de la bibliothèque ou en le dessinant. J'ai fait un peu des deux et je suis arrivé à ceci :

![Le sprite bouton pour changer de niveau](images/levelButton.png)

--- /task ---

--- task ---

Le code de ce bouton est astucieux : il est conçu de telle sorte que chaque fois que tu cliques dessus, tu accèdes au niveau suivant, quel que soit le nombre de niveaux.

Ajoute ces scripts à ton sprite **Bouton**. Pour ce faire, tu devras créer quelques variables.

```blocks3
+    quand ce drapeau est cliqué
+    mettre [niveau-max v] à [2]
+    mettre [niveau-min v] à [1]
+    mettre [niveau-actuel v] à [1]
```

```blocks3
+    quand ce sprite est cliqué
+    ajouter (1) à [niveau-actuel v]
+    si <(niveau-actuel) > (niveau-max ::variables)> alors
        mettre [niveau-actuel v] à (niveau-max ::variables)
    fin
+    envoyer à tous [collectable-nettoyage v]
+    envoyer à tous (regroupe [niveau-](niveau-actuel))
```

--- /task ---

Peux-tu voir comment le programme utilisera les variables que tu as créées ?

+ `niveau-max`{:class="block3variables"} enregistre le niveau le plus élevé
+ `niveau-min`{:class="block3variables"} enregistre le niveau le plus bas
+ `niveau-actuel`{:class="block3variables"} enregistre le niveau actuel du joueur

Tous ces éléments doivent être définis par le programmeur (toi !), donc si tu ajoutes un troisième niveau, n'oublie pas de changer la valeur de `niveau-max`{:class="block3variables"} ! `niveau-min`{:class="block3variables"} n'aura jamais besoin de changer, bien sûr.

Les diffusions sont utilisées pour indiquer aux autres sprites le niveau à afficher et pour nettoyer les collectables lorsqu'un nouveau niveau commence.

### Faire réagir les sprites

#### Le sprite **Collectable**

Maintenant tu dois amener les autres sprites à répondre à ces diffusions ! Commence par le plus simple : efface tous les collectables.

--- task ---

Ajoute le code suivant aux scripts de sprite **Collectable** pour dire à tous ses clones de `cacher`{:class="block3vlooks"} quand ils reçoivent le message de nettoyage :

```blocks3
+    quand je reçois [collectable-nettoyage v]
+    cacher
```

--- /task ---

Comme l'une des premières choses que fait tout nouveau clone est de se montrer, tu n'as pas à te soucier de dissimuler des collectables !

#### Le sprite **Plateforme**

Maintenant, pour basculer le sprite **Plateformes**. Tu pourras concevoir ton propre nouveau niveau plus tard si tu le souhaites, mais pour l’instant, nous allons utiliser celui que j’ai déjà inclus — tu verras pourquoi à la prochaine étape !

--- task ---

Ajoute ce code au sprite **Plateformes** :

```blocks3
+    quand je reçois [niveau-1 v]
+    basculer sur le costume [Niveau 1 v]
+    montrer
```

```blocks3
+    quand je reçois [niveau-2 v]
+    basculer sur le costume [Niveau 2 v]
+    montrer
```

--- /task ---

Il reçoit les messages `regroupés`{:class="block3operators"} de `niveau-`{:class="block3variables"} et `niveau-actuel`{:class="block3variables"} que le sprite **Bouton** envoie et répond en changeant le costume **Plateformes**.

#### Le sprite **Ennemi**

--- task ---

Dans les scripts de sprite **Ennemi**, assure-toi juste que le sprite disparaît lorsque le joueur entre dans le niveau 2, comme ceci :

```blocks3
+    quand je reçois [niveau-1 v]
+    montrer
```

```blocks3
+    quand je reçois [niveau-2 v]
+    cacher
```

--- /task ---

Si tu préfères, tu peux plutôt faire bouger l'ennemi vers une autre plateforme. Dans ce cas, tu utiliserais un bloc `aller à`{:class="block3motion"} au lieu des blocs `montrer`{:class="block3looks"} et `cacher`{:class="block3looks"}.

### Faire en sorte que le **Personnage** apparaît au bon endroit

À chaque fois qu'un nouveau niveau commence, le sprite **Personnage** doit aller au bon endroit pour ce niveau. Pour que cela se produise, tu dois changer d'où le sprite obtient ses coordonnées lorsqu'il apparaît pour la première fois sur la scène. Pour l'instant, il y a des valeurs `x` et `y` fixes dans son code.

--- task ---

Commence par créer des variables pour les coordonnées de départ : `x-départ`{:class="block3variables"} et `y-départ`{:class="block3variables"}. Puis mets-les dans le bloc `aller à`{:class="block3motion"} dans le bloc `réinitialiser-personnage`{:class="block3myblocks"} **Mes blocs** au lieu des valeurs `x` et `y` fixes :

```blocks3
    définir réinitialiser-personnage
    mettre [peut-sauter v] à [vrai]
    mettre [vélocité-x v] à [0]
    mettre [vélocité-y v] to [-0]
+    aller x: (x-départ) y: (y-départ)
```

--- /task ---

--- task ---

Ensuite, pour chaque message annonçant le début d'un niveau, définis les coordonnées `x-départ`{:class="block3variables"} et `y-départ`{:class="block3variables"} dans la réponse, et ajoute un **appel** à `réinitialiser-personnage`{:class="block3myblocks"} :

```blocks3
+    quand je reçois [niveau-1 v]
+    mettre [x-départ v] à [-183]
+    mettre [y-départ v] à [42]
+    réinitialiser-personnage :: custom
```

```blocks3
+    quand je reçois [niveau-2 v]
+    mettre [x-départ v] à [-218]
+    mettre [y-départ v] à [-143]
+    réinitialiser-personnage :: custom
```

--- /task ---

### Démarrer au Niveau 1

Tu dois également t'assurer que chaque fois que quelqu'un commence le jeu, le premier niveau qu'il joue est le niveau 1.

--- task ---

Va au script `réinitialiser-jeu`{:class="block3myblocks"} et retire l'appel de `réinitialiser-personnage`{:class="block3myblocks"}. À sa place, mets le message `niveau-min`{:class="block3variables"}. Le code que tu as déjà ajouté avec cette carte va alors configurer les coordonnées de départ correctes pour le sprite **Personnage** et appeler `réinitialiser-personnage`{:class="block3myblocks"}.

```blocks3
    définir réinitialiser-jeu
    fixer le sens de rotation [gauche-droite v]
    mettre [saut-hauteur v] à [15]
    mettre [gravité v] à [2]
    mettre [vitesse-x v] à [1]
    mettre [vitesse-y à [1]
    mettre [vies v] à [3]
    mettre [points v] à [0]
+    envoyer à tous (regrouper [niveau-](niveau-min ::variables))
```

--- /task ---

--- collapse ---
---
title: Réinitialisation du personnage par rapport à la réinitialisation du jeu
---

Note que le premier bloc du drapeau vert principal du sprite **Personnage** est un appel au bloc `réinitialiser-jeu`{:class="block3myblocks"} **Mes blocs**.

Ce bloc configure toutes les variables pour un nouveau jeu, et appelle le bloc `réinitialiser-personnage`{:class="block3myblocks"} **Mes blocs**, qui replace le personnage dans sa position de départ correcte.

Avoir le code `réinitialiser-personnage`{:class="block3myblocks"} dans son propre bloc, séparé de `réinitialiser-jeu`{:class="block3myblocks"} te permet de réinitialiser le personnage à différentes positions **sans** réinitialiser l'ensemble du jeu.

--- /collapse ---
