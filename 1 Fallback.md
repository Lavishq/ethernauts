## Fallback

1. Read the contract and understand, important thing to understand is contribute function. And look for the places where the owner can modified.
2. Now the contribute() has the requirement that the msg.value i.e eth sent to the function needs to be below 0.001 and once the the senders contribution exceeds the owner contribution then I can be the owner.
3. The problem here is that i need to keep doing passing 0.001 eth repeatedly until the contribution is more than that of the owner.
4. There is another place where the owner can be change and that is receive() where the only requirement is msg.value != 0 so if i directly use metamask to transfer the eth/wei to contract it will be fulfill the first condition but the require uses && operator so we need both the condition to be fulfilled.
5. The second condition is that contribution of sender/caller should be more than 1 wei so we will need to call the contribute() with any amount of eth/wei. And then transfer some eth using metamask or by triggering receive function by where you pass some wei. And you can be the owner.
6. await contract.contribute(value:1) i tried this but i got syntax error and i remembered value should be passed as object (ethersjs and javascipt scripting)
7. I use this await contract.contribute({value:1} and check if it contributed after mined by calling getContribution() so the second condition of receive() .... require is fulfilled and now just transfer using metamask and it will be done.
8. I called contribute using arrow keys again because network is slow as of 1 Feb 2023 and 16.06 ist
9. Okay all of my trx were not passing so went and cancelled all the trasaction and paid high gas fees since it is test ether, maybe by default it gives less gasPrice/limit.
10. I can use metapask and it will trigger receive or use sendTransaction() provided by openZappelin... in the console
11. Now i used metamask and passed 0.0001 eth.
12. Once the tx is mined doing contract.withdraw() will drain the contract.
13. After tx is mined submit.
14. success.
