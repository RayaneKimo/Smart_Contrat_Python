# Smart Contract Transaction Scheduler

The purpose of this smart contract is to facilitate scheduled transactions between different addresses. It allows users to schedule a transaction to be executed at a specific timestamp in the future.

Here's a breakdown of the contract's functionality:

1. **State Variables**:
   - `owner`: An address variable representing the contract owner.

2. **Events**:
   - `ident`: An event triggered when a transaction is scheduled, emitting the transaction ID (`ID`).

3. **Struct**:
   - `transact`: A struct representing a transaction with the following properties:
     - `receiver`: The address of the transaction receiver.
     - `sender`: The address of the transaction sender.
     - `amount`: The amount of ether to be sent in the transaction.
     - `timestamp`: The timestamp at which the transaction is scheduled to be executed.
     - `done`: A boolean indicating whether the transaction has been executed.

4. **Mappings**:
   - `tableau_transaction_remain`: A mapping from transaction IDs (`bytes32`) to `transact` structs representing the scheduled transactions that are yet to be executed.
   - `tableau_transaction_done`: A mapping from transaction IDs (`bytes32`) to `transact` structs representing the transactions that have been executed.

5. **Constructor**:
   - Initializes the `owner` variable with the address of the contract deployer (`msg.sender`).

6. **Fallback Function**:
   - The `receive()` function allows the contract to receive Ether.

7. **Function**:
   - `GetId()`: A pure function that takes destination address, timestamp, amount, and sender address as inputs and returns a unique transaction ID (`bytes32`) using the `keccak256` hash function.
   - `ScheduleTo()`: Allows users to schedule a transaction by providing the destination address (`_dest`) and the desired execution timestamp (`_timestamp`). The function creates a transaction ID, adds the transaction details to `tableau_transaction_remain`, and emits the `ident` event with the transaction ID.
   - `SendWhenTime()`: Executes a scheduled transaction based on the provided transaction ID (`ID`). It checks if the transaction is scheduled and if the current block timestamp is greater than the scheduled timestamp. If conditions are met, the function transfers the specified amount of Ether (`amount`) to the destination address (`_dest`), updates the transaction status, moves the transaction to `tableau_transaction_done`, and deletes it from `tableau_transaction_remain`.
   - `time()`: Returns the current block timestamp.

# Python setup and smart contract interaction

This code is designed for scheduling and sending transactions using a smart contract on the Ethereum network. 

## Prerequisites

- Python 3.0 or higher
- web3.py library
- dotenv library

## Installation

1. Clone the repository:

   ```
   git clone https://github.com/RayaneKimo/Smart_Contrat_Python.git
   ```

2. Install the required dependencies using pip:

   ```
   pip install web3 python-dotenv
   ```

## Usage

1. Ensure that you have a local Ethereum network (in my case I used Ganache) running on `http://127.0.0.1:7545`.

2. Modify the following variables in the `main.py` file to match your specific network and contract details:

   - `contract`: The address of the deployed smart contract.
   - `users`: A dictionary mapping user names to their Ethereum addresses.
   - `abi`: The ABI (Application Binary Interface) of the smart contract.
   - `bytecode`: The bytecode of the smart contract.

3. Run the script by executing the following command:

   ```
   python main.py <me> <receiver> <amount> <timeSchedule> 
   ```

   Replace `<me>` with the name of the user initiating the transaction, `<receiver>` with the name of the transaction receiver, `<amount>` with the amount of Ether to send, and `<timeSchedule>` with the scheduled time delay in __minutes__.

4. The script will schedule the transaction by invoking the `ScheduleTo` function in the smart contract, transferring the specified amount of Ether from the sender to the contract. It will then wait until the scheduled time is reached and automatically send the Ether from the contract to the receiver using the `SendWhenTime` function.

5. The result of the transaction execution and the unique transaction ID will be displayed in the console.

Please note that this script assumes you have set the private keys of the users as environment variables using the `.env` file. Ensure that you have provided the correct private keys and adjusted the environment variables accordingly.

For more details on the smart contract functionality, please refer to the comments within the code.

