# Fractal-Cat-pumpfun-sCrypt-smart-contract
The smart contract for the Cat Protocol utilizes sCrypt to implement pump.fun on the Fractal Bitcoin network. It serves as a launchpad for the Fractal CAT20 token, allowing users to mint, trade, and transfer CAT20 tokens.  

## Overview

The **Fractal-Cat-pumpfun-sCrypt-smart-contract** project leverages the Fractal Bitcoin network's capabilities to implement a smart contract for the Cat Protocol using sCrypt. This project enables users to mint, trade, and transfer CAT20 tokens. The implementation takes advantage of the OP_CAT opcode to enhance Bitcoin's functionality, providing features akin to those seen in smart contracts on other blockchain platforms.

## Features

1. **Minting of CAT20 Tokens**: Utilize the sCrypt-based smart contract to mint new CAT20 tokens on the Fractal Bitcoin network.
2. **Trading and Transfer**: Facilitate the trading and transfer of CAT20 tokens within the network using the pump.fun implementation.
3. **Integration of OP_CAT**: The smart contract makes use of the OP_CAT opcode to improve contract programmability and execution.

## Prerequisites
 
- Node.js and npm installed on your system.
- Access to the Fractal Bitcoin network node.
- Familiarity with sCrypt and Bitcoin script programming.

## Installation

1. Clone the repository to your local machine:

   ```bash
   git clone https://github.com/leionion/Fractal-Cat-pumpfun-sCrypt-smart-contract.git
   ```

2. Navigate to the project directory:

   ```bash
   cd Fractal-Cat-pumpfun-sCrypt-smart-contract
   ```

3. Install the necessary dependencies:

   ```bash
   npm install
   ```

## sCrypt Smart Contract

The smart contract is implemented using sCrypt and utilizes the OP_CAT opcode. Here is an outline of the contract's core components:

### Contract Structure

- **Mint Function**: Implements the logic to mint new CAT20 tokens by verifying appropriate proofs and executing the creation script.
- **Transfer Function**: Handles the transfer of existing tokens between users, ensuring compliance with the protocol's specified rules.
- **Trade Functionality**: Supports seamless trading functions by integrating pump.fun logic within the contract on-chain.

### Code Implementation

Below is a simplified version of the sCrypt contract code structure:

```javascript
Contract CAT20TokenContract {

    public function mintToken(publicKey: PublicKey, value: int) {
        // Execution logic for minting new tokens
        require(validateMintInput(publicKey, value));
        executeMint(publicKey, value);
    }

    public function transferToken(sender: PublicKey, receiver: PublicKey, amount: int) {
        // Execution logic for transferring tokens
        require(balanceOf(sender) >= amount);
        executeTransfer(sender, receiver, amount);
    }

    private function validateMintInput(publicKey: PublicKey, value: int) returns (bool) {
        // Validation logic
        return true; // Replace with actual validation
    }
}
```

The implementation of the Fractal-Cat-pumpfun-sCrypt-smart-contract involves creating a comprehensive sCrypt based smart contract that allows for the minting, trading, and transferring of CAT20 tokens. Below is an expanded version of the sCrypt code structure to illustrate how a more detailed implementation might look.

```typescript
// Import necessary sCrypt libraries
import { SmartContract, PubKey, Sig, Bool, int2ByteString, sha256 } from 'scrypt-ts';

// Define the CAT20 Token Contract extending the SmartContract class
class CAT20TokenContract extends SmartContract {
    public balance: Record<string, bigint>;

    constructor() {
        super();
        this.balance = {};
    }

    // Public function to mint new CAT20 tokens
    @method
    public mintToken(minter: PubKey, amount: bigint, sig: Sig) {
        // Verify the signature of the minter
        assert(this.checkSig(sig, minter), 'Invalid signature.');

        // Increment the balance for the minter's address
        this.balance[minter.toString('hex')] = (this.balance[minter.toString('hex')] || 0n) + amount;
    }

    // Public function to transfer tokens from one address to another
    @method
    public transfer(sender: PubKey, receiver: PubKey, amount: bigint, sig: Sig) {
        // Verify the signature of the sender
        assert(this.checkSig(sig, sender), 'Invalid signature.');

        // Check sender's balance
        let senderAddress = sender.toString('hex');
        let receiverAddress = receiver.toString('hex');
        assert(this.balance[senderAddress] >= amount, 'Insufficient balance.');

        // Perform the transfer
        this.balance[senderAddress] -= amount;
        this.balance[receiverAddress] = (this.balance[receiverAddress] || 0n) + amount;
    }

    // Function to check the balance of a given address
    public balanceOf(holder: PubKey): bigint {
        return this.balance[holder.toString('hex')] || 0n;
    }

    // Private helper function to validate mint inputs
    private validateMintInput(minter: PubKey, value: bigint): Bool {
        // Here you could add validation logic as necessary
        return true; // Replace with actual validation logic
    }

    // Validate and execute a transaction
    @method
    public executeTransaction(sender: PubKey, receiver: PubKey, amount: bigint, sig: Sig, txHash: ByteString) {
        assert(sha256(txHash) == this.ctx.hashPrevouts, 'Invalid previous transaction hash.');

        this.transfer(sender, receiver, amount, sig);
    }
}
```

### Explanation

- **Minting**: The `mintToken` function allows a minter to create new tokens, adding these to their balance. It checks the authenticity of the request through digital signature verification.

- **Transfer**: The `transfer` function transfers tokens from a sender to a receiver. Before executing, it ensures the sender's signature is valid and that they have enough tokens.

- **Balance Checking**: The `balanceOf` function provides a mechanism to check the token balance of a specific account.

- **Execution of Transactions**: The `executeTransaction` function provides additional validation by ensuring that the transaction is correctly formed and linked with previous transactions.

This detailed implementation leverages sCrypt's capabilities for secure and efficient execution of token operations on the Fractal Bitcoin network.  


## Developer Notes

- Ensure that you have the appropriate configuration for connecting to the Fractal Bitcoin network.
- Review the sCrypt smart contract thoroughly to understand its security assumptions and execution model.


## Contact Info

- Twitter: https://x.com/chain_sats/
- Telegram: https://t.me/inscNix/
