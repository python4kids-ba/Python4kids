# Mehr Erweiterungen

Die nachfolgenden Beispiele zeigen weitere wichtige Bausteine, die für fortgeschrittenere Spiele benötigt werden.



## Listen

Listen sind, wie der Name schon verrät, eine Auflistung von Werten. Die Werte einer Liste werden in eckigen Klammern geschrieben, es gibt auch leere Listen. 
Eine solche leere Liste `[]` wird für das nächste Beispiel benötigt. Mit Hilfe einer Schleife werden Alien-Sprites schließlich dieser Liste hinzugefügt. Mit jedem Mausklick wird ein weiteres Alien hinzugefügt. 

```python=
WIDTH = 500
HEIGHT = 500

aliens = []
for i in range(0,10):
    aliens.append(Actor('alien', (i*30, i*30)))

def draw():
    screen.clear()
    for alien in aliens:
        alien.draw()

def update():
    for alien in aliens:
        alien.x += 2
        if alien.x > WIDTH:
            alien.x = 0

def on_mouse_down(pos, button):
    aliens.append(Actor('alien', pos))
```

Während die Aliens, die bereits zu Anfang in der Liste stehen, alle in einer Reihenfolge auf dem Bildschirm erscheinen, werden die neuen genau da platziert, wo der Mausklick war. Zu sehen in der nachfolgenden Beispielausgabe:

