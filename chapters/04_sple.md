## SPLE

Note:
Softwareproduktlinien, auch bekannt als Software Product Lines (SPL), sind ein Ansatz in der Softwareentwicklung, der darauf abzielt, eine Familie von ähnlichen Softwareprodukten effizient zu entwickeln. Der Kerngedanke dabei ist, die gemeinsamen Merkmale und Funktionalitäten, die in mehreren Produkten auftreten, zu identifizieren und wiederverwendbar zu gestalten. Dieser Ansatz ermöglicht es, verschiedene Produkte zu erstellen, indem man auf einer gemeinsamen Basis von Code und Komponenten aufbaut und diese mit spezifischen, für jedes Produkt einzigartigen Funktionen erweitert.

Der Vorteil dieses Ansatzes liegt vor allem in der Wiederverwendung von Software, was zu einer Reduzierung der Entwicklungszeit und -kosten führt. Zudem ermöglicht es eine konsistente Qualität über verschiedene Produkte hinweg und erleichtert die Wartung und Weiterentwicklung der Software. Softwareproduktlinien sind besonders nützlich in Bereichen, wo ähnliche Produkte für unterschiedliche Kunden oder Märkte entwickelt werden müssen.

--

## Unsere Lampenkollektion

![](images/disco-light.png) <!-- .element: class="fragment" data-fragment-index="1" style="float: left; width: 30%" -->
![](images/sleeping-light.png) <!-- .element: class="fragment" data-fragment-index="2" style="float: center; width: 30%" -->
![](images/spa-light.png) <!-- .element: class="fragment" data-fragment-index="3" style="float: right; width: 30%" -->

Note:
Willkommen zu unserer vielfältigen Welt der Beleuchtung! Ich möchte euch drei einzigartige Mitglieder unserer Lampenfamilie vorstellen: das Disco-Licht, das Schlaflicht und das Spa-Licht. Auf den ersten Blick erscheinen diese Lampen völlig unterschiedlich – das Disco-Licht mit seiner energiereichen Blinkfunktion, ideal für Partys; das Schlaflicht, das mit seinem sanften Farbwechsel und der Dimmfähigkeit eine beruhigende Atmosphäre für einen guten Schlaf schafft; und das Spa-Licht, das mit seinem sanften Pulsieren eine entspannende und verjüngende Wirkung entfaltet. Diese Lampen wurden entwickelt, um in verschiedenen Umgebungen jeweils eine einzigartige Stimmung zu schaffen. Doch trotz ihrer Unterschiede teilen sie eine gemeinsame Grundlage.

--

## Ein Kern, viele Möglichkeiten: Unsere Softwareproduktlinie

![](images/core-assets.png) <!-- .element: style="width: 50%" -->

Note:
Was diese Lampen auf einer tieferen Ebene vereint, ist ihr Kern. Alle drei Lampen nutzen die gleiche Grundsoftware zur Ansteuerung ihrer LEDs. Diese gemeinsame Plattform ist das Herzstück unserer Produktlinie. Dazu kommen jetzt kundenspezifische Features. Für das Disco-Licht haben wir eine Blinkfunktion hinzugefügt. Beim Schlaflicht konzentrierten wir uns auf die Integration eines Farbwechsel- und Dimmmechanismus. Und das Spa-Licht verfügt über ein sanftes Pulsieren. Durch das Erkennen von Kernanteilen und dem Wiederverwenden dieser Anteile reduziert sich der Entwicklungsaufwand enorm. Die Wartungskosten sinken, da wir Code-Erosion verhindern. Und wie Kernanteile und kundenspezifische Wünsche zusammen kommen, das bestimmt unser zugrunde liegendes Build-System: Cmake.

--

## CMake + VS Code + KConfig + TDD

