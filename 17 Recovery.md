## Recovery

1.  You have to find the address of the SimpleToken contract that is deployed using Recovery. This is fairly easy to do using blockexplorer. So I completed the chanllenge by going to block explorer => instance address i.e. contract Recovery => internal tx and then checking the deployed contract.

2.  After getting the address just paste the code of SimpleToken on remix and fetch it using address and call the destroy with any contract/eoa address.

3.  success

4.  After that I thought that would be another way to do that since i read somewhere that CREATE2 opcode can reveal the address since deterministic... and i wanted to know more so i searched on internet and found some links that can help in learning if necessary. Also looked the video of Web3 Blockchain youtube channel `https://www.youtube.com/watch?v=ODLTq3yZ0nM` but i did not perform it and added as reference apart from this i found `smart contract programmer` `https://www.youtube.com/@smartcontractprogrammer` video too who is making ethernaut series and he is on challenge 20 on making videos. And here is another video that i watched and will look the resources that he provided `https://www.youtube.com/watch?v=1fI7GkqVQSg` ... resources that i will look in future are `https://www.goodbytes.be/article/ethernaut-walkthrough-level-17-recovery` and `https://medium.com/coinmonks/ethernaut-lvl-18-recovery-walkthrough-how-to-retrieve-lost-contract-addresses-in-2-ways-aba54ab167d3` and will them from this md if they are not that useful.
