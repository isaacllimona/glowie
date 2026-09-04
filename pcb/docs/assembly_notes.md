# Glowie PCB assembly notes

## mainboard

### Charger

R13 is used to program charging current, using Ichrg = 1000/R13.

Example values (From datasheet):

| R13 (kohm) | Ichrg (mA) |
| :--- | :--- |
| 10.00 | 100 |
| 5.10 | 195 |
| 3.30 | 303 |
| 2.49 | 400 |
| 2.00 | 499 |
| 1.50 | 676 |
| 1.20 | 843 |
| 1.10 | 928 |
| 1.00 | 1030 |

### Boost converter

The output voltage of the boost converter is adjusted with R19 and R20.
- R19 = R20 * (vout/0.9 - 1)
- R19 + R20 should be around 1M

Example values:
| Vout | R19  | R20  |
| :--- | :--- | :--- |
| 4v   | 820k | 240k |
| 4.5v | 800k | 200k |
| 5v   | 910k | 200k |
