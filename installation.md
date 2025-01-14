# Installation Guide

This guide walks you through the installation of tools required for setting up your environment everything you need.

## Prerequisites

---

## Tool Installation

### 1. (Optional) **Install Nonodo**
Install Nonodo, a local testing tool emulating Cartesi's Node, using npm:
```bash
npm install -g nonodo
```

---

### 2. (Optional - Recommended) **Coprocessor CLI**

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

> **Note**: Ensure all required dependencies are installed before using this tool.

---

### 3. **Cartesi CLI**
Install the Cartesi CLI globally:
```bash
npm i -g @cartesi/cli
```

---

### 4. **Docker Desktop**
- Download Docker Desktop from [here](https://www.docker.com/products/docker-desktop).
- Install RISC-V support by running:
  ```bash
  docker run --privileged --rm tonistiigi/binfmt --install all
  ```

---

### 5. **Cartesi Machine**

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

> **Note for Mac Users**: You may encounter a security prompt. Refer to the [troubleshooting section](./troubleshooting#cartesi-machine-blocked-by-mac-security) for resolution.

---

### 6. **Install Foundry**
Download and run the Foundry installer:
```bash
curl -L https://foundry.paradigm.xyz | bash
```
Initialize Foundry:
```bash
foundryup
```

---

### 7. **Build the CARize Utility Container**

1. Clone the repository:
   ```bash
   git clone https://github.com/nyakiomaina/carize.git
   ```
2. Build the Docker image:
   ```bash
   cd carize
   docker build -t carize:latest .
   ```

---

### 8. **Install Web3 Storage CLI**
Install the CLI globally:
```bash
npm install -g @web3-storage/w3cli
```

Refer to the [official documentation](https://web3.storage/docs/w3cli/) for more details.

---

With this setup complete, your environment is ready for development and interaction with the Cartesi's Co-Processor