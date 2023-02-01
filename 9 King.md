## King

1. Remember we learnt that only self destruct (in challenge 7 Force) can force send ether to a contract that does not have payable or receive option... we can use that to DOS (i.e. Denial of Service) the King contract.

2. a) If I create an Attack contract and become the king (the contract becomes the king) and it has no way to receive -> nor can King contract selfdestruct.. haha
   > `payable(king).transfer(msg.value);` in line 2 of receive() of Attack... it will try to send us but fail

or

2. b) we can even add receive or fallback function which has a require and is always false or even a revert which revert every call.

3. Let's do 2. a)

   > 2 a.) resurrected.) Just when I went to code I realized that if i want to become the king using the contract then contract has to have enough ether to be king and second method `i.e. 2. b)` will be more easy but i will use a selfdestruct to transfer in Attack contract and attack() will call the King contract and make it unusable

   I get this by running `await contract.prize()` so idk what eth is in prize... i will try sending 1 eth and i think the challenge creators must be knowing that getting goerli eth is difficult and req has be less (it is just that i think it should be low and idk if it will be)

   ````words:Array(3)
   0:13008896
   1:14901161```

   > Code
   ````

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ForceSend {
    receive() payable external {}

    function funcAttack(address _newO) public {
        selfdestruct(payable(_newO));
    }
}

contract Attack {
    // address public instance = "";
    // king of the hills
    function attack() public payable {
        require(msg.value >= 1 ether, "please send exactly 1 ether");
        (bool success, ) = payable(address(0x521cee4Ee115c71a1ab1cdFC8821e4232E0F06fa)).call{value: msg.value}("");
        require(success, "failed tx");
    }
}
```

### Note: You can skip steps 4, 5, and 7

> as that has flawed logic ... i tried doing 2. a) bc i was confident on 2. b) ....in step 9 i have explained what i did wrong

4. I will first deploy ForceSend.

5. And then send 1.1 eth first to ForceSend contract

6. And then deploy Attack.

7. And then selfdestruct ForceSend by passeing Attack contract address

8. Then call attack() of Attack...

> Not Important: Hopefully I succeed... i am doing writeup first and then practical bc i have to think and then do and then think again to do but this is easier ... if I think first and typeup then I already have the roadmap for practical and I can follow that to pass, reducting some time wastage

9. Actually there is no need of steps 4, 5, and 7 because when you call attack() it is payable and i used msg.value instead of address(this).balance so the efforts of step 4, 5 and 7 were lost but i learnt some sick stuff.

10. the Attack contract has 1.1 eth to be the king but is locked bc i used msg.value and what was meant to do didn't happen

11. success
