# Factorio: Defenses

First note: the best defense is a good offense.
If you're under attack, pollution is accelerating biter evolution.
Go take our their base instead of waiting to enter a resource death spiral.

That said, if you'er going for the "Keeping your hands clean" [achievement](https://wiki.factorio.com/Achievements)
or performing another challenge where attacks must be weathered instead of prevented at the source, this might help.



## The Enemy

What we need to defeat.

| [Enemies](https://wiki.factorio.com/Enemies)  | Health    | Physical <br> [Resistances](https://wiki.factorio.com/Damage#Resistance)  | Range <br> (Tiles)    |
| ----------------------------------------------| ---------:| -------------------------------------------------------------------------:| ---------------------:|
| **Biter**                                     |           |                                                                           |
| Small                                         |        15 | <span style="opacity: 25%">*None*</span>                                  |     1 |
| Medium                                        |        75 |  4 <span style="opacity: 25%">/ 10%</span>                                |     1 |
| Large                                         |       375 |  8 <span style="opacity: 25%">/ 10%</span>                                | **2** |
| Behemoth                                      |      3000 | 12 <span style="opacity: 25%">/ 10%</span>                                | **2** |
|                                               |           |                                                                           |
| **Spitter**                                   |           |                                                                           |
| Small                                         |        10 | <span style="opacity: 25%">*None*</span>                                  |    13 |
| Medium                                        |        50 | <span style="opacity: 25%">*None*</span>                                  |    14 |
| Large                                         |       200 | <span style="opacity: 25%">*None*</span>                                  |    15 |
| Behemoth                                      |      1500 | <span style="opacity: 25%">*None*</span>                                  |    16 |
|                                               |           |                                                                           |
|                                               |           |                                                                           |



## Gun Turrets

Your bulkwark defense.
Also the most annoying to supply.
Also the one biters have proper resistances to.

| Stat  | Value                                                                 |
| ------| ----------------------------------------------------------------------|
| Range | 18                                                                    |
| Ammo  | 1 magazine = 10 shots <br> 1 iron ore = 2.5 shots for yellow ammo     |
| Cost  | 10 Copper, 10 Gears, 20 Iron, from <br> 10 Copper Ore, 40 Iron Ore    |
| DPS   | 50/s+                                                                 |

| Ammo                                                                              | Damage <br> Physical  | Cost                                      | Required Tech |
| ----------------------------------------------------------------------------------| ---------------------:| ------------------------------------------| --------------|
| 🟡 [Firearm magazine](https://wiki.factorio.com/Firearm_magazine)                 |  5                    | 4 Iron Ore                                | 🔴
| 🔴 [Piercing rounds magazine](https://wiki.factorio.com/Piercing_rounds_magazine) |  8                    | 9 Iron Ore, 5 Copper Ore                  | 🔴🟢
| 🟢 [Uranium rounds magazine](https://wiki.factorio.com/Uranium_rounds_magazine)   | 24                    | 9 Iron Ore, 5 Copper Ore, 10 Uranium Ore  | 🔴🟢⚫🔵🟡

| [Gun turret] research             | Effect                                                                | Required Tech | Notes |
| ----------------------------------| ----------------------------------------------------------------------| --------------| ------|
| [Gun turret]                      |                                                                       | 🔴            |
|                                   |                                                                       |               |
| [Physical projectile damage]      | 🟡 / 🔴 / 🟢 Damage                                                  |               |
| [Physical projectile damage] 0    |  5.00 /  8.00 /  24.00 <span style="opacity: 25%">(100%)</span>       |               |
| [Physical projectile damage] 1    |  6.05 /  9.68 /  29.04 <span style="opacity: 25%">(121%)</span>       | 🔴            | Keeps 🟡 cost effective vs Medium biters by doubling effective damage (5-4=1 → 6.05-4=2.05)
| [Physical projectile damage] 2    |  7.20 / 11.52 /  34.56 <span style="opacity: 25%">(144%)</span>       | 🔴🟢          |
| [Physical projectile damage] 3    |  9.80 / 15.68 /  47.04 <span style="opacity: 25%">(196%)</span>       | 🔴🟢⚫        |
| [Physical projectile damage] 4    | 12.80 / 20.48 /  61.44 <span style="opacity: 25%">(256%)</span>       | 🔴🟢⚫        | Keeps 🟡 cost effective vs Large biters by more than doubling effective damage (9.8-8=1.8 → 12.8-8=4.8)
| [Physical projectile damage] 5    | 16.20 / 25.92 /  77.76 <span style="opacity: 25%">(324%)</span>       | 🔴🟢⚫🔵      |  Keeps 🟡 cost effective vs Behemoth biters by more than quadrupling effective damage (12.80-12=0.8 → 16.20-12=4.20).
| [Physical projectile damage] 6    | 24.20 / 38.72 / 116.16 <span style="opacity: 25%">(484%)</span>       | 🔴🟢⚫🔵🟡    |
| [Physical projectile damage] 7+   | 33.80+ / 54.08+ / 162.24+ <span style="opacity: 25%">(676%+)</span>   | 🔴🟢⚫🔵🟡⚪  |
|                                   |                                                                       |
| [Weapon shooting speed]           | Rate of Fire                                                          |
| [Weapon shooting speed] 0         | 10 / s <span style="opacity: 25%">(100%)</span>                       |
| [Weapon shooting speed] 1         | 11 / s <span style="opacity: 25%">(110%)</span>                       | 🔴
| [Weapon shooting speed] 2         | 13 / s <span style="opacity: 25%">(130%)</span>                       | 🔴🟢
| [Weapon shooting speed] 3         | 15 / s <span style="opacity: 25%">(150%)</span>                       | 🔴🟢⚫
| [Weapon shooting speed] 4         | 18 / s <span style="opacity: 25%">(180%)</span>                       | 🔴🟢⚫
| [Weapon shooting speed] 5         | 21 / s <span style="opacity: 25%">(210%)</span>                       | 🔴🟢⚫🔵
| [Weapon shooting speed] 6         | 25 / s <span style="opacity: 25%">(250%)</span>                       | 🔴🟢⚫🔵🟡
|                                   |

N.B. [Physical projectile damage] double-applies to turrets, which the above stats reflect.

<!-- https://www.reddit.com/r/factorio/comments/7yydu9/bullet_damage_tech_gets_pretty_insane_when_paired/ -->

Ammo upgrades are suprisingly pricy: red is 3.5x the ore price of yellow.
On the other hand, against medium biters, they do *quadruple* the effective damage (5-4=1 vs 8-4=4)... at least until you start upgrading [Physical projectile damage].

Even if you're avoiding yellow science (e.g. you're aiming for a planetary science first achievement), yellow remains the most cost effective... at least on paper.
With [Physical projectile damage] 5 vs Behemoth Biters, 🟡 does ≈ 4.20 damage, 🔴 does ≈ 13.92 damage.
Probably worth upgrading to avoid turret/wall damage, since the 3.5x resource cost will do ≈3.3x the damage, but technically yellow ammo remains more cost effective.
Especially considering spitters don't have resistances, dropping the bonus to less that double the damage versus them.

OTOH, even large biters might motivate flamethrowers or lasers for extra range - your gun turrets only have 3 range on 'em, which might mean your walls are in range of the spitters, when the spitters aren't in range of your turrets.



## Flamethrower Turrets

Requires engines and crude oil.

| Stat  | Value                                                                 |
| ------| ----------------------------------------------------------------------|
| Range | 6 - 30                                                                |
| Ammo  | 3 Oil/s                                                               |
| Cost  | 5 Engines, 15 Gears, 10 Pipes, 30 Steel                               |
| DPS   | 900/s+ contact, 100/s+ ignited, 13/s+ fire on ground                  |

| [Flamethrower turret] research    | Effect                                            | Required Tech     |
| ----------------------------------| --------------------------------------------------| ------------------|
| [Flamethrower turret]             |                                                   | 🔴🟢⚫
|
| [Refined flammables]              | Damage                                            |
| [Refined flammables] 0            | ...it's complicated...                            |
| [Refined flammables] 1            | <span style="opacity: 25%">(144%)</span>          | 🔴🟢⚫
| [Refined flammables] 2            | <span style="opacity: 25%">(196%)</span>          | 🔴🟢⚫
| [Refined flammables] 3            | <span style="opacity: 25%">(256%)</span>          | 🔴🟢⚫🔵
| [Refined flammables] 4            | <span style="opacity: 25%">(361%)</span>          | 🔴🟢⚫🔵🟡
| [Refined flammables] 5            | <span style="opacity: 25%">(484%)</span>          | 🔴🟢⚫🔵🟡
| [Refined flammables] 6            | <span style="opacity: 25%">(676%)</span>          | 🔴🟢⚫🔵🟡
| [Refined flammables] 7+           | <span style="opacity: 25%">(784%+)</span>         | 🔴🟢⚫🔵🟡⚪

N.B. [Refined flammables] double-appiles to turrets.



## Laser Turrets

Requires batteries.

| Stat  | Value                                     |
| ------| ------------------------------------------|
| Range | 24                                        |
| Ammo  | 1 coal (4 MJ) ≈ 5 shots (5x800 KJ)        |
| Cost  | 12 Batteries, 30 Copper, 20 Iron, 20 Steel, from: <br> 42 Copper Ore, 137 Iron Ore, 25 Sulfur    |

| [Laser turret] research           | Effect                                            | Required Tech |
| ----------------------------------| --------------------------------------------------| --------------|
| [Laser turret]                    |                                                   | 🔴🟢⚫🔵
|                                   |                                                   |
| [Laser weapons damage]            | Damage                                            |
| [Laser weapons damage] 0          | 20 <span style="opacity: 25%">(100%)</span>       |
| [Laser weapons damage] 1          | 24 <span style="opacity: 25%">(120%)</span>       | 🔴🟢⚫🔵
| [Laser weapons damage] 2          | 28 <span style="opacity: 25%">(140%)</span>       | 🔴🟢⚫🔵
| [Laser weapons damage] 3          | 34 <span style="opacity: 25%">(170%)</span>       | 🔴🟢⚫🔵
| [Laser weapons damage] 4          | 42 <span style="opacity: 25%">(210%)</span>       | 🔴🟢⚫🔵
| [Laser weapons damage] 5          | 52 <span style="opacity: 25%">(260%)</span>       | 🔴🟢⚫🔵🟡
| [Laser weapons damage] 6          | 66 <span style="opacity: 25%">(330%)</span>       | 🔴🟢⚫🔵🟡
| [Laser weapons damage] 7+         | 80 <span style="opacity: 25%">(400%)</span>       | 🔴🟢⚫🔵🟡⚪
|                                   |
| [Laser shooting speed]            | Rate of Fire                                      |
| [Laser shooting speed] 0          | 1.50 / s <span style="opacity: 25%">(100%)</span> |
| [Laser shooting speed] 1          | 1.65 / s <span style="opacity: 25%">(110%)</span> | 🔴🟢⚫🔵
| [Laser shooting speed] 2          | 1.95 / s <span style="opacity: 25%">(130%)</span> | 🔴🟢⚫🔵
| [Laser shooting speed] 3          | 2.40 / s <span style="opacity: 25%">(160%)</span> | 🔴🟢⚫🔵
| [Laser shooting speed] 4          | 2.85 / s <span style="opacity: 25%">(190%)</span> | 🔴🟢⚫🔵
| [Laser shooting speed] 5          | 3.45 / s <span style="opacity: 25%">(230%)</span> | 🔴🟢⚫🔵🟡
| [Laser shooting speed] 6          | 4.05 / s <span style="opacity: 25%">(270%)</span> | 🔴🟢⚫🔵🟡
| [Laser shooting speed] 7          | 4.80 / s <span style="opacity: 25%">(320%)</span> | 🔴🟢⚫🔵🟡
|                                   |



## Walls

| Item                                                      | Cost (Approx)                             | Health    |
| ----------------------------------------------------------| ------------------------------------------| ---------:|
| [Stone furnace](https://wiki.factorio.com/Stone_furnace)  | 5 Stone Ore                               |  200      |
| [Stone wall](https://wiki.factorio.com/Wall)              | 10 Stone Ore                              |  350      |
| [Gun Turret]                                              | 10 Copper Ore, 40 Iron Ore                |  400      |
| [Laser Turret]                                            | 42 Copper Ore, 137 Iron Ore, 25 Sulfur    | 1000      |
| [Solar panel](https://wiki.factorio.com/Solar_panel)      | 27 Copper Ore, 40 Iron Ore                |  200      |



## Land Mines

[Land mine](https://wiki.factorio.com/Land_mine)s are great... once you get oil and construction robots.



<!-- References -->

[Gun turret]:                   https://wiki.factorio.com/Gun_turret
[Physical projectile damage]:   https://wiki.factorio.com/Physical_projectile_damage_(research)
[Weapon shooting speed]:        https://wiki.factorio.com/Weapon_shooting_speed_(research)

[Flamethrower turret]:          https://wiki.factorio.com/Flamethrower_turret
[Refined flammables]:           https://wiki.factorio.com/Refined_flammables_(research)

[Laser turret]:                 https://wiki.factorio.com/Laser_turret
[Laser weapons damage]:         https://wiki.factorio.com/Laser_weapons_damage_(research)
[Laser shooting speed]:         https://wiki.factorio.com/Laser_shooting_speed_(research)
