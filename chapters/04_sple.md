## Software Product Line Engineering

Note:

Softwareproduktlinien, auch bekannt als Software Product Lines (SPL), sind ein Ansatz in der Softwareentwicklung, der darauf abzielt, eine Familie ähnlicher Softwareprodukte effizient zu entwickeln.

Die Grundidee besteht darin, gemeinsame Merkmale und Funktionalitäten, die in mehreren Produkten vorkommen, zu identifizieren und wiederverwendbar zu machen. Dieser Ansatz ermöglicht es, verschiedene Produkte auf der Grundlage einer gemeinsamen Code- und Komponentenbasis zu erstellen und diese um spezifische, für jedes Produkt einzigartige Funktionen zu erweitern.

Der Vorteil dieses Ansatzes liegt vor allem in der Wiederverwendbarkeit der Software, was zu einer Reduzierung der Entwicklungszeit und -kosten führt.

Darüber hinaus ermöglicht dieses Vorgehen eine konsistente Qualität über verschiedene Produkte hinweg und erleichtert die Wartung und Weiterentwicklung der Software.

Software-Produktlinien sind besonders nützlich in Bereichen, in denen ähnliche Produkte für verschiedene Kunden oder Märkte entwickelt werden müssen.

Obwohl es in unserem Vortrag um die Automobilindustrie geht, möchte ich SPLE jetzt am Beispiel von Lampen erklären. Es handelt sich dabei um ein kleines Spielprojekt, das wir zu Demonstrationszwecken erstellt haben.

Die Methoden sind aber natürlich auch auf andere Bereiche übertragbar. Unser Framework ist mit realen und komplexen Projekten produktiv im Einsatz.

--

## Unsere Lampenkollektion

![](images/disco-light.png) <!-- .element: class="fragment" data-fragment-index="1" style="float: left; width: 30%" -->
![](images/sleeping-light.png) <!-- .element: class="fragment" data-fragment-index="2" style="float: center; width: 30%" -->
![](images/spa-light.png) <!-- .element: class="fragment" data-fragment-index="3" style="float: right; width: 30%" -->

Note:

Also dann, willkommen in unserer vielfältigen Welt der Beleuchtung!

Ich möchte euch drei einzigartige Mitglieder unserer Lampenfamilie vorstellen.

Auf den ersten Blick scheinen diese Lampen völlig unterschiedlich zu sein.

(click)

Das Disco-Licht mit seiner energetischen Blinkfunktion, ideal für Partys;

(click)

Das Schlaflicht, das mit seinem sanften Farbwechsel und seiner Dimmbarkeit eine beruhigende Atmosphäre für einen guten Schlaf schafft;

(click)

Und das Spa-Licht, das mit seinem sanften Pulsieren entspannend und verjüngend wirkt.

Diese Lampen wurden entwickelt, um in verschiedenen Umgebungen jeweils eine einzigartige Stimmung zu schaffen.

Bei aller Unterschiedlichkeit haben sie jedoch eines gemeinsam.

--

## Ein Kern, viele Möglichkeiten: Unsere Softwareproduktlinie

![](images/core-assets.png) <!-- .element: style="width: 50%" -->

Note:
Was diese Lampen auf einer tieferen Ebene verbindet, ist ihr Kern. 

Alle drei Lampen verwenden die gleiche Basissoftware zur Ansteuerung ihrer LEDs. 

Diese gemeinsame Plattform ist der Kern unserer Produktlinie. 

Hinzu kommen nun kundenspezifische Features. 

Bei der Disco-Leuchte haben wir eine Blinkfunktion hinzugefügt. 

Beim Schlaflicht haben wir uns auf die Integration eines Farbwechsel- und Dimmmechanismus konzentriert. 

Und das Spa-Licht hat ein sanftes Pulsieren. 

Durch das Erkennen und Wiederverwenden von Kernkomponenten wird der Entwicklungsaufwand enorm reduziert. 

Die Wartungskosten sinken, weil wir Code-Erosion vermeiden. 

Und wie Kernteile und Kundenwünsche zusammenkommen, bestimmt unser zugrunde liegendes Build-System bestehend aus Cmake und KConfig.

--

## CMake + VS Code + KConfig + TDD

Note:
In diesem Abschnitt werde ich zeigen, wie wir Open-Source-Tools wie CMake, VSCode und KConfig in einer testgetriebenen Umgebung kombinieren und einsetzen. 

Die 'CMake Tools Variants' in Visual Studio Code sind der erste Einstiegspunkt, um die verschiedenen Varianten unserer Software-Produktlinie effizient zu verwalten.

--

```json [2-4,12|4-11|12-19|21]
{
  "variant": {
    "choices": {
      "CustA/Disco": {
        "buildType": "CustA_Disco",
        "long": "select to build variant 'CustA/Disco'",
        "settings": {
          "VARIANT": "CustA/Disco"
        },
        "short": "CustA/Disco"
      },
      "CustB/Sleep": {
        "buildType": "CustB_Sleep",
        "long": "select to build variant 'CustB/Sleep'",
        "settings": {
          "VARIANT": "CustB/Sleep"
        },
        "short": "CustB/Sleep"
      }
    },
    "default": "CustA/Disco"
  }
}
```

Note:
Schauen wir uns ein konkretes Beispiel an: Wir haben zwei Varianten - 'CustA/Disco' und 'CustB/Sleep'. 

(click)

Jede Variante wird in der Konfiguration des CMake-Tools definiert. 

Hier definieren wir hauptsächlich die Anzeigenamen und Umgebungsvariablen. 

(click)

Durch die Definition dieser Varianten können wir schnell zwischen verschiedenen Build-Konfigurationen wechseln. 

