**Issue**:
The issue lies in `mint` function. It has public accessibility.
```solidity
function mint(address to, uint256 value) public {
        _mint(to, value);
    }
```
Anyone can call this function to mint unlimited tokens to any address.

**Recommended Fix**: 
Add `onlyOwner` modifier so that only owner could mint tokens to addresses 
```diff
- function mint(address to, uint256 value) public 
+ function mint(address to, uint256 value) public onlyOwner
```