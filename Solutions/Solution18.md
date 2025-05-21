**Issue**:
The issue lies in `_mint` function. It doesn't update `_totalSupply` which will break the ERC20 invariant :
> sum(balanceOf(all accounts)) = totalSupply

```solidity
function _mint(address account, uint256 value) internal {
        require(account != address(0), "ERC20: mint to the zero address");

        unchecked {
            _balances[account] += value;
        }
        emit Transfer(address(0), account, value);
    }
```

**Recommended Fix**: 

Add this to the `_mint` function :
```diff
+ _totalSupply += value;
```