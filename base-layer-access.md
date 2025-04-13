# Base Layer Access

Base Layer Access is one of the features available through `/gio` calls within an application built using the Cartesi coprocessor. 

## What is Base Layer Access?

Base Layer Access is a mechanism that allows applications running inside the Cartesi Coprocessor to securely and verifiably access data from the base blockchain (such as Ethereum). This is achieved through specific calls to the Generic I/O (GIO) framework.

## How does it work?

Base layer access is enabled through domain `0x27` in GIO, which allows calls via `eth_getStorageAt`. This enables the application to access the base layer (read-only) and load necessary data for processing within the coprocessor.

### Understanding Storage Slots

In Ethereum, contract storage is organized into slots of 32 bytes each. Each state variable in a contract is assigned a specific storage slot. The layout follows these rules:

1. **Basic Types**: Each basic type (uint256, address, etc.) takes up one full 32-byte slot. Smaller types can be packed together if declared consecutively.

2. **Arrays**:
   - Dynamic arrays: The array length is stored in the declared slot, and elements are stored sequentially starting at keccak256(slot)
   - Fixed-size arrays: Elements are stored inline starting from the declared slot

3. **Mappings**: The declared slot is not used to store data directly. Instead, each value is stored at keccak256(abi.encodePacked(key, slot)), where slot is the position of the mapping variable.

4. **Structs**: Fields are stored sequentially in storage, and smaller fields may be packed into the same slot if they fit (e.g., bool, uint8, etc.), following alignment rules.

For a detailed explanation of how different data types are stored in Solidity, refer to the [official Solidity documentation on storage layout](https://docs.soliditylang.org/en/latest/internals/layout_in_storage.html).

### Finding Storage Slots

To find the storage slot of a specific variable or data structure in a contract, you can use Forge's storage layout inspection:

```bash
forge inspect <path_to_contract>:<ContractName> storage-layout
```

This command will show you a table with the storage layout. Here's a generic example of what you might see:

```
╭----------------+----------------+------+--------+-------╮
| Name           | Type           | Slot | Offset | Bytes |
+========================================================+
| token          | address        | 0    | 0      | 20    |
|----------------+----------------+------+--------+-------|
| balance        | uint256        | 1    | 0      | 32    |
|----------------+----------------+------+--------+-------|
| users          | mapping        | 2    | 0      | 32    |
|----------------+----------------+------+--------+-------|
| items          | uint256[]      | 3    | 0      | 32    |
╰----------------+----------------+------+--------+-------╯
```

In this example:
- `token` is stored at slot 0
- `balance` is stored at slot 1
- `users` mapping starts at slot 2
- `items` array starts at slot 3

### Underlying Mechanism

The Base Layer Access feature internally uses the Ethereum JSON-RPC method `eth_getStorageAt` to read storage slots from smart contracts. This allows the coprocessor to:

1. Access contract state at specific block hashes
2. Read storage variables from any contract address
3. Verify historical data without executing transactions

### Request Specification

```json
{
    "domain": "0x27",
    "id": "0x<block_hash:32_bytes><address:20_bytes><storage_slot:32_bytes>"
}
```

### Common Use Cases

Base Layer Access is commonly used for:

1. **Historical Data Analysis**
   - Loading historical state of contracts
   - Analyzing past events and transactions
   - Verifying historical balances and states

2. **State Verification**
   - Verifying token balances and allowances
   - Checking contract permissions and roles
   - Validating ownership and access rights

3. **Data Aggregation**
   - Collecting data from multiple contracts
   - Building off-chain indexes and databases
   - Creating analytics and reporting systems

### Implementation Example

Here's an example of how to read a storage slot using the API provided for your application:

```python
def get_storage_at(block_hash: bytes, address: str, slot: bytes) -> bytes:
    gio_payload = {
        "domain": 0x27,
        "id": "0x" + block_hash.hex() + address[2:] + slot.hex()
    }
    response = requests.post(rollup_server + "/gio", json=gio_payload)
    if response.status_code == 202:
        return bytes.fromhex(response.json()["data"][2:])
    raise RuntimeError(f"Failed to access base layer. Status code: {response.status_code}")
```