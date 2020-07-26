# Themen für Fortgeschrittene


## Anmerkung

Nachfolgend findest du die ersten Versuche in objektorientierter Programmierung, aber sie sind für jüngere Schüler/Kinder eher nicht zu empfehlen, da sie abstraktes Denken benötigen. 


## Klassen

Wir haben Klassen bereits bei Pygame Zero kennen gelernt, z.B. Rect und Actor. Diese Klassen haben jedoch nicht automatisch die *vx* und *vy* Variablen. Wir müssen also jedes Mal daran denken, *vx* und *vy* hinzuzufügen, wenn wir Actor benutzen.

Wir können jedoch unsere eigene Klasse erschaffen. Wir nennen sie *Sprite* und sie ist dieselbe wie Actor, beinhaltet jedoch die *vx* und *vy* Variablen. 

```python=
WIDTH = 500
HEIGHT = 500

class Sprite(Actor):
    vx = 1
    vy = 1

ball = Sprite('alien')

def draw():
    screen.clear()
    ball.draw()

def update():
    ball.x += ball.vx
    ball.y += ball.vy
    if ball.right > WIDTH or ball.left < 0:
        ball.vx = -ball.vx
    if ball.bottom > HEIGHT or ball.top < 0:
        ball.vy = -ball.vy
```




## Methoden

Klassen können, neben Variablen, auch Funktionen beinhalten, genannt *Methoden*.
Methoden sind der beste Ort, um die Variablen einer Klasse anzupassen.

```python=
WIDTH = 500
HEIGHT = 500

class Sprite(Actor):
    vx = 1
    vy = 1

    def update(self):
        self.x += self.vx
        self.y += self.vy
        if self.right > WIDTH or self.left < 0:
            self.vx = -self.vx
        if self.bottom > HEIGHT or self.top < 0:
            self.vy = -self.vy

ball = Sprite("alien")

def draw():
    screen.clear()
    ball.draw()

def update():
    ball.update()
```


## Joystick-Test

Dieses Programm demonstriert die Nutzung eines Joysticks und *for* Schleifen, aber sein Hauptzweck ist das Testen von Controller-Eingaben. 

(Ich empfehle nicht, dieses Programm selbst zu schreiben.)

```python=
import pygame

def update():
    screen.clear()
    joystick_count = pygame.joystick.get_count()
    y = 0
    for i in range(joystick_count):
        joystick = pygame.joystick.Joystick(i)
        joystick.init()
        name = joystick.get_name()
        axes = joystick.get_numaxes()
        buttons = joystick.get_numbuttons()
        hats = joystick.get_numhats()
        screen.draw.text(
            "Joystick {} name: {} axes:{} buttons:{} hats:{}".format(
                i, name, axes, buttons, hats), (0, y))
        y += 14
        for i in range(axes):
            axis = joystick.get_axis(i)
            screen.draw.text("Axis {} value: {:>6.3f}".format(i, axis),
                             (20, y))
            y += 14
        for i in range(buttons):
            button = joystick.get_button(i)
            screen.draw.text("Button {:>2} value: {}".format(i, button),
                             (20, y))
            y += 14
        for i in range(hats):
            hat = joystick.get_hat(i)
            screen.draw.text("Hat {} value: {}".format(i, str(hat)),
                             (20, y))
            y += 14
```


## Veröffentlichung deiner Pygame Zero Spiele

Das ist nicht ganz einfach, aber du kannst deine Spiele an Leute weitergeben, die weder Python noch Mu installiert haben. Du kannst sie auf einem USB-Stick speichern oder auf einer Webseite zum Download anbieten oder auch auf itch.io zum verkauf. 

1. Installiere die volle Python-Version von [www.python.org](https://www.python.org/downloads/).

6. Bearbeite deinen Code mit der Hilfe von Mu. Am Anfang deines Codes fühe die folgende Zeile ein:
```python
import pgzrun
```

Am Ende des Programms füge diese Zeile ein:
```python
pgzrun.go()
```

Jedes Mal, wenn du im Programm `draw.text()` *musst* du eine Font angeben, z.B. `fontname="arial"`

Speichere.

3. Kopiere alle Fonts, die du benutzt hast, in den `fonts` Ordner und benenne sie um, damit die Namen nur Kleinbuchstaben enthalten. z.B. `arial.ttf`

3. Öffne die Befehlszeile (Klicke das Starsmenü und tippe ```cmd.exe``` ein.)

4. Gib deinen mu_code Ordner ein. Schreibe folgendes in die Aufforderung:
    
    ```cd mu_code```
    
5. Installiere pyinstaller und pgzero.  Schreibe folgendes in die Aufforderung:

    ```pip install pgzero pyinstaller```
    
2. Du solltest den *pgzero* Ordner an folgenden Orten finden:

    \verb+C:\Users\YOURNAME\AppData\\Local\\Programs\+

    \verb+Python\Python37\Lib\site-packages\pgzero+

    Das ist ein versteckter Ordner, deshalb musst du im Windows Explorer versteckte Ordner aktivieren, um ihn sehen zu können.
    
    Kopiere den *pgzero* Ordner in deinen *mu_code* Ordner.
    
    Du solltest deinen *mu_code* Ordner hier finden:
    \verb+C:\Users\YOURNAME\mu_code+
      
7. Lege eine ausführbare Datei an und schreib Folgendes in die Eingabeaufforderung:
    
    ```pyinstaller NAME_OF_GAME.py --distpath . --add-data "pgzero;pgzero" --add-data "images;images" --add-data "fonts;fonts" --add-data "sounds;sounds" --add-data "music;music" --onefile --noconfirm --windowed --clean```
    
   Das generiert ein Programm namens ```NAME_OF_GAME.exe```. Mit einem Doppelklick kannst du dein Spiel spielen.
   
8. Um das Spiel zu verteilen, musst du deinen ganzen *mu_code* Ordner mitkopieren. Du kannst es in eine Zip-Datei packen und auf einer Webseite hochladen.

