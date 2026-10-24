# EVE Online: Missiles



## Launcher Categories

| Size  | Category                          | Range     | Damage    | Notes     |
|:-----:| ----------------------------------| ----------| ----------| ----------|
|   S   | Rocket Launcher                   | Short     | High      |
|   S   | Light Missile Launcher            | Long      | Mid       |
|       |                                   |           |           |
|   M   | Heavy Assault Missile Launcher    | Short     | High      |
|   M   | Rapid Light Missile Launcher      | Mid       | Mid       | Frequent reloads, but better application to small targets
|   M   | Heavy Missile Launcher            | Long      | Mid       |
|       |                                   |           |           |
|   L   | Torpedo Launcher                  | Short     | High      |
|   L   | Rapid Heavy Missile Launcher      | Mid       | Mid       | (Verify:)  Frequent reloads, but better application to small targets
|   L   | Cruise Missile Launcher           | Long      | Mid       |
|       |                                   |           |           |
|  XL   | XL Torpedo Launcher               | Short?    | High?     | (Verify)
|  XL?  | Rapid Torpedo Launcher            | Mid?      | High?     | (Verify)
|  XL   | XL Cruise Missile Launcher        | Long      | Mid       | (Verify)



## TODO

-   Launchers
-   Modifying Modules + Rigs
-   Ships with Bonuses?



## Omega Required

An omega account is required for:
-   Auto-Targeting Missiles
-   Defender Missiles
-   Tech II Missiles
-   Tech II Launchers



## Common Missile Variations

All missiles (including faction and tech 2 variants) share a common naming scheme for damage types:

| Damage Flavor     | Damage Type   | Notes |
| ------------------| --------------| ------|
| Mjolnir           | EM            |
| Nova              | Explosive     |
| Scourge           | Kinetic       |
| Inferno           | Thermal       |
| Standup           | Omni          | Only seen for *Structure Anti(sub)capital Missiles*

| Tech II Flavor    | Range                         | Damage                                                        |
| ------------------| ------------------------------| --------------------------------------------------------------|
| Javelin           | Longer (higher velocity)      | Less damage                                                   |
| Precision         | Shorter (less flight time)    | More damage to small targets only (faster focused explosions) |
| Fury              | Shorter (less flight time)    | More base damage, less focused explosion                      |
| Rage              | Shorter (slower)              | More base damage, less focused explosion                      |

| Faction           | Notes                                                                     |
| ------------------| --------------------------------------------------------------------------|
| Guristas          | Frequently relatively cheap, slight damage boost, more missile HP         |
| Caldari Navy      | Still affordable, better damage boost                                     |
| Dread Guristas    | $$$Expensive$$$, generally not worth the further increased damage boost   |
| Legion            | Auto-Targeting Only.  More damage.                                        |



## Missile HP

Missiles can theoretically be shot down with smartbombs.
Defender missiles used to also work, but they've been nerfed to only target bombs, and to only fit on destroyers.

| Variation                     |             Basic |          Guristas | Auto-Targeting    |
| ------------------------------| -----------------:| -----------------:| -----------------:|
| Rocket                        |                90 |               120 | <span style="opacity: 25%">N/A</span> |
| Light                         |                60 |                80 |                70 |
| Heavy                         |               120 |               160 |                70 |
| Cruise                        |               240 |               320 |                70 |
| Torpedo                       |               360 |               480 | <span style="opacity: 25%">N/A</span> |



## Rockets

| Variation             | Tech          | Damage    | Velocity      | Flight <br> Time  | Explosion <br> Radius | Explosion <br> Velocity   | Notes     |
| ----------------------|:-------------:| ---------:| -------------:|:-----------------:|:---------------------:|:-------------------------:| ----------|
| Basic                 | Tech I        |    33.0   |   2,250 m/s   |          2.00 s   |                20 m   |                 150 m/s   |
| Guristas              | Faction       |    35.6   |   2,250 m/s   |          2.00 s   |                20 m   |                 150 m/s   |
| Caldari Navy          | Faction       |    37.0   |   2,250 m/s   |          2.00 s   |                20 m   |                 150 m/s   |
| Dread Guristas        | Faction       |    39.6   |   2,250 m/s   |          2.00 s   |                20 m   |                 150 m/s   | Obscenely Expensive
| Javelin               | **Tech II**   |    29.7   | **3,375 m/s** |          2.00 s   |                20 m   |                 150 m/s   |
| Rage                  | **Tech II**   |    51.3   | **1,875 m/s** |          2.00 s   |              **34 m** |               **129 m/s** | Reduced Inertia Modifier



