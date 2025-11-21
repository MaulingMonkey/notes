# Gleba



## Learnings and Gotchyas
-   Don't be confused by the [Tree seeding](https://wiki.factorio.com/Tree_seeding_(research)) research: it's only necessary to get wood seeds for *Nauvis* trees.
    [Agricultural tower](https://wiki.factorio.com/Agricultural_tower)s can plant Gleba trees automatically and only require you to mine some iron bacteria to research.
-   [Heating tower](https://wiki.factorio.com/Heating_tower)s require ye olde boilers.
-   Small stompy bois aren't all that scary compared to behemoth spitters on Nauvis.  Stop wimping out.
-   50% solar power is miserable.  Waiting for fancy power is also miserable.
    Next time, bring ye olde burners.
    Perhaps recycle nauvis's power plant after switching to nuclear?  How expensive is it to launch?
-   Purple → Jellynut, Green/Yellow → Yumako



## Recipe: 10 Spoilage → 1 (1.5) Nutrients
-   Only gleba nutrients recipe that can be made in an assembler.
-   Useful for "cold" starts (only requires items + power.)
-   50% Productivity boost from biochambers, so only build 1 assembler even if you want more nutrients?
-   Disable otherwise so you can hoard spoilage for cold starts?



## Recipe: 4 Yumako Mash → 6 (9) Nutrients
-   Simple, use for most of your nutrients initially?
-   Yumako Mash spoils at 20x the speed of Yumako (3 minutes vs 1 hour), so consider not creating mash until nutrients are desired.
-   Ratios for Yumako → Yumako Mash : Yumako Mash → Nutrients
    -   1:2 Assemblers (but you can't actually make an assembler for the nutrients)
    -   1:3 Biochambers (just use some short belts?)



## Recipe: 5 Bioflux → 40 (60) Nutrients
-   Bioflux requires quick-spoiling inputs from *multiple* plants, so this is way more complicated to set up reliably.
-   Bioflux itself, on the other hand, spoils slow (2 hours vs 1 hour for Yumako or Jellynut)
-   Compounding 50% productivity bonuses mean you can halve your Yumako usage... but you need about a third what you saved in Jellynut, so you're not saving much unless you start stacking productivity modules?
-   Save it for late game?



## Recipe: 1 Yumako → 2 (2.5) Yumako Mash
-   Yumako Mash spoils at 20x the speed of Yumako (3 minutes vs 1 hour), so consider not creating mash until nutrients are desired / using direct insertion styles.



## Recipe: 1 Jellynut → 4 (6) Jelly
-   Jelly spoils at 15x the speed of Jellynut (4 minutes vs 1 hour), so consider not creating jelly until nutrients are desired / using direct insertion styles.



## Recipe: 12 Jelly + 15 Yumako Mash → 4 (6) Bioflux
-   Direct inputs spoil quickly (minutes.)
-   Indirect inputs spoil slow (1 hour.)
-   Final output slows slower (2 hours.)
-   Ideal ratio for un-moduled biochambers: 2 Mash (5/s) : 1 Jelly (4/s) : 2 Bioflux (5+4 → 1.33/s)

```text
        nutrients / spoilage / seeds
   ►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►►…
   ↑ ↓ ↑ ↓ ↑ ↓ ↑ ↓ ↑ ↓   ↑ ↓ ↑ ↓ ↑ ↓ ↑ ↓ ↑ ↓ ▲
   MMM BBB JJJ BBB MMM … MMM BBB JJJ BBB MMM ▲
   MMM→BBB←JJJ→BBB←MMM … MMM→BBB←JJJ→BBB←MMM ▲
   MMM BBB JJJ BBB MMM … MMM BBB JJJ BBB MMM ▲
    ↑   ‡   ↑   ‡   ↑     ↑   ‡   ↑   ‡    ↑ ▲
►►►►►►►►►►►►►►► yumako / jellynut ►►►►►►►►►►→▲ spoilage or burn
        ►►►►►►►►►►► bioflux ►►►►►►►►►►►►►►►►►►►►►►…


←↓↑→:   Inserters
‡:      Long-handed Inserter
MBJ:    Biochambers (Yumako Mash, Bioflux, Jelly)
►:      Belt
◘:      Chest or belt (Bioflux Output)
```
