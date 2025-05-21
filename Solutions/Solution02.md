**Issue**:
The issue lies in the `approve` function : 
```solidity
   function approve(address owner, address spender, uint256 amount) public {
        allowance[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
```

So basically the issue is related to missing access control, you must have guessed. Anyone can approve allowances on behalf of any owner. Since there is no check if owner is `msg.sender` who is approving the transaction 

Recommended Fix  :
Add the following validation : 
```solidity
require(owner = msg.sender, "Unauthorized access");
```