## Light Missiles

All light missiles have:
-   A base flight speed of 3,750 m/sec.
-   An additional 100 km range limit for auto-targeting versions

| Variation             | Tech          | Damage    | Flight <br> Time  | Explosion <br> Radius | Explosion <br> Velocity   | Notes     |
| ----------------------|:-------------:| ---------:|:-----------------:|:---------------------:|:-------------------------:| ----------|
| Basic                 | Tech I        |      83   |          5.00 s   |                40 m   |                 170 m/s   |
| Guristas              | Faction       |    **90** |          5.00 s   |                40 m   |                 170 m/s   | +8%
| Caldari Navy          | Faction       |    **95** |          5.00 s   |                40 m   |                 170 m/s   | +15%
| Dread Guristas        | Faction       |    **99** |          5.00 s   |                40 m   |                 170 m/s   | +19%, obscenely expensive
| Precision             | **Tech II**   |      83   |        **2.50 s** |              **25 m** |                 204 m/s   | +0%, Shorter Range, Application
| Fury                  | **Tech II**   |   **116** |        **3.75 s** |              **69 m** |                 143 m/s   | +40%, Shorter Range
|                       |               |           |                   |                       |                           |
| Auto-Targeting        | Tech I        |      62   |          5.00 s   |                40 m   |                 170 m/s   |
| Legion Auto-Targeting | Faction       |    **70** |          5.00 s   |                40 m   |                 170 m/s   |

| ISK       | `S` Launcher                          | Tech          |       RoF |        m3 |       CPU |     Power |      Heat | Notes     |
| ---------:| --------------------------------------| --------------| ---------:| ---------:| ---------:| ---------:| ---------:| ----------|
|           | TODO

| ISK       | `M` Launcher                                      | Tech          |       RoF |        m3 |       CPU |     Power |      Heat | Notes     |
| ---------:| ------------------------------------------        | --------------| ---------:| ---------:| ---------:| ---------:| ---------:| ----------|
|       23k | Rapid Light Missile Launcher I                    | Tech I        |     7.8 s |     0.285 |     35 tf |     73 MW |   1.80 HP |
|        8k | Experimental SV-2000 Rapid Light Missile Launcher | Meta          |     6.6 s |     0.285 |     33 tf |     73 MW |   1.50 HP |
|        2k | Limited 'Limos' Rapid Light Missile Launcher      | Meta          |     7.0 s |     0.285 |     32 tf |     73 MW |   1.60 HP |
|      707k | Prototype 'Arbalest' Rapid Light Missile Launcher | Meta          |     6.2 s |     0.285 |     30 tf |     73 MW |   1.40 HP |
|        6k | Upgraded 'Malkuth' Rapid Light Missile Launcher   | Meta          |     7.4 s |     0.285 |     28 tf |     73 MW |   1.70 HP |
|      743k | Rapid Light Missile Launcher II                   | **Tech II**   |     6.2 s |     0.300 |     39 tf |     77 MW |   1.40 HP |
|    8,084k | 'Pickaxe' Rapid Light Missile Launcher            | Storyline     |     7.7 s |     0.300 |     30 tf |     65 MW |   1.80 HP |
|  105,305k | Caldari Navy Rapid Light Missile Launcher         | Faction       |     5.5 s |     0.315 |     39 tf |     65 MW |   1.30 HP |
|   50,489k | Domination Rapid Light Missile Launcher           | Faction       |     6.0 s |     0.315 |     28 tf |     73 MW |   1.40 HP |
|   70,115k | Dread Guristas Rapid Light Missile Launcher       | Faction       |     5.5 s |     0.315 |     39 tf |     65 MW |   1.30 HP |
|   96,980k | Republic Fleet Rapid Light Missile Launcher       | Faction       |     6.0 s |     0.315 |     28 tf |     73 MW |   1.40 HP |
|      N/A  | True Sansha Rapid Light Missile Launcher          | Faction       |     6.0 s |     0.315 |     28 tf |     73 MW |   1.40 HP |

