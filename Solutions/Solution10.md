**Issue**:
The issue lies in `onlyOwner` modifier. The check is done as expression not as enforcement, which would not affect the functions with `onlyOwner` modifier .

```solidity
modifier onlyOwner() {
        msg.sender == owner;
        _;
    }
```

**Recommended Fix**: 
Here's the fixed version enforcing with `require` : 
```solidity
modifier onlyOwner() {
        require(msg.sender == owner, "Unauthorized, not owner");
        _;
    }
```