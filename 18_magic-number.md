```assembly
PUSH 0x0a
PUSH 0x0c
PUSH 0x00
CODECOPY
PUSH 0x0a
PUSH 0x00
RETURN
PUSH1 0x42
PUSH2 0x80
MSTORE
PUSH 0x20
PUSH 0x80
RETURN
```
Converting the above EVM opcodes into bytecode using [Ethereum Virtual Machine Opcodes](http://ethervm.io).

```bytecode
600a600c600039600a6000f3604260805260206080f3
```

Finally, we can create a new contract by 
```javascript
await sendTransaction({from: player, data:"600a600c600039600a6000f3604260805260206080f3"})
```

Reference:
[Ethernaut Challenge: Level 18 Magic Number](https://medium.com/@sarankhotsathian/ethernaut-challenge-level-18-magic-number-solution-914c8c5d26d5)