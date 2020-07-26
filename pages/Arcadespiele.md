# Arcade-Spiele

## Kollisionen

Für ein Spiel ist es auch wichtig, dass es registriert, wenn ein Sprite auf einen anderen trifft, mit diesem also kollidiert. 

```python=
WIDTH = 500
HEIGHT = 500

alien = Actor("alien")
alien.pos = (400, 50)
box = Rect((20, 20), (100, 100))

def draw():
    screen.clear()
    screen.draw.filled_rect(box, "red")
    alien.draw()

def update():
    if keyboard.right:
        alien.x = alien.x + 2
    elif keyboard.left:
        alien.x = alien.x - 2
    box.x = box.x + 2
    if box.x > WIDTH:
        box.x = 0
    if alien.colliderect(box):
        print("hit")
```

Die Ausgabe dazu sieht dann so aus:

![](https://i.imgur.com/2Lp0ZAY.png)


### Übungsaufgaben

+ Füge vertikale Bewegungen hinzu.
+ (Für Fortgeschrittene): 
Lass die Box den Alien jagen.
+ (Für Fortgeschrittene): 
Gib an, wie oft die Box den Alien getroffen hat (sprich den Score).




## Verfolgungsjagd / Game Over

Anstatt, dass sich die Box konstant nach rechts bewegt, können wir mit einem `if` Statement dafür sorgen, dass die Box das Alien verfolgt. Wir ändern auch, was passiert, wenn die Box das Alien erwischt: das Spiel wird beendet und muss neu gestartet werden, es heißt also Game Over. Nicht unbedingt, was ein Spieler möchte. 

```python=
WIDTH = 500
HEIGHT = 500

alien = Actor("alien")
alien.pos = (400, 50)
box = Rect((20, 20), (100, 100))

def draw():
    screen.clear()
    screen.draw.filled_rect(box, "red")
    alien.draw()

def update():
    if keyboard.right:
        alien.x = alien.x + 2
    elif keyboard.left:
        alien.x = alien.x - 2
    if box.x < alien.x:
        box.x = box.x + 1
    if box.x > alien.x:
        box.x = box.x - 1
    if alien.colliderect(box):
        exit()
```



### Übungsaufgaben
+ Füge vertikale Bewegungen hinzu.
+ (Für Fortgeschrittene): 
Zeichne ein neues Gegner-Bild und speichere es als `enemy.png` im `mu_code/images` Ordner. Füge es als `Actor('enemy')` anstatt des `Rect()` ein.




## Powerup

Für dieses Beispiel wird aus dem Gegner eine Power-Up-Box, die der Spieler aufsammeln muss. Die Box verschwindet nach dem Aufsammeln, taucht aber an einer anderen Stelle wieder auf. 

```python=
WIDTH = 500
HEIGHT = 500
import random

alien = Actor("alien")
alien.pos = (400, 50)
box = Rect((20, 20), (100, 100))
score = 0

def draw():
    screen.clear()
    screen.draw.filled_rect(box, "green")
    alien.draw()

def update():
    global score
    if keyboard.right:
        alien.x = alien.x + 2
    elif keyboard.left:
        alien.x = alien.x - 2
    if alien.colliderect(box):
        box.x = random.randint(0, WIDTH)
        box.y = random.randint(0, HEIGHT)
        score = score + 1
        print("Score:", score)
```

Ausgabe, wenn bereits ein Power-Up aufgesammelt wurde:

![](https://i.imgur.com/t0QkJVX.png)


### Übungsaufgaben

+ Füge vertikale Bewegungen hinzu.
+ Zeichne ein neues Power-Up Bild und speichere es als `powerup.png` im `mu_code/images` Ordner. Ersetze `Rect()` durch `Actor('powerup')`.
+ (Für Fortgeschrittene):
Kombiniere dieses Programm mit einem Gegner und füge auch einen Hintergrund hinzu (oder was auch immer du möchtest, um dein eigenes Spiel zu machen).



## Sound and Animationen

Neben Bewegungen und Gegnern, geben Musik bzw. Sounds und Animationen einem Spiel noch das gewisse Extra. Es gibt im Images-Ordner ein weiteres Beispiel-Sprite: `alien_hurt.png`. Dazu gibt es die Sounddatei `eep.wav`. Wenn das Alien also getroffen wird, ändert sich das Sprite und die Sounddatei wird abgespielt.


```python=
WIDTH = 500
HEIGHT = 500
alien = Actor("alien")
alien.pos = (0, 50)
box = Rect((20, 20), (100, 100))

def draw():
    screen.clear()
    screen.draw.filled_rect(box, "red")
    alien.draw()
def update():
    if keyboard.right:
        alien.x = alien.x + 2
    elif keyboard.left:
        alien.x = alien.x - 2
    box.x = box.x + 2
    if box.x > WIDTH:
        box.x = 0

# PLAY SOUND AND SHOW IMAGE WHEN HIT
    if alien.colliderect(box):
        alien.image = 'alien_hurt'
        sounds.eep.play()
    else:
        alien.image = 'alien'
```


### Übungsaufgaben
+ Nimm deinen eigenen Soundeffekt auf und füge ihn dem Spiel hinzu.
+ (Für Fortgeschrittene):
Füge more Boxen oder Sprites hinzu, die sich in unterschiedliche Richtungen bewegen, denen der Spieler ausweichen muss.
+ (Für Fortgeschrittene):
Füge einen zweiten Alien hinzu, der von einem zweiten Spieler über andere Tasten oder ein Gamepad gesteuert werden kann.


## Mausklicks

Mit Hilfe der Maus lässt sich ebenfalls ein kleines Multiplayer-Spiel daraus machen. Ein Spieler kontrolliert das Alien, während ein zweiter Spieler versucht mit der Maus auf das Alien zu klicken. Der nachfolgende Code zeigt eine mögliche Umsetzung. Bei einem Klick wird der Sound abgespielt und das Bild des Aliens wechselt zu alien_hurt, auch der Score geht bei jedem Treffer um eins nach oben.

  
```python=
alien = Actor("alien")
alien.pos = (0, 50)
score = 0

def draw():
    screen.clear()
    alien.draw()
    screen.draw.text("Score "+str(score), (0,0))

def update():
    if keyboard.right:
        alien.x = alien.x + 2
        alien.image = 'alien'
    elif keyboard.left:
        alien.x = alien.x - 2
        alien.image = 'alien'

def on_mouse_down(pos, button):
    global score
    if button == mouse.LEFT and alien.collidepoint(pos):
        alien.image = 'alien_hurt'
        sounds.eep.play()
        score = score + 1
```

Nach einigen Klicks auf das Alien würde die Ausgabe dann z.B. so aussehen:

![](https://i.imgur.com/z4cjirg.png)



## Mausbewegung

```python=
# wiggle your mouse around the screen!

alien = Actor("alien")

def draw():
    screen.clear()
    alien.draw()

def on_mouse_move(pos):
    alien.pos = pos
```
### Übungsaufgaben


+ Was passiert, wenn du Zeile 10 löschst und durch Folgendes ersetzt:
```python
     animate(alien, pos=pos, duration=1, tween='bounce_end')
```
+ Was passiert, wenn du `on_mouse_move` zu `on_mouse_down` änderst?
+ (Für Fortgeschrittene):
Programmiere ein Spiel bei dem ein Alien über die Maus und ein anderes über die Tastatur kontrolliert werden kann. 






