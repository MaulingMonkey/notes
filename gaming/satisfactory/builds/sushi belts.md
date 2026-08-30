# Builds: Sushi Belts

It's trivial to create a sushi belt where you sink everything.
This instead covers sushi where you stop production on overflow.
To do this, the belt must *loop*.

```text
upstream (sushi)
    ⇓
    ⇓
    ⯐           Smart Splitter:     ⇙ Any Undefined     ⇘ Specific Item
  ⇙   ⇘
⇓       ⯐       Smart Splitter:     ⇓ Any               ⇘ Overflow
⇓       ⇓ ⇘
⇓       ⇓  🗑🗑🗑  Awesome Sink (only used after massive over-production)
⇓       ⇓  🗑🗑🗑
⇓       ⇓
⇓      ▦▦       Storage            "Over-Production"
⇓       ⇓
⇓       ⇓   ?    Input: Production
⇓       ⇓ ⇙
⇓       ▣       Priority Merger     ⇓ High              ⇙ Low
⇓       ⇓
⇓      ▦▦       Storage             "Production"
⇓       ⇓
⇓       ⯐       Splitter            Can be made smart to allow prioritization.
⇓       ⇓ ⇘
⇓       ⇓  ⛋⛋  Dimensional Depot  Optional, but this is where I recommend it.
  ⇘   ⇙
    ▣           Merger              Can be made smart to allow prioritization.
    ⇓
    ⇓
    ⇓
downstream (eventually loops back to upstream)
```

## Notes

The first container (Over-Production) "should" remain nearly empty.
Might surge if production is full and you decrease the rate items merge onto the sushi belt.
Might surge if the main line is too full to merge onto.

Limit sushi belt ingress to avoid flooding main line:
-   Mk. 1 Belts?
-   Splitter looping back to priority merger?  Could reuse before-storage merger if you're into that kind of thing.
