# Minecraft Pi voor beginners

Minecraft is een populair "sandbox" spel dat toelaat werelden te bouwen en daarin te bewegen en taken uit te voeren. Er bestaat een gratis versie van die beschikbaar is op de Raspberry Pi en die bovendien een programmeerbare interface heeft. Dat betekent dat je cammando's en scripts kan schrijven in Python (een volwaardige programmeertaal) om automatisch dingen te bouwen. Een uitstekende manier om zowel Python als algemene programmeerprincipes te leren!

![Minecraft Pi banner](images/minecraft-pi-banner.png)

## Minecraft starten

Om Minecraft op te starten moet je dubbelklikken op het icoon op het bureaublad of `minecraft-pi` ingeven in een terminal venster.

![](images/mcpi-start.png)

Wanneer Minecraft Pi volledig geladen is, klik je op **Start Game**, en vervolgens op **Create New**. Je zal merken dat het window niet helemaal gecentreerd is en je moet de titelbalk gebruiken om het scherm te ververslepen.

![](images/mcpi-game.png)

Je bent nu in een Minecraft spel. Loop wat rond, hak erop los en bouw maar raak!

Gebruik de muis om rond te kijken en gebruik de volgende toetsen om het keyboard om acties uit te voeren:

| Toets        | Actie                |
| :---:        | :-----:              |
| W            | Vooruit              |
| A            | Links                |
| S            | Neerwaarts           |
| D            | Rechts               |
| E            | Voorraad             |
| Space        | Spring               |
| Double Space | Vlieg / Val          |
| Esc          | Pauze / Game menu    |
| Tab          | Muis cursor lossen   |

Je kan een item kiezen uit het quick draw panel met het scroll wheel van de muis (of gebruik de nummers op je toetsenbord). Of klik `E` om iets te selecteren uit je voorraad.

![](images/mcpi-inventory.png)

Je kan ook dubbelklikken op de spatiebalk om rond te vliegen in de lucht. Je stopt met vliegen wanneer je de spatiebalk loslaat en bij de volgende dubbele spatie val je terug op de grond.

![](images/mcpi-flying.png)

Met het zwaard in je hand kan je klikken op blokken die voor je liggen om ze te verwijderen (of om te graven) en met een blok in je hand kan je rechts klikken om het voor je te plaatsen of links om er één te verwijderen.

## De Python programming interface gebruiken

Terwijl Minecraft runt en met een actieve wereld, haal je de window focus weg van het spel door de `Tab` toets in te drukken, waardoor je muis weer actief wordt. Open IDLE (niet IDLE3) via het application menu of het bureaublad en beweeg de windows zodat ze naast mekaar te zien zijn.

Je kan ofwel commando's direct in het Python scherm invoeren of je kan een bestand maken en er je code in schrijven zodat het bewaard blijft om later opnieuw te gebruiken.

Als je een file wil maken, gebruik je de menu's `File > New window` en `File > Save`. Je wil de bestanden waarschijnlijk in je home folder of in een speciaal aangemaakte project folder bewaren.

Begin met de Minecraft library te importeren (lees: software bibliotheek in te lezen), een verbinding te maken met het spel en dit alles te testen door de boodschap "Hello World" te schrijven op het scherm:

```python
from mcpi import minecraft
mc = minecraft.Minecraft.create()
mc.postToChat("Hello world")
```

Als je de commando's rechstreeks in het Python scherm typt, klik je gewoon `Enter` aan het eind van elke lijn. Als je met een bestand werkt, bewaar het dan met `Ctrl + S` en start het resultaat met `F5`. Zodra de code runt, zou je de boodschap moeten zien verschijnen in het spel.

![](images/mcpi-idle.png)

### Je locatie vinden

Om uit te vinden waar in het spel je je bevindt, typ:

```python
pos = mc.player.getPos()
```

`pos` bevat nu je locatie; elk deel de coordinaten waaruit je locatie bestaat (Minecraft is een 3D wereld, dus je positie bestaat uit 3 coordinaten: x, y en z) kan je raadplegen met de eigenschappen van pos:  `pos.x`, `pos.y` and `pos.z`.

Python geeft je ook de mogelijkheid in één stap de positie uit te lezen en de coordinaten "uit te pakken":

```python
x, y, z = mc.player.getPos()
```

