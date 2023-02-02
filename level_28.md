# Level 24: [Gatekeeper 3](https://ethernaut.openzeppelin.com/level/0x762db91C67F7394606C8A636B5A55dbA411347c6)

## Level objectives:
Pass the gates and become the `entrant`.
## Analysis
- Gate one: 
```sol
require(msg.sender == owner);
require(tx.origin != owner);
```
- `owner` is set in a `construct0r`; not the actual constructor. So we can set this. Needs to be called from a contract for `tx.origin != owner`.

- Gate two:
```sol
require(allow_enterance == true);
```
- `allow_enterance` set in `getAllowance` function; need to read value of private variable from `SimpleTrick`'s slot 2 for this. Need to init `SimpleTrick` and then read it. Send the read `password` to `getAllowance` in Gatekeeper.

- Gate three:
```sol
if (address(this).balance > 0.001 ether && payable(owner).send(0.001 ether) == false);
```
- Donate eth for 1st; revert them sending back as `payable(address).send` returns false on failure. See: [solidity docs](https://docs.soliditylang.org/en/v0.8.18/types.html#members-of-addresses)
## Code:


