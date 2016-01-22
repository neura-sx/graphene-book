# FAQs - Interfacing with Graphene

> Why does the CLI client crash immediately when I try to run it for the first time?

The CLI client is unable to run on its own, i.e. without being connected to the witness node (via a web socket connection).  
You have two options:
* either run the CLI client with the `server-rpc-endpoint` parameter pointing to an existing witness node, e.g.:
```
cli_wallet --server-rpc-endpoint ws://testnet.bitshares.eu:11011
```

* or start your own witness node before starting the CLI client.

If you choose to run your own witness node:
* make sure you have the `rpc-endpoint` parameter uncommented and defined in the `witness_node_data_dir/config.ini` file, e.g.:
```
rpc-endpoint = 127.0.0.1:8090
```

* alternatively you can leave the `config.ini` untouched and run the witness node with the `rpc-endpoint` parameter defined, e.g.:
```
witness_node --rpc-endpoint 127.0.0.1:8090
```

Note: you need to wait a little while until the witness node is up and running because if you try to connect with the CLI client too early the connection will fail with an error like this:
```
Underlying Transport Error
    {"message":"Underlying Transport Error"}
    asio  websocket.cpp:436 fc::http::detail::websocket_client_impl::{ctor}::<lambda_a9259faf4b..>::operator ()
    {"uri":"ws://localhost:8090"}
    th_a  websocket.cpp:621 fc::http::websocket_client::connect
```

---
> Sometimes when I start a witness node it gets stuck just after displaying the blockchian id.

If the witness node is running properly, it should produce messages about the creation of new blocks every few seconds (unless you've delibaretly switched off this messaging).  
If it doesn't behave like this, it means it's got stuck and needs to be forecefully restarted. If this happens, please be patient as in this case, after restarting, the witness node needs to replay the whole blockchain before the CLI client can be connected to it. You'll see the progress of this process in messages like these:
```
97.8505%   2778000 of 2839025
97.9209%   2780000 of 2839025
97.9914%   2782000 of 2839025
98.0618%   2784000 of 2839025
98.1323%   2786000 of 2839025
```

---
> How do I check whether the witness node is already synced?

Run the `info` command in the CLI client and check the `head_block_age` value.

---
> I have problems with syncing the witness node - it seems to be unable to sync beyond a certain date.

You should always make sure you use the newest build available [here](https://github.com/bitshares/bitshares-2/releases/latest) as earlier releases might get stuck due to hard-forks.

---
> What is the best way to interact with the witness node? There seems to be no way to control it, apart from opening & closing it.

The only way you can interact with the witness node is through the CLI client by using its API.  
You can also use the GUI (i.e. the light client). In the GUI, change `Settings -> API connection`, add `ws://127.0.0.1:8090/ws` (according to settings of your witness node) and select it.

---
> In the pre-compiled version, in the config file for the wintess node there is some private key enclosed. Whose private key is this and what's its purpose?

Indeed, the `config.ini` file contains a hard-coded private key:  
```
# Tuple of [PublicKey, WIF private key] (may specify multiple times)
private-key = ["BTS6MRyAjQq8u...","5KQwrPbwdL6PhXu..."]
```
It's a shared key for some special purpose. If I remember BM or someone else has ever explained it in the forum, but I can't find the post right now.

---
> What is the meaning of all those different text colors in the witness node console?

* `green` - debug  
* `white` - info/default  
* `dark yellow` - warning  
* `red` - error  
* `blue` - some kind of info, I don't know

Related source files are in `libraries/fc/include/fc/log/` and `libraries/fc/src/log/`.

---
> How can I close the witness node in a clean way?

Use `ctrl-c`.

---
> How can I close the CLI client in a clean way? There seems to be no command for that. I would expect something like `exit` or `quit`.

On Linux and Mac `ctrl-d` does the job.  
On Windows you can try `ctrl-d` which stops the process but it still produces a nasty exception.

---
> Is it safe to delete the log files of the witness node?

Yes, it's safe to delete `witness_node_data_dir\logs\p2p` but they're rotated automatically after 24 hours anyway. If you don't use them you should probably modify `config.ini` so they aren't written to disk in the first place.

---
> How can I import to my CLI client a wallet originally created in the web GUI? I would expect something like `restore_backup`command that would accept a GUI backup file.

CLI and WEB wallet are two separate applications. They use different ways to represent backups. I think you can currently only manually import keys from the GUI into the CLI.

---
> I'd like to create and register a new account in my CLI wallet and pay for the registration from an existing account in the web GUI. How do I do this?

It doesn't work that way with the current implementation.  
But you can work around it by importing from the GUI to the CLI an active private key of a Lifetime Member account that has funds:

1. In the GUI, go to the permissions tab of an account that is funded and has a Lifetime Member status. Click on the BTS public key on the active tab and copy the private key.

2. Import this private key into the CLI wallet by running this command:
```
import_key <account_name> <private_key>
```

3. Create a new brain-key with this command:
```
suggest_brain_key
```

4. With the new-brain key (i.e. `<brain_key>`) you can now create a new account (i.e. `<new_acc_name>`) and set the registrar and referrer to the account you've just imported from the GUI (i.e. `<imp_acc_name>`):
```
create_account_with_brain_key <brain_key> <new_acc_name> <imp_acc_name> <imp_acc_name> true
```

The brain-key can be used to regenerate the newly created account (even in the GUI wallet) so you might want to make a backup of this brain-key somewhere.  
Also, when the process is complete, you might want to manually delete the imported active private key from your wallet (i.e. from the `wallet.json` file).

---
> Is it possible to remove a private key from the `wallet.json` file?

???

---
> Is it possible to connect a hosted wallet GUI (e.g. `https://bitshares.openledger.info`) to a private witness node run locally?

Any GUI can be connected to a local witness node only if you use either of these:
- a light wallet GUI running locally on your machine
- a hosted wallet GUI running in a web browser, provided it is not using a secure domain (i.e. only works with plain `http`, not `https`)

Thus the OpenLedger GUI cannot be connected to a private witness node, as this GUI uses the `https` protocol.