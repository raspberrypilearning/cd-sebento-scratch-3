## Perdiendo el juego

Puede que hayas notado que el bloque `lose`{:class="block3myblocks"} **Mis bloques** en el objeto `Player Character` está vacío. Vas a rellenar esto y configurar todas las piezas necesarias para una bonita pantalla de 'Fin del juego'.

--- task ---

Primero, encuentra el bloque `lose`{:class="block3myblocks"} y complétalo con el siguiente código:

```blocks3
    definir lose
+ detener [otros scripts en objeto v] :: control stack
+ Enviar [fin del juego v]
+ ir a x:(0) y:(0)
+ decir [¡Fin del juego!] durante (2) segundos
+ decir [Es prácticamente imposible atrapar todo el metano, ¿verdad?] por (5) segundos
+ decir [Sería mejor reducir la cantidad producida en primer lugar.] por (6) segundos
+ decir [Considerando las consecuencias de cómo producimos los alimentos...] durante (5) segundos
+ decir [...podemos hacerlo de una manera más sostenible que sea mejor para todos.] durante (6) segundos
+ detener [todo v]
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
    al recibir [fin del juego v]
    ocultar
```

```blocks3
    al presionar la bandera verde
    mostrar
```

--- /task ---

### Deteniendo los gases

Ahora, ¡por algo un poco más complicado! Si miras el código del objeto `Collectable`, verás que funciona con **clonándose** a sí mismo. Es decir, hace copias de sí mismo que siguen las instrucciones especiales `cuando comienzo como un clon`{:class="block3events"}.

Hablaremos más sobre lo que hace que los clones sean especiales cuando lleguemos a la tarjeta sobre cómo hacer coleccionables nuevos y diferentes. Por ahora, lo que necesitas saber es que los clones pueden hacer **casi** todo lo que un sprite normal puede, incluyendo recibir mensajes `enviados`{:class="block3events"}.

Mira cómo funciona el objeto `Collectable`. Comprueba si puedes entender algo de su código:

```blocks3
    al presionar bandera verde
    fijar tamaño a (35) %
    ocultar
    establecer [collectable-value v] a [1]
    establecer [collectable-speed v] a [1]
    establecer [collectable-frequency v] a [1]
    establecer [create-collectables v] a [true]
    establecer [collectable-type v] a [1]
    repetir hasta que <not <(create-collectables) = [true]>>
        esperar (collectable-frequency) segundos
        ir a x: (número al azar entre (-240) y (240)) y: (-179)
        crear clon de [mí mismo v]
    Fin
```

 1. Primero, hace que el coleccionable original sea invisible.
 2. Luego configura las variables de control. Volveremos a esto más tarde.
 3. La variable `create-collectables`{:class="block3variables"} es el interruptor de encendido/apagado para la clonación: el ciclo crea clones si `create-collectables`{:class="block3variables"} es `verdadero`, y no hace nada si no lo es.

--- task ---

Ahora configura un bloque en el objeto `Collectable` para que reaccione al mensaje `fin del juego`:

```blocks3
    al recibir [fin del juego v]
    ocultar
    fijar [create-collectables v] a [false]
```

--- /task ---

Este código es similar al código que controla los objetos `Edges` y `Platforms`. La única diferencia es que también estás configurando la variable `create-collectables`{:class="block3variables"} a `false` para que no se creen nuevos clones cuando termine el juego.

¡Ten en cuenta que puedes usar esta variable para pasar mensajes de una parte de tu código a otra! 
