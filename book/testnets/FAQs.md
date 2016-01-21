# FAQs - Setting up testnets

> What is the difference between public and private testnet?

Not much. The biggest difference is that public testnet are intended for wider audience.

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
> When is it safe to stop using the `--enable-stale-production` flag?

---
> Another question

---
> Another question