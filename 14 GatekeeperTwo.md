## GatekeeperTwo

1. This contract is similar to previous one. I glanced it and the first modifier gives the expression that the contract should be called from a contract.

2. I have seen the code like this with extcodesize and it is sometimes used to check if caller is contract or not but it is insecure way to do this, i think i heard it again by austin griffith... i watch all the videos mostly with 2x speed and if there is something that i don't understand something i reduce the speed... or rewatch. And worst case i watch 2 times then i don't or pick up things somewhere else. We can make our contract with 0 size or atleast make it appear that way but idk how so i will seach.

> i searched and `https://search.brave.com/search?q=extcodesize&source=desktop` and found `https://consensys.github.io/smart-contract-best-practices/development-recommendations/solidity-specific/extcodesize-checks/` i totally have read this in past but i didn't remember or understood it that time maybe ...

3. So, how to bypass `extcodesize` you ask? You will need to do all the operations in the constructor and so the contract does not have size since the contract account is created but information is in memory and not the storage... so `extcodesize` will be 0 during construction.

4. The last part is `XOR` operation. I have learned XOR, OR, NOR, NAND, AND and NOT. And I always remember OR, AND and NOT. I had to google XOR. and it results false or 0 if both the inputs are same and when inputs are different it results true or 1.

> oof, I cheated at some extent for modifier three, the channel that i watched did not have video for gatekeeperOne... but he has for gatekeeperTwo and `https://www.youtube.com/watch?v=PYzz33wEwYE` i skipped to the part where he explains `xor` it is called `bitwise` operator .. the word was forgotten from my memory.

5. The above video was not much helpful to explain ... because he did not explain the part how the key came and the solidity version of the challenge was 6.0 so they use underflow to reach type(uint).max
   `require(uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))) ^ uint64(_gateKey) == type(uint64).max));` this is the required case for modifier. I will go step by step

- `uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))`^`uint64(_gateKey)`= `type(uint64).max)` can be assumed as `a^b=c`
- so if we use normal mathematics and multiple a to both side `a^a^b=a^c`
- `b = a^c` because in `xor` gate/op `a^a = 0` the reason is all the keys in a are same and xor give false if both the keys are same `1^1=0` ... see point 4

6. So, learning from point 5 we do `b = a^c` so `key` will be `uint64(bytes8(keccak256(abi.encodePacked(this))))`^`type(uint64).max)`

7. Well, this was 10 times or even more easier than previous challenge. I only needed to write the line and i was doing inline enter(uint...) but i moved them in seperate line and passed key as variable and fixed type error and removed compiler warnings and in 5-10 mins challenge completed, cheers

```solidity
interface GatekeeperOne {
  function enter(bytes8 _gateKey) external;
}

contract Attack {
    constructor() {
        bytes8 key = bytes8(uint64(bytes8(keccak256(abi.encodePacked(address(this)))))^(type(uint64).max));
        GatekeeperOne(0x01091749dDfDEb64a6Bec8e310230eF1E81773c9).enter(key);
    }

    // (uint64(bytes8(keccak256(abi.encodePacked(msg.sender)))) ^ uint64(_gateKey) == type(uint64).max);
}
```

8. success
