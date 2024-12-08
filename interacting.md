# Sending inputs to your dApp

## Interacting via the Cartesi CLI

- Sending transactions such as deposits or generic messages through the layer 1 is done majorly using the Cartes Cli, though you can also use `cast`, the `cartesi cli` or other approaches. You can follow them here in the [docs](https://docs.cartesi.io/cartesi-rollups/1.5/development/send-requests/)

  - To send executions that will be handled by your dApp, you can run the below command to start the process of sending the transaction:

  ```bash
  Cartesi send
  ```

  - Follow the command path to send any transaction type of your choice, below is an example path for sending a generic text to your application.

  ```
  cartesi send
  ✔ Select send sub-command Send generic input to the application.
  ✔ Chain Foundry
  ✔ RPC URL http://127.0.0.1:8545
  ✔ Wallet Mnemonic
  ✔ Mnemonic test test test test test test test test test test test junk
  ✔ Account 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 9999.968655384479878868 ETH
  ✔ Application address 0xab7528bb862fB57E8A2BCd567a2e929a0Be56a5e
  ✔ Input String encoding
  ✔ Input (as string) hello
  ✔ Input sent: 0xa65e3f82179fac27ac9656f0ae82a4c5d0de5008c6977bbfb5ead18a01a20804
  ```
