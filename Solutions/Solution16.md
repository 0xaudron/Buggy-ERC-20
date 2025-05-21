**Issue**:
The issue lies in `approve` function. It doesn't update the `_allowances` mapping. So the spender would not be able to spend the allowance, due to this discrepancy.  

```solidity
function approve(address spender, uint256 value) public returns (bool) {
        emit Approval(msg.sender, spender, value);
        return true;
    }

```
**Recommended Fix**: 
Add this to the `approve` function :
```diff
+ _allowances[msg.sender][spender] = value;
```