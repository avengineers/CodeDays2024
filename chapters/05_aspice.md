<!-- .slide: data-transition="none-out" -->

## A.SPICE

Automotive Software Process Improvement and Capability Determination <!-- .element: class="fragment" data-fragment-index="1" -->

https://vda-qmc.de/automotive-spice/ <!-- .element: class="fragment" data-fragment-index="1" -->

Note:

Matthias hat gerade gezeigt, wie man mit CMake und Visual Studio Code eine schöne Software-Produkt-Linien-Plattform aufbauen kann.

Kommen wir nun zu unserer zweiten Challenge: A.SPICE.

"click"

Die Abkürzung A.SPICE steht für Automotive Software Process Improvement and Capability Determination.

Klingt kompliziert, ist aber im Wesentlichen ein Prozessmodell, das die Entwicklung von Software in der Automobilindustrie beschreibt.

Es ist auch ein Reifegradmodel, das die Reifegrade 0 bis 5 definiert.

Diese Reifegrade werden durch die Erfüllung von Prozessanforderungen erreicht und müssen durch definierte Artefakte nachgewiesen werden.

Ich will auf die Reifegrade nicht näher eingehen, aber auf die Artifakte, die es zu erstellen gibt.

--

<!-- .slide: data-transition="slide-in fade-out" -->

![](images/aspice_traceability.png)

Note:

Hier sieht man eigentlich recht gut, was von den Entwicklern so erwartet wird: Jede Menge Artefakte.

Und dann muss das Alles auch noch miteinander verknüpft bzgl. verlinkt werden.

Unser Ziel war es von Anfang an, die geforderten Artefakte mit möglichst wenig Zusatzaufwand mit unserer SPLE Plattform erstellen zu können.

Zunächst haben wir uns auf die Prozesse

SWE.3: Software Detailed Design and Unit Construction und SWE.4: Software Unit Verification

konzentriert.

--

<!-- .slide: data-transition="fade" -->

![](images/aspice_traceability_swe34.png)

![](images/lc_dir_tree.png)<!-- .element: class="fragment" data-fragment-index="2" -->

Note:

Hier sehen wir nochmal im Ausschnitt, was wir für SWE.3 und SWE.4 machen müssen.

Die Units schreiben wir in C und testen sie mit Google Test, klar, das haben wir im Griff.

Die Dokumentation inkl. Software Detailed Design erzeugen wir mit Sphinx und Restructured Text, auch einfach.

"click"

Das alles packen wir für eine Komponente in einen Ordner, der so aussieht: doc, src und test.

Aber: wie bringen wir das alles in ein Dokument und verknüpfen es korrekt miteinander?

Die Antwort ist ...

--

## Sphinx-Needs

conf.py:<!-- .element: class="fragment" data-fragment-index="2" -->
```python [152: 1|3-9|11-19]
extensions.append("sphinx_needs")

# Define own needs types
needs_types = [
    { directive="req", title="Requirement", prefix="R_", color="#BFD8D2", style="node" },
    { directive="spec", title="Specification", prefix="S_", color="#FEDCD2", style="node" },
    { directive="impl", title="Implementation", prefix="I_", color="#DF744A", style="node" },
    { directive="test", title="Test Case", prefix="T_", color="#DCB239", style="node" },
]

# Define own link types
needs_extra_links = [
    # SWE.3 BP.5: link from Implementation (Software unit) to Specification (Software detailed design)
    {"option": "implements", "incoming": "is implemented by", "outgoing": "implements"},
    # SWE.4 BP.5: link from Test Case (Unit test specification) to Specification (Software detailed design)
    {"option": "tests", "incoming": "is tested by", "outgoing": "tests"},
    # SWE.4 BP.5: link from Test Case (Unit test specification) to Test Result (Unit test result)
    {"option": "results", "incoming": "is resulted from", "outgoing": "results"},
]
```
<!-- .element: class="fragment" data-fragment-index="2" style="font-size:10pt" -->

Note:

Sphinx-Needs ist eine Erweiterung für Sphinx.

Sie ermöglicht es, sogenannte "Needs"-Typen zu definieren und miteinander zu verlinken.

"click"

Wir haben für die A.SPICE Artefakte unsere eigenen Typen definiert: Requirement, Specification, Implementation und Test Case.

"click"

Außerdem haben wir eigene Link-Typen definiert, die die Verknüpfungen zwischen den Artefakten beschreiben.

Okay, wie sieht das dann in der Umsetzung aus?

--

### Software Detailed Design

![](images/lc_swdd.png) <!-- .element: class="fragment" data-fragment-index="1" style="float: right; width: 40%" -->

```rst [1: 12-18]
Introduction
------------

The Light Controller module is responsible for managing
the behavior of a light based on the system's
power state. This document outlines the design considerations
and the high-level structure of the module.

Design Considerations
---------------------

.. spec:: State Management
   :id: SWDD_LC-001
   :integrity: B

    The light can be in one of two states: ON or OFF.
    The state transitions are triggered by changes
    in the system's power state.


```
<!-- .element: style="float: left; font-size:10pt; width: 55%" -->

