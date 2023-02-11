## Dex Two

1. In this contract we need to drain the token1 as well as token2 and there is a change in swap() that is a requirement that token1 or token2 are the only tokens that can be swaped is not the case anymore, so to solve this we will create a token and add that to liquidity and then swap that for token1 and token2, easy right?

2. The initial steps will be same as previous challenge so you can go back to Dex solution and do the step 2 and step 3 which approves the tokens.

3. Now we can create the token3 for that i will deploy the below contract and deploy it

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";

contract MalicousToken is ERC20 {
  constructor() ERC20("MALIcious", "MAL") {
        _mint(msg.sender, 1000000*(10**18));
  }
}
```

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";

contract MalicousToken is ERC20 {
  constructor() ERC20("MALIcious", "MAL") {
        _mint(msg.sender, 1000000*(10**18));
  }
}
```

4. After the contract is deployed i will approve the Dex challenge contract the approval to move funds, by calling approve and passing amount like anything (you would want to put high amount or else you might need to approve again in one of the steps so i will approve like 10000 tokens)

5. Now we need to add the token3 liquidity to the Dex so it can swap. But before that i will add `t3 = 0x80f52658b46d6622EB70541f98c7d111529A7343` the address of token3 that we created in the variable t3 so that it is ez during swaps, we already have t1 and t2. We already approved all of these tokens to send via Dex. The remaining part is adding t3 in liquidity and draining t1 and t2.

6. Adding liquidity `await contract.add_liquidity(t3, 100)` we added 100 wei as liquidity to the contract.

> Note: You can't add liquidity bcz you are not the owner, i was stuck for some time on this so apart from approve, you have to transfer some appropriate amount of tokens to the dex, so that it can payout.

7. Swapping t3 to t1 `await contract.swap(t3, t1, 100)` and do the same for t2.

> btw i was having some doubts and etherscan on goerli is on maintainance from last 5 hours, the allowance after the token transfers is okay but the token balances add up by 100 wei - felt weird to me and i am spent on this challenge so won't investigate anymore. a screenshot is added in the root dir (./confused dex two.png)... it was cleared when swapping for token 2 so we need to send 100 wei of token to dex, and that will be swapped for all of the token2.

8. success
