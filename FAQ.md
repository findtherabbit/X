# FAQ Find The Rabbit

## What you'll need

### Dapp-compatible browser plugin
Since its cryptocurrency you will make your bets with, you will need a browser plugin that will help operate with the tokens and sign transactions. We personally recommend [MetaMask](https://metamask.io/). Or you can use any other Dapp compatible plugin to your tasting.

If you follow our recommendations, **install the plugin to your browser** and **create an account there**.

During the creation of your account, you will be shown your seed phrase – please create backup for your safety.

## Game rules

- There are two roles in each game: *Host* and *Join*. *Host* has to make a choice where to hide the rabbit, left or right, and to select a bet to place. Once *Host* has created the game, it’s broadcasted as available to join to all online players.


- *Join* has to select a game he/she wants to join with an equivalent bet amount. Then *Join* needs to guess where *Host* has hidden the rabbit, left or right, and click *Join*.



- *Host* receives the message that someone is willing to join his game and has to click "Accept" if he is comfortable with the reputation of the Join player by evaluating it by *Join's* total games and number of disputes. Once *Host* accepts it, both players have to send funds to the smart contract.




- Reveal stage. Each player checks the smart contract automatically. Once they see the smart contract has received funds from their opponent, they reveal their secrets to each other. 

- Finally, one of them doubles its bet by winning and making a transaction to take the profit.

- You can host and join several games at the same time.

## QA
    
> ### 1. Which cryptocurrencies can I use to bet with?

The game supports **POA.network**, **Ethereum mainnet**, and **testnets**. All you need is a native cryptocurrency for one of the supported networks and web3-compatible wallet to play (e.g., [MetaMask](https://metamask.io/)).

> ### 2. Is Find The Rabbit trustless?

**Absolutely.** All secrets are stored in the browser (local storage). Neither *Game Oracle* nor *Smart Contract* knows your choice.

> ### 3. Is that possible to cheat in this game?

All cheaters are punished by honest players. *Game Oracle* and the client side of the Find The Rabbit app keep a closer look at the Smart Contract and will help a player to open a *dispute* against a dishonest player. There is a reputation system, which keeps track of the following:
* Total games played
* Average bet amount
* Disputes opened for not revealing the secret
* Disputes opened by a player against the other.
* Games that weren't funded at the required time.

> ### 4. What is the fee charged by Find The Rabbit?

Let's say a host places a bet of 10 ETH to play the game. Join accepts it and loses. The total sum would be 20 ETH to take minus:
        - 1% goes to Jackpot
        - 1% goes to Referrer (if there's one)
        - 8% goes to Game Oracle
    - in total: 20 ETH - 10%  = 18 ETH with a 8-ETH profit.

> ### 5. What happens if my opponent doesn't reveal the secret? 
Every player has limited time to reveal the secret (2 minutes). Once the time has passed, the player can open a dispute by making a transaction to the Smart Contract. Then the opponent receives a notification that the dispute opener has created the dispute against him/her. The disputant has 5 minutes to resolve the dispute if it has any relevance for him/her to do so because at that time the secret can be seen at the other player's onchain. So, the disputant still has a chance to win the game. If the disputant sees the loss is inevitable, the dispute opener has to wait 5 minutes to pass and only then he/she can make a transaction to take the profit.

> ### 6. What happends if I go offline?

If you were *Host* awaiting other players to play with, all your games will be saved in the browser cache and the *Game Oracle* will make them offline. If there are some in-play games, we highly recommend you not go offline because you might lose them if the other person opens a dispute against you for not revealing a secret.

> ### 7. Jackpot mechanism

Each last completed game takes part in Jackpot drawing - if there are no other games finished before Jackpot Draw date, then participants share Jackpot 50/50. Each next game adds 5 minutes to Jackpot Draw date. That scheme is also known as FOMO3d.