![](https://i.imgur.com/rs5E7um.png)



### Übungsaufgaben
+ (Für Fortgeschrittene):
Geh zurück zu einem der früheren Spiele und füge eine Liste mit Kugeln hinzu, die sich auf dem Bildschirm nach oben bewegen. Wenn der Spieler die Leertaste drückt, wird eine weitere Kugel der Liste hinzugefügt. 



## Animation

Es gibt natürlich die Möglichkeit das Alien zu animieren, um dem Spiel damit noch mehr Leben einzuhauchen. Die Beispiel-Animation ist dabei jedoch leicht eingeschränkt, da nur zwei Bilder zur Verfügung stehen, zur Veranschaulichung reicht das jedoch aus. 
Die Animation wird hier in einem zeitlichen Intervall ausgeführt und würde sich in einem voll ausgearbeiteten Spiel eher für Hintergrundcharaktere, die sich automatisch bewegen sollen, eignen. Eine weitere Möglichkeit wäre auch für Endscreens, die den Charakter, zum Beispiel, tanzend zeigen bei einem Sieg oder auch weinend bei einer Niederlage.


```python=
alien = Actor("alien")
alien.pos = (200, 200)

def draw():
    screen.clear()
    alien.draw()

def update():
    if keyboard.right:
        alien.x = alien.x + 2
    elif keyboard.left:
        alien.x = alien.x - 2

images = ["alien_hurt", "alien"]
image_counter = 0

def animateAlien():
    global image_counter
    alien.image = images[image_counter % len(images)]
    image_counter += 1


clock.schedule_interval(animateAlien, 0.2)
```

Eine Animation lässt sich jedoch auch durch einen Tastendruck auslösen. Das Alien ist sehr statisch und „schwebt“ eher über das Spielfeld anstatt zu laufen, da es keine Beinbewegung gibt. Hat man jedoch Bilder dafür, kann man es mit ein paar Änderungen an dem obigen Code so aussehen lassen, als würde der Spieler-Sprite wirklich gehen.

![](https://i.imgur.com/Dqr1iQF.png)
> Quelle: RPG Maker VX Ace


### Übungsaufgaben

+ Mach die Animation schneller.
+ (Für Fortgeschrittene):
Füge ein weiteres Bild zur Animation hinzu.
+ (Für Fortgeschrittene):
Zeichne deine eigene Animation, z.B. eine Figur, die nach links geht, wenn die entsprechende Taste dafür gedrückt wird. 




## Einfache Physik

Für dieses Beispiel zeichnen wir einen Ball und bewegen ihn, doch anstatt einer festgelegten Anzahl an Pixeln, hinterlegen wir eine Zahl in den Variablen `vx` und `vy`. Hierbei handelt es sich um *velocities*, oder auf Deutsch *Geschwindigkeiten*. Damit ist jedoch nicht nur ein Tempo gemeint, sondern auch die Bewegung an sich. `vx` ist die Geschwindigkeit in die horizontale Richtung und `vy` die Geschwindigkeit in die vertikale Richtung. Hier vertauschen wir die velocity, wenn der Ball auf den Rand des Bildschirms trifft.
  
```python=
WIDTH = 500
HEIGHT = 500

ball = Rect((200, 400), (20, 20))
vx = 1
vy = 1

def draw():
    screen.clear()
    screen.draw.filled_rect(ball, "red")

def update():
    global vx, vy
    ball.x += vx
    ball.y += vy
    if ball.right > WIDTH or ball.left < 0:
        vx = -vx
    if ball.bottom > HEIGHT or ball.top < 0:
        vy = -vy
```


### Übungsaufgaben
+ (Für Fortgeschrittene):
Mach, dass der Ball sich schneller bewegt in dem du seine Geschwindigkeit erhöhst, sobald er auf den Bildschirmrand trifft. 



## Bat and Ball Spiel

*Pong* ist der Klassiker unter den Bat and Ball Spielen.

```python=
WIDTH = 500
HEIGHT = 500

ball = Rect((150, 400), (20, 20))
bat = Rect((200, 480), (60, 20))
vx = 4
vy = 4

def draw():
    screen.clear()
    screen.draw.filled_rect(ball, "red")
    screen.draw.filled_rect(bat, "white")

def update():
    global vx, vy
    ball.x += vx
    ball.y += vy
    if ball.right > WIDTH or ball.left < 0:
        vx = -vx
    if ball.colliderect(bat) or ball.top < 0:
        vy = -vy
    if ball.bottom > HEIGHT:
        exit()
    if(keyboard.right):
        bat.x += 2
    elif(keyboard.left):
        bat.x -= 2
```

![](https://i.imgur.com/b22KRoP.png)


### Übungsaufgaben

+ Mach, dass der Ball sich schneller bewegt.
+ (Für Fortgeschrittene):
Füge einen zweiten Bat am oberen Rand des Bildschirms für Spieler 2 hinzuAdd another bat at the top of the screen for player 2.
+ (Für Fortgeschrittene):
Füge Steine (Rects) hinzu, die verschwinden, wenn der Ball sie trifft. 



## Timer

Die Funktionen `update()` und `draw()` werden von Pygame mehrmals pro Sekunde aufgerufen. Jedes Mal, wenn `draw()` aufgerufen wird, wird ein neuer *Frame* gezeichnet. Die exakte Anzahl der Frames pro Sekunde wird *Framerate* genannt, die sich jedoch von Computer zu Computer unterscheidet. Deshalb ist das Zählen von Frames nicht der zuverlässigste Weg die Zeit anzuzeigen.

Glücklicherweise bietet Pygame die Möglichkeit uns genau anzuzeigen, wie viele Sekunden seit dem letzten Frame in einem Parameter vergangen sind. Wir nutzen diese *delta time* um einen Timer anzuzeigen.


```python=
timer = 0

def update(dt):
    global timer
    timer += dt


def draw():
    screen.clear()
    screen.draw.text(f"Time passed: {timer}", (0, 0))
    if timer > 5:
        screen.draw.textbox("Time's up!", Rect(50, 50, 200, 200))
```


### Übungsaufgaben

+ Mach, dass der Timer nach unten zählt statt nach oben.
+ (Für Fortgeschrittene):
Füge einen Timer zu einem deiner Spiele hinzu.
+ (Für Fortgeschrittene):
Add a timer to Program~\ref{code:arrays} that deletes one of the aliens when the timer runs out, then starts the timer again.



## Callbacks: eine andere Art des Timers

Pygame hat eine eigene Uhr, die wir nach einem *Callback* für eine unsere Funktionen fragen können. Entweder zu einer bestimmten Zeit oder in einem regelmäßigen Intervall. In diesem Beispiel wird nach 0,5 Sekunden ein Alien auf dem Bildschirm dargestellt, anschließend wird alle zwei Sekunden ein Weiteres hinzugefügt. 


```python=
import random

aliens = []

def add_alien():
    aliens.append(
        Actor("alien", (random.randint(0,500), random.randint(0,500)))
    )

# rufe add_alien einmal, in 0.5 seconds von jetzt an, auf
clock.schedule(add_alien, 0.5)

# rufe add_alien immer wieder alle 2 Sekunden auf
clock.schedule_interval(add_alien, 2.0)

def draw():
    screen.clear()
    for alien in aliens:
        alien.draw()
```

### Übungsaufgaben 

+ Mach, dass das Alien öfter erscheint.
+ (Für Fortgeschrittene):
Nutze ```len(aliens)``` um auszugeben, wie viele Aliens da sind.
+ (Für Fortgeschrittene):
Wenn zu viele Aliens da sind, füge keine weiteren hinzu, indem du folgenden Code nutzt: 
    ```python=
           clock.unschedule(add_alien)
    ```