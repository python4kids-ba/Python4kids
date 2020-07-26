# Tutorial: Tiere füttern

In diesem Spiel geht es darum, zwei Tiere zu füttern: eine Fledermaus, die Früchte möchte und ein Wolf, der Fleisch möchte. Füttert man sie nicht rechtzeitig, dann legen sie sich hin und verschwinden vom Spielfeld. Zusätzlich wird der Spieler von einer Ratte verfolgt, die versucht ihn zu schnappen. Angelehnt ist dieses Tutorial an das Spiel "Garten im Glück" aus dem Buch "Spiele mit Python supereasy programmieren" von Carol Vorderman u.a.

## Sprites

Für dieses Spiel brauchen wir mehrere Sprites: eine Spielfigur, einen Wolf, eine Fledermaus und eine Ratte. Du kannst die Tiere selber zeichnen (und dich dabei auch für andere Tiere entscheiden) oder du suchst dir im Internet Sprites. Hierbei können die RPG Maker Communities hilfreich sein. Im [RPG Maker Forum](https://forums.rpgmakerweb.com/index.php?categories/rpg-maker-specific-resources.140/) findet man viele Sprites, die frei verwendet werden dürfen.

Als Hauptcharakter dient eine Fee, erstellt von [skybluehair](https://forums.rpgmakerweb.com/index.php?threads/skylars-character-sprites.41628/)
![](https://i.imgur.com/7NcnguR.png)

Die Tiere stammen von [Haydeos, Kadokawa](https://forums.rpgmakerweb.com/index.php?threads/materials-by-haydeos.47400/)
![](https://i.imgur.com/oWsQNiP.png)
![](https://i.imgur.com/BQ707IO.png)
![](https://i.imgur.com/pNriUgt.png)
![](https://i.imgur.com/BJC4o0b.png)
![](https://i.imgur.com/ULqiBuq.png)

Die Bilder werden wieder im `images` Ordner von mu gespeichert.

## Module

Wir brauchen für dieses Spiel neben `random` auch das Modul `time`, da nebenbei ein Timer läuft, wie lange die Tiere schon zufrieden sind.

```python=
import random
import time
```

## Variablen

Zu Anfang legen wir wieder die Variablen fest.

```python=
WIDTH = 1500
HEIGHT = 900

game_over = False
tiere_satt = True
ratte_erwischt = False

vergangene_zeit = 0
start_zeit = time.time()
```

## Actor einfügen

Als nächstes fügen wir unseren Hauptcharakter ein und legen eine Startposition fest.

```python=
fee = Actor("fee")
fee.pos = 100, 500
```

## Die anderen Actors

Die restlichen Actors werden zufällig ins Spiel eingefügt. Dafür erstellen wir Listen, in denen sie gespeichert werden.

```python=
wolf_liste = []
wolf_ko_liste = []
fledermaus_liste = []
fledermaus_ko_liste = []
ratte_liste = []
```

## Die Ratte

Die Ratte bewegt sich über das Spielfeld und darf dabei nicht aus dem Fenster verschwinden. Dafür verfolgen wir ihre Bewegungen auf der x- und y-Achse in einer Liste.

```python=
ratte_vy = []
ratte_vx = []
```

## Das Spielfeld

Als nächstes wird es Zeit unser Spielfeld zu zeichnen. Bisher gibt es dort nur unsere Fee zu sehen, die restlichen Figuren folgen später, sobald die Listen gefüllt sind.

```python=
def draw():
    global game_over, vergangene_zeit
    if not game_over:
        screen.clear()
        fee.draw()
        for fledermaus in fledermaus_liste:
            fledermaus.draw()
        for wolf in wolf_liste:
            wolf.draw()
        for ratte in ratte_liste:
            ratte.draw()
    vergangene_zeit = int(time.time() - start_zeit)
    screen.draw.text("Tiere satt seit: " +  str(vergangene_zeit) + " Sekunden",
        topleft = (10, 10), color = "white")
```

## Erster Test

Zeit für einen ersten Test, um zu prüfen, ob unser Code soweit funktioniert. Das Spielfeld sollte so aussehen:

![](https://i.imgur.com/lJEEQbd.png)

## Mehr Funktionen

Wir brauchen für dieses Spiel viele Funktionen. Für's erste füllen wir sie erstmal mit dem Platzhalter `pass`, damit wir zwischendurch testen können ohne, dass es deswegen zu Fehlern kommt.

```python=
def neue_fledermaus():
    pass

def add_fledermaus():
    pass
    
def neuer_wolf():
    pass

def add_wolf():
    pass

def check_ko_zeit():
    pass

def fledermaus_ko():
    pass

def wolf_ko():
    pass

def check_wolf_kollision():
    pass
    
def check_fledermaus_kollision():
    pass

def update():
    pass
```

## Bewegung in die Sache bringen

Für das Spiel muss sich unsere Fee natürlich auch bewegen können. Beim Druck der entsprechenden Taste bewegt sich unsere Fee immer fünf Pixel in die angegebene Richtung. Außerdem geben wir an, dass unsere Fee nicht über den Bildschirmrand hinauskommt. Dafür ersetzen wir das `pass` unter der Funktion `def update():` mit folgendem Code:

```python=
    global game_over, ratte_erwischt
    global wolf_liste, fledermaus_liste, ratte_liste, vergangene_zeit
    if not game_over:
        if keyboard.left and fee.x > 0:
            fee.x -= 5
        if keyboard.right and fee.x < WIDTH:
            fee.x += 5
        elif keyboard.up and fee.y > 150:
            fee.y -= 5
        elif keyboard.down and fee.y < HEIGHT:
            fee.y += 5
```

Probier doch gleich einmal aus, ob sich die Fee auch wirklich bewegt!

## Tiere einfügen

Als nächstes brauchen wir unsere Tiere und einen Wert "satt". Diesen Wert fügen wir dann in unsere beiden KO-Listen ein, damit das Spiel weiß, ob ein Tier hungrig ist oder nicht. 
Fügen wir als erstes die Fledermaus ein. Dafür ersetzen wir `pass` in der Funktion `def neue_fledermaus():` durch

```python=
    global fledermaus_liste, fledermaus_ko_liste
    fledermaus = Actor("fleder")
    fledermaus.pos = random.randint(50, WIDTH - 50), random.randint(150, HEIGHT - 250)
    fledermaus_liste.append(fledermaus)
    fledermaus_ko_liste.append("satt")
    return
```

und für unseren Wolf in der Funktion `neuer_wolf():`

```python=
    global wolf_liste, wolf_ko_liste
    wolf = Actor("wolf")
    wolf.pos = random.randint(100, WIDTH - 50), random.randint(250, HEIGHT - 250)
    wolf_liste.append(wolf)
    wolf_ko_liste.append("satt")
    return
```

## Mehr Tiere

Zwei Tiere wären zu einfach, daher taucht alle 7 Sekunden eine neue Fledermaus und alle 12 Sekunden ein neuer Wolf auf. 
Dafür fügen wir unter `def add_fledermaus():` den folgenden Code ein:

```python=
    global game_over
    if not game_over:
        neue_fledermaus()
        clock.schedule(add_fledermaus, 7)
    return
```

und unter `def neuer_wolf():`

```python=
    global game_over
    if not game_over:
        neuer_wolf()
        clock.schedule(add_wolf, 12)
    return
```

ein.

Die Funktionen rufen sich zwar selbst auf, doch dafür müssen wir sie einmal manuell aufrufen. Dafür fügen wir **über** `def update():` noch folgende Zeilen ein:

```python=
add_fledermaus()
add_wolf()
```

## Zweiter Test

Prüfen wir doch mal, ob das auch alles so funktioniert, wie wir uns das vorstellen. Nach einer Weile sieht das Spielfeld dann so aus:

![](https://i.imgur.com/p32bcYF.png)


## Fütterungszeit

Jetzt ist es an der Zeit unsere Tiere zu füttern. Dafür legen wir zwei "Fütterungstasten" fest, die eine füttert Fleisch ('a') und die andere Früchte ('d') und erweitern unsere `update()` Funktion, direkt über den Bewegungstasten für die Fee. Die Funktion prüft, ob wir neben dem richtigen Tier stehen.

```python=
    if keyboard.a:
        check_wolf_kollision()
    if keyboard.d:
        check_fledermaus_kollision()
```

## Richtig gefüttert?

Im nächsten Schritt prüfen wir, ob unsere Fee in der Nähe eines Tieres ist und ob es das richtige Tier ist. Wenn ja, wird es gefüttert und ist wieder "satt". 
Füge folgenden Code bei `check_wolf_kollision()` ein:

```python=
    global fee, wolf_liste, wolf_ko_liste
    index = 0
    for wolf in wolf_liste:
        if (wolf.colliderect(fee) and wolf.image == "wolf_ko"):
            wolf.image = "wolf"
            wolf_ko_liste[index] = "satt"
            break
        index = index + 1
    return
```

Und diesen unter `check_fledermaus_kollision()`

```python=
    global fee, fledermaus_liste, fledermaus_ko_liste
    index = 0
    for fledermaus in fledermaus_liste:
        if (fledermaus.colliderect(fee) and fledermaus.image == "fleder_ko"):
            fledermaus.image = "fleder"
            fledermaus_ko_liste[index] = "satt"
            break
        index = index + 1
    return
```

## KO

Wenn die Tiere "satt" sein können, bedeutet das aber auch, dass sie "hungrig" werden können. In diesem Fall legen sie sich hin und gehen KO, wenn sie nicht rechtzeitig gefüttert werden.
Füge den folgenden Code unter `def fledermaus_ko():` ein. Er sorgt dafür, das alle drei Sekunden eine Fledermaus hungrig wird.

```python=
    global fledermaus_liste, fledermaus_ko_liste, game_over
    if not game_over:
        if fledermaus_liste:
            rand_fledermaus = random.randint(0, len(fledermaus_liste) - 1)
            if (fledermaus_liste[rand_fledermaus].image == "fleder"):
                fledermaus_liste[rand_fledermaus].image = "fleder_ko"
                fledermaus_ko_liste[rand_fledermaus] = time.time()
        clock.schedule(fledermaus_ko, 3)
    return
```

Und alle 8 Sekunden soll ein Wolf hungrig werden. Daher fügen wir unter `def wolf_ko():` folgendes ein:

```python=
    global wolf_liste, wolf_ko_liste, game_over
    if not game_over:
        if wolf_liste:
            rand_wolf = random.randint(0, len(wolf_liste) - 1)
            if (wolf_liste[rand_wolf].image == "wolf"):
                wolf_liste[rand_wolf].image = "wolf_ko"
                wolf_ko_liste[rand_wolf] = time.time()
        clock.schedule(wolf_ko, 8)
    return
```

Anschließend müssen diese beiden Funktionen wieder einmal direkt aufgerufen werden, daher fügen wir sie unter `add_fledermaus()` und `add_wolf()` ein.

```python=
fledermaus_ko()
wolf_ko()
```

Wenn wir es kurz testen, sollte sich alle paar Sekunden der Sprite eines der Tiere verändern.

![](https://i.imgur.com/gUD11wt.png)


## Game Over durch KO

Wenn ein Tier länger als zehn Sekunden hungrig ist, dann ist das Spiel vorbei.
Dafür setzen wir diesen Code unter `def check_ko_zeit():`

```python=
    global wolf_ko_liste, fledermaus_ko_liste, game_over, tiere_satt
    if wolf_ko_liste or fledermaus_ko_liste:
        for ko_seit in wolf_ko_liste or fledermaus_ko_liste:
            if (not ko_seit == "satt"):
                ko_zeit = int(time.time() - ko_seit)
                if (ko_zeit) > 10.0:
                    tiere_satt = False
                    game_over = True
                    break
    return
```

Die Funktion muss noch aufgerufen werden daher fügen wir

```python=
check_ko_zeit()
```

unter `def update()` ein.

## Die Game Over Nachricht

Im Moment passiert jedoch noch nichts, wenn ein Tier KO geht. Dafür gehen wir zurück zu unserer `draw()` Funktion. Dort haben wir eine if-Anweisung, die angibt, was passiert, wenn das Spiel nicht Game Over ist. Doch jetzt brauchen wir eine Anweisung, wenn es Game Over heißt. Dafür vervollständigen wir sie mit einer else-Anweisung.

```python=
    else:
        if not tiere_satt:
            screen.draw.text("GAME OVER!", color = "white", topleft=(10, 50))
```

## Auftritt: die Ratte

Damit das Spiel noch etwas an Schwierigkeit gewinnt, fügen wir noch einen Gegner ein. Alle 20 Sekunden verwandelt sich eins unserer Tiere in eine Ratte, die unserer Fee hinterher jagt. Dafür brauchen wir weitere Funktionen, die wir erst einmal wieder nur mit Platzhaltern füllen. Wir fügen sie über `update()` Funktion ein.

```python=
def check_ratte_kollision():
    pass

def geschwindigkeit():
    pass
    
def mutation():
    pass
    
def update_ratte():
    pass
```

## Mutation

Als erstes kümmern wir uns um die Mutation. Eins der Tiere "spuckt" dabei eine Ratte aus, die sich über das Spielfeld bewegt und der ausgewichen werden muss. Mit diesem Code legen wir fest, wie schnell sich die Ratte bewegt und wie oft, eine neue Ratte erscheint (in unserem Fall alle 20 Sekunden nachdem die erste Ratte erschienen ist). Um uns den Code etwas zu erleichtern, fügen wir `fledermaus_liste` und `wolf_liste` zu einer Liste `tier_liste` zusammen.
Gib den folgenden Code unter `def mutation():` ein:

```python=
    global fledermaus_liste, wolf_liste, ratte_vy, ratte_vx, game_over
    tier_liste = fledermaus_liste + wolf_liste
    if not game_over and tier_liste:   
        rand_tier = random.randint(0, len(tier_liste) - 1)
        ratte_x = tier_liste[rand_tier].x
        ratte_y = tier_liste[rand_tier].y
        del tier_liste[rand_tier]
        ratte = Actor("ratte")
        ratte.pos = ratte_x, ratte_y
        ratte_geschwindigkeit_x = geschwindigkeit()
        ratte_geschwindigkeit_y = geschwindigkeit()
        ratte = ratte_liste.append(ratte)
        ratte_vx.append(ratte_geschwindigkeit_x)
        ratte_vy.append(ratte_geschwindigkeit_y)
        clock.schedule(mutation, 20)
    return
```

## Bewegung

Die Ratte will unsere Fee fangen, daher muss sie sich auch bewegen können. Wir geben eine zufällige Richtung, sowie eine zufällige Geschwindigkeit an.
Das schreiben wir unter `def geschwindigkeit():`:

```python=
    random_richtung = random.randint(0, 1)
    random_geschwindigkeit = random.randint(2, 3)
    if random_richtung == 0:
        return -random_geschwindigkeit
    else:
        return random_geschwindigkeit
```

## Update

Die folgenden Zeilen sorgen dafür, dass unsere Ratte innerhalb des Spielfeld bleibt. Berührt sie den Rand, wird sie davon abgestoßen und bewegt sich in die andere Richtung.
Füge diesen Code unter `def update_ratte():` ein:

```python=
    global ratte_list, game_over
    if not game_over:
        index = 0
        for ratte in ratte_liste:
            ratte_geschwindigkeit_x = ratte_vx[index]
            ratte_geschwindigkeit_y = ratte_vy[index]
            ratte.x = ratte.x + ratte_geschwindigkeit_x
            ratte.y = ratte.y + ratte_geschwindigkeit_y
            if ratte.left < 0:
                ratte_vx[index] = -ratte_geschwindigkeit_x
            if ratte.right > WIDTH:
                ratte_vx[index] = -ratte_geschwindigkeit_x
            if ratte.top < 150:
                ratte_vy[index] = -ratte_geschwindigkeit_y
            if ratte.bottom > HEIGHT:
                ratte_vy[index] = -ratte_geschwindigkeit_y
            index = index + 1
    return
```

## Angriff!

Als nächstes müssen wir prüfen, ob die Ratte die Fett erwischt hat. Wenn ja, heißt das Game Over
Dieser Code kommt unter `def check_ratte_kollision():`

```python=
    global fee, ratte_liste, ratte_kollision, game_over
    for ratte in ratte_liste:
        if ratte.colliderect(fee):
            game_over = True
            break
    return
```   

Damit auch in diesem Fall eine Game Over Nachricht angezeigt wird, fügen wir den folgenden Code zusätzlich unter den Code ein, den wir im Punkt "Die Game Over Nachricht" geschrieben haben.

```python=
    else: 
        screen.draw.text("GAME OVER!", color="white", topleft=(10,50))
return
``` 
        
## Die letzten Schritte

Anschließend brauchen wir noch folgende Zeilen, die in unsere `update()` Funktion eingetragen werden müssen. Die erste Zeile kommt direkt über `check_ko_zeit()`. Die restlichen drei Zeilen kommen ganz ans Ende der Funktion. Sie sorgt dafür, dass die Ratte nach 15 Sekunden erscheint.

```python=
    ratte_kollision = check_ratte_kollision()


    if vergangene_zeit > 15 and not ratte_liste:
        mutation()
    update_ratte()
``` 

## Das fertige Spiel

Im folgenden findest du den kompletten Code des Spiels.

```python=
import random
import time

WIDTH = 1200
HEIGHT = 900

game_over = False
tiere_satt = True
ratte_erwischt = False

vergangene_zeit = 0
start_zeit = time.time()

fee = Actor("fee")
fee.pos = 100, 500

wolf_liste = []
wolf_ko_liste = []
fledermaus_liste = []
fledermaus_ko_liste = []
ratte_liste = []

ratte_vy = []
ratte_vx = []

def draw():
    global game_over, vergangene_zeit
    if not game_over:
        screen.clear()
        fee.draw()
        for fledermaus in fledermaus_liste:
            fledermaus.draw()
        for wolf in wolf_liste:
            wolf.draw()
        for ratte in ratte_liste:
            ratte.draw()
        vergangene_zeit = int(time.time() - start_zeit)
        screen.draw.text("Tiere gluecklich seit: " +  str(vergangene_zeit) + " Sekunden",
            topleft = (10, 10), color = "white")
    else:
        if not tiere_satt:
            screen.draw.text("GAME OVER!", color = "white", topleft=(10, 50))
        else: 
            screen.draw.text("GAME OVER!", color="white", topleft=(10,50))
    return

def neue_fledermaus():
    global fledermaus_liste, fledermaus_ko_liste
    fledermaus = Actor("fleder")
    fledermaus.pos = random.randint(50, WIDTH - 50), random.randint(150, HEIGHT - 250)
    fledermaus_liste.append(fledermaus)
    fledermaus_ko_liste.append("satt")
    return

def add_fledermaus():
    global game_over
    if not game_over:
        neue_fledermaus()
        clock.schedule(add_fledermaus, 7)
    return

def neuer_wolf():
    global wolf_liste, wolf_ko_liste
    wolf = Actor("wolf")
    wolf.pos = random.randint(100, WIDTH - 50), random.randint(250, HEIGHT - 250)
    wolf_liste.append(wolf)
    wolf_ko_liste.append("satt")
    return

def add_wolf():
    global game_over
    if not game_over:
        neuer_wolf()
        clock.schedule(add_wolf, 12)
    return

def check_ko_zeit():
    global wolf_ko_liste, fledermaus_ko_liste, game_over, tiere_satt
    if wolf_ko_liste or fledermaus_ko_liste:
        for ko_seit in wolf_ko_liste or fledermaus_ko_liste:
            if (not ko_seit == "satt"):
                ko_zeit = int(time.time() - ko_seit)
                if (ko_zeit) > 10.0:
                    tiere_satt = False
                    game_over = True
                    break
    return

def fledermaus_ko():
    global fledermaus_liste, fledermaus_ko_liste, game_over
    if not game_over:
        if fledermaus_liste:
            rand_fledermaus = random.randint(0, len(fledermaus_liste) - 1)
            if (fledermaus_liste[rand_fledermaus].image == "fleder"):
                fledermaus_liste[rand_fledermaus].image = "fleder_ko"
                fledermaus_ko_liste[rand_fledermaus] = time.time()
        clock.schedule(fledermaus_ko, 3)
    return

def wolf_ko():
    global wolf_liste, wolf_ko_liste, game_over
    if not game_over:
        if wolf_liste:
            rand_wolf = random.randint(0, len(wolf_liste) - 1)
            if (wolf_liste[rand_wolf].image == "wolf"):
                wolf_liste[rand_wolf].image = "wolf_ko"
                wolf_ko_liste[rand_wolf] = time.time()
        clock.schedule(wolf_ko, 8)
    return

def check_wolf_kollision():
    global fee, wolf_liste, wolf_ko_liste
    index = 0
    for wolf in wolf_liste:
        if (wolf.colliderect(fee) and wolf.image == "wolf_ko"):
            wolf.image = "wolf"
            wolf_ko_liste[index] = "satt"
            break
        index = index + 1
    return

def check_fledermaus_kollision():
    global fee, fledermaus_liste, fledermaus_ko_liste
    index = 0
    for fledermaus in fledermaus_liste:
        if (fledermaus.colliderect(fee) and fledermaus.image == "fleder_ko"):
            fledermaus.image = "fleder"
            fledermaus_ko_liste[index] = "satt"
            break
        index = index + 1
    return

def check_ratte_kollision():
    global fee, ratte_liste, ratte_kollision, game_over
    for ratte in ratte_liste:
        if ratte.colliderect(fee):
            game_over = True
            break
    return

def geschwindigkeit():
    random_richtung = random.randint(0, 1)  
    random_geschwindigkeit = random.randint(2, 3)
    if random_richtung == 0:
        return -random_geschwindigkeit
    else:
        return random_geschwindigkeit

def mutation():
    global fledermaus_liste, wolf_liste, ratte_vy, ratte_vx, game_over
    tier_liste = fledermaus_liste + wolf_liste
    if not game_over and tier_liste:
        rand_tier = random.randint(0, len(tier_liste) - 1)
        ratte_x = tier_liste[rand_tier].x
        ratte_y = tier_liste[rand_tier].y
        del tier_liste[rand_tier]
        ratte = Actor("ratte")
        ratte.pos = ratte_x, ratte_y
        ratte_geschwindigkeit_x = geschwindigkeit()
        ratte_geschwindigkeit_y = geschwindigkeit()
        ratte = ratte_liste.append(ratte)
        ratte_vx.append(ratte_geschwindigkeit_x)
        ratte_vy.append(ratte_geschwindigkeit_y)
        clock.schedule(mutation, 20)
    return

def update_ratte():
    global ratte_list, game_over
    if not game_over:
        index = 0
        for ratte in ratte_liste:
            ratte_geschwindigkeit_x = ratte_vx[index]
            ratte_geschwindigkeit_y = ratte_vy[index]
            ratte.x = ratte.x + ratte_geschwindigkeit_x
            ratte.y = ratte.y + ratte_geschwindigkeit_y
            if ratte.left < 0:
                ratte_vx[index] = -ratte_geschwindigkeit_x
            if ratte.right > WIDTH:
                ratte_vx[index] = -ratte_geschwindigkeit_x
            if ratte.top < 150:
                ratte_vy[index] = -ratte_geschwindigkeit_y
            if ratte.bottom > HEIGHT:
                ratte_vy[index] = -ratte_geschwindigkeit_y
            index = index + 1
    return

add_fledermaus()
add_wolf()
fledermaus_ko()
wolf_ko()

def update():
    global game_over, ratte_erwischt
    global wolf_liste, fledermaus_liste, ratte_liste, vergangene_zeit
    ratte_kollision = check_ratte_kollision()
    check_ko_zeit()
    if not game_over:
        if keyboard.a:
            check_wolf_kollision()
        if keyboard.d:
            check_fledermaus_kollision()
        if keyboard.left and fee.x > 0:
            fee.x -= 5
        if keyboard.right and fee.x < WIDTH:
            fee.x += 5
        elif keyboard.up and fee.y > 150:
            fee.y -= 5
        elif keyboard.down and fee.y < HEIGHT:
            fee.y += 5
        if vergangene_zeit > 15 and not ratte_liste:
            mutation()
        update_ratte()
``` 

## Ideen zur Erweiterung

+ Füge einen passenden Hintergrund ein.
+ Füge Musik und Soundeffekte hinzu.
+ Gib auf dem Bildschirm an, welche Taste bei welchem Tier verwendet werden muss
+ Ändere das Sprite der Fee, wenn sie von der Ratte erwischt worden ist.