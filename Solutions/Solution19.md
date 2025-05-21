**Issue**:
The issue lies in `_transfer` function. So there are two hooks used for extensibility `_beforeTokenTransfer`, `_afterTokenTransfer`. 

So the intended flow of the function is :
- Read `fromBalance` and `toBalance` into local variables.
- Call `_beforeTokenTransfer` hook
- Then check if the sender has enough balance.
- Then subtract amount from the sender’s balance and add amount to the recipient’s balance.

Problem associated with it, unintended flow :
- If `from` and `to` are same, the `fromBalance` and `toBalance` hold the same value
- The subtraction and addition happen independently on those cached values

Attack Scenario (with numbers): 
- `fromBalance` = 100, `toBalance ` = 100 and `amount` = 10
- _balances[from] = 100 - 10; //90 
  _balances[to] = 100 + 10 //110

So the balance of the sender is 110, that 10 extra balance is created out of thin air.

```solidity
    function _transfer(
          address from,
          address to,
          uint256 amount
    ) internal virtual {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
    
        uint256 fromBalance = _balances[from];
        uint256 toBalance = _balances[to];
    
        _beforeTokenTransfer(from, to, amount);
    
        require(
            fromBalance >= amount,
            "ERC20: transfer amount exceeds balance"
        );
        unchecked {
            _balances[from] = fromBalance - amount;
            // Overflow not possible: the sum of all balances is capped by totalSupply, and the sum is preserved by
            // decrementing then incrementing.
            _balances[to] = toBalance + amount;
        }
    
        emit Transfer(from, to, amount);
    
        _afterTokenTransfer(from, to, amount);
    }
```

**Recommended Fix**: 
```
```