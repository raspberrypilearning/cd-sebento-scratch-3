## Perdiendo el juego

Puede que hayas notado que el bloque `lose`{:class="block3myblocks"} **Mis bloques** en el objeto `Player Character` está vacío. Vas a rellenar esto y configurar todas las piezas necesarias para una bonita pantalla de 'Fin del juego'.

--- task ---

Primero, encuentra el bloque `lose`{:class="block3myblocks"} y complétalo con el siguiente código:

```blocks3
 define lose
+    stop [other scripts in sprite v] :: control stack
+    broadcast [game over v]
+    go to x:(0) y:(0)
+    say [Game over!] for (2) secs
+    say [It's pretty much impossible to catch all the methane, right?] for (5) secs
+    say [It would be better to reduce the amount produced in the first place.] for (6) secs
+    say [By considering the consequences of how we produce food...] for (5) secs
+    say [...we can do it in a more sustainable way that's better for everyone.] for (6) secs
+    stop [all v]
```

--- /task ---

--- collapse ---
---
title: ¿Qué hace el código?
---

Siempre que el bloque `lose`{:class="block3myblocks"} se ejecuta, lo que hace es:

 1. Detener la física y otros scripts de juego en el `Player Character`
 2. Decirle a todos los demás sprites que el juego ha terminado **enviando** un mensaje para que puedan cambiar en base a este
 3. Mover al `Player Character` al centro de la pantalla y hace que le diga al jugador que el juego ha terminado
 4. Detener todos los scripts en el juego

--- /collapse ---

Ahora necesitas asegurarte de que todos los sprites sepan qué hacer cuando el juego haya terminado, y cómo restaurarse a sí mismos cuando el jugador inicie una nueva partida. **¡No olvides que cualquier objeto nuevo que añadas también puede necesitar un código para esto!**

### Ocultar las plataformas y bordes

--- task ---

Comienza con lo básico: los objetos `Platforms` y `Edges` necesitan código para aparecer cuando el juego comienza y desaparecer cuando sea 'Fin del juego', así que añade esto a cada uno de ellos:

```blocks3
    when I receive [fin del juego  v]
    hide
```

```blocks3
    when green flag clicked
    show
```

--- /task ---

### Deteniendo los gases

Ahora, ¡por algo un poco más complicado! Si miras el código del objeto `Collectable`, verás que funciona con **clonándose** a sí mismo. Es decir, hace copias de sí mismo que siguen las instrucciones especiales `cuando comienzo como un clon`{:class="block3events"}.

Hablaremos más sobre lo que hace que los clones sean especiales cuando lleguemos a la tarjeta sobre cómo hacer coleccionables nuevos y diferentes. Por ahora, lo que necesitas saber es que los clones pueden hacer **casi** todo lo que un sprite normal puede, incluyendo recibir mensajes `enviados`{:class="block3events"}.

Mira cómo funciona el objeto `Collectable`. Comprueba si puedes entender algo de su código:

```blocks3
    when green flag clicked
    set size to (35) %
    hide
    set [collectable-value v] to [1]
    set [collectable-speed v] to [1]
    set [collectable-frequency v] to [1]
    set [create-collectables v] to [true]
    set [collectable-type v] to [1]
    repeat until &lt;not &lt;(create-collectables) = [true]&gt;&gt;
        wait (collectable-frequency) secs
        go to x: (pick random (-240) to (240)) y: (-179)
        create clone of [myself v]
    end
```

 1. Primero, hace que el coleccionable original sea invisible.
 2. Luego configura las variables de control. Volveremos a esto más tarde.
 3. La variable `create-collectables`{:class="block3variables"} es el interruptor de encendido/apagado para la clonación: el ciclo crea clones si `create-collectables`{:class="block3variables"} es `verdadero`, y no hace nada si no lo es.

--- task ---

Ahora configura un bloque en el objeto `Collectable` para que reaccione al mensaje `fin del juego`:

```blocks3
    when I receive [fin del juego v]
    hide
    set [create-collectables v] to [false]
```

--- /task ---

Este código es similar al código que controla los objetos `Edges` y `Platforms`. La única diferencia es que también estás configurando la variable `create-collectables`{:class="block3variables"} a `false` para que no se creen nuevos clones cuando termine el juego.

¡Ten en cuenta que puedes usar esta variable para pasar mensajes de una parte de tu código a otra! 
