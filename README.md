# DiceCalc

## How to use

Create a new Calc class, and pass the dice expression to the constructor:

```php
$calc = new Calc($expression);
```

Output the itemized interpretation of the roll:

```php
echo $calc->infix();
```

Ouptut a detailed interpretation of the roll::

```php
echo $calc->detailed():
```

Output the result of the roll::

```php
echo $calc();
```

## Basic Expressions

DiceCalc supports three basic formats for defining a given die:

* roll a die with *n* faces
* roll a percentile die
* roll a [Fudge](http://en.wikipedia.org/wiki/Fudge_(role-playing_game_system)) die

|   cmd   |   Detailed  | Infix | Result |
| ------- | ----------- | ----- | ------ |
| **d6**  | (d6: [4])   | [4]   |      4 |
| **d20** | (d20: [12]) | [12]  |     12 |
| **d%**  | (d%: [86])  | [86]  |     86 |
| **df**  | (df: [1])   | [1]   |      1 |

## Multiples

Prefixing any die with a number results in rolling that many of that type of die.

|   cmd    |         Detailed        |      Infix       | Result |
| -------- | ----------------------- | ---------------- | ------ |
| **3d6**  | (3d6: [6 + 3 + 5])      | [6 + 3 + 5]      |     14 |
| **2d20** | (2d20: [20 + 1])        | [20 + 1]         |     21 |
| **4df**  | (4df: [0 + -1 + 1 + 0]) | [0 + -1 + 1 + 0] |      0 |

## Modifiers and Mathematical Expressions

You can modify the roll results with just about any basic mathematic expression to add and subtract numbers and dice, or to multiply each result from a set by a given number.

|       cmd        |           Detailed           |      Infix      |  Result  |
| ---------------- | ---------------------------- | --------------- | -------- |
| **2d6+4**        | (2d6: [1 + 2]) + 4           | [1 + 2] + 4     | 7        |
| **3d8-5**        | (3d8: [1 + 5 + 6]) - 5       | [1 + 5 + 6] - 5 | 7        |
| **2d10+d6**      | (2d10: [8 + 10]) + (d6: [3]) | [8 + 10] + [3]  | 21        |
| **2*[2d10,d20]** | 2 * [8, 12]                  | 2 * [8, 12]     | [16, 24] |

## Targets & Conditions

For success/fail rolls, you can include the target number, and get back a boolean result. You can test for greater than or less than.

|      cmd       |        Detailed       |     Infix     | Result |
| -------------- | --------------------- | ------------- | ------ |
| **2d6 > 5**    | (2d6: [2 + 6]) > 5    | [2 + 6] > 5   | TRUE   |
| **d20+4 > 12** | (d20: [8]) + 4 > 12   | [8] + 4 > 12  | FALSE  |
| **2d10 < 12**  | (2d10: [10 + 5]) < 12 | [10 + 5] < 12 | FALSE  |

## Sets of Numbers

For sets of result rows, use *n*[*dice*] notation.

|    cmd     |         Detailed        |          Infix          |          Result         |
| ---------- | ----------------------- | ----------------------- | ----------------------- |
| **6[3d6]** | [18, 13, 17, 14, 11, 9] | [18, 13, 17, 14, 11, 9] | [18, 13, 17, 14, 11, 9] |
| **4[d%]**  | [57, 76, 41, 14]        | [57, 76, 41, 14]        | [57, 76, 41, 14]        |

## Custom Dice Faces

It is also possible to roll nonstandard dice with custom defined faces.

|             cmd              |               Detailed               |  Infix   |                  Result                  |
| ---------------------------- | ------------------------------------ | -------- | ---------------------------------------- |
| **d[1,2,2,3,3,4]**           | (d[1,2,2,3,3,4]: [4])                | [4]      | 4                                        |
| **d[red,green,blue,yellow]** | (d[red,green,blue,yellow]: [yellow]) | [yellow] | (d[red,green,blue,yellow]: [yellow])[^1] |

## Conditional Keepers

You can choose to keep only results that are higher or lower than a specified value.

|        cmd        |                     Detailed                     |                Infix                | Result |
| ----------------- | ------------------------------------------------ | ----------------------------------- | ------ |
| **5d6 keep > 4**  | (5d6keep>4: [~~2~~ + ~~2~~ + ~~2~~ + ~~3~~ + 5]) | [~~2~~ + ~~2~~ + ~~2~~ + ~~3~~ + 5] |      5 |
| **5d6k>4**        | (5d6k>4: [6 + ~~3~~ + ~~4~~ + 5 + ~~2~~])        | [6 + ~~3~~ + ~~4~~ + 5 + ~~2~~]     |     11 |
| **6d8k<4**        | (6d8k<4: [~~4~~ + ~~8~~ + 1 + 1 + 3 + 2])        | [~~4~~ + ~~8~~ + 1 + 1 + 3 + 2]     |      7 |
| **4d6 highest 3** | (4d6highest3: [~~1~~ + 3 + 6 + 2])               | [~~1~~ + 3 + 6 + 2]                 |     11 |
| **4d6h3**         | (4d6h3: [2 + 4 + ~~2~~ + 3])                     | [2 + 4 + ~~2~~ + 3]                 |      9 |
| **2d20 lowest 1** | (2d20lowest1: [~~17~~ + 6])                      | [~~17~~ + 6]                        |      6 |
| **2d20l1**        | (2d20l1: [13 + ~~14~~])                          | [13 + ~~14~~]                       |     13 |

## Conditional Re-rollers

You can specify that die be rerolled if they are equal to, less than, or greater than a specified target. This ensures that all dice have at least a minimum value.

|      cmd       |          Detailed          |      Infix       |      Result      |
| -------------- | -------------------------- | ---------------- | ---------------- |
| **3d6 reroll < 3** | (3d6reroll<3: [6 + 4 + 6]) | [6 + 4 + 6]      | 16               |
| **3d6r<3**         | (3d6r<3: [4 + 5 + 4])      | [4 + 5 + 4]      | 13               |
| **4[d%r<40]**      | [45, 84, 46, 66]           | [45, 84, 46, 66] | [45, 84, 46, 66] |

## Example Stat Rollers

Here are some common examples of how some of these features can come together for some popular complex rolls. Below are some methods of rolling six stats from 3–18.

* straight roll
* reroll 1s
* roll 4, drop the lowest
* roll 5, drop the two lowest
* roll 2 add six

|      cmd      |         Detailed         |          Infix           |          Result          |
| ------------- | ------------------------ | ------------------------ | ------------------------ |
| **6[3d6]**    | [8, 12, 14, 11, 7, 18]   | [8, 12, 14, 11, 7, 18]   | [8, 12, 14, 11, 7, 18]   |
| **6[3d6r<2]** | [16, 11, 10, 13, 10, 13] | [16, 11, 10, 13, 10, 13] | [16, 11, 10, 13, 10, 13] |
| **6[4d6h3]**  | [15, 14, 11, 7, 8, 13]   | [15, 14, 11, 7, 8, 13]   | [15, 14, 11, 7, 8, 13]   |
| **6[5d6h3]**  | [14, 16, 17, 15, 13, 9]  | [14, 16, 17, 15, 13, 9]  | [14, 16, 17, 15, 13, 9]  |
| **6[2d6+6]**  | [15, 15, 13, 14, 10, 13] | [15, 15, 13, 14, 10, 13] | [15, 15, 13, 14, 10, 13] |

## "Open" Dice

"Open" dice are dice that are rerolled if they meet a given criteria. However unlike regular rerollers, the original value is kept, and the rerolls are *added* to the result. The last one is crazy. You shouldn't do it. Ever.

|       cmd        |           Detailed          |      Infix       | Result |
| ---------------- | --------------------------- | ---------------- | ------ |
| **4d6o=6**       | (4d6o=6: [4 + 4 + 14 + 5])  | [4 + 4 + 14 + 5] |     27 |
| **2d10o=5**      | (2d10o=5: [1 + 12])         | [1 + 12]         |     13 |
| **3d6r<4o=6k>6** | (3d6r<4o=6k>6: [5 + 4 + 9]) | [5 + 4 + 9]      |      9 |

The last example rolls 3d6, rerolls any die less than four. If any die is a 6, it is rerolled and added to the total. Finally, only keep results that are greater than six. See? Don't do it.
