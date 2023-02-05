## Naught Coin

1. Here, all you need to do is to get your balance of tokens from this contract to be 0.

2. you use approve() of ERC20, to do this => you paste the code/interface in remix and compile and the use `At Address` feature to load the contract. And you can see all the read and write options on the contract.

3. then you put an address to which you have access to and can use it to call the transferFrom on remix, that address and your balance will passed and by calling approve() you allow the address that you input to transfer you the funds aka tokens (which you input as second argument).

4. Once the transaction goes through you can check the allowance by calling allowance and input owner (i.e. the person who has the funds ) and spender (i.e. the person who can spend the owner's funds ). But this is not necessary.

5. Now you switch to the account which has approval/allowance to spend the funds/token of owner and simply call the transferFrom() with inputs of owner address, to ( i send the tokens to the contract itself ), amount. ( the amount of tokens that you want to transfer.) Also you cannot send more than the allowance, if you try to transfer more than allowance the transaction fails.

6. You could have done this in browser console itself but i recalled it after i put the code and called approve in remix. So, I used remix itself.

7. success
