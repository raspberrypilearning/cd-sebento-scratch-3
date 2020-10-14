## Nivel 2

Con este paso, agregarás un nuevo nivel al juego al que el jugador puede acceder simplemente presionando un botón. Más tarde, puedes cambiar tu código para hacerlo de modo que necesiten una cierta cantidad de puntos, o algo más, para llegar allí.

### Pasando al siguiente nivel

--- task ---

Primero, crea un nuevo objeto como un botón, ya sea agregando uno de la biblioteca o dibujando el tuyo. Hice un poco de ambas cosas y se me ocurrió esto:

![El objeto botón para cambiar niveles](images/levelButton.png)

--- /task ---

--- task ---

Ahora, el código para este botón es inteligente: está diseñado para que cada vez que hagas clic en él te lleve al siguiente nivel, sin importar cuántos niveles haya.

Añade estos scripts a tu objeto **Botón**. Necesitarás crear algunas variables mientras lo haces.

```blocks3
+    when green flag clicked
+    set [max-level v] to [2]
+    set [min-level v] to [1]
+    set [current-level v] to [1]
```

```blocks3
+    when this sprite clicked
+    change [current-level v] by (1)
+    if <(current-level) > (max-level ::variables)> then
        set [current-level v] to (min-level ::variables)
    end
+    broadcast [collectable-cleanup v]
+    broadcast (join [level-](current-level))
```

--- /task ---

¿Puedes ver cómo el programa usará las variables que creaste?

+ `max-level`{:class="block3variables"} almacena el nivel más alto
+ `min-level`{:class="block3variables"} almacena el nivel más bajo
+ `current-level`{:class="block3variables"} almacena el nivel en el que se encuentra el jugador en este momento

Todos estos deben ser configurados por el programador \(tu!\), de forma que si agregas un tercer nivel, ¡no te olvides de cambiar el valor de `max-level`{:class="block3variables"}! `min-level`{:class="block3variables"} nunca tendrá que cambiar, por supuesto.

Los mensajes se utilizan para indicar a los otros objetos qué nivel mostrar y para hacer desaparecer los coleccionables cuando un nuevo nivel empieza.

### Haz que los objetos reaccionen

#### El objeto **Collectable**

¡Ahora necesitas que los otros objetos respondan a estos mensajes! Comienza con el más fácil: limpiando todos los coleccionables.

--- task ---

Agrega el siguiente código al código de scripts **Collectable** para decirle a todos sus clones que se `oculten`{:class="block3vlooks"} cuando reciban el mensaje de limpieza:

```blocks3
+    when I receive [collectable-cleanup v]
+    hide
```

--- /task ---

Dado que una de las primeras cosas que hace cualquier clon nuevo es mostrarse, ¡no tienes que preocuparte por mostrar los coleccionables!

#### El objeto **Platforms**

Ahora para cambiar el objeto **Platforms**. Puedes diseñar tu propio nuevo nivel más tarde si lo deseas, pero por ahora usemos el que ya he incluido — ¡verás por qué en el siguiente paso!

--- task ---

Añade este código al objeto **Platforms**:

```blocks3
+    when I receive [level-1 v]
+    switch costume to [Level 1 v]
+    show
```

```blocks3
+    when I receive [level-2 v]
+    switch costume to [Level 2 v]
+    show
```

--- /task ---

Este recibe los mensajes `unidos`{:class="block3operators"} de `level-`{:class="block3variables"} y `current-level`{:class="block3variables"} que el objeto **Botón** envía, y responde cambiando el disfraz de **Platforms**.

#### El objeto **Enemy**

--- task ---

En el script del objeto **Enemy**, solo asegúrate de que el objeto desaparezca cuando el jugador ingrese al nivel 2, así:

```blocks3
+    when I receive [level-1 v]
+    show
```

```blocks3
+    when I receive [level-2 v]
+    hide
```

--- /task ---

Si lo prefieres, puedes hacer que el enemigo se mueva a otra plataforma. En ese caso, usarías un bloque de `ir a`{:class="block3motion"} en lugar de los bloques `mostar`{:class="block3looks"} y `ocultar`{:class="block3looks"}.

### Hacer el personaje del jugador **Player Character** aparecer en el lugar correcto

Cada vez que comienza un nuevo nivel, el objeto de **Player Character** necesita ir al lugar correcto para ese nivel. Para que esto ocurra, necesitas cambiar dónde el objeto obtiene sus coordenadas desde el momento en que aparece por primera vez en el escenario. Por el momento, hay valores fijos `x` y `y` en su código.

--- task ---

Comienza creando variables para las coordenadas iniciales: `start-x`{:class="block3variables"} y `start-y`{:class="block3variables"}. Luego, conéctalos al bloque `ir a`{:class="block3motion"} en el bloque de `reset-character`{:class="block3myblocks"} en **Mis bloques**, en lugar de los valores fijos de `x` y `y`:

```blocks3
    define reset-character
    set [can-jump v] to [true]
    set [x-velocity v] to [0]
    set [y-velocity v] to [-0]
+    go to x: (start-x) y: (start-y)
```

--- /task ---

--- task ---

Luego, para cada mensaje que anuncie el inicio de un nivel, configura las coordenadas `start-x`{:class="block3variables"} y `start-y`{:class="block3variables"} en respuesta y agrega una **llamada** para `reset-character`{:class="block3myblocks"}:

```blocks3
+    when I receive [level-1 v]
+    set [start-x v] to [-183]
+    set [start-y v] to [42]
+    reset-character :: custom
```

```blocks3
+    when I receive [level-2 v]
+    set [start-x v] to [-218]
+    set [start-y v] to [-143]
+    reset-character :: custom
```

--- /task ---

### Iniciar en el Nivel 1

También debes asegurarte de que cada vez que alguien comienza el juego, el primer nivel que juegue es el nivel 1.

--- task ---

Ve al script `reset-game`{:class="block3myblocks"} y elimina la llamada para `reset-character`{:class="block3myblocks"} de este. En su lugar, emite el `min-level`{:class="block3variables"}. El código que ya has agregado con esta tarjeta configurará las coordenadas iniciales correctas para el objeto **Player Character**, y también llamará a `reset-character`{:class="block3myblocks"}.

```blocks3
    define reset-game
    set rotation style [left-right v]
    set [jump-height v] to [15]
    set [gravity v] to [2]
    set [x-speed v] to [1]
    set [y-speed v] to [1]
    set [lives v] to [3]
    set [points v] to [0]
+    broadcast (join [level-](min-level ::variables))
```

--- /task ---

--- collapse ---
---
title: Reiniciar el personaje del jugador versus reiniciar el juego
---

Nota que el primer bloque en el código de bandera verde del objeto **Player Character** es una llamada al bloque `reset-game`{:class="block3myblocks"} de **Mis bloques**.

Este bloque configura todas las variables para un nuevo juego y luego llama al bloque de `reset-character`{:class="block3myblocks"} **Mis bloques**, que vuelve a colocar al personaje en su posición inicial correcta.

Tener el código `reset-character`{:class="block3myblocks"} en su propio bloque separado de `reset-game`{:class="block3myblocks"}, te permite reiniciar el personaje a diferentes posiciones **sin** tener que reiniciar todo el juego.

--- /collapse ---
