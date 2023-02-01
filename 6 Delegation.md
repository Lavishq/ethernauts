## Delegation

1. I forget it sometimes and need to revise sometimes, so there is contractA i.e. Delegate and contractB i.e Delegation... the functions of contractA are called by contractB in the context of contractB
2. Meaning contract Delegation has owner and that owner can be changed using the function pwn() of contract Delegate

> Not Important
> ps: you can read soliditybyexamples delegate call which i highly refer bc austin griffith is awesome mentor and always mentioned that reference and that was very helpful to me, in early days i forgot some concept and i went to solidity by examples for reference instead of solidity docs...

> Note: state or address of owner of Delegate contract remains the same even after the pwn() is called via delegatecall() because it changes the state only of Delegation contract, also refer fallback() and receive() if you don't know about them.

3. you need to pass the abiWithEncode/abiWithSignature or interface as msg.data, it is bytes8 hex of the function hash. I will add the examples so that I will be able to quickly read them

> Code

#### detailed

```solidity
function sendAlert(address hero) external {
        bytes4 signature = bytes4(keccak256("alert()"));
        (bool success, ) = hero.call(abi.encodePacked(signature));
        require(success);
    }
```

#### non detailed one from AU

```solidity
function sendAlert(address hero) external {
        bytes4 signature = bytes4(keccak256("alert()"));
        (bool success, ) = hero.call(abi.encodePacked(signature));
        require(success);
    }
```

or

```solidity
bytes memory payload = abi.encodeWithSignature("rumble()");

(bool success, ) = hero.call(payload);
```

#### here is an example with arguments

And if you want to add arguments, you can add them to signature and as comma separted arguments to the encodeWithSignature method. If rumble took two uint arguments, we could pass them like this:

```solidity
bytes memory payload = abi.encodeWithSignature("rumble(uint256,uint256)", 10, 5);

(bool success, ) = hero.call(payload);
```

We won't use another contract to do this but javaScript but you can do it using Remix using above methods

4. To get the keccak hash of pwn(), you can `const pwnsig = web3.utils.sha3("pwn()")` note the web3 node lib.
   Alternatively, you can go to `https://cypherhash.vercel.app/hash` and put `pwn()` and click the hash output and it will be copied...

5. then you can just enter that hash `await contract.sendTransaction({data: <signature/digest/hash>})` => `await contract.owner()` => outputs your address

6. success

> tldr; pwn = own || pwned = owned
> Bonus: in case you already know "pwn" meaning you can skip, pwn is mostly used by hackers who are too much into it... idk any way to explain that, many people know what it means and many didn't ... many people know it bc they are researchers or must have heard it from somewhere and many don't bc it's not like you need to know everything
