## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

-   **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
-   **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
-   **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
-   **Chisel**: Fast, utilitarian, and verbose solidity REPL.

## Project Description 

## Raffle (Lottery) Smart Contract
The Raffle contract is a decentralized lottery system that allows users to participate by sending a minimum amount of Ether. The winner is selected randomly using Chainlink VRF (Verifiable Random Function) for provable fairness. The contract also uses Chainlink Automation (Keepers) to automate winner selection at regular intervals.

## How It Works
1️⃣ Players Enter the Raffle
Users join the lottery by calling enterRaffle() and sending ETH.
If the amount sent is less than the required entranceFee, the transaction reverts.
If the raffle is not open, the transaction also fails.
Once a valid entry is made, the player’s address is added to s_players[], and an event RaffleEnter is emitted.
2️⃣ Chainlink Automation Checks if It's Time to Pick a Winner
The function checkUpkeep() runs automatically to verify if the raffle should proceed to the winner selection phase.
Conditions checked:
✅ Raffle is OPEN
✅ Time interval has passed
✅ At least one player has entered
✅ Contract has ETH balance
If all conditions are met, checkUpkeep() returns true, triggering performUpkeep().
3️⃣ Chainlink VRF Requests a Random Winner
performUpkeep() changes the raffle state to CALCULATING and requests randomness from Chainlink VRF.
An event RequestedRaffleWinner is emitted with the request ID.
The function fulfillRandomWords() receives the random number, determines the winner, and distributes the prize.
4️⃣ Winner is Selected and Paid
The index of the winner is determined using:
ini
Copy
Edit
indexOfWinner = randomWords[0] % s_players.length;
The winner's address is stored in s_recentWinner, and the player list is reset.
The raffle state is set back to OPEN for new entries.
The entire contract balance is transferred to the winner. If the transfer fails, the transaction reverts.
## Key Features
🔹 Uses Chainlink VRF for randomness, ensuring fair winner selection.
🔹 Uses Chainlink Automation (Keepers) to automate the process.
🔹 Implements proper error handling with custom errors.
🔹 Stores player entries and ensures only valid participants can enter.

## Deployment & Testing
Deployment Scripts
The contract is deployed on Sepolia Testnet using Foundry scripts.
Uses Anvil (a local Ethereum test environment) for local testing.
The deployment script initializes:
Subscription ID for Chainlink VRF
Gas Lane (KeyHash) for randomness
VRF Coordinator
Callback Gas Limit
Entrance Fee
## Testing Scripts
Unit Tests: Validate functions like enterRaffle(), checkUpkeep(), and fulfillRandomWords().
Integration Tests: Simulate real-world conditions, including multiple players and random winner selection.


## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell
$ forge script script/Counter.s.sol:CounterScript --rpc-url <your_rpc_url> --private-key <your_private_key>
```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```
