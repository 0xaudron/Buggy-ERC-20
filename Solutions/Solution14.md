**Issue**:
The issue lies in `transferFrom` function. This condition only decreases the allowance when it is `type(uint256).max`, which is the maximum possible value of a `uint256`. That logic is backwards — it should not decrease the allowance in that case, because many dApps and standards (e.g., Uniswap) use `type(uint256).max` as a signal of infinite approval. Instead, if the allowance is not type(uint256).max, then it should be decreased.

```solidity
function transferFrom(address from, address to, uint256 amount) public virtual returns (bool) {
        uint256 allowed = allowance[from][msg.sender]; 
        
        if (allowed == type(uint256).max) allowance[from][msg.sender] = allowed - amount;

        balanceOf[from] -= amount;

        unchecked {
            balanceOf[to] += amount;
        }

        emit Transfer(from, to, amount);

        return true;
    }
```

**Recommended Fix**: 
```diff
- if (allowed == type(uint256).max) allowance[from][msg.sender] = allowed - amount;
+ if (allowed != type(uint256).max) allowance[from][msg.sender] = allowed - amount;
```