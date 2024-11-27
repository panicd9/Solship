# Solship - fully decentralized SVM Battleship game

In web2 making Battleship game is trivial, but in web3 where everything on blockchain is public, it can be challenging. Here we decided to use commitment schemes and Merkle proofs to hide ship positions.

Every player pays 0.005 SOL to play a game. The winner takes 95%, and the game keeps 5%.

First, players place their ships and enter a queue. Player can press R to rotate ship. Merkle tree is created from board state. A second player does the same, and they get matched. 
Once matched, a timer starts. Each turn, a player attacks, and the second player submits a Merkle proof to reveal whether they have a ship on the attacked field. 
Every field has 32-bit secret entropy to prevent brute forcing entire tree.

```
#[derive(AnchorSerialize, AnchorDeserialize, Debug, Clone)]
pub struct GameField {
    index: u8,
    ship_placed: bool,
    secret32: u32,
}
```

To enable a seamless experience, we have implemented session keys so users are not constantly prompted by the wallet extension to sign transactions.

We have implemented backend service that stores game state to not bloat Solana account space, so users can continue playing after disconnecting or reloading page.

After destroying all enemy ships, the player can claim victory by submitting their board. The Solana program verifies it on-chain to ensure fairness.

Also we have added scoreboard that tracks player's win and loses.
