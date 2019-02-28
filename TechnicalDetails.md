## How it works
1. Host creates a game and sends a request to *game oracle* that propagates the request to all available online players.
Host stores the secret and random salt in his localStorage
```js
hostSecret = 1 // localStorage
hostSalt = randomHex(32) // localStorage
hashOfHostSecret = soliditySha3(secret, salt) // publicly sent value
```

2. To accept the game, *join* has to generate his own secret for that game, random salt, hash and signature using [EIP-721](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md)
```js
joinSecret = 1 // localStorage
joinSalt = randomHex(32) // localStorage
hashOfJoinSecret = soliditySha3(secret, salt) // publicly sent value
joinSignature = sign(hashOfHostSecret, hashOfJoinSecret, betAmount)
```

3. Host receives the request from the *Game Oracle* and decides if he wants to play with Join based on the player's statistics:
total games, total disputes.
Once accepted Host generates his signature:
```
hostSignature = sign(hashOfHostSecret, hashOfJoinSecret, betAmount)
```
sends it to *Game Oracle* to notify the Join.

4. Commit phase. Both players should send eth transaction into the network.
https://etherscan.io/tx/0xa3ad2ddc8414756e2dc8f2ab483e694bd6a2636d827acd47318f6e935c24973a
```
0	_isHost	bool	false
1	_hashOfMySecret	bytes32	d5229005f43a51f2ae3c360f52cec0ead3e0229b653bd6c875a3beed3a3e7515
2	_hashOfOpponentSecret	bytes32	d247d558e10bf6044ad63f8f0d05faf9438f782e70cbd87e4eddfb1bb3ef8b28
3	_hostSignature	bytes	b193c61814ff29a5cd854bed0904f93326d1cb308ccb04b2190664f89c4bb69d5e09a73c8d26586f56b31c73720779c298e4191184dad57d984c58dd7d6e2fa51c
4	_joinSignature	bytes	7a524dca250b646d7f0a241645fb6567c4d5ef5cf3a4c80077ccbab1f53a6d5858e33e265df1d0f390238a7fa058a7eb5e2ba34e08c37103c606707b01c37e361b
```

5. Reveal phase. Once both players verify the smart contract that the funds were sent, 
they reveal their secret and salt to the game oracle.

6. Reward phase. Host or Join should call `win` function on smart contract 
https://etherscan.io/tx/0xc58bf999ba545b270ce00e45cf427bd36db4d556e7713616c8514b8beadec6f5
```
0	_gameId	bytes32	d3113f46a630f4898d0dbd9d6c7fe724ffaf8371f12819653ba5a1f106720d5c
1	_hostSecret	uint8	1
2	_hostSalt	bytes32	3a27d621fd379c392e674f16d37cd8ad23d92bd2e891768ee0316ddc79a673a4
3	_joinSecret	uint8	1
4	_joinSalt	bytes32	0003afc1efc4886c3a7cffe17aeff8dcf2cf485de0593a12242d6c8d25cbb65b
5	_referrer	address	0000000000000000000000000000000000000000
```
Smart contract verifies each salt and secret based on previous submission of `hashOfJoinSecret`, `hashOfHostSecret` and
sends the reward to the winner.

-----
Disputes.
1. Cancel transaction. If Host or Join didn't receive a transaction to fund the game, he/she can cancel the game and get a full refund,
after `timeUntilCancel` function reaches `0`.
2. Reveal is not received. If Host or Join didn't receive a reveal, he/she can call `openDispute` to start dispute only
after `timeUntilOpenDispute` reaches `0`.
3. Closing Dispute. If the opponent doesn't resolve the dispute on time, the dispute opener can call `closeDisputeOnTimeout` 
to become a winner in the game. 
