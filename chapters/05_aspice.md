<!-- .slide: data-transition="none-out" -->

## A.SPICE

Automotive Software Process Improvement and Capability Determination <!-- .element: class="fragment" -->

Note:

Matthias hat gerade gezeigt, wie man mit CMake und VSCode eine schöne Software-Produkt-Linie aufbauen kann.

Das ist aber nur ein Teil der Geschichte.

In der Automobilindustrie gibt es eine Reihe von Standards, die eingehalten werden müssen. Einer davon ist A.SPICE.

"click"
Die Abkürzung A.SPICE steht für Automotive Software Process Improvement and Capability Determination.

A.SPICE ist ein Prozessmodell, das die Entwicklung von Software in der Automobilindustrie beschreibt.

Es ist auch ein Reifegradmodell, das die Reifegrade 0 bis 5 definiert.

Diese Reifegrade werden durch die Erfüllung von Prozessanforderungen erreicht und müssen durch definierte Artefakte nachgewiesen werden.

--

<!-- .slide: data-transition="zoom-in none-out" -->

![](images/aspice_model.png)

Note:

Das ist das Prozessmodel von A.SPICE.

Es besteht aus mehreren Prozessgruppen, die wiederum aus Prozessen bestehen.

--

<!-- .slide: data-transition="fade" -->

![](images/aspice_model_swe.png)

Note:

Klar, als Software-Entwickler interessiert uns natürlich vor allem die Software-Entwicklung.

Und uns interessiert natürlich, was von uns erwartet wird.

--

<!-- .slide: data-transition="slide-in fade-out" -->

![](images/aspice_traceability.png)

Note:

Hier sieht man eigentlich recht gut, was von uns erwartet wird.

Jede Menge Artefakte, die wir erstellen müssen.

Und dann muss das alles auch noch miteinander verknüpft werden.

Unser Ziel war es von Anfang an, das alles mit möglichst wenig Aufwand zu erledigen.

--

<!-- .slide: data-transition="fade" -->

![](images/aspice_traceability_swe34.png)

![](images/lc_dir_tree.png)<!-- .element: class="fragment" data-fragment-index="2" -->

Note:

Zu Beginn haben wir uns auf die Prozesse SWE.3 und SWE.4 konzentriert.

Software Detailed Design, Unit Construction und Unit Verification.

Die Units schreiben wir in C und testen sie mit Google Test, das haben wir im Griff.

Aber wie schreiben wir das Software Detailed Design?

Und wie verknüpfen wir das mit den Units und den Unit Tests?

--

## Sphinx und Sphinx-Needs

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

Die Antwort ist Sphinx, Sphinx-Needs, Doxygen und Doxysphinx.

--

### Software Detailed Design

![](images/lc_swdd.png) <!-- .element: class="fragment" data-fragment-index="2" style="float: right; width: 40%" -->

```rst[1: 15-20]
Software Detailed Design
========================

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
<!-- .element: class="fragment" data-fragment-index="1" style="float: left; font-size:10pt; width: 55%" -->

--

### Unit Test Specification

![](images/lc_uts.png) <!-- .element: class="fragment" data-fragment-index="2" style="float: right; width: 40%" -->

```C[50: ]
/*!
* @rst
*
* .. test:: light_controller.test_light_stays_off
*    :id: TS_LC-006
*    :tests: SWDD_LC-001, SWDD_LC-004, R_001
*
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

--

### Productive Code

```C
 /**
  * * @rst
  * .. impl:: Light state
  *    :id: SWIMPL_LC-001
  *    :implements: SWDD_LC-001
  * @endrst
  *
  * @enum LightState
  * @brief Represents the states of the light.
  */
typedef enum {
    LIGHT_OFF,  /**< Represents a state where the light is turned off. */
    LIGHT_ON    /**< Represents a state where the light is turned on with a specific color. */
} LightState;
```
<!-- .element: style="font-size:10pt" -->

Note:
    * Ergebnisse/artefakte (Sphinx + Doxygen)
      * Detailed Design
      * Unit Test Spec
      * Unit Test Results
    * Traceability (Sphinx needs)
      * unique ids
      * traceability
