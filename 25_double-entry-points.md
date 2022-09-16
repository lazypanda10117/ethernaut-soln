```javascript
pragma solidity ^0.6.0;

interface IDetectionBot {
    function handleTransaction(address user, bytes calldata msgData) external;
}

interface IForta {
    function setDetectionBot(address detectionBotAddress) external;
    function notify(address user, bytes calldata msgData) external;
    function raiseAlert(address user) external;
}

contract Forta is IForta {
  mapping(address => IDetectionBot) public usersDetectionBots;
  mapping(address => uint256) public botRaisedAlerts;

  function setDetectionBot(address detectionBotAddress) external override {
      require(address(usersDetectionBots[msg.sender]) == address(0), "DetectionBot already set");
      usersDetectionBots[msg.sender] = IDetectionBot(detectionBotAddress);
  }

  function notify(address user, bytes calldata msgData) external override {
    if(address(usersDetectionBots[user]) == address(0)) return;
    try usersDetectionBots[user].handleTransaction(user, msgData) {
        return;
    } catch {}
  }

  function raiseAlert(address user) external override {
      if(address(usersDetectionBots[user]) != msg.sender) return;
      botRaisedAlerts[msg.sender] += 1;
  } 
}

contract DoubleEntryDetectionBot is IDetectionBot {
    address crypto_vault_addr;

    constructor (address addr) {
        crypto_vault_addr = addr;
    }
    
    function handleTransaction(address user, bytes calldata msgData) external override {
        // 68 = 4 (func sig) + 32 (addr) + 32 (uint)
        bytes32 byte_addr = bytes32(msgData[68: 32]);
        address original_sender = address(uint160(uint256(byte_addr)));
        // Version 2:
        // address original_sender;
        // assembly {
        //     original_sender := calldataload(0xa8) // this is location 168
        // }
        if (original_sender == crypto_vault_addr) {
            // raise alert
            Forta(msg.sender).raiseAlert(user);
        }
    }
}
```
```javascript
let forta_address = await contract.forta();

let crypto_vault_address = await contract.cryptoVault();
```

Deploy the contract `DoubleEntryDetectionBot(crypto_vault_address)`. Let its address be `custom_bot_address`. Then, we call the following to set the forta alert bot.


```javascript
sendTransaction({
	from: player, 
	to: forta_address, 
	data: web3.eth.abi.encodeFunctionCall({
	    name: 'setDetectionBot',
	    type: 'function',
	    inputs: [{
	        type: 'address',
	        name: 'detectionBotAddress'
	    }]
	}, [custom_bot_address])
});
```
