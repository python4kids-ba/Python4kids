# Textbasierte Quiz-Spiele


Für diese Programme ist der [Mu editor](https://codewith.mu/) zu empfehlen, da bei diesem Python, Pygame Zero und anderes bereits vorinstalliert sind und man nach der Installation gleich durchstarten kann.

## Hello, world

Hello, world ist für gewöhnlich das erste Programm, das ausgeführt wird. Es eignet sich gut zum Testen, ob Python funktioniert und damit auch andere Programme laufen. Es besteht aus einem einfachen print-Befehl, welcher für jede Ausgabe nötig ist.


```python=
print("Hello world")

# Diese Zeile ist nur ein Kommentar und nicht nötig
```



Bei Nutzung des mu editors:

1. Versichere dich zu Anfang, dass als Modus `Python3` eingestellt ist. Zu finden links oben unter `Modus`. 
2. Schreibe (oder kopiere) den Code in den Editor.
3. Klicke auf `Speichern` und gib einen Namen für das Programm ein.
4. Klicke `Spielen`.



## Eingaben mit der Tastatur

Dieses Programm erwartet eine Eingabe mit der Tastatur. Der eingegebene Text wird dabei in der Variable `name` gespeichert. Der Name der Variable kann frei gewählt werden. 

```python=
print("Wie heißt du?")
name = input()
print("Hallo,", name +"!")
```

Das nachfolgende Programm ist ähnlich dem ersten Beispiel aufgebaut, die Variable wurde hier mit `x` benannt und der Code wurde um eine if-Anweisung erweitert. Darauf wird im nächsten Punkt näher eingegangen. 

```python=
print("Gib deinen Namen ein:")
x = input()
print("Hallo", x)
if x == "richard": #die Kinder können hier ihren eigenen Namen eintragen
    print("Das ist ein sehr cooler Name")
```

### Übungsaufgaben
* Füge die Namen deiner Freunde hinzu und zeig eine andere Nachricht für jeden von ihnen 


## Entscheidungen treffen: if, elif, else

if/elif/else bietet die Möglichkeit unterschiedliche Ausgaben abhängig von der Eingabe zu erhalten.
if und else ist simples wenn x, dann y und lässt sich durch elif erweitern. Wenn if nicht zutrifft, dann wird als nächstes die elif-Anweisung geprüft, erst wenn diese ebenfalls nicht zutrifft wird else ausgeführt. Es lassen sich beliebig viele elif-Anweisungen einfügen. 

```python=
print("Gib deinen Namen ein:")
x = input()
print("Hallo", x)
if x == "richard": 
    print("Das ist ein sehr cooler Name")
elif x == "nick":
    print("Das ist ein blöder Name")
else:
    print("Deinen Namen kenne ich nicht", x)
```

Ein weiteres Beispiel:

```python=
print("Was ist dein Lieblingstier?")
tier = input()
if tier == "Katze":
    print("Katzen sind auch meine Lieblingstiere.")
elif tier == "Hund":
    print("Hunde sind echt toll!")
else:
    print("Nicht so cool, wie Katzen und Hunde!")
```

Die Ausgabe dazu würde so aussehen:

![](https://i.imgur.com/bhMzCcy.png)


Dies lässt sich dann zu einem etwas komplexeren Beispiel erweitern, wie etwa if-Anweisungen mit mehreren Bedingungen, die mit `or` miteinander verknüpft sind: 

```python=
print("Was ist dein Lieblingstier?")
tier = input()
if tier == "Katze" or tier == "Maus":
    print("Das ist auch mein Lieblingstier!")
elif tier == "Hund" or tier == "Hamster":
    print("Find ich auch echt toll!")
else:
    print("Nicht mein Lieblingstier, aber trotzdem nett.")
```

Bei `or` muss mindestens eine der if/elif-Bedingungen erfüllt sein, ansonsten wird else ausgeführt. Neben `or` gibt es aber auch noch `and`. Bei `and` müssen alle Bedingungen erfüllt sein, um die Ausgabe der if/elif-Anweisung zu bekommen.

```python=
print("Gib bitte eine Zahl ein")
zahl = int(input())
if zahl >= 10 and zahl < 20:
    print("Yeah!")
elif zahl > 60 and zahl < 100:
    print("Toll!")
else:
    print("Langweilig.")
```


## Eine Mathe-Frage mit zufälliger Zahl

`random` ist ein Modul, das importiert werden muss, bevor es dafür genutzt werden kann eine zufällige Zahl zu generieren. Im nachfolgenden Beispiel wird eine Zahl zwischen 0 und 10 generiert und für eine Mathe-Aufgabe verwendet

```python=
import random

n = random.randint(0, 10)

print("Wie viel ist", n, "plus 7?")
g = int(input())  # Warum wird hier int verwendet?
if g == n + 7:
    print("Richtig")
else:
    print("Falsch")
```

### Übungsaufgaben
* Ändere den Code wie folgt:
    * Generiere eine weitere zufällig Zahl und nutze sie anstatt der 7.
    * Erweitere den Bereich aus der eine zufällige Zahl gewählt wird.
    * Multipliziere (benutze `*`), dividiere (benutze `/`) or subtrahiere (benutze `-`) Zahlen.
* Gib am Ende an, wie viele Fragen der Spieler richtig beantwortet hat.



## Punktzahl (Score) ausgeben

Wir erzeugen eine Variable `score` in der gespeichert wird, wie viele Fragen der Spieler richtig beantwortet hat. Der Code kann natürlich auch für das vorherige Beispiel verwendet werden.

```python=
score = 0

print("Wie viel ist 1+1 ?")
g = int(input())
if g == 2:
    print("Richtig")
    score = score + 1

print("Wie viel ist 35-25 ?")
g = int(input())
if g == 10:
    print("Richtig")
    score = score + 1

print("Deine Punktzahl:", score)
```



## Ratespiel mit einer Schleife
 
Schleifen lassen ein Programm solange laufen, bis eine bestimmte Bedingung erfüllt wurde. Die `while` Schleife im nachfolgenden Beispiel läuft also solange bis der Spieler die richtige Zahl erraten hat. Erst dann kommt die `break` und die Schleife stoppt. 
Wichtig: alles innerhalb der Schleife muss eingerückt sein. 
 
```python=
import random

n = random.randint(0, 10)

while True:
    print("Errätst du die Zahl an die ich gerade denke?")
    g = int(input())
    if g == n:
        break
    else:
        print("Falsch")
        print("")
print("Richtig!")
```

Die "leere" Print-Anweisung dient allein optischen Gründen. Sie schafft eine Leerzeile zwischen dein einzelnen Versuchen und sorgt damit für eine bessere Übersicht.

Die Ausgabe würde dann z.B. folgendermaßen aussehen:

![](https://i.imgur.com/GaXZOrS.png)




### Übungsaufgaben

* Gib dem Spieler einen Hinweis, wenn sie falsch liegen. War ihr Tipp zu hoch oder zu niedrig?
* Am Ende soll ausgegeben werden, wie viele Versuche der Spieler gebraucht hat.


## Verbessertes Ratespiel

Dieses Programm zeigt die Lösung der vorherigen Übungsaufgaben. 

```python=
import random

n = random.randint(0, 100)
guesses = 0

while True:
    guesses = guesses + 1
    print("Errätst du die Zahl an die ich gerade denke?")
    g = int(input())
    if g == n:
        break
    elif g < n:
        print("Zu niedrig")
        print("")
    elif g > n:
        print("Zu hoch")
        print("")
print("Richtig! Du hast", guesses, "Versuche gebraucht.")
```

Die Ausgabe:

![](https://i.imgur.com/isXZPai.png)



