<!-- .slide: data-transition="none-out" -->

## Automotive Software Process Improvement and Capability Determination

A.SPICE

--

<!-- .slide: data-transition="zoom-in none-out" -->

![](images/aspice_model.png)

--

<!-- .slide: data-transition="fade" -->

![](images/aspice_model_swe.png)

--

<!-- .slide: data-transition="slide-in fade-out" -->

![](images/aspice_traceability.png)

--

<!-- .slide: data-transition="fade" -->

![](images/aspice_traceability_swe34.png)

conf.py:<!-- .element: class="fragment" data-fragment-index="2" -->
```python [152: 1|3,4|5,6|7,8|9,10]
extensions.append("sphinx_needs")

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
    * Ergebnisse/artefakte (Sphinx + Doxygen)
      * Detailed Design
      * Unit Test Spec
      * Unit Test Results
    * Traceability (Sphinx needs)
      * unique ids
      * traceability