Self manufacture details:

| Missile               | Qty   | Tritanium | Pyerite   | Mexallon  |
| ----------------------| -----:| ---------:| ---------:| ---------:|
| Inferno Light Missile |   100 |        58 |        72 |           |
| Mjolnir Light Missile |   100 |       150 |        18 |        12 |
| Nova Light Missile    |   100 |        54 |        16 |         6 |
| Scourge Light Missile |   100 |        69 |        43 |         2 |

| Ore                           | Tritanium | Pyerite   | Mexallon  |
| ------------------------------| ---------:| ---------:| ---------:|
| Veldspar                      |      4000 |           |           |
| Scordite                      |      1000 |       733 |           |
| Plagioclase                   |       300 |           |       233 |
| Pyroxeres                     |           |       300 |       100 |



## Heavy Assault Missiles

TODO



## Heavy Missiles

All heavy missiles have:
-   A base flight speed of 4,300 m/sec
-   An additional 150 km range limit for auto-targeting versions

| Missile Variation     | Tech          | Damage    |   %   | Flight <br> Time  | Explosion <br> Radius | Explosion <br> Velocity   | Notes     |
| ----------------------|:-------------:| ---------:| -----:|:-----------------:|:---------------------:|:-------------------------:| ----------|
| Basic                 | Tech I        |     149   | base  |          6.50 s   |               140 m   |                  85 m/s   |
| Guristas              | Faction       |   **164** | +10 % |          6.50 s   |               140 m   |                  85 m/s   |
| Caldari Navy          | Faction       |   **171** | +15 % |          6.50 s   |               140 m   |                  85 m/s   |
| Dread Guristas        | Faction       |   **179** | +20 % |          6.50 s   |               140 m   |                  85 m/s   | obscenely expensive
| Precision             | **Tech II**   |     149   |       |        **3.25 s** |             **125 m** |               **102 m/s** |
| Fury                  | **Tech II**   |   **201** | +35 % |        **4.88 s** |             **241 m** |                **71 m/s** |
|                       |               |           |       |                   |                       |                           |
| Auto-Targeting        | Tech I        |     107   | -28 % |          6.50 s   |               140 m   |                **81 m/s** |
| Legion Auto-Targeting | Faction       |     122   | -18 % |          6.50 s   |               140 m   |                **81 m/s** |

| ISK       | `M` Launcher                          | Tech          |       RoF |        m3 |       CPU |     Power |      Heat | Notes     |
| ---------:| --------------------------------------| --------------| ---------:| ---------:| ---------:| ---------:| ---------:| ----------|
|       73k | Heavy Missile Launcher I              | Tech I        |    15.0 s |     0.900 |   50.0 tf |    100 MW |   2.30 HP | **DON'T BUY:** Meta is cheaper and better
|      560k | 'Arbalest' Heavy Missile Launcher     | Meta          |    12.0 s |     1.080 |   43.0 tf |    100 MW |   1.80 HP | Best meta damage, although maybe a bit pricy for a T1 cruiser
|       15k | 'Malkuth' Heavy Missile Launcher      | Meta          |    14.3 s |     0.960 |   40.0 tf |    100 MW |   2.10 HP | Small DPS gains over basic T1, but easiest fit
|       15k | Advanced 'Limos' Heavy Missle Bay I   | Meta          |    13.5 s |     0.990 |   45.0 tf |    100 MW |   2.00 HP | Middle of the road between fitting and dps improvements
|       35k | XR-3200 Heavy Missle Bay              | Meta          |    12.8 s |     1.050 |   48.0 tf |    100 MW |   1.90 HP | Second best meta damage, cheap
|    1,079k | Heavy Missile Launcher II             | **Tech II**   |    12.0 s |     1.200 |   55.0 tf |    105 MW |   1.80 HP | Requires skills, but alpha friendly
|  102,734k | Caldari Navy Heavy Missile Launcher   | Faction       |    10.5 s |     1.350 |   55.0 tf |     90 MW |   1.60 HP |
|   44,033k | Domination Heavy Missile Launcher     | Faction       |    11.6 s |     1.230 |   40.0 tf |    100 MW |   1.70 HP |
|  107,660k | Dread Guristas Heavy Missile Launcher | Faction       |    10.5 s |     1.350 |   55.0 tf |     90 MW |   1.60 HP |
|      N/A  | True Sansha Heavy Missile Launcher    | Faction       |    11.6 s |     1.230 |   40.0 tf |    100 MW |   1.70 HP |
|   22,563k | Republic Fleet Heavy Missile Launcher | Faction       |    11.6 s |     1.230 |   40.0 tf |    100 MW |   1.70 HP |
|   36,107k | 'Undertaker' Heavy Missile Launcher   | Storyline     |    13.5 s |     0.900 |   43.0 tf |     90 MW |   2.00 HP |



