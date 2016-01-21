# FAQs - Setting up testnets

> What is the difference between public and private testnet?

Not much. The biggest difference is that public testnets are intended for wider audience so that they are are constructed in such a way as to enable third-party witnesses to join in. Therefore setting up a public network is a bit more complex than private one.

---
> When I try run the CLI with a specific RPC endpoint or a specific wallet file name, the `server-rpc-endpoint` or `wallet-file` parameters seems to be ignored by the CLI and the default values are applied instead.

These two parameters are implicit. To make them explicit use the `--option=value` syntax, i.e. instead of the standard syntax:
```
cli_wallet --wallet-file my-wallet.json --server-rpc-endpoint ws://testnet.bitshares.eu:11011
```
use this syntax with `=` between the parameter name and its value:
```
cli_wallet --wallet-file=my-wallet.json --server-rpc-endpoint=ws://testnet.bitshares.eu:11011
```

---
> When is it safe to stop using the `enable-stale-production` flag?

The `enable-stale-production` flag tells the witness node to produce on a chain with zero blocks or very old blocks.

If you are running a private testnet on a single machine, you should use it at all times. This flag ensures that blocks are generated even if the blockchain has been stuck due to a server crash or something similar.

If you are running a public testnet and the witness participation is high enough you can safely switch this flag off.