Note:

Hier sehen wir ein Beispiel für ein Needs-Element im Software Detailed Design, also direkt in rst.

Wir verwenden hier den Needs-Typ "spec" für Specification, geben dem spec-Element einen Namen und eine eindeutige ID.

"click"

Das generierte Dokument sieht dann so aus.

Hier sieht man auch schon die Verknüpfung zu den anderen Artefakten.

--

### Unit Test Specification

![](images/lc_uts.png) <!-- .element: class="fragment" data-fragment-index="2" style="float: right; width: 40%" -->

```C++ [50: ]
/**
 * @rst
 * .. test:: light_controller.test_light_stays_off
 *    :id: TS_LC-006
 *    :tests: SWDD_LC-001, SWDD_LC-004, R_001
 * @endrst
 */
TEST(light_controller, test_light_stays_off)
{
    CREATE_MOCK(mymock);

    // Initial state: Power is OFF, so the light should be OFF.
    EXPECT_CALL(
        mymock,
        RteGetPowerState()
    ).WillRepeatedly(Return(POWER_STATE_OFF));
    EXPECT_CALL(
        mymock,
        RteSetLightValue(_)
    ).Times(0); // Expect that the light value doesn't change.

    for (int i = 0; i < 10; i++) {
        lightController();
    }
}
```
<!-- .element: class="fragment" data-fragment-index="1" style="float: left; font-size:10pt; width: 55%" -->

https://boschglobal.github.io/doxysphinx/ <!-- .element: class="fragment" data-fragment-index="2" -->

Note:

In der Unit Test Specification, also direkt im Testcode sieht es etwas anders aus.

"click"

Hier ein Code-Beispiel eines Needs-Elements mit dem Typ "test" in einem rst-Block innerhalb eines Doxygen-Kommentars.

Wie ihr hier sehen könnt, geben wir bei einem test-Element dann auch gleich an, welche spec-Elemente und/oder Requirements getestet werden.

"click"

Um diese rst-Blöcke in unser Sphinx-Dokument zu bekommen, verwenden wir zum einen Doxygen und dazu ein weiteres Open-Source Tool namens doxysphinx.

Doxyshpinx ermöglicht es uns, den kompletten HTML-Output von Doxygen in unser Sphinx-Dokument einzubinden.

--

### Software Unit

![](images/lc_swu.png) <!-- .element: class="fragment" data-fragment-index="2" style="float: right; width: 40%" -->

```C [106: ]
/**
 * @rst
 * .. impl:: Light Controller's main function
 *    :id: SWIMPL_LC-006
 *    :implements: SWDD_LC-001
 * @endrst
 * 
 * @brief Controls the light state.
 *
 * Uses a state machine to determine the light state based on
 * several inputs, e.g., the system's power state.
 */
void lightController(void) {

    switch (currentLightState) {
    case LIGHT_OFF:
...
    }
}
```
<!-- .element: class="fragment" data-fragment-index="1" style="float: left; font-size:10pt; width: 55%" -->

Note:

In der Software Unit sieht es dann ähnlich aus.

"click"

Hier verwenden wir ein Needs-Element vom Typ "impl" und verlinken die spec-Elemente, die wir implementieren.

"click"

Im Sphinx-Dokument erscheint dann in der Beschreibung der dokumentierten Funktion ein impl-Element mit dem Link zu dem entsprechenden spec-Element.

--

### Unit Test Results

![](images/lc_utr.png) <!-- .element: class="fragment" data-fragment-index="2" style="float: right; width: 50%" -->

```rst [1: ]
Unit Test Results
=================

.. test-report:: Unit Test Results
    :id: TEST_RESULT_src_light_controller
    :file: light_controller/junit.xml
```
<!-- .element: class="fragment" data-fragment-index="1" style="float: left; font-size:10pt; width: 45%" -->

https://sphinx-test-reports.readthedocs.io/ <!-- .element: class="fragment" data-fragment-index="2" -->

Note:

Und zu guter Letzt die Unit Test Results.

"click"

Wir verwenden zum Einbinden der Testergebnisse eine weitere Sphinx-Erweiterung namens sphinx-test-reports.

Diese Erweiterung bindet JUnit-XML-Dateien in unser Sphinx-Dokument ein und unterstützt gleichzeitig sphinx-needs.

Die Verlinkung eines Testergebnisses mit dem entsprechenden Test Case erfolgt automatisch über den Namen des Test Cases.

Fazit: mit Sphinx, Sphinx-needs, Doxygen, doxysphinx und sphinx-test-reports haben wir damit alles zusammengebracht, was wir für Automotive SPICE bzgl. Unit Construction und Validation so brauchen.