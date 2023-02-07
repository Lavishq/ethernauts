## Magic Number

1. After reading the question it was apparant that i needed to code the contract using bytecode and i already had watched some videos on evm and some articles in past on how it works but didn't wanted to write the contract manually as it was too cumbursome and i felt i will make too many mistakes and will like end up getting frustrated. So I cheated and it was a smart thing todo in my case because it will give me the understanding needed to move ahead without working on something i already know and btw i already have plans to study huff but after the next 8 weeks presumably, priorities...

2. I first went to smart contract programmer and saw the solution and you can do that too and the other d-squared video (channel) had some res like => https://listed.to/@r1oga/13786/ethernaut-levels-16-to-18 which i read once. And it even had https://medium.com/coinmonks/ethernaut-lvl-19-magicnumber-walkthrough-how-to-deploy-contracts-using-raw-assembly-opcodes-c50edb0f71a2 but i did not read it because i wanted to move ahead.

3. solution `web3.eth.sendTransaction({ from:player,data: '0x69602a60005260206000f3600052600a6016f3' })`
   => `await contract.setSolver("0xb93e3736142852f1f6823d25e5de0c2cbdd76adc")` remember to add your contract

4. success
