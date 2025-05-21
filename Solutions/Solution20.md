**Issue**:
The issue lies in `transferFrom` function, as the function increases allowance instead of decreasing it. SO if Alice `approve` Bob with 20 tokens. And Bob `transferFrom` from Alice's account. The allowance should decrease however it increases hence it can grow unboundly with each call instead of depleting.

```solidity
function transferFrom(address from, address to, uint256 amount) public virtual returns (bool) {
        uint256 allowed = allowance[from][msg.sender];

        if (allowed != type(uint256).max) allowance[from][msg.sender] = allowed + amount;

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
- allowance[from][msg.sender] = allowed + amount;
+ allowance[from][msg.sender] = allowed - amount;

```