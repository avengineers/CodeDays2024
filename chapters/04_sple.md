## SPLE

Note:
  * Projektvielfalt --> Produktfamilie --> Konfigurierbarkeit --> SPLE
    * Zeit (Baselines)
    * Raum(Varianten)
    * eigentlich 3d, aber der Einfachheit halber hier nur 2

--

<!-- .slide: data-background-image="images/classic-projects.png" data-background-size="contain" -->

--

<!-- .slide: data-background-image="images/components.png" data-background-size="contain" -->

--

<!-- .slide: data-background-image="images/isolated-project-development.png" data-background-size="contain" -->

--

<!-- .slide: data-background-image="images/component-development.png" data-background-size="contain" -->

--

<!-- .slide: data-background-image="images/sple-development.png" data-background-size="contain" -->

--

## CMake + VS Code + KConfig + TDD

Note:

  * das gehen wir alles mit Open Source Lösungen an
    * --> konfigurierbare Sourcen --> SPL
      * Git
      * KConfig
      * CMake und CMake Tools (Varianten/ Build Kits)
      * Code und Test Beispiele

--

```json [2-4|5-10|11-19|21]
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

--

<!-- .slide: data-background-image="images/select-variant.png" data-background-size="contain" -->

--

<!-- .slide: data-background-image="images/build-component.png" data-background-size="contain" -->

--

<!-- .slide: data-background-image="images/kconfig.png" data-background-size="contain" -->

--

execution mit Unterschiedlichen Farben noch zeigen

--

<!-- .slide: data-background-image="images/test.png" data-background-size="contain" -->


