## Denial

1. After you read the contract Denial, you will conclude that only setWithdrawPartner() and withdraw() are changing state and we have to first setWithdrawPartner() and then withdraw() sends all the gas using .call() to partner. So, you have to use that to consume all the gas in order to fail the tx and the owner should not be able to withdraw a penny but lose on gas.

2. To solve the first thing that came in mind was to use a while loop in fallback() to consume all the gas, but i wanted to see if there are other methods so i watched D-squared, he gave the assert example and i tried it but it failed the reason is below.

```solidity
// it used to work in previous comipler's /evm/ solidity version and now assert does not consumer all the gas
contract DenialAttacker {
    fallback() external payable {
      assert(false);
    }
}
```

3. Next thing i did is watch smart contract programmer, and then i learnt below solution and used it to pass.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Attack{
  receive() external payable {
    assembly {
      invalid()
    }
  }
}

```

4. I set the partner using console of the browser and then sumbit.

5. success
