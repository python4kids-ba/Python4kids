# Minecraft

Die Minecraft Python library wurde von David Whale und Martin O'Hanlon entwickelt. Ihr Buch *Adventures in Minecraft* enthält noch eine Reihe weiterer Programme. 

## Setup

Du brauchst Minecraft **Java Edition**.  [https://my.minecraft.net/en-us/store/minecraft/)](https://my.minecraft.net/en-us/store/minecraft/)

Wenn du bereits einen Mojang Account hast, klicke auf Login. Solltest du ihn schon seit mehreren Jahren nicht mehr benutzt haben, kann es sein, dass du dein Passwort ändern musst. Es sagt dir, ob du bereits Minecraft besitzt. Wenn du keinen Mojang Account hast, dann registriere dich und kaufe die Minecraft Java Edition. 

### Setup Java

Du musst Java Runtime Environment Version 8 installieren.

Download für Windows: [https://github.com/frekele/oracle-java/releases/download/8u212-b10/jre-8u212-windows-x64.exe](https://github.com/frekele/oracle-java/releases/download/8u212-b10/jre-8u212-windows-x64.exe)

Download für Mac: [https://github.com/frekele/oracle-java/releases/download/8u212-b10/jre-8u212-macosx-x64.dmg](https://github.com/frekele/oracle-java/releases/download/8u212-b10/jre-8u212-macosx-x64.dmg)

### Setup Server

Lade das Adventures In Minecraft Starter Kit von

[https://adventuresinminecraft.github.io/](https://adventuresinminecraft.github.io/) runter und entpacke es in einen Ordner auf deinem Desktop. Auf der Seite gibt es Videos, die erklären, wie man einen Server vorbereitet. 

Ich empfehle vor dem ersten Start des Servers, die *Server/server.properties* Datei zu bearbeiten (nutze Notepad oder [SublimeText](https://www.sublimetext.com/)) und ändere

    level-type=flat

um eine flache Welt zu generieren. (Die Entscheidung liegt dabei aber ganz bei dir!) 

Um den Server zu starten, doppelklicke auf die *StartServer* Datei. Das öffnet ein Fenster für die Serverkonsole und will, dass du die Leertaste (Space) drückst. 

Wenn du später eine andere Welt generieren möchtest, ändere 

    level-name=world2

und starte den Server nochmal. 

Wenn der Server läuft ist es zu empfehlen, die Nacht erstmal zu deaktivieren. Dafür tippst du das folgende in die Serverkonsole: 

    gamerule doDaylightCycle false
    time set day

Du musst das Fenster für die Serverkonsole **immer** offen halten. Wenn du aufhören willst, tippe erst

    stop

in die Konsole, damit es deine Welt speichert. 

### Setup Minecraft

Führe den Minecraft Launcher, den du von minecraft.net runtergeladen hast, aus. Wir werden nicht die Standard-Version nutzen, sondern **version 1.12**.

Klicke *launch options* dann *add new*, setze die Version auf *release 1.12*, klicke *save*. Anschließend klickst du oben auf *Minecraft*. Klicke den Pfeil neben *Play* und wähle *1.12*. Dann klicke auf *Play*.

Minecraft läuft. Klicke 'multiplayer', dann 'add server'. Füge die Serveradresse ein:

    localhost

Klicke 'done' und dann auf deinen Server, um dich mit ihm zu verbinden.

### Setup Mu

Mu ist der Python Editor, den wir bereits verwendet haben, daher sollte er schon installiert sein. Doch du musst auf Nummer sicher gehen, dass du die aktuellste *alpha version 1.1* installiert hast, nicht die Standard 1.0 Version.
Du kannst es von [https://codewith.mu/en/download](https://codewith.mu/en/download) runterladen.

Führe Mu aus. Klicke *Modus* und wähle *Python3*.

Klicke auf das kleine Zahnrad-Icon am unteren rechten Rand und wähle 'Third Party Packages'. Tippe

    mcpi

in die Box und downloade die Bibliothek.

Wenn du einen anderen Editor benutzt, kannst du mcpi mit folgender Befehlszeile installieren:

    pip3 install mcpi
    
## Hallo Minecraft

Dieses Porgramm testet, ob du eine Verbindung zum Minecraft Server hast und zeigt eine Nachricht an.

```python=
from mcpi.minecraft import *
mc = Minecraft.create()
mc.postToChat("Hello Minecraft World")
```

## Koordinaten

Dieses Programm ruft die Spielerkoordinaten ab und gibt sie aus:

```python=
from mcpi.minecraft import *
import time

mc = Minecraft.create()

while True:     # die Schleife sorgt dafür, dass das Spiel für immer läuft
    time.sleep(1)
    pos = mc.player.getTilePos()
    print(pos)
    mc.postToChat(pos)
```


## Ändern der Spielerposition

Finde die Koordinaten eines Ortes in deiner Welt, entweder indem du im Spiel F3 drückst oder das obige Programm laufen lässt. Füge diese Koordinaten in das folgende Programm ein und führe es aus, um zu diesem Ort zu teleportieren. 

```python=
from mcpi.minecraft import *

mc = Minecraft.create()

# ändere diese wohin du willst
x = 10
y = 11
z = 12

mc.player.setTilePos(x, y, z)
```


## Baue einen Teleporter

Bevor du dieses Programm ausführst, baue zwei Tiles in deinem Spiel, die als Teleporter dienen sollen und schreibe ihre Koordinaten auf. 

```python=
from mcpi.minecraft import *

mc = Minecraft.create()

# Ändere diese Zahlen zu den Koordinaten deiner Teleporter

teleporter_x = 742
teleporter_z = 955

destination_x = 735
destination_z = 956

while True:
    # Spielerposition
    pos = mc.player.getTilePos()
    print(pos)

    # Prüfe, ob der Spieler auf dem Teleporter steht
    if pos.x == teleporter_x and pos.z == teleporter_z:
        mc.postToChat("teleport!")
        pos.x = destination_x
        pos.z = destination_z
        mc.player.setTilePos(pos)
```


### Übungsaufgabe
+ Füge diese Zeile ans Ende des Programms:
    ```python
    time.sleep(5)
    ```
+ Füge eine weitere Zeile ein, die den Spieler woanders hin teleportiert.



## Teleportiere den Spieler in die Luft


```python=
from mcpi.minecraft import *

mc = Minecraft.create()

# Ändere diese Zahlen zu den Koordinaten deiner Teleporter

teleporter_x = 9
teleporter_z = 12

height = 30

while True:
    pos = mc.player.getTilePos()

    # Prüfe, ob der Spieler auf dem Teleporter steht
    if pos.x == teleporter_x and pos.z == teleporter_z:
        mc.postToChat("teleport!")
        pos.y += height  # in der Luft
        pos.x += 1  
        mc.player.setTilePos(pos)
```


## Teleportationssprung

Dieses Porgramm führt eine Reihe an schnellen Teleportionen aus, um den Eindruck eines Sprungs zu erwecken.


```python=
from mcpi.minecraft import *
import time

mc = Minecraft.create()

# Ändere diese Zahlen zu den Koordinaten deiner Teleporter

teleporter_x = 9
teleporter_z = 12

height = 20

while True:
    pos = mc.player.getTilePos()

    if pos.x == teleporter_x and pos.z == teleporter_z:
        mc.postToChat("teleport!")
        # bewege dich vom Teleporter, damit wir nicht wieder drauf landen
        pos.x += 1
        for i in range(0, height):
            pos.y += 1  # eine kleine Bewegung nach oben
            time.sleep(0.1) # kurze Verzögerung von 0,2 Sekunden
            mc.player.setTilePos(pos)
```

### Übungsaufgabe

+ Ändere die Höhe des Sprungs.
+ Mach den Sprung schneller. 
+ Bewege den Spieler sowohl in die X und Z Richtung, als auch die Y Richtung, während des Sprungs.
+ (Für Fortgerschrittene):
Anstatt zu prüfen, ob der Spieler auf einem Teleporter Tile steht, prüfen wir, ob er innerhalb eines bestimmten Bereichs ist. Benutze `<`, `and`, `>` Operatoren.



## Erschaffe einen Block

Dieses Programm erschafft einen Block. Jede Art von Block hat seine eigene Nummer, aber wenn wir `mcpi.block` importieren, können wir Namen verwenden und müssen uns keine Nummern merken. 


```python=
from mcpi.minecraft import *
from mcpi.block import *

mc = Minecraft.create()
pos = mc.player.getTilePos()
x = pos.x
y = pos.y
z = pos.z
blocktype = 1
mc.setBlock(x, y, z, blocktype)
```

### Übungsaufgabe

+ Lass den Block in kurzer Distanz vom Spieler erscheinen. 


## Blockarten

|           |                 |        |
|:----------------- | ------------------ | ------------- |
| AIR               | BED                | BEDROCK       |
| BEDROCK_INVISIBLE | BOOKSHELF          | BRICK_BLOCK   |
| CACTUS            | CHEST              | CLAY          |
| COAL_ORE          | COBBLESTONE        | COBWEB        |
| CRAFTING_TABLE    | DIAMOND_BLOCK      | DIAMOND_ORE   |
| DIRT              | DOOR_IRON          | DOOR_WOOD     |
| FARMLAND          | FENCE              | FENCE_GATE    |
| FIRE              | FLOWER_CYAN        | FLOWER_YELLOW |
| FURNACE_ACTIVE    | FURNACE_INACTIVE   | GLASS         |
| GLASS_PANE        | GLOWSTONE_BLOCK    | GOLD_BLOCK    |
| GOLD_ORE          | GRASS              | GRASS_TALL    |
| GRAVEL            | ICE                | IRON_BLOCK    |
| IRON_ORE          | LADDER             |               |
| LAPIS_LAZULI_ORE  | LAVA               | LAVA_FLOWING  |
| LAVA_STATIONARY   | LEAVES             | MELON         |
| MOSS_STONE        | MUSHROOM_BROWN     | MUSHROOM_RED  |
| OBSIDIAN          | REDSTONE_ORE       | SAND          |
| SANDSTONE         | SAPLING            | SNOW          |
| SNOW_BLOCK        | STAIRS_COBBLESTONE |               |
| STAIRS_WOOD       | STONE              | STONE_BRICK   |
| STONE_SLAB        | STONE_SLAB_DOUBLE  | SUGAR_CANE    |
| TNT               | TORCH              | WATER         |
| WATER_FLOWING     | WATER_STATIONARY   | WOOD          |
| WOOD_PLANKS       | LAPIS_LAZULI_BLOCK | WOOL          |



## Erschaffe einen Block innerhalb einer Schleife

Dieses Programm erschafft immer wieder einen Block innerhalb der Schleife. Bewege dich, um es zu sehen. 

```python=
from mcpi.minecraft import *
from mcpi.block import *

mc = Minecraft.create()

while True:
    pos = mc.player.getTilePos()
    x = pos.x
    y = pos.y
    z = pos.z
    blocktype = WOOL
    mc.setBlock(x, y, z, blocktype)

```
### Übungsaufgabe

+ Lass der Block einen Meter **unter** dem Spieler erscheinen.
+ Ändere den Block in etwas anderes, z.B. `ICE`


## Baue einen Turm aus Blöcken

Wir nutzen eine `for` Schleife um einen Turm aus Blöcken zu bauen.

```python=
from mcpi.minecraft import *

mc = Minecraft.create()
pos = mc.player.getTilePos()
x = pos.x + 3
y = pos.y
z = pos.z

for i in range(10):
    mc.setBlock(x, y + i, z, 1)
```

### Übungsaufgabe
+ Wie hoch kannst du den Turm machen?
+ Ändere das Programm und baue 3 Türme nebeneinander.


## Aufräumen

Die `setBlocks()` Funktion lässt uns einen großen Würfel an Blöcken schaffen. Wennw ir Blöcke des Typs `AIR` erschaffen, hat das den Effekt alle Blöcke zu entfernen! Das ist so praktisch, dass wir es in der Zukunft noch benötigen werden, daher packen wir den Code dafür in eine Funktion. Speichere das Programm als `clear_space.py`, damit du es im nächsten Programm importieren kannst.

```python=
from mcpi.minecraft import *
from mcpi.block import *

def clear_space(mc, size):
    pos = mc.player.getTilePos()
    mc.setBlocks(pos.x-size, pos.y, pos.z-size, pos.x+size, pos.y+size, pos.z+size,
                 AIR)

mc = Minecraft.create()

clear_space(mc, 10)
```


## Baue ein Haus

Speichere dieses Programm als `house.py`.
Stelle sicher, dass du das vorherige Programm im selben Ordner gespeichert hast, bevor du es ausführst, da wir die Funktion von `clear_space.py` importieren. 

```python=
from mcpi.minecraft import *
from mcpi.block import *

# Hier MUSS der Name des Aufräum-Programms stehen!
from clear_space import *

def make_house(mc, x, y, z, width, height, length):
    mc.setBlocks(x, y, z, x + width, y + height, z + length, STONE)

    # Was passiert, wenn wir AIR innerhalb des Würfels machen?
    mc.setBlocks(x + 1, y + 1, z + 1,
                 x + width - 2, y + height - 2, z + length - 2, AIR)

mc = Minecraft.create()
pos = mc.player.getPos()
x = pos.x
y = pos.y
z = pos.z

width = 10
height = 50
length = 60

# Nutze die Funktion des anderen Programms
clear_space(mc, 10)
make_house(mc, x, y, z, width, height, length)
```

### Übungsaufgabe
+ Führe das Programm aus und schlage ein Loch in die Wand, um zu sehen, was drinnen ist und damit du einen Weg ins Haus hast. 
+ Ändere das Programm, damit es *automatisch* ein Loch für eine Tür macht. 
+ Senke den Boden im Haus ab.
+ Füge ein paar Möbel, Fackeln und Fenster hinzu. 
+ (Für Fortgeschrittene):
Lass die Fenster größer werden, wenn du dein Haus vergrößerst. 
+ Versuche dein Haus mit `LAVA`, `WATER`, oder `TNT` zu füllen (Sei vorsichtig mit `TNT`, zu viel lässt deinen Computer abstürzen!) 


## Bau eine Straße mit Häusern

Da wir wieder eine Funktion importieren, ist es wichtig, das alles im selben Verzeichnis gespeichert wurde. 

```python=
from mcpi.minecraft import *
from mcpi.block import *

# Hier MUSS der Name des Aufräum-Programms stehen!
from clear_space import *
# Hier MUSS der Name des Haus-Programms stehen!
from house import *

mc = Minecraft.create()
pos = mc.player.getTilePos()

x = pos.x
y = pos.y
z = pos.z

width = 10
height = 5
length = 6

clear_space(mc, 100)

for i in range(1, 100, 20):
    print(x+i, y, z)
    make_house(mc, x+i, y, z, width, height, length)
```
### Übungsaufgaben

+ Wie viele Häuser gibt es? Mach die Straße länger mit mehr Häusern.
+ Lass die Häuser größer werden je länger die Straße ist.
+ Baue ein paar Türme an die Straße.
+ (Für Fortgeschrittene):
Schreib eine Schleife in die Schleife, um mehrere Straßen zu schaffen.
+ (Für Fortgeschrittene):
Baue Wege oder Zäune.
+ Bau deine Häuser aus TNT. Benutze das flint Tool.


## Chatbefehle

Dieses Programm liest die Chatnachrichten der Spieler. Es baut einen Block neben jeden Spieler, der "build" sagt. 
Das ist das erste Beispiel, das für mehr als einen Spieler funktioniert. 

```python=
from mcpi.minecraft import *

mc = Minecraft.create()

while True:
    events = mc.events.pollChatPosts()
    for event in events:
        print(event)
        if event.message == "build":
            id = event.entityId
            pos = mc.entity.getTilePos(id)
            x = pos.x
            y = pos.y
            z = pos.z
            mc.setBlock(x, y, z, 1)
            mc.postToChat("done building!")
```

### Übungsaufgabe

+ (Für Fortgeschrittene):
Baue ein Haus um den Spielern, wenn er "house" sagt.
+ (Für Fortgeschrittene):
Baue eine Lavafalle, wenn der Spieler "trap" sagt.
+ (Für Fortgeschrittene):
Nutze `mc.getPlayerEntityId("fred")` um den Spieler namens Fred (oder wie auch immer dein Freund heißt) loszuwerden. Baue etwas an der Stelle dieses Spielers. 


## Schildkröte

*Hierfür wird das `minecraftstuff` Package benötigt.*
Du kannst es in Mu installieren, indem du unten rechts auf das Zahnrad klickst und `minecraftstuff` zur Liste der third party packages hinzufügst.


```python=
from mcpi.minecraft import *
from mcpi.block import *

from minecraftstuff import MinecraftTurtle

mc = Minecraft.create()
pos = mc.player.getTilePos()
pos.y += 1

turtle = MinecraftTurtle(mc, pos)

turtle.forward(5)
turtle.right(90)
turtle.forward(5)
turtle.right(90)
turtle.forward(5)
turtle.right(90)
turtle.forward(5)
```

### Übungsaufgabe
+ Zeichne ein Dreieck, Hexagon, usw. 
+ Was machen turtle.up(90) und turtle.down(90)?

