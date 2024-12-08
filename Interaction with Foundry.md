# Foundry setup to interact with the Coprocessor

1. **Confirm foundry installation**

```
forge --version
```

If Foundry isn't installed, refer back to the Install Foundry step.

2. **Clone the foundry template repository**

This template repository contains the ICoprocessor.sol interface and a sample contract:

```
git clone https://github.com/zippiehq/cartesi-coprocessor-contract-template.git
cd cartesi-coprocessor-contract-template
```

3. **Get dependencies**

```
forge install
```

4. **Build the contract**

```
forge build
```

5. **Deployment**

##### Obtaining RPC Provider and Etherscan API Key

To interact with the Ethereum mainnet, you will need to obtain:

###### RPC Provider url

- Visit Alchemy or Infura.
- Sign up for an account or log in if you already have one.
- Create a new project targeting the Ethereum mainnet.
- Obtain the RPC URL provided for your project.

###### Etherscan API Key

Go to Etherscan to obtain an API key.

- Register or log in
- Navigate to API-Keys in your account settings
- Generate a new API key for use with your deployments

Replace placeholders with your obtained RPC URL, private key, and Etherscan API key

```
COPROCESSOR_ADDRESS=0xB819BA4c5d2b64d07575ff4B30d3e0Eca219BFd5 MACHINE_HASH=0x<machine_hash_here> forge script script/Deploy.s.sol:DeployScript --rpc-url <rpc-url> --private-key xxx --etherscan-api-key xxx --broadcast --verify
```

(run this to get the machine hash

```
 echo "Machine Hash: $MACHINE_HASH"
```

)

**Note:** Deploying on the Ethereum mainnet involves real transaction costs (gas fees).

**Notice Processing**

The `handleNotice` function in the `CoprocessorCaller.sol` smart contract is triggered by notices that are sent from the Python application. This function is called as a direct response to the coprocessor being invoked within the smart contract.
