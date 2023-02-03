## Privacy

1. I know how storage works as well as reading private variables using ethersJs and web3Js. Also I watched and read many solidity videos and articles so it is not that hard for me but if you don't know than i highly suggest learning that. And I have a good source too because I watched that to see how he solves it ... he uses remix to solve it... I will do it directly

sauce : `https://www.youtube.com/watch?v=VSZl7jXZmfE` ... aslo this `https://medium.com/aigang-network/how-to-read-ethereum-contract-storage-44252c8af925` i haven't read but it was given after I completed the challenge

2. `(await web3.eth.getStorageAt(instance, 5)).slice(0,34)` this gives bytes32 at slot 5 and slot start with 0.... then I slice it to 16 bytes.

3. `await contract.unlock("0x2c978fd42eb73a05428e5f556a780253")` this passes 16 bytes and is key so unlock turns true.

4. success
