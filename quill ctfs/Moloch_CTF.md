### So, the first thing is constructor where it stores all the variables, immutable variables are stored in the bytecode itself

variables Moloch, hy7UIH and hsah are stored in bytecode

# 1.
## Detailed formula for finding slot of dynamic struct

the key for realHacker is stored in slot0 
question[0] and question[1] are stored in slot1 and slot2
slot3 stores the 3 as key since the number of slot is 3 to find the elements of array. Below is the formula to find it.

> calling formula(3) gives the storage location (slot) of cabals[0] which has address identity on the slot and password on slot + 1 and you can just add 1 to the slot location and it will have the next elements of array

```solidity
function formula(uint256 slot) public pure returns (bytes32) {
    bytes32 hash = keccak256(abi.encodePacked(slot));
    return hash;
}
```

You can't solve the problem by calling initialization since it will always fail at `require(lengthAfter  >= lengthBefore,"None added");` and you can't call sendGrant because you won't satisfy first condition. So that leaves us with `uhER778(string[3] memory _openSecrete)` and here we need `hsah` which was stored using `molochPass`, and `hy7UIH` which was stored using _question[0], _question[1] which is public variable. so, we only need these variables to call these functions.

# 2.
## Details of decrypting Moloch-algorithm
from tweet
> "RFG*DWRWPG*QD*FWKCLKRW*PGOWKPGQ*RFG*QCAPKDKAG*QD*WQWP*QFCJJQU*BGQKPGQ"

in constructor
> "THE FUTURE OF HUMANITY REQUIRES THE SACRIFICE YOUR SHALLOW DESIRES" 

so if you see the differences between them you can see that the alphabets leave one position and then got to next somewhat similar to symmetric cryptography
> R -> S -> T, F -> G -> H 
> RF -> TH 

But then it goes in reverse for G -> F -> E

using that we can try to solve 
`ZJQQBW*NFCPKCAKQR"`
> which we get using slots, you can use getStorageAt("<contractAddress>", slot + 6)) to get the slot address/location in storage using ethersJs or web3.js, the reason 6 is added is because the storage slot of cabals[0] will occupy 2 slots and cabals[1] will occupy 2 slots and then `cabals[3]` which has identity and password so identity will be in 5th slot since 0 (0 being the once we calculated using getStorageAt()) and password is encrypted using moloch-encryption will be present in 6th slot so adding 6.

If you try to decrypt the password by adding 1 element it gives `BLSSDY PHERMECMST` but there were cases when the encryption was in reverse direction like `E` of `THE` so we can do that and we get `BLOODY PHARMACIST`

Now, we already know the 

> slot 1 question[0] - # question[0] = _b[0] = `WHO DO YOU`
> slot 2 question[1] - # question[1] = _b[1] = `SERVE?` and even the variable question[] is a public function so it has getter functions

So, calling the function `uhER778()`

["BLOODY PHARMACIST","WHO DO YOU","SERVE?"] with this arguments should solve it but the condition 
`require(keccak256(abi.encode(_openSecrete[1])) != keccak256(abi.encode(question[0])),"grant awarded!!");`
takes the second argument and compares it with the `hash(question[0])` and it should not be equal. It is explained in next block

# 3.
## Explain bypass for keccak256(abi.encodepacked()).

Well, since `abi.encodePacked()` packed the data meaning removes the empty data blocks and packs the data, so we can pass `WHO DO YOUSERVE?` as second argument and we can `""` as third (pass an empty string), that will still have same hash in case of `require(hy7UIH == keccak256(abi.encodePacked(_openSecrete[1],_openSecrete[2])), "Hahahaha!!");` 

And `abi.encode()` does not remove empty data blocks and leaves it as it is.

The reason `keccak256(abi.encode(_openSecrete[1])) != keccak256(abi.encode(question[0])` is because `abi.encode(_openSecrete[1])` is `WHO DO YOUSERVE?` but `abi.encode(question[0])` is `WHO DO YOU` so both will return different hash hence the not equal to check passes and transaction proceeds.

["BLOODY PHARMACIST","WHO DO YOUSERVE?",""]

## finally you need to call using a contract and re-enter
the reason is because the local variables in the the `uhER778()` RGDTjhU stores the balance of contract before and YHUiiFD stores the balance after 1 wei is transferred to msg.sender

> Suppose the contract balance is 98 wei and you pass 2 wei after calling `uhER778()`, `RGDTjhU` will be 100 wei since the state changed and it will trigger external call on sender, sends 1 wei to msg.sender and now the `YHUiiFD` is 99, 

the `require(YHUiiFD - RGDTjhU == 1, "sacrifice your shallow desires" );` causes 99-100 = -1 and it fails since solidity v0.8.x comes with safeMath built in, to remediate this you use the receive() of the contract to which it sends 1 wei and in receive function add the logic to call `MOLOCH_VAULT.receive()` and pass 2 wei
doing that the `YHUiiFD` balance will be `RGDTjhU + 2` so when `YHUiiFD - RGDTjhU` happens it will be `RGDTjhU + 2 - RGDTjhU` but we already sent 1 wei to msg.sender so it should be `RGDTjhU + 1 - RGDTjhU` and it satisfies the condition. 

## Contract:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

interface I_MOLOCH_VAULT {
    function uhER778(string[3] memory _openSecrete) external payable;
}

contract Attack {
    I_MOLOCH_VAULT moloch;

    receive() external payable {
        (bool sent,)= payable(msg.sender).call{value: 2 wei}("");
    }

    // 0xaFB9ed5cD677a1bD5725Ca5FcB9a3a0572D94f6f
    // ["BLOODY PHARMACIST","WHO DO YOUSERVE?",""]

    function attack(string[3] memory _openSecrete) external {
        moloch.uhER778(_openSecrete);
    }

    constructor(address _address) payable {
        moloch = I_MOLOCH_VAULT(_address);
    }
}
```

## JavaScript (Hardhat):
```javascript
async function deploy() {
const Attack = await ethers.getContractFactory("Attack");
const attack = await Token.deploy("0xaFB9ed5cD677a1bD5725Ca5FcB9a3a0572D94f6f");

const arrrr = ["BLOODY PHARMACIST", "WHO DO YOUSERVE?",""]
await token.attack();
}

```