Note:
In diesem Abschnitt werde ich Euch zeigen, wie wir Open-Source Tools wie CMake, VSCode und KConfig in einer testgetriebenen Umgebung kombinieren und nutzen. Die 'CMake Tools Variants' in Visual Studio Code sind der erste Einstiegspunkt, um effizient unterschiedliche Varianten unserer Softwareproduktlinie zu managen.

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
Schauen wir uns also ein konkretes Beispiel an: Wir haben zwei Varianten – 'CustA/Disco' und 'CustB/Sleep'. Jede Variante wird in der CMake-Tools-Konfiguration definiert. An dieser Stelle definieren wir hauptsächlich die Anzeigenamen und Umgebungsvariablen. Durch die Definition dieser Varianten können wir schnell zwischen verschiedenen Build-Konfigurationen wechseln. Eine Konfiguration kann dabei als Standardwert eingestellt werden. Dies ist ein wesentlicher Teil unserer Entwicklungsstrategie, da es uns ermöglicht, maßgeschneiderte Lösungen für verschiedene Kundenanforderungen zu erstellen, während wir gleichzeitig eine konsistente und effiziente Basis beibehalten. Die Konfigurationen sehen in der Regel sehr ähnlich aus und sind leicht erweiterbar.

--

<!-- .slide: data-background-image="images/select-variant.png" data-background-size="contain" -->

Note:
In VS Code sieht die Auswahl einer Variante dann wie folgt aus. Zunächst sieht man unten die aktuell aktive Variante. Ein Klick darauf öffnet den Dialog für die Auswahl. Dort kann ich jetzt entweder das Disco-Licht oder das Schlaflicht auswählen. Das Spa-Licht ist aktuell noch nicht implementiert.
Sobald die Auswahl getroffen wurde, beginnt CMake mit dem Configure Schritt. Das bereitet gleichzeitig auch die IDE und IntelliSense auf diese Variante vor. Schließlich will ich Code-Completion, Präprozessor-Direktiven und andere Dinge für meine aktive Variante sehen.

--

<!-- .slide: data-background-image="images/build-component.png" data-background-size="contain" -->

Note:
Gleichzeitig stellt CMake durch den Configure Schritt auch die Build-Targets zur Verfügung. Ich habe jetzt also die Möglichkeit einzelne Komponenten oder die gesamte Software zu bauen. Ein Klick unten auf die Targets öffnet wie gewohnt das Auswahlmenü. Danach muss man noch mit "Build" bestätigen und das unterliegende Build-Tool, in unserem Fall Ninja, läuft los und erstellt die Binaries.

--

<!-- .slide: data-background-image="images/kconfig.png" data-background-size="contain" -->

Note:
Aber damit ist es noch nicht getan. Wir wollen ja nicht einfach nur unterschiedliche Varianten starten. Irgendwo müssen wir auch festlegen, worin sich unsere Varianten denn unterscheiden. Diese Konfiguration übernimmt KConfig, Kernel Config vom Linux Kernel. Mit KConfig sind wir in der Lage unser Feature Modell zu beschreiben und Varianten daraus abzuleiten. In diesem konkreten Fall konfigurieren wir, dass die Lampe dimmbar sein soll und in einer bestimmten Farbe leuchtet. Standardmäßig ist hier Purple, also Lila, eingestellt. Wir stellen uns jetzt mal vor ich würde das auf rot wechseln.

--

<!-- .slide: data-background-image="images/black-sleep-light.png" data-background-size="contain" -->

Note:
Jetzt kann ich die gebaute App, eine EXE, ausführen und bekomme eine schematische Anzeige meiner Lampenfunktionalität. Es handelt sich um die Schlaflampe und sie ist aktuell ausgeschaltet. Durch Interaktion mit dem Terminal, schalte ich die Lampe auf Knopfdruck ein.

--

<!-- .slide: data-background-image="images/red-sleep-light.png" data-background-size="contain" -->

Note: 
In diesem Fall habe ich sie auf rot konfiguriert, deswegen leuchtet sie rot.

--

<!-- .slide: data-background-image="images/test.png" data-background-size="contain" -->

Note:
Und da wir ja keine Chuck Norris, sondern normale Menschen sind. Kann ich dieses Verhalten eben auch abtesten. Die Tests sind auf Basis von Google Test zusammen mit Google Mock. Da das beides allerdings nur für C++ ist, unsere Lampen aber in embedded-C entwickelt werden, haben wir dafür einen eigenen Open-Source Wrapper/Konverter, auf den wir hier aber nicht im Detail eingehen können. Die Testergebnisse sind aber für weitere Schritte sehr wichtig, nämlich beim ASPICE, dazu kommen wir als nächstes.
