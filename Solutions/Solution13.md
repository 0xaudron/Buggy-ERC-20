**Issue**:
The issue lies in `approve()` function. The parameters are flipped. Hence the spender will give approve allowance to the address calling this function which isn't intended.

```solidity
function approve(address spender, uint256 amount) public virtual returns (bool) {
    allowance[spender][msg.sender] = amount;

    emit Approval(msg.sender, spender, amount);

    return true;
}
```

**Recommended Fix**: 

```diff
- allowance[spender][msg.sender] = amount;
+ allowance[msg.sender][spender] = amount;

```