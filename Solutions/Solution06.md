**Issue #1**:
The issue lies in `approve` function. So there is a blacklist functionality to blacklist addresses with suspicious activities (similar to that of USDC). However there is no check if blacklisted addresses can approve their amount to any spender (could be their account) hence bypassing the checks

```solidity
function approve(address spender, uint256 value) public returns (bool) {
        _approve(msg.sender, spender, value);
        return true;
    }
```

**Recommended Fix**: 
```diff
function approve(address spender, uint256 value) public returns (bool) {
+ require(!blacklist[msg.sender] && !blacklist[sender], "Sender or receiver blacklisted");
        _approve(msg.sender, spender, value);
        return true;
    }
```
___

**Issue #2**:
Another issues lies in `transferFrom()`. Can you spot it by looking at the snippet ? 
```solidity
function transferFrom(address from, address to, uint256 value) public returns (bool) {
        require(!blacklist[msg.sender] && !blacklist[to], "Sender or receiver blacklisted");
        _spendAllowance(from, msg.sender, value);
        _transfer(from, to, value);
        return true;
    }
```

PRACTICE HARD if you didn't. So the blacklist mapping only if `msg.sender` and `to` is in the list. However an non-blacklisted address could spend the allowance of blacklisted address `from` specified in the arguments. 

**Recommended Fix**:  
```diff 
function transferFrom(address from, address to, uint256 value) public returns (bool) {
        require(!blacklist[msg.sender] && !blacklist[to] && !blacklist[from], "Sender or receiver blacklisted");
        _spendAllowance(from, msg.sender, value);
        _transfer(from, to, value);
        return true;
    }
```

If you're thinking Issue #1, and Issue #2 are in the same coin. You are correct, both are required for the reaching the exploitation part. However if there was check in `transferFrom`, the exploitation could not have taken place. 

I agree a fix in just `approve()` could mitigate both as there will be no room of exploitation with `transferFrom()`.