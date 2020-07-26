# Tutorial: Rennspiel

In diesem Tutorial bauen wir Schritt für Schritt ein Rennspiel. Python, das wir dafür brauchen: Bedingungen, Schleifen, Listen, Funktionen und Tupel. Zusätzlich haben wir Bewegung, einen High Score und einen Startbildschirm.

## Basisspiel

Ähnlich wie der Shooter, beginnen wir mit dem Auflisten des kompletten Programms und einigen leeren Funktionen, die wir im Verlauf des Tutorials füllen werden. (Python führt kein Programm mit komplett leeren Funktionen aus, daher werden sie wieder mit `pass` gefüllt, um Python zu zeigen, dass sie nichts machen.)

![Race game](https://i.imgur.com/zXMuFWY.png)


Wie beim Shooter fangen wir mit drei Punkten an:

1. Definieren von globalen Variablen.
2. Eine `draw()` Funktion.
3. Eine `update()` Funktion.

Diese Funktionen prüfen die boolesche Variable `playing`. Boolean ist ein Datentyp, der nur die Werte `True` (Wahr) oder `False` (Falsch) haben kann. Wenn die Variable `False` ist, wird der Startbildschirm gezeigt, anstatt des Spielbildschirm. 

Der einzige, wirklich komplizierte Teil dieses Programms ist, wie wir die Form des Tunnels, den der Spieler entlang fährt, speichern. `lines` ist eine Liste von *Tupeln*. Ein Tupel ist ähnlich wie eine Liste, kann jedoch *nicht verändert werden*. Es kann jedoch in einzelne Variablen *entpackt* werden. Jedes Tupel steht für eine horizontale Linie auf dem Bildschirm. Es wird drei Werte, `x`, `x2` und `color`, haben, die für die Positionen der linken Wand, die Lücke zwischen der linken und rechten Wand und die Farbe der Wand stehen.

```python=
import random
import math

WIDTH = 600
HEIGHT = 600

player = Actor("alien", (300, 580))
player.vx = 0   # horizontale Bewegung
player.vy = 1   # vertikale Bewegung

lines = []          # Liste von Tupeln der horizontalen Linien 
wall_gradient = -3  # Steilheit der Wand
left_wall_x = 200   # x-Koordinate der Wand
distance = 0        # wie weit der Spieler gefahren ist
time = 15           # Zeit bis das Spiel beendet wird
playing = False     # True wenn im Spiel, False wenn im Startbildschirm
best_distance = 0   # merkt sich den höchsten Distanz Score

def draw():
    screen.clear()
    if playing: # wir sind im Spiel
        for i in range(0, len(lines)): # zeichne die Wände
            x, x2, color = lines[i]
            screen.draw.line((0, i), (x, i), color)
            screen.draw.line((x + x2, i), (WIDTH, i), color)
        player.draw()
    else:   # wir sind im Startbildschirm
        screen.draw.text("PRESS SPACE TO START",
            (150, 300),color="green",fontsize=40)
        screen.draw.text("BEST DISTANCE: "+str(int(best_distance / 10)),
            (170, 400), color="green", fontsize=40)
    screen.draw.text("SPEED: " + str(int(player.vy)),
        (0, 0), color="green", fontsize=40)
    screen.draw.text("DISTANCE: " + str(int(distance / 10)),
        (200, 0), color="green", fontsize=40)
    screen.draw.text("TIME: " + str(int(time)),
        (480, 0), color="green", fontsize=40)


def update(delta):
    global playing, distance, time
    if playing:
        wall_collisions()
        scroll_walls()
        generate_lines()
        player_input()
        timer(delta)
    elif keyboard.space:
        playing = True
        distance = 0
        time = 10


def player_input():
    pass

def generate_lines():
    pass

generate_lines()

def scroll_walls():
    pass

def wall_collisions():
    pass

def timer(delta):
    pass

def on_mouse_move(pos):
    pass
```

### Übungsaufgabe
+ Führe das Programm aus. Prüfe, ob es einen Startbildschirm hat und ob du das Spiel starten kannst und den Spieler siehst. (Das ist alles, was es tut bis wir die restlichen Funktionen füllen.)

## Spielereingabe

Ersetze `player_input()` durch Folgendes:

```python
def player_input():
    if keyboard.up:
        player.vy += 0.1
    if keyboard.down:
        player.vy -= 0.1
        if player.vy < 1:
            player.vy = 1
    if keyboard.right:
        player.vx += 0.4
    if keyboard.left:
        player.vx -= 0.4
    player.x += player.vx
```
### Übungsaufgabe
+ Führe das Spiel aus. Prüfe, ob der Spieler sich nach links und rechts bewegen kann und Schwund hat. Versuche die Geschwindigkeit anzupassen oder setze eine Limit, damit es sich nicht zu schnell bewegt. 


## Generiere die Wände

Wir haben bereit den Code um die Wände aufs Spielfeld zu bringen, aber `lines` ist leer, daher passiert momentan nichts. Ersetze die Funktion `generate_lines()` mit nachfolgendem Code.  
Beachte, dass `generate_lines()` sofort abgerufen wird, nachdem wir die Funktion definiert haben, damit die Wände auch gleich zu Beginn des Spiel erscheinen. 

```python
def generate_lines():
    global wall_gradient, left_wall_x
    gap_width = 300 + math.sin(distance / 3000) * 100
    while len(lines) < HEIGHT:
        pretty_colour = (255, 0, 0)
        lines.insert(0, (left_wall_x, gap_width, pretty_colour))
        left_wall_x += wall_gradient
        if left_wall_x < 0:
            left_wall_x = 0
            wall_gradient = random.random() * 2 + 0.1
        elif left_wall_x + gap_width > WIDTH:
            left_wall_x = WIDTH - gap_width
            wall_gradient = -random.random() * 2 - 0.1

generate_lines()
```

### Übungsaufgabe
+ Ändere die Farbe der Wände von rot auf grün.



## Mach die Wände bunt

Ändere die Zeile, die die Farbe festsetzt, zu Folgendem: 

```python
        pretty_colour = (255, min(left_wall_x, 255), min(time * 20, 255))
```

## Scrolling 

Ändere die `scroll_walls()` Funktion, damit sie die Linien am Fuß des Bildschirms, passend zu den vertikalten Bewegungen des Spieler, entfernt. 

```python
def scroll_walls():
    global distance
    for i in range(0, int(player.vy)):
        lines.pop()
        distance += 1
```

### Übungsaufgabe
+ Ändere `scroll_walls()` wie oben genannt und prüfe, ob der Spieler jetzt beschleunigen kann.
+ (Für Fortgeschrittene):
Ändere den Beschleunigungswert um das Spiel schneller oder langsamer zu machen.


## Wandkollision

Zurzeit kann der Spieler noch durch die Wände fahren, was jedoch nicht Sinn der Sache ist. Außerdem wollen wir, dass der Spieler als Strafe an Geschwindigkeit verliert, wenn er mit der Wand kollidiert.

```python
def wall_collisions():
    a, b, c = lines[-1]
    if player.x < a:
        player.x += 5
        player.vx = player.vx * -0.5
        player.vy = 0
    if player.x > a + b:
        player.x -= 5
        player.vx = player.vx * -0.5
        player.vy = 0
```
### Übungsaufgabe
+ Ändere `wall_collisions()` wie oben und prüfe, ob der Spieler jetzt von der Wand abprallt. 
+ (Für Fortgeschrittene):
Lass den Spieler stärker von der Wand abprallen.

## Timer

Im Moment hat der Spieler noch unendlich viel Zeit. Wir wollen, dass die `time` Variable zeigt, wie viel Zeit vergangen ist und dass das Spiel endet, wenn die Zeit um ist. 

```python
def timer(delta):
    global time, playing, best_distance
    time -= delta
    if time < 0:
        playing = False
        if distance > best_distance:
            best_distance = distance
```

### Übungsaufgabe
+ Prüfe, ob das Spiel nach 15 Sekunden endet.
+ Lass das Spiel 30 Sekunden dauern.


## Mausbewegung

Das Spiel ist leichter, aber vielleicht auch unterhaltsamer, wenn man es mit der Maus spielen kann. Pygame ruft die folgende Funktion automatisch ab. 

```python
def on_mouse_move(pos):
    x, y = pos
    player.x = x
    player.vy = (HEIGHT - y) / 20
```
### Übungsaufgabe

+ Wie beschleunigt der Spieler mit der Maus?



## Ideen für Erweiterungen

* Zeichne ein neues Bild für den Spieler. Lasse unterschiedliche Bilder anzeigen, je nachdem ob der Spieler nach links oder rechts steuert. 
* Gib eine Ziel-Distanz vor, die erreicht werden muss. Wenn der Spieler die Distanz erreicht, bekommt er mehr Zeit, die ihn weiterspielen lässt. 
* Füge Musik und Soundeffekte hinzu. 
* Wenn du einen großen Bildschirm hast, mach das Spielfenster größer (und stell sicher, dass das Alien immer noch am unteren Bildschirmrand erscheint). 