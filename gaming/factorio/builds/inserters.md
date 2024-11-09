# Overview

A single row of assemblers feeding neighbors works:

| Assembler     | Plate | Gear  | Green | →Chest| →Belt | Other                                                         | Notes |
| --------------| -----:| -----:| -----:| -----:| -----:| --------------------------------------------------------------| ------|
| Burner        |     1 |     1 |       |  1.20 |  1.19 |                                                               | For boiler use only.
| Long-handed   |     1 |     1 |       |  2.31 |  2.35 | 1x Regular inserter                                           |
| Regular       |     1 |     1 |     1 |  1.67 |  1.64 |                                                               |
| Fast          |     2 |       |     2 |  4.29 |  4.44 | 1x Regular inserter                                           |
| Bulk          |       |    15 |    15 |  7.50 | ≈7.06 | 1x Fast inserter, 1x Red circuit                              | Gear/circuit hog.
| Stack         |       |       |       |       |       | 1x Bulk inserter, 1x Blue circuit, 10x Jelly, 2x Carbon fiber | Gleba only.

Stack inserters require Jelly, which spoils in 4 minutes.
Jelly's source, Jellynuts, last longer - but the seeds should be kept on Gleba.
As such, stack inserters need not be planned for on Nauvis's construction plans.

Inserter throughputs are way more complicated than the columns would imply, but I've made the following simplifying assumptions:
-   <https://wiki.factorio.com/Inserters#Inserter_Throughput> is accurate
-   Belt → Chest → Belt is roughly equivalent to Belt → Assembler → Belt
-   `Inserter capacity bonus 2` is researched
-   Red belts are in use
-   Grabbing from far belt lane