Nu bevatten `x`, `y`, en `z` elk hun deel van de positie coordinaten. `x` en `z` zijn de wandelrichtingen (vooruit/achteruit and links/rechts) en `y` is omhoog/omlaag.

Nota: `getPos()` leest de locatie van de speler op het moment dat de functie aangeroepen wordt en als je beweegt moet je dat opnieuw doen als je de nieuwe locatie wil weten.

### Teleporteren

Je kan niet enkel je huidige positie achterhalen, maar ook teleporteren naar een opgegeven locatie.

```python
x, y, z = mc.player.getPos()
mc.player.setPos(x, y+100, z)
```

Dit teleporteert je karakter meteen 100 posities omhoog de lucht in. Dit betekent dat je midden in de lucht terechtkomt en meteen weer terug naar de grond valt, precies op de plaats waar je vertrokken was.

Probeer ergens anders naartoe te teleporteren!

### Blok plaatsen

Je kan een enkele blok op een bepaalde plaats creëren met `mc.setBlock()`:

```python
x, y, z = mc.player.getPos()
mc.setBlock(x+1, y, z, 1)
```

Nu zou er een stenen blok moeten verschijnen net naast de lokatie van je speler. Als het niet vlak voor je is, zal het naast of achter je zijn. Ga terug naar het Minecraft scherm en gebruik je muis om rond te draaien tot je een grijs blok ziet.

![](images/mcpi-setblock.png)

De argumenten die gegeven werden aan de `set block` functie zijn `x`, `y`, `z` en `id`. De `(x, y, z)` refereren uiteraard naar de positie van de nieuwe blok in onze Minecraft wereld (in dit geval 1 plaats weg van waar de speler staat met `x + 1`) en `id` verwijst naar het type blok dat we willen plaatsen. `1` is steen.

Andere blokken die je kan proberen:

```
Lucht:   0
Gras :   2
Grond:   3
```

Probeer nu, met een blok in zicht, dit blok in iets anders te veranderen:

```python
mc.setBlock(x+1, y, z, 2)
```

Je zou het grijze stenen blok voor je ogen in gras moeten zien transformeren!

![](images/mcpi-setblock2.png)

#### Blokken als variabelen

Je kan een variabele gebruiken om een bloktype ID in op te slaan en zo de code meer leesbaar te maken. Deze ID's vind je via `block`:

```python
dirt = block.DIRT.id
mc.setBlock(x, y, z, dirt)
```

Of als je het ID kent, kan je het ook rechtstreeks zetten:

```python
dirt = 3
mc.setBlock(x, y, z, dirt)
```

### Speciale blokken

Er zijn blokken die extra eigenschappen hebben, zoals Wol dat een extra instelling heeft waarmee je de kleur kan bepalen. Dit gebeurt met een extra vierde parameter in `setBlock`:

```python
wool = 35
mc.setBlock(x, y, z, wool, 1)
```

De vierde parameter `1` stelt de kleur van de wol in op oranje. Zonder de vierde parameter valt de waarde terug op haar default (standaard) waarde (`0`), wat wit voorstelt. Enkele andere kleuren zijn:

```
2: Magenta
3: Licht Blauw
4: Geel
```

Probeer wat andere nummers en kijk hoe de kleuren veranderen!

