# Deine Spiele erweitern und verbessern

Nachfolgend einige Tipps, wie sich Spiele erweitern und verbessern lassen.



## Joystick-Eingabe

Pygame unterstützt auch Joystick-Eingaben. Heutzutage sind Joysticks jedoch eher unter den Namen *Gamepad* oder *Controller* bekannt. PlayStation4 und XBOX Controller, die über ein USB-Kabel mit dem PC verbunden sind, funktionieren am besten. Andere Controller werden möglicherweise nicht unterstützt. 
Wichtig: wenn man versucht, das Programm ohne einen angeschlossenen Controller auszuführen, erhält man eine Fehlermeldung!

```python=
import pygame
WIDTH = 500
HEIGHT = 500

joystick = pygame.joystick.Joystick(0)
joystick.init()

alien = Actor("alien")
alien.pos = (0, 50)

def draw():
    screen.clear()
    alien.draw()

def update():
    if keyboard.right or joystick.get_axis(0) > 0.1:
        alien.x = alien.x + 2
    elif keyboard.left or joystick.get_axis(0) < -0.1:
        alien.x = alien.x - 2
```


### Übungsaufgaben

+ (Optional, wenn Controller vorhanden)
Sorg dafür, dass sich das Alien nach oben und unten, sowie links und rechts mit dem Controller bewegt. Mach dasselbe bei den anderen Beispielen, die eine Tastatureingabe benötigen.




## Farben

Anstatt bereits vordefinierte Farben zu verwenden, wie 'red', 'yellow', etc. lassen sich auch eigene Farben mischen. Dazu benötigen wir ein *Tupel* aus drei Zahlen. Ein *Tupel* ist eine Art Liste, jedoch nicht veränderbar. Die Werte der Variablen lassen sich zwar nach Belieben ändern und somit auch der Inhalt des *Tupels*, doch der *Tupel* direkt lässt sich nicht *per Code* verändern. 
Hierfür wird das RGB-Farbmodell genutzt. Dabei werden die drei Grundfarben Rot, Grün und Blau gemischt, um unterschiedliche Farben zu erzeugen. Der Anteil einer Farbe wird mit einer Zahl zwischen 0 und 255 festgelegt. Setzt man, zum Beispiel, die drei Werte auf 0, ergibt sich daraus Schwarz. Setzt man sie auf 255 bekommt man Weiß. Das Farbmodell bietet viel Raum zum Herumspielen und ausprobieren, um zu sehen, welche Kombinationen, welche Farben ergibt.


```python= 

red_amount = 0
green_amount = 0
blue_amount = 0

def draw():
    my_colour = (red_amount, green_amount, blue_amount)
    screen.fill(my_colour)

# Diese Funktion sorgt dafür, dass sich die Farbe ändert.
# Wenn man das nicht möchte, sondern nur eine statische Farbe, kann man 
# diesen Codeteil weglassen.
def update():
    global red_amount
    red_amount += 1
    red_amount = red_amount % 255
```


### Übungsaufgaben
+ (Für Fortgeschrittene):
Ändere die Grün- und Blauanteile um andere Farben zu erhalten. 
+ Mische die Farbe Grau.
+ (Für Fortgeschrittene):
Programmiere, dass die Zahlenwerte zufällig gewählt werden.




## Schleifen

Schleifen sind eine saubere und übersichtliche Methode denselben Code mehrmals auszuführen, ohne ihn zu wiederholen. 

In diesem Beispiel zeichnen wir rote Kreise mit Hilfe einer *for-Schleife*. Man ist jedoch nicht auf eine Schleife beschränkt, sondern kann innerhalb einer Schleife noch weitere Schleifen ausführen. 

```python=
WIDTH = 500
HEIGHT = 500

def draw():
    screen.clear()
    for x in range(0, WIDTH, 40):
        screen.draw.filled_circle((x, 20), 20, "red")

    for x in range(0, WIDTH, 40):
        for y in range(60, HEIGHT, 40):
            screen.draw.filled_circle((x, y), 10, "green")
```

### Übungsaufgaben
+ `import random` am Anfang des Programm und mach die Positionen zufällig, z.B. 
    ```python
    x = random.randint(0, 100)
    ```
+ (Für Fortgeschrittene):
Füge eine Timer-Variable hinzu und ändere die Farbe abhängig von der Zeit.


## Vollbildmodus

So gut wie alle Spiele heutzutage werden im Vollbildmodus ausgeführt. Mit nur wenigen Zeilen Code können wir das auch bei den von uns programmierten Spielen erreichen. Der Volldbildmodus wird beim unten genannten Beispiel mit der Taste *F* ausgeführt. Mit *ESC* wird er wieder beendet. Diese Befehle können natürlich auch mit anderen Tasten belegt werden. 
  
```python=
import pygame

WIDTH = 500
HEIGHT = 500

alien = Actor("alien")

def draw():
    screen.clear()
    alien.draw()

def update():
    if keyboard.f:
        pygame.display.set_mode((WIDTH, HEIGHT), pygame.FULLSCREEN)
    if keyboard.escape:
        exit()
```


## Score-Anzeige

Der folgende Code zeigt, wie man den Score in einer Variable speichert und sich anzeigen lässt. Er kann auch dafür benutzt werden um andere, beliebige Nachrichten im Spiel anzuzeigen. 

```python=
WIDTH = 500
HEIGHT = 500

score = 0

def draw():
    screen.clear()
    screen.draw.text(f"Player 1 score: {score}", (0, 0))
    screen.draw.textbox("Hello mum", Rect(50, 50, 200, 200))

# Diese Funktion wird von pygame automatisch ausgeführt, wenn eine Taste 
# gedrückt wird. Der Spieler kann nicht einfach nur eine Taste gedrückt lassen.

def on_key_down(key):
    global score
    if key == keys.SPACE:
        score += 1
```

Ausgabe: 

![](https://i.imgur.com/nwSQcTK.png)


### Übungsaufgaben
+ Vergrößere den Score-Text.
+ (Für Fortgeschrittene):
Füge einen zweiten Spieler hinzu, der eine andere Taste drücken muss, um seinen Score anzeigen zu lassen.
+ Füge Text zu einem der anderen Spiele hinzu. 




