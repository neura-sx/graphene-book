# FAQs - Setting up testnets

## Public and private testnet
> What is the difference between public and private testnet?

Not much. The biggest difference is that public testnets are intended for wider audience so that they are are constructed in such a way as to enable third-party witness modes to join in. Therefore setting up a public network is a bit more complex than setting up a local private one.

## CLI parameters
> When I try run the CLI with a specific RPC endpoint or a specific wallet file name, the `server-rpc-endpoint` or `wallet-file` parameters seems to be ignored by the CLI and the default values are applied instead.

These two parameters are implicit. To make them explicit use the `--option=value` syntax, i.e. instead of the standard syntax:
```
cli_wallet --wallet-file my-wallet.json --server-rpc-endpoint ws://testnet.bitshares.eu:11011
```
use this syntax with `=` between the parameter name and its value:
```
cli_wallet --wallet-file=my-wallet.json --server-rpc-endpoint=ws://testnet.bitshares.eu:11011
```

## Stale prodcution flag
> When is it safe to stop using the `enable-stale-production` flag?

The `enable-stale-production` flag tells the witness node to produce on a chain with zero blocks or very old blocks. It can be specified as a parameter on the command line or directly in the `config.ini` file.

If you are running a private testnet on a single machine, you should keep this flag on at all times. This way you ensure that blocks are generated even if the blockchain has been stuck due to a server crash or something similar.

If you are running a public testnet and the witness participation is high enough you can safely switch this flag off. In this case, when starting up, your witness node will connect to another existing witness node over the p2p network, or get the blockchain state from an existing data directory.