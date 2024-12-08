# Inspecting and reading outputs

## Inspecting state

Inspecting the state of your dApp though `handle_inspect` function is done in the same way as using Cartesi Rollups standalone. You can refer to the [docs](https://docs.cartesi.io/cartesi-rollups/1.5/development/send-requests/#make-inspect-calls)

## Querying outputs

Querying outputs directly is the exact same as using Cartesi Rollups standalone. You can refer to the [docs](https://docs.cartesi.io/cartesi-rollups/1.5/rollups-apis/graphql/overview/)

### Example Queries

After sending the input, you can query both the outputs and the inputs given to them using the following GraphQL query:

```
query notices {
  notices {
    edges {
      node {
        index
        input {
          index
          payload
        }
        payload
      }
    }
  }
}
```

Execute query and the output should looks like this:

```
{
  "data": {
    "notices": {
      "edges": [
        {
          "node": {
            "index": 0,
            "input": {
              "index": 0,
              "payload": "0x68656c6c6f"
            },
            "payload": "0x68656c6c6f"
          }
        }
      ]
    }
  }
}
```

**Note:** In production, your program will be executed from point of snapshot every single time, there is no state saved, memory or otherwise. This is not the case during development mode.

#### Ensure Coprocessor has your program

Use the `/ensure` API with the variables you've set:

```
curl -X POST "https://cartesi-coprocessor-solver.fly.dev/ensure/$CID/$MACHINE_HASH/$SIZE"
```