## Cruise Missiles

All cruise missiles have:
-   A base flight speed of 4,700 m/sec.
-   An additional 200 km range limit for auto-targeting versions

| Variation             | Tech          | Damage    | Flight <br> Time  | Explosion <br> Radius | Explosion <br> Velocity   | Notes     |
| ----------------------|:-------------:| ---------:|:-----------------:|:---------------------:|:-------------------------:| ----------|
| Basic                 | Tech I        |     375   |         14.00 s   |               330 m   |                  69 m/s   |
| Guristas              | Faction       |   **413** |         14.00 s   |               330 m   |                  69 m/s   |
| Caldari Navy          | Faction       |   **431** |         14.00 s   |               330 m   |                  69 m/s   |
| Dread Guristas        | Faction       |   **450** |         14.00 s   |               330 m   |                  69 m/s   |
| Precision             | **Tech II**   |     375   |        **7.00 s** |             **297 m** |                **83 m/s** |
| Fury                  | **Tech II**   |   **525** |       **10.50 s** |             **567 m** |                **58 m/s** |
|                       |               |           |                   |                       |                           |
| Auto-Targeting        | Tech I        |     281   |         14.00 s   |               330 m   |                  69 m/s   |
| Legion Auto-Targeting | Faction       |   **323** |         14.00 s   |               330 m   |                  69 m/s   |



## Torpedos

| Variation             | Tech          | Damage    | Velocity      | Flight <br> Time  | Explosion <br> Radius | Explosion <br> Velocity   | Notes     |
| ----------------------|:-------------:| ---------:| -------------:|:-----------------:|:---------------------:|:-------------------------:| ----------|
| Basic                 | Tech I        |     450   |   1,800 m/s   |            7.2 s  |               405 m   |               1,800 m/s   |
| Guristas              | Faction       |   **495** |   1,800 m/s   |            7.2 s  |               405 m   |               1,800 m/s   |
| Caldari Navy          | Faction       |   **517** |   1,800 m/s   |            7.2 s  |               405 m   |               1,800 m/s   |
| Dread Guristas        | Faction       |   **540** |   1,800 m/s   |            7.2 s  |               405 m   |               1,800 m/s   |
| Javelin               | **Tech II**   |   **405** | **2,700 m/s** |            7.2 s  |               405 m   |             **2,700 m/s** |
| Rage                  | **Tech II**   |   **699** | **1,500 m/s** |            7.2 s  |             **697 m** |             **1,500 m/s** |



## XL Cruise Missiles

TODO



## XL Torpedoes

TODO



## Defenders

Used with a dedicated defender launcher.  "Destroy incoming bombs".
This used to work against NPC torpedos, no idea if it still does.
This used to be fittable to any ship.  Now they can only be fit to destroyers.
Requires an Omega account.



## Legacy Content

-   Missiles such as "Arch Angel {damage type} F.O.F. Light Missile" are removed.
-   I have 400 Heavy Defender Missile Is I can't do anything with.
