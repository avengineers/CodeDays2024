## Wer seid ihr?

Menschen <!-- .element: class="fragment" -->

~~Chuck Norris~~ <!-- .element: class="fragment" -->

Note:

click

Diese Präsentation ist für Menschen, die mit Softwareentwicklung zu tun haben.

click

Sie ist nicht für Chuck Norris.

--

### Software Workflow

![Chuck](images/chuck_norris_vs_humans.png) <!-- .element width="80%" class="fragment" -->

Note:

Der Grund, warum unser Vortag nicht für Chuck Norris geeignet ist, ist ziemlich offensichtlich.

click

Was er produziert bzw. liefert, funktioniert sofort.

Für den Rest von uns ist das nicht so einfach. 

- Wir müssen große, komplexe Probleme in kleine Aufteilen.

- Wir müssen uns schlaue Architekturen überlegen, damit wir das Rad nicht immer wieder neu erfinden.

- Wir müssen unsere Software auf unsere eigenen Fehler hin überprüfen und sie auch beseitigen

Das alles müssen wir kontinuierlich verbessern, um auch nur Ansatzweise an eine Chuck Norris Lösung heran zu kommen.

--

## Was sind unsere Challenges?

* Projektvielfalt <div class="fragment" style="color:red">Softwareproduktlinien</div>
* Dokumentation und Nachweis der SW Qualität <div class="fragment" style="color:red">A.SPICE</div>
* Continuous Integration <div class="fragment" style="color:red">Weniger ist mehr</div>

Note:

Die eben angedeuteten Hürden sind vielleicht allen bekannt. Bei uns ist es aber sicher eine spzielle Ausrichtung. 

In der Automobilwelt stehen wir einer unglaublichen Produktvielfalt gegenüber. Zig unterschiedliche Hersteller, mit unterschiedlichen Modellen, Modellvarianten, und dann noch angepasst auf unterschiedliche Märkte und Regionen je nach gesetzlichen Vorgaben. 
Jedes Produkt hier als ein einzelnen Projekt aufzusetzen und die Software immer Anfang an neu zu schreiben, wäre der Tod für jedes Unternehmen.
Man muss einen geeigneten Weg finden, diese Vielzahl an Produktvarianten über Konfigurationen abzubilden.
Wir lösen das über Softwareproduktlinien und brechen damit ein wenig die Komplexität unserer Produktpalette auf.

Dazu müssen wir ständig die Qualität unserer Produkte überprüfen. Das gilt im Prinzip für jedes Softwareprodukt.
In der Automobilwelt geht's aber sehr häufig um safety-relevante Funktionen, bei denen also Menschenleben oder zumindest deren Unversehrtheit im Vordergrund steht.
Dazu bekommt jeder Entwickler ein Prozess-Regelwerk in die Hand gedrückt. Mit dessen Hilfe lässt sich auf eine standardisierte Art und Weise Software in solchen Umgebungen entwickeln.
Auch wenn viele es als Hindernis ansehen, ist der Gedanke eher, dass es eine Hilfe darstellen soll. 
Ich spreche hier von Automotive Spice, oder kurz A.SPICE.

Als drittes wollen wir unsere Produkte natürlich schnell fertig bekommen und ausliefern. 
Time-to-Market ist vielleicht nicht alles, aber eine gute Basis
Hier werden wir im späteren Verlauf erklären, wie unsere Lösung aussieht, und warum man für gutes CI eigentlich gar nicht so viel benötigt.
