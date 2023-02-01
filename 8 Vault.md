## Vault

1. I have done similar things using ethersJs, from learnweb3 dao for the first time. you use provider.getStorageAt(address, slot) // slot usually starts with 0 and i guess this will be different where contracts use upgradables... i am not sure 100%

2. Since we already know that ethernauts has web3 lib installed so we can just `await web3.eth.getStorageAt(instance, 1)` // locked is in 0 and password in 1st slot as per my knowledge ... lets try and it gave me `0x412076657279207374726f6e67207365637265742070617373776f7264203a29`
   should've stored in const var and then passed it to unlock()
3. success

> Note: there is another way to read from blockchain using alchemy or json rpc calls ... i will add that link here if i remember and find it ... so that this repo can be used to reference quickly or to find some res which i forget and bookmarks are always deleted
