## Token

1. It took lots of time to writeup and code so i was going to code first and write the summary from now on but i overlooked and was doing `await contract.transfer(player, 21000000)` (this 21M is total supply which i read) but this adds and subtracts from me bc i am the sender and receiver. I overlooked the underflow vulnerability.
2. `await contract.transfer(player, 21)`
3. success
