# Running the Coprocessor

Follow this guide to set up Foundry, run the Cartesi Coprocessor devnet, and interact with the Coprocessor.

# Option 1: Using Coprocessor CLI

## Step 1: Confirm Foundry Installation

Ensure Foundry is installed and accessible:
```bash
forge --version
```

If Foundry is not installed, refer to the installation guide and set it up before proceeding.

:::info
Run the Cartesi Coprocessor Devnet Environment

1. Clone the Cartesi Coprocessor repository:
   ```bash
   cartesi-coprocessor start-devnet
   ```

   > To stop and clean up the environment later, use:
   ```bash
   cartesi-coprocessor stop-devnet
   ```
:::

:::tip: 
Using the template

You can optionally clone the base contract template for reference and implementation:
```bash
git clone https://github.com/Mugen-Builders/coprocessor-base-contract
```
:::

### Install the Base Contract
Install the base contract in your project:
```bash
forge install https://github.com/Mugen-Builders/coprocessor-base-contract
```

### Import and Inherit the Base Contract
Add the base contract to your project:
```solidity
import "cartesi-coprocessor-base-contract/BaseContract.sol";

contract MyContract is CoprocessorAdapter {
    constructor(address _coprocessorAddress, bytes32 _machineHash)
        CoprocessorAdapter(_coprocessorAddress, _machineHash)
    {}

    // Add your business logic here
}
```

:::note 
The `handleNotice` function in the `CoprocessorAdapterSample.sol` smart contract is triggered by notices sent from the Python application. It is invoked directly when the Coprocessor interacts with the smart contract.
:::
---

## Step 2: Get the important addresses

   ```bash
   cartesi-coprocessor address-book
   ```
   This command prints a list of useful contacts and their addresses.

   ::: Note
      running this command in the directory containing your Cartesi program and not the base directory or solidity contract directory also displays the **machine hash** of your program if your program has been built previously. It also contains the **task issuer** which is the coprocessor address your smart contract needs. 
      
      You can will use those in the contract deploy step.
   :::
---

## Step 3: Deploy the Smart Contract
   Deploys the solidity contract of your project to any specified network of your choice
   ```bash
   cartesi-coprocessor deploy --contract-name <contract name> --network <devnet, mainnet or testnet> --constructor-args <Add as musch argument as needed seperated by single space>
   ```

   > Note: this should be run in the directory for your solidity contract not the base directory. Note; Each successful deployment is logged and stored in a separate directory called deployment_history.


---

# Option 2: Manually running

## Step 1: Confirm Foundry Installation

Ensure Foundry is installed and accessible:
```bash
forge --version
```

If Foundry is not installed, refer to the installation guide and set it up before proceeding.

---
:::info
   Run the Cartesi Coprocessor Devnet Environment

   1. Clone the Cartesi Coprocessor repository:
      ```bash
      git clone https://github.com/zippiehq/cartesi-coprocessor
      cd cartesi-coprocessor
      ```

   2. Initialize and update submodules:
      ```bash
      git submodule update --init --recursive
      ```

   3. Start the devnet environment using Docker Compose:
      ```bash
      docker compose -f docker-compose-devnet.yaml up --wait -d
      ```

      > To stop and clean up the environment later, use:
      ```bash
      docker compose -f docker-compose-devnet.yaml down -v
      ```
:::
---
:::warn
   On devnet you have to perform the following steps to register your dApp

   1. Import the machine CAR file:
      ```bash
      curl -X POST -F file=@output.car http://127.0.0.1:5001/api/v0/dag/import
      ```

   2. Confirm the operator is downloading the dApp machine:
      ```bash
      curl -X POST "http://127.0.0.1:3034/ensure/$CID/$MACHINE_HASH/$SIZE"
      ```

      Ensure the following environment variables are set (if not already):
      ```bash
      CID=$(cat output.cid)
      SIZE=$(cat output.size)
      MACHINE_HASH=$(xxd -p .cartesi/image/hash | tr -d '\n')
      ```

      You can start sending inputs once the status is marked as **ready**.
:::

:::tip: 
Using the template
You can optionally clone the base contract template for reference and implementation:
```bash
git clone https://github.com/Mugen-Builders/coprocessor-base-contract
```
:::

## Step 2: Install the Base Contract
Install the base contract in your project:
```bash
forge install https://github.com/Mugen-Builders/coprocessor-base-contract
```

### Import and Inherit the Base Contract
Add the base contract to your project:
```solidity
import "cartesi-coprocessor-base-contract/BaseContract.sol";

contract MyContract is CoprocessorAdapter {
    constructor(address _coprocessorAddress, bytes32 _machineHash)
        CoprocessorAdapter(_coprocessorAddress, _machineHash)
    {}

    // Add your business logic here
}
```

## Step 3: Install Dependencies

Run the following command to install all required dependencies:
```bash
forge install
```

---

## Step 4: Build the Contract

Build your smart contract:
```bash
forge build
```

---

## Step 5: Deploy the Smart Contract

For example, if you are using the `CoprocessorAdapterSample.sol` contract from the repository, deploy it as follows:

1. Navigate to the contracts directory:
   ```bash
   cd contracts
   ```

2. Deploy the contract using Foundry:
   ```bash
   forge create --broadcast \
       --rpc-url <your_rpc_url> \
       --private-key <your_private_key> \
       ./src/CoprocessorAdapterSample.sol:CoprocessorAdapterSample \
       --constructor-args <coprocessor_address> <machine_hash>
   ```

:::info Example Values for Local Development:
- **RPC URL**: `http://127.0.0.1:8545`
- **Coprocessor Address**: Obtain this value from the `task_issuer` in `config-devnet.toml`.
- **Machine Hash**: Get this from the Cartesi backend deployment output.
:::

:::tip
Save the deployed contract address for frontend interaction.
:::
---

## Additional Notes

- The `handleNotice` function in the `CoprocessorAdapterSample.sol` smart contract is triggered by notices sent from the Python application. It is invoked directly when the Coprocessor interacts with the smart contract.