(click)

Eine Konfiguration kann als Standard gesetzt werden. 

Dies ist ein wichtiger Teil unserer Entwicklungsstrategie, da wir so maßgeschneiderte Lösungen für unterschiedliche Kundenanforderungen erstellen können und gleichzeitig eine konsistente und effiziente Basis beibehalten. 

Die Konfigurationen sind in der Regel sehr ähnlich und leicht erweiterbar.

--

<!-- .slide: data-background-transition="fade-in none-out" data-background-image="images/select-variant.png" data-background-size="contain" -->

Note:

Wir starten hier mit Visual Studio Code, mit einer Reihe von Plugins. Unter anderem dabei: C/C++ Tools und vor Allem auch den CMake-Tools.

In VS Code sieht die Auswahl einer Variante dann wie folgt aus.

--

<!-- .slide: data-background-transition="none" data-background-image="images/select-variant-marker.png" data-background-size="contain" -->

Note:

Zunächst sieht man unten die gerade aktive Variante. 

Ein Klick darauf öffnet den Auswahldialog. 

Dort kann ich nun entweder das Discolicht oder das Schlaflicht auswählen. 

Das Spa-Licht ist derzeit noch nicht implementiert.

Sobald die Auswahl getroffen wurde, beginnt CMake mit dem Configure-Schritt. 

Dieser bereitet gleichzeitig die IDE und IntelliSense auf diese Variante vor. 

Zuletzt möchte ich die Code-Vervollständigung, Präprozessor-Direktiven und andere Dinge für meine aktive Variante sehen.

--

<!-- .slide: data-background-transition="slide-in fade-out" data-background-image="images/build-component.png" data-background-size="contain" -->

Note:

Wenn ich mich für eine Variante entschieden habe, möchte ich natürlich mein Projekt bauen. 

CMake stellt durch den Configure-Schritt auch die Build-Targets zur Verfügung. 

Ich habe nun also die Möglichkeit, einzelne Komponenten oder die gesamte Software zu bauen. 

Immer im Kontext der Variante.

--

<!-- .slide: data-background-transition="none" data-background-image="images/build-component-marker.png" data-background-size="contain" -->

Note:

Ein Klick unten auf die Targets öffnet wie gewohnt das Auswahlmenü. 

Danach muss noch mit "Build" bestätigt werden und das darunterliegende Build-Tool, in unserem Fall Ninja, startet und erstellt die Binaries.

--

<!-- .slide: data-background-transition="slide-in fade-out" data-background-image="images/kconfig.png" data-background-size="contain" -->

Note:

Aber das ist noch nicht alles. 

Wir wollen ja nicht einfach verschiedene Varianten starten. 

Irgendwo müssen wir auch festlegen, worin sich unsere Varianten unterscheiden. 

--

<!-- .slide: data-background-transition="none" data-background-image="images/kconfig-marker.png" data-background-size="contain" -->

Note:
Diese Konfiguration übernimmt KConfig, Kernel Config aus dem Linux Kernel. 

Mit KConfig können wir unser Feature-Modell beschreiben und daraus Varianten ableiten. 

In diesem konkreten Fall konfigurieren wir, dass die Lampe dimmbar sein und in einer bestimmten Farbe leuchten soll. 

Standardmäßig ist hier Purple, also Lila eingestellt. Wir stellen uns nun vor, dass ich das auf Rot ändere.

--

<!-- .slide: data-background-transition="slide-in fade-out" data-background-image="images/black-sleep-light.png" data-background-size="contain" -->

Note:

Jetzt kann ich die erstellte Anwendung, eine EXE, ausführen und erhalte eine schematische Darstellung der Funktionalität meiner Lampe.

--

<!-- .slide: data-background-transition="none" data-background-image="images/black-sleep-light-marker.png" data-background-size="contain" -->

Note:

Es ist die Nachttischlampe und sie ist momentan ausgeschaltet. 

Durch Interaktion mit dem Terminal schalte ich die Lampe per Knopfdruck ein.

--

<!-- .slide: data-background-transition="none" data-background-image="images/red-sleep-light.png" data-background-size="contain" -->

Note: 

In diesem Fall habe ich sie auf rot konfiguriert, deswegen leuchtet sie rot.

--

<!-- .slide: data-background-transition="none" data-background-image="images/test.png" data-background-size="contain" -->

Note:

Und da wir nicht Chuck Norris sind, sondern normale Menschen, muss ich dieses Verhalten auch testen. 

Die Tests basieren auf Google Test zusammen mit Google Mock. 

Da beides aber nur für C++ ist, unsere Lampen aber in embedded-C entwickelt werden, haben wir dafür einen eigenen Open Source Wrapper/Konverter, auf den wir hier aber nicht näher eingehen können.

--

<!-- .slide: data-background-transition="slide-in fade-out" data-background-transition="none" data-background-image="images/test-marker.png" data-background-size="contain" -->

Note:
Wichtig ist auch hier wieder ein Feature der CMake Tools, die Build Kits. 

Denn für die Tests brauchen wir andere Compiler-Einstellungen als für die Produktiv-Sourcen.

Deshalb müssen wir zuerst von 'prod' auf 'test' umschalten.

Dann entscheiden wir, welchen Teil der Software wir testen wollen und sehen direkt die Testergebnisse und die Testabdeckung.

Dabei müssen die Tests in der Lage sein, alle Varianten abzudecken.

So können Tests und Komponenten unabhängig von den Projekten entwickelt werden, in denen sie eingesetzt werden.

Die Testergebnisse sind auch für weitere Schritte sehr wichtig, nämlich beim A.SPICE, doch dazu übergebe ich jetzt an Karsten.
