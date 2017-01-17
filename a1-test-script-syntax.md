## CREATE

Creates a new vending machine (subject to the conditions above) with the specified coin-kinds accepted, and the number of buttons (i.e. different kinds of pops to vend).

Syntax:

`CREATE ( [coin-kind] {, [coin-kind]} ; [number-of-buttons] )`

Usage:

`CREATE (5, 10, 25, 100, 200; 5)` // accepts 5, 10, 25, 100, 200 cent coins, and has five buttons
`CREATE (1, 5, 10; 2)` // accepts 1, 5, 10 cent coins, and has two buttons
`CREATE (1, 1; 2)` // fails because coins types are not unique

## CONFIGURE

Configures a vending machine (subject to the conditions above) specified by an index with pop-name and price pairs for as many pops as has been specified by a previous `CREATE` command. If you attempt to configure more pops than the vending machine has buttons, this is an error.

Pop names should only have letters and no special characters or spaces.

Syntax:

`CONFIGURE ([ [vending-machine-index] ] [pop-name], [price] { ; [pop-name], [price] })`

Usage:

`CONFIGURE ([0] "Coke", 100; "Pepsi", 100; "7-Up", 150)` // configures the first vending machine (0th) with three pops with their respective prices
`CONFIGURE ([0] "Milk", 10)` // configures the first vending machine to sell milk for $0.10
`CONFIGURE ([1] "Coke", 0)` // fails because the price needs to be greater than 0; could also fail if a second vending machine has not yet been created using the `CREATE` command twice

## COIN_LOAD

Loads the specified number of the specified kind of coins into the specified vending machine.

Syntax:

`COIN_LOAD ([ [vending-machine-index] ] [coin-chute] ; [coin-kind] , [number-of-coins] )`

Usage:

`COIN_LOAD ([0] 0; 25, 1)` // loads one $0.25 coin into the first slot of the first vending machine
`COIN_LOAD ([0] 1; 5, 10)` // loads 10 $0.05 coins into the second slot of the first vending machine

## POP_LOAD

Loads the specified number of the specified kind of pops into the specified vending machine.

Syntax: 

`POP_LOAD ([ [vending-machine-index] ] [pop-chute] ; [pop-name] , [number-of-pops] )`

Usage:

`POP_LOAD ([0] 0; "Coke", 1)` // loads one can of coke into the first slot of the first vending machine
`POP_LOAD ([0] 1; "Milk", 10)` // loads 10 milk cans into the second slot of the first vending machine

## INSERT

Inserts a coin of the specified value into the specified vending machine.

Syntax:

`INSERT ([ [vending-machine-index] ] [coin-value])`

Usage:

`INSERT ([0] 25)` // inserts a 25 cent coin into the first vending machine
`INSERT ([1] 2)` // inserts a 2 cent coin in the second vending machine
`INSERT ([0] -2)` // fails because it is a negative coin value

## PRESS

Presses a button on a vending machine (to vend a pop). Depending on the conditions above (enough money, that there's a pop there, etc.), this command might vend a pop and/or dispense change. It just depends. Fails if the button doesn't exist on the vending machine. Buttons are 0-indexed.

Syntax:

`PRESS ([ [vending-machine-index] ] [button])`

Usage:

`PRESS ([0] 1)` // presses the second button on the first vending machine

## EXTRACT & CHECK_DELIVERY

`EXTRACT` is related to its cousin command `CHECK-DELIVERY`. Extract takes out all of the items that are in the delivery chute (dispensed pops, coins that are change, etc.). Check-delivery checks to see whether what has been extracted matches the specified expectation.

Syntax:

`EXTRACT ([ [vending-machine-index] ])`
`CHECK_DELIVERY ( [total-coin-value] { , [pop-name] })`

Usage:

`EXTRACT ([0])` // extracts coins and pops from delivery chute of the first vending machine
`CHECK_DELIVERY (10, "Coke")` // checks to see if we had 10 cents returned (total) and a coke in the chute
`CHECK_DELIVERY (0, "Coke", "Sprite")` // checks to see if we had 0 cents returned, and a coke and a sprite in the chute (not necessarily in that order). Notice that this would only pass if we had vended two pops without first extracting the first one. Prints PASS or FAIL in the console.

## UNLOAD and CHECK_TEARDOWN

`UNLOAD` is paired with its cousin command `CHECK-TEARDOWN`. Unload returns all of the unsold pop, all the money that has been collected, and all the reamining change. Check-Teardown checks to see if what has been unloaded matches with what is expected.

Syntax:

`UNLOAD ([ [vending-machine-index] ])`
`CHECK_TEARDOWN ( [value-of-change-remaining] ; [value-of-money-collected] { ; [pop-name] { , [pop-name]}})`

Usage:

`UNLOAD ([1])` // unloads everything from the second vending machine
`CHECK_TEARDOWN (15; 200; "Coke", "Sprite", "Milk")` // passes if we had 15 cents remaining in change, $2 in money collected, and the three drinks remaining unsold
`CHECK_TEARDOWN (10; 500)` // passes if we sold all of our pops, and made $5, and had 10 cents left in change