# Private testnet

### Creating the genesis file
The genesis file defines the initial state of the network.  
For our testnet we need to have a unique blockchain id and thus we need to create a new genesis file as blockchain id is derived from this file.  
We create a new genesis json file named `my-genesis.json` by running this command:
```
witness_node --create-genesis-json my-genesis.json
```
The `my-genesis.json` file will be created in your current working directory.

### Editing the genesis file
If you want to customize the network's initial state, edit the newly created `my-genesis.json` file. This allows you to control things such as:
* The accounts that exist at genesis, their names and public keys
* Assets and their initial distribution (including core asset)
* The initial values of chain parameters (including fees)
* The account / signing keys of the init witnesses (or in fact any account at all).


### Getting the blockchain id
The blockchain id is a hash of the genesis state. All transaction signatures are only valid for a single blockchain id. So editing the genesis file will change your blockchain id, and make you unable to sync with all existing chains (unless one of them has exactly the same genesis file you do).

Run this command:
```
witness_node --data-dir data --genesis-json my-genesis.json 
```
and when a message like this shows up:
```
3501235ms th_a main.cpp:165 main] Started witness node on a chain with 0 blocks.
3501235ms th_a main.cpp:166 main] Chain ID is 8b7bd36a146a03d0e5d0a971e286098f41230b209d96f92465cd62bd64294824
```
the initialization is complete. Use `ctrl-c` to close the witness node.

As a result, you should get two things:
* a directory named `data` has been created with a file named `config.ini` located in it.
* your blockchain id is now known - it's displayed in the message above

> Note that your blockchain id will be different than the one used in the above example

### Editing the config file

Open the `data/config.ini` file in your favorite text editor, and set the following settings, uncommenting them if necessary:
```
rpc-endpoint = 127.0.0.1:11011  
genesis-json = my-genesis.json  
enable-stale-production = true 
```
Also, just under this entry in the `config.ini` file:
```
# ID of witness controlled by this node (e.g. "1.6.5", quotes are required, may specify multiple times)
```
add the following multiple entry: 
```
witness-id = "1.6.1"
witness-id = "1.6.2"
witness-id = "1.6.3"
witness-id = "1.6.4"
witness-id = "1.6.5"
witness-id = "1.6.6"
witness-id = "1.6.7"
witness-id = "1.6.8"
witness-id = "1.6.9"
witness-id = "1.6.10"
witness-id = "1.6.11"
```
The above list authorizes the witness node to produce blocks on behalf of the listed witness ids. Normally each witness would be on a different node, but for the purposes of this private testnet, we will start out with all witnesses signing blocks on a single node.  
The private keys for all those witness ids (needed to sign blocks) are already supplied in the `config.ini` file:
```
# Tuple of [PublicKey, WIF private key] (may specify multiple times)
private-key = ["TEST6MRyA...T5GDW5CV","5KQwrPb...tP79zkvFD3"]
```



---

---

---


```
witness_node --data-dir data  
```
block production should start at this stage

---

```
cli_wallet --wallet-file=my-wallet.json --chain-id 8b7bd36a146a03d0e5d0a971e286098f41230b209d96f92465cd62bd64294824 --server-rpc-endpoint=ws://127.0.0.1:11011
```

```
set_password southpark
unlock southpark

import_key nathan 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
import_balance nathan ["5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"] true
get_account nathan
upgrade_account nathan true
!!! restart CLI
get_account nathan
>>"membership_expiration_date": "1970-01-01T00:00:00"<<
>>"membership_expiration_date": "1969-12-31T23:59:59"<<

suggest_brain_key
{
  "brain_priv_key": "MYAL SOEVER UNSHARP PHYSIC JOURNEY SHEUGH BEDLAM WLOKA FOOLERY GUAYABA DENTILE RADIATE TIEPIN ARMS FOGYISH COQUET",
  "wif_priv_key": "5JDh3XmHK8CDaQSxQZHh5PUV3zwzG68uVcrTfmg9yQ9idNisYnE",
  "pub_key": "TEST78CuY47Vds2nfw2t88ckjTaggPkw16tLhcmg4ReVx1WPr1zRL5"
}

register_account alpha TEST78CuY47Vds2nfw2t88ckjTaggPkw16tLhcmg4ReVx1WPr1zRL5 TEST78CuY47Vds2nfw2t88ckjTaggPkw16tLhcmg4ReVx1WPr1zRL5 nathan nathan 0 true
import_key alpha 5JDh3XmHK8CDaQSxQZHh5PUV3zwzG68uVcrTfmg9yQ9idNisYnE

transfer nathan alpha 2000000000 TEST "here is the cash for upgrade" true
list_account_balances alpha
upgrade_account alpha true
get_account alpha
list_account_balances alpha
```

https://github.com/BitSharesEurope/docs.bitshares.eu/blob/3ea1f3f4d3cf222a421362efff0bb8b2db1dafb2/source/testnet/Private.rst