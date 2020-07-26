# Tutorial: Alienjagd

In diesem Tutorial programmieren wir Schritt für Schritt ein Spiel in dem wir Punkte für das Klicken auf Aliens bekommen. Je nach Farbe erhält man unterschiedlich hohe Punkte. Dafür brauchen wir if- und else-Bedingungen, sowie Schleifen. Angelehnt ist dieses Tutorial an das Spiel "Alarmstufe Rot" aus dem Buch "Spiele mit Python supereasy programmieren" von Carol Vorderman u.a.

## Spielfiguren

Für dieses Spiel brauchen wir mehrere Aliens in unterschiedlichen Farben. Dafür können wir einfach das bereits vorhandene Alien nehmen und mit einer Bildbearbeitungssoftware anders einfärben. Anschließend werden sie im Ordner `mu_code/images` (erreichbar über den `Bilder` Button in Mu) gespeichert.

![](https://i.imgur.com/DaKNYNS.png)

## Random importieren

Zu Beginn importieren wir das Python-Modul `random`. Das brauchen wir, damit später die Aliens in zufälligen Farben auf dem Spielfeld erscheinen.

```python=
import random
```

## Konstanten und globale Variablen festlegen

Hier legen wir die Anfangsvariablen fest. Darunter Konstanten, die sich das ganze Spiel über nicht verändern, wie die Spielfeldgröße und die globalen Variablen, die sich im Laufe des Spiels verändern, wie das Level.

```python=
WIDTH = 900
HEIGHT = 600
START_SPEED = 10 #Startgeschwindigkeit in der sich die Aliens bewegen
FARBEN = ["gruen", "rot", "lila", "blau"] 
Endlevel = 9 #nach 9 Level wird das Spiel beendet
game_over = False #prüft, ob Game Over erreicht wurde
game_complete = False #prüft, ob das Endlevel erreicht wurde
aktuelles_level = 1 #momentanes Level auf dem sich der Spieler befindet
aliens = []
animations = [] #diese Listen speichern die Anzahl der Aliens
score = 0
```

## Aliens zeichnen

Mit der Funktion `draw()` fügen wir die Aliens ein und lassen die "Game Over" Nachricht anzeigen, wenn das Spiel beendet wurde.

```python=
def draw():
    global aliens, aktuelles_level, game_over, game_complete
    screen.clear()
    screen.draw.text(f"Score: {score}", (0, 0))
    if game_over:
        screen.draw.textbox("Game Over!", Rect(350, 150, 200, 200))
    elif game_complete:
        screen.draw.textbox(f"Geschafft! Dein Score ist: {score}", (350, 150, 200, 200))
    else:
        for alien in aliens:
            alien.draw()
```

## Prüfen, ob bereits Aliens in der Liste sind

Mit der `update()` Funktion können wir prüfen, ob die Liste "aliens" bereits Aliens enthält. Wenn nicht, soll die Funktion `erstelle_aliens()` aufgerufen werden, die die Aliens erstellt.

```python=
def update():
    global aliens
    if len(aliens) == 0:
        aliens = erstelle_aliens(aktuelles_level)
```

## Aliens erstellen

Die folgende Funktion erstellt die Aliens und gibt die Farben aus, in denen die Aliens dargestellt werden können (also rot, grün, blau und lila). Anschließend wird aus der Liste der Farben für jedes Alien ein Actor erstellt. Damit müssen wir das nicht einzeln für jedes Alien erledigen. Die Funktion platziert außerdem die Aliens auf dem Spielbildschirm und sorgt dafür, dass sie sich nach unten bewegen.

```python=
def erstelle_aliens(alienzahl):
    create_farbe = farbwahl(alienanzahl)
    neue_aliens = erschaffe_aliens(create_farbe)
    layout_aliens(neue_aliens)
    animate_aliens(neue_aliens)
    return neue_aliens
```

## Ein erster Test

Um das Spiel zu testen, geben wir für die Funktionen erst einmal Platzhalter ein. Funktionen müssen immer definiert sein, damit ein Programm läuft, doch für Testzwecke reicht es auch erst einmal nur pass und return [] als Platzhalter einzugeben.

```python=
def farbwahl(alienanzahl):
    return []
    
def erschaffe_aliens(create_farbe):
    return []
    
def layout_aliens(alien_layout):
    pass
    
def animate_aliens(animate_aliens):
    pass
```

Es sollte erst einmal nur ein schwarzer Hintergrund ohne Aliens ausgegeben werden. Sollte dir irgendwo ein Fehler unterlaufen sein, solltest du hier eine Rückmeldung bekommen.

## Liste der Farben

Die Aliens im Spiel haben die Farben rot, grün, blau und lila. In den Variablen haben wir diese Farben bereits angegeben. Mit Hilfe einer Schleife werden die Farben zufällig in die Liste eingefügt.
Füge den folgenden Code unter `def farbwahl(alienanzahl):` ein:

```python=
    create_farbe = []
    for i in range(0, alienanzahl):
        random_farbe = random.choice(FARBEN)
        create_farbe.append(random_farbe)
    return farbe_erzeugen
```

## Aliens einfügen

Als nächstes brauchen wir die Aliens. Dafür legen wir eine neue Liste namens `neue_aliens` an. Mit Hilfe einer Schleife wird je ein neues Alien in einer zufälligen Farbe erschaffen und in die Liste eingefügt.
Gib folgenden Code unter `def erschaffe_aliens(create_farbe):` ein:

```python=
    neue_aliens = []
    for farbe in create_farbe:
        alien = Actor(farbe)
        neue_aliens.append(alien)
    return neue_aliens
```

## Ein zweiter Test

Zeit für einen zweiten Testdurchlauf. Der Spielbildschirm sollte jetzt so aussehen:

![](https://i.imgur.com/KvGlbgg.png)

Noch sitzen alle Aliens in der linken oberen Ecke, aber das ändern wir im nächsten Schritt.

## Positionierung

Für die Positionierung müssen wir berechnen, wie viele Lücken zwischen den Aliens sein müssen. Dafür teilen wir die Breite des Fensters durch die Zahl der Lücken. Bei zwei Aliens, gibt es drei Lücken. Damit das blaue Alien immer an einer anderen Stelle steht, greifen wir auf `random.shuffle()` zurück.
Füge folgenden Code unter `def layout_aliens(alien_layout):` ein:

```python=
    gaps = len(alien_layout) + 1
    gap_size = WIDTH / gaps
    random.shuffle(alien_layout)
    for index, alien in enumerate(alien_layout):
        new_x = (index + 1) * gap_size
        alien.x = new_x
```

## Test Nr. 3

Testen wir doch gleich mal, ob es funktioniert hat. So sollte das Ergebnis aussehen:

![](https://i.imgur.com/NXNwA0P.png)


## Bewegung in die Sache bringen

Der nächste Schritt ist die Animation. Die Aliens sollen sich über den Bildschirm nach unten bewegen. Da sich die Geschwindigkeit mit jedem Level steigern soll, müssen wir festlegen, wie lange die Animation dauern soll. Die Animation soll enden, wenn die Aliens die untere Bildschirmkante erreichen, dafür müssen wir einen Anker setzen. Ein Anker ist ein bestimmter Punkt auf einer Grafik. (Anker gibt es auch im HTML: Klickt man auf einen Link und springt direkt zu einem bestimmten Textteil, so wurde hier ein Anker gesetzt.)
Füge folgenden Code unter `def animate_aliens(animate_aliens):` ein:

```python=
    for alien in animate_aliens:
        duration = START_SPEED - aktuelles_level
        alien.anchor = ("center", "bottom")
        animation = animate(alien, duration=duration, on_finished=handle_game_over, y=HEIGHT)
        animations.append(animation)
```

## Game Over

Jedes Spiel muss auch mal enden. Dafür brauchen wir die folgende Funktion:

```python=
def handle_game_over():
    global game_over
    game_over = True
```

Sobald die Aliens, den Bildschirmrand erreichen, schließt sich das Spielfenster.

## Punktezahl festlegen

Um unseren Score zu erhöhen, müssen wir festlegen, wie viele Punkte die unterschiedlichen Aliens wert sind. Die Zahlen können wir ganz beliebig bestimmen. Mit dem Klick auf ein Alien der entsprechenden Farbe, sollte sich der Score also erhöhen:

```python=
def on_mouse_down(pos):
    global aliens, aktuelles_level, score
    for alien in aliens:
        if alien.collidepoint(pos):
            if "blau" in alien.image:
                score += 10
                alien_click()
            elif "rot" in alien.image:
                score += 20
                alien_click()
            elif "gruen" in alien.image:
                score += 30
                alien_click() 
            elif "lila" in alien.image:
                score += 40
                alien_click()
```

## Das nächste Level

Sobald auf eins der Alien geklickt wurde, springt das Spiel auf das nächste Level und die Geschwindigkeit erhöht sich.

```python=
def alien_click():
    global aktuelles_level, aliens, animations, game_complete
    stop_animations(animations)
    if aktuelles_level == Endlevel:
        game_complete = True
    else:
        aktuelles_level = aktuelles_level + 1
        aliens = []
        animations = []
```

## Animation anhalten

```python=
def stop_animations(stop):
    for animation in stop:
        if animation.running:
            animation.stop()
```


## Das fertige Spiel

Nachfolgend der vollständige Code des Spiels:

```python=
import random

WIDTH = 900
HEIGHT = 600
START_SPEED = 10 
FARBEN = ["gruen", "rot", "lila", "blau"] 
Endlevel = 9 
game_over = False 
game_complete = False
aktuelles_level = 1 
aliens = []
animations = [] 
score = 0

def draw():
    global aliens, aktuelles_level, game_over
    screen.clear()
    screen.draw.text(f"Score: {score}", (0, 0))
    if game_over:
        screen.draw.textbox("Game Over!", Rect(350, 150, 200, 200))
    elif game_complete:
        screen.draw.textbox(f"Geschafft! Dein Score ist: {score}", (350, 150, 200, 200))
    else:
        for alien in aliens:
            alien.draw()

def update():
    global aliens
    if len(aliens) == 0:
        aliens = erstelle_aliens(aktuelles_level)

def erstelle_aliens(alienanzahl):
    create_farbe = farbwahl(alienanzahl)
    neue_aliens = erschaffe_aliens(create_farbe)
    layout_aliens(neue_aliens)
    animate_aliens(neue_aliens)
    return neue_aliens

def farbwahl(alienanzahl):
    create_farbe = ["blau"]
    for i in range(0, alienanzahl):
        random_farbe = random.choice(FARBEN)
        create_farbe.append(random_farbe)
    return create_farbe

def erschaffe_aliens(create_farbe):
    neue_aliens = []
    for farbe in create_farbe:
        alien = Actor(farbe)
        neue_aliens.append(alien)
    return neue_aliens

def layout_aliens(alien_layout):
    gaps = len(alien_layout) + 1
    gap_size = WIDTH / gaps
    random.shuffle(alien_layout)
    for index, alien in enumerate(alien_layout):
        new_x = (index + 1) * gap_size
        alien.x = new_x

def animate_aliens(animate_aliens):
    for alien in animate_aliens:
        duration = START_SPEED - aktuelles_level
        alien.anchor = ("center", "bottom")
        animation = animate(alien, duration=duration, on_finished=handle_game_over, y=HEIGHT)
        animations.append(animation)

def handle_game_over():
    global game_over
    game_over = True

def on_mouse_down(pos):
    global aliens, aktuelles_level, score
    for alien in aliens:
        if alien.collidepoint(pos):
            if "blau" in alien.image:
                score += 10
                alien_click()
            elif "rot" in alien.image:
                score += 20
                alien_click()
            elif "gruen" in alien.image:
                score += 30
                alien_click() 
            elif "lila" in alien.image:
                score += 40
                alien_click()

def alien_click():
    global aktuelles_level, aliens, animations, game_complete
    stop_animations(animations)
    if aktuelles_level == Endlevel:
        game_complete = True
    else:
        aktuelles_level = aktuelles_level + 1
        aliens = []
        animations = []

def stop_animations(stop):
    for animation in stop:
        if animation.running:
            animation.stop()
```

## Ideen zur Erweiterung

+ Füge ein gelbes Alien ein, auf das nicht geklickt werden darf. Ein Klick führt zum Game Over.
+ Füge Musik und Soundeffekte ein.
+ Füge ein Hintergrundbild ein.
+ Mach, dass sich die Aliens von Anfang an sehr schnell nach unten bewegen für einen erhöhten Schwierigkeitsgrad