# Tutorial: Verfolgungsjagd

In diesem Kapitel programmieren wir, Schritt für Schritt, ein Spiel für eine Verfolgungsjagd. Der verwendete Python-Code ist dabei sehr einfach: nur Bedingungen und Schleifen. 




## Spielfigur über einen Hintergrund bewegen

Wir brauchen zwei Bilddateien für unser Spiel. Du kannst ein Programm wie Krita[^krita_footnote] benutzen, um sie zu zeichnen. Anschließend werden sie im Ordner `mu_code/images` (erreichbar über den `Bilder` Button in Mu) gespeichert. Eins sollte den Spieler darstellen und `player.png` genannt werden. Für die Größe sind 64x64 Pixel zu empfehlen und idealerweise sollte es einen transparenten Hintergrund haben. 

[^krita_footnote]: https://krita.org

Das andere Bild soll als Hintergrund für das Spiel dienen. Es kann aussehen, wie immer du willst, aber es sollte dieselbe Größe wie das Spielfenster haben. In diesem Fall also 600×600 Pixels.

![New image in Krita, 600×600 pixels\label{fig:krita](https://i.imgur.com/SzPiCuv.png)


```python=
WIDTH = 600
HEIGHT = 600

background = Actor("background")
player = Actor("player")
player.x = 200
player.y = 200

def draw():
    screen.clear()
    background.draw()
    player.draw()

def update():
    if keyboard.right:
        player.x = player.x + 4
    if keyboard.left:
        player.x = player.x - 4
    if keyboard.down:
        player.y = player.y + 4
    if keyboard.up:
        player.y = player.y - 4
```

### Übungsaufgabe

+ Lass das Programm laufen und bewege die Spielfigur mit den Tasten.
 
## Bildschirmbegrenzung

Ein Problem, das das Spiel momentan hat, ist, dass sich die Spielfigur aus dem Bildschirm bewegen kann und damit verschwindet. Eine Möglichkeit wäre es Bewegungen am Rand zu stoppen. 
Für dieses Tutorial jedoch sorgen wir dafür, dass die Spielfigur wieder zum gegenüberliegenden Rand teleportiert wird, wenn sie das Spielfeld verlässt. Füge den folgenden Code ans Ende des bisherigen Programms hinzu und achte darauf, dass er eingerückt ist, damit er Teil der `update()` Funktion wird.

```python
    if player.x > WIDTH:
        player.x = 0
    if player.x < 0:
        player.x = WIDTH
    if player.y < 0:
        player.y = HEIGHT
    if player.y > HEIGHT:
        player.y = 0
```

### Übungsaufgabe

+ Ändere Code, sodass die Spielfigur am Rand stoppt anstatt teleportiert zu werden.


## Gegner verfolgt den Spieler

Um einen Gegner ins Spiel zu bringen, der den Spieler jagt, fügen wir am Anfang des Programms eine Variable für den Gegner ein: 

```python
enemy = Actor("alien")
```

Am Ende der `draw()` Funktion (aber eingerückt, damit es noch Teil der Funktion ist), fügen wir den Gegner ein.  
Wichtig: es gibt immer nur eine `draw()` Funktion pro Programm! 

Nachfolgend die komplette Funktion inklusive der neuen Zeile:

```python
def draw():
    screen.clear()
    background.draw()
    player.draw()
    enemy.draw()
```


Am Ende der `update()` Funktion (eingerückt, als Teil der Funktion),fügen wir die folgenden Zeilen an, damit der Gegner sich auch bewegt:

```python
    if enemy.x < player.x:
        enemy.x = enemy.x + 1
    if enemy.x > player.x:
        enemy.x = enemy.x - 1
    if enemy.y < player.y:
        enemy.y = enemy.y + 1
    if enemy.y > player.y:
        enemy.y = enemy.y - 1
    if player.colliderect(enemy):
        exit()
```

### Übungsaufgabe

+ Teste das Programm, um zu sehen, ob der Gegner auch den Spieler jagt.
+ Erhöhe die Geschwindigkeit des Gegners für einen schwierigeres Spiel.
+ Erstelle eine Bilddatei namens `enemy.png` und speichere es im `images` Ordner. Ändere den Code, sodass `"enemy"` anstatt `"alien"` geladen wird.



## Items sammeln

Erstelle eine kleine Bilddatei namens `coin.png` und speichere sie im `images` Ordner. Es sollte wie eine Münze oder etwas anderes, das eingesammelt werden soll, aussehen. Wir brauchen auch eine Variable, um den *score* zu speichern, sprich, wie viele Münzen aufgesammelt wurden. 

```python
coin = Actor("coin", pos=(300,300))
score = 0
```

Den Aufruf für die Coin fügen wir, wie die anderen zuvor, in die `draw()` Funktion ein.

Nachfolgend die komplette Funktion, inklusive der neuen Zeile am Ende:

```python
def draw():
    screen.clear()
    background.draw()
    player.draw()
    enemy.draw()
    coin.draw()
```

Damit die Münze nach dem Aufsammeln an einer anderen Stelle wieder auftaucht, müssen wir die nachfolgenden Zeilen als Teil der `update()` Funktion einfügen: 

```python
    if coin.colliderect(player):
        coin.x = random.randint(0, WIDTH)
        coin.y = random.randint(0, HEIGHT)
        score = score + 1
        print("Score:", score)
```
### Übungsaufgabe

+ Lass das Programm laufen und sammle eine Münze auf. Was passiert?


```
NameError: name 'random' is not defined
```

Diese Fehlermeldung taucht auf, weil wir `randint()` benutzen um eine zufällige Zahl zu bekommen. Diese Funktion ist aber nicht automatisch in Python enthalten, sondern Teil der *random* Bibliothek. Daher müssen wir am Anfang des Programms folgende Zeile einfügen und damit das Modul laden:  

```python
import random
```
### Übungsaufgabe

+ Lass das Programm nochmal laufen und sammle eine Münze. Funktioniert es jetzt?


Nein!

```
UnboundLocalError: local variable 'score' referenced before assignment
```

Der Grund für diese Fehlermeldung ist, dass es sich bei `score` um eine globale Variable handelt und wir versuchen, sie innerhalb der `update()` Funktion zu verändern. Daher müssen wir **ganz oben** von der `update()` Funktion noch folgende Zeile einfügen, damit Python weiß, dass wir vorhaben eine globale Variable zu verändern:

```python
global score
```

Es muss die **erste** Zeile der Funktion und **eingerückt** sein. Es sollte folgendermaßen aussehen: 

```python
def update():
    global score
    if keyboard.right:
```
### Übungsaufgabe

+ Führe das Programm noch einmal aus, um zu testen, ob es jetzt funktioniert!


## Spieler 2

Das Spiel lässt sich auch in ein Muliplayer-Spiel verwandeln. Dazu müssen wir ganz oben im Programm eine weitere Variable für die Spielfigur des zweiten Spielers einfügen: 

```python
player2 = Actor("alien")
```

Wie auch zuvor, wird Spieler 2 anschließend in die `draw()` Funktion eingefügt. Nachfolgend wieder die komplette Funktion mit der neuen Zeile: 

```python
def draw():
    screen.clear()
    background.draw()
    player.draw()
    enemy.draw()
    coin.draw()
    player2.draw()
```

Damit Spieler 2 seine Figur auch bewegen kann, müssen folgende Zeilen am Ende der `update()` Funktion eingefügt werden:

```python
    if keyboard.d:
        player2.x = player2.x + 4
    if keyboard.a:
        player2.x = player2.x - 4
    if keyboard.s:
        player2.y = player2.y + 4
    if keyboard.w:
        player2.y = player2.y - 4
    if player.colliderect(player2):
        exit()
```

### Übungsaufgabe
+ (Für Fortgeschrittene):
Schreibe eine Variable `score2`, die die Punktzahl für gesammelte Münzen von Spieler 2 speichert. 


## Score auf dem Bildschirm anzeigen

Damit der Score auf dem Bildschirm angezeigt wird, fügen wir folgende Zeilen zur `draw()` Funktion hinzu: 

```python
    screen.draw.text("My game", (200,0), color='red')
```

`draw.text()` ist jedoch anders als `print()`, da es nur strings (also Text) ausgeben kann, aber keine Zahlen. Deshalb müssen wir den Score in einen string umwandeln. Dafür fügen wir die folgenden Zeilen ans Ende der `draw()` Funktion:
```python
    score_string = str(score)
    screen.draw.text(score_string, (0,0), color='green')
```

### Übungsaufgabe
+ Ändere die Farbe des Textes.
+ (Für Fortgeschrittene):
Lasse das Wort "Score: " vor der Punktezahl anzeigen.
+ (Für Fortgeschrittene):
Wenn der `score` 10 erreicht, lasse eine Nachricht auf dem Bildschirm anzeigen, die dem Spieler gratuliert.


## Timer

Füge eine Variable am Anfang des Programms ein (bevorzugt nach den `import` Angaben), um die verbleibende Spielzeit in Sekunden anzuzeigen. 

```python
time = 20
```

Die `update()` Funktion wird von Pygame Zero mehrmals pro Sekunde aufgerufen. Wir können abfragen, wie viel Zeit vergangen ist, indem wir einen Parameter, `delta`, zur Funktion hinzufügen. Anschließend ziehen wir das von der verbleibenden Zeit ab. Ändere `update()` damit die ersten Zeilen so aussehen: 

```python
def update(delta):
    global score, time
    time = time - delta
    if time <= 0:
         exit()
```

Wir können die Zeit auch auf dem Bildschirm anzeigen lassen. Dafür müssen wir die folgenden Zeilen am Ende der `draw()` Funktion einügen: 

```python
    time_string = str(time)
    screen.draw.text(time_string, (50,0), color='green')

```

### Übungsaufgabe

+ Führe das Programm aus. Kann die Anzeige noch verbessert werden?


Wir brauchen keine Dezimalanzeige, daher benutzen wir die `round()` Funktion, um die Anzeige zu runden:

```python
    time_string = str(round(time))
    screen.draw.text(time_string, (50,0), color='green')

```

## Das fertige Spiel

Nachfolgend das fertige Spiel, inklusive aller Änderungen:

```python=
import random

WIDTH = 600
HEIGHT = 600

background = Actor("background")
player = Actor("player")
player.x = 200
player.y = 200

enemy = Actor("alien")
player2 = Actor("player")
coin = Actor("alien", pos=(300,300))
score = 0
time = 20


def draw():
    screen.clear()
    background.draw()
    player.draw()
    enemy.draw()
    player2.draw()
    coin.draw()
    screen.draw.text("My game", (200,0), color='red')
    score_string = str(score)
    screen.draw.text(score_string, (0,0), color='green')
    time_string = str(round(time))
    screen.draw.text(time_string, (50,0), color='green')

def update(delta):
    global score, time
    time = time - delta
    if time <= 0:
        exit()
    if keyboard.right:
        player.x = player.x + 4
    if keyboard.left:
        player.x = player.x - 4
    if keyboard.down:
        player.y = player.y + 4
    if keyboard.up:
        player.y = player.y - 4

    if player.x > WIDTH:
        player.x = 0
    if player.x < 0:
        player.x = WIDTH
    if player.y < 0:
        player.y = HEIGHT
    if player.y > HEIGHT:
        player.y = 0

    if enemy.x < player.x:
        enemy.x = enemy.x + 1
    if enemy.x > player.x:
        enemy.x = enemy.x - 1
    if enemy.y < player.y:
        enemy.y = enemy.y + 1
    if enemy.y > player.y:
        enemy.y = enemy.y - 1
    if player.colliderect(enemy):
        exit()

    if keyboard.d:
        player2.x = player2.x + 4
    if keyboard.a:
        player2.x = player2.x - 4
    if keyboard.s:
        player2.y = player2.y + 4
    if keyboard.w:
        player2.y = player2.y - 4
    if player.colliderect(player2):
        exit()

    if coin.colliderect(player):
        coin.x = random.randint(0, WIDTH)
        coin.y = random.randint(0, HEIGHT)
        score = score + 1
        print("Score:", score)



```


## Ideen zur Erweiterung

Es gibt noch einige Dinge, die man zum Spiel hinzufügen könnte: 

* Mehr Gegner.
* Gib dem Spieler drei Leben.
* Musik und Soundeffekte.
* Kreiere ein Power-Up durch das sich der Spieler schneller bewegt.
* Mach die Gegner intelligenter.
* Lass den Spieler die Gegner töten.