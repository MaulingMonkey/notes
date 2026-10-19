# EVE Online: Lasers



## TODO

-   Tech II Crystals (can be used without Omega, but requires expensive faction laser turrets!)
-   Faction Variations
-   Turrets
-   Modifying Modules + Rigs
-   Ships with Bonuses?



## Frequency Crystals

Some notes:
-   Each increase in crystal size has exactly double the damage of the previous size
-   Each increase in crystal range has exactly 1/2/4/8 less damage
-   Standard crystals are a sweet spot - they use the least capacitor, and beyond that damage starts to fall off in dire fashion.
-   Damages are listed in the form: EM + Thermal

<table class="crystals">
    <thead>
        <th>
        <th>Name
        <th>Range
        <th>Capacitor
        <th>Damage <br> (S)
        <th>Damage <br> (M)
        <th>Damage <br> (L)
        <th>Damage <br> (XL)
    <tbody>
        <td class="crystal" style="color: #ffffff">⬤
        <td>Multifrequency
        <td>-50.0 %
        <td>0 %
        <td>7+5
        <td>14+10
        <td>28+20
        <td>56+40
    <tr>
        <td class="crystal" style="color: #3a6db4">⬤
        <td>Gamma
        <td>-37.5 %
        <td>-15 %
        <td>7+4
        <td>14+8
        <td>28+16
        <td>56+32
    <tr>
        <td class="crystal" style="color: #139013">⬤
        <td>Xray
        <td>-25.0 %
        <td>-25 %
        <td>6+4
        <td>12+8
        <td>24+16
        <td>48+32
    <tr>
        <td class="crystal" style="color: #9a29a6">⬤
        <td>Ultraviolet
        <td>-12.5 %
        <td>-35 %
        <td>6+3
        <td>12+6
        <td>24+12
        <td>48+24
    <tr>
        <td class="crystal" style="color: #dcdc4c">⬤
        <td>Standard
        <td>0.0 %
        <td>-45 %
        <td>5+3
        <td>10+6
        <td>20+12
        <td>40+24
    <tr>
        <td class="crystal" style="color: #ff0000">⬤
        <td>Infrared
        <td>20.0 %
        <td>-35 %
        <td>5+2
        <td>10+4
        <td>20+8
        <td>40+16
    <tr>
        <td class="crystal" style="color: #ff8800">⬤
        <td>Microwave
        <td>40.0 %
        <td>-25 %
        <td>4+2
        <td>8+4
        <td>16+8
        <td>32+16
    <tr>
        <td class="crystal" style="color: #880000">⬤
        <td>Radio
        <td>60.0 %
        <td>-15 %
        <td>5+0
        <td>10+0
        <td>20+0
        <td>40+0
</table>



<style>
    table.crystals {
        & > :is(thead,tbody) {
            & > tr {
                & > :nth-child(1) /* Color          */ { text-align: center;    }
                & > :nth-child(2) /* Name           */ { text-align: left;      }
                & > :nth-child(3) /* Range          */ { text-align: right;     }
                & > :nth-child(4) /* Capacitor      */ { text-align: right;     }
                & > :nth-child(5) /* Damage (S)     */ { text-align: center;    }
                & > :nth-child(6) /* Damage (M)     */ { text-align: center;    }
                & > :nth-child(7) /* Damage (L)     */ { text-align: center;    }
                & > :nth-child(8) /* Damage (XL)    */ { text-align: center;    }
            }
        }
    }

    table.crystals .crystal {
        text-shadow:
             1px  1px 1px black,
             1px -1px 1px black,
            -1px  1px 1px black,
            -1px -1px 1px black;
    }
</style>
