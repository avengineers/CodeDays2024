## A Practical Example of Software Product Line Engineering from the Automotive Industry

Target Audience: SW Developers, Architects and DevOps
Prerequisites: Experience with C/C++ might be an advantage.
Level: Advanced

In the automotive sector, suppliers provide ready-made products to car manufacturers to boost revenue and stay competitive. But car manufacturers demand customised products to gain unique selling points. Every client request can potentially require a new software project. Simply copying an old one ensures initial functionality, but results in enduring software erosion. The use of Software Product Lines (SPL) allows for the maximisation of reusability and quality. Our open-source system, which relies on VS Code and CMake, facilitates Test-Driven Development (TDD) in the C/C++ language and enables Continuous Integration (CI) for SPLs.


* Challenges für den automotive Software-Entwickler
  * Projektvielfalt --> Produktfamilie --> Konfigurierbarkeit --> SPLE
    * Zeit (Baselines)
    * Raum(Varianten)
    * eigentlich 3d, aber der Einfachheit halber hier nur 2
  * ASPICE (Verifikation --> wie stelle ich sicher, dass in allen Varianten die 
  Anforderungen korrekt abgedeckt sind, Reifegrad)
    * Überblick über alle Prozesse
    * dann mit Fokus für den Vortrag
  * das gehen wir alles mit Open Source Lösungen an
    * --> konfigurierbare Sourcen --> SPL
      * Git
      * KConfig
      * CMake und CMake Tools (Varianten/ Build Kits)
      * Code und Test Beispiele
    * Ergebnisse/artefakte (Sphinx + Doxygen)
      * Detailed Design
      * Unit Test Spec
      * Unit Test Results
    * Traceability (Sphinx needs)
      * unique ids
      * traceability
* CI/CD
  * mini Pipeline
  * Pytest
