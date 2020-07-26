# Tutorial: Labyrinth

In diesem Tutorial schreiben wir Schritt für Schritt ein Spiel bei dem wir ein Labyrinth durchqueren müssen. Der benutzte Code ist dabei recht simpel: hauptsächlich Bedingungen und Schleifen. Tilemaps sind in 2D-Spielen recht beliebt. Dabei baut man die Spielwelt aus kleinen, quadratischen Kacheln, oder eben Tiles. Nach diesem Tutorial sollte es dir möglich sein, Tilemaps auch für deine eigenen Projekte zu erstellen. 

![Maze game](https://i.imgur.com/RjXI6oO.png)


## Tilemap

Wie bereits erwähnt, nutzt eine Tilemap eine kleine Zahl an Bildern (Tiles) und wiederholt sie viele Male, um daraus ein größeres Spiellevel (Map) zu bauen. Das erspart einen den Aufwand für jedes Level eine individuelle Map zu zeichnen und erleichtert es, dass Mapdesign bei Bedarf schnell zu verändern. In diesem Beispiel bauen wir ein Labyrinth. 

Für die Tiles brauchen wir drei Bilddateien: `empty.png`, `wall.png` und `goal.png`, die wir im `mu_code/images` Ordner (erreichbar über den `images` Button in Mu) speichern.

Die Größe der Tiles sollte 64×64 Pixels betragen. Als Spielfigur nutzen wir das bereits vorgegebene `alien` Bild.

```python
TILE_SIZE = 64
WIDTH = TILE_SIZE * 8
HEIGHT = TILE_SIZE * 8

tiles = ['empty', 'wall', 'goal']

maze = [
    [1, 1, 1, 1, 1, 1, 1, 1],
    [1, 0, 0, 0, 1, 2, 0, 1],
    [1, 0, 1, 0, 1, 1, 0, 1],
    [1, 0, 1, 0, 0, 0, 0, 1],
    [1, 0, 1, 0, 1, 0, 0, 1],
    [1, 0, 1, 0, 1, 0, 0, 1],
    [1, 0, 1, 0, 0, 0, 0, 1],
    [1, 1, 1, 1, 1, 1, 1, 1]
]

player = Actor("alien", anchor=(0, 0), pos=(1 * TILE_SIZE, 1 * TILE_SIZE))

def draw():
    screen.clear()
    for row in range(len(maze)):
        for column in range(len(maze[row])):
            x = column * TILE_SIZE
            y = row * TILE_SIZE
            tile = tiles[maze[row][column]]
            screen.blit(tile, (x, y))
    player.draw()
```


Die Dateinamen der Bilder werden in einer *Liste* `tiles` gespeichert. Das Leveldesign ist in Listen innerhalb einer Liste hinterlegt, dies wird *zweidimensionales Array* genannt. In diesem Array gibt es 8 Reihen, sowie 8 Spalten. Wenn du die Größe des Array änderst, dann musst du auch die `WIDTH` und `HEIGHT` Werte ändern. Die Zahlen im `maze` Array beziehen sich auf die Elemente der `tiles` Liste: `0` steht für `empty` und `1` steht für `wall`, etc.

![Tile grid](https://i.imgur.com/25bPJDK.png)

Um das Labyrinth zu zeichnen, nutzen wir eine `for` Schleife innerhalb einer weiteren `for` Schleife. Die äußere Schleife wiederholt die Reihen und die Innere die Spalten, sprich die Elemente der Reihe. 


### Übungsaufgaben

+ Zeichne deine eigenen Bilder `empty.png`, `wall.png` und `goal.png` und führe das Programm aus.
+ Der Spieler erscheint oben links in Reihe 1, Spalte 1. Ändere den `pos` Parameter, damit er stattdessen unten erscheint.
+ Ändere das Design des Labyrinths, indem du die Nummern in `maze` veränderst.
+ (Für Fortgeschrittene):
Vergrößere das Labyrinth.




## Die Spielfigur bewegen

Füge diesen Code *am Ende* des Programms ein:

```python
def on_key_down(key):
    row = int(player.y / TILE_SIZE)
    column = int(player.x / TILE_SIZE)
    if key == keys.UP:
        row = row - 1
    if key == keys.DOWN:
        row = row + 1
    if key == keys.LEFT:
        column = column - 1
    if key == keys.RIGHT:
        column = column + 1
    player.x = column * TILE_SIZE
    player.y = row * TILE_SIZE
```

Die Funktion wird, wie `draw()` und `update()`, automatisch von Pygame abgerufen. `on_key_down()` wird dabei jedoch nicht in jedem Frame abgrufen, sondern nur, wenn der Spieler eine Taste drückt. Welche Taste gedrückt wurde wird an die Funktion im `key` *Parameter* weitergegeben.

### Übungsaufgabe
+ Führe das Programm aus und bewege die Spielfigur. Gibt es dabei Probleme?


## Einschränkung der Bewegungen

Lösche die letzten zwei Zeilen des Programms und ersetze sie durch das Folgende:

```python
    tile = tiles[maze[row][column]]
    if tile == 'empty':
        player.x = column * TILE_SIZE
        player.y = row * TILE_SIZE
    elif tile == 'goal':
        print("Well done")
        exit()
```

### Übungsaufgabe
+ Führe das Programm aus und prüfe, ob die Spielfigur sich *nur* dann bewegt, wenn das Tile leer ist.
+ Prüfe, ob das Spiel beendet wird, wenn der Spieler das Ziel erreicht.



## Bewegungsanimation

Da das Alien-Bild etwas zu groß ist, brauchen wir ein neues in der Größe 64x64 Pixel. Zeichne eine neue Spielfigur und speichere es als `player.png` im `images` Ordner. Im Programm ändern wir folgende Zeile:

```python
player = Actor("alien", anchor=(0, 0), pos=(1 * TILE_SIZE, 1 * TILE_SIZE))
```

zu

```python
player = Actor("player", anchor=(0, 0), pos=(1 * TILE_SIZE, 1 * TILE_SIZE))
```

Die Bewegungen der Spielfigur wirken sehr abgehakt, doch Pygame hat eine eingebaute Funktion, die Bewegungen für uns automtaisch flüssiger macht. Finde diese Zeilen im Programm: 

```python
    if tile == 'empty':
          player.x = column * TILE_SIZE
          player.y = row * TILE_SIZE
```
          
und ersetze sie durch:
 
```python
    if tile == 'empty':
        x = column * TILE_SIZE
        y = row * TILE_SIZE
        animate(player, duration=0.1, pos=(x, y))

```

### Übungsaufgabe
+ Überprüfe, ob sich das Bild der Spielfigur geändert hat und sich flüssig bewegt.



## Gegner hinzufügen

Wir fügen einen einfachen Gegner hinzu, der sich nach auf und ab bewegt. Füge diesen Code gleich *über* der `draw()` Funktion ein:

```python
enemy = Actor("enemy", anchor=(0, 0), pos=(3 * TILE_SIZE, 6 * TILE_SIZE))
enemy.yv = -1
```

Um den Gegner sichtbar zu machen, füge noch die folgende Zeile am Ende der `draw()` Funktion ein:

```python
  enemy.draw()
```

`enemy.yv` ist die Bewegung auf der Y-Achse (auf und ab). Füge diese Zeilen am Ende des Programms, aber innerhalb der `on_key_down()` Funktion ein, damit der Gegner sich bewegt und die Bewegung sich umkehrt, wenn er eine Wand berührt:

```python
    # enemy movement
    row = int(enemy.y / TILE_SIZE)
    column = int(enemy.x / TILE_SIZE)
    row = row + enemy.yv
    tile = tiles[maze[row][column]]
    if not tile == 'wall':
        x = column * TILE_SIZE
        y = row * TILE_SIZE
        animate(enemy, duration=0.1, pos=(x, y))
    else:
        enemy.yv = enemy.yv * -1
    if enemy.colliderect(player):
        print("You died")
        exit()
```

### Übungsaufgabe
+ Prüfe, ob der Gegner sich auf und ab bewegt, sowie den Spieler tötet.
+ (Für Fortgeschrittene):
Füge einen weiteren Gegner hinzu, der sich horizontal (links nach rechts) bewegt.

+ (Für Fortgeschrittene):
Die Kollisionsprüfung ist recht gnädig (sprich buggy), weil sie Kollisionen zwischen Gegner und Spieler nur prüft, wenn eine Taste gedrückt wird. Definiere eine neue Funktion namens `update()` und füge die Kollisionsprüfung dort ein, damit jeder Frame geprüft wird.

## Eine verschlossene Tür und ein Schlüssel

Wir fügen zwei weitere Tiles ins Spiel ein. Zeichne Bilder für `door.png` und `key.png` und speichere sie im `images` Ordner.

Gehe zur `tiles` Liste am Anfang und **ändere** sie, damit sie die neuen Bilder enthält. Passe `maze` an und füge ein paar 3en und 4en, wo du willst, dass die neuen Tiles erscheinen hinzu. Ein Beispiel:

```python
tiles = ['empty', 'wall', 'goal', 'door', 'key']
unlock = 0

maze = [
    [1, 1, 1, 1, 1, 1, 1, 1],
    [1, 0, 0, 0, 1, 2, 0, 1],
    [1, 0, 1, 0, 1, 1, 3, 1],
    [1, 0, 1, 0, 0, 0, 0, 1],
    [1, 0, 1, 0, 1, 0, 0, 1],
    [1, 0, 1, 4, 1, 0, 0, 1],
    [1, 0, 1, 0, 0, 0, 0, 1],
    [1, 1, 1, 1, 1, 1, 1, 1]
]
```

Füge am Anfang des Programms eine neue Variable ein, die anzeigt, wie viele Schlüssel der Spieler bei sich trägt.

```python
unlock = 0
```

Gehe zum `if` Statement, wo auf das Erreichen des `goal` getestet wird:

```python
   if tile == 'goal':
        print("Well done")
        exit()
```

Passe es an, damit es auch auf Schlüssel und Türtiles prüft. Da wir eine *globale* Variable innerhalb einer Funktion ändern, müssen wir diese auch angeben. 

```python
    global unlock
    if tile == 'goal':
        print("Well done")
        exit()
    elif tile == 'key':
        unlock = unlock + 1
        maze[row][column] = 0 # 0 is 'empty' tile
    elif tile == 'door' and unlock > 0:
        unlock = unlock - 1
        maze[row][column] = 0 # 0 is 'empty' tile
```



## Das fertige Spiel

Nachfolgend das fertige Spiel inklusive aller Änderungen:

```python=
TILE_SIZE = 64
WIDTH = TILE_SIZE * 8
HEIGHT = TILE_SIZE * 8

tiles = ['empty', 'wall', 'goal', 'door', 'key']
unlock = 0

maze = [
    [1, 1, 1, 1, 1, 1, 1, 1],
    [1, 0, 0, 0, 1, 2, 0, 1],
    [1, 0, 1, 0, 1, 1, 3, 1],
    [1, 0, 1, 0, 0, 0, 0, 1],
    [1, 0, 1, 0, 1, 0, 0, 1],
    [1, 0, 1, 4, 1, 0, 0, 1],
    [1, 0, 1, 0, 0, 0, 0, 1],
    [1, 1, 1, 1, 1, 1, 1, 1]
]

player = Actor("player", anchor=(0, 0), pos=(1 * TILE_SIZE, 1 * TILE_SIZE))
enemy = Actor("enemy", anchor=(0, 0), pos=(3 * TILE_SIZE, 6 * TILE_SIZE))
enemy.yv = -1

def draw():
    screen.clear()
    for row in range(len(maze)):
        for column in range(len(maze[row])):
            x = column * TILE_SIZE
            y = row * TILE_SIZE
            tile = tiles[maze[row][column]]
            screen.blit(tile, (x, y))
    player.draw()
    enemy.draw()

def on_key_down(key):
    # player movement
    row = int(player.y / TILE_SIZE)
    column = int(player.x / TILE_SIZE)
    if key == keys.UP:
        row = row - 1
    if key == keys.DOWN:
        row = row + 1
    if key == keys.LEFT:
        column = column - 1
    if key == keys.RIGHT:
        column = column + 1
    tile = tiles[maze[row][column]]
    if tile == 'empty':
        x = column * TILE_SIZE
        y = row * TILE_SIZE
        animate(player, duration=0.1, pos=(x, y))
    global unlock
    if tile == 'goal':
        print("Well done")
        exit()
    elif tile == 'key':
        unlock = unlock + 1
        maze[row][column] = 0 # 0 is 'empty' tile
    elif tile == 'door' and unlock > 0:
        unlock = unlock - 1
        maze[row][column] = 0 # 0 is 'empty' tile

    # enemy movement
    row = int(enemy.y / TILE_SIZE)
    column = int(enemy.x / TILE_SIZE)
    row = row + enemy.yv
    tile = tiles[maze[row][column]]
    if not tile == 'wall':
        x = column * TILE_SIZE
        y = row * TILE_SIZE
        animate(enemy, duration=0.1, pos=(x, y))
    else:
        enemy.yv = enemy.yv * -1
    if enemy.colliderect(player):
        print("You died")
        exit()
```


## Ideen für Erweiterungen

* Zeige einen Score an.
* Füge Münzen hinzu, die den Score erhöhenCoins that the player collects to increase score.
* Fallen Tiles, die schwer zu sehen sind und den Spieler töten.
* Eine Schatztruhe, die mit einem Schlüssel aufgeschlossen wird und ebenfalls den Score erhöht.
* Anstatt das Spiel zu beenden, gib dem Spieler drei Leben.
* Füge mehr Tiles zur Map hinzu: Wasser, Feuer, Steine, usw.
* Ändere das Spielerbild, je nachdem in welche Richtung es sich bewegt.