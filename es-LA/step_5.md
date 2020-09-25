## ¡Súper recompensas!

Ahora que tienes una nueva recompensa, ¡es el momento de hacer que haga algo genial! Hagamos que ¨lluevan¨ recompensas durante unos segundos, en lugar de sólo dar una vida extra.

Para esto, necesitas crear otra pieza de código que comenzará mientras el bloque `react-to-player`{:class="block3myblocks"} termina de ejecutarse. Para que eso suceda, utilizarás un bloque `enviar`{:class="block3events"} para enviar un mensaje a otra pieza de código dentro de este objeto.

--- task ---

Crea este bloque para el objeto `Collectable`. Llamemos al mensaje `collectable-rain`{:class="block3events"}.

```blocks3
+    when I receive [collectable-rain v]
+    set [collectable-frequency v] to [0.000001]
+    wait (1) secs
+    set [collectable-frequency v] to [1]
```

--- /task ---

--- collapse ---
---
title: ¿Qué hace el nuevo bloque?
---

Este bloque solo establece `collectable-frequency`{:class="block3variables"} a un número muy pequeño (¡cámbialo a diferentes valores y mira qué pasa!) y luego espera un segundo y lo vuelve a cambiar a `1`.

Esto no parece que hiciera mucho, pero piensa en lo que está sucediendo durante ese segundo: el código `al presionar la bandera verde`{:class="block3events"} sigue funcionando, y el ciclo `repetir hasta que`{:class="block3control"} dentro de él está repitiéndose. Mira el código en ese ciclo:

```blocks3
    repeat until <not <(create-collectables ::variables) = [true]>>
        if < [50] = (pick random (1) to (50))> then
            set [collectable-type v] to [2]
        else
            set [collectable-type v] to [1]
        end
        wait (collectable-frequency ::variables) secs
        go to x: (pick random (-240) to (240)) y:(-179)
        create clone of [myself v]
    end
```

Puedes ver que el bloque de `esperar` aquí detiene el código durante el tiempo establecido por `collectable-frequency`{:class="block3variables"}. Entonces, si el valor de `collectable-frequency`{:class="block3variables"} cambia a `0.000001`, el bloque `esperar`{:class="block3control"} solo se detiene por **una millonésima** de segundo; lo que significa que el ciclo se ejecutará muchas más veces de lo normal. Como resultado, el código creará **muchas** más recompensas de lo normal, hasta que `collectable-frequency`{:class="block3variables"} se cambie de nuevo a `1`. ¿Puedes pensar en algúnos problemas que eso pueda causar? Habrá muchas más recompensas…¿qué pasaría si las siguieras atrapando?

--- /collapse ---

Ahora tienes el objeto listo para recibir al bloque `collectable-rain`{:class="block3events"}, pero aún no has creado código para enviar el mensaje.

--- task ---

A continuación, actualiza el bloque `react-to-player`{:class="block3myblocks"} para que se vea así, y envíe el mensaje `lluvia de coleccionables`{:class="block3events"} cuando el jugador toca una recompensa de tipo `2`.

```blocks3
    define react-to-player (type)
    if <(type ::variable) = [1]> then
        change [points v] by (collectable-value ::variables)
    end
    if <(type ::variable) = [2]> then
+        broadcast [collectable-rain v]
    end
```

--- /task ---
