# Tutorial: Shooter (Schieß-Spiel)

In diesem Tutorial werden wir zusammen ein Schieß-Spiel, oder auch Shooter, programmieren. Python, das wir dafür brauchen ist: Bedingungen, Schleifen, Listen und Funktionen.

## Schritt 1: Entscheide, was für Spielfiguren du brauchst

Das Spiel braucht diese Spielfiguren, Items, sowie einen Hintergrund, daher **müssen wir für alle ein Bild zeichnen und als `.png` Datei im `images` Ordner speichern.**


| Variablenname  | Bildname        | Bildgröße  | Beschreibung           |
|:-------------- | --------------- | ---------- | ---------------------- |
| player         | player.png      | 64x64      | Spielerraumschiff      |
| background     | background.png  | 600x800    | Bild von Sternen       |
| enemies (list) | enemy.png       | 64x64      | böses Alien            |
| bullets (list) | bullet.png      | 16x16      | abgefeuert vom Spieler |
| bombs (list)   | bomb.png        | 16x16      | abgeworfen von Gegnern |

Die `player` und `background` Variablen sind Actors. Die anderen sind erstmal leere Listen `[]`. Die Actors werden später zu den Listen hinzugefügt. 


```python
import random

WIDTH = 600
HEIGHT = 800
MAX_BULLETS = 3

level = 1
lives = 3
score = 0

background = Actor("background")
player = Actor("player", (200, 580))
enemies = []
bullets = []
bombs = []
```



## Schritt 2: Zeichne die Spielfiguren (Actors)

Jedes Pygame benötigt eine `draw()` Funktion, die all unsere Actors auf dem Spielfeld abbildet.


```python
def draw():
    screen.clear()
    background.draw()
    player.draw()
    for enemy in enemies:
        enemy.draw()
    for bullet in bullets:
        bullet.draw()
    for bomb in bombs:
        bomb.draw()
    draw_text()
```


## Schritt 3: Bewege die Actors

Jedes Pygame braucht auch eine `update()` Funktion, die die Actors bewegt, auf Kollisionen prüft, etc. 


```python
def update(delta):
    move_player()
    move_bullets()
    move_enemies()
    create_bombs()
    move_bombs()
    check_for_end_of_level()
```

### Übungsaufgabe
Zeichne die .png Bilddateien (`player.png, background.png, bullet.png, bomb.png, enemy.png`) und speichere sie. Warum läuft das Programm nicht?


## Schritt 4: Funktionen definieren

Python kann keine Funktion aufrufen, die nicht vorher definiert worden ist. Daher erstellen wir leere Platzerhalterfunktion ohne Funktion, die wir später füllen. Da Python aber auch nicht mit einer komplett leeren Funktion arbeiten kann, müssen wir mindestens eine Zeile einfügen. Dafür nutzen wir `pass`, um Zeilen einzufügen, die nichts tun. 

```python
def move_player():
    pass

def move_enemies():
    pass

def move_bullets():
    pass

def create_bombs():
    pass

def move_bombs():
    pass

def check_for_end_of_level():
    pass

def draw_text():
    pass
```

### Übungsaufgabe
Füge es obige ans Ende des Codes. Teste, ob das Spiel jetzt funktioniert. Du solltest die Spielerfigur am unteren Ende des Bildschirms sehen (aber noch nicht bewegen können). 

## Erschaffe Gegner

Füge die nachfolgende Funktion ans Ende des Programms und rufe sie gleich ab. Sie nutzt eine Schleife innerhalb einer Schleife, um die Gegner zu erschaffen und packt sie in die `enemies` Liste. Der Grund, warum wir es in einer Funktion unterbringen ist, dass wir sie zu Beginn jedes Levels wieder abrufen. 

```python
def create_enemies():
    for x in range(0, 600, 60):
        for y in range(0, 200, 60):
            enemy = Actor("enemy", (x, y))
            enemy.vx = level * 2
            enemies.append(enemy)


create_enemies()
```

## Bewege die Spielfigur

Ersetze die `move_player()` Platzhalterfunktion durch den nachfolgenden Code. 
Wichtig: Es kann **nur eine Funktion mit einem bestimmten Namen geben**. *Es kann nicht zwei `move_player()` Funktionen geben.*

```python
def move_player():
    if keyboard.right:
        player.x = player.x + 5
    if keyboard.left:
        player.x = player.x - 5
    if player.x > WIDTH:
        player.x = WIDTH
    if player.x < 0:
        player.x = 0
```

## Bewege die Gegner

Ersetze die `move_enemies()` Platzhalterfunktion mit Folgendem:

```python
def move_enemies():
    global score
    for enemy in enemies:
        enemy.x = enemy.x + enemy.vx
        if enemy.x > WIDTH or enemy.x < 0:
            enemy.vx = -enemy.vx
            animate(enemy, duration=0.1, y=enemy.y + 60)
        for bullet in bullets:
            if bullet.colliderect(enemy):
                enemies.remove(enemy)
                bullets.remove(bullet)
                score = score + 1
        if enemy.colliderect(player):
            exit()
```

## Gib Text auf dem Bildschirm aus

Ersetze die `draw_text()` Platzhalterfunktion mit Folgendem:

```python
def draw_text():
    screen.draw.text("Level " + str(level), (0, 0), color="red")
    screen.draw.text("Score " + str(score), (100, 0), color="red")
    screen.draw.text("Lives " + str(lives), (200, 0), color="red")
```

## Munition

Füge die nachfolgende neue Funktion am Ende des Programms ein. Pygame ruft sie automatisch auf, wenn eine Taste gedrückt wird. 

```python
def on_key_down(key):
    if key == keys.SPACE and len(bullets) < MAX_BULLETS:
        bullet = Actor("bullet", pos=(player.x, player.y))
        bullets.append(bullet)
```

Ersetze die `move_bullets()` Platzhalterfunktion mit Folgendem:

```python
def move_bullets():
    for bullet in bullets:
        bullet.y = bullet.y - 6
        if bullet.y < 0:
            bullets.remove(bullet)

```

## Gegnerbomben

Ersetze die `create_bombs()` Platzhalterfunktion mit Folgendem:

```python
def create_bombs():
    if random.randint(0, 100 - level * 6) == 0:
        enemy = random.choice(enemies)
        bomb = Actor("bomb", enemy.pos)
        bombs.append(bomb)
```

Ersetze die `move_bombs()` Platzhalterfunktion mit Folgendem:

```python
def move_bombs():
    global lives
    for bomb in bombs:
        bomb.y = bomb.y + 10
        if bomb.colliderect(player):
            bombs.remove(bomb)
            lives = lives - 1
            if lives == 0:
                exit()
```

## Prüfe, ob das Ende des Levels erreicht wurde

Ersetze die `check_for_end_of_level()` Platzhalterfunktion mit Folgendem:

```python
def check_for_end_of_level():
    global level
    if len(enemies) == 0:
        level = level + 1
        create_enemies()
```

## Ideen für eine Erweiterung

* Zeichne das Bild einer Explosion und lasse es jedes Mal erscheinen, wenn ein Alien stirbt.
* Lass die Explosion nach kurzer Zeit wieder verschwinden.
* Zeichne mehrere Explosionsbilder, packe sie in eine Liste und animiere sie, sodass sie nacheinander erscheinen.
* Der Spieler kann nur jeweils 3 Kugeln abschießen. Kreiere ein Powerup, das ihm erlaubt weitere kugeln zu schießen. 
* Füge Musik und Soundeffekte hinzu.