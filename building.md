# Building your dApp

:::info 
All the instructions listed on this page are refering to building using the [Coprocessor CLI](https://github.com/Mugen-Builders/co-processor-cli). For an approach that does not you the CLI refer to the manual section of this tutorial.
:::
---

## Step 1: Bootstrap a Project
Create a new Cartesi dApp project:
```bash
cartesi-coprocessor create --dapp-name <project_name> --template <go, python, javascript, rust>
cd my-cartesi-project
```

Implement your business logic by editing the pre-generated child contract in the `src` folder to customize your logic.

## Step 2: Register the Program
Register your program with the Co-Processor:
```bash
cartesi-coprocessor register --email <w3 storage account email> --network <devnet, mainnet or testnet>
```

This command not only builds the image or you Cartesi Machine, containing the application logic, but also uploads it to the Web3Storage and registers it to the solver so it can invoke your application logic whenever a smart contract makes a call to it.

Check Status:

```bash
cartesi-coprocessor check-status --network <devnet, mainnet or testnet>
```

Checks with the coprocessor task issuer for the status of the machine download process
:::note  
This should be run in the directory for your Cartesi program.
Not the base directory or the solidity contract directory.
:::

:::info On your local environment
To be able to use these commands on your local environment you need to startup your devnet first.

**Run the Cartesi Coprocessor Devnet Environment:**
```bash
cartesi-coprocessor start-devnet
```

**To stop and clean up the environment later, use:**
```bash
cartesi-coprocessor stop-devnet
```
:::

---