Andere blokken met extra eigenschappen zijn hout (`17`): eik, berk, etc; hoog gras (`31`): bosje, gras, varen; toorts (`50`): richting oost, west, noord, zuid; en meer. Bekijk de [API reference](http://www.stuffaboutcode.com/p/minecraft-api-reference.html) voor meer details.

### Meerdere blokken plaatsen

Net zoals `setBlock` dient om een enkele blok te plaatsen, kan je met `setBlocks` in één keer een volume opvullen:

```python
stone = 1
x, y, z = mc.player.getPos()
mc.setBlocks(x+1, y+1, z+1, x+11, y+11, z+11, stone)
```

Dit vult een 10 x 10 x 10 kubus volledig op met stenen blokken.

![](images/mcpi-setblocks.png)

Je kan grotere volumes creëren met de `setBlocks` functie, maar hoe groter het volume, hoe langer het duurt!

## Bloks aanmaken waar je loopt

Nu we weten hoe we blokken kunnen creëren, kunnen we onze locatie gebruiken om tijdens het lopen blokken aan te maken.

De volgende code laat overal waar je stapt bloemen groeien:

```python
from mcpi import minecraft
from time import sleep

mc = minecraft.Minecraft.create()

flower = 38

while True:
    x, y, z = mc.player.getPos()
    mc.setBlock(x, y, z, flower)
    sleep(0.1)
```

Wandel nu een eindje voorwaarts en draai dan om om de bloemen te zien die je hebt achtergelaten.

![](images/mcpi-flowers.png)

Aangezien we een `while True` loop gebruikt hebben, zou deze code voor eeuwig blijven herhalen. Om de herhaling te onderbreken, moet je `Ctrl + C` typen in het Python window.

Probeer ook eens te vliegen om de bloemen te zien die je in de lucht achterlaat:

![](images/mcpi-flowers-sky.png)

En wat als we alleen bloemen willen achterlaten als de speler op gras rondwandelt? We kunnen `getBlock` gebruiken om uit te vinden op welke ondergrond we ons bevinden:

```python
x, y, z = mc.player.getPos()  # player position (x, y, z)
this_block = mc.getBlock(x, y, z)  # block ID
print(this_block)
```

Dit geeft ons natuurlijk het type block waar we ons *in* bevinden (wat `0` zal zijn, een lucht blok). We willen weten *op* welk blok we ons bevinden. Dus moeten we van de `y` waarde één aftrekken (één positie naar beneden) en dan `getBlock()` gebruiken.

```python
x, y, z = mc.player.getpos()  # player position (x, y, z)
block_beneath = mc.getBlock(x, y-1, z)  # block ID
print(block_beneath)
```

Dit geeft ons het ID van het blok waarop de speler op dat moment staat.

Test dit uit door een loop te laten lopen die het ID uitprint en vervolgens wat rond te rennen:

```python
while True:
    x, y, z = mc.player.getPos()
    block_beneath = mc.getBlock(x, y-1, z)
    print(block_beneath)
```

![](images/mcpi-block-test.png)

We kunnen een `if` (als...dan) statement gebruiken om te kiezen of we al dan niet een bloem willen achterlaten:

```python
grass = 2
flower = 38

while True:
    x, y, z = mc.player.getPos()  # player position (x, y, z)
    block_beneath = mc.getBlock(x, y-1, z)  # block ID

    if block_beneath == grass:
        mc.setBlock(x, y, z, flower)
    sleep(0.1)
```

En we kunnen een `else` (... en anders ...) statement gebruiken om de niet-gras blokken waarover we lopen in gras te veranderen:

```python
if block_beneath == grass:
    mc.setBlock(x, y, z, flower)
else:
    mc.setBlock(x, y-1, z, grass)
```

Nu kan je rondlopen en als je op gras loopt, laat je een bloem achter. Zoniet, dan wordt de ondergrond gras. En als we dan omdraaien en terugwandelen, laten we wel bloemen achter, want nu is het wel gras...

![](images/mcpi-flowers-grass.png)

## Spelen met TNT blokken

Nog een interessante blok is TNT (een springstof)! Om een normaal TNT blok te plaatsen gebruik je:

```python
tnt = 46
mc.setBlock(x, y, z, tnt)
```

![](images/mcpi-tnt.png)

Maar dat is maar een saai TNT blok. Probeer eens als `data` een `1` mee te geven:

```python
tnt = 46
mc.setBlock(x, y, z, tnt, 1)
```

Gebruik nu je zwaard en klik op het TNT blok - het zal activeren en enkele seconden later ontploffen!

Maak nu een groot volume TNT blokken!

```python
tnt = 46
mc.setBlocks(x+1, y+1, z+1, x+11, y+11, z+11, tnt, 1)
```

![](images/mcpi-tnt-blocks.png)

Je zal nu een enorme kubus van TNT blokken zien. Geef een flinke mep op één van de blokken en ren dan een eindje weg om je show te bewonderen. Omdat er zoveel dingen tegelijk moeten veranderen zal de weergave op het scherm wel vertraagd zijn.

![](images/mcpi-tnt-explode.png)

## Wat nu?

Er zijn nog veel dingen te ontdekken in de Minecraft wereld (en in Python!)

Voor meer documentatie van de functies en een volledige lijst van blok types, kan je de API reference raadplegen op [stuffaboutcode.com](http://www.stuffaboutcode.com/p/minecraft-api-reference.html).
