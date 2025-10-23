# Fulgora



## Build Layout: 2 Miners → 1 Recycler

2 scrap miners can feed a little under 1 recycler at base speeds.
They can be used in-situ quite nicely:

```text
 ◘◘◘◘  ◘◘◘◘
►◘◘◘→►►◘◘◘→►
☼↑☼☼↑☼☼↑☼☼↑☼
☼☼☼☼☼☼☼☼☼☼☼☼
☼☼☼☼☼☼☼☼☼☼☼☼
 ◘◘◘◘  ◘◘◘◘
►◘◘◘◘►►◘◘◘◘►
☼↑☼☼↑☼☼↑☼☼↑☼
☼☼☼☼☼☼☼☼☼☼☼☼
☼☼☼☼☼☼☼☼☼☼☼☼

☼↑: Miner
◘:  Recycler
►:  Underground belt weaving or bot chests
```



## Build Layout: 3-1 Filtering Splitters

1/3rd of [Scrap](https://wiki.factorio.com/Scrap) recycling's output is gears.
This makes 3-1 filtering splitter setups ≈ideal for simple high throughput early settling.

```text
       ▲▲▲
       ▲▲▲
►►►►►►☼▲▲▲                             ◘►►►►►►►
►►►►►☼◘☼▲▲  ←  filtered belts         ◘☼◘►►►►►►
►►►►☼◘☼◘☼▲                           ◘☼◘☼◘►►►►►
    ◘☼◘☼◘►►►►►►►►►►►►►►►►►►►►►►►►►►►►☼◘☼◘☼▼
     ◘☼◘►►►  unfiltered mainline  ►►►►☼◘☼▼▼
      ◘►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►☼▼▼▼
                                        ▼▼▼
                     filtered belts  →  ▼▼▼
                                        ▼▼▼

►▲: Belt       (Right, Up)
☼◘: Splitter   (Filtered Priority, Non-Priority)
```



## Scrap Ratios

Recyclers per Belt

| Item                  | R/🟡 | R/🔴 | R/🔵 | R/🟢 | Notes/Priorities |
| ----------------------| -----:| -----:| -----:| -----:| ----- |
| Gears                 |     1 |     2 |     3 |     4 |
| Solid Fuel            |       |       |       |       | P1: Rocket Fuel, Void (2x Recyclers - prefer storing rocket fuel over storing solid fuel?)
| Concrete              |       |       |       |       | P1: Store, P2: Void (→ 2x Hazard Concrete Assemblers ↔ 2x Hazard Concrete Recyclers)
| Ice                   |       |       |       |       | P1: Water, P2: Store (denser than water), P3: Void (2x Recyclers)
| Stone                 |       |       |       |       | SCIENCE! P1: Holmium, P2: Store → Landfill, Bricks, Rail, P5: Void (?x Recyclers)
| Steel                 |       |       |       |       | P1: ???, Rail, P2: Store, P3: Void (→ 2x Steel Chest Assemblers ↔ 2x Steel Chest Recyclers)
| Battery               |       |       |       |       | P1: Accumulators, P2: Store, P3: Recycle for Copper/Iron
| Copper Wire           |     1 |     2 |     3 |     4 | Or maybe half that?  P1: Store (Misc.), P2: Recycler for Copper
| 🔴 Circuit            |
| 🔵 Circuit            |       |       |       |       | Recycle for 🟢🔴, Store, Launch Rockets
| Low Density Structure |       |       |       |       | P1: Store for rocket launches, P2: Recycle for Copper, Plastic (and technically Steel)
| Holmium Ore           |       |       |       |       | SCIENCE!
| Most Ore              |     1 |     2 |     3 |     4 |


## References

-   <https://wiki.factorio.com/>
    -   [Scrap](https://wiki.factorio.com/Scrap)
    -   [Recycler](https://wiki.factorio.com/Recycler)
    -   [Tutorial: Scrap recycling strategies](https://wiki.factorio.com/Tutorial:Scrap_recycling_strategies)
