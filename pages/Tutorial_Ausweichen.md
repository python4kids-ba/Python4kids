# Tutorial: Ausweichen! Ausweichen!!!

In diesem Spiel geht es darum, einem fallenden Stern so lange wie möglich auszuweichen. Dabei nimmt die Geschwindigkeit immer weiter zu. Wie lange schaffst du es zu überleben?

## Module laden

Für dieses Spiel brauchen wir die Module `random` und `time`.

```python=
import random
import time
```

## Konstanten und Variablen festlegen

Als erstes legen wir die Konstanten und Variablen fest. Wir nutzen das bereits vorhandene Alienbild als Spielfigur und suchen uns noch ein Sprite von einem Stern. Der hier verwendete Sprite stammt aus [RPG Maker VX Ace](https://www.rpgmakerweb.com/products/programs/rpg-maker-vx-ace). Wir geben auch die Position unseres Aliens an. Es startet am unteren Bildschirmrand, während unser Stern an einer beliebigen Position am oberen Bildschirmrand ist.

```python=
WIDTH = 600
HEIGHT = 700
geschwindigkeit = 10
game_over = False
player = Actor("alien")
player.x = 300
player.y = 600
stern = Actor("stern")
stern.x = random.randint(0,600)
stern.y = 30
ueberlebte_zeit = 0
start_zeit = time.time()
```

## Player und Stern zeichnen

Mit der `draw()` Funktion fügen wir unser Alien und den Stern auf unser Spielfeld ein. 

```python=
def draw():
    screen.clear()
    player.draw()
    stern.draw()
```

Wenn wir das kurz testen, sollte unser Spiel bisher so aussehen:
![](https://i.imgur.com/PUGaZE8.png)


## Steuerung

Umdem fallenden Stern auszuweichen, muss unser Alien sich bewegen können, solange das Spiel noch nicht vorbei ist. Für dieses Spiel reichen einfache Bewegungen nach links und rechts. Außerdem sorgen wir dafür, dass wir nicht versehentlich das Spielfeld verlassen können.

```python=
def update():
    global game_over
    if not game_over:
        if keyboard.right and player.x < (WIDTH-40):
            player.x = player.x + 4
        if keyboard.left and player.x > (0+40):
            player.x = player.x - 4
```

## Fallender Stern

Jetzt, da wir uns bewegen können, müssen wir noch den Stern zum Fallen bringen. Jedes Mal, wenn der Stern den unteren Bildschirmrand erreicht, wird er wieder nach oben gesetzt und seine Fallgeschwindigkeit erhöht sich. Das können wir auch direkt in unsere `update()` Funktion schreiben. Solange das Spiel noch nicht vorbei ist, soll der Stern nach unten fallen. Dafür schreiben wir direkt unter unsere Steuerung für das Alien:

```python=
        if stern.y < (HEIGHT+40):
            stern.y += geschwindigkeit
        if stern.y > 700:
            stern.x = random.randint(0+40,WIDTH-40)
            stern.y = 0
            geschwindigkeit += 1
```

Zusätzlich müssen wir die Variable `geschwindigkeit` zu den globalen Variablen schreiben. `update()` sieht daher anschließend so aus: 

```python=
def update():
    global game_over, geschwindigkeit
    if not game_over:
        if keyboard.right and player.x < (WIDTH-40):
            player.x = player.x + 4
        if keyboard.left and player.x > (0+40):
            player.x = player.x - 4
        if stern.y < (HEIGHT+40):
            stern.y += geschwindigkeit
        if stern.y > 700:
            stern.x = random.randint(0+40,WIDTH-40)
            stern.y = 0
            geschwindigkeit += 1
```


## Game Over

Wenn der Spieler von einem Stern getroffen wird, dann ist das Spiel vorbei. Dafür fügen wir eine Kollisionskontrolle in unsere `update()` Funktion ein. Diese Zeilen stehen **nicht** in der `if not game_over:` Anweisung.

```python=
    if player.colliderect(stern):
        game_over = True
```

## Wie lange hast du überlebt?

Am Ende wollen wir noch wissen, wie lange wir dem Stern ausweichen konnten. Dafür lassen wir oben links einen Timer anzeigen. Wir updaten also unsere `draw()` Funktion zum einen mit der Variablen `ueberlebte_zeit`, die bei den globalen Variablen steht und mit einer Zeitanzeige, die solange angezeigt wird, solange das Spiel noch nicht vorbei ist.
Der Code dafür ist:

```python=
    global ueberlebte_zeit

    if not game_over:
        ueberlebte_zeit = int(time.time() - start_zeit)
        screen.draw.text("Ueberlebt seit: " +  str(ueberlebte_zeit) + " Sekunden",
            topleft = (10, 10), color = "white")    
```

## Die Game Over Nachricht

Abschließend wollen wir noch eine Nachricht, die angezeigt wird, sobald das Spiel vorbei ist. Neben "Game Over!" soll sie auch anzeigen, wie lange wir überlebt haben. Dafür erweitern wir die `draw()` Funktion mit folgenden Zeilen:

```python=
    if game_over:
        screen.draw.textbox(f"Game Over! Du hast {ueberlebte_zeit} Sekunden ueberlebt.", Rect(200, 150, 200, 200))
```

Im Spiel würde das dann so aussehen:

![](https://i.imgur.com/EMCeRE5.png)



## Das komplette Spiel

Hier findet ihr das komplette Spiel in einem Stück

```python=
import random
import time

WIDTH = 600
HEIGHT = 700
geschwindigkeit = 10
game_over = False
player = Actor("alien")
player.x = 300
player.y = 600
stern = Actor("stern")
stern.x = random.randint(0,600)
stern.y = 30
ueberlebte_zeit = 0
start_zeit = time.time()

def draw(): 
    global game_over, ueberlebte_zeit
    screen.clear()
    player.draw()
    stern.draw()
    if not game_over:
        ueberlebte_zeit = int(time.time() - start_zeit)
        screen.draw.text("Ueberlebt seit: " +  str(ueberlebte_zeit) + " Sekunden",
            topleft = (10, 10), color = "white")
    if game_over:
        screen.draw.textbox(f"Game Over! Du hast {ueberlebte_zeit} Sekunden ueberlebt.", Rect(200, 150, 200, 200))
        
   
def update():
    global geschwindigkeit, game_over
    if not game_over:
        if keyboard.right and player.x < (WIDTH-40):
            player.x = player.x + 5
        if keyboard.left and player.x > (0+40):
            player.x = player.x - 5
        if stern.y < (HEIGHT+40):
            stern.y += geschwindigkeit
        if stern.y > 700:
            stern.x = random.randint(0+40,WIDTH-40)
            stern.y = 0
            geschwindigkeit += 1
    if player.colliderect(stern):
        game_over = True
```

## Ideen für Erweiterungen

+ Füge einen passenden Hintergrund ein.
+ Zeichne deine eigenen Figuren.
+ Vergößere das Spielfeld und füge einen zweiten Spieler hinzu.
+ Gib dem Alien 3 Leben.
+ Ändere den Sprite des Aliens, wenn es getroffen wurde.