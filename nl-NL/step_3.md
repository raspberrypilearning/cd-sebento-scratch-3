## Het spel verliezen

Het is je misschien opgevallen dat het `verlies`{:class="block3myblocks"} **Mijn blokken** blok van de `speler` sprite leeg is. Je gaat dit invullen en alle stukken instellen die nodig zijn voor een leuk 'Game over' scherm.

--- task ---

Zoek eerst het blok `verlies`{:class="block3myblocks"} en vul het aan met de volgende code:

```blocks3
    definieer verlies
+ stop [andere scripts in sprite v] :: control stack
+ zend signaal [game over v]
+ ga naar x: (0) y: (0)
+ zeg [Game over!] (2) sec.
+ zeg [Het is vrijwel onmogelijk om alle methaan te vangen, toch?] (5) sec.
+ zeg [Het is beter om de hoeveelheid uitstoot te verminderen.] (6) sec.
+ zeg [Door rekening te houden met de gevolgen van hoe we voedsel produceren...](5) sec.
+ zeg [...kunnen we het op een duurzamere manier doen die beter is voor iedereen.] (6) sec.
+ stop [alle v]
```

--- /task ---

--- collapse ---
---
titel: Wat doet de code?
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
    wanneer ik signaal [game over v] ontvang
    verdwijn
```

```blocks3
    wanneer op de groene vlag wordt geklikt
    verschijn
```

--- /task ---

### De scheten stoppen

Nu is het tijd voor iets moeilijkers! Als je naar de code voor de `Collectable` sprite kijkt, zie je dat deze werkt door **zichzelf** te klonen. Dat wil zeggen dat het kopieën van zichzelf maakt die de speciale `wanneer ik als kloon start`{:class="block3events"} instructies volgen.

We zullen meer vertellen over wat klonen speciaal maakt als we op de kaart komen over het maken van nieuwe en verschillende verzamelobjecten. Voor nu moet je weten dat klonen **bijna** alles kunnen doen wat een normale sprite kan, inclusief het ontvangen van `verzonden`{:class="block3events"} signalen.

Kijk hoe de `Collectable` sprite werkt. Kijk of je een deel van de code kunt begrijpen:

```blocks3
    wanneer op de groene vlag wordt geklikt
    maak grootte (35) %
    verdwijn
    maak [prijs-waarde v] [1]
    maak [prijs-tempo v] [1]
    maak [prijs-frequentie v] [1]
    maak [maak-prijzen v] [true]
    maak [prijs-type v] [1]
    herhaal tot <niet <(maak-prijzen) = [true]>>
        wacht (prijs-frequentie) sec.
        ga naar x: (willekeurig getal tussen (-240) en (240)) y: (-179)
        maak een kloon van [mijzelf v]
    einde
```

 1. Eerst maakt het de originele prijs onzichtbaar.
 2. Vervolgens worden de besturingsvariabelen ingesteld. We komen hier later op terug.
 3. De `maak-prijzen`{:class="block3variables"} variabele is de aan/uit schakelaar voor het klonen: de lus maakt klonen als `maak-prijzen`{:class="block3variables"} `waar` is en doet niets als dat niet zo is.

--- task ---

Zet nu een blok op de `Collectable` sprite zodat deze reageert op het `game over` signaal:

```blocks3
    wanneer ik signaal [game over v] ontvang
    verdwijn
    maak [maak-prijzen v] op [false]
```

--- /task ---

Deze code is vergelijkbaar met de code die de sprites `Edges` en `Platforms` bestuurt. Het enige verschil is dat je ook de `maak-prijzen`{:class="block3variables"} variabele op `onwaar` instelt, zodat er geen nieuwe klonen worden gemaakt wanneer het 'game over' is.

Merk op dat je deze variabele kunt gebruiken om berichten van het ene deel van je code naar het andere door te geven! 
