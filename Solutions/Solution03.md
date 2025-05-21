**Issue**:
The issue lies in the `burn` function : 

```solidity
    function burn(address account, uint256 value) public {
        require(account != address(0), "Invalid burner");
        uint256 accountBalance = _balances[account];
        require(accountBalance >= value, "Insufficient balance");

        _balances[account] = accountBalance - value;
        _totalSupply -= value;
        emit Transfer(account, address(0), value);
    }
```

Yes you, guessed it correct again, it's msising access control in `burn` function.  Anyone can burn tokens from any account since there is no validation on the account provided in parameter is `msg.sender`.

**Recommended Fix**:  
```solidity
require(account == msg.sender, "Wrong caller");
```

