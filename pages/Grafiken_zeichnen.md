# Grafiken zeichnen

Um die Grafiken für unsere Spiele zu erstellen, nutzen wir die [Pygame Zero library](https://pygame-zero.readthedocs.io)[^f1]. Auf der Webseite lässt sich eine nützliche Dokumentation finden. 

[^f1]: https://pygame-zero.readthedocs.io

Das kleinste Quadrat, das auf einem Monitor dargestellt werden kann, nennt man *Pixel*. Die nachfolgende Abbildung zeigt eine Nahaufnahme eines Fensters, das 40 Pixel breit und 40 Pixel hoch ist. Bei normaler Ansicht wären die Gitternetzlinien nicht zu sehen. 

![Model View Controller](https://i.imgur.com/lC70PS6.png)


Mit Hilfe von Koordinaten *(x,y)*, können wir uns auf jeden beliebigen Pixel beziehen. Da alles, was wir in Pygame Zero tun, Koordinaten benötigt, ist es wichtig diese auch zu verstehen, bevor man weiter macht. 

## Linien und Kreise

Pygame Zero ist, wie zuvor erwähnt, bereits in den Mu Editor integriert, aber **nicht vergessen 'Pygame Zero' unter 'Modus' auszuwählen, bevor man das Programm ausführt!** 

Wenn du einen anderen Editor benutzt, findet man [Anleitungen dafür online](https://pygame-zero.readthedocs.io/en/stable/ide-mode.html)[^f2]

Zeichnen ist ein guter Start für Kinder, doch sollte man darunter kein freies Zeichnen, wie bei Microsoft Paint oder auf einem simplen Blatt Papier verstehen, sondern es handelt sich um das Zeichnen von Linien und Kreisen, die selbstgewählte Größen und Farben haben und via Koordinaten platziert werden können. 

[^f2]: https://pygame-zero.readthedocs.io/en/stable/ide-mode.html

```python=
WIDTH = 500  # Was sind diese Zahlen? Was passiert, wenn wir sie ändern?
HEIGHT = 500  # Was passiert, wenn wir diese Zeile löschen?


def draw():
    screen.clear()
    screen.draw.circle((250, 250), 50, "white")
    screen.draw.filled_circle((250, 100), 50, "red")
    screen.draw.line((150, 20), (150, 450), "purple")
    screen.draw.line((150, 20), (350, 20), "purple")
```

Die Ausgabe dieses "Spiels" ist deutlich anders, als es bei den textbasierten Spielen war, denn es öffnet sich in einem neuen Fenster.

![](https://i.imgur.com/3WSYEvG.png)



### Übungsaufgabe

* Zeichne das Bild zu Ende.

Die Kinder können hier nach Belieben weitere Kreise und Striche in ihren Lieblingsfarben hinzufügen. Es bietet viel Raum zum Probieren und kreativ werden.


## Eine weitere kleine Grafik


```python=
WIDTH = 400
HEIGHT = 400

def draw():
    screen.fill("White")
    screen.draw.filled_circle((100,100), 50, "Pink")
    screen.draw.filled_circle((300,300), 50, "Pink")
    screen.draw.line([100, 100],[300, 300], "Black")
    screen.draw.filled_circle((100,300), 50, "Lime Green")
    screen.draw.filled_circle((300,100), 50, "Lime Green")
    screen.draw.line([100, 300],[300, 100], "Black")
    screen.draw.rect(Rect((150,150),(100,100)), "Blue")
    screen.draw.text("Eine Beispielgrafik", (60, 385), color="Black")
```
### Übungsaufgabe

* Zeichne dein eigenes Bild.

Passend hierzu bietet Kapitel 5 ein Programm, mit dem sich eigene Farben mischen lassen. Definitv einen Blick wert! 


## Bewegung in die Sache bringen

Damit sich auch etwas in unserem Spiel bewegt, nutzen wir die `update()` Funktion.
Dafür ist es nicht nötig, dass man eine eigene Schleife schreibt, da Pygame Zero mit dieser Funktion bereits eine Schleife ausführt. Die Box wird mit `Rect` gezeichnet. Rect ist dabei die Abkürzung für "rectangel", also ein Rechteck. Gibt man, wie in unserem Beispiel, Koordinaten an, zeichnet Pygame Zero ein Objekt mit diesen Maßen. 
Die Box bewegt sich von links nach rechts über das Spielfeld und taucht am linken Rand wieder auf, nachdem sie rechts aus dem Bild verschwunden ist.

```python=
WIDTH = 500
HEIGHT = 500

box = Rect((20, 20), (50, 50))

def draw():
    screen.clear()
    screen.draw.filled_rect(box, "red")


def update():
    box.x = box.x + 2
    if box.x > WIDTH:
        box.x = 0
```

### Übungsaufgaben

+ Lass die Box sich schneller bewegen.
+ Mach, dass sich die Box in unterschiedliche Richtungen bewegt.
+ Füge eine zweite Box in einer anderen Farbe hinzu.


## Sprites

Ein Sprite ist ein Grafikobjekt, das sich vor dem Hintergrundbild befindet[^f3]. Dabei kann es sich um Charaktere, aber auch Objekte, wie Stühle und Tische handeln. Oder eben auch um eine Box! Der mu-Editor bietet bereits einen ersten Sprite zum Testen. `Alien.png` sollte im Images-Ordner (mu_code/images) zu finden sein. 

[^f3]: https://techterms.com/definition/sprite

Natürlich lassen sich auch andere Grafiken als Sprites verwenden. Die Kinder können auch ihre eigenen Sprites zeichnen. Dafür reicht schon Microsoft Paint oder ein anderes Zeichenprogramm. Besonders zu empfehlen wäre hier auch [Krita](https://krita.org)[^f4].

`Actor` ist ähnlich wie `Rect`, doch anstatt eine Form zu zeichnen, greift Actor auf gespeicherte Bilder zurück, in diesem Fall `Alien.png`. 

[^f4]: https://krita.org

```python=
WIDTH = 500
HEIGHT = 500

alien = Actor('alien')
alien.x = 0
alien.y = 50


def draw():
    screen.clear()
    alien.draw()


def update():
    alien.x += 2
    if alien.x > WIDTH:
        alien.x = 0
```

Die Ausgabe dazu würde dann so aussehen (natürlich bewegt sich das Alien im Spiel dabei über das Spielfeld):

![](https://i.imgur.com/XCumVL4.png)


### Übungsaufgaben

* Zeichne oder downloade dein eigenes Bild und benutze es anstelle des Aliens. 



## Hintergrundbild

Als nächstes fügen wir ein Hintergrundbild ein. 
Wie schon die Sprites, muss das Hintergrundbild im Images-Ordner gespeichert werden. Auch hier lässt sich ein beliebiges Bild verwendet. Es ist jedoch zu empfehlen, dass die Bildgröße auch der Größe unseres Spielfensters entspricht; in diesem Beispiel also 500x500 Pixel. Man kann für den Hintergrund zwar einen beliebigen Namen wählen, aber es ist zu empfehlen hintergrund.png oder background.png zu verwenden, um die Übersicht zu behalten. Gerade bei größeren Spielen mit mehreren hundert Dateien ist eine genaue Benennung essentiell.

**Wichtig: Das Bild muss im `.png` Format gespeichert werden!**

```python=
WIDTH = 500
HEIGHT = 500

alien = Actor('alien')
alien.x = 0
alien.y = 50

background = Actor('background')

def draw():
    screen.clear()
    background.draw()
    alien.draw()


def update():
    alien.x += 2
    if alien.x > WIDTH:
        alien.x = 0
```

Der schwarze Hintergrund wird dabei durch das Hintergrundbild ersetzt:

![](https://i.imgur.com/arjPHVE.png)

Quelle: [Wallpaper Safari](https://wallpapersafari.com/w/aJCRnW)

### Übungsaufgaben

* Zeichne deinen eigenen Hintergrund, speichere es als `background.png` und führe das Programm aus.



## Die Steuerung

Der Hauptcharakter bewegt sich zwar jetzt über den Bildschirm, doch für ein Spiel sollte er auch steuerbar sein. Dies kann durch die Änderung der `update()` Funktion erreicht werden, sodass die Spielfigur auf Tastendruck reagiert.

```python=
alien = Actor('alien')
alien.pos = (0, 50)

def draw():
    screen.clear()
    alien.draw()

def update():
    if keyboard.right:
        alien.x = alien.x + 2
    elif keyboard.left:
        alien.x = alien.x - 2
```


### Übungsaufgaben

* Mach, dass das Alien sich auch nach unten und oben, sowie links und rechts bewegt.
* Benutze den += Operator um den `alien.x` Wert zu verändern. 
* Benutze den `or` Operator, damit das Alien zusätzlich zu den Pfeiltasten auch mit WASD bewegt werden kann. 

