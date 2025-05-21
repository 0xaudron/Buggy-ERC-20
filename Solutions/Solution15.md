**Issue**:
The issue lies in `_mint` function. The function increases `totalSupply`, but it does not credit the to account:
```solidity
function _mint(address to, uint256 amount) internal virtual {
            totalSupply += amount;

            emit Transfer(address(0), to, amount);
        }
```

**Recommended Fix**: 
Increase the `balanceOf` mapping for `to` address :
 
```solidity
function _mint(address to, uint256 amount) internal virtual {
            totalSupply += amount;
            balanceOf[to] += amount;
            emit Transfer(address(0), to, amount);
        }
```