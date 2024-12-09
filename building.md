# Building and Running locally

## Running Nonodo with a Cartesi Machine

- Start **nonodo** using the command;

```bash
nonodo
```

## Running the Machine

- In another terminal, create and build a new Cartesi dApp using the following commands:

### 1. **Python**

```bash
cartesi create my-dapp --template=python --branch "wip/coprocessor"
cd my-dapp
cartesi build
```

- Run the Cartesi Machine Locally on bare metal using the command;

```bash
cartesi-machine --network --flash-drive=label:root,filename:.cartesi/image.ext2 \
--volume=.:/mnt --env=ROLLUP_HTTP_SERVER_URL=http://10.0.2.2:5004 --workdir=/mnt -- python dapp.py
```

### 2. **Rust**

```bash
cartesi create my-dapp --template=rust --branch "wip/coprocessor"
cd my-dapp
cartesi build
```

- Run the Cartesi Machine Locally on bare metal using the command;

```bash
cartesi-machine --network --flash-drive=label:root,filename:.cartesi/image.ext2 --env=ROLLUP_HTTP_SERVER_URL=http://10.0.2.2:5004 -- /opt/cartesi/dapp/dapp
```

### 3. **Golang**

```bash
cartesi create my-dapp --template=go --branch "wip/coprocessor"
cd my-dapp
cartesi build
```

- Run the Cartesi Machine Locally on bare metal using the command;

```bash
cartesi-machine --network --flash-drive=label:root,filename:.cartesi/image.ext2 --env=ROLLUP_HTTP_SERVER_URL=http://10.0.2.2:5004 -- /opt/cartesi/dapp/dapp
```

### 4. **Javascript**

```bash
cartesi create my-dapp --template=javasctipt --branch "wip/coprocessor"
cd my-dapp
cartesi build
```

- Run the Cartesi Machine Locally on bare metal using the command;

```bash
cartesi-machine --network --flash-drive=label:root,filename:.cartesi/image.ext2 \
--volume=.:/mnt --env=ROLLUP_HTTP_SERVER_URL=http://10.0.2.2:5004 --workdir=/opt/cartesi/dapp -- node index
```

## Run the CARize Utility Container

After building your dApp, run the CARize container to generate necessary files:

```bash
docker run --rm \
    -v $(pwd)/.cartesi/image:/data \
    -v $(pwd):/output \
    carize:latest /carize.sh
```

## Set environment variables

Set the variables by reading the values from the output files generated after running the carize.sh script:

```bash
CID=$(cat output.cid)
SIZE=$(cat output.size)
MACHINE_HASH=$(xxd -p .cartesi/image/hash | tr -d '\n')
```

## Uploading CAR Files to Web3.Storage

1. **Log In to web3.storage**

```bash
w3 login yourEmail@example.com
```

2. **Create a storage space**

```bash
w3 space create preferredSpaceName
```

3. **Upload files to Web3.Storage**

```bash
w3 up --car output.car
```

## Ensure CoProcessor has your program

```bash
curl -X POST "https://cartesi-coprocessor-solver.fly.dev/ensure/$CID/$MACHINE_HASH/$SIZE"
```
