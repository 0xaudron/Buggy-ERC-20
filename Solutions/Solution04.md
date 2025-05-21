**Issue #1**:
The issue is in `transferFrom` function : 

```solidity
    function transferFrom(address from, address to, uint256 value) public returns (bool) {
        _spendAllowance(from, msg.sender, value);
        _transfer(from, to, value);
        return true;
    }
```

Basically there is no validation of transfers paused. 

**Recommended Fix**: 
```solidity
require(!paused, "Challenge4: transfers paused");
```

___

**Issue #2**:
Another issue is that in `approve` function : 

```solidity
function approve(address spender, uint256 value) public returns (bool) {
    _approve(msg.sender, spender, value);
    return true;
}

function _approve(address tokenOwner, address spender, uint256 value) internal {
    require(tokenOwner != address(0), "Challenge4: approve from zero address");
    require(spender != address(0), "Challenge4: approve to zero address");

    _allowances[tokenOwner][spender] = value;
    emit Approval(tokenOwner, spender, value);
}
```
The issue is that of direct overwriting of existing allowances. This creates a well-known ERC20 vulnerability called the "approve race condition". An attacker can exploit this by front-running or manipulating token approvals

Potential Attack Scenario:
1. User A approves Spender with 100 tokens
2. Later, user A wants to change allowance to 50 tokens
3. Malicious actor can potentially drain tokens during the approval transition

**Recommended Fix**:  
Implement increaseAllowance() and decreaseAllowance() methods with checks