# Factorio: Signals

-   `Alt`+`G` for **g**reen wire    ("global" signals by convention?)
-   `Alt`+`R` for **r**ed wire      ("local" signals by convention?)
-   `Alt`+`C` for **c**opper wire   (power)

Radars can now carry signals on-planet.



## Technology

| Cost  | Technology            |
|:-----:| ----------------------|
| <span style="opacity: 25%">(free)</span> | Basic circuit wires
|  20x 🔴     10s                       | `Radar`, which can carry signals.
| 100x 🔴🟢   15s                       | `Circuit network`, which provides most combinators.
|  50x 🔴🟢🔵 30s                       | `Advanced combinators`, which provides the `Selector combinator`.



## Components

| Cost                                                  | Item                  |
|:-----------------------------------------------------:| ----------------------|
| <span style="opacity: 25%">(free)</span>              | Wires                 |
| 5x Copper Cable, 5x Electronic circuit                | Arithmetic combinator |
| 5x Copper Cable, 5x Electronic circuit                | Decider combinator    |
| 5x Copper Cable, 2x Electronic circuit                | Constant combinator   |
| 5x Iron plate, 5x Copper Cable, 2x Electronic circuit | Power switch          |
