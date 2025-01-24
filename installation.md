# Installation

This guide outlines the installation of the tools needed to set up your environment. Currently, all these tools are essential for proper setup, though efforts are underway to streamline the process and minimize external dependencies.

## Prerequisites

### 1. **Coprocessor CLI**

The Cartesi Coprocessor CLI Tool simplifies bootstrapping, registering, and deploying Cartesi coprocessor programs. This tool streamlines the development workflow for Cartesi-based applications, allowing developers to focus on building their logic instead of wrestling with setup and deployment processes.

#### Using Cargo

Ensure Rust and Cargo are installed, then install the CLI tool from crates.io:

```bash
cargo install cartesi-coprocessor
```

#### From Source

Clone and build the tool manually:

```bash
git clone https://github.com/Mugen-Builders/co-processor-cli
cd co-processor-cli
cargo install --path .
```

:::warning
Ensure all required dependencies are installed before using this tool.
:::

### 2. **Docker Desktop**

Docker Desktop is a must-have requirement that comes pre-configured with two necessary plugins for building dApps Cartesi:

- Docker Buildx
- Docker Compose
  [Install the latest version of Docker Desktop](https://www.docker.com/products/docker-desktop/) for your operating system.

Install RISC-V support by running:

```bash
docker run --privileged --rm tonistiigi/binfmt --install all
```

### 3. **Cartesi CLI**

The Cartesi CLI is an easy-to-use tool for developing and deploying Cartesi Rollups dApps. In this tutorial, we use the Cartesi CLI as a prerequisite for the Coprocessor CLI, as it is responsible for building the Cartesi Machine image that will be used by your solution in the background.

Install the Cartesi CLI globally:

```bash
npm i -g @cartesi/cli
```

### 4. **Install Foundry**

Foundry is a portable and modular toolkit for Ethereum application development.

Foundry consists of:

- Forge: Ethereum testing framework (like Truffle, Hardhat and DappTools).
- Cast: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
- Anvil: Local Ethereum node, akin to Ganache, Hardhat Network.
- Chisel: Fast, utilitarian, and verbose solidity REPL.

Download and run the Foundry installer:

```bash
curl -L https://foundry.paradigm.xyz | bash
```

Initialize Foundry:

```bash
foundryup
```

### 5. **Build the CARize Utility Container**

This utility is used to generate the final artifact of your dApp to be uploaded to the Web3Storage.

Clone the repository:

```bash
git clone https://github.com/nyakiomaina/carize.git
```

Build the Docker image:

```bash
cd carize
docker build -t carize:latest .
```

### 6. **Install Web3 Storage CLI**

Web3 Storage will be responsible for holding the artifact of your application to be executed upon calls of the Coprocessor.

Install the CLI globally:

```bash
npm install -g @web3-storage/w3cli
```

Refer to the [official documentation](https://web3.storage/docs/w3cli/) for more details.

:::info
With this setup complete, your environment is ready for development and interaction with the Cartesi Coprocessor
:::

---

## Optional tools that can help on your journey

### Nonodo

[Nonodo](https://github.com/Calindra/nonodo) is a development node for Cartesi Rollups that was designed to work with applications running in the host machine instead of the Cartesi Machine. So, the application developer doesn't need to be concerned with compiling their application to RISC-V. The application back-end should run in the developer's machine and call the Rollup HTTP API to process advance and inspect inputs. Nonodo is a valuable development workflow help, but there are some caveats the developer must be aware of.

Install Nonodo, a local testing tool emulating Cartesi's Node, using npm:

```bash
npm install -g nonodo
```

### Cartesi Machine Binary

The goal of these binaries is to make it easy to distribute the Cartesi Machine across different platforms and architectures. Just grab and run no matter what host you are running on!

The archives are designed to be dependency free, it will not install much in your host system. Notably you don't need Lua to run anything, since the Cartesi Machine becomes an executable binary.

The goal is to make it easy for anyone to hack applications using the Cartesi Machine, no matter if you want to use just the cli, its C API, script with Lua, or build other statically linked binaries on top.

#### Download and Extract

1. Download the Cartesi Machine for your OS from [this link](https://github.com/edubart/cartesi-machine-everywhere/releases).
2. Extract the `.tar.xz` file:

```bash
tar -xf <filename>.tar.xz
```

Replace `<filename>` with the actual file name.

#### Setup Environment Variables

1. Rename the extracted folder to `cartesi-machine`:

```bash
mv <extracted-folder-name> cartesi-machine
```

2. Add the `bin` directory to your system’s PATH:

- Open your shell configuration file (`~/.bashrc`, `~/.zshrc`, etc.) and add:

```bash
export PATH=$PATH:/path/to/cartesi-machine/bin
```

- Replace `/path/to/cartesi-machine/bin` with the actual path.

3. Refresh your terminal:

```bash
source ~/.bashrc   # For bash
source ~/.zshrc    # For zsh
```

#### Verify Installation

Run the following command to verify:

```bash
cartesi-machine --help
```

:::note
For Mac Users\*\*: You may encounter a security prompt. Refer to the [troubleshooting section](./troubleshooting#cartesi-machine-blocked-by-mac-security) for resolution. :::
