# Level 8: [Vault](https://ethernaut.openzeppelin.com/level/0x3A78EE8462BD2e31133de2B8f1f9CBD973D6eDd6)

## Level objectives:
1. Unlock the vault by changing the value of `bool public locked;`

## Analysis (lol)
- To unlock, we need to call the `unlock(bytes32 _password)` with the password.
- The password is set in the constructor, and is a private variable.
- But nothing's _really_ private on the blochain.

## Steps:
- Get the password stored in the storage using:
```js
await web3.eth.getStorageAt(address, 1);
```
- Pass that to the `unlock` function. `contract.unlock(password);`
- Verify using `await contract.locked()`
