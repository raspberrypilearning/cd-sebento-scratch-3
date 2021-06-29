## Super power-ups!

Nu je een nieuwe power-up hebt, is het tijd om het iets cools te laten doen! Laten we het een paar seconden power-ups laten 'regenen' in plaats van gewoon een extra leven te geven.

Hiervoor moet je nog een stuk code maken dat start terwijl het `reageer-op-speler`{:class="block3myblocks"} blok klaar is met uitvoeren. Om dat mogelijk te maken, gebruik je een `zend signaal`{:class="block3events"} blok om een bericht naar een ander stuk code in deze sprite te verzenden.

--- task ---

Maak dit blok voor de `Collectable` sprite. Laten we het signaal `prijzen-regen`{:class="block3events"} noemen.

```blocks3
+    when I receive [prijzen-regen v]
+    set [prijs-frequentie v] to [0.000001]
+    wait (1) secs
+    set [prijs-frequentie v] to [1]
```

--- /task ---

--- collapse ---
---
title: Wat doet de nieuwe code?
---

Dit blok stelt `prijs-frequentie`{:class="block3variables"} in op een heel klein aantal \(verander het in verschillende waarden en kijk wat er gebeurt!\), wacht dan een seconde en verandert het terug in `1`.

Dit ziet er niet naar uit dat het veel zou moeten doen, maar denk na over wat er tijdens die seconde gebeurt: de `wanneer op de groene vlag wordt geklikt`{:class="block3events"} code is nog steeds actief en de `herhaal tot`{:class="block3control"} lus is ook nog bezig. Bekijk de code in die lus:

```blocks3
    repeat until <not <(create-collectables ::variables) = [true]>>
        if < [50] = (pick random (1) to (50))> then
            set [prijs-type v] to [2]
        else
            set [prijs-type v] to [1]
        end
        wait (prijs-frequentie ::variables) secs
        go to x: (pick random (-240) to (240)) y:(-179)
        create clone of [myself v]
    end
```

Je kunt zien dat het `wacht` blok hier de code pauzeert voor de tijdsduur ingesteld door `prijs-frequentie`{:class="block3variables"}. Dus als de waarde van `prijs-frequentie`{:class="block3variables"} verandert in `0.000001`, pauzeert het blok van `wacht` slechts **één miljoenste** seconde, wat betekent dat de lus veel vaker zal lopen dan normaal. Als gevolg hiervan gaat de code **veel** meer power-ups genereren dan normaal, totdat `prijs-frequentie`{:class="block3variables"} `1` weer terug verandert. Kun je mogelijke problemen bedenken? Er zullen nog veel meer super-scheten zijn…wat als je ze blijft vangen?

--- /collapse ---

Nu heb je de sprite klaar om het `prijzen-regen`{:class="block3events"} signaal te ontvangen, maar je hebt nog geen code gemaakt voor het verzenden van het signaal.

--- task ---

Werk vervolgens het `reageer-op-speler`{:class="block3myblocks"} blok bij zodat het er zo uitziet, zodat het het bericht `prijzen-regen`{:class="block3events"} verstuurt wanneer de speler een type `2` power-up aanraakt.

```blocks3
    define reageer-op-speler (type)
    if <(type ::variable) = [1]> then
        change [punten v] by (prijs-waarde ::variables)
    end
    if <(type ::variable) = [2]> then
+        broadcast [prijzen-regen v]
    end
```

--- /task ---
