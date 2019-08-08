# Decentralized Sums and Cows
Decentralized Sums and Cows(D-SnC) is a modified version of classical code-breaking mind game Bulls and Cows incentivized by blockchain.

## Rules

### Characters

Tow characters each game: 1 player and 1 server

### Main Steps

1. The server chooses 10 non-repeating letters from Latin alphabet as set `S`.
2. The server assigns 10 numbers from 0 to 9 to each element in `C`.
3. The player chooses 10 non-repeating letters from Latin alphabet as set `C`.
4. The player assings 10 numbers from 0 to 9 to each element in `C`.
5. The player sends `C` and the assigned numbers to the server. If `C` is exactly the same as `S` and the assgined numbers are also the same, the game ends and the player wins. Otherwise, go to Step 6.
6. The server calculates the intersect set `R` of `S` and `C`. Each element in `R` inherits the corresponding assigned number to `S`.
7. The server calculates the sum `sum(R)` of the assigned numbers to `R`.
8. The server sends `|R|` and `sum(R)` to the player. Go to Step 3. Otherwise, the game ends and the player loses.

### Fees and Rewards

1. The player is charged the starting fee to start a new game with the server.
2. Step 5 charges the player the requesting fee.
3. If the player loses, nothing happens.
4. If the player wins, the player is rewarded the prize.
