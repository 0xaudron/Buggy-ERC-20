**Issue**:
The issue lies in `burn()` function and results in discrepancy being caused with `totalSupply` as its not updated when anyone calls this function.

```solidity
function burn(uint256 value) public {
        _balances[msg.sender] -= value;
        emit Transfer(msg.sender, address(0), value);
    }
```

**Recommended Fix**: 
```diff
function burn(uint256 value) public {
    +   _totalSupply -= value;
        _balances[msg.sender] -= value;
        emit Transfer(msg.sender, address(0), value);
    }
```


___

**Issue #2**:
The issue lies in `_approve()` function. The function doesn't emit the `Approval` which could create discrepancy with the off-chain components who are listening for events.


**Recommended Fix**: 
Add this to the `_approve()` in the end :
```diff
+ emit Approval(address indexed owner, address indexed spender, uint256 value);`
```