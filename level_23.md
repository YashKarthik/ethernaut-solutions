# Level 23: [Dex 2](https://ethernaut.openzeppelin.com/level/0x0b6F6CE4BCfB70525A31454292017F640C10c768)

## Level objectives:
Drain both tokens from the Dex contract.
## Analysis
- It's the same contract as before, but this time it allows any token-pair.
- So we can mint our own erc20 token then swap that for the tokens from the dex.
- To determine how much tokens to send to the contract for the swap use the formula. Also send some extra tokens at the start.
## Code:
```js
contract.swap(myToken, token1, 1)
contract.swap(myToken, token2, 1)
```