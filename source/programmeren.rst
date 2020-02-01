Programmeren
============

Nu we de hardware hebben aangesloten kunnen we ons programma gaan schrijven.
We gaan hierbij wat gevorderde technieken voor gebruiken! Dit is echter niet nodig,
en je kan het ook simpeler aanpakken. Als je dat liever doet, nadat het bijvoorbeeld te lastig is, 
vraag het dan aan een van de mentoren.

Arduino
-------

Een arduino programma ziet er in de basis altijd hetzelfde uit, het heeft een **setup** en een **loop** methode.

.. code:: arduino

    void setup() {
        //Dit wordt 1x aangeroepen, als de arduino start.
    }

    void loop() {
        //Dit wordt continu aangeroepen, tot je de stroom er uit haalt :^).
    }

Om te beginnen kun je een nieuwe `schets` aanmaken in het arduino programma.
Hier kun je vervolgens in gaan programmeren.

Initialisatie
-------------

In onze arduino omgeving moeten we allereest de verschillende pinnen initialiseren.
Ze kunnen of een **input** zijn, of een **output**.

Een input is bijvoorbeeld een *sensor*, want we lezen hier een waarde van uit.
Een output is bijvoorbeeld een *led*, we zitten dit aan. 

Om een pin te initialiseren gebruiken we de volgende methode:

.. code:: arduino

    //Dit is of INPUT, of OUTPUT
    pinMode(pin, INPUT);
    pinMode(pin, OUTPUT);


pin is de betreffende pin, dit is dus bijvoorbeeld 10, 11 of 12. 
Maar dit kan ook A1 of A2 zijn, zoals ze aangegeven worden op het arduino bord zelf.

Deze initialisatie code zetten we in het `setup` block.

.. code:: arduino

    void setup() {

    }

We moeten voor deze `pinMode` natuurlijk wel onze pinnen definieren.
We kunnen dit per pin doen, dat gaat als volgt in de **setup** functie:

.. code:: arduino

    int pin1 = 11;
    int pin2 = 12;
    int pin3 = 13;
    //En zo voort..


    void setup() {
        pinMode(pin1, OUTPUT);
        pinMode(pin2, OUTPUT);
        pinMode(pin3, OUTPUT);
        //En zo voort..
    }


Geadvanceerd met arrays
-----------------------

Maar we kunnen ook gebruik maken van een andere type variabele, de **array**.
Een **array** is een lijst van bepaalde types. 
Hier kunnen we dus bijvoorbeeld gebruik van maken om meerdere pinnen in een keer te initialiseren.

Een array definieer je met `[ ]` (blokhaken). Dit ziet er zo uit:

.. code:: arduino

    int pins[1];
    
Hierbij moet je in de blokhaken een getal meegeven, dit is de grootte van de lijst.
Om de lijst te initialiseren, en er dus waardes in te zetten, kunnen we gebruik maken van de volgende constructie:

.. code:: arduino

    int pins[3] = {11, 12, 13};


Nu hebben we een lijstje met naam **pins**, waar 3 getallen in zitten: 11, 12 en 13.

Om een getal uit een **array** te halen gebruik je blokhaken met een **index** getal (`[ ]`).
Deze index bepaald de plek waar je de waarde van op haalt. **Let hierbij op** dat arrays op 0 beginnen. 
Dus het eerste element zit op plaats 0 :

.. code:: arduino

    int pin = pins[0]; //Dit levert dus 11 op!

Om nu alle pinnen in een efficiente manier te initialiseren kunnen we gebruik maken van een loop:

.. code:: arduino
    
    int pins[3] = {11, 12, 13};

    for(int i = 0; i < 3; i++) {
        pinMode(pins[i], OUTPUT);
    }


Vergeet de lichtsensor niet!
----------------------------

We moeten de lichtsensor pin natuurlijk als **input** initialiseren.
Dit doen we als volgt:

.. code:: arduino

    int lichtPin = A1;

    void setup() {
        pinMode(lichtPin, INPUT);
    }


Voortgang tot nu toe:
---------------------
Tot nu toe hebben we de volgende sketch:


.. container:: toggle

   .. container:: header

       klik om voorbeeld te tonen

   .. code:: arduino

                    
            const int totaalAantalLedjes = 5;
            int ledPins[totaalAantalLedjes] = { 0, 1, 2, 3, 4};
            int lichtPin = 7;

            void setup() {
                for(int i = 0; i < totaalAantalLedjes; i++) {
                    pinMode(ledPins[i], OUTPUT);
                }
                
                pinMode(lichtPin, INPUT);
            }

            void loop() {
                
            }


Sensor uitlezen
---------------

Weet je nog dat de lichtsensor een **analoge sensor** is?
We kunnen deze analoge waarde uitlezen met de **analogRead()** functie. Dit doen we als volgt:

.. code:: arduino

    void loop() {
        int sensorWaarde = analogRead(lichtPin);
    }

Deze waarde zit tussen de 0 en de 1023. Dit komt door de **analog to digital** converter in de arduino.
Hoe dit precies werkt is een complex verhaal, maar een mentor kan dit altijd uitleggen!

We willen echter dat we deze waarde kunnen gebruiken om een bepaald aantal ledjes aan te zetten, hierbij is 1023 natuurlijk te hoog!

Resultaat
---------

Dit is nu het resultaat:

* We initialiseren al onze pinnen;
* We lezen de sensor uit;
* We zorgen dat we dit getal tussen 0 en 5 zetten;
* We zetten de correcte hoeveelheid ledjes aan, en de andere uit.

.. container:: toggle

   .. container:: header

       klik om voorbeeld te tonen

   .. code:: arduino

                    
            const int totalLedjes = 5;
            int ledPins[totalLedjes] = { 0, 1, 2, 3, 4};
            int lichtPin = 7;

            void setup() {
                for(int i = 0; i < totalLedjes; i++) {
                    pinMode(ledPins[i], OUTPUT);
                }
                
                pinMode(lichtPin, INPUT);
            }

            void loop() {
                int sensorWaarde = analogRead(lichtPin);
                int hoeveelheidLedjes = map(sensorWaarde, 0, 1023, 0, totalLedjes);

                for(int i = 0; i < hoeveelheidLedjes; i++) {
                    digitalWrite(ledPins[i], HIGH);
                }

                for(int i = hoeveelheidLedjes; i < totalLedjes; i++) {
                    digitalWrite(ledPins[i], LOW);
                }
            }
