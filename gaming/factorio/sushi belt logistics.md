# Factorio: Sushi Belt Logistics

-   Early red/green wires makes this something that can be done from the very start.
-   Centralizing storage is way nicer than bot spam.
-   A single fast-inserter trash box will auto-sort trash without overwhelming the sinks.


## Mall

-   A bunch of storage chests loading from the sushi belt
-   Nice and compact
-   A single fast-inserter from trash bin to sushsi belt for auto-sorting


## Direct-to-sushi Assemblers

-   Always keep a tile gap between assemblers for easy refactoring.
-   Each side of the assembler can support 3 belts.
    I *think* I recommend the following layout, from top to bottom.
    However, it introduces weaving belts/inserters early, unless you bump the sushi belt one closer:
    -   Dedicated: Stone, Coal
    -   Dedicated: Copper, Bricks
    -   Dedicated: Iron, Steel
    -   (Assemblers)
    -   Dedicated: Circuits, Gears
    -   Dedicated: Red Circuits, Blue Circuits?
    -   **Sushi**
-   For weaving, start with middle of assembler to middle of assembler as a starting point, I think.
-   Initial power line at the top edge of the assemblers.



## Multi-Destination Items (Intermediate products shipped via sushi belt, personal logistics)

#### Production inserter

-   Connect inserter to belt.
-   Limit insertion count to desired belt capacity.
-   Set insertion item explicitly.  (This can't be set to "what the inserter is holding", right?  What about "what's being manufactured"?)

#### Storage inserter (Personal logistics only)

-   Connect inserter to storage.
-   Limit insertion count to desired storage capacity.
-   Set logistics item to wildcard for easier copy+paste?
-   Set filter to single item.



## Single-Destination Items

This variation allows you to keep the belt clean:

#### Production inserter

-   Connect inserter to storage instead of belt.  (Run via power poles at the top of the manufacturers?)
-   Limit insertion count to desired storage capacity.

### Storage inserter

-   Can aggregate multiple items.  Kinda like it, kinda hate it.



## Military Trains

#### Cargo

-   Ammo, turrets, walls, repair packs, construction bots.
-   Reserve slots with middle mouse button.

### Home Base Station (Load Supplies)

-   Load via whitelist filter inserters.
-   Unload trash via blacklist filter inserter.
-   Set station → train insertion limits if you want the train to support trash collection.
-   Perhaps load directly from sushi belt?
-   If using buffer chests, inserters into chests don't need circuit logic: just use filters and cap the chest.
-   Set wait condition to `(Inactivity OR Timeout)`.

#### Destination Stations (Unload Supplies)

-   For trash:  Use a storage chest.  Blacklist repair packs on the inserter.
-   Use 🔴🟢 circuit network stuff to conditionally enable destination stations when low on any appropriate item.
    -   I think `* < 0` and a constant combinator full of negative values might suffice?
    -   Alternatively, positive constants, and add an arithmetic combinator.
-   Set wait condition to `(Inactivity OR Timeout)` (don't wanna perma-trickle near-full ammo.)
-   Don't use "Single-Destination Item" sushi belt logistics (above.)
