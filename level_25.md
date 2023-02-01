# Level 25: [Motorbik](https://ethernaut.openzeppelin.com/level/0x9b261b23cE149422DE75907C6ac0C30cEc4e652A)

## Level objectives:
`selfdestruct` the implementation contract.
## Analysis
- The current logic contract calls `delegatecall` once–in it's `_upgradeToAndCall` (called by `upgradeToAndCall`) function.
- It is used to change the implementation contract address, hence "upgrading" the contract.
- So we will call `upgradeToAndCall`, point it to our contract and call `selfdestruct` via `delegatecall` => the delegate caller, i.e. the Engine contract, will get `selfdestruct`-ed.
- We just need permissions to call `upgradeToAndCall`. It can be called only by the `upgrader` set in the init function.
- The upgrader is the proxy contract, so we will call the upgrade method via the original proxy contract.
## Steps: