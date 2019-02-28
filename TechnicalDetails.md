## Find The Rabbit Protocol
1. *Host* creates a game and sends a request to *game oracle* that propagates the request to all available online players.
*Host* stores the secret and random salt in his `localStorage`
```js
hostSecret = 1 // localStorage
hostSalt = randomHex(32) // localStorage
hashOfHostSecret = soliditySha3(secret, salt) // publicly sent value
```

2. To accept the game, *Join* has to generate his own secret for that game, random salt, hash and signature using [EIP-721](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md)
```js
joinSecret = 1 // localStorage
joinSalt = randomHex(32) // localStorage
hashOfJoinSecret = soliditySha3(secret, salt) // publicly sent value
joinSignature = sign(hashOfHostSecret, isHost: false, hostAddress, hashOfJoinSecret, betAmount)
```

3. *Host* receives the request from the *Game Oracle* and decides if he wants to play with *Join* based on the player's statistics:
* total games
* total disputes.
Once accepted *Host* generates his signature:
```
hostSignature = sign(hashOfHostSecret, hashOfJoinSecret, isHost: true, joinAddress, betAmount)
```
sends it to *Game Oracle* to notify the Join.

4. Commit phase. Both players should send eth transaction into the network.
https://etherscan.io/tx/0xa3ad2ddc8414756e2dc8f2ab483e694bd6a2636d827acd47318f6e935c24973a
```
0 _isHost                bool     false
1 _hashOfMySecret        bytes32  d5229005f43a51f2ae3c360f52cec0ead3e0229b653bd6c875a3beed3a3e7515
2 _hashOfOpponentSecret  bytes32  d247d558e10bf6044ad63f8f0d05faf9438f782e70cbd87e4eddfb1bb3ef8b28
3 _hostSignature         bytes    b193c61814ff29a5cd854bed0904f93326d1cb308ccb04b2190664f89c4bb69d5e09a73c8d26586f56b31c73720779c298e4191184dad57d984c58dd7d6e2fa51c
4 _joinSignature         bytes    7a524dca250b646d7f0a241645fb6567c4d5ef5cf3a4c80077ccbab1f53a6d5858e33e265df1d0f390238a7fa058a7eb5e2ba34e08c37103c606707b01c37e361b
```
At this stage, contract validates all parameters:
* Players' addresses
* Bet, which they both agreed
* Signatures

5. Reveal phase. Once both players verify the smart contract that the funds were sent, 
they reveal their `secret` and `salt` to the *Game Oracle*.

6. Reward phase. *Host* or *Join* should call `win` function on smart contract 
https://etherscan.io/tx/0xc58bf999ba545b270ce00e45cf427bd36db4d556e7713616c8514b8beadec6f5
```
0 _gameId     bytes32 d3113f46a630f4898d0dbd9d6c7fe724ffaf8371f12819653ba5a1f106720d5c
1 _hostSecret uint8   1
2 _hostSalt   bytes32 3a27d621fd379c392e674f16d37cd8ad23d92bd2e891768ee0316ddc79a673a4
3 _joinSecret uint8   1
4 _joinSalt   bytes32 0003afc1efc4886c3a7cffe17aeff8dcf2cf485de0593a12242d6c8d25cbb65b
5 _referrer   address 0000000000000000000000000000000000000000
```
Smart contract verifies each `salt` and `secret` based on previous submission of `hashOfJoinSecret`, `hashOfHostSecret` and
sends the reward to the winner.

-----
Disputes.
1. Game Cancel. If Host or Join didn't receive a transaction to fund the game, he/she can cancel the game and get a full refund. They can do that after `timeUntilCancel` function reaches `0` only.
2. Reveal is not received. If Host or Join didn't receive a reveal, he/she can call `openDispute` to start dispute. They can do that only after `timeUntilOpenDispute` reaches `0` The dispute opener should reveal on-chain at this stage:
```
0 _gameId               bytes32  de346b0603be205b9ca4be9363089d58510f068837ad091c00bcefa1bb5d3f46
1 _secret               uint8    1
2 _salt                 bytes32  1962f9d0581efdf52597cd0c199e7f5e23a5cd9c87258ea2a7eb01910abceea4
3 _isHost               bool     true
4 _hashOfOpponentSecret bytes32  803cce932639aeae95d449a01b8ed52794b19f2cc039c058cfdedcd00fe461e9
```
https://etherscan.io/tx/0xd0efa4f200e103095d1e149af79fc15e7b41b1908c3321bf7b02d1d77dc6063e

Opened dispute can be closed by dispute opener or resolved by opponent:
* Resolving Dispute. As soon as dispute has been opened, opponent can resolve it by calling `resolveDispute` (params are the same as for `openDispute`). Then smart contract will calculate who win and send reward to *Join* or *Host*.
* Closing Dispute. If the opponent doesn't resolve the dispute on time (5 minutes), the dispute opener can call `closeDisputeOnTimeout` to become a winner in the game. 
https://etherscan.io/tx/0xc269952867cbad250aee7daa6d59377a67b626268cb80c600c6e847188f8c917
```
0 _gameId bytes32 d13b28ea8eb2f0b56198f9dfe37eb971c61b1e3530f077cc097070e7a321997b
```

**Note.**
All required actions like calling smart contract or keeping track of smart contract state are implemented on client side of Find The Rabbit game (and on Game Oracle side actually), so user will be prompted for any neccessary interactions. Happy hunting!
