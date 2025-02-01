---
sidebar_position: 7
slug: '/nonodox'
label: "NoNodox: An iterative development environment for Cartesi Coprocessor applications"
---

# NoNodox: Development tool

## Overview

This is an iterative tool designed to accelerate the "debugging" and "development" process of Cartesi Coprocessor applications locally, providing a faster path to the production environment.

## Getting Started

### Prerequisites

Please make sure that your system meets all the requirements listed below before proceeding with this tutorial.

1. [Foundry](https://book.getfoundry.sh/getting-started/installation)
2. [Golang](https://go.dev/doc/install)
3. [Nonodo](https://github.com/Calindra/nonodo?tab=readme-ov-file#installation)

### Running

:::info
Before running the command below, ensure you have created a `.toml` file and set the **environment variables** correctly. Below is the structure of the content that should be included in the file:

```toml
[anvil]
http_url = "http://127.0.0.1:8545"
ws_url = "ws://127.0.0.1:8545"
private_key = "<private-key-without-0x 
input_box_block = "7"
```
:::

#### 1. Install the package:
Install the nonodox binary and ensure that it is available in the system's $PATH.

```sh
go install github.com/Mugen-Builders/cartesi-coprocessor-nonodox/cmd/nonodox@latest
```

:::warning
The command above installs NoNodoX into the `bin` directory inside the directory defined by the `GOPATH` environment variable.
If you don't set the `GOPATH` variable, the default install location is `$HOME/go/bin`.
So, to call the `nonodox` command directly, you should add it to the `PATH` variable.
The command below exemplifies that.

```sh
export PATH="$HOME/go/bin:$PATH"
```
:::

#### 2. Download the state file (.json) and start the anvil instance:

Download the instance state that should be started in Anvil with all the required contracts already deployed, in order to interact with your 
CoprocessorAdapter instance that will be deployed later, and start Anvil.

```sh
curl -O https://raw.githubusercontent.com/Mugen-Builders/cartesi-coprocessor-nonodox/refs/heads/main/anvil_state.json
anvil --load-state anvil_state.json
```

:::tip

Before running the command below, please make sure that you have [deployed](./cartesi-co-processor-tutorial/running#step-5-deploy-the-smart-contract) the CoprocesorAdapter instance, passing `0x68B1D87F95878fE05B998F19b66F4baba5De1aed` as the coprocessor address to its constructor
:::

#### 3. Running the tool:

Running the tool, notice that you must have created a `.toml` file and passed it as an argument with the `--config` flag. If you haven't created the file yet, make sure it has the same structure as I shared earlier.

```sh
nonodox --config <filename>.toml
```

Now you can run your application, send inputs, and iteratively observe the outputs (Notices) of your application as they are executed and reach your CoprocesorAdapter instance.

#### 4. How to run your application with NoNodox:

You can run your application so that inputs are processed and outputs are generated according to the way your chosen language does it, for example in Python:

```bash
python my_dapp.py
```

:::info
For those who want an environment closer to production **( ensuring that all libraries and packages used by your application support the Cartesi Machine architecture )** you can use the [Cartesi Machine](https://github.com/edubart/cartesi-machine-everywhere) binary with the system above. This replaces the previous step of running your program on the host.
:::

Below are some examples of how to run your application using the cartesi machine binary for Python, JavaScript, Rust, and Go:

Python:

```bash
cartesi-machine --network --flash-drive=label:root,filename:.cartesi/image.ext2 \
--volume=.:/mnt --env=ROLLUP_HTTP_SERVER_URL=http://10.0.2.2:5004 --workdir=/mnt -- python dapp.py
```

Javascript:

```bash
cartesi-machine --network --flash-drive=label:root,filename:.cartesi/image.ext2 \
--volume=.:/mnt --env=ROLLUP_HTTP_SERVER_URL=http://10.0.2.2:5004 --workdir=/opt/cartesi/dapp -- node index
```

Rust:

```bash
cartesi-machine --network --flash-drive=label:root,filename:.cartesi/image.ext2 \
--env=ROLLUP_HTTP_SERVER_URL=http://10.0.2.2:5004 -- /opt/cartesi/dapp/dapp
```

Golang:

```bash
cartesi-machine --network --flash-drive=label:root,filename:.cartesi/image.ext2 \
--env=ROLLUP_HTTP_SERVER_URL=http://10.0.2.2:5004 -- /opt/cartesi/dapp/dapp
```