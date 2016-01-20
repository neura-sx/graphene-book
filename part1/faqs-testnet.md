# FAQs - Setting up a testnet

> This file has public keys starting with GPH..  
`cryptonomex/graphene/testnet-shared-accounts.txt`  
while this has public keys starting with BTS..  
`cryptonomex/graphene/testnet-shared-balances.txt`  
Is it OK to mix those different types of keys? (i.e. GPH.. and BTS..)

---
> When I try run the CLI with a specific RPC endpoint, the `server-rpc-endpoint` parameter seems to be ignored by the CLI and the default value is applied instead.

The `server-rpc-endpoint` parameter is implicit. To make it explicit use the `--option=value` syntax, i.e.:
```
cli_wallet --server-rpc-endpoint=ws://testnet.bitshares.eu:11011
```

---
> Another question

---
> Another question

---
> Another question