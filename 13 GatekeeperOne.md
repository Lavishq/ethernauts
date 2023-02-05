## GatekeeperOne

1. This was hardest challenge so far, i tweeted the problems i faced so here,
   which is necessary to look `https://twitter.com/_Lavishq/status/1622140404870320130?s=20&t=lBtKIQXjgKSgX60U53cvrg`

2. The easiest case is to pass modifier one, it needs to be called using a contract.

3. Second modifier looks really easy as well but it is the sole reason i am not passing the test... the gasLeft() when modifier is called, it should be a multiple of 8191 and it needs to be have remainder 0... below are some examples of gas.. but i have still not figured correct gas to be passed.

> some examples : 1. if gasLeft() is 8190 then when `gasLeft() % 8191 == 0` occurs the remainder is `8190` != 0 so it fails.... and so on 2. gasLeft() should be something like `8191`, `16381` or any of the multiples of `8191` will give remainder 0 but when you call the transaction there is gas used at when performing operations so gasLeft() will be different than what you pass. So, in order to make it work you have pass the correct gas to be used and when gasLeft modifier is called then the gas should be a multiple of 8191 so that it the remainder is 0 and modifier passes then it goes to modifier 3.... i still have not figured a way to calculate initalial gas_used when the gasLeft() is called... so i will explain modifier 3 now and once that is done we will come back to calculate gasLeft()

4.

- uint64 takes bytes8 of space so each byte is uint8 and that makes 8\*8 bytes =>64 bits of uint
  > 1.  require(uint32(uint64(\_gateKey)) == uint16(uint64(\_gateKey)))
  >     // 00000000 "0000 810a" (uint32) == 0000 "`810a`" (uint16) => here the first
  > 2.  require(uint32(uint64(\_gateKey)) != uint64(\_gateKey))
  >     // `00000000 "0000 810a"` (uint32) != "00000000 00000000" + "00000000 0000 810a" => ...this fails because both are equal
  >     // 00000000 "0000 810a" (uint32) != `"00000000 00000001" + "00000000 0000 810a"` => we can edit the first 32 bits to any value and so it becomes unequal i just gave `0x01` and then the rest of the 32 bits
  > 3.  require(uint32(uint64(\_gateKey)) == uint16(uint160(tx.origin)))
  >     // 00000000 "0000 810a" (uint32) == `0000 "810a"` => (uint16 of tx.origin)
  >     finally, we pass the requirement for second gate => "00000000 00000001" + "00000000 0000 810a" => `"0x1000000000000810a"`

5. The code should look like this...

> Note: And we need to pass the correct gas and level will be completed but the problem is how to find the correct gas... maybe we have to use solidity debug on remix to see what each instruction and add the gas used till it reaches gasLeft() and then add that to a multiple of 8191. and the challenge passes and we capture the flag and be happy :)
> or there will be a need to do a for loop and pass different gas on every external call but idk how efficient will it be and maybe doing instructional method will be advantageous during the learning. So i will try to solve it using remix debug
> I will put how to calculate the gasLeft)(), after the code block once i finish the challenge :(

```solidity
interface GatekeeperOne {
  function enter(bytes8 _gateKey) external;
}

contract Attack {
    GatekeeperOne gate;

    //0x5eDca3A569692DFBca1CD961fBAD660b37875716 //instance
    constructor(address _addr) {
        gate = GatekeeperOne(_addr);
    }
    //0xB7cA5Ae7FFcf1E7846e3C3eDc1bD36b7ff33 810a //tx.origin => key => "0x1000000000000810a"
    function attack(uint256 _gas, bytes8 addr) public {
        gate.enter{gas: _gas}(addr);
    }
}
```

6. Calculating gasLeft() =>
