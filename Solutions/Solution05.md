**Issue**:
The issues lies in `transferFrom()` function :
```solidity
function transferFrom(address from, address to, uint256 value) public returns (bool) {
        _spendAllowance(from, msg.sender, value);
        _transfer(to, from, value); //@audit wrong order - _transfer(from,to,value)
        return true;
    } 
```
The order of the arguments in the `_transfer` is misconfigured which will transfer the amount from `to` address to `from` address which isn't the intended use case.

**Recommended Fix**:  
```diff
- _transfer(to, from, value);
+ _transfer(from, to, value);
```