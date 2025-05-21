**Issue**:
The issue lies in `_transfer` function :
```solidity
 function _transfer(address from, address to, uint256 value) internal {
        if (from == address(0)) revert InvalidSender(from);
        if (to == address(0)) revert InvalidReceiver(to);

        uint256 fromBalance = _balances[from];
        if (fromBalance < value) revert InsufficientBalance(from, fromBalance, value);

        _balances[to] += value;
        emit Transfer(from, to, value);
    }
```

It doesn't subtract from the sender's balance, which is a critical oversight. So basically the senders balance is never deducted hence it remains the same and could be used to infinite times. 

**Recommended Fix**: 
Add this effect to the `_transfer` function :
```diff
_balances[to] += value;
+_balances[from] -= value;
```