# Architecture

The Cartesi Coprocessor Architecture is designed to enable processing complex computations in specialized computers off-chain. Through the integration of a new layer called Coprocessors, complex application logics can be run externally by these special computers (coprocessors) while maintaining the state of the application on-chain. This integration is composed of two major components: the on-chain layer and the off-chain layer.

![cartesi - coprocessor architecture](./img/CoprocessorArch.jpg)


### The On-chain components

The on-chain component are a set of smart contract deployed to the base layer, they are the point of contact with regular users interacting with applications deployed to the Cartesi coprocessor. It is composed of key contracts like:

- **Application smart Contract:** This is a regular smart contract existing on a base layer like ethereum, it contains logic to request for certain executions to be executed off-chain it's also designed to receive and handle the result of these executions according to a developer's specification.

- **Coprocessor Contract:** This is a single contract deployed to the base layer, it contains functions that can be called by any application smart contract to request for executions, it's designed to receive requests from application contract, relay these request to the off-chain components of the coprocessor then receive the results of these requests and forward them back to the application which requested them.

### The Off-Chain components

The off-chain component are a collection of external components consisting of servers and also specialized machines called operators which execute operations on request.

- **Solver:** This is an off-chain component of the coprocessor integration it's a specialized server which listens to the coprocessor contract for emitted events, these events contain request for external computation, it's job here is to relay your request to these specialized machines called operators which would execute your request, it's also tasked with making sure that the operators have the developer defined logic with which to execute the request from the on-chain components. After a successful operation, the solver finally signs and sends the result of these operations along with an aggregated signature from the operators on-chain to the coprocessor contract.

- **Operators:**
  This is a network of specialized machines, which hold user defined logics with which complex executions are performed, they do not hold the state of these programs rather once called they execute operations based on the prestored logic then send off the result of the operations to the caller. For each operation that these operators execute they are incentivized through tokens based on the complexity of the operation. As at this moment the Cartesi Coprocessor integration is currently utilizing operators on Eigen Layer.

![cartesi - coprocessor architecture](./img/CoprocessorArch2.jpg)
