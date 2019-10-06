# Decentralized Sums and Cows
Decentralized Sums and Cows(D-SnC) is a modified version of classical code-breaking mind game Bulls and Cows incentivized by blockchain.

## Rules

### Characters

Tow characters each game: 1 player and 1 server

### Main Steps

1. The server chooses `d` of `n` non-repeating letters as set `S`.
2. The server assigns `d` consecutive numbers starting from 0 to each element in `C`.
3. The player chooses `d` of `n` non-repeating letters as set `C`.
4. The player assigns `d` consecutive numbers starting from 0 to each element in `C`.
5. We call `C` and its assigned numbers as a *guess*. The player sends its *guess* to the server. If `C` is exactly the same as `S` and the assigned numbers are also the same, the game ends and the player wins. Otherwise, go to Step 6.
6. The server calculates the intersect set `R` of `S` and `C`. Each element in `R` inherits the corresponding assigned number to `S`.
7. The server calculates the sum `sum(R)` of the assigned numbers to `R`.
8. The server sends `|R|` and `sum(R)`, called the *result* to the *guess*, to the player. Go to Step 3. Otherwise, the game ends and the player loses.

### Fees and Rewards

1. The player is charged the starting fee to start a new game with the server.
2. Step 5 charges the player the requesting fee.
3. If the player loses, nothing happens.
4. If the player wins, the player is rewarded the prize.

## Hierarchical Design

 ### Layer 1

#### Functionalities

- Deposit: Exchange built-in token to game token at a certain rate.
- Withdraw: Exchange game token to built-in token at a certain rate. It is allowed if and only if for all games of the account, there is no one's state is *end*.
- Initial a game: One account consumes a certain amount of game token to start a game G along with a unique game id. The state of one game can be *on* or *end*. One account can initial multiple games at the same time.
- Handle game end claims: Verify claims. If passed, change the state of the game from *on* to *end* and update the client's account balance.
- Handle server misbehavior claims: Verify claims. If passed, the client wins the game. Otherwise, the game comes to the end immediately.
- Handle server failure claims: Store claims from clients and create time windows respectively waiting for the server to respond. Verify a respond and corresponding claim. If no response from the server within the time window or the verification fails, the client wins the game.

### Layer 2

#### Client Functionalities

- Guess: Send a *guess* request to the server. Refer to Step 5.
- Verify: Verify responses from the server. If failed, claim server misbehavior.
- Claim the end of a game: Submit all the *guesses* and *results* within one game to Layer 1, so that to bring a game to the end.
- Claim server failure: If the server doesn't respond for a certain time, push a claim to Layer 1.
- Claim server misbehavior

#### Server Functionalities

- Respond to clients' requests: Upon receiving a *guess* request from a client, respond to the client with the *result*. Refer to Step 8. The server will check if the client's account balance is sufficient.
- Respond to server failure claims on Layer 1: Push the corresponding *result* to Layer 1.