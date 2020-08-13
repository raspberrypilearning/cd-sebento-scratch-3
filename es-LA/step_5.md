## ¡Súper recompensas!

Ahora que tienes una nueva recompensa, ¡es el momento de hacer que haga algo genial! Hagamos que ¨lluevan¨ recompensas durante unos segundos, en lugar de sólo dar una vida extra.

Para esto, necesitas crear otra pieza de código que comenzará mientras el bloque `react-to-player`{:class="block3myblocks"} termina de ejecutarse. Para que eso suceda, utilizarás un bloque `enviar`{:class="block3events"} para enviar un mensaje a otra pieza de código dentro de este objeto.

--- task ---

Crea este bloque para el objeto `Collectable`. Llamemos al mensaje `lluvia de coleccionables`{:class="block3events"}.

```blocks3
+ al recibir [lluvia de coleccionables v]
+ establecer [recolectable-frequency v] a [0.000001]
+ esperar (1) segundos
+ establecer [recolectable-frequency v] a [1]
```

--- /task ---

--- collapse ---
---
title: ¿Qué hace el nuevo bloque?
---

Este bloque solo establece `collectable-frequency`{:class="block3variables"} a un número muy pequeño \(¡cámbialo a diferentes valores y mira qué pasa!) y luego espera un segundo y lo vuelve a cambiar a `1`.

Esto no parece que hiciera mucho, pero piensa en lo que está sucediendo durante ese segundo: el código `al presionar la bandera verde`{:class="block3events"} sigue funcionando, y el ciclo `repetir hasta que`{:class="block3control"} dentro de él está repitiéndose. Mira el código en ese ciclo:

```blocks3
    repetir hasta que <not <(create-collectables ::variables) = [true]>>
        si < [50] = (número al azar entre (1) y (50))> entonces
            establecer [collectable-type v] a [2]
        si no
            establecer [collectable-type v] a [1]
        fin
        esperar (collectable-frequency ::variables) segundos
        ir a x: (número al azar entre (-240) y (240)) y:(-179)
        crear clon de [mí mismo v]
    fin
```

Puedes ver que el bloque de `esperar` aquí detiene el código durante el tiempo establecido por `collectable-frequency`{:class="block3variables"}. Entonces, si el valor de `collectable-frequency`{:class="block3variables"} cambia a `0.000001`, el bloque `esperar`{:class="block3control"} solo se detiene por **una millonésima** de segundo; lo que significa que el ciclo se ejecutará muchas más veces de lo normal. Como resultado, el código creará **muchas** más recompensas de lo normal, hasta que `collectable-frequency`{:class="block3variables"} se cambie de nuevo a `1`. ¿Puedes pensar en algúnos problemas que eso pueda causar? Habrá muchas más recompensas…¿qué pasaría si las siguieras atrapando?

--- /collapse ---

Ahora tienes el objeto listo para recibir al bloque `lluvia de coleccionables`{:class="block3events"}, pero aún no has creado código para enviar el mensaje.

--- task ---

A continuación, actualiza el bloque `react-to-player`{:class="block3myblocks"} para que se vea así, y envíe el mensaje `lluvia de coleccionables`{:class="block3events"} cuando el jugador toca una recompensa de tipo `2`.

```blocks3
    definir react-to-player (type)
    si <(type ::variable) = [1]> entonces
        cambiar [points v] en (collectable-value ::variables)
    fin
    si <(type ::variable) = [2]> entonces
+        enviar [lluvia de coleccionables v]
    fin
```

--- /task ---
