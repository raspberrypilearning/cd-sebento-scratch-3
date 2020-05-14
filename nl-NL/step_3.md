## Het spel verliezen

Het is je misschien opgevallen dat het `verlies`{:class="block3myblocks"} **Mijn blokken** blok van de `speler` sprite leeg is. Je gaat dit invullen en alle stukken instellen die nodig zijn voor een leuk 'Game over' scherm.

--- task ---

Zoek eerst het blok `verlies`{:class="block3myblocks"} en vul het aan met de volgende code:

```blocks3
    define verlies
+    stop [other scripts in sprite v] :: control stack
+    broadcast [game over v]
+    go to x:(0) y:(0)
+    say [Game over!] for (2) secs
+    say [Het is vrijwel onmogelijk om alle methaan te vangen, toch?] for (5) secs
+    say [Het is beter om de hoeveelheid uitstoot te verminderen.] for (6) secs
+    say [Door rekening te houden met de gevolgen van hoe we voedsel produceren...] for (5) secs
+    say [...kunnen we het op een duurzamere manier doen die beter is voor iedereen.] for (6) secs
+    stop [all v]
```

--- /task ---

--- collapse ---
---
title: Wat doet de code?
---

Wanneer het `verlies`{:class="block3myblocks"} blok uitgevoerd wordt, doet het dit:

 1. Stop de natuurkunde en andere spelscripts van de `speler`
 2. Vertel alle andere sprites dat het spel over is door **een signaal** te zenden zodat ze op basis daarvan kunnen veranderen
 3. Verplaats de `speler` naar het midden van het scherm en laat ze de speler vertellen dat het spel afgelopen is
 4. Stop alle scripts in het spel

--- /collapse ---

Nu moet je ervoor zorgen dat alle sprites weten wat ze moeten doen als het spel is afgelopen en hoe ze zichzelf kunnen resetten als de speler een nieuw spel start. **Vergeet niet dat nieuwe sprites die je toevoegt hiervoor mogelijk ook code nodig hebben!**

### De platforms en randen verbergen

--- task ---

Begin met de basis: de sprites `Platforms` en `Edges` hebben beide code nodig om te verschijnen wanneer het spel start en te verdwijnen bij 'Game over', dus voeg dit aan elk van hen toe:

```blocks3
    when I receive [game over  v]
    hide
```

```blocks3
    when green flag clicked
    show
```

--- /task ---

### De scheten stoppen

Nu is het tijd voor iets moeilijkers! Als je naar de code voor de `Collectable` sprite kijkt, zie je dat deze werkt door **zichzelf** te klonen. Dat wil zeggen dat het kopieën van zichzelf maakt die de speciale `wanneer ik als kloon start`{:class="block3events"} instructies volgen.

We zullen meer vertellen over wat klonen speciaal maakt als we op de kaart komen over het maken van nieuwe en verschillende verzamelobjecten. Voor nu moet je weten dat klonen **bijna** alles kunnen doen wat een normale sprite kan, inclusief het ontvangen van `verzonden`{:class="block3events"} signalen.

Kijk hoe de `Collectable` sprite werkt. Kijk of je een deel van de code kunt begrijpen:

```blocks3
    when green flag clicked
    set size to (35) %
    hide
    set [prijs-waarde v] to [1]
    set [prijs-tempo v] to [1]
    set [prijs-frequentie v] to [1]
    set [maak-prijzen v] to [true]
    set [prijs-type v] to [1]
    repeat until <not <(maak-prijzen) = [true]>>
        wait (prijs-frequentie) secs
        go to x: (pick random (-240) to (240)) y: (-179)
        create clone of [myself v]
    end
```

 1. Eerst maakt het de originele prijs onzichtbaar.
 2. Vervolgens worden de besturingsvariabelen ingesteld. We komen hier later op terug.
 3. De `maak-prijzen`{:class="block3variables"} variabele is de aan/uit schakelaar voor het klonen: de lus maakt klonen als `maak-prijzen`{:class="block3variables"} `waar` is en doet niets als dat niet zo is.

--- task ---

Zet nu een blok op de `Collectable` sprite zodat deze reageert op het `game over` signaal:

```blocks3
    when I receive [game over v]
    hide
    set [maak-prijzen v] to [false]
```

--- /task ---

Deze code is vergelijkbaar met de code die de sprites `Edges` en `Platforms` bestuurt. Het enige verschil is dat je ook de `maak-prijzen`{:class="block3variables"} variabele op `onwaar` instelt, zodat er geen nieuwe klonen worden gemaakt wanneer het 'game over' is.

Merk op dat je deze variabele kunt gebruiken om berichten van het ene deel van je code naar het andere door te geven! 
