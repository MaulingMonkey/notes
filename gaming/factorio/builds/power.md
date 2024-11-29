# Factorio: Power

## Boiler → Steam Engine

| Qty   | Building          |
| -----:| ------------------|
|     1 | Offshore Pump     |
|   200 | Boiler            |
|   200 | Burner Inserter   |
|   400 | Steam Engine      |

A full build of this will generate 360 MW (less than a 2x2 nuclear setup!) by consuming one of:

| Qty   | Fuel          | MJ    | Belts         |
| -----:| --------------| -----:| --------------|
| 180/s | Wood          | 2     | 4 Blue        |
|  90/s | Coal          | 4     | 2 Blue        |
|  30/s | Solid Fuel    | 12    | 1 Red         |
| 3.6/s | Rocket Fuel   | 100   | \<1 Yellow    |

N.B. Rocket Fuel is a bit less efficient than Solid Fuel, even if it has more capacity.
In 1.x the ratio was `1:20:40` - `1:200:400` represents 2.x numbers.



## Burner Tower → Steam Turbine

Another alternative for fuel burning.
More fuel efficient, but generates more pollution per MW, and requires post-nuclear tech.
Mainly useful for Gleba?

| Qty   | Building          | Notes             |
| -----:| ------------------| ------------------|
|     1 | Offshore Pump     | Water: 1,200/s    |
| 116.5 | Heat Exchanger    | Heat: 1,165 MW    |
| 29.125| Burner Tower      | Fuel:  466 MW     |
|   200 | Steam Turbine     | Power: 1,165 MW   |

| Qty   | Fuel          | MJ    |
| -----:| --------------| -----:|
| 233/s | Wood          | 2     |
| 116/s | Coal          | 4     |
|  39/s | Solid Fuel    | 12    |
| 4.6/s | Rocket Fuel   | 100   |



## Nuclear

| Layout    | Pumps | Reactors  | Exchangers    | Turbines  | Power     |
|:---------:| -----:| ---------:| -------------:| ---------:| ---------:|
| 1 x 1     | 1     |         1 |             4 |         7 |     40 MW |
| 2 x 1     | 1     |         2 |            16 |        28 |    160 MW |
| 2 x 2     | 1     |         4 |            48 |        83 |    480 MW |
| 2 x 3     | 1     |         6 |            80 |       138 |    800 MW |
| 2 x 4     | 1     |         8 |           112 |       193 |   1120 MW |
| ...
| 2 x 7     | 2     |        14 |           208 |       358 |   2080 MW |
| 2 x 8     | 3     |        16 |           240 |       413 |   2400 MW |


## Solar

Main ratio:
-   ≈1x Solar Panel
-   ≈1x Accumulator (assuming steady consumption.  laser defenses might warrant more storage.)



## Vulcanus

| Qty   | Building                              | Notes |
| -----:| --------------------------------------| ------|
| 1     | Chemical Plant (Acid neutralization)  | 200 Sulfuric acid → 2,000 Steam/s
| 33.33 | Steam Turbines                        | 60 Steam/s each, 193.9806 MW



## References
-   <https://wiki.factorio.com/Power_production>
-   <https://wiki.factorio.com/Nuclear_reactor>
-   <https://wiki.factorio.com/Tutorial:Nuclear_power#Neighbor_bonus>
-   <https://wiki.factorio.com/Heat_exchanger>
-   <https://wiki.factorio.com/Steam_turbine>
-   <https://wiki.factorio.com/Heating_tower>
-   <https://wiki.factorio.com/Boiler>
