# DiceCalc

## How to use

Create a new Calc class, and pass the dice expression to the constructor:

	$calc = new Calc($expression);

Output the itemized interpretation of the roll:

	echo $calc->infix();

Ouptut a detailed interpretation of the roll::

	echo $calc->detailed():

Output the result of the roll::

	echo $calc();

## Examples

| cmd          | Description                                                                                                                                              | Detailed                    | Infix            | Result |
| 4d6o=6       | roll 4d6, if any come up six, reroll and add that result                                                                                                 | (4d6o=6: [4 + 4 + 14 + 5])  | [4 + 4 + 14 + 5] | 27     |
| 2d10o=5      | roll 2d10, if any come up five, reroll and add that result                                                                                               | (2d10o=5: [1 + 12])         | [1 + 12]         | 13     |
| 3d6r<4o=6k>6 | crazy, roll 3d6, reroll any die less than four. If any of the dice come up six, reroll and add that result. Total only the dice that are greater than 6. | (3d6r<4o=6k>6: [5 + 4 + 9]) | [5 + 4 + 9]      | 9      |

