**Issue**:
The issue lies in `_transfer` function. This code checks and deducts from the recipient's `to` balance, not the sender's `from` balance. That breaks the core logic of how ERC20 tokens are supposed to work. :

```solidity
function _transfer(address from, address to, uint256 value) internal {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");

        uint256 toBalance = _balances[to];
        require(toBalance >= value, "ERC20: transfer amount exceeds balance");

        _balances[from] = toBalance - value;
        _balances[to] += value;

        emit Transfer(from, to, value);
    }
```

**Recommended Fix**: 
Update the mapping for correct address : 
```diff
- uint256 toBalance = _balances[to];
- require(toBalance >= value, "ERC20: transfer amount exceeds balance");
+ uint256 fromBalance = _balances[from];
+ require(fromBalance >= value, "ERC20: transfer amount exceeds balance");

- _balances[from] = toBalance - value;
+ _balances[from] = fromBalance - value;
```