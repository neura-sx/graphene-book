# Interacting with a CLI
### Prerequisites
We assume that you have both `witness_node` and `cli_wallet` already compliled (or downloaded from [the offical respository](https://github.com/bitshares/bitshares-2/releases/latest)).

### Run the witness node
To run the witness node use this command:
```
witness_node --rpc-endpoint 127.0.0.1:11011
```
### Run the CLI
To run the CLI use this command:
```
cli_wallet --server-rpc-endpoint=ws://127.0.0.1:11011
```