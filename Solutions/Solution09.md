**Issue**:
The issue lies in `transfer` function due to usage of `unchecked`. This bypass the default check of underflow or overflow. So if attacker have specified `amount` more than their `_balances[msg.sender]`. The check would pass and on the other hand attacker will have a lot of balance.  

```solidity
function transfer(address to, uint256 amount) public returns (bool) {
        unchecked {
            _balances[msg.sender] -= amount;
        }
        _balances[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }
```

So if attacker has 1 token and he decides to transfer 2 token. The `_balances[msg.sender]` woould be equal to `type(uint256.max)` = 115792089237316195423570985008687907853269984665640564039457584007913129639935

**Recommended Fix**: 
Remove the unchecked wrapper : 
```solidity
function transfer(address to, uint256 amount) public returns (bool) {
        _balances[msg.sender] -= amount;
        _balances[to] += amount;
        emit Transfer(msg.sender, to, amount);
        return true;
    }
```